---
layout: post
title: Dependencies. Lasagna raviolis and spaghettis
date: '2006-11-09'
tags:
- newkde
---

Yesterday, while providing some info for bug [#216495][1], at some point we had to realize if it was a HAL bug, a NetworkManager bug, or a bug in the KDE or Gnome specific applets. So had to install the Gnome one, so if the bug was there too, then both applets were innocent.

So, I went to YaST and selected NetworkManager-gnome, and I fell from my chair, when I saw the packages that were going to be pulled from dependencies. Poor laptop!. Does a Gnome user needs to suffer the same if he wants to run the KDE applet?

 ![]({{ site.baseurl }}/assets/images/posts/media_httpimg83images_hakja-scaled500.png)

So, I pressed cancel. Using my zypp-debug tool, I checked the list of packages NetworkManager-kde, and NetworkManager-gnome pulled. The number is 124 for the KDE applet, and 190 for the gnome applet. This number says nothing, as there is no 1:1 equivalence in KDE and Gnome packages, so lets analyze the package lists and see how related are those packages so they end depending on them.

The first two suspicious packages in the KDE applet, are OpenEXR (because I had no idea what was it), and gnome-filesystem. But then if you go down, you will find packages that make sense, all core packages, plus kdelibs and Qt dependencies. Still looks like the applet depends on a "framework", not on applications. At the end you see X.org, and that is. The dependencies form a perfect lasagne, with the exception of gnome-filesystem, which I don't know why is there.

Now, looking at the dependencies pulled by the Gnome applet. The first dependencies are a couple of gnome libraries, fine. Then, gnome2-user-docs, ups. Why?.  
Then, gnome-menus, mmm, the applet depends on gnome-libs, but why the libraries know that menus exists in a upper layer. Then comes evolution-data-server, ok I accept this one, as it could be named gnome-libs-pim-storage as well, so part of the framework.  
The mess starts here. Next package is gnome-desktop. So the applet depends on the framework, and the framework depends on the desktop built over the framework (loop). It is followed for some lib\* packages, which again makes sense. Then comes gnome-main-menu. Uhm. Why gnome-libs would ever know somebody did a menu using the libs?. Then gnome-system-monitor, another application. Ups! gnome-panel there too.

Then I found dbus-1-mono and I raised an eyebrow. I thought "Mono is coming too.". Not big deal, only that not only mono-core was there but mono-data and mono-web. Why a framework would depend in mono-web?. I am completely sure an IDE would depend on it, but a network applet?.  
Then comes... metacity!. So the framework depends also on a specific window manager. And after finding nautilus I realized it depends on a file manager. And also on the software updater applet.

I hear often "I won't install k3b in my gnome desktop because that means pulling all the KDE crap and bloat". Yes, the bloat is Qt, kdelibs and some dependencies. Not a window manager, not a SDK+runtime, not Konqueror, not the documentation of the desktop, not the panel.

Our goal is to build a common Linux desktop strategy, moving pieces to the common grounds of the lasagne and share them. This is already happening thanks to some freedesktop technologies like dbus, NetworkManager and Qt4 and its glib event loop integration plus Gnome look and feel. But can you mix lasagne with spaghettis?.

I wanted to provide the whole dependency graph, so this can be improved. Perhaps there is only one guilty package pulling stuff it don't really needs, perhaps it is not a limitation of the platform design but a bug in some spec file. The tool in zypp/repotools is not working fine at the moment. I provide the package list on request, and I will try to post the graph soon.

[1]: https://bugzilla.novell.com/show_bug.cgi?id=216495

