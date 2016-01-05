---
layout: post
title: Updater applets
date: '2007-04-10'
tags:
- newsuse
- suse
---

The openSUSE updater is a desktop applet which shows the user the available updates (patches). It is very light and simple as the real work is done by the YaST solver. This allows us to provide one applet per desktop, much like you have a a NetworkManager applet per desktop.

The KDE applet was born last year as a Google SoC project. Today I imported the opensuseupdater KDE applet sources from berlios to svn.opensuse.org/svn/zypp. The gnome applet was already in the zypp repository.

There is a [new wiki page][1] for both the KDE and Gnome applet.

The sources for the Gnome applet are [here][2], for the KDE applet [here][3].

Also, there is a [work branch][4] for the KDE applet where a major refactoring is going on to cleanup legacy code from SoC, use cmake as build system and load the backends as KDE plugins. It will become trunk once it works.

[1]: http://en.opensuse.org/Updater_Applet  
 [2]: http://svn.opensuse.org/svn/zypp/trunk/updater-gnome/  
 [3]: http://svn.opensuse.org/svn/zypp/branches/SuSE-Linux-10_2-Branch/updater-kde/  
 [4]: http://svn.opensuse.org/svn/zypp/branches/work/updater-kde-refactoring/updater-kde/

