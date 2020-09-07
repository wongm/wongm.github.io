---
layout: post
title: Debugging page crashes on my WordPress site
author: wongm
comments: true
tags: [plugins, WordPress, debugging, PHP]
excerpt_separator: <!--end_excerpt-->
---

I run multiple WordPress sites across a variety of different topics, so the other day I was rather annoyed when this generic error message popped up instead of a blog post.

	There has been a critical error on your website.
	
	Learn more about debugging in WordPress.

Since my WordPress sites are all self-hosted on a Ubuntu virtual private service that I manage myself, it was time to do some debugging.

<!--end_excerpt-->

First step, how often did the error occur? 

The first place I saw the crash was on a specific post, but the problem was also occurring on the home page, one but not all of the category pages, and confusingly - scattered through a separate WordPress site hosted on the same server.

Next - what has changed recently? 

I had recently modified how the 'related posts' are displayed on my blog, but the errors were also appearing on pages where this logic is not called - maybe somewhere else? I had just updated to WordPress version 5.5.1 so maybe a plugin was to blame?

To help in this debugging, I copied the HTML troublesome blog post into my local staging installation of WordPress, enabled the [WP_DEBUG](https://wordpress.org/support/article/debugging-in-wordpress/) flag, and opened the page. Bingo!

	Fatal error: Uncaught Exception: Unable to retrieve URL: https://tools.wmflabs.org/magnus-toolserver/commonsapi.php?image=File:19860811a_Mals.jpg&thumbwidth=500 
	in \htdocs\wordpress\wp-content\plugins\embed-wikimedia\embed-wikimedia.php:187 
	Stack trace: #0 \htdocs\wordpress\wp-content\plugins\embed-wikimedia\embed-wikimedia.php(89): 
	embed_wikimedia_get_data('https://tools.w...', 'xml') 
	#1 \htdocs\wordpress\wp-includes\class-wp-embed.php(172): 	embed_wikimedia_commons(Array, Array, 'https://commons...', Array) 
	#2 \htdocs\wordpress\wp-includes\class-wp-embed.php(411): 	WP_Embed->shortcode(Array, 'https://commons...') 
	#3 [internal function]: WP_Embed->autoembed_callback(Array) 
	#4 \htdocs\wordpress\wp-includes\class-wp-embed.php(393): preg_replace_callback('|^(\\s*)(https?:...', Array, 'Start content.\r...') 
	#5 \htdocs\wordpress\wp-includes\class-wp-hook.php(286): WP_Embed->autoembed('Start content.\r...') 
	#6 \htdocs\wordpress\wp-includes\plugin in \htdocs\wordpress\wp-content\plugins\embed-wikimedia\embed-wikimedia.php on line 187

A misbehaving WordPress plugin was to blame - the [Embed Wikimedia](https://wordpress.org/plugins/embed-wikimedia/)  plugin that I used to quickly embed freely licensed photos from Wikimedia Commons in my posts.

So what about the misbehaving URL - had someone at the Wikimedia Foundation taken the Wikimedia Commons API server offline?

Nope - [loading fine](https://tools.wmflabs.org/magnus-toolserver/commonsapi.php?image=File:19860811a_Mals.jpg&thumbwidth=500 ) in a normal web browser.

	<response version="0.92">
		<file>
			<name>19860811a Mals.jpg</name>
			<title>File:19860811a_Mals.jpg</title>
			<urls>
				<file>https://upload.wikimedia.org/wikipedia/commons/9/96/19860811a_Mals.jpg</file>
				<description>http://commons.wikimedia.org/wiki/File:19860811a_Mals.jpg</description>
				<thumbnail>https://upload.wikimedia.org/wikipedia/commons/thumb/9/96/19860811a_Mals.jpg/500px-19860811a_Mals.jpg</thumbnail>
			</urls>
	[SNIP]
	</response>

Maybe the API server is blocking my requests because they consider it abuse? I had a look at how WordPress makes HTTP requests, and ended up in [class-http.php](https://github.com/WordPress/WordPress/blob/master/wp-includes/class-http.php) and the setting of the HTTP user-agent header.

	'user-agent'          => apply_filters( 'http_headers_useragent', 'WordPress/' . get_bloginfo( 'version' ) . '; ' . get_bloginfo( 'url' ), $url ),

The header has `WordPress` in there, plus metadata about the blog making the request, so there is enough unique data there to block someone. So what  if I change it to something completely different?

	'user-agent' => apply_filters( 'http_headers_useragent', 'xcvxcvxcvxcvv' ),

Hmmm - no difference there!

I then started stuffing around with [Fiddler](https://www.telerik.com/download/fiddler) to sniff the HTTP traffic between WordPress and the API server, but then I realised something... why was the server so slow to respond?

I did a bit of research on how WordPress makes HTTP requests, and [got an answer in this blog post](https://refactored.co/blog/extending-wordpress-http-request-timeout-lengths).

> The WordPress HTTP API allows developers to easily make HTTP requests across different server configurations, but by default the request times out after 5 seconds. 
> 
> This keeps page load times down on your site if an external request is taking too long, but in some situations you run into errors like "name lookup timed out". 

As well as this useful code snipped.

	<?php
	/**
	 * Extend http timeout duration to 15 seconds
	 * 
	 * @param int $timeout The timeout duration in seconds. Default is 5.
	 *
	 * @return int The filtered timeout duration in seconds.
	 */
	function __extend_http_request_timeout( $timeout ) {
		return 15; // seconds
	}
	add_filter( 'http_request_timeout', '__extend_http_request_timeout' );

Adding this to the troublesome WordPress plugin fixed my test page - the error was gone, but at the cost of slowing down page load times. So time for some more fixes!

The first one - replace the hard page crash.

	throw new Exception( sprintf( $msg, $url ) );

With an error logged in the backend.

	error_log( sprintf( $msg, $url ) );
	return;

Add fallback code so that if the API request fails, a plain HTML link appears in place of the expected image.

![Fallback code for API failure](\images\Fallback-code-for-embed-wikimedia-on-api-failure.jpg)

And something new about the plugin I didn't realise - it uses the [WordPress 'Transients' API](https://developer.wordpress.org/apis/handbook/transients/) to cache the data from the Wikimedia Commons API server.

	set_transient( $transient_name, $info, 60 * 60 );

The last number is the expiry time for the cache - 3600 seconds, which is one hour. I didn't want my blog to reach out to a 3rd party every hour, for every single image that I'm embedding from Wikimedia Commons, so I set the cache lifetime to forever.

	set_transient( $transient_name, $info, 0 );

The worst case scenario of caching forever is that someone updates their username at Wikimedia Commons, and the attribution that appears on my WordPress site stay the same - something that would happen anyway if I manually embedded the photo instead.