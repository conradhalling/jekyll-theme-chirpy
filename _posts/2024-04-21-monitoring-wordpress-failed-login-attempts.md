---
title: Monitoring WordPress Failed Login Attempts
author: conrad
date: 2024-04-21 22:19:00 -0500
description: Bad actors on the internet attempt to log in to WordPress sites. I use a plugin to slow them down.
categories: [Blogging]
media_subpath: /assets/img/2024-04-21/
---

For simplicity and convenience, I use [WordPress](https://wordpress.org/) as the
content management service for this site and for my other site at
[conradhalling.com](https://conradhalling.com/). The hosting service for each of
my sites provides tools that simplify installing and updating WordPress.

A standard WordPress installation has its login page at wp-login.php, and
nefarious actors on the web know this and will attempt to log in to and hack
WordPress sites via the login page.

For both of my WordPress installations, I am using the free version of a plugin
called [Limit Login Attempts Reloaded](https://www.limitloginattempts.com/).
This plugin records the IP address of a failed login and blocks repeated login
attempts. Using the plugin’s settings, a WordPress administrator can limit the
number of failed login attempts from a given IP address over a given time
interval.

My sites are hobby sites with very low traffic, so I am surprised at the number
of attempted logins. I expect that this is all automated using bots, but I’m not
curious enough right now to find out these are implemented.

At conradhalling.com, the number of failed attempts is still increasing daily:
![A chart of failed WordPress login attempts at
conradhalling.com](failed-login-attempts-conradhalling.png){:
width="1402" height="652" .w-100 .normal}_Failed login attempts at conradhalling.com._

On this site, sphaerula.com, the number of failed login attempts peaked a few
days ago at more than ninety, and the number has diminished since then: ![A
chart of failed WordPress login attempts at
sphaerula.com.](failed-login-attempts-sphaerula.png){:
width="1380" height="652" .w-100 .normal}_Failed login attempts at
sphaerula.com._

I am not worried about these login attempts since for both sites I have used
long pseudorandom passwords that are effectively unguessable.
