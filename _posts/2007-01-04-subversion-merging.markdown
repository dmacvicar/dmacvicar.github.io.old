---
layout: post
title: Subversion merging
date: '2007-01-04'
tags:
- software
---

Subversion does not make the task of merging multiple branches easy. Just discovered svnmerge.py.

The tutorial and downloads links [are here][1]. There are three use cases: development branch, release branch, and merging  
branches back to trunk. You can do any combination of them at the same time.

Merge information is stored as standard svn properties!

(I am standing in a clean trunk checkout)

dmacvicar@piscola:/space/sources/trunk/myproject\> svnmerge.py avail  
 4711-4712,4721,4731,4733,4735-4736,4741-4746

(if I am tracking more than 1 branch, I need to specify it, here it assumed I  
asked for 1.0-Branch)

You can block certain revisions to be shown as available ( for example stuff that I  
don't want to integrate from 1.0-Branch into trunk) and you can also query  
the integrated revisions.

Finally, you can merge:

svnmerge.py merge --bidirectional

it does not commit for you, of course.

You can see how they are stored as svn properties:

dmacvicar@piscola:/space/sources/trunk/myproject\> svn pg svnmerge-integrated  
 /branches/1.0-Branch/myproject:1-4704

[1]: http://www.orcaware.com/svn/wiki/Svnmerge.py

