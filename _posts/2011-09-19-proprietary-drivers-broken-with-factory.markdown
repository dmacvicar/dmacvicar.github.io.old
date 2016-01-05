---
layout: post
title: proprietary drivers broken with Factory
date: '2011-09-19'
categories:
- Software
tags:
- suse
---

ld.so.conf seems to have changed to include /etc/ld.so.conf.d/\*.conf.

Unfortunately, proprietary drivers like the NVIDIA installed a file without the .conf extension. Therefore XOrg did not find the custom openGL library included by the driver.

Tracked as [bug 718734](http://bugzilla.novell.com/718734 "bug 718734").

[home:dmacvicar:branches:X11:Drivers:Video](https://build.opensuse.org/project/show?project=home%3Admacvicar%3Abranches%3AX11%3ADrivers%3AVideo) contains a fixed nvidia-gfxG02 (Also updated to 280.13).

