---
layout: post
title: Who’s behind WordPress comment spam?
date: 2011-09-05 16:00
author: wongm
comments: true
tags: [advertising, blogging, interwebs, spam, Technology]
---
I've discussed <a href="https://wongm.com/2011/02/comment-spammer-wanting-help-fighting-comment-spamming/" target="_blank">comment spamming previously</a> - automatically generated comments that are posted to blogs in an attempt to game the Google search rankings.

The content of the comments is usually in the form of a generic compliment, in a naive attempt to stroke the ego of the blog owner and have the comment remain in place. The true "payload" of the spam is a link to a website selling products, with the spammer hoping that the link stays in place until Google finds it, which will enhance the search engine ranking of the linked website.

<a href="http://www.flickr.com/photos/legoblock/5467375049/" title="Comment spammer wanting help fighting comment spam by legoblock, on Flickr"><img src="http://farm6.static.flickr.com/5134/5467375049_5b17e1af49.jpg" width="500" height="77" alt="Comment spammer wanting help fighting comment spam"></a>

Most of the comment spam posted today is posted by automated software to thousands of websites, hence the content of each message is easily found on Google. Here is one example of the software in use: I'm not going to link to the dirtbags because it will improve their Google ranking, but this slimeball is currently the top Google hit for "<a href="http://www.google.com.au/search?q=wordpress+comment+spammer" target="_blank">wordpress comment spammer</a>". 

<a href="http://www.flickr.com/photos/legoblock/6111305756/" title="Biggest douchebag in the Wordpress community? by legoblock, on Flickr"><img src="http://farm7.static.flickr.com/6064/6111305756_5d8bf3a819.jpg" width="500" height="346" alt="Biggest douchebag in the Wordpress community?"></a>

The software is designed to allow the user to evade spam protection on blogs; with one of the available options enabling the use of proxy servers to distribute the source of the spam across multiple IP addresses, instead of the single address belonging to the spammer. The content itself is also randomised, the input being customisable list files that allow each comment to have a different author, email address and comment content. 

It also seems that some software is smart enough to vary the words used inside the sentences it generates, which is all well and good until it fails, such as the example comments below.

At first glance it appears that the spammer's software does not support the use of merge fields inside HTML &lt;b&gt; tags, as the bold text is a syntactical mess, while the normally formatted text below differs slightly between the two comments, showing the merging of words was successful.

<a href="http://www.flickr.com/photos/legoblock/6110453895/" title="Failed comment spam: merge fields have not been merged by legoblock, on Flickr"><img src="http://farm7.static.flickr.com/6064/6110453895_a1aeacd254.jpg" width="500" height="194" alt="Failed comment spam: merge fields have not been merged"></a>

A second way that spammers customise their automated comments is by including the article title inside the comment. In most cases this looks ridiculous as the software blindly parses the content of the HTML &lt;title&gt; tags of the page, which includes the website name: for example the title of this page is "What’s behind WordPress comment spam? | Waking up in Geelong".

However there is one even more obvious bug in a piece of spamming software, which is seen when URLs redirect to another page instead of displaying an article. An example of this failure can be seen in the comment posted on my article <a href="https://wongm.com/2011/03/mtr-east-rail-line-an-intro/" target="_blank">MTR East Rail Line: an intro</a>: originally located at this domain, it was moved to my Hong Kong-focused website Checkerboard Hill a few months ago. Due to the use of a HTTP 301 redirect I created to allow visitors to find the article at the new address, the software spammer has been tricked by the page title, believing the page is titled "301 Moved Permanently".

<a href="http://www.flickr.com/photos/legoblock/6110454113/" title="Failed comment spam: the page title is '301 Moved Permanently' by legoblock, on Flickr"><img src="http://farm7.static.flickr.com/6083/6110454113_5cddd69a07.jpg" width="500" height="76" alt="Failed comment spam: the page title is '301 Moved Permanently'"></a>

Despite these glitches, I would assume spammers aren't particularly concerned about the bugs in their software, because as long as their deluge of useless comments continues to flood the web and search engines continue to be tricked, they are achieving their goal.

<strong>Further reading</strong>
<ul>
	<li><a href="http://en.wikipedia.org/wiki/Spam_in_blogs" target="_blank">Spam in blogs</a>: an introduction to the comment spam at Wikipedia.</li>
</ul>
