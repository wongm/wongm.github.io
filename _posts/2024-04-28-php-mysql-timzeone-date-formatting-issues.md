---
layout: post
title: Debugging a timezone issue on a LAMP stack server
author: wongm
comments: true
tags: [PHP, MySQL, date formatting]
excerpt_separator: <!--end_excerpt-->
---

Dealing with timezones is the bane of my life as a developer, but on the other hand, I didn't exactly make life easy for myself - when I setup my server a decade ago, I left it configured to run on the local timezone it was running it, which doesn't match my local time or UTC time, causing all kinds of drama.

The most recent one occured today, when I upgraded the PHP version, and all of a sudden my code is now mangling the time because it's somehow confused about the timzeone.  

In this case "28/04/2024 16:15:02"  should actually be showing 20:15 - my code runs on 'Australia/Melbourne' timezone but server had been configured on 'America/New_York', but for some reason it's switched over to UTC, and now everything is a few hours out of whack. 😭

On first encountering the issue, I swore I've seen it before, but unforutnatly I didn't document it - but then I realised what was going on - each version of PHP has it's own copy of php.ini, and that has a 'date.timezone' directive against it. So you need to update the timezone directive in each file, and I happen to be using both CLI and FPM favours.

With that lead, I made the change - but unfortunately updating the php.ini timezone config for the FPM flavour then restarting Apache web server hasn't fixed the display of times. 

My next theory was that FastCGI Process Manager (FPM) mode in PHP doesn't care about Apache being restarted - so time to reboot the server instead.

And tada - then it worked. 🎉

Moral of the story - [run your server on UTC time](https://www.reddit.com/r/linuxadmin/comments/18rl5rs/should_all_servers_timezone_be_utc/) and convert to local time on the way out to the user.