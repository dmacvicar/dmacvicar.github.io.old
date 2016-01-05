---
layout: post
title: gspcav1 in build servce
date: '2006-11-08'
tags:
- newsuse
- suse
---

You may know that in the drivers:webcam project of the [openSUSE][2] build service you find a repository with the spca5xx package, which includes drivers for lot of webcams.

Since kernel 2.6.11 is out, this driver set was renamed to gspcav1 ( Generic Softwares Package for Camera Adaptator ). I planned since some weeks to package it, but never pop out of my TODO. Even if I needed to, because spca5xx never worked with my camera after a kernel upgrade.

That is the cool thing about the build service. If someone else does it, it can be put in the same automated place and share maintenance with more people. [Martin][1] came to my office and told me that he had a package for gspcav1. woooho. cool. So now it is in the same repository among the old spca5xx (for use with kernels below 2.6.11). Enjoy it.

[1]: http://en.opensuse.org/User:Mlasars  
 [2]: http://build.opensuse.org/

