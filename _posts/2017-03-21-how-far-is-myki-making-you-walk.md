---
layout: post
title: "How far is Myki making you walk?"
author: wongm
comments: true
canonical: https://wongm.com/2017/03/how-far-is-myki-making-you-walk/
tags: [Google Maps, programming, PHP, PTV API, Public Transport Victoria, Myki]
---

If you want to catch a tram in Melbourne then you need a Myki, despite the fact you can't buy one or top it up onboard the tram. In 2017 <em>The Age</em> highlighted the difficulty this can pose for intending tram passengers, <a href="http://www.theage.com.au/victoria/melbournes-myki-retailers-where-is-the-nearest-place-to-top-up-my-myki-20170307-gus8x7.html" target="_blank">in an article on myki "dead zones"</a> - tram stops where the nearest place to top up your myki is at least a kilometre away. Coincidently I started work on an almost identical project years ago but never finished it, so what better time to polish it off?

<a href="https://wongm.com/wp-content/uploads/2017/03/how-far-is-myki-making-you-walk-screenshot.png"><img src="https://wongm.com/wp-content/uploads/2017/03/how-far-is-myki-making-you-walk-screenshot-500x463.png" alt="" width="500" height="463" class="alignnone size-medium wp-image-7646" /></a>

<strong>Using the Public Transport Victoria API</strong>

Back in March 2014 Public Transport Victoria finally opened up the application program interface (API) which powers their mobile apps, so I <a href="https://wongm.com/2014/03/consuming-ptv-transport-data-api-from-php/" target="_blank">decided to have a play around with it</a>.

With the mobile landscape already littered with hundreds of different trip planning apps, I decided to build something slightly different - something to point out how the lack of ticket purchase options onboard trams was wasting the time of the intending passengers.

The API allows programmers to access all kinds of data - tram routes and Myki retailers being two of them, so I built an app that caters for two use cases:

<ul>
	<li>you're at home, work, or a friend's house - and you've discovered that you don't have a Myki on hand. Where is the nearest place to buy a new one, and how far will this detour take compared to purchasing a ticket onboard the tram?</li>
	<li>you've just stepped onto a tram and discovered that you don't have any credit left on your Myki. How far will you have to walk to top up, and then where can you get back on your way?</li>
</ul>

The end result is '<a href="https://wongm.com/walki/" target="_blank">walki</a>' - a small app that works on any device with a web browser.

<a href="https://wongm.com/walki" target="_blank"><img src="https://wongm.com/wp-content/uploads/2017/03/how-far-is-myki-making-you-walk-homepage.png" alt="" width="481" height="779" class="alignnone size-full wp-image-7647" /></a>

The logic in the app is as follows:

<ol>
	<li>Show the user their current location,</li>
	<li>Calculate distance to nearest tram stop,</li>
	<li>Calculate distance to nearest Myki retailer,</li>
	<li>Calculate distance from Myki retailer back to nearest tram stop,</li>
	<li>Plot the walking routes on a map,</li>
	<li>Compare the distances for each,</li>
	<li>And finally, show the user much further they have to walk thanks to the lack of ticket sales onboard trams.</li>
</ol>

Simple?

<a href="https://wongm.com/wp-content/uploads/2017/03/how-far-is-myki-making-you-walk-screenshot.png"><img src="https://wongm.com/wp-content/uploads/2017/03/how-far-is-myki-making-you-walk-screenshot-500x463.png" alt="" width="500" height="463" class="alignnone size-medium wp-image-7646" /></a>

You can see it for yourself at <a href="https://wongm.com/walki/" target="_blank">https://wongm.com/walki/</a>, or <a href="https://wongm.com/walki/#examples" target="_blank">using these examples</a>.

<strong>Technology</strong>

The app itself isn't anything revolutionary from a technology standpoint.

In the backend I'm using boring old PHP to gather tram stop and myki retailer locations through calls to the PTV API, with the resulting data being mashed around in Javascript until they are drawn out on a pretty map.

The frontend code is static HTML files with a smattering of jQuery Mobile ('state of the art' for 2014? :-P) over the top, with the maps being drawn using the Google Maps JavaScript API v3.

The files are all hosted on my vanilla Apache web server, and you can <a href="https://github.com/wongm/walki" target="_blank">find the source code on GitHub</a>.