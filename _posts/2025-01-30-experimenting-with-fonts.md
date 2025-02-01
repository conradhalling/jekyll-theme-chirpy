---
title: Experimenting with Fonts
author: conrad
description: If a website requests a generic font, what font does your computer supply?
date: 2025-01-30 12:46:00 -0500
categories: [Jekyll]
---

The generic font families specified by CSS styles include monospace, serif,
sans-serif, fantasy, and cursive. When a CSS style requests one of these generic
font families, what actual font is used on your computer? This page provides
an experiment in seeing what fonts are used.

## Generic Fonts

<h3 style="font-family: monospace">Monospace</h3>
<p style="font-family: monospace">
    Lorem ipsum dolor sit amet consectetur, adipisicing elit. Eaque modi
    quisquam ratione commodi! Debitis, quis ducimus! Sunt repellat ea
    aspernatur illum temporibus numquam omnis quod quia, nemo eos. Cum,
    quia?
</p>

<h3 style="font-family: serif">Serif</h3>
<p style="font-family: serif">
    Lorem ipsum dolor sit amet consectetur, adipisicing elit. Eaque modi
    quisquam ratione commodi! Debitis, quis ducimus! Sunt repellat ea
    aspernatur illum temporibus numquam omnis quod quia, nemo eos. Cum,
    quia?
</p>

<h3 style="font-family: sans-serif">Sans-serif</h3>
<p style="font-family: sans-serif">
    Lorem ipsum dolor sit amet consectetur, adipisicing elit. Eaque modi
    quisquam ratione commodi! Debitis, quis ducimus! Sunt repellat ea
    aspernatur illum temporibus numquam omnis quod quia, nemo eos. Cum,
    quia?
</p>

<h3 style="font-family: fantasy">Fantasy</h3>
<p style="font-family: fantasy">
    Lorem ipsum dolor sit amet consectetur, adipisicing elit. Eaque modi
    quisquam ratione commodi! Debitis, quis ducimus! Sunt repellat ea
    aspernatur illum temporibus numquam omnis quod quia, nemo eos. Cum,
    quia?
</p>

<h3 style="font-family: cursive">Cursive</h3>
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

## Notes

As part of the experiment, this Markdown source for this post contains HTML
code. The table of contents on the right side of the page is generated from the
Markdown heads and from the HTML headings. But the links in the table of
contents for the HTML headings don't work. If I used Markdown headings, the
links would work, but I can't set the font family using Markdown headings.
