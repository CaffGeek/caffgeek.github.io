---
layout: post
title: Completely remove Printer Driver
date: 2015-01-20 16:49:04.000000000 -06:00
categories:
- Hardware
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
I recently had to keep retesting an installer, part of it's actions were to install a print driver, but to be able to keep testing, I had to remove the driver from the computer, not just the printer. So, here's how it's done, in case someone else needs to do this.

1.  Find your driver name. (ex.“Brother MFC-J6520DW Printer”)
2.  In "Devices and Printers" remove all printers using the driver from step 1
3.  Open Regedit.
4.  Navigate to (on 32 bit(x86) computers) “HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Print\Environments\Windows NT x86\Drivers\Version-3”
5.  Or (on 64 bit(x64) computers) “HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Print\Environments\Windows x64\Drivers\Version-3”
6.  And then remove the folder that matches the printer driver name.
7.  Open Services, and restart the print spooler service

That's it.