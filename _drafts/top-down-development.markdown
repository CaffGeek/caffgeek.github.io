---
layout: post
title: Top-Down-Development
date: 2015-03-26 15:41:25.000000000 -05:00
categories: []
tags: []
status: draft
type: post
published: false
meta:
  _edit_last: '1'
author:
  login: gibble
  email: chad.hurd@gcsquared.com
  display_name: caffgeek
  first_name: Chad
  last_name: Hurd 
---
Develop top down.  
 need to know what you're building before a foundation can be designed  
 detailed design: engineer bluprints = developer code  
 build: construction = compile

more thoguhts:

It’s going to be difficult to fix the way the project was handled from the beginning, so this is a rather long response; I’ve broken it up into sections

Current State

It appears the work plan focused initially on layers and developed the Database first, then the data and business tiers second, it ties your hands come time to develop the screens. You’re handcuffed by any decisions that were made prior. This is making further development both difficult, and slower than it should be. The current API exposed a whole bunch of individual calls for updating addresses, separate from updating the CUInfo, therefor you are stuck making half a dozen calls and you can’t properly encapsulate it into a single atomic transaction. The application has the following layers DB->BusinessLogic->Repositories->Orchestration->ControllerAPI->UI, but there’s no real logic in any of the layers. No layer uses multiple repositories and merges data together for consumption by the API or UI. For example, it just passes an Address to an AddressRepo, to a Get/Save Address Method in Orchestration, to a Get/Save Address method at the API, ignoring that you never actually just update an address on its own. It’s always part of updating CUInfo. Having separate repositories is good, however, BusinessLogic, Orchestration and/or the API should be abstracting that away, creating methods that match actual use cases of the application. That’s a very important point, while your entities and lower layers are very use case agnostic, the closer to the UI you get, the more your objects, and methods should map to the actual use cases of your application.

Ideal State

Ideally what would happen is you start with an empty database (or in this case, the database without any CURT specific changes). You then start creating a shell for your app, getting security and the main screen up and running. They’ll have a little bit of database interaction, which will force you to create your DBContext, Repositories, and Controllers, BUT only those which are needed to support the work you are doing right now. Work in vertical slices through your application, completing entire features. You never create code you need later, you always create as needed. You typically want to build from the consumer of information, down to the provider of information. It’s seems backwards, building the provider of data last, but it results in much cleaner code; and a much simpler to use and maintain system. (Similar to building a house, you can’t actually design the foundation, until you know what the house it’s supporting looks like.) This is because when developing the screen, you can mock objects and calls to the API that provide information the way the UI needs to consume it. Once that’s done. Building the API controllers, and data model to support it are MUCH simpler, as you KNOW what you need, you’re not guessing as to what will be required. This does mean that you’ll refactor your business logic more often, to find a balance between a clean data model, and a clean API, but it ends up being quicker to develop, and if done while writing unit tests, refactoring becomes simpler and safer as you know immediately if you’ve broken something. In addition, by building an app in vertical slices, feature by feature, you always have a working application. It might not have all the features you want, but it’s very easy to give the user something to see or test early, and features can be prioritized in such a way that you can ensure when the deadline hits, you have a working application, perhaps with some features missing, but it’s a working system, that can be put into production, and features added as they are completed. It’s about limiting work in progress. By working in vertical slices, your API would have a CreditUnionController, with Get and Post methods. And the object you pass in would be a deep object, with address objects as properties, rather than just an AddressId. This data would be quickly given basic validation and a permission check, then passed on to Orchestration, where any business logic would happen. Then, mapped to entities and passed on to the repositories for saving as a single unit of work that succeeds or fails together (no partial saves). All of this is built feature by feature, from the top UI, down to the database. Nothing is developed before it’s needed. And ideally, everything is having tests verifying behavior as it’s written.

Transition from Current to Ideal State

Now, we’re limited in approach to actual get to the ideal state. I would start by adding a method to the CUInfo controller that accepts a single, deep object (basically the entire view model as it sits on the client), then that’s passed to a similar orchestration method that breaks it up into entities for the repositories to save. It would take a major overhaul to get a proper unit of work that can be saved or rejected as a single unit, because each repository currently calls SaveChanges() on its own. So they will still succeed or fail individually if that’s not done, however this first step would make it possible for you to start improving the API to something that’s usable by the application’s UI without multiple calls to perform a single save. By writing GET methods in the API that match more closely with the object model the UI requires it simplifies that interface. The underlying lack of units of work would still remain until the orchestration and data layers could be rewritten to support a unit of work that spans repositories. But, cleaning up calls from the top down is a first step to getting there.