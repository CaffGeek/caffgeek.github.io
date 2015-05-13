---
layout: post
title: 'Getting started with Hybrid development with Ionic - Part 1: System Setup'
date: 2015-01-23 23:37:52.000000000 -06:00
categories:
- Android
- Development
- Javascript
- Mobile
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
So, let's just dive in and get the dev machine setup. We are going to be using [Apache Cordova](http://cordova.apache.org/), [Ionic](http://ionicframework.com/), and [AngularJS](https://angularjs.org/) frameworks. And we'll get [Jasmine](http://jasmine.github.io/2.1/introduction.html) and [Karma](http://karma-runner.github.io/0.12/index.html) setup for testing. We will also use npm (node package manager) to install these libraries, so let's get that installed first.

Install nodejs using the install link at [nodejs.org](http://nodejs.org/ "nodejs"). Then let's ensure we have the latest version by running this **as administrator**.

{% highligh powershell %}npm install npm -g{% endhighlight %}

We also need the Java JDK (not just JRE). That can be downloaded and installed from [here](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html). Just follow the instructions, and ensure it's in your PATH when complete.

Now let's get Apache ANT from [here](http://ant.apache.org/bindownload.cgi), and install it.

Also required is the [Android SDK](https://developer.android.com/sdk/installing/index.html?pkg=tools). Just follow the instructions, and run the Android SDK Manager to install the latest SDK Platform. Cordova requires the ANDROID_HOME environment variable. This should point to the android-sdk directory.

Now that we have npm, let's install cordova and ionic **as administrator**.

{% highligh powershell %}npm install -g cordova
npm install -g ionic
{% endhighlight %}

Now, navigate to the location you want to create your application. And generate a blank ionic app with

{% highligh powershell %}cd c:\source\
ionic start helloworld blank
{% endhighlight %}

**NOTE:** I had a problem here, I received the following error: "Unable to add plugins. Perhaps your version of Cordova is too old. Try updating (npm install -g cordova), removing this project folder, and trying again. (CLI v1.3.2)". However, nothing I did solved the problem. Turns out, for some reason, my helloworld/config.xml was blank. You can fetch a new [config.xml here](https://github.com/driftyco/ionic-app-base/blob/master/config.xml), replace the empty one, and things should start working.

Open the helloworld folder and let's add the android platform, and run the blank app

{% highligh powershell %}cd helloworld 
ionic platform add android
ionic serve
{% endhighlight %}

[![ionic_serve](assets/ionic_serve-300x63.png)](http://caffeinatedgeek.ca/wp-content/uploads/2015/01/ionic_serve.png)

Or connect your android device by usb, and load the application on it with

{% highligh powershell %}ionic run android
{% endhighlight %}

You can also run it in the Android emulator, but I don't recommend it, as it's slooooooow.

{% highligh powershell %}ionic build android
ionic emulate android
{% endhighlight %}

Next we'll start adding some code and tests.