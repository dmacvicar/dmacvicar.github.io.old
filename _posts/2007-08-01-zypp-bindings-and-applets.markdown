---
layout: post
title: ZYpp, bindings and applets.
date: '2007-08-01'
tags:
- newkde
- newsuse
- suse
---

ZYpp

We are close to release the last alpha of 10.3. This will be the second one containing a much improved package management stack.

In the last weeks we have been squashing bugs. Most of them show up when we integrated all the pieces together (like YaST and the installation ).

A couple of small features are now showing up. The ability to use variables in repository urls is now in. Only $arch and $basearch are implemented in svn right now. This will allow for easier repository maintenance. We will try to implement all relevant variables for SUSE Linux specific features, but our strategy is to provide maximum compatibility with other tools like YaST and smart, so expect their relevant variables to be there too.

Bindings

Lot of time ago, I created a prototype of ruby bindings for libzypp. Klaus improved them a lot. After the refactoring, it was all the time more difficult to catch up with the changes using hand written binding code.

Time later I tried a new approach and tried with [SWIG][2]. What seemed impossible at the beginning later turned more real, when I quickly was able to run the old bindings [simple examples][4]. Most of the trick was about feeding "clean" interfaces to SWIG, which usually got confused with all the black magic and bleeding edge C++ techniques used in the library.

The last weeks, [Arvin][5] has been giving them lot of love. My first woooho! was when he created the [python subdirectory][3] in examples/. Not that I like python. But having ZYpp running in the most popular languages will be a nice added value. Other languages will come as we find people mastering the SWIG typemaps for those.

Applets

Other application that has got tons of love for 10.3 is the [openSUSE updater applet collection][12]. Well, the collection is just 2. If you remember, this applet has its origin in a [Google SOC Project][7] by Narayan Newton, which I hacked later to provide a backend using libzypp. Later [Jano][9] introduced the [Gnome version][10].

Now we have versions for both desktops. Before jumping into the libzypp refactoring I tweaked the [KDE version][11] to use [kparts][8], KDE's component technology, for the backends. [Thomas][6] has been working on it since then, fixing most bugs and giving it a fresh and polished look. On the other desktop, Joerg has been enhancing the Gnome version as well. Both are improving the experience of installing updates, with some advice from [the UX team][13].

[1]: http://duncan.mac-vicar.com/blog/archives/254  
 [2]: http://www.swig.org/  
 [3]: http://svn.opensuse.org/svn/zypp/trunk/libzypp-bindings/examples/python/  
 [4]: http://svn.opensuse.org/svn/zypp/trunk/libzypp-bindings/examples/ruby  
 [5]: http://arvin.schnell-web.net/  
 [6]: http://en.opensuse.org/User:Tgoettlicher  
 [7]: http://theopenstandard.com/blogs/raven/index.php?/archives/15-A-Change-In-The-Wind.html  
 [8]: http://techbase.kde.org/Development/Architecture/KDE3/KParts  
 [9]: http://en.opensuse.org/User:Jkupec  
 [10]: http://en.opensuse.org/GNOME_Updater_Applet  
 [11]: http://en.opensuse.org/KDE_Updater_Applet  
 [12]: http://en.opensuse.org/Updater_Applet  
 [13]: http://en.opensuse.org/UX

