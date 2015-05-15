---
layout: post
title: 'TIL: The subtle difference between Keypress and Keydown (aside from the obvious)'
date: 2014-02-18 23:38:00.000000000 -06:00
categories:
- Development
- Javascript
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
The other day at work, an innocuous question was posed...

> So, in javascript ' (apostrophe) and right arrow have the same event key code. What am I supposed to do with that?

This did not seem right to me, but, sure enough, a [js fiddle](http://jsfiddle.net/qHM4d/ "js fiddle") showed this to be true...almost (_go to result tab, and hit different keyboard keys, it will show the character code_).

<iframe width="100%" height="300" src="http://jsfiddle.net/qHM4d/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

What appeared to be the same code (if you hit apostrophe then an arrow) was actually just that the arrow didn't register as a keypress, and you were still seeing the last keys code. However, this was only true in some browsers, IE and Chrome, Firefox I believe worked as one would expect.

Changing to a keydown event works, and shows the right arrow as 39, like it should.

<iframe width="100%" height="300" src="http://jsfiddle.net/SLtFk/1/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

[Quirksmode](http://www.quirksmode.org/js/keys.html "quirksmode") clears up a bit of the confusion, and has a nice little tester at the bottom of the page.