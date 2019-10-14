---
layout: post
title: A day at New Relic FutureStack 19
author: wongm
comments: true
tags: [New Relic, websites]
---

Every year application performance monitoring and analytics company New Relic runs a day called 'FutureStack' where they show off new features  of their product, and New Relic clients show how they've used it to improve the software that they deliver to their users.

I'm an occasional of New Relic at work, so I went along to the 2019 edition in Melbourne for free food and swag, and gained a few interesting insights along the way.

## Speakers ##

**Keynote: Creating More Perfect Software, Together**

- Jim Stoneham, CMO, New Relic
- Julian Guica, Product GM Logging & Principal Software Engineer, New Relic
- Greg Taylor, Group VP Asia Pacific, New Relic

> New Relic CMO Jim Stoneham will discuss how organizations can harness modern software as a strategic asset to gain competitive advantage and drive top-line growth. Stoneham and guests will demonstrate how New Relic’s latest innovations empower teams to find, visualize, and understand everything they need to deliver more perfect software.

The big push was around New Relic's ability to monitor your application in their unified 'New Relic ONE' application suite, instead of jumping around in different tools.

Interesting features:

- tracing AWS Lambda requests from end to end
- unified tracing between microservices
- including the above requests with legacy code monitored using the New Relic agent
- ingesting application logs from AWS CloudWatch as well as other open source logging frameworks, and show them alongside New Relic traces
- 'New Relic One' is a programmable platform that you can build apps on top of - it appears to be similar to the Salesforce platform

**Driving Digital Customer Experience**

- Chris Fechner, Chief Digital and Product Officer, Service NSW

Government agency using IT in a modern agile way and putting customers first, not the old fashioned waterfall model that is the butt of jokes about the public service.

Service NSW acts as the customer face for multiple government agencies, so are in effect building a facade over existing legacy backend systems.

Aiming to deliver the same experience in person, phone and digital channels by being smart about the systems that they build.

**The Digital Future of Insurance**

- Daragh Martin, DevOps Chapter Lead, Digital Engineering, IAG

They have multiple insurance brands, acquired over time as separate companies, so have a massive technical legacy to deal with. Have been rebuilding core customer facing systems in a multi-branded way.

**Accelerating and Optimising your Cloud Journey**

- Sean Hooper, Transformation Architect, Amazon Web Services
- Max Bausher, Solutions Architect, New Relic

AWS and New Relic have been working together to get companies into the cloud from on-premises infrastructure.

New Relic's new 'cloud optimise' tool looks at on-premises  resource usage gathered via the New Relic agent, and suggests the savings if you moved the application to AWS.

**Learnings in Scaling Software and Teams at Hyperspeed**

- Sohail Siddiqui, CTO, Afterpay Touch

They seem to be doing the same devops as every modern tech startup:

- team that builds the product supports it
- support cases and alerts aren't a separate ops team
- they've used New Relic features to monitor their canary servers post deployment

**From Legacy Systems to Leading Edge Technologies**

- Leigh Gibson, Tech Area Lead - Customer Self Service Tribe, ANZ Bank

Bank IT is the but of jokes, but ANZ has been running a successful digital transformation project thanks to it being led by the CEO and executive level, not bottom down from IT.  The change to the ways of working did result in discontent from 'old timer' IT staff who didn't like change, so some churn in staff. Teams have been empowered to make changes, to learn and adapt from failure, instead of the previous locked down environments. However bank is still subject to government regulation, so not moving as fast as they would like to. In additional still a large legacy structure - 128 interfaces to the existing monolith.

**Monitoring Lambda and Serverless Applications**

- Adrian De Luca, Head of Solution Architecture, Amazon Web Services
- Jill Macmurchy, Technical Director Asia Pacific, New Relic

New Relic now lets you see AWS Cloudwatch logs alongside APM style graphs, allowing you to see the number of unhandled exceptions, cold starts, and the stack trace of errors.

Also a few random AWS features got mentioned:
- Amazon EventBridge for sending events cross account
- check reserved concurrency per accounts, otherwise you Lambda might get throttled
- Lambdas still need to have CPU and memory allocations reserved
- AWS has made a VPC network fix to reduce the number of Lambda cold starts

**Australia Post's Journey to Modern Architecture and Observability**

- Andrew Nette, Head of Platform Engineering, Australia Post

They've gone beyond monitoring of system being up, to understanding the internals of them.  By using standard tagging and logging, they have been able to make use of New Relic data better. 

One gotcha they discovered with existing systems: health check endpoints were actually checking all dependencies that that system needs, resulting in outages that monitoring didn't pick up.

**Generating Context From Your Log Data**

- Spence Taylor, Fullstack Engineer, New Relic

> Modern cloud infrastructure, applications, and web services require deep traceability and insights throughout the stack to troubleshoot and resolve issues faster and provide a superior customer experience. In this modern, digitally-driven economy, developers and DevOps teams don’t have the luxury to spend the time to manually piece together information to analyze data and find the root cause from disparate telemetry sources. In this talk, see how you can bridge the visibility gap between your metrics, events and log data, gain deeper insights to identify performance bottlenecks and troubleshoot production issues, and reduce MTTR. 

Sales pitch for a new feature - pulling logs from your existing logging systems into New Relic, so that you see it alongside the other data that New Relic already collects.

Some interesting points beyond the pitch:

- decorate your logs with useful information like machine ID, etc, so that you can then search for common themes.
- setting correlation IDs in a request means you can track them cross service boundaries.
- once you know what logs results in system errors, you can then add alerts against them, so that you can preemptively fix them before the entire system goes down.

And a wider UX takeout - New Relic put lots of effort into making search super fast so that users don't need to carefully consider what search query they are making. Instead allows them to keep on tweaking the search term until they see what they want. You can do the same for your own product.

**Our Journey to Serverless Adoption**

- Hesky Ji, Engineering Manager, Australian Broadcasting Corporation (ABC)

When they first moved to AWS Lambdas they had a problem - what is failture if you have no health check endpoints? They had to revisit what the definition of uptime is, and improve their monitoring.

At scale they found problems: they work around the problem of post deployment cold starts in Lambdas by prewarming the 'new' version with duplicate requests. They also needed to validate their assumptions around CPU and RAM allocations once in production.

**Origin’s Digital Journey: Pushing the Speed Limits on the Road to Success**

- Evgueni Iakhontov, Head of Digital IT, Origin

After the government regulated the way that energy company advertised pricing discounts, they have moved to differentiating themselves based on services.

Their previous mobile app was a monolith that took forever to deploy and difficult to test, resulting in bugs that didn't get found until the end users found them and complained to their call centre. They now use New Relic Synthetics to monitor the app.

**How we Got Our Deployments Soaring**

- Kiel Frost, Ecommerce Technology Manager, Flight Centre Travel Group

A small team within a company that sees IT as a cost, and not their product. Their previous system was a monolith developed around a 3-6 week cycle time. They have to integration with multiple 3rd party APIs, that are often changed by airlines with little notice.

They were able to speed up the development cycle by examining their workflows - two week testing cycle actually only needed 3 days, but it that way 'just because'. By isolating credit card payment code (PCI DSS) code separate from the rest of the system, they have been able to reduce regulatory overhead while improving security.

**Getting to Sub-Second Incident Response with AIOps**

- Dor Sasson, Senior Product Manager AIOps, New Relic

> Machine learning and artificial intelligence are revolutionizing the way operations teams work. In this talk, we’ll explore the incident management practices that can get you toward zero downtime, and talk about how New Relic can help you along that path.

Nuisance alerts are a pain for devops teams - New Relic thinks their new AI logic means they've fixed the problem without you needing to do anything.

## My thoughts overall ##

I've been using New Relic for monitoring systems at work since at 2012, as well as my own personal server since 2013, which is a completely different tech stack, and the insights it gives me is amazing at just the 'free' level. 

In addition it has allowed by to debug some hairy system speed issues in Azure hosted services, where a New Relic paid account was the only way to get the visibility I needed in order to fix the issue.

The only downside of New Relic is the cost -  and given that I don't have enough time to use that data New Relic is already giving me for free to improve the health of the system, it's been hard for me to be able to justify the ongoing spending. 

That doesn't really seems to be changing - while the new features described above are only available to paying customers, but there are other much lower cost monitoring systems that provide much of the same information, albeit in separate silos.

So my final take - New Relic is the 'Rolls Royce' of application monitoring, but there are many cheaper alternatives.