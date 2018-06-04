---
layout: post
title: Fixing my blog robot
date: 2015-05-25 07:30
author: wongm
comments: true
canonical: https://wongm.com/2015/05/wordpress-scheduled-post-issues/
tags: [blogging, cron jobs, interwebs, PHP, scheduled posts, Technology, web hosting, WordPress]
---
One thing you might not know about this site is that I don't actually wake up each morning and type up a new blog post - I actually write them ahead of time, and set them up to be published at a future time. Unfortunately this doesn't always work, such as what happened to me a few weeks ago.

<a href="http://railgallery.wongm.com/xpt-derailment-july-2014/F107_1218.jpg.html"><img src="http://railgallery.wongm.com/cache/xpt-derailment-july-2014/F107_1218_500.jpg" alt="XPT derailed outside Southern Cross - July 11, 2014" /></a>

I use WordPress to host my various blog sites, and it has a feature called "<a href="https://make.wordpress.org/support/user-manual/content/posts/schedule-a-post/" target="_blank">scheduled posts</a>" - set the time you want the post to go online, and in theory they'll magically appear in the future, without any manual intervention.

For this magic to happen, WordPress has to regularly check what time it is, check if any posts are due to be published, and if so, publish them - a process that is triggered in two different ways:
<ul>
	<li>run the check every time someone visits the site, or</li>
	<li>run the check based on a <a href="http://en.wikipedia.org/wiki/Cron" target="_blank">cron job</a> (scheduled task)</li>
</ul>
The <a href="https://stevengliebe.com/2013/11/18/using-real-wordpress-cron-job-increased-reliability/" target="_blank">first option is unreliable</a> because it delays page load times, and you can't count on people visiting a low traffic web site, so the second option is what I put in place when <a href="https://wongm.com/2014/07/rebuilding-websites/" target="_blank">setting up my server.</a>

I first encountered troubles with my scheduled posts in early April.
<blockquote class="twitter-tweet" lang="en" data-conversation="none">
<p dir="ltr" lang="en">Why has my blog robot been broken all week?</p>
— Marcus Wong (@aussiewongm) <a href="https://twitter.com/aussiewongm/status/583522182728257540">April 2, 2015</a></blockquote>
<script src="//platform.twitter.com/widgets.js" async="" charset="utf-8"></script>

My initial theory was that a recently installed WordPress plugin was to blame, running at the same time as the scheduled post logic and slowing it down.
<blockquote class="twitter-tweet" lang="en">
<p dir="ltr" lang="en">Looks like my blog robot is working again (I think the 'Broken Link Checker' plugin was making the WordPress cron page time out)</p>
— Marcus Wong (@aussiewongm) <a href="https://twitter.com/aussiewongm/status/586631369062662144">April 10, 2015</a></blockquote>
I removed the plugin, and scheduled posts on this site started to work again - I thought it was all fixed.

However, a few weeks later I discovered that new entries for <a href="http://www.checkerboardhill.com/" target="_blank">my Hong Kong blog</a> were missing in action.
<blockquote class="twitter-tweet" lang="en">
<p dir="ltr" lang="en">Turns out my blog robot still isn't working right, as it missed a post from April 8th. :-(</p>
— Marcus Wong (@aussiewongm) <a href="https://twitter.com/aussiewongm/status/587820152898461697">April 14, 2015</a></blockquote>
I took a look the the config for my cron job, and it seemed to be correct.
{% highlight bash %}
*/2 * * * * curl http://example.com/wp-cron.php &gt; /dev/null 2&gt;&amp;1{% endhighlight %}
I hit the URL featured in the command, and it triggered the publication of a new blog post - so everything good on that front!

I then dug a bit deeper, and ran the curl command directly on my server.
{% highlight bash %}
user@server:~$ curl http://example.com/wp-cron.php
&lt;!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN"&gt;
&lt;html&gt;&lt;head&gt;
&lt;title&gt;301 Moved Permanently&lt;/title&gt;
&lt;/head&gt;&lt;body&gt;
&lt;h1&gt;Moved Permanently&lt;/h1&gt;
&lt;p&gt;The document has moved 
&lt;a href="http://www.example.com/wp-cron.php"&gt;here&lt;/a&gt;.
&lt;/p&gt;
&lt;hr&gt;
&lt;address&gt;Apache Server at example.com Port 80&lt;/address&gt;
&lt;/body&gt;&lt;/html&gt;{% endhighlight %}
Bingo - I had found my problem!

Turns out I had previous added a <a href="http://stackoverflow.com/questions/234723/generic-htaccess-redirect-www-to-non-www" target="_blank">non-www to www redirect</a> for the website in question via a new .htaccess rule - and by default <a href="http://stackoverflow.com/questions/20905210/curl-302-redirect-not-working-command-line" target="_blank">curl doesn't follow HTTP redirects</a>.

The end result was my cron job hitting a URL, finding a redirect but not following it, resulting in the PHP code never being executed, and my future dated blog posts laying in limbo.

my fix was simple - update my cron job to hit the www. version of the URL, and since then, my future dated blog posts have all appeared on the days they were supposed to.

<strong>About the lead photo</strong>

The train in the lead photo is the Melbourne-Sydney XPT - <a href="http://www.atsb.gov.au/publications/investigation_reports/2014/rair/ro-2014-013.aspx" target="_blank">on 11 July 2014 it derailed</a> near North Melbourne Station due to a brand new but poorly designed turnout.
