---
layout: post
title: Painlessly Audit Your SQL2005 Schema & DDL Information
date: 2009-04-04 01:51:19.000000000 -05:00
categories:
- Development
tags:
- sql
- version control
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
I recently stumbled across the following page

[Painlessly Audit Your SQL2005 Schema & DDL Information](http://www.clanmonroe.com/Blog/archive/2007/10/04/painlessly-audit-your-sql2005-schema-amp-ddl-information.aspx).

We all know about SQL DML (Data Manipulation Language) triggers, these are the kind we use  occassionally when data is manipulated.

What you may not know about are DDL (Data Definition Language) triggers.

In short, you can write a trigger to fire whenever tables, stored procedures, views, etc change.

So with the following table and trigger (code stolen from link above) you can audit your stored proc and table changes

{% highlight sql %}
CREATE TABLE [dbo].[DBChangeLog](
    [LogId] [int] IDENTITY(1,1) NOT NULL,
    [DatabaseName] [varchar](256) NOT NULL,
    [EventType] [varchar](50) NOT NULL,
    [ObjectName] [varchar](256) NOT NULL,
    [ObjectType] [varchar](25) NOT NULL,
    [SqlCommandXml] XML NOT NULL,
    [EventDate] [datetime] NOT NULL CONSTRAINT [DF_EventsLog_EventDate]  DEFAULT (getdate()),
    [LoginName] [varchar](256) NOT NULL
) 

CREATE TRIGGER [LOG_DB_OBJECT_CHANGE] ON DATABASE
FOR create_procedure, alter_procedure, drop_procedure
    , create_table, alter_table, drop_table
    , create_function, alter_function, drop_function
AS

    SET NOCOUNT ON

    DECLARE @data XML
    SET @data = EVENTDATA()

    INSERT INTO DBChangeLog(databasename, eventtype, objectname,
                            objecttype, sqlcommandxml, loginname)
    Select @data.value('(/EVENT_INSTANCE/DatabaseName)[1]', 'varchar(256)')
        , @data.value('(/EVENT_INSTANCE/EventType)[1]', 'varchar(50)')
        , @data.value('(/EVENT_INSTANCE/ObjectName)[1]', 'varchar(256)')
        , @data.value('(/EVENT_INSTANCE/ObjectType)[1]', 'varchar(25)')
        , @data.query('(/EVENT_INSTANCE/TSQLCommand)[1]')
        , @data.value('(/EVENT_INSTANCE/LoginName)[1]', 'varchar(256)')

GO
SET ANSI_NULLS OFF
GO
SET QUOTED_IDENTIFIER OFF
GO
ENABLE TRIGGER [LOG_DB_OBJECT_CHANGE] ON DATABASE
{% endhighlight %}

And that's all there is to it.

You could now easily write a webpage to search and filter this table, and perform diffs on different versions.  And never again have a stored procedure get changed on you, clobbering a fix you did the day before...without having a backup of your fix!

And you can log who made the change, so you can exact some revenge.

-c