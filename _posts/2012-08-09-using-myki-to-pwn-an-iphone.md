---
layout: post
title: Using Myki to pwn an iPhone
date: 2012-08-09 07:20
author: wongm
comments: true
canonical: https://wongm.com/2012/08/using-myki-to-pwn-an-iphone/
tags: [hacking, Melbourne, Metro Trains Melbourne, Myki, social engineering, Technology, then and now, transport]
---
Yesterday morning the big Myki news story in the mainstream media was the revelation that Myki ticket machines spit out EFT receipts containing excessive amounts of personal details, even if the user doesn't ask for one, leaving people open to identity fraud if they don't collect their receipt.

<a href="http://railgallery.wongm.com/myki/E115_3453.jpg.html"><img src="http://railgallery.wongm.com/cache/myki/E115_3453_500.jpg" alt="Unwanted receipts build up in a Myki ticket machine" /></a>

The story broke in <em>The Age</em> article '<a href="http://www.theage.com.au/it-pro/security-it/myki-flaw-risks-credit-card-security-20120807-23s9f.html" target="_blank">Myki flaw risks credit card security</a>' by Adam Carey:

<blockquote>Passengers who decline a printed receipt after topping up at a vending machine with a credit or eftpos card are automatically issued one anyway, often unwittingly leaving behind a receipt that includes their full name, nine digits of their credit card and the card's expiry date. Passengers who accept a receipt are automatically issued two copies.</blockquote>

The issue isn't a new one: the 'feature' has been part of Myki since the machines were first rolled out, with the Transport Ticketing Authority being unwilling to fix the issue.

<a href="http://railgallery.wongm.com/myki/E114_9414.jpg.html"><img src="http://railgallery.wongm.com/cache/myki/E114_9414_500.jpg" alt="More abandoned Myki receipts, this time on top of the CVM" /></a>

Soon after reading the article in <em>The Age</em>, I did my usual rounds of the technology news sites, and came across a seeming unrelated article in <em>Wired</em> titled '<a href="http://www.wired.com/gadgetlab/2012/08/apple-amazon-mat-honan-hacking/" target="_blank">How Apple and Amazon Security Flaws Led to My Epic Hacking</a>'. Here reporter Mat Honan details how his entire digital life was destroyed when hackers gained access his Apple account using social engineering and a few key snippets of personal information.

<blockquote>It turns out, a billing address and the last four digits of a credit card number are the only two pieces of information anyone needs to get into your iCloud account. Once supplied, Apple will issue a temporary password, and that password grants access to iCloud.</blockquote>

Credit card numbers out in the open? Full names that could be tied to a physical location? I put two and two together pretty quickly, and it seems like the rest of Twitter did the same thing:

<blockquote class="twitter-tweet"><p>Now imagine <a href="https://twitter.com/search/?q=%23Myki"><s>#</s><b>Myki</b></a> receipt (with your full name, 9/16 credit card digits + expiry date) in the hands of these guys <a href="http://t.co/hps9rhfU" title="http://www.wired.com/gadgetlab/2012/08/apple-amazon-mat-honan-hacking/all/">wired.com/gadgetlab/2012…</a></p>&mdash; Daniel Bowen (@danielbowen) <a href="https://twitter.com/danielbowen/status/233046182186868737" data-datetime="2012-08-08T03:45:14+00:00">August 8, 2012</a></blockquote>
<script src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

I don't use EFT to top up my Myki and don't own an iPhone, but if you fall into either group, I hope these aren't your receipts littering Melbourne, or you might be next.

<a href="http://railgallery.wongm.com/myki/E114_8890.jpg.html"><img src="http://railgallery.wongm.com/cache/myki/E114_8890_500.jpg" alt="Abandoned Myki receipts at a tram stop" /></a>
