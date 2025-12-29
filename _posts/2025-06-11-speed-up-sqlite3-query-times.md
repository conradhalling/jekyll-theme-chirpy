---
title: Speeding up SQLite3 Query Times on a GoDaddy Shared Hosting Server
description: Nearly identical SQLite3 queries on a GoDaddy shared hosting server had significantly different run times. I solved the problem with the SQLite 3 'pragma temp_store=2;' command.
author: conrad
date: 2025-06-11 15:50:00 -0400
categories: [Computing]
---

I have been creating a website,
[Crawler Activity](https://sphaerula.com/cgi/crawlers.cgi?summary),
that presents the data from monitoring the activity of web crawlers that are
indexing and scraping my websites at
[conradhalling.com](https://conradhalling.com/blog/) and
[sphaerula.com](https://sphaerula.com/blog/). The website is hosted on a
[GoDaddy](https://www.godaddy.com/) shared hosting server on which I installed
[SQLite 3.50.0](https://sqlite.org/)
and [Python 3.13.3](https://www.python.org/). (For more information, see my post
[Installing SQLite 3.50.0 and Python 3.13.3 on a GoDaddy Shared Hosting Server](/blog/posts/installing-sqlite-3.50.0-and-python-3.13.33-on-a-godaddy-shared-hosting-server/).)

I experienced a performance issue that I describe here in detail.
I had tested two database queries that were identical except for the field
that they selected from the same table. The first
query selected `tbl_domain.id` whereas the second query selected
`tbl_domain.name`. I expected these queries to take the same amount of
time, but the run times were significantly different. I experienced a similar
run time difference with many other queries.

```console
$ sqlite3 data/crawlers.sqlite3
SQLite version 3.50.0 2025-05-29 14:26:00
Enter ".help" for usage hints.
sqlite> .mode columns
sqlite> .timer on
sqlite> SELECT
    tbl_domain.id,
    COUNT(tbl_domain.id)
FROM
    tbl_request
    INNER JOIN tbl_ip_address
        ON tbl_request.ip_address_id = tbl_ip_address.id
    INNER JOIN tbl_domain
        ON tbl_request.domain_id = tbl_domain.id
WHERE
    tbl_ip_address.is_site_owner = 0
    AND tbl_request.iso_date <> (
        SELECT MAX(tbl_request.iso_date) FROM tbl_request
    )
GROUP BY
    tbl_domain.id
;
id  COUNT(tbl_domain.id)
--  --------------------
1   72542
2   48347
Run Time: real 0.226 user 0.152617 sys 0.063156


sqlite> SELECT
    tbl_domain.name,
    COUNT(tbl_domain.name)
FROM
    tbl_request
    INNER JOIN tbl_ip_address
        ON tbl_request.ip_address_id = tbl_ip_address.id
    INNER JOIN tbl_domain
        ON tbl_request.domain_id = tbl_domain.id
WHERE
    tbl_ip_address.is_site_owner = 0
    AND tbl_request.iso_date <> (
        SELECT MAX(tbl_request.iso_date) FROM tbl_request
    )
GROUP BY
    tbl_domain.name
;
name               COUNT(tbl_domain.name)
-----------------  ----------------------
conradhalling.com  72542
sphaerula.com      48347
Run Time: real 1.255 user 0.174469 sys 0.068221
```

This table extracts the significant results for easy comparison.

| Select          | Run Time (real s) |
|-----------------|------------------:|
| tbl_domain.id   |             0.226 |
| tbl_domain.name |             1.255 |

The `tbl_domain` table was a small table with three columns and two rows.
Why would selecting the `name` instead of the `id` field from the table cause
the query to run more than five times slower?

```console
sqlite> .schema tbl_domain
CREATE TABLE tbl_domain
        (
            id INTEGER PRIMARY KEY,
            name TEXT NOT NULL UNIQUE,
            access_log_file_glob_pattern TEXT NOT NULL
        ) STRICT
    ;
sqlite> .mode columns
sqlite> select * from tbl_domain;
id  name               access_log_file_glob_pattern
--  -----------------  ----------------------------
1   conradhalling.com  access.log.*.gz
2   sphaerula.com      sphaerula.com-*.gz
Run Time: real 0.000 user 0.000000 sys 0.000239
```

After I studied the SQLite3 documentation for several hours, I found
the description of
[`PRAGMA temp_store`](https://www.sqlite.org/pragma.html#pragma_temp_store).
The default setting for this pragma turned out to be 0, causing the
storage used for TEMP tables and indices to be *file* storage. Setting
the value of `PRAGMA temp_store` to 2 would change the storage to *memory*,
which should be much faster.

In fact, when I set `PRAGMA temp_store=2`, as shown below, the run times for
both queries where essentially equally fast.

```console
sqlite> PRAGMA temp_store;
0
sqlite> PRAGMA temp_store=2;
sqlite> pragma temp_store;
2


sqlite> SELECT
    tbl_domain.id,
    COUNT(tbl_domain.id)
FROM
    tbl_request
    INNER JOIN tbl_ip_address
        ON tbl_request.ip_address_id = tbl_ip_address.id
    INNER JOIN tbl_domain
        ON tbl_request.domain_id = tbl_domain.id
WHERE
    tbl_ip_address.is_site_owner = 0
    AND tbl_request.iso_date <> (
        SELECT MAX(tbl_request.iso_date) FROM tbl_request
    )
GROUP BY
    tbl_domain.id
;
id  COUNT(tbl_domain.id)
--  --------------------
1   72542
2   48347
Run Time: real 0.240 user 0.167511 sys 0.063543


sqlite> SELECT
    tbl_domain.name,
    COUNT(tbl_domain.name)
FROM
    tbl_request
    INNER JOIN tbl_ip_address
        ON tbl_request.ip_address_id = tbl_ip_address.id
    INNER JOIN tbl_domain
        ON tbl_request.domain_id = tbl_domain.id
WHERE
    tbl_ip_address.is_site_owner = 0
    AND tbl_request.iso_date <> (
        SELECT MAX(tbl_request.iso_date) FROM tbl_request
    )
GROUP BY
    tbl_domain.name
;
name               COUNT(tbl_domain.name)
-----------------  ----------------------
conradhalling.com  72542
sphaerula.com      48347
Run Time: real 0.236 user 0.176746 sys 0.058760
```

This table extracts the significant values for easy comparison.

| Select          | Pragma temp_store | Run Time (real s) |
|-----------------|------------------:|------------------:|
| tbl_domain.id   |                 0 |             0.226 |
| tbl_domain.id   |                 2 |             0.240 |
| tbl_domain.name |                 0 |             1.255 |
| tbl_domain.name |                 2 |             0.236 |

I speculate that the query selecting `tbl_domain.name` required temporary
storage and that disk storage was much slower than memory storage on the shared
hosting server.

I modified my Python code for connecting to a SQLite3 database to set
`PRAGMA temp_store=2`, as shown below. This code snippet includes code
that enforces foreign key constraints.

```python
import sqlite3

# conn is a singleton containing the connection to the SQLite database file.
conn = None


def connect(db_file):
    """
    Connect to the database file.

    Enforce foreign key constraints.

    Set temp_store to 2 (memory); this greatly reduces query time.
    """
    global conn
    conn = sqlite3.connect(database=db_file, isolation_level=None)
    set_temp_store_in_memory()
    enforce_foreign_key_constraints()


def set_temp_store_in_memory():
    """
    Set PRAGMA temp_store = 2 and check its value.

    On sphaerula.com, setting this pragma greatly speeds up query
    performance.
    """
    global conn
    # The API doesn't allow binding a value to a pragma using '?'.
    in_memory = 2
    # Set the pragma.
    cur = conn.execute(f"PRAGMA temp_store={in_memory}")
    # Get the pragma value and check it.
    cur.execute("PRAGMA temp_store")
    row = cur.fetchone()
    cur.close()
    pragma_temp_store_value = None
    if row is not None:
        pragma_temp_store_value = row[0]
    if pragma_temp_store_value != in_memory:
        raise sqlite3.IntegrityError(f"PRAGMA temp_store is not {in_memory}")


def enforce_foreign_key_constraints():
    """
    Set PRAGMA foreign_keys = ON and check its value.
    """
    global conn
    cur = conn.cursor()
    # Set the pragma.
    if conn.in_transaction == True:
        # This is needed when conn.autocommit is False because a transaction
        # is already open, and PRAGMA foreign_keys = ON has no effect in
        # an open transaction.
        cur.executescript("COMMIT; PRAGMA foreign_keys = ON; BEGIN;")
    else:
        cur.execute("PRAGMA foreign_keys = ON")
    cur.close()
    verify_foreign_key_constraints()


def verify_foreign_key_constraints():
    """
    Verify that the PRAGMA foreign_keys value is 1.
    Raise a sqlite3.IntegrityError exception if the value is not 1.
    """
    # Get the pragma. It must be 1.
    global conn
    cur = conn.cursor()
    cur.execute("PRAGMA foreign_keys")
    rows = cur.fetchall()
    cur.close()
    pragma_foreign_keys_value = None
    if len(rows) != 0:
        pragma_foreign_keys_value = rows[0][0]
    logger.debug(f"pragma_foreign_keys_value: {pragma_foreign_keys_value}")
    if pragma_foreign_keys_value != 1:
        raise sqlite3.IntegrityError("PRAGMA foreign_keys is not ON")
```

<em>A note on performance of modern laptop computers.</em> I am developing this
code on my M4 Pro MacBook Pro, which has 1 TB of SSD storage and 48 GB of RAM,
using SQLite 3.50.0 and Python 3.13.3 installed using
[homebrew](https://brew.sh/).

With `pragma temp_store=0`, the run times for the same queries were:

| Select          | Run Time (real s) |
|-----------------|------------------:|
| tbl_domain.id   |             0.051 |
| tbl_domain.name |             0.061 |

My MacBook Pro laptop executes these queries about four times faster than the
shared hosting server.
