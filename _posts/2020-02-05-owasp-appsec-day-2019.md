---
layout: post
title: Notes from OWASP AppSec Day 2019
author: wongm
comments: true
tags: [security, conferences]
excerpt_separator: <!--end_excerpt-->
---

On Friday, 1 November 2019 I attended the [2019 OWASP AppSec Day](https://appsecday.io/schedule/) conference at the Melbourne Convention and Exhibition Centre - here are my quick takeaways from the sessions that I attended.

<!--end_excerpt-->

## Security Learns to Sprint (KeyNote) ##

Tanya Janca, Security Sidekick

- appsec is not covered in uni
- security team enables devops to make system secure
- dev/sec/ops = common 100/10/1 ratio of staff
- security only wins if business wins- look at whole system not parts of it
- security can't be a blocker
- provide secure templates
- reduce false positives
- parallel build pipelines for security
- fix framework, contribute back
- feedback both ways, not push or forced
- think security in requirements, push left. Eg: file uploads, add security requirements
- use dev tooling to flag insecure code in IDE
- negative unit tests around bad data- red team / simulations / chaos ops
- helping people save face
- blameless PIRs
- security champions, encourage them
- impending doom, pessimism is common, so celebrate wins
- first ask about fixes, use proof of concepts, give 'sign off form' to stakeholders who won't fix something

## Hunting Bugs to Extinction with Static Analysis ##

Paul Theriault, Mozilla

- is it just grep?
- audit only once for each type of flaws
- reputation for false positives
- DOM based XSS
- source vs sink (doc write, eval)
- search for innerHTML using grep, gives false positives
- use a parser instead, AST walker, save usages ignore, unsafe ones checked each
- aid for code reviews
- find bad patterns
- use eslint then put it in build process
- fix them all, then add rule
- but new data flows might add insecure data
- Semmle maps to data store, then write queries to see path between them, better than basic string check. same source, different sink
- security flaws are hard to analyse for security team, how to involve developers, need to know context of code
- run these async, in code review, not in the build
- false positive and false negatives: LCI vs audit
- understand current flaws
- some things you'll never find


## How Do I Content Security Policy? ##

Kirk Jackson, RedShield

- XSS is everywhere
- input should be validated or whitelisted
- output should be encoded
- CSP whitelist means you don't need source code
- use report URL tooling
- kill these: <a href="javascript onclick and eval()
- whitelist, but because you can't trust whole domain
- so strict CSP instead, include nonce in header + HTML
- cache gotchas, static nonce + heading, but not a vulnerable security page

## Spot the vuln - Identifying security problems in source code ##

Eldar Marcussen, xen1thLabs

- examples of bad source code

## When Bots Attack - Mischievous Puppets and Stolen Treasures ##

Andrew Logue, REA Group

- IP address blocks, user agent, for rate limiting
- challenge endpoints instead
- KASADA
- returns JS for browser to run
- adding more hoops for user to run
- integration with system

## Adding the Sec to DevOps ##

Andrew Bailey, Telstra

- make things machine readable
- decide on standards: level of security- credentials outside code
- logging: timezones OR UTC
- DAST - dynamic app security testing, end to end, mocks might be misleading
- IAST: instrumented
- SAST: static
- secure build tools, backdoor to prod, devs being targeted
- build in credential rolling
- run baselines
- know what 'normal' is
- OSINT: search for it


## Bulletproof Shoes: Protecting from Accidental Token Leaks ##

Max Feldman, Slack

- tokens on Github are remnants
- DLP apps?
- regex logs to find stuff

## Q&A ##

Tanya Janca, Security Sidekick
Ken Johnson, Github
Sam (Frenchie) Stewart, Cruise Automation
Teri Radichel, 2nd Sight Lab

- Zero days, supply chain attacks: who cares because you've got a lot of basic stuff you're not doing