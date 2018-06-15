---
layout: post
title: Debugging why WordPress missed a scheduled post
author: wongm
comments: true
tags: [WordPress, blogging, server admin]
---
One thing you might not know about my blogs is that I don’t wake up each morning and type up a new blog post – I write them ahead of time, and set them up to be published at a future time. Unfortunately this doesn’t always work - which sent me down a rabbit hole of debugging how WordPress manages scheduled posts.

**The backstory**

I use WordPress to host my various blog sites, and it has a feature called "[scheduled posts](https://make.wordpress.org/support/user-manual/content/posts/schedule-a-post/)" – set the time you want the post to go online, and in theory they’ll magically appear in the future, without any manual intervention.

For this magic to happen, WordPress has to regularly check what time it is, check if any posts are due to be published, and if so, publish them – a process that is triggered in two different ways:

* run the check every time someone visits the site, or
* run the check based on a [cron job](https://en.wikipedia.org/wiki/Cron) (scheduled task)

The [first option is unreliable](https://stevengliebe.com/2013/11/18/using-real-wordpress-cron-job-increased-reliability/) because it delays page load times, and you can’t count on people visiting a low traffic web site, so the second option is what I put in place when setting up my server - and it has worked for years - at least [most of the time](https://wongm.com/2015/05/wordpress-scheduled-post-issues/).

**Adding more automation to make my life easy**

Being able to future date a post is nifty enough, but given I always publish my blog posts at the same time each day, once a week - why even bother setting the date, when I can get WordPress to do all of the work for me!

I achieve this via the [Publish to Schedule](https://wordpress.org/plugins/publish-to-schedule/) plugin created by Alex Benfica:

> With this plugin you don’t need to manually choose the date when a post will be published. After a simple configuration the plugin will schedule your posts when you click publish.
> 
> Just configure days of week, the number of posts each day and time interval you want posts to be auto scheduled and each post will be automatically scheduled for these days with no more than the number you specified of posts per day.

And to allow easy reordering of scheduled posts, I use the [Editorial Calendar](https://wordpress.org/plugins/editorial-calendar/) plugin by Colin Vernon, Justin Evans, Joachim Kudish, Mary Vogt, and Zack Grossbart:

> Did you remember to write a post for next Tuesday? What about the Tuesday after that? WordPress doesn’t make it easy to see when your posts are scheduled. The editorial calendar gives you an overview of your blog and when each post will be published. You can drag and drop to move posts, edit posts right in the calendar, and manage your entire blog.

But it appears this automation doesn't always play nice.

**So my problem**

I first noticed the problem over at my [Euro Gunzel](https://eurogunzel.com/) blog - my scheduled post from June 7 was still sitting in the "Publishing Soon" list. So what gives?

If you ask Google, [you get answers like this](http://dominiquej.com/wordpress-missed-schedule-error-fix/):

> Why WordPress misses scheduled posts
> 
> It all comes down to these little things called cron jobs.
> 
> The scheduled post feature of WordPress is run by cron jobs. And unfortunately, sometimes WordPress can be finicky and the tasks misfire.
>
> So basically, if someone doesn't visit your site around the time your post is scheduled to go live, the event won't trigger and your WordPress scheduled post won't publish. Instead, you'll see the Missed Schedule error.

But in my case this had nothing to do with it - my cron jobs are ticking over every 5 minutes, run as a *real* cron job by my server, not jerry rigged up to page requests.

In addition, manually requesting the `wp-cron.php` page via my web browser made no difference - the unpublished blog post stayed unpublished.

Visitors to the WordPress.org forums were also suffering from posts never being published, and [asking why WordPress doesn't retry](https://wordpress.org/ideas/topic/missed-schedule-post-retry).

> Why is it that when WordPress misses a post, it just says "Missed Schedule" instead of retrying until the post is actually published?
> 
> I do care to an extent that it wasn't published on time, but it bothers me even more when I rely on WordPress to publish a post I scheduled, and I check to make sure it was published hours later, or even the next day, only to find out that my post didn't go out on time, or at all, for the matter.
> 
> If WordPress retried and published within the next 5 to 10 minutes after it missed the scheduled post, I really wouldn't be too upset about it.
> 
> But instead of making sure the task was complete, WordPress simply gives up. I'm sure it would be fairly easy to check to see that a post "missed schedule" and just to publish it ASAP.

For some people scheduled posts end up in a 'failed' status:

> It only runs when someone visits the site, and if no one does, and 'enough' time passes, it will flag a post or event as a failure rather than reschedule.

But not for this user:

> We have a actual cronjob running aswell so every 15 minutes the WP cron gets triggered.
> 
> Still WP fails to schedule the post.

Sounds just like my problem.

**Digging deeper**

If my `wp-cron.php` page is being executed regularly, does my problem lie in WordPress itself?

To debug this, I installed the [WP Crontrol](https://wordpress.org/plugins/wp-crontrol/) plugin by John Blackbourn & contributors.

> WP Crontrol lets you view and control what’s happening in the WP-Cron system. From the admin screens you can:
> 
> * View all cron events along with their arguments, recurrence, callback functions, and when they are next due.
> * Edit, delete, and immediately run any cron events.
> * Add new cron events.
> * Bulk delete cron events.
> * Add, edit, and remove custom cron schedules.
> 
> The admin screen will show you a warning message if your cron system doesn’t appear to be working

Once installed, I took a look through the list of cron jobs. There were plenty of background tasks listed, belonging to various plugins that I have installed, as well as a series on one-off tasks appearing on the same day each week - except for some weeks.

With this I finally understood how WordPress was scheduling future dated blog posts.

No - it doesn't poll for upcoming posts every X minutes, and if any are found, post them - but instead:

1. you save future dated blog post.
2. a future dated `publish_future_post` event is added to the list of cron tasks to be executed by WordPress.
3. WordPress cron executes on a regular basis.
4. `publish_future_post` event gets executed.
5. the `check_and_publish_future_post()` method for the relevant blog post is executed.
6. blog post gets published.

It also revealed why some of my future dated posts were not being published - no `publish_future_post` record was being written to the list of WordPress cron tasks, so they would never get published.

So why were they missing? I am assuming some kind of interaction between the 'Editorial Calendar' and 'Publish to Schedule' plugins is resulting in posts being saved, but the cron tasks were getting missed.

**Finding a solution**

My fix for the missing tasks that I found was simple - edit each affected blog post, edit the schedule date, then click save. Boom - a `publish_future_post` record appeared in the list of cron tasks.

But what if the same thing happens in future?

Since I didn't want to end up down a rabbit hole of debugging 3rd party plugins, I decided to patch over the problem, and take a belt and braces approach.

When I searched the WordPress plugin directory for "missed scheduled posts" I found dozens of plugins, with the most popular having 10,000+ active installations. 

All of them appear to work the same way:

1. run frequently.
2. poll the database for blog posts that are past their publication date but still sitting in the 'scheduled' status, 
3. if any posts are found, trigger their publication. 

But what is the difference between them?

#1 is [WP Missed Schedule Posts](https://wordpress.org/plugins/wp-missed-schedule-posts/) by newvariable, but [when I took a look at the code](https://plugins.trac.wordpress.org/browser/wp-missed-schedule-posts/trunk/wp-missed-schedule-posts.php), I was disappointed:

```
//Hook into WordPress
add_action( 'init', 'nv_wpmsp_init', 0 );

function nv_wpmsp_init() {

	// check last run time

	//snipped
	
	$sql_query = "SELECT ID FROM {$wpdb->posts} WHERE ( ( post_date > 0 && post_date <= %s ) ) AND post_status = 'future' LIMIT 0,%d";
	
	//snipped
}
```

For those who don't speak WordPress plugin hooks, the `init` hook is [triggered on every page load](https://developer.wordpress.org/reference/hooks/init/):

> Fires after WordPress has finished loading but before any headers are sent.

The plugin does have some smarts to prevent the main database query from being carried out every time someone visits your site, but still this isn't the best code.

I then had a look at plugin #2 - [Scheduled Post Trigger](https://wordpress.org/plugins/scheduled-post-trigger/) by Jennifer Moss of Moss Web Works - and was [disappointed by the code yet again](https://plugins.trac.wordpress.org/browser/scheduled-post-trigger/trunk/scheduled-post-trigger.php).

```
function pubMissedPosts() {
	if (is_front_page() || is_single()) {
		global $wpdb;
		$now=gmdate('Y-m-d H:i:00');
		$sql="Select ID from $wpdb->posts where post_status='future' and post_date_gmt<'$now'";
		$resulto = $wpdb->get_results($sql);
 		if($resulto) {
			foreach( $resulto as $thisarr ) {
				wp_publish_post($thisarr->ID);
			}
		}
	}
}
add_action('wp_head', 'pubMissedPosts'); 
```

The `wp_head` hook is again [called on every page load](https://codex.wordpress.org/Plugin_API/Action_Reference/wp_head), but things get worse - the database query is triggered on every visit to the front page or blog post.

So where to find a scheduled post plugin that doesn't flog my web server?

**And problem solved**

I eventually found the [MY Missed Schedule](https://wordpress.org/plugins/my-missed-schedule/) plugin by 水脉烟香, and was [pleasantly suprised by the code](https://plugins.trac.wordpress.org/browser/my-missed-schedule/trunk/my-missed-schedule.php):

```
function missed_schedule() {
	global $wpdb;
	$scheduledIDs = $wpdb -> get_col("SELECT ID FROM $wpdb->posts WHERE post_status='future' AND post_date<=CURRENT_TIMESTAMP() LIMIT 5");
	if ($scheduledIDs) {
		foreach($scheduledIDs as $postID) {
			wp_publish_post($postID);
		} 
	} 
} 

add_action('missed_schedule_cron', 'missed_schedule', 10);
```

Nice and clean - this plugin adds a WordPress cron job triggered every 10 minutes, which executes the database query for scheduled posts.

**Footnote**

Even for users on shared hosting accounts, relying on a missed scheduled post plugin that relies on the WordPress cron framework is acceptable - you now have a 'belt and braces' tool that will catch posts that get missed due to a lack of visitors to your site.


