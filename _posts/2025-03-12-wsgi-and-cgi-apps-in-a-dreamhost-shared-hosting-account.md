---
title: WSGI and CGI Apps in a DreamHost Shared Hosting Account
description: I experimented with running WSGI and CGI apps in my DreamHost shared hosting account.
author: conrad
date: 2025-03-12 17:50:00 -0400
categories: [Computing]
---

## Introduction

As I have mentioned before, for my
[conradhalling.com](https://conradhalling.com) website, which is hosted by
[DreamHost](https://www.dreamhost.com), I am working on a Python-based web
application. DreamHost supports CGI applications but [no longer supports WSGI
applications](https://www.reddit.com/r/dreamhost/comments/19fjg18/passenger_removed/). 

I'm comfortable with writing a CGI application, but it's still possible to
create a WSGI application using
[Flask](https://flask.palletsprojects.com/en/stable/) and run it as a CGI
application.

In the Python world, it is getting more difficult to write CGI applications
simply because the modules and documentation are disappearing. The Python `cgi`
and `cgitb` modules were deprecated in Python 3.11 and removed from Python
3.13. The instructions for deploying a Flask application using CGI were removed
from the Flask documentation in July 2022 with version 2.1.3. These changes are
unfortunate because there is still a use case for CGI in a shared hosting
account where WSGI is not available.

Fortunately for Python 3.13 users, the `cgi` and `cgitb` modules were moved to
the [`legacy-cgi` package](https://pypi.org/project/legacy-cgi/), which is
available from the [Python Packaging Index (PyPI)](https://pypi.org/). Since I'm
working on a server that has Python 3.10 installed, the `cgi` and `cgitb`
modules are readily available.

## An Example CGI Script

[On the DreamHost
server](https://help.dreamhost.com/hc/en-us/articles/216128557-Guidelines-for-setting-up-a-Python-file-at-DreamHost),
CGI scripts needed to have a file extension of `.py` or `.cgi` and needed to be
executable. I made the scripts executable by providing a `#!` line containing
the path to Python and setting the file's mode to `755`, as in this example,
where the script was named `helloworld.py`.

```console
$ chmod 755 helloworld.py
```

On [conradhalling.com](https://conradhalling.com/), I installed a simple "hello
world" CGI script and tested it successfully at
<https://conradhalling.com/projects/helloworld.py>. The script was:

```python
#!/usr/bin/env python
print("Content-type: text/html\n");
print("Hello, World, from Python CGI!");
```

## Running WSGI Scripts as CGI

The documentation for Python's
[`wsgiref`](https://docs.python.org/3.13/library/wsgiref.html) module didn't
provide a working example, but it was possible to use
[`wsgiref.handlers.CGIHandler`](https://docs.python.org/3.13/library/wsgiref.html#wsgiref.handlers.CGIHandler)
to run a WSGI app as a CGI app.

### A Basic WSGI App

As a working example, I was fortunate to find a basic WSGI script by
Keith Gaughan at [Running a
Python WSGI application as a CGI script](https://tilde.club/~talideon/wsgi-cgi/).
I tested the script successfully at
<https://conradhalling.com/projects/helloworld_wsgi.py>. The script, which
conformed exactly to the WSGI specifications described in [PEP
3333](https://peps.python.org/pep-3333/), was:

```python
#!/usr/bin/env python3

from wsgiref.handlers import CGIHandler

def simple_app(environ, start_response):
    status = "200 OK"
    headers = [("Content-Type", "text/plain")]
    start_response(status, headers)
    return [b"Hello, world, from WSGI running as CGI!"]

CGIHandler().run(simple_app)
```

### A Flask App Modified to Run as CGI

For this example, I modified the [Flask Quickstart
application](https://flask.palletsprojects.com/en/stable/quickstart/) to run under CGI
at <https://conradhalling.com/projects/helloworld_flask.py>. Note that I changed
the `#!` line to use the Python virtual environment into which I had installed
flask.

```python
#!/home/conhal1/venv/bin/python3

from flask import Flask
from wsgiref.handlers import CGIHandler

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<p>Hello, World, from Flask!</p>"

CGIHandler().run(app)
```

### A Flask App Run by a CGI Script

Finally, I borrowed this example from [Running Flask and Python with
CGI](https://stackoverflow.com/questions/64580682/running-flask-and-python-with-cgi).
Here, the flask application is contained in its own module without any commands
that run it. A second script imports the Flask application and runs it using
`wsgiref.handlers.CGIHandler`.

This is the module containing the [Flask Quickstart
application](https://flask.palletsprojects.com/en/stable/quickstart/) in a file
named `myapplication.py`.

```python
from flask import Flask
app = Flask(__name__)
@app.route('/')
def index():
    return '<h1>Hello, World, from a Flask module!</h1>'
```

And this was the CGI application file, called `myapplication.cgi`, which
ran at <https://conradhalling.com/projects/myapplication.cgi>.

```python
#!/home/conhal1/venv/bin/python3
from wsgiref.handlers import CGIHandler
from myapplication import app

CGIHandler().run(app)
```
