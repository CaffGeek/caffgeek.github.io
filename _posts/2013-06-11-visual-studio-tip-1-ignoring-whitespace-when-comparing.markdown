---
layout: post
title: 'Visual Studio Tip #1: Ignoring Whitespace when Comparing'
date: 2013-06-11 13:31:35.000000000 -05:00
categories:
- Development
tags:
- ".NET"
- Visual Studio
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
When comparing files, it's frustrating when the default compare, doesn't ignore whitespace, and many differences are caused by spaces vs tabs.

Solve it, by adjusting the parameters DiffMerge uses.

In Visual Studio:

1.  Tools->Options
2.  Source Control->Visual Studio Team Foundation Server
3.  Click the Configure User Tools button.

Now add an item with the following settings.

*   Extension : .*
*   Operation : Compare
*   Command : C:\Program Files\Microsoft Visual Studio 10.0\Common7\IDE\diffmerge.exe
*   Or: C:\**Program Files (x86)**\Microsoft Visual Studio 10.0\Common7\IDE\diffmerge.exe
*   Arguments : %1 %2 %6 %7 %5 /ignorespace