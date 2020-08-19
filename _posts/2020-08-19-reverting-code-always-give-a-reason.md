---
layout: post
title: Going down a rabbit hole - reverting the reverted code
author: wongm
comments: true
tags: [support tickets, source control]
excerpt_separator: <!--end_excerpt-->
---

This is the tale of going down the Git history rabbit hole, and being doomed to make the same mistake as the people who came before.

<!--end_excerpt-->

## The tale begins ## 

It all started with an innocuous support ticket.

> Users with the 'system administrators' permission are unable to see the full list of reports, but users with the 'view report' permission explicitly set can see it.

I set off into the code, and found the problem pretty easily - the list of reports was being returned by a stored procedure, which had a `@IsSystemAdministrator` boolean as one of the parameters passed into it, but the value was always set to false.

Easy fix I thought - I'll just modify the code to set `@IsSystemAdministrator` to the actual permissions of the user, and call it a day.

## Nothing is ever simple ##

The fix seemed ridiculously simple, so I took a look at the Git history of the project to see why the boolean parameter was hard coded, and found an interesting commit back from 2018.

> [Ticket-814] Fix system administrators context not being checked by using SimplePermission like report builder

A closer inspection showed the exact same fix I was about to commit, but that was not all - a more shocking commit message also appeared in the history.

> Revert "[Ticket-814] Fix system administrators context not being checked by using SimplePermission like report builder"

But that was it - no explanation of *why* the revert was required.

So why did the fix I was about to apply get reverted back in 2018, and what was I about to unleash by opening Pandora's box? 

## Through the looking glass ##

By this point I was feeling like [XKCD #979 "Wisdom of the Ancients"](https://www.explainxkcd.com/wiki/index.php/979:_Wisdom_of_the_Ancients):

> Never have I felt so close to another soul
> 
> And yet so helplessly alone
> 
> As when I Google an error
> 
> And there's one result
> 
> A thread by someone with the same problem
> 
> And no answer
> 
> Last posted to in 2003

At least for me, the original developer of the 2018 commit was still with the business, so I asked them...

> so i continued the investigation to see if I can remember what caused us to revert the commit but.. there is a but
> 
> i cannot find anything in the two diffs or the ticket itself
> 
> so then i went to our slack channels, which is private...
> 
> and I cannot find ANYTHING in both the channels
> 
> nothing.
> 
> in fact, the discussions are completely different and not even related to any topic related to the problem
> 
> now, I got nothing mate.. apologies as I feel if the diff / ticket had some more info, that would have helped.. ESPECIALLY if the revert diff could have an explanation reason

An apologetic dead end.

## Just wing it ##

Techbros love the "Move Fast and Break Things" mantra, and if there ever was a time for it, it was now. 

I reached out to the customer facing support team, and let them know that I had a fix for the support ticket, but I wasn't confident in it based on the unanswered questions. We decided that we should deploy the fix anyway, and wait to see what happened.

The code fix went to production fine, the support ticket was closed, and I forgot about the fix.

## Attack of the zombie stored procedure ##

Fast forward two months, and a new support ticket landed in my lap.

> Users with the 'system administrators' permission are unable to see the full list of reports, but users with the 'view report' permission explicitly set can see it.

The exact same scenario I had thought was fixed for good - so what was happening now?

I took a look at the client data:

```
SELECT * FROM Reports

3579 records returned
```

And I dug into the code further, and found an interesting line.

```
var reportList = _repository.StoredProcQuery<Report>("reportsByPermission",
    1,                                         // page_number
    1500,                                      // per_page
    where,                                     // where
    "lReportID",                               // col_names
    "sTitle",                                  // order_by
    _currentUser.ID,                           // UserID
    _permission.HasAllPermissions(),           // IsSystemAdministrator
    );
```

Turns out the stored procedure was only returning the top 1500 records, when the client had a total of 3579 reports in their system.

For the bulk of users, which was not a problem, as they had been granted permission to view less than 1500 reports. But for a system administrator this fell over - they got the first 1500 reports, and missed out on the remainder.

So that's why back in 2018 the system administrators permission check code fix was rolled back! 

## And fixing it for good ##

Turns out the page I was trying to fix doesn't show every report in the system - it displays a limited subset of reports by taking the results of the stored procedure, then filtering them using a LINQ statement in the C# code that follows.

Luckily a fix was pretty simple - filter the records using the `where` parameter that the stored procedure already exposes, and hope that no user has permission to see more than 1500 reports. 

And the moral of the story - always document your revert commits. Future you will be thankful!