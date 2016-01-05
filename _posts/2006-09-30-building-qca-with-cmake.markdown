---
layout: post
title: Building QCA with cmake
date: '2006-09-30'
---

As you might know, Kopete in KDE 4 depends on QCA (Qt Crypto Architecture). Also, KDE 4 uses cmake as it build system.  
QCA was imported into KDE's svn, but it uses qmake as a build system. This is not a problem at all. But as qmake was not enough, they decided to implement its own configuration system to compile QCA, and of course you have to hand compile it.

I am not going to get deep into QConf, its description files with embedded C++ code, etc. Just the good news. If you are developing on KDE and need to compile QCA, I managed to get a basic cmake build enviroment wich compiles QCA core, 2 plugins and 2 examples. The black magic is already there, and adding the rest should be piececake repetitive work. The only part I haven't figured yet are the testcases.

I will post the patch to kopete-devel.

I broke my glasses, so I get tired really soon with the screen :-(

