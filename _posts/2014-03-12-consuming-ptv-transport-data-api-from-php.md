---
layout: post
title: Testing the PTV transport data API using PHP
date: 2014-03-12 20:49
author: wongm
comments: true
tags: [Melbourne, PHP, programming, Public Transport Victoria, real time train information, Technology, train, trams, transport]
---
Last week on March 7 Public Transport Victoria <a href="http://techgeek.com.au/2014/03/07/public-transport-victoria-finally-releases-timetable-data-public/" target="_blank">finally released</a> something many Melbourne coders and app developers have been waiting for - a public API to query their timetable data.

<a href="http://railgallery.wongm.com/southern-cross-station/F100_8126.jpg.html"><img src="http://railgallery.wongm.com/cache/southern-cross-station/F100_8126_500.jpg" alt="Passengers depart the train on platform 10" /></a>

Unfortunately the <a href="http://www.theage.com.au/technology/technology-news/melbourne-public-transport-tram-train-bus-users-stranded-with-unusable-data-20140310-34hxv.html" target="_blank">new API isn't perfect</a> - it doesn't meet the Google-defined <a href="http://en.wikipedia.org/wiki/General_Transit_Feed_Specification" target="_blank">GTFS standard</a> that many existing apps support, and no real-time arrival data is available - for trams you have to turn to TramTracker, and for trains and buses you're on your own.

Despite the above limitations, I jumped into playing around with the API. The full set of documentation can be found at <a href="https://www.data.vic.gov.au/raw_data/ptv-timetable-api/6056" target="_blank">www.data.vic.gov.au</a>, and after I made my API key request on the evening of March 6, I received a reply yesterday - March 11.

<a href="https://wongm.com/wp-content/uploads/2014/03/PTV-API-key-received.jpg"><img src="https://wongm.com/wp-content/uploads/2014/03/PTV-API-key-received-500x339.jpg" alt="API key received from PTV" width="500" height="339" class="alignnone size-medium wp-image-4504" /></a>

With my developer ID and security key received, the first thing to try out is the 'healthcheck' endpoint. Each API request needs to be digitally signed with a HMAC-SHA1 hash - this ensures that each request to the PTV servers is coming from a registered developer - and the healthcheck page allows a developer to test if their requests are being built up correctly.

For some people, working out the HMAC-SHA1 signature of each request is easy - the API documentation includes sample code in C#, Java and Objective-C to do just that. Unfortunately for me, my random hacking about is done in PHP, so I needed to roll my own version.

(jump to the end of my sample code)

My initial attempt generated something that looked like a HMAC-SHA1 signature, but it didn't pass validation - I screwed something up! An hour or so of bashing my head against the wall followed, as I played around with Base64 encoding and byte arrays. but it still wasn't working

I then tried a different tack - getting the sample C# code <a href="http://blogs.technet.com/b/stefan_gossner/archive/2010/05/07/using-csharp-c-code-in-powershell-scripts.aspx" target="_blank">working in Powershell</a>. For such an odd tactic it was simple enough to achieve, and I was able to get the code to run and generating a valid signature, which I used to make my first valid API hit. From there I was able to hack about my PHP code until it generated the same signature as the C# code generated - the bit I missed was the upper casing.

The end result from PTV - a small snippet of JSON.

<a href="https://wongm.com/wp-content/uploads/2014/03/PTV-working-API-response.jpg"><img src="https://wongm.com/wp-content/uploads/2014/03/PTV-working-API-response-500x284.jpg" alt="Valid response received from the PTV API" width="500" height="284" class="alignnone size-medium wp-image-4505" /></a>

<strong>Sample code</strong>

Here is the method I wrote to take an API endpoint URL (for example "/v2/healthcheck") and build up a valid signed URL to request data from.

{% highlight php %}
function generateURLWithDevIDAndKey($apiEndpoint, $developerId, $key)
{
	// append developer ID to API endpoint URL
	if (strpos($apiEndpoint, '?') > 0)
	{
		$apiEndpoint .= "&";
	}
	else
	{
		$apiEndpoint .= "?";
	}
	$apiEndpoint .= "devid=" . $developerId;
	
	// hash the endpoint URL
	$signature = strtoupper(hash_hmac("sha1", $apiEndpoint, $key, false));
	
	// add API endpoint, base URL and signature together
	return "http://timetableapi.ptv.vic.gov.au" . $apiEndpoint . "&signature=" . $signature;
}
{% endhighlight %}

And here is a sample usage of the above method. (note the sample data for key and developer ID!)

{% highlight php %}
$key = "9c132d31-6a30-4cac-8d8b-8a1970834799"; // supplied by PTV
$developerId = 2; // supplied by PTV

$date = gmdate('Y-m-d\TH:i:s\Z');
$healthcheckEndpoint = "/v2/healthcheck?timestamp=" . $date;

$signedUrl = generateURLWithDevIDAndKey($healthcheckEndpoint, $developerId, $key);
{% endhighlight %}

Now all I need to do is build the wonderful app I have in mind...

<strong>Code in GitHub</strong>

You can find the above sample code on GitHub at <a href="https://github.com/wongm/ptv-api-php-test-harness" target="_blank">https://github.com/wongm/ptv-api-php-test-harness</a> - all you need to do is update the $key and $developerId values with your own credentials, and it should work on your own server.

<strong>Footnote</strong>

It looks like plenty of other people are interested in the API - here is a <a href="http://www.data.vic.gov.au/blog/transport-ptv-timetable-api-release-sets-new-high/6067" target="_blank">media release from Data.Vic</a>, who host it on behalf of PTV.

<blockquote>The recent release of the PTV Timetable API has, not surprisingly, created the highest level of interest and activity for a single data release on Data.Vic.

In the 7 days since the release of the API data record we have seen:

- Over 3500 data record page views
- More than 700 downloads of the API document, and
- Close to 100 API key requests</blockquote>
