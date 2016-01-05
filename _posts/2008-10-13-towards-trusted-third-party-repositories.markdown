---
layout: post
title: Towards trusted third party repositories
date: '2008-10-13'
tags:
- newsuse
- software
- suse
- yum
- zypp
---

I got pointed to [Dan's Kegel post on Linux Foundation's packaging mailing list][1] called "Towards trusted third party repositories". I am not subscribed to the list so I am commenting here.

> I've been intrigued by http://en.opensuse.org/Standards/One_Click_Install  
> for some time now. (That's a way to provide a one-click web  
> install experience for .deb/.rpm/.psi etc. packages, implemented  
> as a mime type handler that parses a simple .xml file pointing  
> to the package/repository appropriate for each distro.)  
> When this idea was brought up on the Packagekit mailing  
> list, it generated lots of negative feedback. The summary at  
>  http://packagekit.org/pk-faq.html#1-click-install  
> gives a bunch of non-central objections, followed by  
> the central objection that one cannot trust third party repositories:  
> "Allowing to easily add third party repositories and install  
> third party software without a certification infrastructure is like  
> opening the gates to hell"

One Click Install is only a simple description to add a repository and some software, but the security is not a property of this simple description, but of the package manager. A malicious one click install file can point ZYpp to "hell", but "hell:

* Either is signed with a non trusted key and the user needs to trust it.  
* Is signed with the distribution key, which is already in the trusted keyring therefore it just works.

> This is a real problem. Here are a couple risks:

No, it is not. It depends on the package manager you are using. Basic security here is independent of one click install.

> 1) users might click on malware sites and add  
> completely malicious sites to their repository lists

Again, this is not true for ZYpp, and I guess not all package managers allows to download metadata from a repository without trusting it first.

> 2) a compromised third-party repository  
> might update system packages maliciously.

If you trusted it, there is nothing you can do.

> 3) several genuinely well-intentioned  
> repositories might include conflicting versions of  
> a commonly needed package not provided by the  
> system repositories.

This has nothing to do with one click install. Packman repository does it already. This is a dependency resolving problem and vendor management: ZYpp for example, does not allow implicit vendor jump on update (only on dist-upgrade). And vendor jump is a conflict you need manually to approve. Only package managers that threat all packages with the same name as the same package suffer here.

> After mulling this problem over for a long time,  
> two ideas came to mind:  
> 1) Since the distribution is trusted, it could decide  
> to trust some third-party repositories. For instance,  
> it might decide to trust Adobe's hypothetical  
> repository so that people could get flash and air  
> updates straight from the source.

This is possible now:

* You can import Adobe's key into the distribution together with the distribution key, so it goes automatically to the trusted keyring  
* You can create metadata in the distribution side, linking to Adobe's packages, and sign the metadata with the distribution key  
* You can do nothing and let the user trust explicitly the repository

> This idea of using the distribution as arbiter of  
> trust for third party repositories could be extended  
> to games publishers, etc. This could provide a  
> partial solution to the first threat listed above;  
> if the "good" third-party repositories are already  
> known to the distribution, there's less need for users  
> to be doing something dangerous like deciding  
> on their own to trust a random third-party repo.  
> This addresses the first threat identified above.

I think repository trusting and the required package manager infrastructure has nothing to do with one click install. It just needs to be there.

> 2) A simple way to keep repositories from  
> updating packages they shouldn't is to  
> have package managers enforce some sort  
> of namespacing. e.g. Adobe's repository  
> could be allowed to only update packages  
> whose names start with "adobe-".  
> (System repositories would continue to be  
> able to update any package at all.)  
> This addresses the second and third threats  
> identified above.

Why inventing a new thing? You already have the vendor tag there. A package should be considered update candidate only for packages that are from the same vendor.

Imagine you have mediaplayer-1.0 from the distribution, without mp3 support. Then a 3rd party repo offers a compiled 2.0 version with mp3 support but without other features, and then the distribution offers 3.0. You would be getting and losing features all the time as you update. Same package names does not means it is the same package. If your package manager does that, it is a bug. If there is no solution to the dependency solving than jumping vendor, do a conflict and make the user explicitly switch. Once he does, the update candidate would be the same vendor packages.

> I think something like this is going to be needed  
> before we can have a thriving -- and safe --  
> ecosystem of ISVs providing  
> easily-downloaded-and-installed binary packages  
> for Linux.  
> What do people think about the package namespacing  
> idea?  
> - Dan

I don't think reinventing the vendor tag and repository signatures is a good idea.

The problem is not at this level, IMHO the only thing we can do is improve the usability of the security chain. For the user is meanless to trust a repository with gpg key 0x32432432 and they will probably just click "yes". The pending task is to make the cryptographic chain something that makes sense for the user.

[1]: https://lists.linux-foundation.org/pipermail/packaging/2008-October/000842.html  
[2]: http://en.opensuse.org/Standards/One_Click_Install

