---
layout: post
title: Hackweek III (day 4)
date: '2007-06-28'
tags:
- newsuse
- ruby
- suse
---

Martin Vidner succeeded in [making possible to use YaST UI:: from the language bindings][1]. This means you could write a complete module in Perl, and get the benefits of the abstracted Qt/ncurses/gtk user interface.

These changes means that my Ruby bindings will be able to support it too.

I also got the code as yast2-bindings-ruby in the build service, it doesn't build yet, but soon will appear in the home:dmacvicar project.

Srinivasa Ragavan is working on [Desktop status awareness][2], which means trying to do useful tasks when you are away from your computer. I pointed him to the [Kopete motion-auto-away][3] plugin I hacked some years ago in order to set the user away in the messaging client if no motion was detected in a webcam stream.

[1]: http://mvidner.blogspot.com/2007/06/yast-user-interface-library-useable.html  
 [2]: http://idea.opensuse.org/content/ideas/desktop-status-awareness  
 [3]: http://websvn.kde.org/trunk/KDE/kdenetwork/kopete/plugins/motionautoaway/

