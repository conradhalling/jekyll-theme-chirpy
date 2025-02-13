---
title: Using Apache httpd 2.4 mod_rewrite
description: Here's an Apache httpd 2.4 mod_rewrite formula for my blogs.
author: conrad
date: 2025-02-12 19:03:00 -0500
categories: [Jekyll]
---

I recorded this here because I couldn't find a good recommendation for a rewrite
rule to put in the `.htaccess` file in the document root of my websites at
[sphaerula.com](https://sphaerula.com) and
[conradhalling.com](https://conradhalling.com).

My issue was that I built my Jekyll sites on my MacBook Pro and synchronized them
to my web hosting accounts using the `rsync` utility. I synchronized the files
into the `blog/` subdirectory to avoid overwriting or deleting any files in the
document root directory. But I wanted my blog to appear when a user went to my
website.

My use case was, for example, that the user would enter <https://sphaerula.com>
into their web browser, but the website would redirect them to
<https://sphaerula.com/blog/>. I needed a rewrite rule to put in the `.htaccess`
file in the document root folder that would accomplish this.

I spent an afternoon of trial and error, reading many stackoverflow entries,
blog pages, and even the [Apache httpd
documentation](https://httpd.apache.org/docs/2.4/mod/mod_rewrite.html), but I
could not find a formula that actually worked. What appeared to be the exact
solution I needed was presented in the [Moved
DocumentRoot](https://httpd.apache.org/docs/2.4/rewrite/remapping.html#moveddocroot)
section of the Apache httpd documentation, but it did not work!

After intense debugging on my Mac using `LogLevel alert rewrite:trace3` in my
Apache httpd configuration file, as recommended in the [Apache Module
mod_rewrite |
Logging](https://httpd.apache.org/docs/2.4/mod/mod_rewrite.html#logging)
section, I figured out a rewrite rule that worked:

```apache
RewriteEngine on
RewriteRule ^$ /blog/ [R]
```
{: file='.htaccess'}

For an `.htaccess` file at the document
root, the RewriteEngine was working in the per-directory context. The [Apache
httpd
documentation](https://httpd.apache.org/docs/2.4/mod/mod_rewrite.html#rewriterule)
stated:

> In per-directory context (`Directory` and `.htaccess`), the *Pattern* is matched
> against only a partial path, for example a request of "/app1/index.html" may
> result in comparison against "app1/index.html" or "index.html" depending on
> where the RewriteRule is defined.

This rewrite rule was pretty simple. The syntax for a rewrite rule was
`RewriteRule Pattern Substitution [flags]`.

*Pattern*: Since the path for the document root was `/`, but in per-directory context, the
leading `/` was stripped off, the rewrite rule received an empty string. The pattern
for matching an empty string was `^$`.

*Substitution*: The substitution string was `/blog/` with
the leading and trailing `/` characters (because that was what worked
empirically).

*Flags*: Finally, the `[R]` flag indicated a redirect.

Surrounding the pattern and substitution strings with double quote characters
broke the rewrite rule (despite many examples in the documentation using quoted
strings).

I installed this `.htaccess` file in the document root directory of each of my
websites, and it worked correctly to redirect HTTP requests to the `blog/`
subdirectory, as desired.
