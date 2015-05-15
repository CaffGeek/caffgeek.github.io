---
layout: post
title: 'TIL: How easy it is to hack Windows.'
date: 2014-02-28 11:23:47.000000000 -06:00
categories:
- Security
- TIL
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '1'
  NYSEmailSent: '1'
author:
  login: gibble
  email: chad.hurd@gcsquared.com
  display_name: caffgeek
  first_name: Chad
  last_name: Hurd 
---
Found this on a friends [site](http://www.ppartyka.com/2014/02/info-hacking-windows-forgot-your.html "Systems Administration NYC") (which I recommend you read as he posts some great sys admin tips and tricks!)

<iframe src="//www.youtube.com/embed/k1YMJEryzBo" height="315" width="560" allowfullscreen="" frameborder="0"></iframe>

We watched it, then tried it out, and it worked. It takes about 2 minutes to change the password on an account and gain access to any windows computer.

The basic steps:

1.  When your computer is booting, reset it during the splash screen
2.  The prompt to repair appears, durin gthe repair there is an option to show the details in Notepad.exe
3.  You can use it's Open/Save dialog to rename your sethc.exe (sticky keys) and replace it with a copy of cmd.exe
4.  Reboot
5.  On the login, hit shift 5 times, and get a cmd.exe window
6.  Use the 'net' commands to reset a local admin password
7.  Login and profit.

It's way way way too easy.  Looks like the only way to secure your machine is to encrypt the entire drive so a password is required just to start the boot process.
