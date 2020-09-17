---
layout: post
title: Apidays Live Australia 2020
author: wongm
comments: true
tags: [conferences, Apidays]
---

Apidays Live Australia 

https://www.apidays.co/australia/

...

> Apidays is about the opportunities and technologies enabled by the programmable economy. As APIs become mainstream, our world becomes more connected, more automated and more intelligent. APIs are the gateway to data, services, devices and emerging technologies. APIs put power into the hands of developers, citizens and consumers.

## Speakers ##

**Speeding up the Future of APIs: From Oil Pipelines to Summer Cottages to the Internet of Things - Mike Amundsen, Author of "Designing and building Web APIs" and "Restful API Design"**
MQTT new protocol - early on oil pipelines
wasn't standardised early on
then used in 2008 to send IOT data to twitter
event driven - need to think of consumer, think about business not just tech
APIs change the state of domain

**Supporting a versioned API - Andrew McCauley, Developer Success Lead at Shopify**
new feature - support inventory multiple locations
APIs used by 'apps' that clients then use
api drove growth flywheel
communication overload leading up to API migration
4 people for developer support
decouple new and old functionality - allow user to opt in to new flow
so didn't remove old pattern, but default away from it
then introduced versioning
named by date, quarterly releases, 12 month lifetime, 9 month to migrate from braking change
deprecated reason header


**Designing for Developers - Gregory Koberger, Founder & CEO of ReadMe**
show rather than tell
don't worry about repeating youself on every page
give them a code snippet that runs



**API Simplicity and Consistency - Luke Sneeringer, Software Engineer at Google**
string vs strongly typed - decision
properties by name don't line up between apis
same domain has different names
be consistant - design program
art not science, debates with people
four layers of governace - education, code reviews, guidelines, linter
AIPs - API design decisions


**From micro to macro-coordination through domain-centric DDL pipeline - Alex Khilko, CTO of PlayQ Inc**
information is fragmented, multiple stakeholders
once domain is finally defined, everything else is just mechanical work but time consuming
using code generation from domain definitions
creating API clients
60-70% generated






**Building distributed systems on the shoulders of giants - Dasith Wijesiriwardena, Telstra Purple (Readify)**
can build distributed system same as conventional, but it doesn't really work
if you do, find issues in production
we're making assumptions about network, latency, bandwidth, security
rule - don't distribute your objects
Azure Service Fabric does it all for you, actor patern
Dapr is open source
Istio/Knative for Kubanates






**Leveraging DevOps to visualise your digital ecosystem -  Mark Crawshaw, Aplas**
they do software mapping










**An economic approach to rewriting a complex payments service - David Julia, Sr Manager at VMware Pivotal Labs Sydney & Venkatesh Arunachalam, Product Manager at VMware Pivotal Labs Sydney**
focus on ROI
map value chain, discover pain points, key business metrics
forked code, using common database as integration









**Strangling the monolith with a reactive GraphQL gateway - Martin Varga, Atlassian**
Jira vs Jira cloud, legacy monolith
extracting elements from monolith
graphQL has strongly typed schema
use it to query the monolith
can compose page with data from multiple sources











**Have your cake and eat it too: GraphQL? REST? Why not have both! - Roy Mor, Sisense**
no over/under fetching - get exactly the data that is needed, not multiple REST calls
not silver bullet
pushes burden to the client






**Locknote: APIOps Cycles: how to develop business & software together - Marjukka Niinioja, Founding Partner of Osaango**
devops for APIs





day 2

**Where Great Architecture Comes From - Mary Poppendieck, author of "Lean Software Development: An Agile Toolkit"**
who cares about it on paper, look at what is actually built

**Events are Cool Again - Nelson Petracek Global CTO at TIBCO and Author of "API Success: The Journey to Digital Transformation"**
cycles of technology and patterns
represent the world, change in state





**The Evolution of APIs: Events and the AsyncAPI specification - Aaron Lee, Solace**
moving from REST towards eventing
multiple messaging protocols





**Solving the maze of technology ethics - Anand Tamboli, CEO of Anand Tamboli & Co**
premortem before starting project
look at ethics from the start






**Evaluating the usability of security APIs - Dr Nalin Asanka Gamagedara Arachchilage, La Trobe University**
are developers using the APIs wrong, and making security mistakes



**Growing Domain APIs - "T'aint what you do..." - Liz Douglass, Partner at Deloitte & Saul Caganoff, Principal at Deloitte**
bottlenecks, contention, silos
need product thinking, domain boundaries



**Authorised is Not a Yes/No Question - Ben Dechrai, Developer Advocate at Auth0**
attributed based control:
subject / action / object / contextual
role based vs Attribute-based access control

**API Value Chain - Identifying and developing business assets that deliver the most value - Christian Raquel, Industrie & Co**
involve the business



**Discovering API Version differences with ease - Jaap Brasser, Developer Advocate at Rubrik**
comparison framework on swagger files
creates a report


**API Product for Business Ecosystems - Amancio Bouza, Co-Author of "API Product Management**
not selling the API
but what that APi lets you do
comparison with ikea selling lifestyle, not furniture
interface to value proposition

## My thoughts overall ##

...