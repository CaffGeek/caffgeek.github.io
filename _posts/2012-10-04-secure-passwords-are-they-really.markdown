---
layout: post
title: Secure Passwords...are they really?
date: 2012-10-04 07:47:49.000000000 -05:00
categories:
- General
- Security
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
Why do so many sites require me to enter a weak password? They claim to require a strong password, they also will show a handy dandy password strength meter. However, most of the time they restrict how strong my password can actually be.

They limit the number of characters to 8 - 15\. I want a longer password!  
 They limit the special characters I can use. Why can I use an exclamation point, but not an ampersand?

[http://xkcd.com/936/  
![correct horse battery staple](/assets/password_strength.png)  
](http://xkcd.com/936/)

It's a password, nothing should be off limits! If I want my password to be "clip clop coconut horse riding swallows" I should be able to use it, it's simple to remember, and hard to guess.

This is more secure and easier to remember than a password like 'c0rnf1@k3s'. But on most sites, it would be rejected because it's just alphabetic.

Let's check some stats at [https://www.grc.com/haystack.htm](https://www.grc.com/haystack.htm "https://www.grc.com/haystack.htm")

The password "c0rnf1@k3s" would take 9.47 months at 100,000,000,000 guesses/second (One Hundred Billion).  
 The password "clip clop coconut horse riding swallows" which only requires I remember 6 words would take 3.74 trillion trillion trillion trillion centuries.

Now, this isn't actually quite that strong, as I've effectively just replaced letters with words. So, when doing a brute force attack, the attacker would know that, they just need to combine words, instead of letters to generate passwords. That is, if everyone were to switch to this technique, and the attacker included this password scheme. And since this password is only 6 words long it's pretty weak...right? Well, according to here, there are between 170,000 and 750,000 depending on your definition of a word. Since in the case of passwords the word dog, and dogs are completely different, we could safely say, there are 750,000 one word passwords. I'm using six in a nonsensical order. Which means if I did my math correctly there are about 750,000^6 = 1.77978515625e35 different possibly 6 word passwords. At one hundred billion guesses per second it would still take 5.63992e14 centuries to go through all the possibly combinations, that's no 3.74 trillion trillion trillion trillion centuries, but it's much longer than I plan to live. Maybe the attacker gets lucky and it's the first guess...maybe it's the last. Odds are it's somewhere in the middle. But, I can remember it. And it's secure.

So, if you're limiting password length or the number of characters, please, do me a favour and stop...I want to use my longer, far more secure password.

Thanks.