---
layout: post
title: PTV website redesign, and a trail of broken links
date: 2013-04-24 07:30
author: wongm
comments: true
tags: [broken URLs, government, interwebs, mystery, Public Transport Victoria, Technology, train, transport, Victoria, wasted money]
---
On 21 April 2013 Public Transport Victoria launched their new public transport information website, <a href="http://ptv.vic.gov.au/news-and-events/news/public-transport-victoria-has-launched-its-new-website/" target="_blank">saying in their press release</a>:

<blockquote>The website has been redesigned to make it quicker and easier for you to access the information you need.</blockquote>

However they missed one big item in the move to the new website - redirecting each of their old URLs to the relevant page on the new website. I came across the issue while using Google to find information relating to concession tickets.

<a href="http://www.flickr.com/photos/legoblock/8675155636/" title="Google Search results pointing to broken pages on the PTV website by Marcus Wong from Geelong, on Flickr"><img src="http://farm9.staticflickr.com/8541/8675155636_66f5623155.jpg" width="500" height="407" alt="Google Search results pointing to broken pages on the PTV website"></a>

The first search result from Google was to this URL:

<blockquote>http://www.ptv.vic.gov.au/fares-tickets/concessions/</blockquote>

Which when clicked on lead me to a useless error page:

<a href="http://www.flickr.com/photos/legoblock/8674051925/" title="'Page not found' error for an old URL on the PTV website by Marcus Wong from Geelong, on Flickr"><img src="http://farm9.staticflickr.com/8117/8674051925_16ddc4bedd.jpg" width="500" height="407" alt="'Page not found' error for an old URL on the PTV website"></a>

After a bit of clicking around on the new PTV website, I finally found the page I was originally supposed to land on:

<a href="http://www.flickr.com/photos/legoblock/8675155530/" title="The page that should be found on the PTV website by Marcus Wong from Geelong, on Flickr"><img src="http://farm9.staticflickr.com/8540/8675155530_9d17d68ccf.jpg" width="500" height="407" alt="The page that should be found on the PTV website"></a>

Note the URL up in my browser bar:

<blockquote>http://www.ptv.vic.gov.au/tickets/concessions/</blockquote>

The only difference between the old and the new URLs is one word in the middle, where the 'fares-tickets' portion on the old has been replaced by 'tickets' on the new one. Nothing else!

I touched on the issue of website migrations leaving a legacy of broken links a few months ago, when I encountered a <a href="https://wongm.com/2013/03/fairfax-website-redesign-and-a-trail-of-broken-links/" target="_blank">similar issue with the Fairfax ‘Metro Media Publishing’ newspaper websites</a>.

In the case of Public Transport Victoria, they have done two things wrong:

<ul>
	<li>When they moved their website, PTV didn't bother to keep the URLs the same between the new and the old systems.</li>
	<li>If keeping the old URLs was not possible, then PTV should have set up redirects to the new URL, to ensure any links already out on the internet continue to work after the move was complete.</li>
</ul>

So when will all of the broken links disappear from Google Search? The 'page not found' page on the PTV website returns a correct <a href="http://en.wikipedia.org/wiki/HTTP_404" target="_blank">'HTTP 404' status code</a>, so in time Google will purge the bad links from their database, and replace them with the correct links when they next crawl the website.

<a href="http://www.flickr.com/photos/legoblock/8674105919/" title="'HTTP 404' response from the redesigned PTV website by Marcus Wong from Geelong, on Flickr"><img src="http://farm9.staticflickr.com/8534/8674105919_327770d98d.jpg" width="500" height="407" alt="'HTTP 404' response from the redesigned PTV website"></a>

All this messing around, all because someone decided to change 'fares-tickets' to just 'tickets'!
