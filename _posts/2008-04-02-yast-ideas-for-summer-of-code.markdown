---
layout: post
title: YaST ideas for Summer of Code
date: '2008-04-02'
tags:
- cmake
- english
- google
- newsuse
- soc
- suse
- yast
---

Because the Google Summer of Code deadline was extended, I just added two projects I consider interesting for YaST:

###Automatic generation of YCP bindings

Right now, if you want to access low level libraries from YCP code, you have two options:

* Create manual bindings, may be with help of /usr/share/YaST2/data/devtools/bin/generateYCPWrappers script. (used by package-bindings and libyui ycp bindings )  
* Wrap your library using swig, and then generate perl bindings from that, and then use the module using YaST perl bindings (libstorage is accessed this way).

This problem could be solved in various ways:

* Create a [SWIG][4] module that allows ycp glue as output.

This approach would get all the swig parsing magic for free.

* Extend generateYCPWrappers so it could wrap any C library in a easy and automatic way. C++ is not required, as YCP is not object oriented.

Expected result:

Get package bindings or libstorage output which don't depend on perl bindings and are automaticaly updated if the target API changes.

* Required knowledge: C++, YaST2, languages  
* Skill level: medium  
* Willing mentors: me!

### Build YaST using [cmake][3]

YaST builds using autotools and a automated layer of scripts over that (called y2conf, y2make, etc).

Some modules as libzypp and friends, yast2-qt and ruby bindings build using cmake.

It would be great to build complete YaST using cmake. This would bring benefits like:

* Access to the CTest test framework  
* out of the box out of source builds  
* Easier and more intuitive.

This task means:

* making yast2-core to compile using cmake  
* create devtools skeletons for modules and agents using cmake  
* add support for devtools utilities which may break  
* port all modules. (porting 3 would be a challenge, the others is mostly mechanical work once core and some problematic ones compile).

Automating the conversion of modules would also be a goal.

* Required knowledge: cmake, building, packaging  
* Skill level: easy  
* Willing mentors: me!

If these projects aren't for you, there are [more ideas][2] in the wiki page. Check it out!.

[1]: http://groups.google.com/group/google-summer-of-code-announce/browse_thread/thread/9fa88f31aa401f70  
 [2]: http://en.opensuse.org/Summer_of_Code_2008#YaST_ideas  
 [3]: http://www.cmake.org  
 [4]: http://www.swig.org

