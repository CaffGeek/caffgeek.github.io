---
layout: post
title: 'Entity Framework Tip #1: Loading Grandchildren'
date: 2013-06-11 13:02:46.000000000 -05:00
categories:
- Development
tags:
- ".NET"
- C#
- Entity Framework
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
When calling .FindById(), it's a relatively simple task, to have it include child properties explicitly. Simply add a lambda expression.

{% highlight csharp %} myRepository.FindById(1, x=>x.ChildList);{% endhighlight %}

This will ensure that the ChildListis populated and not null.

But what if you need to ensure a GrandChildren entity is populated as well? Writing this won't work because ChildList is a collection.

{% highlight csharp %}	myRepository.FindById(1, x=>x.ChildList.GrandChildren);{% endhighlight %}

What you need to do, is use .Select()

{% highlight csharp %}	myRepository.FindById(1, 
		x=>x.ChildList, 
		x.ChildList.Select(y=>y.GrandChildren)
		);
{% endhighlight %}

Simple...albeit non-intuitive.