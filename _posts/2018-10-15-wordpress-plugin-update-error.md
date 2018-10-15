---
layout: post
title: Debugging a PCLZIP_ERR_BAD_FORMAT error from WordPress
author: wongm
comments: true
tags: [WordPress, websites]
---

Not so long ago I attmepted to update the plugins that power by WordPress blog, when I got this error message.

>  The package could not be installed. PCLZIP_ERR_BAD_FORMAT (-10) : 
>  Unable to find End of Central Dir Record signature

I attempted to retry update, and it failed again every time with the same error.

Stack Overflow [gave this advice](https://stackoverflow.com/questions/17771578/wordpress-theme-upload-error-pclzip-err-bad-format):

> I've increased the upload_max_filesize, post_max_size and memory_limit as well. But still I'm getting the same error.

But in my case it was something different - my server ran out of disk space, and a corrupted zip file was downloaded.

But how to fix it?

WordPress saves the plugin packages into the temp directory of the server, so all I had to do was open the temp directory, delete the corrupted file, free up some extra disk space, and then retry the plugin update.

**References** 

* [WordPress Codex for `get_temp_dir()`](https://developer.wordpress.org/reference/functions/get_temp_dir/)
* [How to define the WordPress temp directory](https://cometcache.com/kb-article/how-do-i-define-a-wordpress-temp-directory/)