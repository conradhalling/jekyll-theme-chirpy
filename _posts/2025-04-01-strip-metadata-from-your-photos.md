---
title: Strip Metadata from Your Photos
description: Use a tool like exiftool to strip metadata from your photos before posting them.
author: conrad
date: 2025-04-01 09:21:00 -0400
categories: [Computing]
media_subpath: /assets/img/2025-04-01/
---

## Introduction

If you take photos with an iPhone, the software captures a lot of information
including the GPS coordinates of the photo's location. This is fine for travel
photos, but if you're posting photos you took at home and don't want to give
away the location of where you live, you should remove the GPS coordinates.

## Example

For example, this is a selfie I took on top of the Fremont Bridge in Portland,
Oregon, USA, during the [Providence Bridge
Pedal](https://www.providence.org/lp/bridge-pedal) on August 11, 2024.

![Selfie looking north from the Fremont Bridge in Portland, Oregon, USA](fremont-bridge-selfie.png)
_Selfie looking north from the Fremont Bridge in Portland, Oregon, USA_

I opened this photo using the macOS Preview app, clicked on Tools > Show Inspector,
and in the window that appeared clicked on GPS. Very precise GPS coordinates
are available, and the Preview app even provides a zoomable map to show the
location of the coordinates.

![The photo's GPS coordinates displayed by the macOS Preview
app](macos-preview-gps-info.png) _The photo's GPS coordinates displayed by the
macOS Preview app_

You can copy the coordinates, `Latitude: 45° 32’ 15.522” N, Longitude: 122° 41’ 0.528” W`, 
reformat them to standardize them as `(45° 32' 15.522", -122° 41' 0.528")`, and
paste them into the Apple Maps application on a macOS computer to
view the location on a map.

![Apple Maps screenshot showing the photo's GPS coordinates](apple-maps-screenshot.png)
_Apple Maps screenshot showing the photo's GPS coordinates_

And you can do the same with the Google Maps website.

![Google Maps screenshot showing the photo's GPS coordinates](google-maps-screenshot.png)
_Google Maps screenshot showing the photo's GPS coordinates_

## ExifTool

It is possible to remove the GPS coordinates from a photo's metadata one photo
at a time, but I wanted to find software that would do this for a batch of
photos. I did some [Kagi](https://kagi.com) searches for recommendations for
such software, and a good article I found was [How to Install ExifTool on
Mac](https://havecamerawilltravel.com/install-exiftool-mac/.) from the [Have
Camera Will Travel](https://havecamerawilltravel.com) website.

[ExifTool](https://exiftool.org/), by Phil Harvey, is free software available for Windows and macOS.
I used [homebrew](https://brew.sh) to install ExifTool on my M4 MacBook Pro.

```console
$ brew install exiftool
```

ExifTool is a command line tool. To remove the metadata from three photos I had used in
a [post](https://conradhalling.com/blog/posts/new-england-spring/) on my blog at
[conradhalling.com](https://conradhalling.com), I used these commands:

```console
$ cd ~/src/conradhalling/conradhalling.com/assets/img/2025-03-23
$ exiftool -all= -overwrite_original pansies.png snow_on_2025-02-09.png crocuses.png
```
