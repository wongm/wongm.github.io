---
layout: post
title: News Limited and the 'sslcam' redirect
date: 2014-07-08 07:30
author: wongm
comments: true
canonical: https://wongm.com/2014/07/news-limited-websites-sslcam-redirect/
tags: [content delivery networks, interwebs, mystery, newspapers, paywalls, Technology]
---
Recently I was in the middle of researching a blog post, when my internet connection crapped out, leaving me at an odd looking URL. The middle bit of it made sense - www.theaustralian.com.au - but what is up with the sslcam.news.com.au domain name?

<blockquote>https://sslcam.news.com.au/cam/authorise?channel=pc&url=http%3a%2f%2fwww.theaustralian.com.au%2fbusiness%2flatest%2fsmartphone-app-to-track-public-transport-woes%2fstory-e6frg90f-1226863043667</blockquote>

I then started researching the odd looking domain name, with the only thing of note being somebody else complaining about it:

<blockquote class="twitter-tweet" lang="en"><p>What is that stupid sslcam redirect you get trying to load News stories? It&#39;s so stupidly slow.</p>&mdash; Richard Chirgwin (@R_Chirgwin) <a href="https://twitter.com/R_Chirgwin/statuses/180893162443776000">March 17, 2012</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

I then went back to the original link I clicked on, and followed the chain of network activity that followed.

<a href="https://wongm.com/wp-content/uploads/2014/06/news-limited-sslcam-tracker.png"><img src="https://wongm.com/wp-content/uploads/2014/06/news-limited-sslcam-tracker-500x131.png" alt="News Limited using &#039;sslcam&#039; domain to track users" width="500" height="131" class="alignnone size-medium wp-image-4838" /></a>

First hit - the shortened link I found on Twitter:

<blockquote>http://t.co/wLP4Lj9kXP</blockquote>

Which redirected to the article on the website of <em>The Australian</em>:

<blockquote>http://www.theaustralian.com.au/business/latest/smartphone-app-to-track-public-transport-woes/story-e6frg90f-1226863043667</blockquote>

When then redirected me to a page to check for <a href="http://en.wikipedia.org/wiki/HTTP_cookie" target="_blank">cookies</a> - presumably part of their <a href="http://en.wikipedia.org/wiki/Paywall" target="_blank">paywall</a> system:

<blockquote>http://www.theaustralian.com.au/remote/check_cookie.html?url=http%3a%2f%2fwww.theaustralian.com.au%2fbusiness%2flatest%2fsmartphone-app-to-track-public-transport-woes%2fstory-e6frg90f-1226863043667</blockquote>

It then sent me back to the original article:

<blockquote>http://www.theaustralian.com.au/business/latest/smartphone-app-to-track-public-transport-woes/story-e6frg90f-1226863043667</blockquote>

Which then bounced me to the mysterious sslcam.news.com.au domain:

<blockquote>https://sslcam.news.com.au/cam/authorise?channel=pc&url=http%3a%2f%2fwww.theaustralian.com.au%2fbusiness%2flatest%2fsmartphone-app-to-track-public-transport-woes%2fstory-e6frg90f-1226863043667</blockquote>

And third request lucky - the original article:

<blockquote>http://www.theaustralian.com.au/business/latest/smartphone-app-to-track-public-transport-woes/story-e6frg90f-1226863043667</blockquote>

Quite the chain of page redirects!

<strong>The sslcam.news.com.au domain</strong>

 Internet services company Netcraft <a href="http://toolbar.netcraft.com/site_report?url=sslcam.news.com.au" target="_blank">have collated the following information</a>:

<blockquote>Date first seen: January 2012
Organisation: News Limited
Netblock Owner: Akamai International, BV
Nameserver: dns0.news.com.au
Reverse DNS: a23-51-195-181.deploy.static.akamaitechnologies.com</blockquote>

<a href="http://en.wikipedia.org/wiki/Akamai_Technologies" target="_blank">Akamai Technologies</a> is a company that runs a <a href="http://en.wikipedia.org/wiki/Content_delivery_network" target="_blank">content delivery network</a> used by many media companies use - their systems make websites faster to load by saving a copy of frequently viewed content to servers located closer to the end users.

As for the reason for the cascade of page redirects and the mysterious sslcam.news.com.au domain, I'm at a loss to explain it - sorry!

<strong>Footnote</strong>

The sslcam.news.com.au domain is also used by other News Limited websites - the Herald Sun also routes traffic to their website via it.
