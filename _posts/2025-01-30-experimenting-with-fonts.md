---
title: Experimenting with Fonts
author: conrad
description: If a website requests a generic font, what font does your computer supply?
date: 2025-01-30 12:46:00 -0500
last_modified_at: 2025-02-02 10:42:00 -0500
categories: [Jekyll]
---

The generic font families specified by CSS styles include monospace, serif,
sans-serif, fantasy, and cursive. When a CSS style requests one of these generic
font families, what actual font is used on your computer? This page provides
an experiment in seeing what fonts are used.

These were the fonts that were used on my different devices:

| Font Family | macOS 15.3      | iOS/iPadOS 18.3 | Windows 11 24H2 |
|:------------|:----------------|:----------------|:----------------|
| monospace   | Menlo           | Menlo           | Consolas        |
| serif       | Times New Roman | Times New Roman | Times Hew Roman |
| sans-serif  | Helvetica       | Helvetica       | Arial           |
| fantasy     | Papyrus         | Zapfino         | Arial           |
| cursive     | Apple Chancery  | Snell Roundhand | Comic Sans MS   |
| Verdana     | Verdana         | Verdana         | Verdana         |
| Georgia     | Georgia         | Georgia         | Georgia         |

## Generic Fonts

<h3 id="monospace" style="font-family: monospace">Monospace</h3>
<p style="font-family: monospace">
    Lorem ipsum dolor sit amet consectetur, adipisicing elit. Eaque modi
    quisquam ratione commodi! Debitis, quis ducimus! Sunt repellat ea
    aspernatur illum temporibus numquam omnis quod quia, nemo eos. Cum,
    quia?
</p>

<h3 id="serif" style="font-family: serif">Serif</h3>
<p style="font-family: serif">
    Lorem ipsum dolor sit amet consectetur, adipisicing elit. Eaque modi
    quisquam ratione commodi! Debitis, quis ducimus! Sunt repellat ea
    aspernatur illum temporibus numquam omnis quod quia, nemo eos. Cum,
    quia?
</p>

<h3 id="sans-serif" style="font-family: sans-serif">Sans-serif</h3>
<p style="font-family: sans-serif">
    Lorem ipsum dolor sit amet consectetur, adipisicing elit. Eaque modi
    quisquam ratione commodi! Debitis, quis ducimus! Sunt repellat ea
    aspernatur illum temporibus numquam omnis quod quia, nemo eos. Cum,
    quia?
</p>

<h3 id="fantasy" style="font-family: fantasy">Fantasy</h3>
<p style="font-family: fantasy">
    Lorem ipsum dolor sit amet consectetur, adipisicing elit. Eaque modi
    quisquam ratione commodi! Debitis, quis ducimus! Sunt repellat ea
    aspernatur illum temporibus numquam omnis quod quia, nemo eos. Cum,
    quia?
</p>

<h3 id="cursive" style="font-family: cursive">Cursive</h3>
<p style="font-family: cursive">
    Lorem ipsum dolor sit amet consectetur, adipisicing elit. Eaque modi
    quisquam ratione commodi! Debitis, quis ducimus! Sunt repellat ea
    aspernatur illum temporibus numquam omnis quod quia, nemo eos. Cum,
    quia?
</p>

## Website Standard Fonts

### Sans-serif

I have configured the Jekyll theme for this website to use Verdana (a sans-serif
web safe font family) for headings and labels and to use Georgia (a serif web
safe font family) for content text when converting Markdown to HTML.

Lorem ipsum dolor sit amet consectetur, adipisicing elit. Eaque modi quisquam
ratione commodi! Debitis, quis ducimus! Sunt repellat ea aspernatur illum
temporibus numquam omnis quod quia, nemo eos. Cum, quia?
