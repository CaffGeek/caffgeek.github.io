---
layout: post
title: 'TIL: How to use adb to take a screenshot from Android'
date: 2014-03-03 17:19:47.000000000 -06:00
categories:
- Android
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
I was writing up another article and needed screen grabs from my device, and wanted a simple way to grab them, well, adb to the rescue!

`  
 adb shell /system/bin/screencap -p /sdcard/screenshot.png  
 adb pull /sdcard/screenshot.png screenshot.png  
`

It's that easy.

The first line saves the screenshot on the device, the second pulls a file from the device to the local machine.

All of this assumes you have adb working and the drivers setup for your device. Which I will be showing how to do in a future post.
