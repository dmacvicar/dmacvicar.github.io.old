---
layout: post
title: kopete-facebook 0.1.4
date: '2009-10-16'
tags:
- newsuse
- software
- suse
---

I have released kopete-facebook 0.1.4 which fixes idle status of contacts and message sending.

I still have some important bugs, which I hope I can fix before the openSUSE 11.2 release.

From those known bugs, one is the inability to reconnect (go offline and offline), which is due to the “myself” contact using the email as userid, which is in turn due to a Kopete limitation of needing to know an id, but facebook sends it after login. I have tried so many hacks (like switching the myself contact at runtime), but all have other side effects.

Another one is a crash at exit, which I still can’t understand. Help appreciated.

_Note: this plugin is not affiliated with Facebook in any way._

Should appear [here](http://software.opensuse.org/search?baseproject=ALL&p=1&q=kopete-protocol-facebook) soon (or [build it yourself](http://github.com/dmacvicar/kopete-facebook)).

