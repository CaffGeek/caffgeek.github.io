---
layout: post
title: Regexes and Named Capture Groups
date: 2009-04-04 01:40:40.000000000 -05:00
categories:
- Development
tags:
- capture groups
- regex
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
Not everyone knows about named capture groups, not everyone knows about regexes for that matter, but for those that do, capture groups are great, they allow us to grab portions of our regular expression match, and use them in our application.  The problem is, as your regex gets more and more complicated, if you still rely on the index of the capture group, to get the data you want, it can change as we make modifications, and be hard to find, and introduce undesired behaviours when you set a variable to capture group 7, when it’s now capture group 8…but you didn’t notice.  Yeah, it should be caught in testing, but it’s nearly 2009, we shouldn’t be referencing data by numbers when we can reference it by name.

So let’s say we have a flat file that we are parsing, it’s an old data file from a legacy system that we are trying to read in the format

id[tab]name[tab]position

And we’d write a simple regex to match and capture the 3 parts /^(\w+)\t(\w+)\t(\w+)$/

so

var id =  match.Group[1];

var name =  match.Group[2];

var position =  match.Group[3];

But, let’s say that the file changes slightly and we insert extension between name and position, our regex would change to /^(\w+)\t(\w+)\t(\w+)\t(\w+)$/ and we’d have to change position to use group 4.

A small inconvenience, but unnecessary.

if our matches are named /^(?<id>\w+)\t(?<name>\w+)\t(?<extension>\w+)\t(?<position>\w+)$/

We can use match.Group["id"] to get the name, match.Group["name"], etc

In more complex scenarios this saves a LOT of time and headaches.  Not only that, it comments your regex making it easier to maintain and read in the future!