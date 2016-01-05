---
layout: post
title: SUSE Manager, a year later retrospective
date: '2012-05-03'
categories:
- Software
tags:
- spacewalk
- suse
- susemanager
- zypp
---

It has been more than a year. Around March 2011 we shipped SUSE Manager 1.2 and enhanced the management story for our customers. Since then we have been very busy! Time to look back and see what we have done. This first post will describe the features we have been working on. In a future post I will address more details about our development process and relationship with Spacewalk.

![SUSE Manager screenshot]({{ site.baseurl }}/assets/images/posts/40_system_groups.png)

### Setup reinvented

SUSE shines not only in the [number of certified enterprise applications](http://www.marketwatch.com/story/suse-leads-with-most-certified-applications-across-linux-vendors-2012-04-25) but also in the appliances area with tools like [SUSE Studio](http://susestudio.com). We allow our customers to build custom SUSE-based distributions with a few clicks.

When we set to build SUSE Manager as a product we decided to eat our own dog-food. After looking at the installation procedures of Spacewalk we found a natural way to make setting up SUSE Manager simple by using our existing technologies.

- Appliance form-factor: SUSE Manager is a simple bare-metal or virtual appliance. Just boot it, answer a few questions and you have a SUSE Manager server running.
- [YaST-based setup and migration](http://suse.gansert.net/?p=136): a first-boot work-flow assists you with any configuration and data migration.

![]({{ site.baseurl }}/assets/images/posts/18_db_setup.jpeg)

### Creation of SUSE Manager-ready appliances from SUSE Studio

Not all the cool stuff happens in SUSE Manager itself. The Studio team [added a feature](http://www.suse.com/blogs/from-studio-to-manager-with-a-click-of-your-mouse/) that allows you to create appliances in [SUSE Studio](http://susestudio.com) that are _SUSE Manager-ready_. This means once the image boots, it will automatically register itself to your SUSE Manager server and be ready to be managed.

![]({{ site.baseurl }}/assets/images/posts/suse-manager.png)

James did a very nice demo at BrainShare creating an image in SUSE Studio, deploying it to a private OpenStack cloud directly from the Studio user interface, and having the machine automatically register itself to SUSE Manager after booting. [Watch it here.](http://www.youtube.com/watch?v=B59G-gl4COc#t=4m7s)

### Audit logging

Regulatory and corporate auditing requirements require our customers to record what actions (and by whom) were done to the managed systems. We introduced [an audit logging feature](http://www.suse.com/blogs/suse-manager-eases-the-buden-of-compliance/) that allows you to record actions to a remote log, database, xml files, etc.

Audit Log Keeper, the buffer that receives the actions from the application is not specific to SUSE Manager and any application can be integrated using XML-RPC. Keeper is open-source and [available on github](https://github.com/SUSE/auditlog-keeper "Audit Log Keeper on github.com").

### Deploying images from SUSE Studio

SUSE Manager can deploy images to a physical host so that they run as virtual machines. If you are a SUSE customer, you will use Studio to create images. Creating in Studio, download the image, upload to SUSE Manager, deploy...? No way.

We added a [feature to deploy the images from Studio directly](http://www.suse.com/blogs/deploying-linux-images-can-be-fun/) in the SUSE Manager user interface. The code is already being reviewed upstream.

![]({{ site.baseurl }}/assets/images/posts/images.png)

### Code10 client support

For our customers running SLE-10 we back-ported the Code11 ZYpp stack (including a very fast zypper using the [SAT solver](http://libsolv.org)). The Code11 stack includes a [plugin architecture](http://doc.opensuse.org/projects/libzypp/12.1/zypp-plugins.html) that we use to hook with the spacewalk agent in order to get the server-side repositories and keeps the managed server software inventory up-to-date.

### SUSE Linux Enterprise Point of Service

Joe has been working with the SLEPOS team making sure that there is a story for them to work together. Check [his blog post to know more.](http://www.suse.com/blogs/suse-manager-and-point-of-service-a-dream-team)

### SUSE Manager Mobile (Android)

![]({{ site.baseurl }}/assets/images/posts/device-2012-05-02-105334.png)

During the last hack-week, part of the team took the mission to think how we could bring some of the SUSE Manager functionality to your mobile phone. We went beyond thinking and completed a prototype, which was [presented at Brainshare](www.youtube.com/watch?v=B59G-gl4COc#t=7m20s).

Today, we are releasing it, and you can [get it for free from Google Play](https://play.google.com/store/apps/details?id=de.suse.android.susemanager.app&feature=more_from_developer). Have fun with it!.

