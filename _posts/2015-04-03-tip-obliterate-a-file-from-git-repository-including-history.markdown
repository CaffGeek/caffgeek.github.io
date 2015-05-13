---
layout: post
title: 'TIP: Obliterate a file from GIT Repository (including history)'
date: 2015-04-03 16:55:56.000000000 -05:00
categories: Security TIL TIP
---
I did something stupid, and accidentally pushed some .pubxml files to a public GitHub repo that contained passwords.

I fixed up my .gitignore file, which took care of part of it, but if you viewed the history, the password were still exposed.

After much mucking about, this magical line, obliterated any trace of the file in my repo.

{% highlight bash %}
git filter-branch --index-filter "git rm --cached --ignore-unmatch *.pubxml" --prune-empty --tag-name-filter cat -- --all
{% endhighlight %}

Then this line, forced an update of the entire repo to GitHub, history and all.

{% highlight bash %}
git push origin --force --all
{% endhighlight %}

...that was a close call.
