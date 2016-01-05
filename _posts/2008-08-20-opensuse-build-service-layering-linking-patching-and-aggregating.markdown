---
layout: post
title: 'openSUSE Build service : layering, linking, patching and aggregating'
date: '2008-08-20'
tags:
- buildservice
- newsuse
- rpm
- software
- suse
---

Today I used some of the coolest [openSUSE Build Service][8] features: project layering, patches against linked packages and aggregates. I want to write about them.

I needed to test a feature in PackageKit. I am using openSUSE 11.0. However upstream PackageKit does not play nice with 11.0. [Stefan Haas][9] fixed this, taking our PackageKit sources, adding some patches, and building them in his [home project][2], which results in a repository [here][5].

My feature involved a patch against PackageKit and then testing it from a client application (to adapt it), so what I wanted to achieve was to use Stefan's PackageKit plus my patch, packaged in a rpm.

I could just copy Stefan's sources and patches to my home project, and add my patch, but that would be duplication. Any change Stefan does later would require to copy it again.

Luckily the build service has a feature call linking, so I can link Stefan's package to my home project. You can do that from osc or from the web user interface (Link package from another project), and that would result in a new package called PackageKit in my project (I created a subproject home:dmacvicar:packagekit for this purpose) with a single file called \_link.

This version of PackageKit only builds with Factory's libzypp, so here we have various options:

* build on top of zypp:svn/openSUSE\_11.0 which is ZYpp svn built on openSUSE\_11.0  
* link all [zypp:svn][3] packages to my project (too inefficient, Adrian would kill me if I start to rebuild ZYpp in every project :-) )  
* aggregate all ZYpp openSUSE\_11.0 packages from [zypp:svn][3] in my project (aggregate explained later)

Because I already have the [repo zypp:svn][4] in my repo list, I chose building on top of [zypp:svn][3]. (This will mean that in order to use this modified PackageKit repo, you need to add [zypp:svn][3] as well )

So now, I can add my patch next to the \_link file. However, How to make this patch appear in Stefan's specfile? Do we need to patch the spec file too? No. The build service does it for you. Just edit the \_link file which looks like this:

So it looks like:

This will insert the patch in the spec file when building. That is real magic.

Sadly, this bleeding edge PackageKit version requires some libtar-devel not available in 11.0. Stefan had a [backport to 11.0 in his home project][1] too. Do we need to link the package to our project too? No. Linking here is unnecessary because we don't really need to rebuild a copy of this package, we only need to make it available in our repository for people installing PackageKit from our project, who don't have Stefan's project added in their repository list, that will get libtar grabbed via dependencies, but we don't care if the metadata grabs the final rpm from Stefan's project.

That is called "aggregate". To aggregate a package from another project, we create a new package (called libtar) and add to it a file called "\_aggregate" (the web interface does not help you here). Edit that file so it looks like:

libtar  
 libtar-devel

This will map home:haass repositories 1:1 to our repositories. So if we build our project against openSUSE\_11.0, it will aggregate our package with home:haass/openSUSE\_11.0 too. If you don't have a 1:1 mapping, look the [Build Service Tips and Tricks][6] to do more complex aggregates.

Then I am ready to install my new PackageKit:

sudo zypper in -r home\_dmacvicar\_packagekit PackageKit PackageKit-devel  
 Reading installed packages...

The following NEW packages are going to be installed:  
 PackageKit-devel PackageKit

Overall download size: 1.1 M. After the operation, additional 5.1 M will be used.  
 Continue? [YES/no]: y

[1]: https://build.opensuse.org/package/show?package=libtar&project=home%3Ahaass  
[2]: https://build.opensuse.org/package/show?package=PackageKit&project=home%3Ahaass  
[3]: https://build.opensuse.org/project/show?project=zypp%3Asvn  
[4]: http://download.opensuse.org/repositories/zypp:/svn/openSUSE_11.0/  
[5]: http://download.opensuse.org/repositories/home:/haass/openSUSE_11.0/  
[6]: http://en.opensuse.org/Build_Service/Tips_and_Tricks#Example_of_an__aggregate  
[7]: https://build.opensuse.org/project/show?project=home%3Ahaass  
[8]: http://build.opensuse.org  
[9]: http://en.opensuse.org/User:Haass

