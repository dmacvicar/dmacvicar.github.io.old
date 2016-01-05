---
layout: post
title: Solving the famous "smart" case 2
date: '2008-05-17'
tags:
- newsuse
- smart
- software
- suse
- yast
- yum
- zypp
---

In my [recent post about ZYpp, yum and smart][2] speed and memory usage, [I got this comment][3]:

> It would be also interesting to know how the new libzypp manages the “Case Studies” from http://svn.labix.org/smart/trunk/README][4]  
>   
> I don’t know, but could be that yum fixed “Case 2″ and libzypp “fails”? So the extra time and memory over libzypp taken by yum to make the dep resolution would be justified.

At first glance I thought the case was very vague, but actually it was really simple and concrete, so it was a good candidate to be tested with our test suite tools. Let's look at it:

> This is another real case, and is being reproduced in a controlled  
> environment for tests with YUM, APT-RPM, and Smart.  
>   
> The issue is, a package named `A` requires package `BCD` explicitly, and  
> RPM detects implicit dependencies between `A` and `libB`, `libC`, and `libD`.  
> Package `BCD` provides `libB`, `libC`, and `libD`, but additionally there  
> is a package `B` providing `libB`, a package `C` providing `libC`, and  
> a package `D` providing `libD`.  
>   
> In other words, there's a package `A` which requires four different symbols,  
> and one of these symbols is provided by a single package `BCD`, which happens  
> to provide all symbols needed by `A`. There are also packages `B`, `C`, and `D`,  
> that provide some of the symbols required by `A`, but can't satisfy all  
> dependencies without `BCD`.

So, what is so special about this case?

> The expected behavior for an operation asking to install `A` is obviously  
> selecting `BCD` to satisfy `A`'s dependencies, on the other hand, `YUM` and  
> APT fail to deliver that as a guaranteed operation, as is shown  
> below.

Ok, there are a couple of things that must be said about this example.

* The dependencies of the package A are broken. Unless the package A depends on the libraries libA, libB and libC and some runtime "thing" provided by it, then BCD should be split in three packages. But as none of B, C, D provides any runtime "thing", but they just "replace" it, this is not true. So either fix BCD or fix A to depend on the libraries only, the explicit BCD dependency is bogus.

* Debian's apt-get won't succeed here, because AFAIK Debian only has dependencies across packages, not on libraries. Actually I still wonder how smart could succeed here using a debian backend. I ignore if apt-rpm does support library dependencies.

* The result yum produces makes no sense too.

* So yes, this is the expected result, but the example is a really dumb package.

How to simulate dependencies using the ZYpp sat solver?. The magic is call deptestomatic. There are two versions of this tool, one provided by satsolver-tools, and another one provided by libzypp-testsuite. The one in sat-solver tools should be sufficient. The one in libzypp-testsuite has extra features like graphical display of dependencies.

So, we use the helix format for testcases, which was the format of the original RedCarpet testuite. A testcase is a xml file refering to one or more xml files describing repositories, and one repository represents the installed packages (in this case we can simulate with no installed packages and just one repository).

We start by creating a [packages.xml][6].

And now, we define the [test.xml][7] describing the testcase.

If there are conflicts, the xml language has features to select solutions, but that is advanced stuff [documented here][5]. For our case, the test is really, simple, just install A.

Now run the test:

```
# deptestomatic test.xml
```

And you get the result:

```
>!> Installing A from channel test
>!> Solution #1:
>!> install A-2.0-1.noarch[test]
>!> install BCD-2.0-1.noarch[test]
>!> installs=2, upgrades=0, uninstalls=0
```

Which is the "smart" result. It only installs A and BCD, and not B, C and D (which is what yum does).

Learning to develop testcases and run them is an excellent way to help the development team to fix bugs without having to request extra information, so you can play yourself with the simulation before actually reporting it.

You can also generate testcases for day to day operations in a very simple way (no need to write xml):

* in the YaST (Qt) package selector, go to Extras, and choose generate solver testcase (after selecting your transactions, like installing or removing packages).  
* Use zypper command --debug-solver (like zypper install --debug-solver foo )

Then you can run them with deptestomatic by referencing the test xml file. Note that you can reuse package lists (repositories) across tests. libzypp-testsuite and the sat-solver source includes a big list of testcases representing simple operations, distributions, upgrades, and distribution upgrades.

[1]: http://labix.org/smart  
 [2]: http://duncan.mac-vicar.com/blog/archives/309  
 [3]: http://duncan.mac-vicar.com/blog/archives/309#comment-96588  
 [4]: http://svn.labix.org/smart/trunk/README  
 [5]: http://en.opensuse.org/Libzypp/Testsuite_solver  
 [6]: http://pastebin.com/f4117ec82  
 [7]: http://pastebin.com/f6add727c

