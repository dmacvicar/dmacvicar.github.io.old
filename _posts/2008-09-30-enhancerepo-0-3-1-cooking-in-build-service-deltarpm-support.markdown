---
layout: post
title: 'enhancerepo 0.3.1 cooking in build service: deltarpm support'
date: '2008-09-30'
tags:
- enhancerepo
- newsuse
- rpmmd
- software
- suse
- yum
- zypp
---

A new version of [enhancerepo][1] (0.3.1) is cooking itself in [the build service][2].

The new feature is deltarpm metadata generation support and also some kind of smart deltarpm generation.

This means, enhancerepo can look which package has several versions in the repository, and generate delta rpms for N steps to the older versions. The default is one step, that is, only a delta rpm to the newest to the previous one. And then it can generate the metadata for you too (and add it to the index and such).

Example run:

```
# enhancerepo --disk-usage --keywords --eulas --create-deltas 2 --deltas -- /space/repo/duncan2
Adding eula: /space/repo/duncan2/zapping-0.9.6-72.eula to zapping-0.9.6-72-i586
Adding eula: /space/repo/duncan2/zaptel.eula to zaptel-1.2.10-70-i586
Adding keyword: /space/repo/duncan2/zaptel-debuginfo.keywords to zaptel-debuginfo-1.2.10-70-i586
Preparing disk usage...
Creating delta - amarok-1.4.10-3-i586 -> amarok-1.4.10-17-i586 (1/2)
Creating delta - amarok-1.4.9.1-53-i586 -> amarok-1.4.10-17-i586 (2/2)
Saving /space/repo/duncan2/repodata/susedata.xml.gz ..
Adding /space/repo/duncan2/repodata/susedata.xml.gz to /space/repo/duncan2/repodata/repomd.xml index
repodata/susedata.xml.gz already exists. Replacing.
Saving /space/repo/duncan2/repodata/deltainfo.xml.gz ..
Adding /space/repo/duncan2/repodata/deltainfo.xml.gz to /space/repo/duncan2/repodata/repomd.xml index
repodata/deltainfo.xml.gz already exists. Replacing.
Saving /space/repo/duncan2/repodata/repomd.xml ..
```

Some important notes:

* It now requires ruby-rpm, which is available on the [devel:languages:ruby:extensions][3] repository.  
* Be careful with running createrepo on top of a directory with deltarpms. createrepo will index them incorrectly as packages (So clean deltarpms, run createrepo, and then generate deltas with enhancerepo on top).  
* I did not test this release as much as the latest :-)

[1]: http://en.opensuse.org/Enhancerepo  
 [2]: http://software.opensuse.org/search?q=enhancerepo  
 [3]: http://software.opensuse.org/search?q=ruby-rpm

