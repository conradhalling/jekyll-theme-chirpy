---
title: Initializing the SQLite 3 CLI
description: I set up an alias and a SQLite 3 initialization file to initialize the SQLite 3 command line interface efficiently.
author: conrad
date: 2025-06-14 05:29:00 -0400
categories: [Computing]
---

## Introduction

I am working on three projects where I use
[SQLite 3](https://www.sqlite3.org/)
for storing data. After
growing tired of entering the same series of commands when I started the
`sqlite3` command line interface (CLI), I modified my `.bash_profile` to add
an alias, and I created a SQLite initialization file in my home directory.

## Installing SQLite 3.50.1

On my MacBook Pro running macOS 15.5, the system `sqlite3` is version
3.43.2.

```console
$ which sqlite3
/usr/bin/sqlite3

$ sqlite3 --version
3.43.2 2023-10-10 13:08:14 1b37c146ee9ebb7acd0160c0ab1fd11017a419fa8a3187386ed8cb32b709aapl (64-bit)
```

I installed the latest version of `sqlite3` using [Homebrew](https://brew.sh).
If you have used Homebrew to install Python or PHP, `sqlite3` may already
be installed. At the time of writing, the latest version of SQLite 3 is
3.50.1.

```console
$ brew install sqlite3
```

## Starting SQLite 3

When I started this version of `sqlite3`, I had to enter the full path.

```console
$ /opt/homebrew/opt/sqlite3/bin/sqlite3 --version
3.50.1 2025-06-06 14:52:32 b77dc5e0f596d2140d9ac682b2893ff65d3a4140aa86067a3efebe29dc914c95 (64-bit)
```

When I started `sqlite3`, I needed to enter five commands to set it up for my
needs, as shown in this CLI session:

```console
$ /opt/homebrew/opt/sqlite3/bin/sqlite3 ~/data/crawlers/crawlers.sqlite3
SQLite version 3.50.1 2025-06-06 14:52:32
Enter ".help" for usage hints.
sqlite> pragma foreign_keys=on;
sqlite> pragma temp_store=2;
sqlite> .mode columns
sqlite> .timer on
sqlite> .changes on
sqlite> select * from tbl_domain;
id  name               access_log_file_glob_pattern
--  -----------------  ----------------------------
1   conradhalling.com  access.log.*.gz
2   sphaerula.com      sphaerula.com-*.gz
Run Time: real 0.002 user 0.000428 sys 0.001109
changes: 0   total_changes: 0
sqlite> .quit
```

## Using an Initialization File

I created a file in my home directory named `init.sql` that contained
these commands:

```
pragma foreign_keys=1;
pragma temp_store=2;
.mode columns
.timer on
.changes on
```

Using `sqlite3`'s `-init` option, I could start `sqlite3` and initialize it
using the `init.sql` file:

```console
$ /opt/homebrew/opt/sqlite3/bin/sqlite3 -init ~/init.sql
-- Loading resources from /Users/xxxxx/init.sql
SQLite version 3.50.1 2025-06-06 14:52:32
Enter ".help" for usage hints.
```

## Creating an Alias

To shorten the command, I modified my `.bash_profile` to add an alias named
`sqlite3`.

```shell
alias sqlite3="/opt/homebrew/opt/sqlite3/bin/sqlite3 -init ${HOME}/init.sql"
```

In a new `bash` window, I could now enter just the `sqlite3` command, as
shown in this CLI session:

```console
$ sqlite3 ~/data/crawlers/crawlers.sqlite3
-- Loading resources from /Users/xxxxx/init.sql
SQLite version 3.50.1 2025-06-06 14:52:32
Enter ".help" for usage hints.
sqlite> select * from tbl_domain;
id  name               access_log_file_glob_pattern
--  -----------------  ----------------------------
1   conradhalling.com  access.log.*.gz
2   sphaerula.com      sphaerula.com-*.gz
Run Time: real 0.000 user 0.000307 sys 0.000467
changes: 0   total_changes: 0
sqlite> .quit
```

## Using the SQLite 3 CLI

The SQLite 3 command line shell is thoroughly documented at
[https://sqlite.org/cli.html](https://sqlite.org/cli.html).
