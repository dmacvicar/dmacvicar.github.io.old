---
layout: post
title: Solving the famous “smart” case 3
date: '2008-05-17'
tags:
- smart
- yast
- yum
- zypp
---

In my [last post][1], I showed how sat-solver (solver's ZYpp uses) would solve correctly the "case 2" included in [smart's README][5], an exercise smart uses to claim its "smartness" (because apt and yum can't handle them).

How does it with case 3?

> That's another interesting case which was tested with APT-RPM and YUM.  
>   
> In this case, there's a package `A` version 1.0 installed in the  
> system, and there are two versions available for upgrading: 1.5 and 2.0.  
> Version 1.5 may be installed without problems, but version 2.0 has a  
> dependency on `B`, which is not available anywhere.  
>   
> In this case, the best possibility is upgrading to 1.5, since upgrading  
> to 2.0 is not an option.

Here both yum and apt fail:

> Just like APT, YUM selected version 2.0 and didn't consider the  
> availability of an intermediate version.

But smart does the right thing:

> Smart correctly selects the intermediate version 1.5, which is the  
> only viable possibility given the current options.

So, we setup a repository [packages.xml][2] with A version 1.5 and 2.0 ( 2.0 requiring an non-existing B) and a [system.xml][3] representing the installed packages, with only A 1.0 installed. We put all together in a [test.xml][4] file, and select a "upgrade" action.

We run the already introduced deptestomatic:

```
# deptestomatic test.xml
>!> Solution #1:
>!> upgrade A-1.0-1.noarch => A-1.5-1.noarch[test]
>!> !unflag A-2.0-1.noarch[test]
>!> installs=0, upgrades=1, uninstalls=0
```

And we confirm, that ZYpp (sat-solver) would also select A 1.5 as expected.

[1]: http://duncan.mac-vicar.com/blog/archives/310  
 [2]: http://pastebin.com/f1811aef8  
 [3]: http://pastebin.com/f49446f8b  
 [4]: http://pastebin.com/f2658140e  
 [5]: http://svn.labix.org/smart/trunk/README

