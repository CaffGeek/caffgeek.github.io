---
layout: post
title: AOP - Logging with Unity
date: 2013-03-13 07:17:10.000000000 -05:00
categories:
- Development
tags:
- ".NET"
- AOP
- Logging
- Unity
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
I couple months ago I was tasked with adding logging to an application we are developing. Now, the typical way people handle this, is with a call to "`Logger.Log`" wherever they want to log that an activity took place. However, this relies on the programmers memory, and while the original team members may know and remember to keep up this practice. The new guy on the team may not. And this is where this technique really starts to break down. Logging is a cross cutting feature. We can't keep all the code in one place, because it's called from all over the application.

The solution, is AOP ([Aspect Oriented Programming](http://en.wikipedia.org/wiki/Aspect-oriented_programming "Aspect Oriented Programming")). Since we were using [Unity](http://unity.codeplex.com/releases/view/31277 "Unity"), it's quite simple to intercept calls based on an interface. There is more it's capable of, but the issue I had was I couldn't find a simple, "hello world" example. Everything felt the need to over complicate the example.

First things first, on your project, right click, Manage NuGet packages and add Unity Interception Extension.

Second: Config changes

{% highlight xml %}
 <configuration><configsections>

<section name="unity" type="Microsoft.Practices.Unity.Configuration.UnityConfigurationSection, Microsoft.Practices.Unity.Configuration">

    <unity xmlns="http://schemas.microsoft.com/practices/2010/unity">
      <sectionextension type="Microsoft.Practices.Unity.InterceptionExtension.Configuration.InterceptionConfigurationExtension, Microsoft.Practices.Unity.Interception.Configuration"><container>
        <extension type="Interception"><register type="LoggingTest.Namespace.IInterfaceToLog, LoggingTest.Namespace.Assembly">
          <interceptor type="InterfaceInterceptor">
          <interceptionbehavior type="LoggingTest.Namespace.Loggers.LoggerBehavior, LoggingTest.Namespace.Assembly"></interceptionbehavior> </interceptor></register></extension> </container></sectionextension> </unity>

</section>

</configsections></configuration>{% endhighlight %}

Third: Create Behavior

{% highlight csharp %}   public class LoggerBehavior : IInterceptionBehavior
    {
        public IMethodReturn Invoke(IMethodInvocation input, GetNextInterceptionBehaviorDelegate getNext)
        {
            var stopwatch = new Stopwatch();

            var logger = LogManager.GetLogger(input.MethodBase.ReflectedType);

            var declaringType = input.MethodBase.DeclaringType;
            var className = declaringType != null ? declaringType.Name : string.Empty;
            var methodName = input.MethodBase.Name;
            var generic = declaringType != null && declaringType.IsGenericType
                              ? string.Format("<{0}>", string.Join<type>(", ", declaringType.GetGenericArguments()))
                              : string.Empty;

            var argumentWriter = new StringWriter();
            for (var i = 0; i < input.Arguments.Count; i++)
            {
                var argument = input.Arguments[i];
                var argumentInfo = input.Arguments.GetParameterInfo(i);
                argument.Dump(argumentInfo.Name, argumentWriter);
            }
            var methodCall = string.Format("{0}{1}.{2}\n{3}", className, generic, methodName, argumentWriter);

            logger.InfoFormat(@"Entering {0}", methodCall);

            stopwatch.Start();
            var returnMessage = getNext()(input, getNext);
            stopwatch.Stop();

            logger.InfoFormat(@"Exited {0} after {1}ms", methodName, stopwatch.ElapsedMilliseconds);

            return returnMessage;
        }

        public IEnumerable <type>GetRequiredInterfaces()
        {
            return Type.EmptyTypes;
        }

        public bool WillExecute
        {
            get { return true; }
        }
    }</type> </type>{% endhighlight %}

That's it. One simple class that does the logging. And a config change to mark what interfaces you want logged.

{% highlight csharp %}        <register type="LoggingTest.Namespace.IInterfaceToLog, LoggingTest.Namespace.Assembly">
          <interceptor type="InterfaceInterceptor">
          <interceptionbehavior type="LoggingTest.Namespace.Loggers.LoggerBehavior, LoggingTest.Namespace.Assembly"></interceptionbehavior> </interceptor></register>
{% endhighlight %}

This config change will run the `LoggerBehavior` on the methods defined in `LoggingTest.Namespace.IInterfaceToLog`. If you want to log the methods on more interfaces, just add another `register` node to the config. While you still need to add these manually. You do it at the interface level, rather than the method level. AND you can add/change what is logged **after** compiling.

There is more you can do, check the [Unity](http://unity.codeplex.com/ "Unity") codeplex page.