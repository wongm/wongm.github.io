---
layout: post
title: How do developers deal with change
author: wongm
comments: true
tags: [microservices, websites]
---

It is easy for some developers to get set in their ways...

I have to do Task X:

- If I've done it before, find that code and base it on that
- If I haven't, then search the codebase for something similar, and base it on that
 
But where does that lead to problems?

An example:	

- Q: Why isn't the build command I always use not working?
- A: It shouldn’t - because it has been replaced by a new one.

Another example:

- Q: How do I initialise this `QueryFactory` class?
- A: You don't - that is an old design pattern.

In a large codebase this means we we have plenty of great new things, but not every developer knows about them.

We should be reducing technical debt, but often we add to it thorugh ignorance.

By default people will keep on doing things the way they know. But repetition turns “new way” into “normal”.

There are a few different ways to bed in a new change.

- Take away the alternative (Git vs TFS)

- Mark old way as deprecated (all of those errors in our codebase)

- Beat everyone over the head until they can't take it any longer (what I am doing now)

So…	

Once can't rewrite an entire code base overnight, but one should keep refactoring in mind. 

After a few times, new dev practices become second nature, and if you aren’t sure, then ask.






