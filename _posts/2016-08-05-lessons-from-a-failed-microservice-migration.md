---
layout: post
title: Lessons from a failed microservice migration
author: wongm
comments: true
tags: [microservices, websites]
---

This is the tale of a failed pivot of a development team to a microservice architecture, and the lessons they learnt along the way for anyone else attempting the same thing.

**First - some definitions**

From [Wikipedia](https://en.wikipedia.org/wiki/Microservices)

> A workshop of software architects held near Venice in May 2011 used the term "microservice" to describe what the participants saw as a common architectural style that many of them had been recently exploring. In May 2012, the same group decided on "microservices" as the most appropriate name. 

And [Martin Fowler](http://martinfowler.com/microservices/):

> "Microservices" became the hot term in 2014, attracting lots of attention as a new way to think about structuring applications. I'd come across this style several years earlier, talking with my contacts both in ThoughtWorks and beyond

**And the story starts**

We begin in the early 2000s with a Software as a Service platform created before SaaS was an acronym. The product consisted of three Classic ASP web applications with a shared Microsoft SQL Server database, and a number of backend services to provide batch operations. Over time additional ASP.NET code was added to the system, resulting in a classic monolith.

By the late 2010s the limitations of this architecture were starting to bite, with developers unable to move as quick as they used to be, so we started looking at alternatives to the monolith application pattern.

The result was a number of standalone applications, each unique in their own way...

**Microservice A**

Standalone app to built to demonstrate Microsoft technology.

Technology:

- Built by small standalone dev team
- Written in C# / ASP.NET MVC hosted in Azure
- No links to the monolith system or user accounts
- Later it was used to power the backend of a Windows Mobile app

What happened?

- Product didn’t get traction so was shut down
- Concept was rolled into the core monolith product

**Microservice B**

Middleware layer between monolith system and a 3rd party service provider.

Technology:

- C# and WCF webservice hosted in Azure

What happened?

- Logging data not accessible to support team via existing tooling
- Code was moved into monolith system

**Microservice C**

Revamped segment of core product, but targeted at emerging markets.

Technology:

- Built by small standalone dev team using C# and .NET, along with contractors
- They tried to fix every problem with the monolith system
- Spent months yet delivered no business value
- Switched to Ruby and PostgreSQL DB on Linux and eventually got a product out

What happened?

- Existing C# developers didn't want to program in Ruby
- Product overlaped existing core product without adding any extra value
- Expanding features in two system was not sustainable
- Monolith application was updated to support the target market

**Microservice D**

Revamped segment of core product, due to the existing code being unmaintainable.

Technology:

- Built by small project team, along with contractors
- Ruby and PostgreSQL DB on Linux boxes
- Had to build out API and authentication services to connect to rest of monolith system

What happened?

- No developers wanted to maintain Ruby code
- No devops teams wanted to maintain Linux boxes
- Hard to migrate existing clients onto new platform
- Hard to onboard new clients onto new platform
- Speed for end user atrocious due to slow API and authentication services
- No support for reporting
- System was replaced by expanded monolith application

**Microservice E**

Standalone authentication service to support new microservice applications.

Technology:

- C# code hosted in Microsoft Azure
- Homebrew OAuth implementation

What happened?

- Moved to monolith data centre to save hosting costs
- Homebrew OAuth implementation replaced by out of the box OAuth implementation

**Microservice F**

New functionality that extended existing product.

Technology:

- Written in C# / ASP.NET MVC
- Standalone look and feel
- Used API calls to get data from monolith database
- Monolith system had hard coded links into microservice hosted pages

What happened?

- Product didn’t get traction
- Similar functionality added to the monolith product

**Microservice G**

Internal tool to manage client resources.

Technology:
- Written in C# / ASP.NET MVC
- Standalone look and feel
- No API calls or authentication with monolith

What happened?
- Open source product did the same job, so tool was shut down

**Some lessons learned**

The microservices 'fad' detailed above lasted around three years, and resulted in the birth and death of six separate microservices. After their failure, the decision was to double down on the existing monolith, with all new functionality being built inside it again.

That also failed to improve the velocity of developers - so we started a second, more successful attempt at building new functionality outside of the monolith codebase.

So what is there to learn from the experiment? From a product perspective:

- Speed for end user is everything: distributed system is no excuse.
- Users only want to log in once: users don't care if it's a microservice or monolith.
- Users want to be able to report on data: both microservice and monolith data needs to end up in one place for reporting.
- Customer facing support teams need to access application logs: your internal tooling needs to integrate microservice and monolith data.

And for dev teams:

- Developers have to be onboard with the tech stack.
- Don't spend developer time on undifferentiated heavy lifting: find a 3rd party product to do it for you.
- Don’t try to fix *every* problem with a big bang rewrite.
- Think about how existing clients will be migrated to new platform.
- Think about how new clients will be implemented on the new platform.
- Somebody will always think hosting costs are too high.
- Parallel development on new and old platforms is a killer.

And finally...

If it all gets too hard, crawling back to the monolith system probably isn't the answer - the problems that led you to try and escape will still be there. 