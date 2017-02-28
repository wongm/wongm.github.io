---
layout: post
title: Backup your Wordpress database with a cron job
date: 2012-09-25 07:30
author: wongm
comments: true
tags: [backups, blogging, shared hosting, Technology, WordPress]
---
I run this blog with WordPress - today it powers <a href="http://www.forbes.com/sites/jjcolao/2012/09/05/the-internets-mother-tongue/" target="_blank">one of every 6 websites on the Internet</a>. I use a shared hosting account to run the site so I have total control, but it also means I need to make sure everything gets backed up regularly so if my hosting provider derails it, I can quickly get things... <em>back on track</em>.

<img src="http://railgallery.wongm.com/cache/stonyford-derailment/E100_6900_500.jpg" alt="V/Line derailment in September 2009 at Stonyford, Victoria - the train hit a fallen tree, leaving the two locomotives in the dirt" />

The easiest way to backup a self-hosted WordPress blog is by using the "<a href="http://wordpress.org/extend/plugins/wp-dbmanager/" target="_blank">WP-DBManager</a>" plugin - install it via the control panel, tick a few boxes, and a backup of your entire database gets sent to you via email every night, giving you a way to get your website content back if something goes wrong.

For me WP-DBManager did everything it promised, at least until my hosting provider changed their security settings and disabled a number of functions that the backup plugin needed to work, leaving me with a failing backup job and this error message:

<blockquote>Checking PHP Functions (passthru(), system() and exec()) ...
passthru() disabled.
system() disabled.
exec() disabled.	

I'm sorry, your server administrator has disabled passthru(), system() and exec(), thus you cannot use this backup script. You may consider using the default WordPress database backup script instead.	</blockquote>

Unfortunately for me, I didn't realise my backups weren't working until a few weeks after the server config change was made, when I discovered that my nightly emails only contained an empty backup file!

With the easy to use WP-DBManager option ruled out, I turned to a cron job - a small program that runs on your server, based on a regular schedule you define. Thankfully someone had already come up with a script to do just what I wanted to do - you can find his instructions at <a href="http://www.tamba2.org.uk/wordpress/cron/" target="_blank">http://www.tamba2.org.uk/wordpress/cron/</a>.

Once I added the above cron job to my server I ran into a major issue - the 'mutt' command for sending email was not available to me, so I needed to jump into the world of shell scripting to find another way  to do the same thing. This is the replacement script I came up with:

{% highlight shell %}
#Set the 4 variables
#Replace what is AFTER the = with the information from your wp-config.php file
#That's your information on the right okay ?

DBNAME=DB_NAME
DBPASS=DB_PASSWORD
DBUSER=DB_USER

#Keep the " around your address
EMAIL="you@email.com"

DATE=`date +%Y%m%d-%H%M%S`
PARTDATE=`date +%Y-%m-%d`

mysqldump -u $DBUSER -p$DBPASS $DBNAME > $DBNAME-$DATE.sql
gzip $DBNAME-$DATE.sql
uuencode $DBNAME-$DATE.sql.gz $DBNAME-backup-$DATE.sql.gz | mail -s "MySQL backup for $PARTDATE" $EMAIL
rm $DBNAME-$DATE.sql.gz{% endhighlight %}

My shell script does much the same thing as the original script I based it upon, except that the email you get isn't anywhere as nice looking - just a subject line and an attached backup file.

Fill in your own values for the first four variables, save the script as a file, and then follow the instructions back at <a href="http://www.tamba2.org.uk/wordpress/cron/" target="_blank">http://www.tamba2.org.uk/wordpress/cron/</a> to get it running as a Cron job on your own server.

I setup my backups to run once daily, and since I've set it up the emails have been successfully arriving in my inbox, with a gzipped SQL script as the attachment. Let's hope I never have a need to restore them!

<strong>And a sidenote...</strong>

The derailed V/Line train picture above was the aftermath of a tree falling over the tracks at Stonyford, Victoria back in 2009. No-one was killed or seriously injured when the leading locomotive ended up sideways, with the second locomotive and a few of the carriages coming off the rails - more details in <a href="http://www.transport.vic.gov.au/__data/assets/pdf_file/0009/31014/Derailment-of-VLine-Passenger-Train-at-Stonyford-12-September-2009.pdf" target="_blank">the accident investigation report</a>.
