---
layout: post
title: zypp on factory
date: '2007-03-07'
tags:
- newsuse
- suse
---

This week we are integrating a new libzypp which uses a new build system and a new .so version, which will be the base to integrate 10.3 features.

This uncovered some problems with link order and static initialization which we are solving this week. So it is very likely applications will crash/segfault.

The integration of new packages had some effect in zypp stack. Like a curl bug affecting ftp source probing, which is [now fixed][1].

So be careful, I'd recommend to install / update the package manager stack only in a virtual machine this week.

Also, we updated [the instructions how to build][2] for this new series.

[1]: http://article.gmane.org/gmane.comp.web.curl.cvs/6893  
 [2]: http://en.opensuse.org/Libzypp/Devel

