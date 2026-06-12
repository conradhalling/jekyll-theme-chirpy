---
title: "PHP & MySQL"
description: "These are my notes about the book “PHP & MySQL” by Jon Duckett."
date: 2025-01-15 10:20:00 -0500
author: conrad
categories: [Computing]
---

I am considering building a web application that provides a user interface for
storing information about the hundreds of audiobooks I have read, and I want to
install it on my website at
[conradhalling.com](https://conradhalling.com/).
My website is hosted by
[DreamHost](https://www.dreamhost.com/),
who provide modern versions of
[PHP](https://www.php.net)
for web development. DreamHost also makes
[MySQL](https://www.mysql.com)
available, primarily for support of
[WordPress](https://wordpress.org/download/)
installations, and I could use that for the database back end.

Late last month (December 2024), when I looked into
[books for learning PHP]( {% link _posts/2024-12-28-books-for-learning-php.md %} ),
I chose
<cite>[PHP and MySQL: Server-side Web Development](https://phpandmysql.com/)</cite>
by Jon Duckett (published in January, 2022). I ordered the book from my local
bookstore,
[Porter Square Books](https://www.portersquarebooks.com/),
and it arrived in just a few days. The physical book is printed on heavy,
high-quality paper and weighs three pounds six ounces!

It is twenty years since I used PHP for a project, and I didn't like the
language then (in those days I preferred Perl), but I have read that PHP has
been modernized and greatly improved. I already have extensive experience using
MySQL.

The book provides a modern presentation of PHP rather than being a revision of
an old book with outdated code. The book presents a complete example of a PHP
web application.

The book is divided into four parts:

<ol type="A">
    <li>Basic Programming Instructions</li>
    <li>Dynamic Web Pages</li>
    <li>Database Driven Websites</li>
    <li>Extending the Sample Application</li>
</ol>

The book has several important additions:

-   It teaches how to use [phpmyadmin](https://www.phpmyadmin.net) for managing MySQL databases.
-   It provides good instructions for creating a local SSL certificate for testing https connections.
-   It teaches how to store user login credentials using the latest encryption methods.

I am old-fashioned in believing that I learn faster by doing, so I am entering
the source code myself using the
[Visual Studio Code editor](https://code.visualstudio.com),
but all of the source code for the book is available at the book's website.

I am working on my M1 MacBook Pro, using a local installation of the
[Apache httpd web server](https://httpd.apache.org/)
and the MySQL database server. I have installed PHP, Apache httpd, and
MySQL using the
[Homebrew package manager](https://brew.sh).
Having a local installation has enabled rapid development.

I recommend this book highly.

Rating: Five of five stars (excellent)
