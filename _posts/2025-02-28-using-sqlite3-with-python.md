---
title: Using SQLite3 with Python 3.12 and 3.13
description: With Python 3.12 and 3.13, it turned out to be difficult to enforce foreign key constraints and manage transactions at the same time. Here is a working example.
author: conrad
date: 2025-02-27 18:32:00 -0500
categories: [Computing]
---

March 14, 2025: I extensively revised this post and the script.

## Introduction

Using Python 3.13, I started a project that saved data in a
[SQLite3](https://www.sqlite.org) database. I wanted to manage transactions, and
I wanted to enforce foreign key constraints. But because of surprising behavior
introduced into the [sqlite3](https://docs.python.org/3/library/sqlite3.html)
package in Python 3.12, it took me an entire day to figure out how to do this
correctly. I will write in detail about this problem in a future post.

If you want to enforce foreign key constraints in a straightforward way by
issuing a `PRAGMA foreign_keys = ON` command, and if you want to manage
transactions to ensure a *complete* rollback if an error occurs, in my opinion
it's best to set the `autocommit` parameter to `sqlite3.connect()` to `True` and
not `False` as [recommended by the
documentation](https://docs.python.org/3/library/sqlite3.html#transaction-control-via-the-autocommit-attribute).
When you do this, you need to issue `BEGIN`, `COMMIT`, and `ROLLBACK` commands
yourself. This gives you complete control over transactions.

## A Demonstration Script

This demonstration script requires Python 3.12 or 3.13 since the `autocommit`
parameter was introduced in Python 3.12 and results in an exception in Python
3.11 and earlier. You can copy the script by clicking on the clipboard icon at
the top right of the code block window below.

See my additional notes below the script.

```python
"""
INTRODUCTION

This is an example of using Python 3.12 or 3.13 to create a SQLite database.
The script creates a sqlite3 database in the file db.sqlite3.

This script meets the following requirements:
    -   it manages database transactions, allowing a complete rollback when
        requested
    -   it enforces foreign key constraints

This script manages database transactions by issuing explicit "BEGIN" 
and "COMMIT" or "ROLLBACK" commands.

This script enforces foreign key constraints by issuing a
"PRAGMA foreign_keys = ON" command.

The script later tests the foreign key constraint and catches the exception.

EXAMPLES

# Roll back all changes.
$ python3 create_db.py rollback

# Commit all changes.
$ python3 create_db.py commit
"""


import os
import sqlite3
import sys


def save_data(db, commit_flag):
    # Open a connection, enforce foreign key constraints, and begin a
    # transaction.
    conn = sqlite3.connect(database=db, autocommit=True)
    conn.execute("PRAGMA foreign_keys = ON")
    conn.execute("BEGIN TRANSACTION")

    # Wrap all database interactions in a try...except block.
    # This guarantees the database will be rolled back if an
    # error occurs.
    try:
        # Create two tables.
        print("Creating tables and inserting data...")
        sql01 = """
            CREATE TABLE IF NOT EXISTS
            authors (
                id INTEGER PRIMARY KEY,
                first_name TEXT,
                last_name TEXT)"""
        conn.execute(sql01)
        
        sql02 = """
            CREATE TABLE IF NOT EXISTS
            books (
                id INTEGER PRIMARY KEY,
                title TEXT,
                author_id INTEGER,
                FOREIGN KEY(author_id) REFERENCES authors(id))"""
        conn.execute(sql02)

        # Insert data into the two tables, establishing the foreign
        # key for each row in table books.
        books_list = [
            ("Dawn", "Octavia", "Butler"),
            ("Foundation", "Isaac", "Asimov"),
            ("Ancillary Justice", "Anne", "Leckie"),
        ]
        sql03 = """
            INSERT INTO authors
            ( first_name, last_name )
            VALUES (?, ?)"""
        sql04 = """
            INSERT INTO books
            ( title, author_id )
            VALUES (?, ?)"""

        cur = conn.cursor()
        for book_row in books_list:
            (title, first_name, last_name) = book_row
            cur.execute(sql03, (first_name, last_name,))
            author_id = cur.lastrowid
            cur.execute(sql04, (title, author_id,))
        cur.close()

        # Attempt to delete a row from table authors.
        # This should raise a sqlite3.IntegrityError exception.
        print("Attempting to violate foreign key constraint...")
        sql08 = """
            DELETE from authors
            WHERE id = 1
        """
        try:
            conn.execute(sql08)
            print("  Foreign key constraint was not enforced.")
        except sqlite3.IntegrityError as exc:
            print("  Foreign key constraint was enforced.")
            print("  This exception was caught:")
            print("  ", type(exc).__name__, ": ", exc, sep="")

        # Commit or roll back database changes. If the rollback is successful,
        # the size of the database file will be 0 bytes.
        if commit_flag:
            print(f"commit_flag is {commit_flag}: Committing changes...")
            conn.execute("COMMIT")
            print("  Done.")
        else:
            print(f"commit_flag is {commit_flag}: Rolling back changes...")
            conn.execute("ROLLBACK")
            print("  Done.")
    
    except Exception as exc:
        # Roll back changes if any exception occurred.
        print("An exception was raised. Rolling back changes...")
        conn.execute("ROLLBACK")
        print("  Done.")
        print(f"The exception was {exc}.")

    # Clean up.
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
    file_size = os.path.getsize(db)
    print(f"The size of the database file {db} is {file_size}.")

if __name__ == "__main__":
    main()
```

## Running the Script

In these examples, the foreign key constraint in the `book` table is tested and
the `sqlite3.IntegrityError` exception is caught.

### Rolling Back Changes

With the `rollback` argument, the tables are created and loaded, but all changes
are rolled back, and no data is saved in the database:

```console
$ python3 create_db.py rollback
Creating tables and inserting data...
Attempting to violate foreign key constraint...
  Foreign key constraint was enforced.
  This exception was caught:
  IntegrityError: FOREIGN KEY constraint failed
commit_flag is False: Rolling back changes...
  Done.
The size of the database file db.sqlite3 is 0.
```

### Committing Changes

With the `commit` argument, all changes are committed to the database.

```console
$ python3 create_db.py commit
Creating tables and inserting data...
Attempting to violate foreign key constraint...
  Foreign key constraint was enforced.
  This exception was caught:
  IntegrityError: FOREIGN KEY constraint failed
commit_flag is True: Committing changes...
  Done.
The size of the database file db.sqlite3 is 12288.
```

## Examing the Database

Use the `sqlite3` command line utility to view the data in the database.

```console
$ /opt/homebrew/opt/sqlite3/bin/sqlite3 db.sqlite3
SQLite version 3.49.1 2025-02-18 13:38:58
Enter ".help" for usage hints.

sqlite> .schema
CREATE TABLE authors (
                id INTEGER PRIMARY KEY,
                first_name TEXT,
                last_name TEXT);
CREATE TABLE books (
                id INTEGER PRIMARY KEY,
                title TEXT,
                author_id INTEGER,
                FOREIGN KEY(author_id) REFERENCES authors(id));

sqlite> .mode columns

sqlite> select * from books;
id  title              author_id
--  -----------------  ---------
1   Dawn               1
2   Foundation         2
3   Ancillary Justice  3

sqlite> select * from authors;
id  first_name  last_name
--  ----------  ---------
1   Octavia     Butler
2   Isaac       Asimov
3   Anne        Leckie

sqlite> .exit
```

## Versions

-   Python 3.13.2
-   sqlite3 3.49.1
-   macOS 15.3.2
