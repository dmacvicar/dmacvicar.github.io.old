---
layout: post
title: User's feedback
date: '2008-05-26'
tags:
- newsuse
- software
- suse
- yast
- zypp
- zypper
---

I got some feedback from users about managing software.

[Raúl Moratalla][5]. who has been blogging about [using 11.0 ZYpp/YaST in 10.3][3], suggests two changes:

* Updated packages filter.

Adding that feature is really easy. However, 11.0 is almost out.

* New packages filter

This shouldn't be impossible, but I don't have an idea on how to do it. I think we don't have the information on when a package appears in a repository, and even worse, we don't keep information about the previous refresh when finishing one.

John Tomas asks and suggests an easier way to [remove applications][1]. I think the use case is valid, but I don't like the [exact proposal][2], specifically, I don't like user interfaces with trees. And adding more check boxes for repo removal clutters the interface more. Moreover, I think this usecase is just part of a more generic one, how to install applications, without worrying about "packages". This problem is already being solved by, for example, Ubuntu Appinstaller.

This problem could be solved in various way.

* Packages could be "tagged" as such.

However that requires changing the current world, therefore it won't work.

* Do a .desktop file -\> package mapping.

Ubuntu appinstaller does this by scanning the complete distribution archive, and generating special metadata from it. This metadata is distributed as a package called appinstaller-data. This method is sane, but the approach of scanning the distribution archive won't work with 3rd party repositories.

Stephan Binner has a [prototype for the same][4] concept on openSUSE, called app-installer.

Any other ideas on how to solve this problem?

[1]: http://opensuse.awardspace.com/#remove  
 [2]: http://opensuse.awardspace.com/treeview.png  
 [3]: http://www.raulmoratalla.com/2008/05/probando-caractersticas-de-opensuse-11.html  
 [4]: http://download.opensuse.org/repositories/home:/Beineri/Factory  
 [5]: http://www.raulmoratalla.com

