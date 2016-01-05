---
layout: post
title: Another impressed developer
date: '2006-06-30'
tags:
- newkde
- newsuse
- suse
---

As you may know, I am mentoring Narayan Newton's [KDE based OpenSUSE updater applet][2] for [Google Summer of Code][1].

Narayan has most of the pieces working, the project target, updating using zmd is working, and he designed it to support other backends, that will come later.

In his [last post][3] on his blog he states:

>  This project has forced me to dig around kdelibs multiple times and I have to say, I am very impressed. I have developed for both GNOME and KDE. I am no zealot. However, the breadth and clarity of kdelibs is without a doubt one of the most impressive things I have seen. On Monday, I got up in the morning thinking I would spend an hour adding support for some sort of configuration db to the updater, so a user could save the updater interval and switch between backends. It took me fifteen minutes to add that support. I was flabbergasted.

Many people think integration comes from the fact using the same toolkit to get the same look. Because there are frameworks which provide little more than a toolkit and some extra functions.

When you see the KDE framework and it technologies like KParts, KIO, KConfigXT, etc. You realize you are not using the API just to get the look, but you are reusing lot of components you will not need to maintain yourself, components which probably saved you hours of coding and which are common to all applications. Then you can concentrate in your own business. The "common" layer makes the magic for you and integrations happens at a much deeper level. Then you realize why KDE apps not only look but feel the same, without having to rewrite the same code once per application.

KDE is the most advanced framework for developing applications out there. And it is nice when you see a developer falling in love with it after giving it a look.

If you know every developer will fall in love after trying developing with the KDE framework, the strategy is clear. Just make than chance to happen.

[1]: http://code.google.com/soc/  
 [2]: http://en.opensuse.org/KDE_Updater_Applet  
 [3]: http://madpenguin.org/blogs/raven/index.php?/archives/12-In-Praise-Of-Kdelibs.html

