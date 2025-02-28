---
title: Using SQLite3 with Python 3.13
description: With Python 3.13, it turned out to be difficult to enforce foreign key constraints and manage transactions at the same time. Here is a working example.
author: conrad
date: 2025-02-27 18:32:00 -0500
categories: [Computing]
---

Using Python 3.13, I started a project that saved data in a
[SQLite3](https://www.sqlite.org) database. I wanted to manage transactions, and
I wanted to enforce foreign key constraints. But because of surprising behavior
introduced into the [sqlite3](https://docs.python.org/3/library/sqlite3.html)
package in Python 3.12, it took me an entire day to figure out how to do this
correctly. I will write in detail about this problem in a future post.

In the meantime, for my benefit and yours, here is a demonstration script. This
script requires Python 3.13 or 3.12 to run. You can copy the script by clicking
on the clipboard icon at the top right of the code block window below.

```python
"""
INTRODUCTION

This is an example of using Python to create a SQLite database. The script
creates a sqlite3 database in the file db.sqlite3.

This script meets the following requirements:
    -   it manages database transactions, allowing a complete rollback when
        requested
    -   it enforces foreign key constraints

This script manages database transactions by issuing explicit "BEGIN
TRANSACTION" and "COMMIT" or "ROLLBACK" commands.

This script enforces foreign key constraints by issuing a
"PRAGMA foreign_keys = ON" command.

EXAMPLES

In these examples, foreign key constraints are enforced.

With the rollback argument, no data is saved in the database:

    $ python3 create_db.py rollback
    Creating tables and inserting data...
      Done.
    Attempting to violate foreign key constraint...
      IntegrityError: FOREIGN KEY constraint failed
    Rolling back changes...
      Done.
      The file db.sqlite3 should be empty.

    $ ls -l db.sqlite3
    -rw-r--r--@ 1 halto  staff  0 Feb 28 07:45 db.sqlite3

With the commit argument, data is saved in the database:

    $ python3 create_db.py commit
    Creating tables and inserting data...
      Done.
    Attempting to violate foreign key constraint...
      IntegrityError: FOREIGN KEY constraint failed
    Committing changes...
      Done.
    The file db.sqlite3 should contain the data.

    $ ls -l db.sqlite3
    -rw-r--r--@ 1 halto  staff  12288 Feb 28 07:46 db.sqlite3

CHECK DATABASE

Here's how to use the sqlite3 command line interface to view what is in the
database.

    $ /opt/homebrew/opt/sqlite3/bin/sqlite3 db.sqlite3
    SQLite version 3.49.1 2025-02-18 13:38:58
    Enter ".help" for usage hints.

    sqlite> .schema
    CREATE TABLE authors
            (
                id INTEGER PRIMARY KEY,
                first_name TEXT,
                last_name TEXT
            );
    CREATE TABLE books
            (
                id INTEGER PRIMARY KEY,
                title TEXT,
                author_id INTEGER,
                FOREIGN KEY(author_id) REFERENCES authors(id)
            );
    sqlite> .mode column

    sqlite> select * from authors;
    id  first_name  last_name
    --  ----------  ---------
    1   Isaac       Asimov
    2   Anne        Leckie
    3   Octavia     Butler

    sqlite> select * from books;
    id  title              author_id
    --  -----------------  ---------
    1   Dawn               3
    2   Foundation         1
    3   Ancillary Justice  2

    sqlite> .quit

VERSIONS

Python 3.13.2
sqlite3 3.49.1
macOS 15.3.1
"""


import sqlite3
import sys


def save_data(db, commit_flag):
    # Open a connection.
    conn = sqlite3.connect(database=db, autocommit=True)

    # Enforce foreign key constraints.
    sql01 = "PRAGMA foreign_keys = ON"
    conn.execute(sql01)

    # Begin an explicit transaction.
    sql02 = "BEGIN TRANSACTION"
    conn.execute(sql02)

    # Get a cursor.
    cur = conn.cursor()

    print("Creating tables and inserting data...")
    # Create tables.
    sql03 = """
        CREATE TABLE IF NOT EXISTS
        authors
        (
            id INTEGER PRIMARY KEY,
            first_name TEXT,
            last_name TEXT
        )"""
    cur.execute(sql03)
    
    sql04 = """
        CREATE TABLE IF NOT EXISTS
        books
        (
            id INTEGER PRIMARY KEY,
            title TEXT,
            author_id INTEGER,
            FOREIGN KEY(author_id) REFERENCES authors(id)
        )"""
    cur.execute(sql04)

    # Insert data.
    authors_list = [
        ("Isaac", "Asimov"),
        ("Anne", "Leckie"),
        ("Octavia", "Butler"),
    ]
    sql05 = """
        INSERT INTO authors
        (
            first_name,
            last_name
        )
        VALUES (?, ?)"""
    cur.executemany(sql05, authors_list)

    books_list = [
        ("Dawn", "Octavia", "Butler"),
        ("Foundation", "Isaac", "Asimov"),
        ("Ancillary Justice", "Anne", "Leckie"),
    ]
    sql06 = """
        SELECT
            id
        FROM
            authors
        WHERE
            first_name = ?
            AND last_name = ?"""
    sql07 = """
        INSERT INTO
        books
        (
            title,
            author_id
        )
        VALUES (?, ?)"""

    for row in books_list:
        cur.execute(sql06, (row[1], row[2]))
        author_id = cur.fetchone()[0]
        cur.execute(sql07, (row[0], author_id))
    print("  Done.")

    # Attempt to delete a row from table authors.
    # This should cause a foreign key constraint problem.
    print("Attempting to violate foreign key constraint...")
    sql08 = """
        DELETE from authors
        WHERE id = 1
    """
    try:
        cur.execute(sql08)
        print("  Foreign key constraint not enforced!")
    except sqlite3.IntegrityError as exc:
        print("  ", type(exc).__name__, ": ", exc, sep="")

    # Commit or roll back database changes. If the rollback is successful, the
    # size of the database file will be 0 bytes.
    if commit_flag:
        print("Committing changes...")
        conn.execute("COMMIT")
        print("  Done.")
        print(f"  The file {db} should contain the data.")
    else:
        print("Rolling back changes...")
        conn.execute("ROLLBACK")
        print("  Done.")
        print(f"  The file {db} should be empty.")
    
    # Clean up.
    cur.close()
    conn.close()


def get_commit_flag():
    help_str = "The first script argument must be 'commit' or 'rollback'."
    if len(sys.argv) == 1:
        raise ValueError(help_str)
    elif sys.argv[1] == "commit":
        commit_flag = True
    elif sys.argv[1] == "rollback":
        commit_flag = False
    else:
        raise ValueError(help_str)
    return commit_flag


def main():
    db = "db.sqlite3"
    commit_flag = get_commit_flag()
    save_data(db=db, commit_flag=commit_flag)


if __name__ == "__main__":
    main()
```
