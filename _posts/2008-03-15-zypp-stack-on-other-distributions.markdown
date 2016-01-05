---
layout: post
title: ZYpp stack on other distributions?
date: '2008-03-15'
tags:
- fedora
- mandriva
- newsuse
- suse
- zypper
---

Keeping a permanent build of svn on the build service motivated me to try to build it on other non-SUSE based distros. On the way I discovered not only simple things as different package names, but linking problems, compile errors, etc.

In the past it did not make much sense to try libzypp outside of SUSE as we were trying to catchup with other tools speed. But now that libzypp speed outperforms any other tool in speed, while keeping it complete set of features, we may start thinking about taking over the world. May be other distros want to use libzypp, or why not, the small and powerful sat-solver library alone.

So, since today I got the first successful build of [sat-solver/libzypp/zypper][4] on [Fedora 8][1]. Mandriva is also close, but I still get a compiler error I should not get when compiling the rpm backend.

At the beginning, it may be a consuming effort, but once it is a continuous process, our software should adopt a more agnostic view of the world and just compile there out of the box. If anyone has the time to test it, I would be interested in the results ;-)

By the way, our colleagues at internal tools finished processing the FOSDEM talk's videos we had in the openSUSE Developer's room. You can find the ogg video files [here][2]. Also you can watch on [google video here][3].

[1]: http://fedoraproject.org/  
 [2]: http://tube.opensuse.org/  
 [3]: http://video.google.de/videosearch?q=FOSDEM2008+site%3Avideo.google.com&amp;num=10&amp;so=0&amp;hl=de&amp;start=0  
 [4]: https://build.opensuse.org/project/monitor?project=zypp%3Asvn

