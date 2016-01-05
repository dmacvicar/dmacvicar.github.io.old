---
layout: post
title: Facebook on Kopete, take II
date: '2009-05-25'
tags:
- facebook
- newkde
- newsuse
- software
- suse
---

Last week I blogged about http://duncan.mac-vicar.com/blog/archives/541, just after I  
was able to see my buddies for first time on the screen.

Since then I have made some improvements to message handling and other  
code cleanups. The code is now available in a http://github.com/dmacvicar/kopete-facebook at  
github.

As KDE’s svn trunk is frozen, I will keep it there for now.

You can get [http://software.opensuse.org/search?baseproject=openSUSE%3AFactory&p=1&am...](http://software.opensuse.org/search?baseproject=openSUSE%3AFactory&p=1&q=kde4-kopete-protocol-facebook) (version 0.1.2). I gave  
up trying to build it for openSUSE 11.1, as Kopete API has changed quite  
a bit. However the package may build on 11.1 plus the KDE 4.2+  
repositories. You need http://software.opensuse.org/search?baseproject=openSUSE%3AFactory&p=1&q=qjson installed (or  
-devel package if you want to build it).

Roadmap for next 0.1.3:

Add caching to avoid downloading the pictures every 3 minutes.

More bugfixes

Roadmap for later:

Look into adding , searching, and other stuff.

Be aware. This is weeks-old-code. It has not been tested much and has  
lot of debug messages. Use it if you are a early adopter only.

