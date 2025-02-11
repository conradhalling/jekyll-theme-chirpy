---
title: Modifying the Jekyll Chirpy Theme
author: conrad
description: I modified the Jekyll Chirpy theme to use different font families and to add color and contrast.
date: 2025-02-09 18:06:00 -0500
categories: [Blogging]
---

## Introduction

[Jekyll](https://jekyllrb.com) is a tool for building static websites, and the
popular [Chirpy theme](https://github.com/cotes2020/jekyll-theme-chirpy) is
designed for creating a technical blog that contains code blocks, tables, math, and
figures.

Jekyll combined with the Chirpy theme has a complete set of 
[features](https://chirpy.cotes.page/about/#features)—everything I was looking for,
including the following:

- static pages
- responsive web design
- dark and light modes
- writing posts using Markdown
- code syntax highlighting
- easy installation and configuration
- easy to create and deploy new content

However, I found Chirpy's grayscale theme a little bland. (You can view the default
Chirpy theme at its [demonstration website](https://chirpy.cotes.page).) I
wanted to use a larger serif font to make the text content easier to read, and I
wanted to add more color and contrast.

I documented my revisions here mainly for my benefit if I needed to go back
later to review my revisions or make additional revisions. But perhaps these
notes will be useful to others.

## Modifying the Font Families

### The Default Font Families

In the unmodified Chirpy theme, the font families were defined in
`_sass/abstracts/_variables.scss` using SCSS variables:

```scss
/* fonts */

$font-family-base: 'Source Sans Pro', 'Microsoft Yahei', sans-serif !default;
$font-family-heading: Lato, 'Microsoft Yahei', sans-serif !default;
```

The font files for Source Sans Pro and Lato were stored in `assets/lib/fonts`, where
both fonts were sans serif fonts. The [Microsoft
Yahei](https://learn.microsoft.com/en-us/typography/font-list/microsoft-yahei)
font family supplied Chinese glyphs.

### The Modified Font Families

I wanted to use a serif font for body text because it was more readable. I chose
Georgia, a [web safe](https://www.dreamhost.com/blog/web-safe-fonts/) serif font
designed for high legibility. For headers and labels, I chose Verdana, a web
safe sans serif font also designed for high legibility. I was able to change the
font families in `assets/css/jekyll-theme-chirpy.scss` with the following code:

```scss
:root {
  /* omitted code */

  /*
    Set the font families.
  */
  --content-font-family: Georgia, serif;
  --heading-font-family: Verdana, sans-serif;
  --table-font-family: Verdana, sans-serif;

  /* omitted code */
}
```
{: file='assets/css/jekyll-theme-chirpy.scss'}

My later testing revealed that these font families did not provide Chinese,
Japanese, or Korean (CJK) glyphs, so the browser needed to find and use a system
font family for these. For an example of the use of Chinese characters, see
["Jekyll Chirpy Theme Blog Customization"](../jekyll-chirpy-theme-blog-customization/).

For code, the browser needed to use a system-provided monospace font. See
["Experimenting with Fonts"](../experimenting-with-fonts/) for examples.

### Font Size for Content

It was easy for me to change the font size for content in
`assets/css/jekyll-theme-chirpy.scss`. I enlarged the font size
to match the size that the *New York Times* or the *Washington Post*
used for their web pages.

```css
/*
  Increase font size for content and adjust line spacing for readability.
*/
.content {
  font-size: 1.2rem;
  line-height: 2rem;
}
```
{: file='assets/css/jekyll-theme-chirpy.scss'}

### Titles, Headings, and Labels

I wanted to use a sans-serif font for titles, headings, and labels. However,
after I changed the content font family to Georgia, I discovered that many
titles, headers, and labels used Georgia instead Verdana, the sans-serif font. I
had to add the following styles to `assets/css/jekyll-theme-chirpy.scss` to
override the default styles.

```scss
/*
  Style titles, headings, and labels using the same font family.
*/
/* _includes/topbar.html */
search#search,
#topbar-title,
/* _sass/base/_base.scss */
.content a.popup + em,
div#tail-wrapper,
/* _sass/base/_syntax.scss */
div[class^="language-"] .code-header,
/* _sass/base/_typography.scss */
h1,
h2,
h3,
h4,
h5,
div.content div#tags,
/* _sass/components/_popups.scss */
#notification > .toast-body,
#toc-popup .header .label,
#toc-popup #toc-popup-content,
/* _sass/layout/_panel.scss */
div.access > section,
#panel-wrapper .panel-heading,
/* _sass/layout/_sidebar.scss */
#sidebar .site-title,
#sidebar .site-subtitle,
#sidebar ul,
/* _sass/layout/_topbar.scss */
#topbar #breadcrumb,
/* _sass/pages/_archives.scss */
div#archives,
#archives .date.day,
/* _sass/pages/_categories.scss */
article div div.categories.card,
/* _sass/pages/_category-tag.scss */
div#page-category ul,
div#page-tag ul,
/* _sass/pages/_home.scss */
#post-list .card .card-body .post-meta,
#post-list .card .card-body .card-text.content,
ul .pagination,
/* _sass/pages/_post.scss */
.share-mastodon,
#toc-solo-trigger .label,
header > div.post-meta,
div.post-tail-wrapper div.license-wrapper,
div.post-tail-wrapper div.post-tags,
section#toc-wrapper,
header .post-desc,
/* _sass/pages/_search.scss */
div#search-result-wrapper div.content,
/* _sass/pages/_tags.scss */
.tag span,
{
  font-family: var(--heading-font-family);
}
```
{: file='assets/css/jekyll-theme-chirpy.scss'}

After changing the font families, I needed to make small adjustments.

```css
/*
  - Override Lato font for the kbd element.
  - _sass/base/_typography.scss
*/
kbd {
  font-family: monospace;
}

/*
  Override default styles to maintain font size.
*/
#post-list .card .card-body .card-text.content,
div.content div#tags,
div#search-result-wrapper div.content,
{
  font-size: 1rem;
}
```
{: file='assets/css/jekyll-theme-chirpy.scss'}

```css
/*
  In the Archives page, adjust the font size for the year to make make the
  timeline indicators align correctly.
*/
div#archives .year {
  font-size: 1.17rem;
}
```
{: file='assets/css/jekyll-theme-chirpy.scss'}

### Tables

I preferred to use a sans-serif font family for table content.

```scss
/*
  - In tables, use a small sans-serif font.
  - _sass/base/_base.scss
*/
.table-wrapper > table tr th,
.table-wrapper > table tr td,
{
  font-family: var(--table-font-family);
  font-size: 0.8rem !important;
  padding: 0.5rem !important;
  line-height: 1.2rem;
  vertical-align: top;
}
```
{: file='assets/css/jekyll-theme-chirpy.scss'}

## Modifying Color and Contrast

### The Default Colors

When I studied the theme's color styles (in files `_sass/themes/_light.scss` and
`_sass/themes/_dark.scss`), I discovered that the theme used more than 80 CSS
variables to specify colors for various HTML elements.

For example, these were 23 of the CSS variables: 

<table>
  <thead>
    <tr>
      <th rowspan="2" style="vertical-align: bottom;">CSS Variable</th>
      <th colspan="2" style="text-align: center;">Dark Mode</th>
      <th colspan="2" style="text-align: center;">Light Mode</th>
    </tr>
    <tr>
      <th>Color Definition</th>
      <th>Color</th>
      <th>Color Definition</th>
      <th>Color</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>--main-bg</td>
      <td>rgb(27, 27, 30)</td>
      <td style="background-color: rgb(27, 27, 30)"></td>
      <td>white</td>
      <td style="background-color: white"></td>
    </tr>
    <tr>
      <td>--mask-bg</td>
      <td>rgb(68, 69, 70)</td>
      <td style="background-color: rgb(68, 69, 70)"></td>
      <td>#c1c3c5</td>
      <td style="background-color: #c1c3c5"></td>
    </tr>
    <tr>
      <td>--main-border-color</td>
      <td>rgb(44, 45, 45)</td>
      <td style="background-color: rgb(44, 45, 45)"></td>
      <td>#f3f3f3</td>
      <td style="background-color: #f3f3f3"></td>
    </tr>
    <tr>
      <td>--text-color</td>
      <td>rgb(175, 176, 177)</td>
      <td style="background-color: rgb(175, 176, 177)"></td>
      <td>#34343c</td>
      <td style="background-color: #34343c"></td>
    </tr>
    <tr>
      <td>--text-muted-color</td>
      <td>#868686</td>
      <td style="background-color: #868686"></td>
      <td>#757575</td>
      <td style="background-color: #757575"></td>
    </tr>
    <tr>
      <td>--heading-color</td>
      <td>#cccccc</td>
      <td style="background-color: #cccccc"></td>
      <td>#2a2a2a</td>
      <td style="background-color: #2a2a2a"></td>
    </tr>
    <tr>
      <td>--label-color</td>
      <td>#a7a7a7</td>
      <td style="background-color: #a7a7a7"></td>
      <td>#585858</td>
      <td style="background-color: #585858"></td>
    </tr>
    <tr>
      <td>--blockquote-border-color</td>
      <td>rgb(66, 66, 66)</td>
      <td style="background-color: rgb(66, 66, 66)"></td>
      <td>#eeeeee</td>
      <td style="background-color: #eeeeee"></td>
    </tr>
    <tr>
      <td>--blockquote-text-color</td>
      <td>#868686</td>
      <td style="background-color: #868686"></td>
      <td>#757575</td>
      <td style="background-color: #757575"></td>
    </tr>
    <tr>
      <td>--link-color</td>
      <td>rgb(138, 180, 248)</td>
      <td style="background-color: rgb(138, 180, 248)"></td>
      <td>#0056b2</td>
      <td style="background-color: #0056b2"></td>
    </tr>
    <tr>
      <td>--link-underline-color</td>
      <td>rgb(82, 108, 150)</td>
      <td style="background-color: rgb(82, 108, 150)"></td>
      <td>#dee2e6</td>
      <td style="background-color: #dee2e6"></td>
    </tr>
    <tr>
      <td>--button-bg</td>
      <td>#131313</td>
      <td style="background-color: #1e1e1e"></td>
      <td>#ffffff</td>
      <td style="background-color: #ffffff"></td>
    </tr>
    <tr>
      <td>--button-border-color</td>
      <td>#232f31</td>
      <td style="background-color: #232f31"></td>
      <td>#e9ecef</td>
      <td style="background-color: #e9ecef"></td>
    </tr>
    <tr>
      <td>--button-backtotop-color</td>
      <td>rgb(175, 176, 177)</td>
      <td style="background-color: rgb(175, 176, 177)"></td>
      <td>#686868</td>
      <td style="background-color: #686868"></td>
    </tr>
    <tr>
      <td>--button-backtotop-border-color</td>
      <td>#213122</td>
      <td style="background-color: #212122"></td>
      <td>#f1f1f1</td>
      <td style="background-color: #f1f1f1"></td>
    </tr>
    <tr>
      <td>--checkbox-color</td>
      <td>rgb(118, 120, 121)</td>
      <td style="background-color: rgb(118, 120, 121)"></td>
      <td>#c5c5c5</td>
      <td style="background-color: #c5c5c5"></td>
    </tr>
    <tr>
      <td>--checkbox-checked-color</td>
      <td>rgb(138, 180, 248)</td>
      <td style="background-color: rgb(138, 180, 248)"></td>
      <td>#07a8f7</td>
      <td style="background-color: #07a8f7"></td>
    </tr>
    <tr>
      <td>--site-title-color</td>
      <td>#717070</td>
      <td style="background-color: #717070"></td>
      <td>rgb(113, 113, 113)</td>
      <td style="background-color: rgb(113, 113, 113)"></td>
    </tr>
    <tr>
      <td>--site-subtitle-title-color</td>
      <td>#868686</td>
      <td style="background-color: #868686"></td>
      <td>717171</td>
      <td style="background-color: #717171"></td>
    </tr>
    <tr>
      <td>--sidebar-bg</td>
      <td>#1e1e1e</td>
      <td style="background-color: #1e1e1e"></td>
      <td>#f6f8fa</td>
      <td style="background-color: #f6f8fa"></td>
    </tr>
    <tr>
      <td>--sidebar-border-color</td>
      <td>#292929</td>
      <td style="background-color: #292929"></td>
      <td>#efefef</td>
      <td style="background-color: #efefef"></td>
    </tr>
    <tr>
      <td>--sidebar-muted-color</td>
      <td>#868686</td>
      <td style="background-color: #868686"></td>
      <td>#545454</td>
      <td style="background-color: #545454"></td>
    </tr>
    <tr>
      <td>--sidebar-active-color</td>
      <td>rgb(255, 255, 255, 0.95)</td>
      <td style="background-color: rgb(255, 255, 255, 0.95)"></td>
      <td>#1d1d1d</td>
      <td style="background-color: #1d1d1d"></td>
    </tr>
  </tbody>
</table>

### Modified Colors

I overrode the default Chirpy styles by editing the
`assets/css/jekyll-theme-chirpy.scss` file. I wanted to achieve high contrast
between the text and the background, using `black` for text in light mode and
`white` for text in dark mode.

I experimented with different color styles and settled on specifying colors
using the CSS `hsl()` function, where all colors on the site had the same hue
and saturation and differed only in the lightness. (See
[hsl()](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/hsl) for
more information.)

The table below displays colors for hues ranging from `0deg` to `330deg`,
saturation `20%`, and lightness ranging from `0%` (black) to `100%` (white). The
rightmost, grayscale column has saturation set to `0%`. At the time of writing,
the colors in this blog were derived from hue `150deg` and saturation `20%`. 

<table>
  <thead>
    <tr>
      <th rowspan="2" style="text-align: center; vertical-align: bottom">Lightness<br>(%)</th>
      <th colspan="13" style="text-align: center">Hue (deg)</th>
    </tr>
    <tr>
      <th style="min-width: 3rem; text-align: end">0</th>
      <th style="min-width: 3rem; text-align: end">30</th>
      <th style="min-width: 3rem; text-align: end">60</th>
      <th style="min-width: 3rem; text-align: end">90</th>
      <th style="min-width: 3rem; text-align: end">120</th>
      <th style="min-width: 3rem; text-align: end">150</th>
      <th style="min-width: 3rem; text-align: end">180</th>
      <th style="min-width: 3rem; text-align: end">210</th>
      <th style="min-width: 3rem; text-align: end">240</th>
      <th style="min-width: 3rem; text-align: end">270</th>
      <th style="min-width: 3rem; text-align: end">300</th>
      <th style="min-width: 3rem; text-align: end">330</th>
      <th style="min-width: 3rem; text-align: end">NA</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th style="text-align: end">0</th>
      <td style="background-color: hsl(0, 20%, 0%)"></td>
      <td style="background-color: hsl(30, 20%, 0%)"></td>
      <td style="background-color: hsl(60, 20%, 0%)"></td>
      <td style="background-color: hsl(90, 20%, 0%)"></td>
      <td style="background-color: hsl(120, 20%, 0%)"></td>
      <td style="background-color: hsl(150, 20%, 0%)"></td>
      <td style="background-color: hsl(180, 20%, 0%)"></td>
      <td style="background-color: hsl(210, 20%, 0%)"></td>
      <td style="background-color: hsl(240, 20%, 0%)"></td>
      <td style="background-color: hsl(270, 20%, 0%)"></td>
      <td style="background-color: hsl(300, 20%, 0%)"></td>
      <td style="background-color: hsl(330, 20%, 0%)"></td>
      <td style="background-color: hsl(0, 0%, 0%)"></td>
    </tr>
    <tr>
      <th style="text-align: end">10</th>
      <td style="background-color: hsl(0, 20%, 10%)"></td>
      <td style="background-color: hsl(30, 20%, 10%)"></td>
      <td style="background-color: hsl(60, 20%, 10%)"></td>
      <td style="background-color: hsl(90, 20%, 10%)"></td>
      <td style="background-color: hsl(120, 20%, 10%)"></td>
      <td style="background-color: hsl(150, 20%, 10%)"></td>
      <td style="background-color: hsl(180, 20%, 10%)"></td>
      <td style="background-color: hsl(210, 20%, 10%)"></td>
      <td style="background-color: hsl(240, 20%, 10%)"></td>
      <td style="background-color: hsl(270, 20%, 10%)"></td>
      <td style="background-color: hsl(300, 20%, 10%)"></td>
      <td style="background-color: hsl(330, 20%, 10%)"></td>
      <td style="background-color: hsl(0, 0%, 10%)"></td>
    </tr>
    <tr>
      <th style="text-align: end">20</th>
      <td style="background-color: hsl(0, 20%, 20%)"></td>
      <td style="background-color: hsl(30, 20%, 20%)"></td>
      <td style="background-color: hsl(60, 20%, 20%)"></td>
      <td style="background-color: hsl(90, 20%, 20%)"></td>
      <td style="background-color: hsl(120, 20%, 20%)"></td>
      <td style="background-color: hsl(150, 20%, 20%)"></td>
      <td style="background-color: hsl(180, 20%, 20%)"></td>
      <td style="background-color: hsl(210, 20%, 20%)"></td>
      <td style="background-color: hsl(240, 20%, 20%)"></td>
      <td style="background-color: hsl(270, 20%, 20%)"></td>
      <td style="background-color: hsl(300, 20%, 20%)"></td>
      <td style="background-color: hsl(330, 20%, 20%)"></td>
      <td style="background-color: hsl(0, 0%, 20%)"></td>
    </tr>
    <tr>
      <th style="text-align: end">30</th>
      <td style="background-color: hsl(0, 20%, 30%)"></td>
      <td style="background-color: hsl(30, 20%, 30%)"></td>
      <td style="background-color: hsl(60, 20%, 30%)"></td>
      <td style="background-color: hsl(90, 20%, 30%)"></td>
      <td style="background-color: hsl(120, 20%, 30%)"></td>
      <td style="background-color: hsl(150, 20%, 30%)"></td>
      <td style="background-color: hsl(180, 20%, 30%)"></td>
      <td style="background-color: hsl(210, 20%, 30%)"></td>
      <td style="background-color: hsl(240, 20%, 30%)"></td>
      <td style="background-color: hsl(270, 20%, 30%)"></td>
      <td style="background-color: hsl(300, 20%, 30%)"></td>
      <td style="background-color: hsl(330, 20%, 30%)"></td>
      <td style="background-color: hsl(0, 0%, 30%)"></td>
    </tr>
    <tr>
      <th style="text-align: end">40</th>
      <td style="background-color: hsl(0, 20%, 40%)"></td>
      <td style="background-color: hsl(30, 20%, 40%)"></td>
      <td style="background-color: hsl(60, 20%, 40%)"></td>
      <td style="background-color: hsl(90, 20%, 40%)"></td>
      <td style="background-color: hsl(120, 20%, 40%)"></td>
      <td style="background-color: hsl(150, 20%, 40%)"></td>
      <td style="background-color: hsl(180, 20%, 40%)"></td>
      <td style="background-color: hsl(210, 20%, 40%)"></td>
      <td style="background-color: hsl(240, 20%, 40%)"></td>
      <td style="background-color: hsl(270, 20%, 40%)"></td>
      <td style="background-color: hsl(300, 20%, 40%)"></td>
      <td style="background-color: hsl(330, 20%, 40%)"></td>
      <td style="background-color: hsl(0, 0%, 40%)"></td>
    </tr>
    <tr>
      <th style="text-align: end">50</th>
      <td style="background-color: hsl(0, 20%, 50%)"></td>
      <td style="background-color: hsl(30, 20%, 50%)"></td>
      <td style="background-color: hsl(60, 20%, 50%)"></td>
      <td style="background-color: hsl(90, 20%, 50%)"></td>
      <td style="background-color: hsl(120, 20%, 50%)"></td>
      <td style="background-color: hsl(150, 20%, 50%)"></td>
      <td style="background-color: hsl(180, 20%, 50%)"></td>
      <td style="background-color: hsl(210, 20%, 50%)"></td>
      <td style="background-color: hsl(240, 20%, 50%)"></td>
      <td style="background-color: hsl(270, 20%, 50%)"></td>
      <td style="background-color: hsl(300, 20%, 50%)"></td>
      <td style="background-color: hsl(330, 20%, 50%)"></td>
      <td style="background-color: hsl(0, 0%, 50%)"></td>
    </tr>
    <tr>
      <th style="text-align: end">60</th>
      <td style="background-color: hsl(0, 20%, 60%)"></td>
      <td style="background-color: hsl(30, 20%, 60%)"></td>
      <td style="background-color: hsl(60, 20%, 60%)"></td>
      <td style="background-color: hsl(90, 20%, 60%)"></td>
      <td style="background-color: hsl(120, 20%, 60%)"></td>
      <td style="background-color: hsl(150, 20%, 60%)"></td>
      <td style="background-color: hsl(180, 20%, 60%)"></td>
      <td style="background-color: hsl(210, 20%, 60%)"></td>
      <td style="background-color: hsl(240, 20%, 60%)"></td>
      <td style="background-color: hsl(270, 20%, 60%)"></td>
      <td style="background-color: hsl(300, 20%, 60%)"></td>
      <td style="background-color: hsl(330, 20%, 60%)"></td>
      <td style="background-color: hsl(0, 0%, 60%)"></td>
    </tr>
    <tr>
      <th style="text-align: end">70</th>
      <td style="background-color: hsl(0, 20%, 70%)"></td>
      <td style="background-color: hsl(30, 20%, 70%)"></td>
      <td style="background-color: hsl(60, 20%, 70%)"></td>
      <td style="background-color: hsl(90, 20%, 70%)"></td>
      <td style="background-color: hsl(120, 20%, 70%)"></td>
      <td style="background-color: hsl(150, 20%, 70%)"></td>
      <td style="background-color: hsl(180, 20%, 70%)"></td>
      <td style="background-color: hsl(210, 20%, 70%)"></td>
      <td style="background-color: hsl(240, 20%, 70%)"></td>
      <td style="background-color: hsl(270, 20%, 70%)"></td>
      <td style="background-color: hsl(300, 20%, 70%)"></td>
      <td style="background-color: hsl(330, 20%, 70%)"></td>
      <td style="background-color: hsl(0, 0%, 70%)"></td>
    </tr>
    <tr>
      <th style="text-align: end">80</th>
      <td style="background-color: hsl(0, 20%, 80%)"></td>
      <td style="background-color: hsl(30, 20%, 80%)"></td>
      <td style="background-color: hsl(60, 20%, 80%)"></td>
      <td style="background-color: hsl(90, 20%, 80%)"></td>
      <td style="background-color: hsl(120, 20%, 80%)"></td>
      <td style="background-color: hsl(150, 20%, 80%)"></td>
      <td style="background-color: hsl(180, 20%, 80%)"></td>
      <td style="background-color: hsl(210, 20%, 80%)"></td>
      <td style="background-color: hsl(240, 20%, 80%)"></td>
      <td style="background-color: hsl(270, 20%, 80%)"></td>
      <td style="background-color: hsl(300, 20%, 80%)"></td>
      <td style="background-color: hsl(330, 20%, 80%)"></td>
      <td style="background-color: hsl(0, 0%, 80%)"></td>
    </tr>
    <tr>
      <th style="text-align: end">90</th>
      <td style="background-color: hsl(0, 20%, 90%)"></td>
      <td style="background-color: hsl(30, 20%, 90%)"></td>
      <td style="background-color: hsl(60, 20%, 90%)"></td>
      <td style="background-color: hsl(90, 20%, 90%)"></td>
      <td style="background-color: hsl(120, 20%, 90%)"></td>
      <td style="background-color: hsl(150, 20%, 90%)"></td>
      <td style="background-color: hsl(180, 20%, 90%)"></td>
      <td style="background-color: hsl(210, 20%, 90%)"></td>
      <td style="background-color: hsl(240, 20%, 90%)"></td>
      <td style="background-color: hsl(270, 20%, 90%)"></td>
      <td style="background-color: hsl(300, 20%, 90%)"></td>
      <td style="background-color: hsl(330, 20%, 90%)"></td>
      <td style="background-color: hsl(0, 0%, 90%)"></td>
    </tr>
    <tr>
      <th style="text-align: end">100</th>
      <td style="background-color: hsl(0, 20%, 100%)"></td>
      <td style="background-color: hsl(30, 20%, 100%)"></td>
      <td style="background-color: hsl(60, 20%, 100%)"></td>
      <td style="background-color: hsl(90, 20%, 100%)"></td>
      <td style="background-color: hsl(120, 20%, 100%)"></td>
      <td style="background-color: hsl(150, 20%, 100%)"></td>
      <td style="background-color: hsl(180, 20%, 100%)"></td>
      <td style="background-color: hsl(210, 20%, 100%)"></td>
      <td style="background-color: hsl(240, 20%, 100%)"></td>
      <td style="background-color: hsl(270, 20%, 100%)"></td>
      <td style="background-color: hsl(300, 20%, 100%)"></td>
      <td style="background-color: hsl(330, 20%, 100%)"></td>
      <td style="background-color: hsl(0, 0%, 100%)"></td>
    </tr>
  </tbody>
</table>

For a great demonstration of using `hsl()`, see [Learn how to use hue in CSS colors with
HSL](https://developer.mozilla.org/en-US/blog/learn-css-hues-colors-hsl/).

I edited `assets/css/jekyll-theme-chirpy.scss` to define variables for
the hue and saturation values.

```scss
:root {
  /*
    Set the base hue and saturation for creating style colors. The variables
    are used in assets/css/light_colors.scss and assets/css/dark_colors.scss.
  */
  --base-hue: 150; // 0 to 360
  --base-saturation: 20; // 0 to 100
}
```
{: file='assets/css/jekyll-theme-chirpy.scss'}

The files `assets/css/dark_colors.scss` and `assets/css/light_colors.scss` used
the `--base-hue` and `--base-saturation` variables to define six background
colors and four (foreground) colors for styling elements.

```scss
  /*
    Use the base hue and saturation values to define the colors used to style
    page elements, where the final colors have the same hue and saturation
    and differ only in their lightness values.
  */
  --bg-color-005: hsl(var(--base-hue) var(--base-saturation) 5);
  --bg-color-010: hsl(var(--base-hue) var(--base-saturation) 10);
  --bg-color-015: hsl(var(--base-hue) var(--base-saturation) 15);
  --bg-color-020: hsl(var(--base-hue) var(--base-saturation) 20);
  --bg-color-030: hsl(var(--base-hue) var(--base-saturation) 30);
  --bg-color-050: hsl(var(--base-hue) var(--base-saturation) 50);

  --text-color-100: hsl(var(--base-hue) var(--base-saturation) 100);
  --text-color-080: hsl(var(--base-hue) var(--base-saturation) 80);
  --text-color-075: hsl(var(--base-hue) var(--base-saturation) 75);
  --text-color-050: hsl(var(--base-hue) var(--base-saturation) 50);
```
{: file='assets/css/dark_colors.scss'}

I used this small and manageable set of colors to redefine the CSS variables
from the original theme. This was a small subset of the color styles
redefined in `_sass/css/jekyll-theme-chirpy.scss`:

```scss
  /*
    Override Chirpy's default style colors.
  */

  /*
    All pages
  */
  --heading-color: var(--text-color-100);
  --text-color: var(--text-color-100);
  --text-muted-color: var(--text-color-080);
  --text-muted-highlight-color: var(--text-color-080);
  --label-color: var(--text-color-100); // For the Recently Updated label
  --blockquote-border-color: var(--bg-color-030);
  --blockquote-text-color: var(--text-color-100);
  --link-color: var(--text-color-075);
  --link-underline-color: var(--text-color-075);
  --button-bg: var(--bg-color-015); // back to top button
  --btn-border-color: var(--bg-color-030); // back to top button; not used?
```
{: file='assets/css/jekyll-theme-chirpy.scss'}

## Other Changes

### Hide the Share Icons

I did not anticipate that anyone would want to share my posts on social media.
Furthermore, I did not use X, FaceBook, or Instagram (for reasons you can guess).
Consequently, I hid the share icons at the bottom of posts.

```css
/*
  Hide the share icons at bottom of posts.
*/
div .share-wrapper {
  display: none !important;
}
```
{: file='assets/css/jekyll-theme-chirpy.scss'}

### Hide Tags

I decided I didn't want to use tags in my sites. I deleted the file
`_tabs/tags.md`, which removed the TAGS item from the left panel. I also hid tag
information in the pages.

```css
/*
  - Hide tags in posts and other pages.
  - Comment out this code if you use tags.
*/
div.access section:not(#access-lastmod) {
  display: none;
}

div.post-tail-wrapper div.post-tags {
  display: none;
}

div.post-meta div:nth-child(2) {
  display: none;
}
```
{: file='assets/css/jekyll-theme-chirpy.scss'}

### Display Search Results

The search tool provided by the Jekyll Chirpy theme searched titles,
URLs, categories, tags, dates, and descriptions by default, as configured in the
file `assets/js/data/search.json`. The search did not include page or post
contents.

I modified the presentation of search results to use cards, where the user could
click anywhere on a card to go to the post found by the search. (This was
modelled after the HOME page, where information about each post appeared in a
card.)

I modified the following files:
  - `_includes/search-loader.html`
  - `_sass/pages/_search.scss`
  - `assets/css/_light_colors.scss`
  - `assets/css/_dark_colors.scss`
  - `assets/css/jekyll-theme-chirpy.scss`
  
### Display the Avatar

In the default theme, the avatar was displayed in a circle at the top of the
left panel. The circle was created using the `border-radius` style. For my blog
at [conradhalling.com](https://conradhalling.com/blog/), I displayed the avatar
in a rectangle using the following changes:

```scss
#sidebar #avatar {
  box-shadow: none;
  width: 11rem; /* 176 px */
  height: 7rem; /* 112 px */
  border-radius: 0 !important;
}

#sidebar #avatar img {
  width: 11rem;
  height: 7rem;
}
```
{: file='assets/css/jekyll-theme-chirpy.scss'}

## Source Code

In the `docs/CONTRIBUTING.md` file of the [Jekyll Chirpy source
code](https://github.com/cotes2020/jekyll-theme-chirpy), the developers made
this statement:

> We want to avoid chaos in the UI design and therefore do not accept requests
for changes like color schemes, fontfamilies, typography, and so on.

Consequently, these revisions will not be submitted as a pull request.

You can obtain my revisions of the source code at
[https://github.com/conradhalling/jekyll-theme-chirpy](https://github.com/conradhalling/jekyll-theme-chirpy)
in branch
[conradhalling](https://github.com/conradhalling/jekyll-theme-chirpy/tree/conradhalling)
or branch
[sphaerula](https://github.com/conradhalling/jekyll-theme-chirpy/tree/sphaerula).
