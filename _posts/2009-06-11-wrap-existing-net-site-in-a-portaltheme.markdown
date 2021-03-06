---
layout: post
title: Wrap existing .NET site in a Portal/Theme
date: 2009-06-11 18:17:29.000000000 -05:00
categories:
- Development
tags:
- ".NET"
- App_Browsers
- Page Adapter
- Portal
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
Recently I ran into the problem where we were attempting to have all of our websites run inside of a portal. Where you could easily jump from one app to another, without having to go back and forth, or always back to a home page with links.

What we wanted was a header menu added to ALL of our websites that handled some authentication, and showed what websites you had access to, and allowed you to easily navigate between them, as though they were one large application.

Now, rewriting all these applications would be a nightmare. There was no time. Even if the goal was simply to modify the master pages of each app...some of which were very dated, and didn't have the luxury of master pages.

So with a little research on the Page Adapter Class and some creative use of the .browsers file, I came up with an unobtrusive method to easily add our new portal to all existing applications, without any recompiling.

First, lets create our Portal Page Adapter project. Add a new class file named PortalPageAdapter.cs

{% highlight csharp %}
namespace PortalPageAdapter
{
	using System;
	using System.Collections.Generic;
	using System.Linq;
	using System.Text;
	using System.Web.UI;
	using System.Web.UI.HtmlControls;
	using System.Xml;
	using System.Web.UI.WebControls;
	using System.Web;
	using System.Security.Principal;
	using System.Xml.Linq;

	public class PortalPageAdapter : System.Web.UI.Adapters.PageAdapter
	{
		protected override void OnInit(EventArgs e)
		{
			// Do whatever security checks you need to do...
			string user = HttpContext.Current.Request.LogonUserIdentity.Name;
			
			List<string> roles = new List<string>();

			//populate your roles

			// Setup Context
			HttpContext.Current.User = new GenericPrincipal(HttpContext.Current.User.Identity, roles.Distinct().ToArray());

			base.OnInit(e);
		}

		protected override void CreateChildControls()
		{
			base.CreateChildControls();

			// Inject portal onto page
			HtmlForm html = this.Page.Controls.OfType<htmlform>().First();
			html.Controls.AddAt(0, new LiteralControl("<br />PORTAL GOES HERE"));

			// Hide WebControls based on their Role Attribute
			// 	This allows the portal, to handle the rendering of any websites 
			//	you have, it can show/hide items at the control level
			
			List<webcontrol> controls = html.Controls.OfType<webcontrol>()
				.Where(c => ! string.IsNullOrEmpty(c.Attributes["role"]))
				.ToList();

			foreach (WebControl control in controls)
			{
				control.Visible = false;

				foreach (string role in control.Attributes["role"].Split(' '))
				{
					if (HttpContext.Current.User.IsInRole(role))
					{
						control.Visible = true;
						break;
					}
				}				
			}
		}
	}
}
{% endhighlight %}

Now that we have our portal written, with a trick up it's sleeve to hide/show content based upon the users roles, let's look at how we add it to an existing website.

In the App_Browsers folder, add a file called default.browsers

{% highlight xml %}
<browsers>
	<browser refid="Default">
		<controladapters>
			<adapter controltype="System.Web.UI.Page" adaptertype="PortalPageAdapter.PortalPageAdapter" />
		</controladapters>
	</browser>
</browsers>
{% endhighlight %}

And add the compiled dll of our PortalPageAdapter class to the bin folder.

Navigate to your website and the first thing you see should be the line "PORTAL GOES HERE", which you can modify in the Portal Page Adapter class to create whatever content you would like. How you generate the html is up to you. Be it includes, databases, xml/xsl, whatever. Get creative.

Now, the last thing, is, the 'trick' that has been programmed in, on any aspx page, you can add the "role" attribute to any WebControl (asp:whatever) and it will be rendered (or not) based upon the roles loaded by the PortalPageAdapter class.

{% highlight html %}
<%@ Page Language="C#" AutoEventWireup="true" CodeBehind="Default.aspx.cs" Inherits="PageAdapter._Default" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head runat="server">
	<title></title>
</head>
<body>
	<form id="form1" runat="server">
	<div>
		<asp:panel runat="server">
			Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vestibulum imperdiet justo id sem. Aenean convallis mi sed ipsum. Donec semper dapibus erat. Suspendisse consequat libero quis felis feugiat aliquam. Sed nec elit. Fusce sapien tellus, vestibulum id, pharetra in, scelerisque sit amet, lectus. Vivamus eu turpis at nunc elementum ultricies. Morbi feugiat fringilla est. Fusce urna diam, accumsan vitae, ultrices non, varius vitae, quam. Vivamus vitae enim vel nulla mattis aliquam. Suspendisse lacinia arcu non urna. Praesent convallis ante ut sapien. Nunc sit amet mauris ornare mauris accumsan ultricies. Praesent venenatis tellus id quam. Fusce vel enim a est malesuada venenatis.
			<p>
		</p></asp:panel>
		<asp:panel runat="server" role="ADMIN">
			Sed eget mauris vitae libero imperdiet malesuada. Suspendisse feugiat semper erat. Duis sit amet odio ultricies dui ultrices volutpat. Sed id risus vitae odio bibendum varius. Nam rutrum consectetur felis. Donec vitae velit. Phasellus facilisis ornare mi. Etiam quis dui id leo bibendum vehicula. Proin egestas, neque quis pulvinar malesuada, odio tellus porta orci, nec dictum est nunc id diam. Integer eu dui. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Ut nec ante ut pede ornare ornare. Mauris eget mauris et urna tincidunt tincidunt. Suspendisse sit amet lacus in dui fermentum aliquam. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Suspendisse odio. Phasellus orci. Aliquam erat volutpat. Nam eget ligula id arcu fermentum lobortis. Sed vel leo.
			<p>
		</p></asp:panel>
		<asp:panel runat="server" role="USER ADMIN">
			Suspendisse pulvinar. Etiam ipsum. Proin vulputate nibh et purus. Nunc hendrerit. Morbi consequat nibh id tortor. In mollis rhoncus velit. Pellentesque magna ipsum, cursus sit amet, viverra eu, porttitor et, dolor. Cras quis sem. Ut interdum nisi quis magna. Donec mauris erat, consequat id, gravida eu, scelerisque ac, nisi. Nulla ante purus, dignissim nec, mollis quis, euismod quis, arcu. In vel arcu. Duis enim erat, accumsan vel, blandit vel, ultricies id, pede. Nulla luctus, nisi at faucibus tincidunt, orci sapien ornare erat, vitae iaculis justo risus ut magna.
			<p>
		</p></asp:panel>
		<asp:panel runat="server" role="MANAGER">
			Etiam imperdiet lacus at dui fringilla dictum. Etiam condimentum, diam vitae fringilla faucibus, ante ante facilisis nulla, at porttitor diam ipsum vel odio. Aliquam sollicitudin neque et diam. Aenean feugiat, justo ut imperdiet cursus, sem risus ultrices ligula, eget rutrum orci leo tempor enim. Nunc eget velit. Pellentesque dictum, odio a tempor aliquet, ipsum justo lacinia mauris, sed posuere metus enim vel ipsum. Donec dolor. Aliquam semper, eros vitae vehicula lacinia, dolor massa suscipit libero, a molestie enim justo tincidunt sem. Praesent eget mi ut purus interdum vehicula. Nulla a leo. Morbi lacinia, ligula id pharetra vulputate, sapien arcu dignissim tortor, ac condimentum velit libero at purus.
			<p>
		</p></asp:panel>
		<asp:panel runat="server">
			Donec adipiscing lacus ac sapien. Nulla ac quam. Sed enim. Curabitur magna. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Cras viverra, libero quis molestie cursus, justo lacus semper nisl, in fringilla enim pede ac elit. Pellentesque facilisis. Etiam fringilla adipiscing mauris. Vivamus bibendum nibh nec massa. Maecenas risus. Cras tempus accumsan pede. Cras dictum hendrerit dolor. Curabitur et tortor ullamcorper elit mattis lacinia. Cras tempus euismod velit. Donec feugiat nunc quis dui. Vivamus nisi urna, bibendum et, imperdiet sit amet, facilisis in, libero. Maecenas lobortis velit at ante. Aenean in lacus et massa fringilla molestie. Praesent mollis nibh ut nunc.
			<p>
		</p></asp:panel>
		<asp:button runat="server" role="ADMIN" text="Admin Button" />
		<asp:button runat="server" role="USER MANAGER" text="User Button" />
	</div>
	</form>
</body>
</html>
{% endhighlight %}

This would also be a simple way to add an embeded IM client to a series of websites.

Happy coding.