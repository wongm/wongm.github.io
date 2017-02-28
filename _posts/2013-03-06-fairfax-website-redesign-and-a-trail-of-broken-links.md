---
layout: post
title: Fairfax website redesign, and a trail of broken links
date: 2013-03-06 07:30
author: wongm
comments: true
tags: [broken URLs, Fairfax, interwebs, Moonee Valley Weekly, mystery, newspapers, research, Technology]
---
The other night I was doing some research for an upcoming blog post, and via Google I found some useful newspaper articles published in the '<em>Moonee Valley Weekly</em>':

<a href="http://www.flickr.com/photos/legoblock/8527900152/" title="Broken search results from the 'Moonee Valley Weekly' website by Marcus Wong from Geelong, on Flickr"><img src="http://farm9.staticflickr.com/8365/8527900152_af2d0ccf7e.jpg" width="500" height="396" alt="Broken search results from the 'Moonee Valley Weekly' website"></a>

But when I clicked through to each of the links, I ended up at this page:

<a href="http://www.flickr.com/photos/legoblock/8527914840/" title="Missing article at the 'Moonee Valley Weekly' website by Marcus Wong from Geelong, on Flickr"><img src="http://farm9.staticflickr.com/8369/8527914840_3d1432369e.jpg" width="500" height="368" alt="Missing article at the 'Moonee Valley Weekly' website"></a>

On seeing the error I put on my web developer hat, turned on the Network panel in Google Chrome's developer tools, and found that in addition to the <em>Sorry, but this page is temporarily unavailable</em> message on the page, the web server was also telling me why it was broken - a <a href="http://en.wikipedia.org/wiki/List_of_HTTP_status_codes#5xx_Server_Error" target="_blank">HTTP 503 'Service Unavailable'</a> message. Internet standards define this message as the server being currently unavailable because it is overloaded or down for maintenance, and that it is usually a temporary state.

Unfortunately for me the above error wasn't transient - I have been hitting my head up against the same brick wall when trying to read other online articles from a number of other local newspaper. The common factor - Fairfax. The <em>Moonee Valley Weekly</em> is one of a few dozen local newspapers that form part of the Fairfax 'Metro Media Publishing' group, and all of the broken newspaper websites belonged to them.

The root cause of the missing webpages appears to be their website revamp that was launched sometime in August 2012. Here is the homepage of the old version:

<a href="http://www.flickr.com/photos/legoblock/8526785397/" title="Older version of the 'Moonee Valley Weekly' homepage by Marcus Wong from Geelong, on Flickr"><img src="http://farm9.staticflickr.com/8526/8526785397_dd628572eb.jpg" width="500" height="333" alt="Older version of the 'Moonee Valley Weekly' homepage"></a>

And the new homepage:

<a href="http://www.flickr.com/photos/legoblock/8526785275/" title="Current version of the 'Moonee Valley Weekly' homepage by Marcus Wong from Geelong, on Flickr"><img src="http://farm9.staticflickr.com/8096/8526785275_9befb79d83.jpg" width="500" height="333" alt="Current version of the 'Moonee Valley Weekly' homepage"></a>

URLs at the old website conformed to this format:

<blockquote>http://www.mooneevalleyweekly.com.au/news/local/news/general/moonee-ponds-shoppers-stung-in-parking-fines/<strong>2638396</strong>.aspx</blockquote>

While the new URLs are much shorter:

<blockquote>http://www.mooneevalleyweekly.com.au/story/<strong>291023</strong>/moonee-ponds-shoppers-stung-in-parking-fines/</blockquote>

Note that both of the above URLs include the same random number inside them - 291023 - and that if you enter either into your web browser, you will end up at the same newspaper article - motorists in Moonee Ponds copping parking fines down at their local shopping centre.

If we break down the URLs, with my nerd hat back on I see two things - the 'random' number is actually the unique identifier for the newspaper article in the newspaper database, and the rest of the link is just pretty text that makes the URL nicer for humans to understand.

So what has Fairfax gone and done when creating their new website? As well as updating the styling of the website, it also appears they have migrated articles from their old website into a new <a href="http://en.wikipedia.org/wiki/Content_management_system" target="_blank">content management system</a>, and this new system creates links to pages in a new manner. Thankfully Fairfax asked their web developers to add a layer of backwards compatibility so that URLs in the old format are translated to those used in new website, and so ensured that anyone clicking on old links end up at the page they were looking for.

But their mistake? Fairfax missed migrating a big slab of old articles into their new website, but the backwards compatibility logic still attempts to look them up in the new database but fails, resulting in the server crashing, and a HTTP 503 'Service Unavailable' error being presented to the user.

A bonus gotcha arises due to the HTTP 503 error - Google interprets this as a '<a href="http://www.askapache.com/seo/503-service-temporarily-unavailable.html" target="_blank">things here are broken, but you can try again later and the website should be back</a>' status. This means that for every link that Google has to a Fairfax article that no longer exists, it will continue to be included in the search results - just waiting for the day when Fairfax either makes the article comes back to life, or kills it off for good.
