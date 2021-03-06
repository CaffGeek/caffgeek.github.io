---
layout: post
title: Reduce your Technical Debt
date: 2012-08-30 08:40:51.000000000 -05:00
categories:
- Development
tags: []
status: publish
type: post
published: true
meta:
  _edit_last: '1'
  ny_notify_subscribers: '1'
author:
  login: gibble
  email: chad.hurd@gcsquared.com
  display_name: caffgeek
  first_name: Chad
  last_name: Hurd
---
Now, let me preface all this by saying it's not a critique on the quality of the person. And I'm not writing this to rip apart anyone. The first goal of development is to write working software that provides business value. However, in providing business value, we can improve the value by improving the quality of code. It's often cited that 80% of the cost of a system is maintaining it after it's written. A lot of that maintenance is feature changes. Which is expected. However, the cost of those changes can be greatly reduced by improving the quality of code that is written, and increasing the maintainability of the codebase. Code is written for humans to comprehend, not computers. They do that in the blink of an eye. It takes us developers much longer to understand hundreds of thousands of lines of code.

[Technical Debt](http://en.wikipedia.org/wiki/Technical_debt "Technical Debt") is what all these bad decisions add up to. Eventually you have to pay it off, and fix it, or you'll drown, and your application goes bankrupt. Forcing you to rewrite it (more on that later).

So, on with the examples of what not to do.

This is a new take on the normal misuse of booleans

{% highlight vbnet %}        
Select Case isBoolean
	Case True
		myValue = 10
	Case False
		myValue = 99
End Select
{% endhighlight %}

Why not write it clearly in one line?

{% highlight csharp %}	myValue = IF(isBoolean, 10, 99)
{% endhighlight %}

or in c# (which I prefer)

{% highlight vbnet %}	myValue = isBoolean ? 10 : 99; 
{% endhighlight %}

If your code is FULL of hardcoded constants, put them in a config or the database or use enums! There should be NO [MAGIC NUMBERS](http://en.wikipedia.org/wiki/Magic_number_(programming)).

Use your database properly. There is no good reason to have a field, that contains delimited values!

Use tools that check the cyclomatic complexity of your functions. Anything over 10 is getting high. Anything over 15 is too high, and should likely be refactored and split up. If you have a complexity of 339! like a 1600 line function I will soon have the luxury of modifying you should be beaten and while in recovery forced to read a book about clean code...like say the book [Clean Code: A Handbook of Agile Software Craftsmanship](http://www.amazon.com/gp/product/0132350882/ref=as_li_ss_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=0132350882&linkCode=as2&tag=webifyitca-20).

If you are copy/pasting in your app, you're doing something wrong. If the functionality is that similar, extract the commonality and refactor your code properly. You should NEVER need to make the same change in two places. A very common violation of this rules is a switch statement. If you have the same switch statement in two places to control flow, your program is structured wrong.

A recent example of copy/paste programming

{% highlight csharp %}
if (fileName.IndexOf(".zg") > 0) {
	partFileName = fileName.SubString(0, fileName.IndexOf(".zg"))
	bolExtentionRecognized = true
}

if (fileName.IndexOf(".rur") > 0) {
	partFileName = fileName.SubString(0, fileName.IndexOf(".rur"))
	bolExtentionRecognized = true
}

if (fileName.IndexOf(".dat") > 0) {
	partFileName = fileName.SubString(0, fileName.IndexOf(".dat"))
	bolExtentionRecognized = true
}
{% endhighlight %}

While the intent is clear, it's not easily maintainable, it's much simpler if you just had to maintain the list of extensions, like this

{% highlight csharp %}       
// acceptableExceptions loaded in a config, not inline
// acceptableExceptions = new \[\] { ".zg", ".rur", ".dat" }
foreach (var extension in acceptableExtensions) {
	var extensionStartsAt = fileName.IndexOf(extension)
	if (extensionStartsAt > 0) {
		partFileName = fileName.SubString(0, extensionStartsAt)
		isExtentionRecognized = true
	}
}
{% endhighlight %}

Use the right data types. Don't store "True" and "False" or "Y" and "N" in a string when you can use a boolean variable. If it's a number, put it in an int. Don't store money in floating point numbers. Etc, etc.

Most importantly, know your framework. Stop reinventing the wheel. More than likely the framework has covered all the edge cases you'll miss, and it's been veted by thousands of programmers actively using the framework. There are methods to work with directories and files. Path.Combine is a good one to know. There are methods to parse dates, and convert strings of numbers into integers (decimals, etc). These methods can return wether it was successful ([TryParse](http://msdn.microsoft.com/en-us/library/f02979c7.aspx)) while returning the parse result to prevent a need for exception handling. Which reminds me, [exceptions should be exceptional](http://blogs.msdn.com/b/marklon/archive/2005/09/21/472343.aspx). If you can check for it, do so first, and ensure the exception can't happen. If you can't check, but can't handle it, there's no need to catch it.

{% highlight vbnet %}    
Function returnProperTime(ByVal strDate As String) As String
	Dim strHour As String
	Dim strMinute As String
	Dim strSecond As String

	strHour = Mid(strDate, 1, 2)
	strMinute = Mid(strDate, 3, 2)
	strSecond = Mid(strDate, 5, 2)

	returnProperTime = strHour & ":" & strMinute & ":" & strSecond
End Function
{% endhighlight %}

Should be using [ParseExact](http://msdn.microsoft.com/en-us/library/w2sa9yss.aspx) (and should be using proper types)

{% highlight vbnet %}    var dateTime = Date.ParseExact(strDate, "HHmmss", CulterInfo.InvariantCulter)
var formattedTime = dateTime.ToString("HH:mm:ss")
{% endhighlight %}

When you see these things, fix them. You don't have to make the app perfect all at once, but every file you open, leave it in a little better condition than you found it. Follow the boyscout rule. If everyone does that, it won't take long and the codebase everyone dreaded to work on will become enjoyable to work on. And it didn't require a grueling rewrite ([which you should almost never do](http://www.joelonsoftware.com/articles/fog0000000069.html)).