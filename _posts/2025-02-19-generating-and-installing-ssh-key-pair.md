---
title: Generating and Installing an ssh Public Key
description: I generated a public/private key pair and installed the public key in my blogging accounts so I could log in without having to enter a password.
author: conrad
date: 2025-02-19 09:25 -0500
categories: [Computing]
---

## Introduction

I set up a public/private key pair so I could use `ssh` from my MacBook Pro to
connect to my DreamHost account for
[conradhalling.com](https://conradhalling.com) and to my GoDaddy account for
[sphaerula.com](https://sphaerula.com). I accomplished this by entering commands
in a terminal window. I have obscured sensitive information with `X` characters.

## Generate Public/Private Key Pair

Using the `ssh-keygen` command, I created the public/private key pair. I
accepted the default file in which to save the key. I omitted passphrase-protecting
the key by pressing `enter` at the prompts for entering a passphrase.

```console
$ ssh-keygen
Generating public/private ed25519 key pair.
Enter file in which to save the key (/Users/halto/.ssh/id_ed25519):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /Users/halto/.ssh/id_ed25519
Your public key has been saved in /Users/halto/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:VuHuWL14ynXlWDgHrgM1BOV8hy5/nR+KX1n0s2+r5to halto@arcturus
The key's randomart image is:
+--[ED25519 256]--+
|          ooo    |
|         . =   . |
|          o = + o|
|         o o = =.|
|        S + o =.*|
|       . + o = BB|
|        . o * +*+|
|         . +o+o.=|
|          oo=E.o+|
+----[SHA256]-----+
```

## Install the Public Key on conradhalling.com

Using the `ssh-copy-id` command, I copied the public key to my DreamHost account
for [conradhalling.com](https://conradhalling.com).

```console
$ ssh-copy-id -i ~/.ssh/id_ed25519.pub cXXXXX1@iXXXXXXXXXXXXXXX3.dreamhost.com
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/Users/halto/.ssh/id_ed25519.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
cXXXXX1@iXXXXXXXXXXXXXXX3.dreamhost.com's password:

Number of key(s) added:        1

Now try logging into the machine, with:   "ssh 'cXXXXX1@iXXXXXXXXXXXXXXX3.dreamhost.com'"
and check to make sure that only the key(s) you wanted were added.
```

I confirmed that I could log into the DreamHost account without needing to enter
a password. I looked at the authorized keys that were installed; there was only
the one key.

```console
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
```

## Install the Public Key on sphaerula.com

Using the `ssh-copy-id` command, I copied the public key to my GoDaddy account
for [sphaerula.com](https://sphaerula.com).

```console
$ ssh-copy-id -i ~/.ssh/id_ed25519.pub yXXXXXXXXXX9@sphaerula.com
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/Users/halto/.ssh/id_ed25519.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
yXXXXXXXXXX9@sphaerula.com's password:
tput: No value for $TERM and no -T specified

Number of key(s) added:        1

Now try logging into the machine, with:   "ssh 'yXXXXXXXXXX9@sphaerula.com'"
and check to make sure that only the key(s) you wanted were added.
```

I confirmed that I could log into the GoDaddy account without needing to enter a
password. I looked at the authorized keys that were installed; there was only
the one key.

```console
$ ssh 'yXXXXXXXXXX9@sphaerula.com'
yXXXXXXXXXX9@pXXXXXXXXXXXXX3 [~]$ cat .ssh/authorized_keys
ssh-ed25519 AXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX7 halto@arcturus
```

## Visually Checking a Key

I used a variation of the `ssh-keygen` command to view the key's fingerprint and
[randomart image](https://bytes.zone/posts/what-is-the-randomart-image-for/).
This was generally not needed for keys I created for my own use. The
same fingerprint and randomart image were generated from the public or private
key of the key pair.

First, I viewed the fingerprint and randomart image for the original keys:

```console
$ ssh-keygen -lv -f ~/.ssh/id_ed25519.pub
256 SHA256:VuHuWL14ynXlWDgHrgM1BOV8hy5/nR+KX1n0s2+r5to halto@arcturus (ED25519)
+--[ED25519 256]--+
|          ooo    |
|         . =   . |
|          o = + o|
|         o o = =.|
|        S + o =.*|
|       . + o = BB|
|        . o * +*+|
|         . +o+o.=|
|          oo=E.o+|
+----[SHA256]-----+

$ ssh-keygen -lv -f ~/.ssh/id_ed25519
256 SHA256:VuHuWL14ynXlWDgHrgM1BOV8hy5/nR+KX1n0s2+r5to halto@arcturus (ED25519)
+--[ED25519 256]--+
|          ooo    |
|         . =   . |
|          o = + o|
|         o o = =.|
|        S + o =.*|
|       . + o = BB|
|        . o * +*+|
|         . +o+o.=|
|          oo=E.o+|
+----[SHA256]-----+
```

The `ssh-copy-id` command copied the public key into the ~/.ssh/authorized_key
file of the remote account. I logged in and viewed the signature and randomart image
of the public keys stored in this file.

```console
$ ssh yXXXXXXXXXX9@sphaerula.com

yXXXXXXXXXX9@pXXXXXXXXXXXXX3 [~]$ ls .ssh
authorized_keys

yXXXXXXXXXX9@pXXXXXXXXXXXXX3 [~]$ ssh-keygen -lv -f ~/.ssh/authorized_keys
256 SHA256:VuHuWL14ynXlWDgHrgM1BOV8hy5/nR+KX1n0s2+r5to halto@arcturus (ED25519)
+--[ED25519 256]--+
|          ooo    |
|         . =   . |
|          o = + o|
|         o o = =.|
|        S + o =.*|
|       . + o = BB|
|        . o * +*+|
|         . +o+o.=|
|          oo=E.o+|
+----[SHA256]-----+
```

## Verifying Host Keys

This information is available from the `man ssh` page in the section `VERIFYING
HOST KEYS`.

On my laptop, I created the file `~/.ssh/config` with the following option:

```conf
VisualHostKey=yes
```

This made it possible to view the signature and randomart image for the host key
when I logged in. For example, I saw the following host key fingerprint and
randomart image when I logged into my sphaerula.com account:

```console
$ ssh yXXXXXXXXXX9@sphaerula.com
Host key fingerprint is SHA256:LlWM0c5tz551c3qnTLhkSIg2MhyEhI+ntMxvXrCdgEo
+--[ED25519 256]--+
| o...   ..       |
|. ..     +.      |
| o  .   .oo.     |
|..+. . . oo o    |
|+E.o+ + S .. o   |
|++  =+.+ . . .o.+|
|. .. +. . . +..++|
|   o.  .   o ++ o|
|  o.        . oo.|
+----[SHA256]-----+
```
