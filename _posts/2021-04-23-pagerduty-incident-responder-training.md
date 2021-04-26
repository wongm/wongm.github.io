---
layout: post
title: PagerDuty Incident Responder Training
author: wongm
comments: true
tags: [conferences]
excerpt_separator: <!--end_excerpt-->
---

I recently added a session on "Incident Responder Training" by PagerDuty - a company who runs a SaaS incident response platform. So what did I get out of it?

<!--end_excerpt-->

## The session ##

PagerDuty [described the session](https://meet.pagerduty.com/certification) as:

> Further your understanding of Incident Response methodologies by learning about PagerDuty's 10+ years of perfecting our own open-source framework.
> 
> We've adapted the framework used globally by emergency responders to manage technical incidents in a way that reduces downtime, efficiently facilitates resolution efforts, and reviews processes to improve both technical workflows and peer-to-peer culture.
> 
> Throughout the training, we'll demonstrate how you can incorporate our findings into your own organisation, including how to scale each process to reflect your team's structure. This training is suitable for anyone looking to improve their organisation's incident response process and is platform-agnostic.

And it covered:

> 1. INCIDENT COMMANDER SYSTEM 101
> 
> - History and Introduction of Incident Response
> - Establishing Definitions
> - People Roles and Incident Categorisation
> - Being Prepared for Incidents
>
> 2. INCIDENT COMMANDER SYSTEM 201
>
> - Best Practices for Incident Commanders
> - Other Roles in Incident Command
> - Incident Response at Scale
> - Incident Response Pitfalls and How to Avoid Them
>
> 3. POST-MORTEM WORKSHOP
> 
> - Post Mortem Basics and Introduction
> - Blame and Biases in the Workplace
> - Buy-in and Introducing Postmortems to your Org
> - Quick Tips for Success

## My takeways ##

The term "incident response" is something used by PagerDuty, but other companies might call it something else. The are a response to unplanned disruption that affects users. Aim is to handle the issue, not necessarily solving them. A structured process will help to replace replace chaos with calm. The Incident Command System (ICS) is a standardized hierarchical structure used in many fields, not just IT.

Minor incidents include:

- known failures
- responsibility of small team
- not urgent

Major incidents are:

- a surprise
- cross functional
- urgent

When responding to an incident, initial resources is just a 'responder'. The incident response can then bring in other roles as severity increases. Roles include:

Command level: 
- commander
- deputy commander
- scribe 

Liaison level:
- internal liaison
- customer liaison

Operations level:
- Subject matter experts

Anyone can trigger incident response process - don't need to wait for the specified metrics to occur, because people are smart enough to foresee if the issue will get worse.

Commander doesn't do things to solve the problem, but delegate to others. They also need to ensure that actions are documented for future review.  Commander needs consensus from the response them - to save time ask for strong objections, instead of agreement from everyone.  They also shields / buffers the SMEs doing the response from outsiders wanting updates. If there is no incident commander, then you are. :-P 

When communicating avoid abbreviations, want clear rather than concise discussion. Ask what are the risks - time taken to address issue vs impact of the outage. Assign tasks to specific person not to a group, as it might get missed. Timebox tasks to follow up, and if they run out of time and task not finished, ask them how much time they needed.

Deputy commander is not just following along, but takes on extra tasks for the commander. They follows up reminders, ensure items are not missed. They are also standby if commander is drawn away.

Scribe capture information for follow-up or post mortem. However they are not just a stenographer - they are allowed to add extra details as needed, logging as it is happening to somewhere, Slack is common example.

Liason role typically supports team members, and notifying customers in appropriate language. Be careful - are updates too frequent that you spend more time writing them than creating them.

When in a major incident, standard rules might be loosened, for example deploying of code without automated tests. Need to define roles, and create runbooks ready for when an incident occurs.

Postmortems are not a punishment, but for learning from the incident. They are blameless, should be asking 'what' and 'how' instead of 'who' and 'why' questions. Anyone can facilitate one. Physiological safety helps to bring down barriers.

## My thoughts overall ##

## Further reading ##

- https://meet.pagerduty.com/certification?mkt_tok=MDkzLVZQSi04MDcAAAF8TaVE2BupVDP8Tte7GmmLRYrY3sVLfomrKwdD5OC_NfJFNEVEJIq-oMIz1yKf5_z4LWWbID6Pabud7KmR4C51q44BpB3cgUsP20BCpewCA-M1A7U

https://response.pagerduty.com
https://postmortems.pagerduty.com/


SNIP