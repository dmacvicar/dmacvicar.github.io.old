---
layout: post
title: On upgrading openSUSE with 3rd party repositories
date: '2009-10-26'
tags:
- newsuse
- software
- suse
---

With the [availability of up to date software](http://software.opensuse.org/search) for openSUSE, thanks to the amazing [build service](http://build.opensuse.org/), our users have started managing the software in ways we did not imagined some years ago.

## Update? Upgrade?

This has created some confusion between operations like update and upgrade, package vendors and the package selector functionality. This new situation plus the confusion leads to [bug 548551](http://bugzilla.novell.com/548551)

When you install a package, it has a vendor. Official packages have the “openSUSE” vendor. While packman packages vendor is something like “Packman Project”, and packages from the [build service](http://build.opensuse.org/), have a vendor based on the project name, like “OBS KDE Project”.

Updating your system means looking for the latest package of the same vendor that can be installed without doing destructive operations to your system, like uninstalling another package due to a conflict. This is the reason for behaviors like:

- “zypper up” does not pick latest KDE packages even if the repository is added
- An official update (same vendor) for SoftwareFoo is available, but because your custom libSomething packages conflict with it, the update is not applied.
- However, if you manually switch to the packman package, “zypper up” will upgrade from now on using new versions of the package in the packman repository, and therefore ignoring the openSUSE one.

This behavior is expected as we don’t consider a package with the same name to be the same package across vendors. A packman package may include mp3 support and the one in the openSUSE repo may not. If packman updates this package every monday, and openSUSE does every Friday, “zypper up” would end with your application having mp3 support depending on the day you are upgrading.

## Bleeding edge repositories

If you are using a distribution like openSUSE Factory, packages are removed, split, renamed and aggressively replaced by new versions. This makes moving from a openSUSE Factory installation on a given day, to the state of the repository some time later a process that may require more aggressive operations, like downgrading packages or removing others, in order to reach the goal of moving to the second point.

As we explained above, the update operation won’t do this. The right way to do it is to perform a distribution upgrade. People using “zypper dup” are doing exactly this. This operation will move your system to the versions of the available packages. This may actually mean downgrading and removing installed packages. The recommended way to do this, is to perform it against a specific repository. This helps narrowing the “goal” further than telling the solver to do a “wide” distribution upgrade. This is what “zypper dup –from repo” should do.

If you intend to use “zypper dup” without an specific repository. You need to define repository priorities if you don’t want to encounter some surprises. For example you could set packman to a higher priority than openSUSE Factory so every Factory update does not remove your mp3 support.

## What was missing?

Part of the confusion, and the difficulties to follow repositories with latest versions of big components (like KDE) on top of a stable release (like 11.1) comes from two sides:

- “zypper dup –from” was broken, and therefore performing a “wide” distribution upgrade ( [bug #549490](http://bugzilla.novell.com/549490)), which is fixed now (1.2.7).
- YaST package selector had no support for performing upgrade operations. This is available starting yast2-qt-pkg 2.18.18.

Upgrade operations are requests to the solver. When selecting packages by selecting the latest one manually (what “upgrade all in this list” did) you are not necessarily selecting the one the solver will select. Therefore such operations are best represented as “requests”, which can be combined with manual user selection/removal of other packages.

When browsing the packages in the repository view, you will see a notification that allows to insert a request to the solver to switch all installed packages to the versions of this repository.

 ![]({{ site.baseurl }}/assets/upgrade-repo-1-300x137.png)

Once you file the request, you will see the list of repository upgrade requests. You can cancel any of these.

 ![]({{ site.baseurl }}/assets/upgrade-repo-2-300x129.png)

And finally, you can do this in multiple repositories (equivalent to passing –from multiple times to zypper).

 ![]({{ site.baseurl }}/assets/upgrade-repo-3-300x132.png)

I hope this post made more clear how this operations work. The changes described above should be available in Factory soon. And may be we need to think what to do with legacy functionality like “upgrade all in this list”.

You can use the new functionality to stay current with your favorite desktop environment community repositories, or why not, to stay current with Factory.

