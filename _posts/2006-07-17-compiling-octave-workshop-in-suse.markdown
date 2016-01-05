---
layout: post
title: Compiling Octave Workshop in SUSE
date: '2006-07-17'
tags:
- newsuse
- open-source
- software
- suse
---

[Octave Workshop][3] is a nice Qt 4 GUI for [GNU Octave][2], a Matlab like numerical computation tool.

![Graph sombrero][4]

To compile it in SUSE 10.1, get [this patch][1] and apply it to your installed Octave 2.1.72 headers:

`
  linux:/usr/include/octave-2.1.72/octave # cat /home/duncan/octave.patch | patch -p0
  patching file ArrayN.h
  patching file DiagArray2.h
`

Then in Octave Workshop, replace the assert for QT\_ASSERT in octave-workshop-0.10/editwindow.cpp

`
  - assert(editors.find(current)!=editors.end());
  + Q_ASSERT(editors.find(current)!=editors.end());
`

then it should compile.

[1]: http://velveeta.che.wisc.edu/octave/lists/archive//octave-maintainers.2006/msg00561.html  
 [2]: http://www.octave.org  
 [3]: http://www.math.mcgill.ca/loisel/octave-workshop/  
 [4]: http://www.gnu.org/software/octave/images/sombrero.jpg

