---
layout: post
title: A quick overview of New Relic
author: wongm
comments: true
tags: [New Relic, websites]
---

New Relic is an application performance monitoring and analytics service that allows you to watch how software behaves one deployed it to production. I've been using it since 2012 - here is a quick introduction of how it works.

For many developers, it was "the email we all get on Monday mornings but never look at".

![New Relic weekly email](/images/newrelic-1-weekly-email.png)

But it is much more than that - New Relic gathers data from our production applications:

- Page load times (server AND end user)
- Server load (CPU and memory)
- Slow pages
- Errors
- Profiling page requests

And can then send alerts on this data.

It does this with a New Relic agent installed on the server. It is configured using `app.config` or `web.config` file to enable tracking of applications, and sniffs inside IIS and other applications to gather data. It also injects Javascript into outgoing pages to collect browser data.

Once you log in, you get a list of all applications that New Relic is tracking. From there you can drill down to statistics for each application, application errors and stack traces, traces for slow pages, list of all extenal service calls, server CPU and memory usage, and end to end page load times in the web browser. 

![New Relic browser load time data](/images/newrelic-9-browser-data.png)

There is a free tier that provides 7 days of data retention, with paid plans giving extra features, along with longer retention of data.


