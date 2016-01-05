---
layout: post
title: package management in openSUSE 10.3
date: '2007-07-19'
tags:
- newsuse
- suse
---

The next alpha6 of openSUSE 10.3 will contain lot of changes in the package management code.

I will try to summarize what is new so you can help us testing. The integration can bring some regressions, so please test it and help us to fix the remaining bugs during the betas.

What were the goals:

* no more refresh and parsing during startup.  
* more compatibility with tools like yum and smart  
* more speed for the common use case: install a package

Before ZYpp did not make difference between launching the sources and refreshing them.  
This was quite annoying, and turned into needing network access for any operation requiring launching the sources first, like deleting, adding, searching.

How does it work now?

* Repositories now have a unique (in your system) alias.

* Adding a repository is just a matter of copying [a .repo file][1] (like those provided by the build service) to /etc/zypp/repos.d directory. If you use zypper or YaST it will create them for you, and perform additional checks.

* Download of metadata happens during refresh. Also in this operation a binary cache is created.

* deleting a repository is just a matter of deleting its .repo file, or doing it via zypper/YaST (they take additional checks like vacuum the cache).

* YaST was just adapted and is still WIP, so in some places the old semantics could be still used (like refreshing repos even if it is not needed).

* The binary cache allow to start up zypper and install packages in a much faster way. Caching the metadata is slow. But you do it once per refresh, and the data is stored preparsed in the cache. This allows to launch small repos in fraction of a second, and big repos in seconds later. No more parsing files in each startup.

* Before lot of memory was used because most of the GUI data (descriptions, etc) was read into memory. Now all attributes (except resolvable information, for which we need indexes) is loaded on demand from the cache.

* refresh operation uses now much less memory. One one object is kept in memory at time while writing the cache.

For the developers:

* Lot of testcases. We can do more aggressive development without breaking basic stuff.  
* Lot of redesign, to be able to implement new features.  
* We build with cmake now

You expect for the future (post 10.3):

* Integrate search with the cache, so no need to launch the resolvables and loop for searching  
* solving some queries from the cache?

[1]: http://en.opensuse.org/Standards/RepoInfo

