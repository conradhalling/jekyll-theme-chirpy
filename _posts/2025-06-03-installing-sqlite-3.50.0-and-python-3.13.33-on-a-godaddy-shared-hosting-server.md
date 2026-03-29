---
title: Installing SQLite 3.50.0 and Python 3.13.3 on a GoDaddy Shared Hosting Server
description: I needed SQLite 3.50.0 and Python 3.13.3 in my account on a GoDaddy shared hosting server. Here's how I compiled and installed them.
author: conrad
date: 2025-06-03 14:19:00 -0400
categories: [Computing]
---

## Introduction

I have been building a database and web tool for tracking the web crawlers
that are indexing and scraping my websites at
[conradhalling.com](https://conradhalling.com/blog/) and
[sphaerula.com](https://sphaerula.com/blog/). I am now hosting the
[Crawler Activity](https://sphaerula.com/cgi/crawlers.cgi) reporting tool on
[sphaerula.com](https://sphaerula.com/blog/), which is hosted by
[GoDaddy](https://godaddy.com).

I have been developing the tool using
[SQLite 3.50.0](https://sqlite.org) and
[Python 3.13.3](https://www.python.org/) on my M4 Pro MacBook Pro. But the
GoDaddy shared hosting server has SQLite 3.26.0 and Python 3.6.8 installed.
These were too old for my requirements.

```console
$ ssh yxxxxxxxxxx9@sphaerula.com
$ /usr/bin/python3 --version
Python 3.6.8
$ /usr/bin/sqlite3 --version
3.26.0 2018-12-01 12:34:55 bf8c1b2b7a5960c282e543b9c293686dccff272512d08865f4600fb58238alt1
```

To support my web application, I needed to compile and install SQLite 3.50.0 and
Python 3.13.3. GoDaddy kindly provides excellent directions for installing
Python 3,
[How to install and configure Python on a hosted server](https://www.godaddy.com/resources/skills/how-to-install-and-configure-python-on-a-hosted-server).
But when I followed those directions, I discovered that Python was not linked to
SQLite 3, and I couldn't import the
[sqlite3 package](https://docs.python.org/3/library/sqlite3.html).

After five tries, I succeeded in installing SQLite 3.50.0 and Python
3.13.3. These were the steps I used.

## Create the `local` Directory

I chose to install the software into a directory named `local` in my account.
(The directions provided by GoDaddy install Python into a hidden directory named
`.local`, but I preferred not to hide the directory.) I created the directory.

```console
$ mkdir local
```

## Configure Environment Variables

In my experience, compiling and installing software in a Linux environment is a
tricky and error-prone business. (I used to do this regularly at `$JOB` when I
was working as a bioinformatics scientist before I retired.) It is necessary to
initialize some environment variables to get different software installations to
recognize one another. (I won't go into the details here about why these
environment variables are needed.)

I modified my `.bash_profile` file using the `nano` editor to add the following
lines:

```shell
# Configure the environment for compiling and installing software
# into ${HOME}/local:
export PATH=${HOME}/local/bin:${PATH}:${HOME}/bin
export PKG_CONFIG_PATH=${HOME}/local/lib/pkgconfig:${PKG_CONFIG_PATH}
export CFLAGS=-I${HOME}/local/include
export LDFLAGS=-"L${HOME}/local/lib -Wl,-rpath,${HOME}/local/lib"
```

I logged out and logged in again to my account on the shared hosting server
to initialize these variables properly in my working environment.

## Install readline 8.2

The first time I compiled and installed SQLite 3, it wasn't linked to the
[readline library](https://tiswww.case.edu/php/chet/readline/rltop.html),
meaning I couldn't efficiently recover previous SQLite3 commands by pressing
the up arrow key on my keyboard. Consequently, these directions begin with
installing readline 8.2.

I downloaded the tarball, uncompressed it, configured the build, compiled
the software, and installed the software using the following commands:

```console
$ cd
$ wget http://git.savannah.gnu.org/cgit/readline.git/snapshot/readline-master.tar.gz
$ tar -xzf readline-master.tgz
$ cd readline-master
$ ./configure --prefix=${HOME}/local
$ make
$ make install
```

I cleaned up my home directory.

```console
$ cd
$ rm -rf readline-master
$ rm readline-master.tgz
```

## Install SQLite 3.50.0

SQLite 3.50.0 provides directions, [Compiling for Unix-like
systems](https://github.com/sqlite/sqlite?tab=readme-ov-file#compiling-for-unix-like-systems),
for compiling and installing the software. I followed these fairly closely.

I downloaded the source code, extracted it, and built the software in a
separate directory named `bld` as instructed. I compiled and installed
SQLite 3.

```console
$ wget https://sqlite.org/2025/sqlite-autoconf-3500000.tar.gz
$ tar -xzf sqlite-autoconf-3500000.tar.gz
$ mkdir bld
$ cd bld
$ ../sqlite-autoconf-3500000/configure --help
$ ../sqlite-autoconf-3500000/configure --prefix=${HOME}/local
Checking libs for readline...-lreadline
Using readline flags: -I/home/xxxxxxxxxxx9/local/include -lreadline -lncurses
Line-editing support for the sqlite3 shell: readline
$ make
$ make install
```

I confirmed that the shell could find SQLite 3.50.0.

```console
$ cd
$ which sqlite3
~/local/bin/sqlite3
$ sqlite3 --version
3.50.0 2025-05-29 14:26:00 dfc790f998f450d9c35e3ba1c8c89c17466cb559f87b0239e4aab9d34e28f742 (64-bit)
```

I tested that SQLite 3 worked correctly by opening my database file,
`data/crawlers.sqlite3`, and executing three commands.

```console
$ sqlite3 data/crawlers.sqlite3
SQLite version 3.50.0 2025-05-29 14:26:00
Enter ".help" for usage hints.
sqlite> .schema tbl_domain
CREATE TABLE tbl_domain
        (
            id INTEGER PRIMARY KEY,
            name TEXT NOT NULL UNIQUE,
            log_file_glob_pattern TEXT NOT NULL
        ) STRICT
    ;
sqlite> .mode columns
sqlite> select * from tbl_domain;
id  name               access_log_file_glob_pattern
--  -----------------  ----------------------------
1   conradhalling.com  access.log.*.gz
2   sphaerula.com      sphaerula.com-*.gz
```

I confirmed that I could recover a previous command by pressing the up arrow
on my keyboard three times to recover the `.schema tbl_domain` command, which
I executed again. I quit sqlite.

```console
sqlite> .schema tbl_domain
CREATE TABLE tbl_domain
        (
            id INTEGER PRIMARY KEY,
            name TEXT NOT NULL UNIQUE,
            log_file_glob_pattern TEXT NOT NULL
        ) STRICT
    ;
sqlite> .quit
```

I cleaned up my home directory.

```console
$ cd
$ rm -rf bld
$ rm -rf sqlite-autoconf-3500000
$ rm sqlite-autoconf-3500000.tar.gz
```

## Install Python 3.13.3

I downloaded the source code and configured, compiled, and installed Python
3.13.3. I carefully checked the output from the `./configure` utility to make
sure SQLite3 was recognized for linking (as shown in the console output below).

```console
$ cd
$ wget https://www.python.org/ftp/python/3.13.3/Python-3.13.3.tgz
$ tar -xzf Python-3.13.3.tgz
$ cd Python-3.13.3
$ ./configure --help
$ ./configure --prefix=${HOME}/local
[output omitted]
checking for sqlite3 >= 3.15.2... yes
checking for sqlite3.h... yes
checking for sqlite3_bind_double in -lsqlite3... yes
checking for sqlite3_column_decltype in -lsqlite3... yes
checking for sqlite3_column_double in -lsqlite3... yes
checking for sqlite3_complete in -lsqlite3... yes
checking for sqlite3_progress_handler in -lsqlite3... yes
checking for sqlite3_result_double in -lsqlite3... yes
checking for sqlite3_set_authorizer in -lsqlite3... yes
checking for sqlite3_trace_v2 in -lsqlite3... yes
checking for sqlite3_value_double in -lsqlite3... yes
checking for sqlite3_load_extension in -lsqlite3... yes
checking for sqlite3_serialize in -lsqlite3... yes
[output omitted]
checking for stdlib extension module _sqlite3... yes
$ make
$ make install
```

I checked that the shell could find Python 3.13.3, that I could start
Python 3.13.3, and that I could import and use the `sqlite3` package.

```console
$ cd
$ which python3
~/local/bin/python3
$ python3 --version
Python 3.13.3
$ python3
Python 3.13.3 (main, Jun  2 2025, 18:56:45) [GCC 8.5.0 20210514 (Red Hat 8.5.0-26)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import sqlite3
>>> sqlite3.sqlite_version
'3.50.0'
>>> exit()
```

I cleaned up my home directory.

```console
$ cd
$ rm -rf Python-3.13.3
$ rm Python-3.13.3.tgz
```

## Set Up a Virtual Environment

I used the `venv` module to create a virtual environment, `${HOME}/venv313`,
into which I could install external packages.

```console
$ cd
$ python3 -m venv venv313
```

## Install Python Packages

I initialized the virtual environment and installed the packages I needed
for the Crawler Activity tool.

```console
$ source ~/venv/bin/activate
$ pip install --upgrade pip
$ pip install python-dotenv legacy-cgi
$ pip list
Package       Version
------------- -------
legacy-cgi    2.6.3
pip           25.1.1
python-dotenv 1.1.0
```

## Set the #! Line of the CGI Script

CGI scripts that used Python 3.13.3 required a `#!` line with the correct
path. The path was:

`#!/home/yxxxxxxxxxx9/venv313/bin/python3`

Starting the CGI script with this `#!` path meant Python could find the external
packages installed in the virtual environment.
