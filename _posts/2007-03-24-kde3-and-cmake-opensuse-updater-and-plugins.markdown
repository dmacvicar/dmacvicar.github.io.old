---
layout: post
title: KDE3 and cmake, opensuse-updater and plugins
date: '2007-03-24'
tags:
- newkde
- newsuse
- suse
---

Yesterday I was able to compile opensuse-updater with cmake. The solution was to look in [KPilot][1] build system. I did not realize kde3\_automoc figures QObject classes looking for a .moc include in the implementation file instead of looking for Q\_OBJECT in the header. I can understand this, because sometimes you define some quick QObject classes when doing test programs in one implementation file. Still, took me a while to figure why it was not working.

Moving the current updater backends to KDE generic components was easy. It still lacks a way to select a default component.

Performancing, the Firefox blogging add-on I use, was renamed to [ScribeFire][2].

[1]: http://www.kpilot.org  
 [2]: http://scribefire.com/

