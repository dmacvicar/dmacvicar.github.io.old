---
layout: post
title: The greatest unknown openSUSE 11.0 package management feature
date: '2008-05-21'
tags:
- fedora
- newsuse
- suse
- yast
- yum
- zypp
---

During the development of openSUSE 11.0, we have been reporting in real time cool improvements like the [fast installation][4], [how YaST became sexy][5], [how YaST/ZYpp/zypper became fast][1], [how YaST/ZYpp/zypper performs better than others][2] and even [that our solver is also really smart][3].

However, there is something else...

## Interoperability

 ![]({{ site.baseurl }}/assets/images/posts/media_httpimg232image_yfnpi-scaled500.jpg)

### Background

One of the features of our stack is the availability of patches and patterns. The first provide updates in a sense of "fix for a problem" (which can mean various, or none updated packages), while patterns are intelligent groups that can recommend, require and suggest packages in order to make certain functionality available, without being too strict in the specific packages to install.

Unlike in other systems where groups and updates are handled as special entities, ZYpp patterns and patches are just objects with dependencies like packages, and the solver threats them in the same way.

Because rpm installed packages database does not know about patterns and patches, in openSUSE 10.x (and SLE10) those objects are installed in a separate database, only viewable to libzypp. This is hidden to the users, but does not allow for easy management using 3rd party tools.

In addition to that, the patch metadata format is our own extension to the [metadata][7] handled by [yum][6], the tool used by Fedora and Centos. That means, even if other distribution provide similar concepts, they will mostly ignore our extended metadata.

This is sad, if we share 90% of the metadata format, why not go further?. Sometimes it is no worth to wait that others do steps in becoming more interoperable with you, so what about doing those steps ourselves?

At this time, Fedora was implementing updates metadata by using a yum plugin and a updateinfo.xml description. Metadata for deltarpm availability is handled via the yum-presto plugin.

### Sharing tools and data, a step for Interoperability

#### Metadata format

In openSUSE 11.0, ZYpp reads patches from updateinfo.xml too! ([check 11.0 update repo!][17]). Not only that, our delta rpm availability metadata will be in the same format [yum-presto][9] (with some modifications agreed with its author).

How will this benefit you?

* You will be able to use yum stack with updates out of the box with updates and deltarpms, and they will just work.  
* You will be able to generate custom patches for openSUSE using existing tools like [Bodhi][8], from Fedora, or generate custom deltarpm information using the yum-presto included tools.  
* We are working hard to get the [ZYpp/YaST stack to build on Fedora][10] (and other distros), in a near future, you will be able to enjoy ZYpp performance and features on Fedora with their own repositories!  
* We decoupled the delta-rpm information from patches, so we may start adding delta-rpm to normal factory packages and it will start to work out of the box!  
* Much more!

#### Handling of patches and patterns

As we mentioned, in 10.x codebase we used to install patterns and patches in a special database. This is no longer the case.

Patterns and patches are no longer installed, which means your system is rpm only! Patches are shown now as (un)satisfied (and (un)relevant). Which means you have all the requirements to consider them present.

All the information of patches and patterns (and products) is extra information that openSUSE applications use to add more value to you. So if you for example remove a repository offering patches, then you just lose the information about which patches do you have, the real information is the rpm packages you have installed. When you re-add the update repository, and you can immediately see which patches published affect you, which ones are irrelevant, and which ones are relevant but you don't need because your packages are up to date.

Patterns and patches become "advice" and "value", not extra non-compatible information.

How will this benefit you?

* Simpler system.  
* No conflicts because using 3rd party tools, "rpm by hand" or our native tools.

#### openSUSE, PackageKit enabled

[PackageKit][11] is the new actor in the package management world. It is a thin layer that provides applications access to the package management system as a [DBUS][14] service. You may heard about it because Fedora 9 is coming PackageKit enabled. How it benefits you?

* Role based (non-root) package management, via [PolicyKit][15].  
* Sharing of upstream tools across distributions.  
* Gives the desktop the chance to integrate with software manipulation.

So, openSUSE 11.0 is fully PackageKit enabled. You will be able to use all [PackageKit compatible applications][16] on openSUSE and they will use the ZYpp stack underneaths. Not only that it is enabled, but our hackers [Scott Reeves][13] and [Stefan Haas][14] did an amazing job on the backend, I would dare to say it is one of the most robust backend implementations, and it fully benefits from the ZYpp speed and features.

### Future

All this improvements are available now. May be you are already enjoying them in Factory. However this opens the door for new possibilities, just a few examples come to mind:

* The openSUSE Build Service, the great software building platform from our openSUSE team, builds packages for all major distributions since long time. The build service could allow to enter and generate patch information for fixed bugs and the update/patch information will be compatible across yum/Fedora and openSUSE. Same with deltarpm information.  
* We could extend ZYpp parser to understand Fedora groups stored in comps.xml and threat them as patterns.  
* Do you have more ideas?

### Community involvement

We welcome any help on creating more interoperability possibilities, especially about building the ZYpp stack and YaST on Fedora, Mandriva and others. There are already some packages building in the build service, but we still have a long way to go.

[1]: http://duncan.mac-vicar.com/blog/archives/296  
 [2]: http://duncan.mac-vicar.com/blog/archives/309  
 [3]: http://duncan.mac-vicar.com/blog/archives/311  
 [4]: http://www.kdedevelopers.org/node/3385  
 [5]: http://www.kdedevelopers.org/node/3143  
 [6]: http://en.wikipedia.org/wiki/Yellow_dog_Updater%2C_Modified  
 [7]: http://linux.duke.edu/projects/metadata/  
 [8]: http://fedorahosted.org/bodhi  
 [9]: http://hosted.fedoraproject.org/presto  
 [10]: http://download.opensuse.org/repositories/zypp:/Backport/Fedora_8/  
 [11]: http://www.packagekit.org/  
 [12]: http://en.opensuse.org/User:Haass  
 [13]: http://en.opensuse.org/User:Sreeves1  
 [14]: http://freedesktop.org/wiki/Software/dbus  
 [15]: http://hal.freedesktop.org/docs/PolicyKit/index.html  
 [16]: http://www.packagekit.org/pk-screenshots.html  
 [17]: http://download.opensuse.org/update/11.0

