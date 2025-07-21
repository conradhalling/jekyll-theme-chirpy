---
title: Web Bots and Crawlers (Part 2)
description: More than 96% of traffic on my websites is from web bots and crawlers.
author: conrad
date: 2025-07-21 14:37:00 -0400
categories: [Computing]
---

## Introduction

This is a long overdue followup about monitoring the
[Apache httpd](https://httpd.apache.org/)
access log files on my websites
[conradhalling.com](https://conradhalling.com/blog/)
and
[sphaerula.com](https://sphaerula.com/blog/)
to determine how much traffic on these websites comes from web bots and
crawlers.

## Data Collection and Reporting

Since my post of April 17, 2025,
[Web Bots and Crawlers (Part 1)](/blog/posts/web-bots-and-crawlers/),
I have extracted data from the access log files into a SQLite3 database, I have
built tools on my local computer to analyze the data and store the results, and
I have built and installed a public reporting tool,
[Crawler Activity](https://sphaerula.com/cgi/crawlers.cgi?summary).

## Analysis and First Results

The [Summary](https://sphaerula.com/cgi/crawlers.cgi?summary) page reports
today that at least 96% of the page requests on my websites come from
crawlers.

More than 25% of all requests are unproductive, returning 404 (page
not found), 403 (forbidden), 429 (too many requests), or other errors.

As reported by the
[Crawlers](https://sphaerula.com/cgi/crawlers.cgi?crawlers)
page, the data includes requests from 114 crawlers that identify themselves,
from three anonymous crawlers that I have given names to based on their
behaviors, and from an unknown number of unidentified and anonymous crawlers
that fall into nine different classes based on what they're requesting.

## Good Crawlers, Bad Crawlers

The data shows good and bad behavior by crawlers. Good crawlers identify
themselves in the user agent string, check the /robots.txt file before crawling
the site, use IP addresses that have host names that help identify the crawler
or company making the requests, and don't make too many requests in too short
a time.

Bad crawlers exhibit one or more undesireable behaviors. For example, many do
not identify themselves in the user agent string. Many set the user agent string
to "-", others use a user agent string from a browser, and some use a user agent
string from another crawler. Some crawlers never check the /robots.txt file.
Some crawlers use multiple user agent strings. Some crawlers use multiple IP
addresses that do not report their host names, and they make very few requests
from any one IP address in what appears to be an effort to prevent blocking.

## Protection by Hosting Providers

My
[conradhalling.com](https://conradhalling.com/blog/)
website is hosted
by DreamHost. I can see from my data that DreamHost provides some protections
against aggressive crawling, such as returning a 403 (forbidden) error for
certain requests or a 429 (too many requests) error when the request rate grows
too high.

My [sphaerula.com](https://sphaerula.com/blog/) website is hosted by
GoDaddy, which doesn't block high request rates but which returns 403
(forbidden) for certain crawlers.

## Future Plans

I am writing additional analysis tools that will make it easier for me to
identify patterns of behavior for anonymous crawlers. I want to create some
visualizations of the data. And once the code base is stable, I plan to make the
source code public in my GibHub repository.

In subsequent posts, I will delve into details.
