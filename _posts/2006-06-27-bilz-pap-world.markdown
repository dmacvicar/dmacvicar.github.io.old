---
layout: post
title: Bilz & Pap world
date: '2006-06-27'
tags:
- open-source
---

In Chile the expression "living in Bilz & Pap world" means living in a fantasy world, far from reality. How dangerous can be for a company when the people planning their strategies live there?

Bill Hilf, general manager of competitive strategy at Microsoft made those 2 comments in [this article][1].

>  "The magic of open-source software is not the software. It has nothing to do with the code at all. Most open-source code is terribly inferior to commercial software code,"

Another missuse of the =\> and logical operators. Why you make a difference between opensource and commercial?. Why you guys \*insist\* in doing it?. MySQL code is both opensource and commercial. Your products could be opensource and commercial.

With that ilogical reasoning from Mr. Hilf, I could say, "humans are mammals, a dog is a mammal, then dogs are humans.".

Now, going to the point, quality. Let's continue with the MySQL example:

>  A study by Reasoning, the leading provider of automated software inspection (ASI) services, concluded that the code quality of MySQL ranks higher than commercial equivalents. [reference][2]

![Defects Image][3]

Also, you can go [here][4] and explore a static code analisys to most opensource projects and see ther defect ratio per lines of code. You will see it is pretty low. And even worse for you, as the code is open, most of the defects are already fixed.

>  "There was a ton of work and engineering put into the Win32 API. Why do people want to clone the Win32 API, like the WINE project?" he added.

Win32 API sucks. The community did not clone t, they just did an alternative implementation of it in order to run Windows programs  
on it. That is all, legacy compatibility. No sane guy will ever think on using libWINE to write a new program on Linux.

Win32 API will never compare to the API [Qt] offers. Even Windows programs use it, to gain a decent API, and better, source code portability across Linux, Windows and Mac. Go and meet any MFC programmer and you will notice they lost all their hair :-)  
Or ask why Opera, Skype, German Brockhaus Encylopaedia, Google Earth, Adobe Photoshop Album, all Windows programs are not using the horrible Win32 API.

Even Microsoft moved away from the Win32 API for high level applications, and it is pushing the .NET and WinForms API (now to be replaced by WinFX?).

[1]: http://news.yahoo.com/s/cmp/20060623/tc_cmp/189600635  
 [2]: http://www.mysql.com/why-mysql/quality/  
 [3]: http://www.mysql.com/why-mysql/quality/reasoning_graph.png  
 [4]: http://scan.coverity.com/  
 [5]: http://www.trolltech.com

