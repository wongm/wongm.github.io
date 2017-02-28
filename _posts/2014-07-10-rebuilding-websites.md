---
layout: post
title: Rebuilding all of my websites
date: 2014-07-10 07:30
author: wongm
comments: true
tags: [blogging, interwebs, PHP, Technology, web hosting]
---
I've had quite busy recently - on Thursday last week I discovered all of my web sites were offline, which resulted in me moving to a new hosting provider, and rebuilding every bit of content. So how did I do it?

<strong>Going offline</strong>

I first realised something was wrong when I discovered all of my web sites displaying the following ominous error message:

<a href="https://wongm.com/wp-content/uploads/2014/07/cpanel-message-website-suspended.png"><img src="https://wongm.com/wp-content/uploads/2014/07/cpanel-message-website-suspended-500x322.png" alt=" &#039;Website Suspended&#039; message from cPanel" width="500" height="322" class="alignnone size-medium wp-image-4881" /></a>

I checked my email, and I couldn't find any notification from my hosting provider that my account was suspended - a pretty shit job from them!

However, I wasn't exactly surprised, as over the past few years I've been receiving these automated emails from their system:

<blockquote>Your hosting account with username: [XYZ] has over the last few days averaged CPU usage that is in excess of your account allocation.

This could be caused by a number of factors, but is most likely to be due to a misconfigured installation of a 3rd party script, or by having too many features, modules or plugins enabled on your web site.

If you simply have a very busy or popular web site, you may need to upgrade your account which will give you a higher CPU allocation. Please contact our support team if you need help with this.

Until your usage average drops back below your CPU quota, your account will be throttled by our CPU monitoring software. If your account continues to use more CPU than what it is entitled to, you risk having your account suspended.
</blockquote>

All up I was running about a dozen different web sites from my single <a href="http://en.wikipedia.org/wiki/Shared_web_hosting_service" target="_blank">shared web hosting</a> account, and over the years I've have had to increase the amount of resources available to my account to deal with the increasing load.

Eventually I ended up on a '5 site' package from my hosting provider, which they were charge me almost $300 a year to provide - a steep price, but I was too lazy to move everything to a new web host, so I just kept on paying it.

Having all of my sites go offline was enough of a push for me to move somewhere new!

<strong>What needed to be moved</strong>

All up my online presence consisted of a dozen different sites spread across a handful of domain names, running a mix of open source code and code I had written myself. With my original web host inaccessible, I had to rebuild everything from backups.

You do have backups don't you?

<a href="https://www.flickr.com/photos/legoblock/6260303872" title="Welcome to the western suburbs by Marcus Wong, on Flickr"><img src="https://farm7.staticflickr.com/6225/6260303872_42b2160f21.jpg" width="500" height="333" alt="Welcome to the western suburbs"></a>

<strong>The rebuild</strong>

I had been intending to move my web sites to a <a href="http://en.wikipedia.org/wiki/Virtual_private_server" target="_blank">virtual private server (VPS)</a> for a while, and having to rebuild everything from scratch was the perfect excuse to do so. 

I ended up deciding to go with <a href="https://www.digitalocean.com/" target="_blank">Digital Ocean</a> - they offer low-ish prices, servers in a number of different locations around the world, fast provisioning of new accounts, and an easy migration path to a faster server if you ever need it.

After signing up to their bottom end VPS (512 MB RAM and a single core) I was able to get cracking on the rebuild - they emailed me the <a href="http://en.wikipedia.org/wiki/Superuser" target="_blank">root password</a> a minute later and I was in!

As I had a bare server with nothing installed, a lot of housekeeping needed to be done before I could start restoring my sites:

<ul>
	<li>Swapping over the DNS records for my domains to my new host,</li>
	<li>Locking down access to the server,</li>
	<li>Setting up a swap file,</li>
	<li>Installing Apache, MySQL and PHP on the server,</li>
	<li>Creating virtual directories on the server for each separate web site,</li>
	<li>Creating user accounts and empty databases in MySQL</li>
</ul>

I've only ever played around with Linux a little, but after 30 minutes I had an empty page appearing for each of my domain names.

To get my content back online, thankfully I had the following backups available to me:

<ul>
	<li>I run three blogs on the open source WordPress software, so I could just install that from scratch to get a bare website back</li>
	<li>My main photo gallery on the open source ZenPhoto software, so that was another internet download</li>
	<li>Each blog and photo gallery uses a custom theme, of which I had backups on my local machine to re-upload</li>
	<li>I keep a mirror of my WordPress uploads on my local machine, so I just had to reupload those to make the images work again</li>
	<li>When I upload new photos to my gallery, I keep a copy of the web resolution version on my local machine which I was unable to reupload</li>
	<li>Every night I have a <a href="http://en.wikipedia.org/wiki/Cron" target="_blank">cron job</a> automatically emailing me a backup copy of my WordPress and ZenPhoto databases to me, so my blog posts and photo captions were safe</li>
	<li>Some of my custom web code is <a href="https://github.com/wongm/" target="_blank">available on GitHub</a>, so a simple <i>git pull</i> got those sites back online</li>
</ul>

Unfortunately I ran into a few issues when restoring my backups (doesn't everyone...):

<ul>
	<li>My WordPress backup was from the day before, and somebody has posted a new comment that day, so it was lost</li>
	<li>I had last mirrored my WordPress uploads about a week before the crash, so I was missing a handful of images</li>
	<li>The last few months of database backups for Rail Geelong were only 1kb in size - it appears the MySQL backup job on my old web host was defective</li>
	<li>Of the 32,000 photos I once had online, around 2,000 files were missing from the mirror I maintained on my local machine, and the rest of them were in a folder hierarchy that didn't match that of the database</li>
</ul>

I wasn't able to recover the lost comment, but I was able to chase up the missing WordPress uploads from other sources, and thankfully in the case of Rail Geelong my lack of regular updates meant that I only lost a few typographical corrections.

As for the 2,000 missing web resolution images, I still had the original high resolution images available on my computer, so my solution was incredible convoluted:

<ul>
	<li>Move all of the images from the mirror in a single folder</li>
	<li>Use SQL to generate a batch file to create the required folder structure</li>
	<li>Use more SQL to generate a second batch file, this time to move images into the correct place in the older structure</li>
	<li>Run a diff between the images that exist, and those that do not</li>
	<li>Track down the 2,000 missing images in my collection of high resolution images, and create a web resolution version in the required location</li>
</ul>

Three hours after I started, I had my first win.

<blockquote class="twitter-tweet" lang="en"><p>And my blogs are back! <a href="https://twitter.com/hashtag/WebsiteRebuildSpeedrun?src=hash">#WebsiteRebuildSpeedrun</a> <a href="http://t.co/UQ518Muwn2">http://t.co/UQ518Muwn2</a> <a href="http://t.co/iDiKdh8yMR">http://t.co/iDiKdh8yMR</a> <a href="http://t.co/SZVMGKGQNf">http://t.co/SZVMGKGQNf</a></p>&mdash; Marcus Wong (@aussiewongm) <a href="https://twitter.com/aussiewongm/statuses/484637468281425920">July 3, 2014</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Unfortunately I found a number of niggling issues throughout the night.

<blockquote class="twitter-tweet" lang="en"><p>Now bashing my head against the wall trying to get mod_rewrite working <a href="https://twitter.com/hashtag/WebsiteRebuildSpeedrun?src=hash">#WebsiteRebuildSpeedrun</a></p>&mdash; Marcus Wong (@aussiewongm) <a href="https://twitter.com/aussiewongm/statuses/484676482195652608">July 3, 2014</a></blockquote>

<blockquote class="twitter-tweet" lang="en"><p>Protip: page redirect rules won&#39;t work if your .htaccess file is empty <a href="https://twitter.com/hashtag/WebsiteRebuildSpeedrun?src=hash">#WebsiteRebuildSpeedrun</a></p>&mdash; Marcus Wong (@aussiewongm) <a href="https://twitter.com/aussiewongm/statuses/484678122194075648">July 3, 2014</a></blockquote>

<blockquote class="twitter-tweet" lang="en"><p>Failure to install cURL prevented Wordpress from sending comment notification emails <a href="https://twitter.com/hashtag/WebsiteRebuildSpeedrun?src=hash">#WebsiteRebuildSpeedrun</a></p>&mdash; Marcus Wong (@aussiewongm) <a href="https://twitter.com/aussiewongm/statuses/484854790828986368">July 4, 2014</a></blockquote>

By 2am I was seven hours in, and had managed to get another domain back online.

<blockquote class="twitter-tweet" lang="en"><p>Now my horrible kludge of custom code is back online: <a href="http://t.co/uxJcmPh7V6">http://t.co/uxJcmPh7V6</a> <a href="https://twitter.com/hashtag/WebsiteRebuildSpeedrun?src=hash">#WebsiteRebuildSpeedrun</a> <a href="https://twitter.com/hashtag/allnighter?src=hash">#allnighter</a></p>&mdash; Marcus Wong (@aussiewongm) <a href="https://twitter.com/aussiewongm/statuses/484722741262561280">July 3, 2014</a></blockquote>

Eventually I called it quits at 4am, as I waited for my lethargic ADSL connection to push an elephant up a drinking straw.

<blockquote class="twitter-tweet" lang="en"><p>How long will it take to upload 5 GB of images? <a href="http://t.co/9j5DyxP6LZ">http://t.co/9j5DyxP6LZ</a> <a href="https://twitter.com/hashtag/WebsiteRebuildSpeedrun?src=hash">#WebsiteRebuildSpeedrun</a></p>&mdash; Marcus Wong (@aussiewongm) <a href="https://twitter.com/aussiewongm/statuses/484740654434500609">July 3, 2014</a></blockquote>

I spent the weekend out and about so didn't get much time to work on rebuilding my content - it wasn't until the fourth day after my sites went down that I started to track down the 2,000 missing images from my photo gallery.

Thankfully I got a lucky break - on Monday afternoon I somehow regained access to my old web host, so I was able to download all of my missing images, as well as export an up-to-date version of the Rail Geelong database.

After a lot more stuffing around with file permissions and monitoring of memory usage, by Tuesday night it seems that I had finally rebuilt everything and running somewhat reliably!

<strong>What's next</strong>

Plenty of people online seem to rave about replacing the Apache web server and standard PHP stack with <a href="http://reviewsignal.com/blog/2014/06/25/40-million-hits-a-day-on-wordpress-using-a-10-vps/" target="_blank">Nginx and PHP-FPM</a> to increase performance - it's something I'll have to try out when I get the time. However for the moment, at least I am back online!
