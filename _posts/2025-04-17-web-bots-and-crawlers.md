---
title: Web Bots and Crawlers
description: Most traffic on one of my websites is from web bots and crawlers.
author: conrad
date: 2025-04-17 09:14:00 -0400
categories: [Computing]
---

## Introduction

I have started monitoring the [Apache httpd](https://httpd.apache.org/) access
log files on my website [conradhalling.com](https://conradhalling.com/blog/),
which is hosted by [DreamHost](https://www.dreamhost.com). My
[conradhalling.com](https://conradhalling.com/blog/) site is a hobby blog site
with posts and data that will interest very few people other than me.

But the site is averaging more than a thousand requests a day, and I want to
know where the traffic is coming from. For example, on Monday, April 14, 2025,
there were 1,523 requests for HTML pages. (My counts ignore requests for
auxiliary files such as CSS, JavaScript, font, and image files.)

## Analysis and Preliminary Results

I have started building a Python-based tool and database for collecting and
analyzing the access log data. In my preliminary analysis, at least 82% of the
traffic on the site is from web bots and crawlers. I predict that when my
analysis is finished, I will find that more than 95% of the traffic is from web
bots and crawlers. I will make another report in a week or two when I have
completed my analysis.

## Self-Identifying Bots

On April 14, 2025, the following self-identifying bots made requests:

| Bot    | URL    |
| ------ | ------ |
| AhrefsBot/7.0                   | [http://ahrefs.com/robot/](http://ahrefs.com/robot/) |
| Amazonbot/0.1                   | [https://developer.amazon.com/support/amazonbot](https://developer.amazon.com/support/amazonbot) |
| bingbot/2.0                     | [http://www.bing.com/bingbot.htm](http://www.bing.com/bingbot.htm) |
| Bytespider                      | [https://zhanzhang.toutiao.com/](https://zhanzhang.toutiao.com/) |
| ChatGPT-User/1.0                | [https://openai.com/bot](https://openai.com/bot) |
| CheckMarkNetwork/1.0            | [http://www.checkmarknetwork.com/spider.html](http://www.checkmarknetwork.com/spider.html) |
| DreamHost Data Team             | [http://www.dreamhost.com/support/](http://www.dreamhost.com/support/) |
| facebookexternalhit/1.1         | [http://www.facebook.com/externalhit_uatext.php](http://www.facebook.com/externalhit_uatext.php) |
| Googlebot/2.1                   | [http://www.google.com/bot.html](http://www.google.com/bot.html) |
| linkbot 1.0                     | [http://suite.seozoom.it/bot.html](http://suite.seozoom.it/bot.html) |
| meta-externalagent/1.1          | [https://developers.facebook.com/docs/sharing/webmasters/crawler](https://developers.facebook.com/docs/sharing/webmasters/crawler) |
| MJ12bot/v1.4.8                  | [http://mj12bot.com/](http://mj12bot.com/) |
| OAI-SearchBot/1.0               | [https://openai.com/searchbot](https://openai.com/searchbot) |
| SemrushBot/7~bl                 | [http://www.semrush.com/bot.html](http://www.semrush.com/bot.html) |
| SeznamBot/4.0                   | [https://o-seznam.cz/napoveda/vyhledavani/en/seznambot-crawler/](https://o-seznam.cz/napoveda/vyhledavani/en/seznambot-crawler/) |

## Anonymous Bots

In addition to self-identifying bots, there are many anonymous bots, some
nefarious. Based on their search patterns, I have so far grouped anonymous bots
into five categories:

| Anonymous Bot Type | Search Pattern |
| ------------------ | ----------- |
| Indexer/Scraper        | Identifies HTML pages for indexing and/or scraping |
| WordPress Explorer     | Tests for the existence of WordPress application, configuration, and data files |
| Directory Explorer     | Tests for the existence of specific directories |
| PHP Explorer           | Tests for the existence of specific PHP scripts and configuration files |
| Configuration Explorer | Tests for the existence of specific configuration files |

The anonymous explorer bots are wasting bandwidth and energy for the site
because they are not finding the files or directories they are looking for.
