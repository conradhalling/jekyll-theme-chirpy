---
title: Generating and Installing an ssh Key
description: I generated a public/private key pair and installed the public key in my blogging accounts so I could log in without having to enter a password. 
author: conrad
date: 2025-02-19 09:25 -0500
categories: [Blogging]
---

## Introduction

I wanted to set up a public/private key pair so I could ssh from my MacBook Pro
(named arcturus) to my DreamHost account for
[conradhalling.com](https://conradhalling.com) and my GoDaddy account for
[sphaerula.com](https://sphaerula.com). I accomplished this by entering commands
in a terminal window. I have obscured sensitive information with `X` characters.

## Generate Public/Private Key Pair

Using the `ssh-keygen` command, I created the public/private key pair. I
accepted the default file in which to save the key. I did not passphrase-protect
the key by pressing `enter` at the prompts for entering a passphrase.

    $ ssh-keygen
    Generating public/private ed25519 key pair.
    Enter file in which to save the key (/Users/halto/.ssh/id_ed25519):
    Enter passphrase (empty for no passphrase):
    Enter same passphrase again:
    Your identification has been saved in /Users/halto/.ssh/id_ed25519
    Your public key has been saved in /Users/halto/.ssh/id_ed25519.pub
    The key fingerprint is:
    SHA256:VuXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXto halto@arcturus
    The key's randomart image is:
    +--[ED25519 256]--+
    |          ooo    |
    |XXXXXXXXXXXXXXXXX|
    |XXXXXXXXXXXXXXXXX|
    |XXXXXXXXXXXXXXXXX|
    |XXXXXXXXXXXXXXXXX|
    |XXXXXXXXXXXXXXXXX|
    |XXXXXXXXXXXXXXXXX|
    |XXXXXXXXXXXXXXXXX|
    |          oo=E.o+|
    +----[SHA256]-----+

## Install the Public Key on conradhalling.com

Using the `ssh-copy-id` command, I copied the public key to my DreamHost account
for [conradhalling.com](https://conradhalling.com).

    $ ssh-copy-id -i ~/.ssh/id_ed25519.pub cXXXXX1@iXXXXXXXXXXXXXXX3.dreamhost.com
    /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/Users/halto/.ssh/id_ed25519.pub"
    /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
    /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
    cXXXXX1@iXXXXXXXXXXXXXXX3.dreamhost.com's password:

    Number of key(s) added:        1

    Now try logging into the machine, with:   "ssh 'cXXXXX1@iXXXXXXXXXXXXXXX3.dreamhost.com'"
    and check to make sure that only the key(s) you wanted were added.

I confirmed that I could log into the DreamHost account without needing to enter
a password. I looked at the authorized keys that were installed; there was only
the one key.

    $ ssh cXXXXX1@iXXXXXXXXXXXXXXX3.dreamhost.com

    Welcome to iXXXXXXXXXXXXXXX3.dreamhost.com

    Any malicious and/or unauthorized activity is strictly forbidden.
    All activity may be logged by DreamHost Web Hosting.

    Last login: Wed Feb 19 05:11:37 2025 from 72.106.189.6

    cXXXXX1@iXXXXXXXXXXXXXXX3 ~
    $ ls .ssh
    authorized_keys

    cXXXXX1@iXXXXXXXXXXXXXXX3 ~
    $ cat .ssh/authorized_keys
    ssh-ed25519 AXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX7 halto@arcturus

## Install the Public Key on sphaerula.com

Using the `ssh-copy-id` command, I copied the public key to my GoDaddy account
for [sphaerula.com](https://sphaerula.com).

    $ ssh-copy-id -i ~/.ssh/id_ed25519.pub yXXXXXXXXXX9@sphaerula.com
    /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/Users/halto/.ssh/id_ed25519.pub"
    /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
    /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
    yXXXXXXXXXX9@sphaerula.com's password:
    tput: No value for $TERM and no -T specified

    Number of key(s) added:        1

    Now try logging into the machine, with:   "ssh 'yXXXXXXXXXX9@sphaerula.com'"
    and check to make sure that only the key(s) you wanted were added.

I confirmed that I could log into the GoDaddy account without needing to enter a
password. I looked at the authorized keys that were installed; there was only
the one key.

    $ ssh 'yXXXXXXXXXX9@sphaerula.com'
    yXXXXXXXXXX9@pXXXXXXXXXXXXX3 [~]$ cat .ssh/authorized_keys
    ssh-ed25519 AXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX7 halto@arcturus
