---
layout: post
title: Apidays Live Australia 2020
author: wongm
comments: true
tags: [conferences, Apidays]
---

On September 15 and 16, 2020 I attended '[Apidays Live Australia](https://www.apidays.co/australia/)' - a conference about APIs. They describe it as.

> Apidays is about the opportunities and technologies enabled by the programmable economy. As APIs become mainstream, our world becomes more connected, more automated and more intelligent. APIs are the gateway to data, services, devices and emerging technologies. APIs put power into the hands of developers, citizens and consumers.

Due to the COVID-19 situation this  one was held online using the [Hopin](https://news.crunchbase.com/news/meet-hopin-the-platform-for-virtual-conferences/) platform. for a two day conference with three parallel virtual 'stages' there were a massive amount of speakers  - here what I took away from the talks I attended.  

## Speakers ##

### Day 1 ###

**Speeding up the Future of APIs: From Oil Pipelines to Summer Cottages to the Internet of Things - Mike Amundsen, Author of "Designing and building Web APIs" and "Restful API Design"**

- MQTT was a protocol created for oil pipeline telemetaty, and wasn't standardised in the early years. But software developers didn't pay much attention to it until 2008 when someone used it to send IoT data to twitter
- for event driven systems need to think of who will consume the events, think about business not just tech
- use APIs change the state of a domain

**Supporting a versioned API - Andrew McCauley, Developer Success Lead at Shopify**

- they added a new feature to support inventory multiple locations
- their APIs are used by 'apps' (plugins) that clients then add to their online shop
- they use APIs to drive the company's growth flywheel: enable new 3rd party functionality, which drive more users
- they had communication overload leading up to the API migration, with 4 people full time on developer support
- decided to decouple new and old functionality, allow user to opt in to new flow
- didn't remove old pattern initially, but defaulted new users away from it
- then they introduced API versioning, named by date, with quarterly releases, 12 month lifetime of each version, and 9 months to migrate following a breaking change
- if you call a deprecated API method, they return custom HTTP header tell you this, as well detailing migration path 

**Designing for Developers - Gregory Koberger, Founder & CEO of ReadMe**

- in API documentation include sample code, show rather than tell 
- give them a code snippet that actually run on the page using sample credentials 
- don't worry about repeating yourself on every page of documentation, include everything someone needs to use a given method

**API Simplicity and Consistency - Luke Sneeringer, Software Engineer at Google**

- need to look at APIs from a whole of company level, across all products
- make things consistent between all APIs to make it easier for developers using them
- need to choose either string or strongly typed values for properties
- inconsistent property names between APIs for a given domain field will cause confusion 
- so will the same domain having different names
- be consistent, need to design the APIs
- more art than science, you'll spent a lot of time in debates with people
- four layers of governance to keep things consistent: education, code reviews, guidelines, linter
- Google has AIPs to document API design decisions

**From micro to macro-coordination through domain-centric DDL pipeline - Alex Khilko, CTO of PlayQ Inc**

- when modelling the domain information is fragmented, multiple stakeholders
- once domain is finally defined, everything else is 'only' mechanical work - but still time consuming
- you can use code generation from domain definitions to create API clients
- PlayQ has 60-70% of the API client code generated

**Building distributed systems on the shoulders of giants - Dasith Wijesiriwardena, Telstra Purple (Readify)**

- can build a distributed system in the same way as a monolith , but it doesn't really work
- if you do, you'll find all kinds of issues in production
- in a monolith we're making assumptions about network, latency, bandwidth, security
- rule: don't distribute your objects between services
- there are platforms you can build upon
- Azure Service Fabric does it all for you, uses an actor pattern
- Dapr is open source
- Istio/Knative for Kubanates

**Leveraging DevOps to visualise your digital ecosystem -  Mark Crawshaw, Aplas**

- Aplas has a product that does software mapping and indexing

**An economic approach to rewriting a complex payments service - David Julia, Sr Manager at VMware Pivotal Labs Sydney & Venkatesh Arunachalam, Product Manager at VMware Pivotal Labs Sydney**

- they discovered company had forked versions of application, using common database as integration
- focus on ROI when rewriting
- map the value chain, discover pain points, key business metrics

**Strangling the monolith with a reactive GraphQL gateway - Martin Varga, Atlassian**

- they had two platforms, Jira vs Jira cloud, and a legacy monolith
- started extracting elements from monolith
- GraphQL has strongly typed schema
- they used GraphQL to query data in the monolith
- then compose page with data from multiple sources

**Have your cake and eat it too: GraphQL? REST? Why not have both! - Roy Mor, Sisense**

- using GraphQL there is no over/under fetching 
- get exactly the data that is needed, not multiple REST calls
- but GraphQL not silver bullet
- it pushes burden of working out what data you need to the consumer

**Locknote: APIOps Cycles: how to develop business & software together - Marjukka Niinioja, Founding Partner of Osaango**

- APIOps is DevOps for APIs

### Day 2 ###

**Where Great Architecture Comes From - Mary Poppendieck, author of "Lean Software Development: An Agile Toolkit"**

- it doesn't matter what a design looks like paper
- what really matters is what is actually built

**Events are Cool Again - Nelson Petracek Global CTO at TIBCO and Author of "API Success: The Journey to Digital Transformation"**

- we've been going through cycles of technology and patterns
- events are back again
- they allow use to represent the world, by describing change in state

**The Evolution of APIs: Events and the AsyncAPI specification - Aaron Lee, Solace**

- world is moving moving from REST towards eventing
- there are multiple messaging protocols for sharing events

**Solving the maze of technology ethics - Anand Tamboli, CEO of Anand Tamboli & Co**

- do premortem before starting project
- look at ethics from the start

**Evaluating the usability of security APIs - Dr Nalin Asanka Gamagedara Arachchilage, La Trobe University**

- researcher who looked at how developers use common security APIs
- gave them a task to complete using popular open source APIs
- discovered that developers were using the APIs wrong due to poor documentation
- end result is security holes in the resulting code

**Growing Domain APIs - "T'aint what you do..." - Liz Douglass, Partner at Deloitte & Saul Caganoff, Principal at Deloitte**

- when mapping the domain there are bottlenecks, contention, silos of knowledge
- need product thinking, to map the domain boundaries

**Authorised is Not a Yes/No Question - Ben Dechrai, Developer Advocate at Auth0**

- there are levels of permissions
- attribute based access control looks at information like subject / action / object / context 
- role based access control is much simpler, less granular

**API Value Chain - Identifying and developing business assets that deliver the most value - Christian Raquel, Industrie & Co**

- need to involved involve the rest of the business

**Discovering API Version differences with ease - Jaap Brasser, Developer Advocate at Rubrik**

- they use a comparison framework on Swagger files to see differences between builds
- that framework creates a report of the changes

**API Product for Business Ecosystems - Amancio Bouza, Co-Author of "API Product Management**

- businesses are not selling the API, but what that API lets you do
- comparison with Ikea marketing who sell the idea of a lifestyle, not just their furniture
- an API is an interface to the value proposition

## My thoughts overall ##

Normally my main driver for attending conferences is the free food and swag, but that doesn't work online. But luckily for me, there were still plenty of worthwhile talks to listen to from home.  

Kubernetes and GraphQL were popular topics across the conference, but they aren't technologies I use in my day job, but it was interesting to see what they do compared to the alternative technologies I do use.

Business buy-in and event sourcing were also up and coming subjects, both of which I haven't quite nailed at work, so seeing other opinions on the subject was valuable.