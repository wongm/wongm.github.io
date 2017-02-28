---
layout: post
title: Making TramTracker work on 'Dumbphones'
date: 2014-03-18 07:30
author: wongm
comments: true
tags: [Melbourne, programming, real time train information, Technology, trams, TramTracker, transport, Yarra Trams]
---
Over the years I've been using TramTracker on my ancient Nokia mobile phone, until December 2013 when Yarra Trams <a href="https://wongm.com/2013/12/yarra-trams-tramtracker-website-broken/" target="_blank">updated the web version of the service</a>, and rendered it unusable on older devices. 

<a href="http://railgallery.wongm.com/tram-bits/F102_4487.jpg.html"><img src="http://railgallery.wongm.com/cache/tram-bits/F102_4487_500.jpg" alt="Detail of D2.5004 advertising 'TramTracker' " /></a>

Unlike other public transport operators, Yarra Trams actually took my feedback on board and released an updated web version of TramTracker a month later, but unfortunately for me, they were not enough to make it work with my ancient WAP only phone. 

As a result, I took inspiration from my <a href="https://wongm.com/2014/03/consuming-ptv-transport-data-api-from-php/" target="_blank">recent experiments with PTV's new API</a>, and came up with a simple website to give access to TramTracker data.

Called 'Jake Junior: TramTracker for Dumbphones', you can give it a go at <a href="http://jakejunior.wongm.com/" target="_blank">http://jakejunior.wongm.com/</a>.

<img src="https://wongm.com/wp-content/uploads/2014/03/jake-junior-tramtracker-on-nokia-dumbphone.jpg" alt="Using &#039;Jake Junior&#039; to access TramTracker on a dumbphone" width="500" height="333" class="alignnone size-full wp-image-4518" />

<strong>Some background</strong>

TramTracker first appeared in Melbourne <a href="http://melbourneontransit.blogspot.com.au/2006/12/testing-tramtracker-you-might-have.html" target="_blank">way back in December 2006</a>, with only two ways of accessing tram arrival information: voice or SMS reply. However usage of the service didn't take off until 2009, when an <a href="http://infodesign.com.au/uxpod/tramtracker/" target="_blank">incredibly well designed iPhone app</a> was released, providing a much more convenient way to find tram stops and arrival times.

Since then additional methods of access have been provided - a mobile friendly website for non-iPhone devices, a 'webPID' for desktop computers <a href="http://www.yarratrams.com.au/media-centre/news/articles/2009/tramtracker%C2%AE-webpid/" target="_blank">in 2009</a>, and an Android app <a href="http://www.yarratrams.com.au/media-centre/news/articles/2012/tramtracker-now-on-android/" target="_blank">in October 2012</a>. Third-party developers have also built their own apps for the TramTracker service - the most interesting one I've found so far is <a href="http://www.mypebblefaces.com/apps/20239/10021/" target="_blank">TramFinder for Pebble watches</a>! 

One thing these third-party apps have in common is their source of tram arrival data - a public API found at <a href="http://ws.tramtracker.com.au/pidsservice/pids.asmx" target="_blank">http://ws.tramtracker.com.au/pidsservice/pids.asmx</a>. This API isn't the easiest thing to play around with because it follows the <a href="https://en.wikipedia.org/wiki/SOAP" target="_blank">SOAP</a> specification for webservices - a horrible tarball of <a href="https://en.wikipedia.org/wiki/XML" target="_blank">XML</a> requests being sent back and forth between devices.

Messing around with SOAP isn't my idea of fun, so I put building my own 'simple' version of TramTracker on the backburner, until I discovered that the new version of the TramTracker website was actually using <a href="https://en.wikipedia.org/wiki/Representational state transfer" target="_blank">RESTful</a> calls to a backend service that was returning <a href="https://en.wikipedia.org/wiki/JSON" target="_blank">JSON</a>

I was unsure if that version of the API was actually designed for the public to consume, so I posed the question on Twitter, only for the original developer of the TramTracker iPhone app to give me an answer:

<blockquote class="twitter-tweet" lang="en"><p><a href="https://twitter.com/aussiewongm">@aussiewongm</a> <a href="https://twitter.com/petehare">@petehare</a> they’re on the same infrastructure so it shouldn’t matter so much.</p>&mdash; Rob Amos (@bok_) <a href="https://twitter.com/bok_/statuses/444321108943069184">March 14, 2014</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

With that in mind, I spent a few hours cobbling together some PHP code, with 'Jake Junior' being the end result.

<strong>How it works</strong>

Internally 'Jake Junior' is just a single PHP file - you can find the source code at <a href="https://github.com/wongm/jake-junior/blob/master/index.php" target="_blank">https://github.com/wongm/jake-junior</a>.

All of the HTML is incredibly simple and all of the heavy lifting is done server side - no javascript or images are required. The home page presents a simple TramTracker ID input box and a 'Go button, which then loads the next three trams for each route passing through the given stop. Where multiple routes pass through a stop, links are provided to allow you to filter by route. Bookmarking a stop is also simple, as the URLs always lead you back to your intended tram stop and route combination.

Stop information and tram arrival times are retrieved using two hits to the TramTracker API: unfortunately it doesn't combine the two types of data into a single method.

To get the name and direction of a tram stop, I used to the 'GetStopInformation' endpoint:

<a href="http://tramtracker.com/Controllers/GetStopInformation.ashx?s=1234" target="_blank">http://tramtracker.com/Controllers/GetStopInformation.ashx?s=1234</a>

And for the arrival times, the 'GetNextPredictionsForStop' endpoint:
<a href="http://www.tramtracker.com/Controllers/GetNextPredictionsForStop.ashx?stopNo=1234&routeNo=0&isLowFloor=false" target="_blank">http://www.tramtracker.com/Controllers/GetNextPredictionsForStop.ashx?stopNo=1234&routeNo=0&isLowFloor=false</a>

<small>By the way - TramTracker ID 1234 is on route 1 in South Melbourne!</small>

Once I had the results of the two API calls as a JSON string, I was able to deserialise the data, do some simple maths to convert the JSON timestamp into a minutes until arrival figure, and then output it all as HTML.

You can play around with the end result at <a href="http://jakejunior.wongm.com" target="_blank">http://jakejunior.wongm.com</a>.

<strong>Footnote</strong>

If you are wondering about the 'Jake Junior' name - the official TramTracker mascot is <a href="http://www.theage.com.au/victoria/pet-project-was-just-a-cliche-idea-20110226-1b9b6.html" target="_blank">dog named Jake</a>.
