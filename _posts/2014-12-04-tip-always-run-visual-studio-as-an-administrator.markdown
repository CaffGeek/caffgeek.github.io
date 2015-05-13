---
layout: post
title: 'TIP: Always run Visual Studio as an Administrator'
date: 2014-12-04 17:42:35.000000000 -06:00
categories:
- ".NET"
- Development
- TIP
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '1'
author:
  login: gibble
  email: chad.hurd@gcsquared.com
  display_name: caffgeek
  first_name: Chad
  last_name: Hurd
---
If you're like me, you always want Visual Studio opening as Administrator, allowing it to install services, and do whatever you need it to do. But you often forget to right click and "Run as Administrator". Or you just want to be able to use the pinned shortcuts on the task bar. It's really amazingly simple to make it always run as an administrator, just follow these simple steps:

1.  _Right Click_ on "**devenv.exe**" in Explorer
2.  _Click_ "**Troubleshoot compatibility**"
3.  _Click_ "**Troubleshoot program**"
4.  _Check_ "**The program requires additional permissions**"
5.  _Click_ "**Next**"
6.  _Click_ "**Test the program...**". It should launch Visual Studio as Administrator
7.  _Click_ "**Next**"
8.  _Click_ "**Yes, save these settings for this program**"
9.  _Click_ "**Close the troubleshooter**"

You can revert by following the same steps, but unchecking "The program requires additional permissions"