---
layout: post
title: What is with rpm-metadata (createrepo) and yum on openSUSE?
date: '2009-10-16'
tags:
- newsuse
- software
- suse
---

Long time ago, I wrote about [our interoperability efforts](http://duncan.mac-vicar.com/blog/archives/314) built around [rpm-metadata](http://lists.baseurl.org/mailman/listinfo/rpm-metadata) format and first-class PackageKit support.

On the rpm-metadata side however, even if we depend a lot on these tools, the situation was far from ideal:

- We need usually extensions to the rpm-metadata to support the features that make the openSUSE software management more powerful compared to other tools
- We are in continuous talk with the [yum](http://yum.baseurl.org/) team to make those extensions common so they can be standardized instead of staying in suseinfo/susedata.xml
- Some of those extensions got implemented, in [createrepo](http://createrepo.baseurl.org/) 0.9.x
- We are stuck with [createrepo](http://createrepo.baseurl.org/) 0.4.x plus a high stack of patches
- [createrepo](http://createrepo.baseurl.org/) 0.9.x requires a recent [yum](http://yum.baseurl.org/) 
- yum on openSUSE is unmaintained, and not included in the distribution
- [openSUSE Build Service](http://build.opensuse.org/) and other infrastructure depend on a proven createrepo (which means they depend on the custom patches)

This situation won’t sustain in the long term, so the following action plan was agreed between various stakeholders:

- we will update yum and [createrepo](http://createrepo.baseurl.org/) to the upstream latest versions, and maintain them in [the system:packagemanager] project
- all patches except a couple were discarded, so don’t expect those versions to work flawlessly for now, we will reevaluate them one by one, upstream them or discard them
- [openSUSE](http://www.opensuse.org/) infrastructure will freeze their production version from our project once the newer versions work, instead of maintaining a fork
- as we don’t want to include yum in the default package selection, but [createrepo](http://createrepo.baseurl.org/) depends on it, yum was split in yum and yum-common (libraries).
- yum is still available in the [system:packagemanager](https://build.opensuse.org/project/show?project=system%3Apackagemanager), and will be kept up-to-date. We are interested in competition, as it makes the [ZYpp][10] team work harder
 ![]({{ site.baseurl }}/assets/images/posts/icon_smile.gif)
- I will improve [enhancerepo](http://en.opensuse.org/Enhancerepo) (for custom tag extensions generation) and include it in this project in a near future
- Most patches in the current packages were one liner fixes where the developer did not spent 5 minutes to upstream it. This attitude created a uncomfortable patch mess. We will change this by upstreaming fixes or just rejecting having those patches in the package.

If you use [createrepo](http://createrepo.baseurl.org/) or yum in your infrastructure, we invite you to contribute to this project! [yum-3.2.25 was released](http://yum.baseurl.org/wiki/whatsnew/3.2.25) a couple of days ago, and it is already available there.

[10]: http://en.opensuse.org/Libzypp

