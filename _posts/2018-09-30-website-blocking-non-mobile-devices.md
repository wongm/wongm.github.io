---
layout: post
title: The mobile-only website that blocks desktop users
author: wongm
comments: true
tags: [mobile, websites, responsive-design]
---

Back when the internet was first created desktop computers were the only means of access, but with the advent of smart phones in the past decade, visitors are just as likely to be using a smaller screen. Responsive design means that web developers can create websites that work just as good on both.

But for a business I stumbled upon the other week, that won't do - they want to explicitly block people who aren't using a smart phone.

<img src="/images/mobile-detection---What-kind-of-muppet-presents-this-to-website-visitors.jpg" />

Anyone using a desktop computer is presented with this pigheaded message.

<blockquote>Don’t be a sheep!<br />
Goats run free.<br />
Try us on your mobile.<br /><br />

Sorry, we are mobile only.<br />
For the best experience, please visit [useless site] on your favourite handset.
</blockquote>

Inquisitive me wouldn't that for an answer, so I fired up the developer tools in my desktop browser, and lo and behold - the site loaded.

<img src="/images/mobile-detection---next-minute.jpg" />

I had a look at the source code, and immediately cringed - the entire site was being rendered client side by a Javascript.

<img src="/images/mobile-detection---Sniffing-the-source-code.jpg" />

How were they blocking desktop users - sniffing the user agent, perhaps?

But luckily I didn't need to dig into the Javascript mess to find the redirect logic - the 'piss off desktop users' page is just an overlay over the top, driven by [media queries](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries).

<img src="/images/mobile-detection---their-error-page-is-responsive.jpg" />

The breakpoint is set at 767 pixels - anything smaller is considered a mobile device, and worthy of viewing their insipid content.

<img src="/images/mobile-detection---largest-screen-you're-allowed-to-have.jpg" />

And anything bigger gets the banhammer.

**Footnote**

Australian media blog Mumbrella [explains the thinking](https://mumbrella.com.au/nova-launches-youth-website-goat-com-au-504103) behind the website owner's mobile-only push.

<blockquote>One of Goat’s other key differences is it is only available on mobile. Kane Reiken, Nova Entertainment’s digital commercial director, said the decision was really about “standing out in market” but that it also helps Nova be incredibly focused internally, on the mobile experience.<br /><br />

“That’s an industry first and it’s something no one else has really done,” he said.<br /><br />

“Mobile is obviously a big strategy as part of our overall company guidelines and ambitions and we want to make sure that really resonates through our entire product.<br /><br />

“You also see a lot of bad mobile advertising. There is minimal audience lost and we can really focus in on creating a product that works perfectly for mobile.”</blockquote>

Putting mobile first is one thing - but in my opinion if the only way you achieve it is by blocking desktop devices, then you're doing it wrong. 