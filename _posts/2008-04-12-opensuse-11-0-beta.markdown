---
layout: post
title: openSUSE 11.0 beta
date: '2008-04-12'
tags:
- fedora
- newsuse
- packagekit
- suse
---

openSUSE 11.0 beta is coming. This means that all the pieces of the update stack have to be in place by then.

While reading [Review: Hat Trick For Fedora 9 Beta][11], it caught my attention that the author says "Fedora continues to wear the innovation hat" because some of the changes described there. I did not see any killer feature openSUSE 11.0 does not have too, and a few ones are worth to highlight:

* KDE 4 integration: No distro will ever beat openSUSE here ;-) you know that. openSUSE is a multi desktop distribution and both the Gnome and the KDE team are unbeatable. Period. (I am objective today ;-) )  
* The installation experience and look essentially remained the same from Fedora 8: Sorry guys, but YaST installer [looks far cooler][12] than in our last release. The [experience is also much different][13].  
* except there is now support for resizing ext2, ext3, and NTFS partitions from the installer: zzzzzzz. YaST resizes partitions before Linux was invented.  
* The installer also checks for password strength for the root account: YaST in the 80's ?  
* The new package management solution, PackageKit, is another interesting feature: openSUSE 11.0 will have native PackageKit support (the backend is upstream) and it works really well. The yum backend has no good reputation. The ZYpp backend in 11.0 inherits all the unbeatable speed of ZYpp 4.x and it is robust at the same time.

Sorry guys, the innovation hat is green. Ok, enough with articles. Lets back to 11.0 beta.

We talked about [package management speed][7], we talked about [new looks and features already][8]. However our work around patches and patterns was still missing.

During the last weeks, we have been working on this and now all the pieces start to fall together. Click on any image to see it in full size. Also note that ugly scrollbar in the disk usage is was also fixed already.

You may remember the pattern selector:

[![Old Pattern Selector][9]][10]

New pattern selector:

[![New Pattern Selector][3]][4]

If you go to the repository view, it was a little boring. openSUSE has the Build Service, which has generated a big community of repositories. Visually, we want to make a difference to the eye if a repo is a normal repository, or the home project of a friend, or a well known repository, or may be a update repository. Result?

[![New repo view][1]][2]

The old patch view was confusing. The category column never had space for itself.

[![Old patch selector][14]][15]

So we introduced categories just like in the patterns view.

[![Old patch selector][5]][6]

There are still some details. In that view the reboot patch should not be shown in the default filter, because I don't have the package the patch fixes installed. For some reason isRelevant() is not working there. Same with the security patch. It shows correctly the fact that the patch is satisfied, but therefore it should be hidden.

In a next post I will write a bit about how patterns, products and patches are handled now, plus other features.

[1]: http://files.opensuse.org/opensuse/en/thumb/3/3f/Repos.png/783px-Repos.png  
 [2]: http://files.opensuse.org/opensuse/en/3/3f/Repos.png  
 [3]: http://files.opensuse.org/opensuse/en/thumb/e/e6/Patterns.png/750px-Patterns.png  
 [4]: http://files.opensuse.org/opensuse/en/e/e6/Patterns.png  
 [5]: http://files.opensuse.org/opensuse/en/thumb/8/8c/Patches.png/800px-Patches.png  
 [6]: http://files.opensuse.org/opensuse/en/8/8c/Patches.png  
 [7]: http://duncan.mac-vicar.com/blog/archives/296  
 [8]: http://duncan.mac-vicar.com/blog/archives/303  
 [9]: http://files.opensuse.org/opensuse/de/thumb/b/b8/Selecting-patterns-de.png/755px-Selecting-patterns-de.png  
 [10]: http://files.opensuse.org/opensuse/de/b/b8/Selecting-patterns-de.png  
 [11]: http://www.crn.com/software/207200137  
 [12]: http://news.opensuse.org/2008/03/19/announcing-opensuse-110-alpha-3/  
 [13]: http://www.kdedevelopers.org/node/3385  
 [14]: http://files.opensuse.org/opensuse/de/thumb/e/e7/Yast-gui-online-update.png/550px-Yast-gui-online-update.png  
 [15]: http://de.opensuse.org/Bild:Yast-gui-online-update.png

