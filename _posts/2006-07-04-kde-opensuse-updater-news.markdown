---
layout: post
title: "(KDE) OpenSUSE updater news"
date: '2006-07-04'
---

Today I have been trying the lastest enhancements Narayan did to his [updater applet][1] in order to provide feedback to him. It is looking really good and it feels really light and fast.

![updater screenshot][2]

There are some issues pending, one has to do with authentication as [ZMD][3] does not provide a way to authenticate without knowing the root password for tcp/ip based connections. For now Narayan implemented a password dialog on startup which uses a helper program to authenticate, but we hope to have a usable solution soon. Zen Updater can do it because it uses .NET remoting over Unix domain sockets, so zmd knows who is talking to.

Narayan's SOC project covers all the Zen functionality, and that is mostly done. The next step will be adding support for smart and direct system access (via zypp) backends, which should be trivial to add.

If you want to help testing, or give it a try, I have created packages using the build service, you can find the [repositories here][4]. The package name is opensuse-updater.

[1]: http://en.opensuse.org/KDE_Updater_Applet  
 [2]: http://files.opensuse.org/opensuse/en/2/28/R177-images/posts/1.png  
 [3]: http://en.opensuse.org/Zmd  
 [4]: http://repos.opensuse.org/home:/dmacvicar/

