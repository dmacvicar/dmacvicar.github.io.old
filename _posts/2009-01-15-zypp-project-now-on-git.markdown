---
layout: post
title: ZYpp project now on git
date: '2009-01-15'
tags:
- git
- newsuse
- software
- suse
- yast
- zypp
---

You may have noticed (or not?) that svn.opensuse.org/svn/zypp is now  
read-only :-)

Since a couple of weeks the ZYpp project repository is now hosted on git.opensuse.org. Please read the official announcement [here][3].

Now you can fork and develop much easily without needing access to the "official repository". Developers can work disconnected, enjoy the git features to handle merges and branches, oh, and you can keep your forks in git publishing sites like the cool [GitHub][13]

We updated the following pages to reflect the move:

* [ZYpp Development page][4]  
* [sat-solver homepage][5]

Those also link to the new pages, written as a starting point:

* [General git information and useful links][6]  
* [git hosting (including git.opensuse.org) information][7]

One of the challenges of the migration was continuous integration. We needed to replicate the automated  
building/testing process, which is a core part of the ZYpp team development model.

The process has been migrated from cruisecontrol.rb to [Hudson][8] ( Extensible continuous integration engine, [https://hudson.dev.java.net/)](https://hudson.dev.java.net/)) which provides better job handling support, plugins and it is not tied  
to svn.

Some new features:

* We now build zypper automatically over the rest of the ZYpp stack.  
* We publish the last successful build that passes the testsuite automatically to [zypp:Head][10] project. This one will obsolete zypp:svn.  
* We are working to build other pieces of the stack automatically, like PackageKit.  
* We are working to also build and test the whole YaST stack, so it can also be published automatically. So expect a YaST:Head project soon too, to obsolete YaST:SVN (outdated).

I would like to thank R. Tyler for his help with Hudson plugins, [Jens Daniel][12] for his original automation scripts we used, which are still the base for the Hudson build scripts :-), [Stano][11] for adding on-request features to y2makeall, and the [Hudson][8] developers for such a amazing piece of software.

[1]: http://www.qtsoftware.com/about/news/lgpl-license-option-added-to-qt  
[2]: http://arstechnica.com/news.ars/post/20090114-nokia-qt-lgpl-switch-huge-win-for-cross-platform-development.html  
[3]: http://lists.opensuse.org/zypp-devel/2009-01/msg00000.html  
[4]: http://en.opensuse.org/Libzypp/Devel  
[5]: http://en.opensuse.org/Package_Management/Sat_Solver  
[6]: http://en.opensuse.org/Git  
[7]: http://en.opensuse.org/Git_Hosting  
[8]: https://hudson.dev.java.net/  
[9]: http://labs.trolltech.com/blogs/2009/01/14/nokia-to-license-qt-under-lgpl/  
[10]: http://download.opensuse.org/repositories/zypp:/Head/  
[11]: http://en.opensuse.org/User:Visnov  
[12]: http://en.opensuse.org/User:Jdsn  
[13]: http://www.github.com

