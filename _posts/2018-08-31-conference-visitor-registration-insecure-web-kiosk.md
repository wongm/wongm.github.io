---
layout: post
title: Conference visitor registration via an insecure web portal
author: wongm
comments: true
tags: [security, privacy, server admin]
---
I recently paid a visit to a trade show where visitor registration was required. I was directed to a bank of tablet PCs, where I was asked to enter my personal details so that a visitor badge could be printed out.

When I started filling in the form, I noticed something concerning - as soon as I started typing in any single form field, it was being prefilled with the personal information of the people who had used the kiosk before me - first name, last name, company name, email addresses and phone numbers.

[Example personal details form]({{ site.url }}/images/upload.wikimedia.org PT015-_Figure_6.5_(3978882756).png)

When I got back to work I shared my experiences on Slack, including the fact that our company name was in the list of prefill selections, and our security and compliance officer immediately replied:

> I noticed the same thing! I'll raise it this afternoon at their booth as a casual FYI

So how do you screw up something so simple?

**So who's fault is it?**
Turns out trade shows doesn't run their own registration process - they outsource it to a specialist convention registration manager, who then use either their own bespoke software to colelct data, or licences a 3rd party product to do the job.

**How to fix it**

Getting the creator of the web form to add the `autocomplete` attribute to form elements [is one solution](https://css-tricks.com/snippets/html/autocomplete-off/).

```
<form autocomplete="off">
```

And...

```
<input name="q" type="text" autocomplete="off"/>
```

But this is often [ignored by browsers like Google Chrome](https://webscripts.softpedia.com/blog/Chrome-34-Seeks-to-Save-All-Your-Passwords-436693.shtml) which also supports 'autofill' between different web sites.

[Screenshot of Google Chrome autofill in use]({{ site.url }}/images/webmasters.googleblog.com Sherlock Screenshot.png).

So you need to also ensure that autofill is disabled in the web browser being used to run the data collection form.

- [Google Chrome](https://support.google.com/chrome/answer/142893?co=GENIE.Platform%3DDesktop&hl=en)
- [Internet Explorer](https://support.microsoft.com/en-au/help/17499/windows-internet-explorer-11-remember-passwords-fill-out-web-forms)
- [Firefox](https://support.mozilla.org/en-US/kb/control-whether-firefox-automatically-fills-forms)

This example is for Google Chrome.

[How to disable Google Chrome autofill]({{ site.url }}/images/wikihow Manage-Passwords-and-Autofill-Settings-on-Google-Chrome-Step-4.jpg)

**Further reading**

Cloud Four has an excellent piece on the difference between autocomplete and autofill titled [Autofill: What web devs should know, but don’t](https://cloudfour.com/thinks/autofill-what-web-devs-should-know-but-dont/).



