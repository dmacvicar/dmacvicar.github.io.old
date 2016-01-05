---
layout: post
title: fast package management
date: '2008-02-07'
tags:
- newsuse
- suse
---

I haven't blogged since some time. We have been working hard to get package management stack changes in so we can have them in one of the next alphas. I wanted to make a small pause and write about it because communication is (sometimes) as important as coding ;-).

See it yourself:

[youtube [http://www.youtube.com/watch?v=XB3o4Skka5Q&rel=1](http://www.youtube.com/watch?v=XB3o4Skka5Q&rel=1)]

[video link][1].

### History

You know how was the package management stack on 10.1. On 10.3 we reached a point where we were fast enough, but with really bad performances in some cases, especially cold starts, and big lists of repositories, which is usually the usecase where the people complaining about speed are: factory users. Some of this bad performance was caused by design mistakes (like not using 1 db per repository), and others because sqlite horrible performance after fragmentation occurs.

This started sometime last year, when Michael Schröder started to show some people a [new solver][2] which was much faster than current libzypp solver. At the same time, he designed some memory and cpu efficient structures and file formats to deal with the huge amounts of metadata. The solver operated later over this optimized pool.

One of the features of the solver is its simplicity. Current libzypp operates over a dependency problem at the resolvable level while the sat solver converts the pool into a set of rules and then operates the problem as a [boolean satisfiability problem][3], which makes it codebase concentrate only on this, making it much smaller and simpler. The error messages on conflicts are supposed to be more human friendly too.

### The Road

During the YaST workshop times, [coolo][4] started to try the libzypp testsuite with the solver, by creating tools to use the existing testcases with it, and making sure it could serve as our solver.

Once coolo came and said, "ok, all testcases pass". So we met and agreed on some milestones to see the feasibility of integration with libzypp. One of the milestones was to use the new solver without replacing all the infrastructure libzypp had. That is, taking libzypp pool, converting the transactions into rules, solve, convert them back. Stefan Schubert did a great work in this area, and actually if you are using factory you are already using the sat solver.

To familiarize myself with the solver at that time, I ported it to build with cmake and implemented swig bindings (focusing on ruby ones), which later [Klaus][5] started to enhance and experiment new concepts on top of them.

There was one part the sat solver did not take into account and it was the huge amount of non-solvable data you need to have cached. It is easy to claim to be fast if you ignore 95% of the data, so Michael Matz started to enhance the sat library by designing a special attribute store for repository metadata. The final concept and implementation was submited some weeks ago and it is really fast.

During January, Michael Andres got rid of thousands of lines of code in libzypp, keeping the nice APIs but trying to build everything on top of the sat library. I ported some classes like RepoManager, and later this week the final pieces were connected when Michael connected our Pool class with the sat pool which allows to "see" the packages "world" to applications. I ported zypper and YaST package bindings and Michael Matz told us yesterday he actually installed packages using zypper.

The whole stack is now designed as unix tools, so this will allow our repositories to ship solv files, which can in turn be downloaded by libzypp. So you don't need to generate a cache locally if the solv file is already available on the server. This is not implemented yet.

There also other open issues. We don't parse translations and only the suse media parser stores attributes like descriptions. Patches and packages are not installed (but this has also another reason I will talk about it later). The package management user interface won't compile (I haven't tried) yet.

This new level of performance would allow us to bring package management on openSUSE not to the smart or yum level, but a complete new generation ahead. And this will open the door for smooth integration with CIM and PackageKit and will bring fresh air during the road to the next generation of SLES and SLED.

If you are an openSUSE contributor, we do need help with testing, and there are lot of "mechanical" jobs to do which don't require much experience with the platform (but compiling). If you want to see this in 11.0 and can contribute, please [contact us][6] .

[1]: http://www.youtube.com/watch?v=XB3o4Skka5Q  
 [2]: http://en.opensuse.org/Libzypp/Sat_Solver  
 [3]: http://en.wikipedia.org/wiki/Boolean_satisfiability_problem  
 [4]: http://www.kdedevelopers.org/blog/124  
 [5]: http://kkaempf.blogspot.com  
 [6]: http://lists.opensuse.org/zypp-devel/

