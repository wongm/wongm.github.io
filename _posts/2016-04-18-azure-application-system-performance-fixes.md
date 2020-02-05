---
layout: post
title: Performance tuning an Azure microservice post go-live
author: wongm
comments: true
tags: [Azure, cloud, devops, operations]
---

Back in the early days of 'devops' I was a young developer who inherited a brand new microservice hosted in Azure, and with it the responsibility for keeping it up and running in a cost effective manner as the number of clients using it grew. 

**The service**

The microservice used Cloud Services for the web frontend and backend worker process; along with Table Storage, Queue Storage and Blob Storage to store data.

Unfortunately for me the original developers were no longer around, but I had the support of our wider ops team to debug the hairer issues. This is what I learnt. 

**Common themes**

- Poorly written code that doesn't perform at production scale.
- Cached data not being invalidated resulting in incorrect behaviour.
- Frequently used data not cached at all causing performance issues.
- Autoscale settings means system does not respond under load.

**The problems**

- Problem: Data beyond first 1000 records is not displayed 
 - Solution: Use continuation tokens for Azure table storage 
 - Expected result: All data is shown 
 - Success? Yes 

- Problem: Ballooning item count in Azure blob storage 
 - Solution: Ensure worker role deleting old search indexes after new index is created 
 - Expected result: Item count is kept low 
 - Success? Yes 

- Problem: Worker role memory leak 
 - Solution: Add New Relic monitoring 
 - Expected result: System is faster 
 - Success? Inconclusive 

- Problem: Worker role memory leak 
 - Solution: Remove static variables 
 - Expected result: System is faster 
 - Success? Yes 

- Problem: Data feed processing issues 
 - Solution: Add tasks to an Azure queue instead of looping over records returned by a database query
 - Expected result: Processing is faster 
 - Success? Yes 

- Problem: System configuration is reverted following edit
 - Solution: Ensure configuration is pulled directly from table storage, not via cache 
 - Expected result: Correct configuration is seen 
 - Success? Yes 

- Problem: Newly added records taking forever to appear 
 - Solution: Add HTTP 304 not modified support to data feed, skip feed processing if no new records
 - Expected result: Feed processing is quicker 
 - Success? Yes 

- Problem: Incorrect HTTP 304 response is being returned, so that new records don't appear 
 - Solution: Can't invalidate cache when record is saved due to monolith issues. Instead generated an ETag based on the data included in the feed.
 - Expected result: Updated records appear in timely manner 
 - Success? Yes 

- Problem: Needing to turn off email sending for deployment 
 - Solution: Change processing logic to use a queue to avoid concurrent actions 
 - Expected result: Each email is only sent once 
 - Success? Yes 

- Problem: Queues growing to 10000s of records 
 - Solution: Don't add queue items blindly - check for existing records, only add when empty 
 - Expected result: Queue doesn't expand forever 
 - Success? Yes 

- Problem: Search is slow 
 - Solution: Replace Lucene search filtering with LINQ queries for simple queries 
 - Expected result: Search is faster 
 - Success? Yes 

- Problem: Table Storage queries are slow 
 - Solution: Repartition data to use logical groupings and row keys 
 - Expected result: Table Storage queries are faster 
 - Success? Yes 

- Problem: Worker roles are not running as redundant roles in Azure
 - Solution: Ensure all worker roles are redundant 
 - Expected result: Microsoft can take down worker roles and code keeps on running 
 - Success? Yes 

- Problem: Cache is slow 
 - Solution: Use Azure shared cache for careers instead of home spun Memcached impementation 
 - Expected result: Cache is faster 
 - Success? Yes 

- Problem: Sourced records disappearing 
 - Solution: Don't blindly delete and recreate records each time the data feed is reprocessed. Instead, delete the records that are no longer valid, add the records that need to be added, and update any of the records that are already there. Also change calls to Table Storage entities to use batch operations. 
 - Expected result: Correct records are seen 
 - Success? Yes 

- Problem: Sourced records disappearing 
 - Solution: Ensure old Lucene databases are not deleted quite as fast as before. There is issue with other deployments on same account retaining references to a Lucene database in cache, then having it deleted beneath them, breaking the search page. 
 - Expected result: Correct records are seen 
 - Success? Yes 

- Problem: Slow system speed 
 - Solution: Change templating engine to use lexer / parser to get list of attributes, not a complicated regular expression. Old regex was timing out at 30 seconds on long templating fields! 
 - Expected result: System is faster 
 - Success? Yes 

- Problem: Cache times out 
 - Solution: Change to use dedicated cache role, not shared cache on web roles 
 - Expected result: Cache no longer crashes out 
 - Success? Yes 

- Problem: New records not appearing in search 
 - Solution: Add logging when add/remove records from cache 
 - Expected result: Work out if a stale cache is to blame 
 - Success? Yes 
 - Notes: Cache was not being refreshed

- Problem: Data feed processing takes too long 
 - Solution: Change default worker role count to 4 
 - Expected result: Data feed is processed quicker 
 - Success? Yes 

- Problem: System dies when Microsoft updates web roles 
 - Solution: Update config to deploy 5 web tier boxes by default 
 - Expected result: Microsoft can update a single role, and existing roles don't die from overloading 
 - Success? Yes 

- Problem: Search is slow 
 - Solution: Increase worker role instance size to 'medium' - Lucene runs on those machines! 
 - Expected result: Search errors stop 
 - Success? Inconclusive 
 - Notes: Stays less than 25% CPU, but 75% to 100% RAM usage.

- Problem: Cache role running out of memory 
 - Solution: Increase role size from 'small' to 'medium' 
 - Expected result: Random 'internal server error' issues go away 
 - Success? Inconclusive 
 - Notes: Using 1 GB to 1.4 GB of RAM on the new instance sizes

- Problem: Cache too expensive to run 
 - Solution: Change cache to be 3x 'small' instances 
 - Expected result: Available memory is shared between all servers in cluster - cut costs 
 - Success? Inconclusive 
 - Notes: Still using 100% RAM usage, but across three boxes

- Problem: Search is slow 
 - Solution: Increase default search worker role count to 3x 
 - Expected result: Heavy overnight load on search roles declines 
 - Success? Inconclusive 
 - Notes: Peaks and troughs in RAM usage, cycle about 2 days long

- Problem: Web frontend is slow 
 - Solution: Upgrade to SDK v2.6 
 - Expected result: Web transaction response time in New Relic is less 
 - Success? Yes 
 - Notes: Consistent 400ms or so app server response time, massive peaks in response time much less common, hits on Table Storage now higher

- Problem: Massive spikes in response time still exist 
 - Solution: Change bottom end target CPU to 20% in autoscale config 
 - Expected result: Azure no longer kills off servers when they are still needed 
 - Success? Yes 
 - Notes: Peaks in response time once once that day

- Problem: Massive spikes in response time still exist 
 - Solution: Add log item each time template parsing logic is run 
 - Expected result: Is there a correlation between template parsing and high CPU load on servers? 
 - Success? Still in progress 
 - Notes: Have seen a massive spike in template parsing that correlated with slow system speed

- Problem: High number of hits on Table Storage 
 - Solution: Readd the caching logic that was accidentally removed 
 - Expected result: Number of hits on table storage declines to previous level 
 - Success? Yes 
 - Notes: Number of table storage hits is down to 5k-20k from 60k - but not down to consistent 5k seen before

- Problem: Spike in template parsing times 
 - Solution: Correlate logs against metrics for rest of the system 
 - Expected result: What is the cause of the spikes in template parsing load time? 
 - Success? Yes 
 - Notes: Template parsing slows down when rest of system slows down due to RAM usage

- Problem: High RAM usage is to blame for load time spikes 
 - Solution: Come up with ideas to reduce RAM usage 
 - Expected result: Ideas to reduce RAM usage 
 - Success? Yes 
 - Notes: Prioritise IOC change, and switch to stringbuilder in templating engine

- Problem: Table Storage queries are slow 
 - Solution: Upgrade Table Storage SDK from WCF to JSON calls 
 - Expected result: Table Storage queries are faster 
 - Success? Inconclusive 
 - Notes: New Relic says hits to Table Storage just as frequent, and take the same amount of time

- Problem: High RAM usage is to blame for load time spikes 
 - Solution: Change IOC lifecycle config to reduce RAM usage 
 - Expected result: RAM usage will grow slower 
 - Success? Yes 
 - Notes: RAM usage was growing at a slower rate than before

- Problem: High RAM usage is to blame for load time spikes 
 - Solution: Change templating engine to use stringbuilder, not string concatenation 
 - Expected result: RAM usage will grow slower 
 - Success? Yes 
 - Notes: RAM usage was growing at a slower rate than before

- Problem: High RAM usage is to blame for load time spikes 
 - Solution: Change IIS application pool to reset based on memory usage 
 - Expected result: Each server will recycle app pool before load time spikes occur 
 - Success? Yes 
 - Notes: Spikes in load time are GONE!

- Problem: High RAM usage is to blame for load time spikes 
 - Solution: Manually close MemoryStream 
 - Expected result: RAM usage will grow slower 
 - Success? Yes 
 - Notes: RAM usage was growing at a slower rate than before

- Problem: Using too many web servers costs $$$ 
 - Solution: Scale up new web servers less aggressively. From 20-35% 4 at a time to 30-60% 2 at a time 
 - Expected result: Use less web servers 
 - Success? Yes 
 - Notes: Less servers, but we did see an outage due to high CPU usage

- Problem: Outage due to high CPU usage 
 - Solution: Scale up new web servers when needed. Decrease the 'add more server' CPU usage % from 60% to 55% 
 - Expected result: No high CPU usage outages 
 - Success? Yes 
 - Notes: Less web servers running, no outages

- Problem: Autoscale is adding servers that are not needed 
 - Solution: Increase minimum number of servers overnight from 3 to 4 
 - Expected result: New web servers scale up less aggressively 
 - Success? Yes 
 - Notes: Peak of scaling is 9 instances - not 20 or so!

- Problem: Worker roles cost too much 
 - Solution: Reduce number of worker roles from 4 to 2 
 - Expected result: Worker roles cost less 
 - Success? Yes 
 - Notes: Processing queue down from 5 minutes to just 2 minutes

- Problem: Worker roles cost too much 
 - Solution: Run more threads on current worker roles 
 - Expected result: Worker roles cost less 
 - Success? Yes 
 - Notes: Processing queue down from 5 minutes to just 2 minutes

- Problem: Worker roles cost too much 
 - Solution: Change from 'Medium' to 'Small' size instances 
 - Expected result: Worker roles cost less 
 - Success? Didn't try 
 - Notes: Production worker role starts out at 50% memory usage, eventually hits 100% / 3.5 GB. Small instance has half the RAM of current version.

- Problem: Email processing too slow 
 - Solution: Make number of processing threads configurable, set to 3 for now 
 - Expected result: Queue is processed faster 
 - Success? Yes 
 - Notes: Setup to use 3 threads, more items were processed, long running tasks less likely to block others

- Problem: Email processing runs out of memory 
 - Solution: Reduce number of processing threads to 2 
 - Expected result: Less errors in the logs 
 - Success? Yes 
 - Notes: Fewer out of memory issues than before

- Problem: Email for big clients blocks smaller clients 
 - Solution: Add 'watchdog' timer to abort processing of big tasks 
 - Expected result: Queue is processed faster 
 - Success? Didn't try 
 - Notes: We audited run times for processing, decided that current runtimes are not an issue

- Problem: Too many table storage hits 
 - Solution: Cache frequently used records records
 - Expected result: Fewer table storage hits 
 - Success? True


