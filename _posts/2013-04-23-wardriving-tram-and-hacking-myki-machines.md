---
layout: post
title: The wardriving tram and ‘hacking’ Myki machines
date: 2013-04-23 07:30
author: wongm
comments: true
canonical: https://wongm.com/2013/04/wardriving-tram-and-hacking-myki-machines/
tags: [hacking, Melbourne, Myki, mystery, Technology, transport, wardriving, Yarra Trams]
---
As an eagle-eyed observer of Melbourne's Myki ticketing system, I have <a href="http://railgallery.wongm.com/myki/E114_4892.jpg.html" target="_blank">stumbled</a> <a href="http://railgallery.wongm.com/myki/E115_1419.jpg.html" target="_blank">across</a> <a href="http://railgallery.wongm.com/myki/E114_5901.jpg.html" target="_blank">many</a> <a href="http://railgallery.wongm.com/myki/E116_3767.jpg.html" target="_blank">different</a> <a href="http://railgallery.wongm.com/myki/E113_4749.jpg.html" target="_blank">error</a> <a href="http://railgallery.wongm.com/myki/E114_8800.jpg.html" target="_blank">messages</a> displayed on the Tram Driver Consoles located inside the cab of each of Melbourne's trams. But this message is a new one...

<a href="http://railgallery.wongm.com/myki/E121_7552.jpg.html"><img src="http://railgallery.wongm.com/cache/myki/E121_7552_500.jpg" alt="Playing around with WiFi SSID (network names) on a Tram Driver Console" /></a>

If you really squint, one of the lines on the display reads 'myki was p0wned'. So how did it get there?

<strong>Background</strong>

The story starts on my tram home from work, when I noticed the Tram Driver Console in the rear cab was stuck in a reboot loop. The first screen was a simple 'Launching application' message on the <a href="https://wongm.com/2012/06/myki-runs-on-top-of-windows-ce/" target="_blank">standard Windows CE desktop</a>.

<a href="http://railgallery.wongm.com/myki/E121_7410.jpg.html"><img src="http://railgallery.wongm.com/cache/myki/E121_7410_500.jpg" alt="Myki tram driver console stuck in a reboot loop" /></a>

Next was a Myki splash screen, and the message 'Install Manager Loading. Please Wait'

<a href="http://railgallery.wongm.com/myki/E121_7414.jpg.html"><img src="http://railgallery.wongm.com/cache/myki/E121_7414_500.jpg" alt="Myki 'Melbourne Install Manager' starting up" /></a>

After a moment the splash screen disappeared, leaving the console back at the Windows CE desktop, and a wireless network configuration dialog.

<a href="http://railgallery.wongm.com/myki/E121_7419.jpg.html"><img src="http://railgallery.wongm.com/cache/myki/E121_7419_500.jpg" alt="Tram Driver Console on a 'wardriving' mission, displaying the names of all nearby WiFi networks" /></a>

And so the cycle repeated. As I continued on my trip home, I realised that the list of networks displayed onscreen changed, as the WiFi signals dropped in and out of range of the tram - it was on a <a href="http://en.wikipedia.org/wiki/Wardriving" target="_blank">wardriving</a> mission!

<a href="http://railgallery.wongm.com/myki/E121_7539.jpg.html"><img src="http://railgallery.wongm.com/cache/myki/E121_7539_500.jpg" alt="A short distance down the tracks, and a new set of WiFi networks" /></a>

I then realised I could have a little fun with Myki screen, setting up my phone as a wireless hotspot with a smart alec SSID (network name), and wait for the rebooting console to pick it up.

'myki was p0wned' was an obvious one.

<a href="http://railgallery.wongm.com/myki/E121_7547.jpg.html"><img src="http://railgallery.wongm.com/cache/myki/E121_7547_500.jpg" alt="Wireless network called 'myki was p0wned' displayed on a Tram Driver Console" /></a>

Getting my name up there with 'wongm was here' was another.

<a href="http://railgallery.wongm.com/myki/E121_7570.jpg.html"><img src="http://railgallery.wongm.com/cache/myki/E121_7570_500.jpg" alt="Wireless network called 'wongm was here' displayed on a Tram Driver Console" /></a>

And 'Penis!' appealed to the immature part of me.

<a href="http://railgallery.wongm.com/myki/E121_7586.jpg.html"><img src="http://railgallery.wongm.com/cache/myki/E121_7586_500.jpg" alt="Wireless network called 'penis!' displayed on a Tram Driver Console" /></a>

I was the only one there to notice it, but it was a giggle while it lasted.

<strong>So how bad is this flaw?</strong>

For a start, the reboot loop I saw isn't an everyday occurrence - this is the first time I've seen one just like it. The cause was hidden in an error message that flashed up when the 'Melbourne Installation Manager' program was starting up. After many attempts, I managed to snap a photo while it flashed up on screen for a fraction of a second.

<a href="http://railgallery.wongm.com/myki/E121_7602.jpg.html"><img src="http://railgallery.wongm.com/cache/myki/E121_7602_500.jpg" alt="The reason the Tram Driver Console was rebooting: an incorrect SD card was installed" /></a>

If you can't read it, the details are:

<blockquote>Configuring this device to the new SD card
An SD card from another device has been detected</blockquote>

The above suggests a few things:

<ul>
	<li>The startup process for the Tram Driver Console goes: Boot screen > Windows CE desktop > Myki 'Melbourne Installation Manager' program</li>
	<li>The device has a SD card slot so that software updates can be carried out to the console.</li>
	<li>The Myki software has some form of security check when reading from the SD card, ensuring that only data from authorised media is loaded.</li>
</ul>

From that, it seems that at least some security has been baked into the update process: while the Tram Driver Console is locked up inside the cab, even if one gained physical access to the device in order to insert an external storage device, the software won't update itself from anything you give it - some form of validation is occurring.

However, the device itself isn't locked down enough to avoid showing the Windows CE desktop: once someone had physical access to the machine, it seems that loading and executing an arbitrary piece of software on the console might be possible before the 'Melbourne Installation Manager' program starts up. Tram driver playing solitaire anyone?

As for WiFi access being enabled - why is it even needed for it on a tram travelling the streets of Melbourne? The reason lies in the way Myki is architected: <a href="https://wongm.com/2013/02/what-data-does-myki-save-on-your-card/" target="_blank">the card is the source of truth of all data</a>, with the backend systems needing to kept in sync on a regular basis. In the case of railway stations the list of online topups and blocked cards can be updated in real time via a hardwired network connection, but for moving vehicles likes trams they need some other way.

Back in the early 2000s when Myki was being scoped, ubiquitous data connections through the 3G network were still new, so instead it was decided to install a <a href="http://www.hawthorntramdepot.org.au/papers/fare/fare6.htm" target="_blank">WiFi connection covering each bus and tram depot</a>, which the Myki devices automatically connect to when they head home each night. This intermittent connection also explains why <a href="http://www.danielbowen.com/2012/11/26/myki-topup-instant/" target="_blank">Myki online topup doesn't happen instantly</a> - the request to topup your card needs to reach the reader on the tram before it can be applied.

<strong>Look out for hackers?</strong>

So is this a hack, or just a mere intellectual curiosity? Definitely the latter - every day millions of people turn on their WiFi enabled smartphones and laptops looking for wireless networks to connect to, and malicious wireless network names aren't crashing their devices - using them to <a href="http://www.bbc.co.uk/news/magazine-19760006" target="_blank">send passive-aggressive notes to neighbours</a> seems to be as bad as it gets. If you did the same thing to a friend's mobile phone you aren't even a <a href="http://en.wikipedia.org/wiki/Script_kiddie" target="_blank">script kiddie</a>, let along a hacker.

<strong>Footnote</strong>

A search of the <a href="http://www.google.com.au/search?q=malicious+SSID+site:cvedetails.com" target="_blank">Common Vulnerabilities and Exposures database</a> shows that broadcasting a maliciously named SSID over the air isn't a common attack vector, with <a href="http://www.google.com.au/search?q=malicious+SSID+site:technet.microsoft.com" target="_blank">Microsoft TechNet</a> also draws a blank.

Also, I spent a moment investigating the significance of the 'CW981' title of the wireless network dialog box. The first relevant hit on Google was a <a href="http://be300.org/forums/index.php?action=printpage;topic=1516.0" target="_blank">forum thread where someone was trying to get a wireless network card working</a> - where the 'CW981' is an internal code inside the Windows Registry. The device in question was a <a href="http://www.amazon.com/Netgear-802-11b-Wireless-Compact-Adapter/sim/B00006B9HN/2" target="_blank">NETGEAR MA701 Wireless CF Card</a>, which was designed for Windows CE devices. Possibly the Tram Driver Console uses one of these to access the wireless network?
