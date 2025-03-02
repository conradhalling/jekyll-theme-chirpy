---
title: Python Virtual Environments in a DreamHost Shared Hosting Account
description: I experimented with Python virtual environments in my dreamhost.com shared hosting account.
author: conrad
date: 2025-03-02 12:25:00 -0500
categories: [Computing]
---

## Summary

For my [conradhalling.com](https://conradhalling.com) website, which is hosted
by [DreamHost](https://www.dreamhost.com), I am working on a Python-based web
application that requires the
[argon2-cffi](https://pypi.org/project/argon2-cffi/) package for
[password hashing](https://argon2-cffi.readthedocs.io/en/stable/). However, the
`argon2-cffi` package was not installed for the system python.

I experimented with creating a Python virtual environment in which I could
install and use `argon2-cffi`. I tried using the `virtualenv` module and the `venv` module
and succeeded in creating a virtual environment with both.

## Testing the System Python

I logged into my DreamHost account, started the system Python, and attempted
to import the `argon2` module. It was not installed.

```console
$ ssh cxxxxx1@ixxxxxxxxxxxxxxx3.dreamhost.com

$ python3 --version
Python 3.10.12

$ python3
Python 3.10.12 (main, Feb  4 2025, 14:57:36) [GCC 11.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import argon2
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
ModuleNotFoundError: No module named 'argon2'
>>> exit()
```

## virtualenv

DreamHost's
[documentation](https://help.dreamhost.com/hc/en-us/articles/115000695551-Installing-and-using-virtualenv-with-Python-3)
recommended using `virtualenv` for setting up a Python virtual environment.

I created a virtual environment in the `~/venv` directory and activated it. When
the virtual environment was active, the shell printed `(venv)` above the prompt.

```console
$ python3 -m virtualenv venv
created virtual environment CPython3.10.12.final.0-64 in 326ms
[output omitted]

$ source venv/bin/activate

(venv)
$
```

I upgraded the installations of the `pip`, `setuptools`, and `wheel` packages.

```console
(venv)
$ pip3 list
Package    Version
---------- -------
pip        22.0.2
setuptools 59.6.0
wheel      0.37.1

(venv)
$ pip3 install --upgrade pip setuptools wheel
[output omitted]
Successfully installed pip-25.0.1 setuptools-75.8.2 wheel-0.45.1
```

I installed the `argon2-cffi` package as recommended by the
[documentation](https://argon2-cffi.readthedocs.io/en/stable/installation.html).
I successfully tested importing the `argon2` module.

```console
(venv)
$ python3 -Im pip install argon2-cffi
[output omitted]
Successfully installed argon2-cffi-23.1.0 argon2-cffi-bindings-21.2.0 cffi-1.17.1
pycparser-2.22

(venv)
$ python3
Python 3.10.12 (main, Feb  4 2025, 14:57:36) [GCC 11.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import argon2
>>> exit()

(venv)
$
```

I deactivated the virtual environment and deleted it since I wanted to test
using the `venv` module to set up a virtual environment.

```console
(venv)
$ deactivate

$ rm -rf venv
```

## venv

I tried to create a virtual environment using `venv`, but this failed because
the `ensurepip` package was not installed.

```console
$ python3 -m venv venv
The virtual environment was not created successfully because ensurepip is not
available.  On Debian/Ubuntu systems, you need to install the python3-venv
package using the following command.

    apt install python3.10-venv

You may need to use sudo with that command.  After installing the python3-venv
package, recreate your virtual environment.

Failing command: /home/cxxxxx1/venv/bin/python3

$ rm -rf venv
```

The [`pip` documentation](https://pip.pypa.io/en/stable/installation/) described
how to install `pip` if `ensurepip` was not available.

I downloaded the `get-pip.py` script. I created a virtual environment without
`pip` in the `~/venv` directory and activated the environment. I ran the
`get-pip.py` script, which installed `pip`, `setuptools`, and `wheel` packages
in the virtual environment. As before, when the virtual environment was active,
the shell printed `(venv)` above the prompt.

```console
$ curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
    % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                    Dload  Upload   Total   Spent    Left  Speed
100 2246k  100 2246k    0     0  7565k      0 --:--:-- --:--:-- --:--:-- 7563k

$ python3 -m venv --without-pip venv

$ source venv/bin/activate

(venv)
$ python3 get-pip.py
[output omitted]
Successfully installed pip-25.0.1 setuptools-75.8.2 wheel-0.45.1

(venv)
$
```

I installed `argon2-cffi` and was able to import the `argon2` module successfully.

```console
(venv)
$ python3 -Im pip install argon2-cffi
[output omitted]
Successfully installed argon2-cffi-23.1.0 argon2-cffi-bindings-21.2.0 cffi-1.17.1
pycparser-2.22

(venv)
$ python3
Python 3.10.12 (main, Feb  4 2025, 14:57:36) [GCC 11.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import argon2
>>> exit()

(venv)
$
```

I deactivated the virtual environment and deleted it because my testing was
complete.

```console
(venv)
$ deactivate

$ rm -rf venv

$
```
