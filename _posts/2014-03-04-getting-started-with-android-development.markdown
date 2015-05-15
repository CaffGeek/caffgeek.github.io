---
layout: post
title: Getting Started with Android Development
date: 2014-03-04 18:14:51.000000000 -06:00
categories:
- Android
- Development
- Mobile
tags: []
status: publish
type: post
published: true
meta:
  NYSEmailSent: '1'
  _edit_last: '1'
author:
  login: gibble
  email: chad.hurd@gcsquared.com
  display_name: caffgeek
  first_name: Chad
  last_name: Hurd
---
Last fall I purchased a [Nexus 7 (2013)](http://www.amazon.com/gp/product/B00DVFLJKQ/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=B00DVFLJKQ&linkCode=as2&tag=webifyitca-20 "Nexus 7  (2013)") and have really enjoyed the pure Android experience.  I'll be retiring my S3 and buying a Nexus phone to replace it in the near future.  Anyhow, one thing that's always lacking is a way to develop on the tablet...or so I thought!  Enter [AIDE](http://www.android-ide.com/), an Android IDE that allows you to develop and run applications right from your tablet!

[![aide-devices](/assets/aide-devices-300x200.png)](/assets/aide-devices.png)

It comes with the code for Tetris, a Clock Widget, and a Hello World app.  I started hacking together my first native app.  And tried to run it.  Of course, it crashed. So, Google, and I download a few logcat readers.  None seem to work. I do some more research and learn that as of the Jelly Bean update, apps can no longer read each others logs. This was added for security reasons, as some applications (facebook) were logging passwords in the log file, meaning, a maliscous app could easily read your facebook password from it's logfile. Which now means, you need to connect your tablet to a pc to read the logs, and determine why your application is crashing.  At first, this annoyed me.  It took a while to get that work, and I'll show you how. But if you plan to deploy your app, you're going to want to have log files sent to you anyway. After showing you how to read logcat from your PC, I'll show you how to write your first app in AIDE, that will email (or change it to write to a file you can open) it's logcat after it crashes. It will allow you to develop, without the need for a PC.

First, you need to enable developer mode on your tablet. It used to just be in the settings, but now it's more complicated.  Open your settings and scroll down to "About tablet" (phone, or whatever).  Scroll down to the Build number, and click it 7 times.

[![about tablet](/assets/about-tablet-300x187.png)](/assets/about-tablet.png)

You'll see a message that developer mode is now enabled. Success. Back in the Settings screen you'll now see Developer Options.

[![dev options](/assets/dev-options-300x187.png)](/assets/dev-options.png)

In there, turn on USB Debugging

[![USB Debugging](/assets/USB-Debugging-300x187.png)](/assets/USB-Debugging.png)

Next we need to install drivers, you can get the latest Google USB Driver from [http://developer.android.com/sdk/win-usb.html](http://developer.android.com/sdk/win-usb.html "Google USB Drivers"). Unzip them to a location your machine, I chose _c:\Android Tools\Usb_Driver_. Now, open Device Manager, and find your device under "Other devices"

[![Update Nexus 7 Driver](/assets/Update-Nexus-7-Driver-300x159.png)](/assets/Update-Nexus-7-Driver.png)

Choose "Browse my computer..." and navigate to the folder you extracted the drivers into. Make sure "Include Subfolders" is checked. And install them, you should now see the device installed correctly.

[![Nexus-7-Driver-Installation-Completed](/assets/Nexus-7-Driver-Installation-Completed-300x82.png)](/assets/Nexus-7-Driver-Installation-Completed.png)

So, now that the drivers are installed, let's get adb installed, you can download [r19.0.1 for windows here](http://dl.google.com/android/repository/platform-tools_r19.0.1-windows.zip "ADB") which is currently the latest version. Just extract the files onto your computer, I stored them at "_C:\Android Tools\platform-tools_" on mine.

Open a command prompt, go to the directory you just extracted adb.exe into, and try issuing the command "_adb devices_", you should see a result listing the attached device.

`C:\Android Tools\platform-tools>adb devices  
 List of devices attached  
 06e96062 device`

If nothing is attached, you many need to change the Connection type with your computer, on your Android device, go to Settings -> Storage, and click the elipses in the top right, you'll come to this screen, try changing between the two modes, one should work. On mine it was Camera, others have reported differently.

[![USB Computer Connection](/assets/USB-Computer-Connection-300x187.png)](/assets/USB-Computer-Connection.png)

If all is working correctly, you should be able to dump the logcat contents with the command

`adb logcat`

Or to a file

`adb logcat -d > logcat.txt`

This logcat grows quickly, if you want to clear the logcat of your device, issue the command

`adb logcat -c`

And that's all for now. My next article should be a walkthrough of using AIDE to get a simple app up and running, with logging, so you can do do your development without needing adb to view logs, and thus, remove the tether to your computer.

Now, back to coding!