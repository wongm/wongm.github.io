---
layout: post
title: Debugging why random websites fail to load behind a Belkin router
author: wongm
comments: true
tags: [networking, ADSL, routers, Belkin, stateful packet inspection, firewalls]
excerpt_separator: <!--end_excerpt-->
---

For a few months now there have been a random selection of websites that fail to load on my home ADSL connection, but work fine if I connect via VPN or a 4G connection. This is the story of how I debugged it.

<!--end_excerpt-->

The first website I had trouble loading was https://vicsig.net/ - a websites about trains in Victoria, Australia. On the initial page load everything would work fine, but subsequent page loads would time out, continuing to fail any time I tried to refresh, until they eventually came good again.

One of the broken websites was https://www.thingiverse.com/ - an online collection of digital models for 3D printing. However for me website refused work work at all - a selection of web browsers on my Windows 10 desktop as well my Android phone connected via WiFi gave me a `ERR_CONNECTION_TIMED_OUT` error message, but I could load it via the same devices via VPN, or a 4G connection.

My first attempt at fixing the problem was switching DNS providers - both [Google 8.8.8.8](https://developers.google.com/speed/public-dns)  and [Cloudflare 1.1.1.1](https://blog.cloudflare.com/announcing-1111/) - but nothing. Pings still timed out.

	C:\Users\Marcus>ping thingiverse.com

	Pinging thingiverse.com [35.227.80.0] with 32 bytes of data:
	Request timed out.
	Request timed out.
	Request timed out.
	Request timed out.
	
	Ping statistics for 35.227.80.0:
    Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

As did traceroute.

	C:\Users\Marcus>tracert thingiverse.com
	
	Tracing route to thingiverse.com [35.227.80.0]
	over a maximum of 30 hops:
	
	1    <1 ms    <1 ms    <1 ms  . [192.168.1.1]
	2     *        *        *     Request timed out.
	3     *        *        *     Request timed out.
	4     *        *        *     Request timed out.
	5     *        *        *     Request timed out.
	6     *        *        *     Request timed out.
	7     *        *        *     Request timed out.

My next try was my ADSL modem - a F5D7633-4A from decades ago.

![Belkin router](https://upload.wikimedia.org/wikipedia/commons/thumb/b/b4/Belkin_Wireless_G_Router_F5D7231-4_Version_1000de-1121.jpg/500px-Belkin_Wireless_G_Router_F5D7231-4_Version_1000de-1121.jpg)

[© Raimond Spekking / CC BY-SA 4.0 (via Wikimedia Commons)](https://commons.wikimedia.org/wiki/File:Belkin_Wireless_G_Router_F5D7231-4_Version_1000de-1121.jpg) 

I discovered it was still on version 6.01.06 firmware from early 2006, so I looked for an update - an updated existed in version 6.01.08, dated late 2006. Not much of an improvement, but I installed it anyway.

While pottering about in the administrator web interface, I took a look at the security logs.

	04/07/2020  14:39:22 sending ACK to 192.168.1.2
	04/07/2020  14:39:03 **Smurf** 192.168.1.13->> 35.227.80.0, Type:8, Code:0 (from LAN Inbound)
	04/07/2020  14:39:02 sending ACK to 192.168.1.2
	04/07/2020  14:38:58 **Smurf** 192.168.1.13->> 35.227.80.0, Type:8, Code:0 (from LAN Inbound)
	04/07/2020  14:38:53 **Smurf** 192.168.1.13->> 35.227.80.0, Type:8, Code:0 (from LAN Inbound)
	04/07/2020  14:38:48 **Smurf** 192.168.1.13->> 35.227.80.0, Type:8, Code:0 (from LAN Inbound)
	04/07/2020  14:38:42 sending ACK to 192.168.1.2   

Interesting - the router was treating the ping requests as a [Smurf attack](https://en.wikipedia.org/wiki/Smurf_attack). But how to fix it?

On the Whirlpool forums I found [discussion on the topic](https://forums.whirlpool.net.au/archive/579647), but it was just dickheads making smurf jokes - useless!

However [this post by Abdul69](https://forums.whirlpool.net.au/thread/3nrx11r3#r27437468) on a different Whirlpool thread was more useful:

> Even though this might be too late for you I wanted to reply to this since I found a solution for this for the Belkin Router and people will no doubt run into this posting when searching for a solution, like I just did.
> 
> My problem was very similar; I had problem loading sites with lots of images that load fast such as google maps, google image search and so on. My wife also reported some ebay/clothing related web sites were pretty hit and miss.
> 
> I found after lots of searching that it could be related to the router firewall (may not be Belkin specific) blocking numerous outbound connections to the same site (in order to load those images). This had the effect for me of maps only partially loading, only the first page of searched images loading and so on. This problem has been a pain in my RS for quite some time now.
> 
> So the settings you need exist, but are not linked to by the standard pages. Once you log into your router home page go to:
> 
> http://10.21.119.1/firewall_spi_h.stm
> 
> Where 10.21.119.1 is the IP address of your LAN (typically it's 192.168.0.1 or something similar).
> 
> This page lets you adjust a wad of firewall specific settings. Change "Maximum incomplete TCP/UDP sessions number from same host" to 50 (from 10) and that will fix loading problems with Google, etc and no need to buy new router hardware.

Unfortunately for me, messing with the suggested settings didn't make a difference - but there was a wall of other settings for me to mess with.

![](/images/Belkin-ADSL-modem-router-hidden-firewall_spi_h.htm-settings.png)

And eventually I found success - disabling the "SPI and Anti-DoS firewall protection" option allowed me to successfully load all of my previously broken websites. 

**Footnote**

Here is a full list of [hidden system settings pages](https://kitz.co.uk/routers/belkin_commands.htm) for Belkin routers. 