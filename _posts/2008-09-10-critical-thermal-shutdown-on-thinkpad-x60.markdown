---
layout: post
title: Critical thermal shutdown on Thinkpad x60
date: '2008-09-10'
tags:
- notfunny
- software
---

At the beginning it looked like some material for the [Linuxhater blog][2] :-)

It happened a couple of times before, while working on my my X60 and suddenly the computer switched to shutdown mode and turned off. Uh? annoying!

Yesterday it happened again, and this time I was able to find the answer in the logs:

Sep 10 01:29:26 piscolita powersaved[2658]: WARNING (checkTemperatureStateChanges:217)  
 Temperature state changed to critical.  
 Sep 10 01:29:26 piscolita kernel: Critical temperature reached (128 C), shutting down.  
 Sep 10 01:29:26 piscolita shutdown[15371]: shutting down for system halt

Then googling I found out it [is a bug in the kernel][1] ([google groups link, prettier][3]), and it seems there is a patch for it. I think this is a good backport candidate for 11.0 updates.

\*update\*: tracking as [bug #425077][4]

[1]: http://lkml.org/lkml/2008/8/6/192  
 [2]: http://linuxhaters.blogspot.com  
 [3]: [http://groups.google.com/group/linux.kernel/browse\_thread/thread/46bd42cb7435...](http://groups.google.com/group/linux.kernel/browse_thread/thread/46bd42cb74351c7d/beb6f414fce8b331?lnk=raot)  
 [4]: https://bugzilla.novell.com/show_bug.cgi?id=425077

