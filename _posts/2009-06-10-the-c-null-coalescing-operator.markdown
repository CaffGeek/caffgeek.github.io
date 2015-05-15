---
layout: post
title: The C# ?? null coalescing operator
date: 2009-06-10 23:09:12.000000000 -05:00
categories:
- Development
tags:
- ".NET"
- "??"
- 'Null'
- Operator
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
A simple tip to save typing and increase your codes readability is the "double question mark operator", more accurately called the "null coalescing operator".

Instead of using this to set defaults in a function

{% highlight csharp %} function void test(int aVar) { int myVar = aVar; if (aVar == null) { myVar = 42; } } {% endhighlight %}

or

{% highlight csharp %} function void test(int aVar) { int myVar = aVar == null ? 42 : aVar; } {% endhighlight %}

You can simply use

{% highlight csharp %} function void test(int aVar) { int myVar = aVar ?? 42; } {% endhighlight %}

Granted, you'd likely find much better ways to use this than simply defaults in a function call, but hey, this is just an example, let your imagination do the work.

MSDN reference: [http://msdn.microsoft.com/en-us/library/ms173224.aspx](http://msdn.microsoft.com/en-us/library/ms173224.aspx)