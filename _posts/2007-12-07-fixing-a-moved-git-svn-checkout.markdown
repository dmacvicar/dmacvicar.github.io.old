---
layout: post
title: fixing a moved git-svn checkout
date: '2007-12-07'
tags:
- git
- newsuse
- suse
- svn
---

I had a git-svn checkout of coolo's branch where we are porting yast2-qt to Qt 4.x. I also publish that git repo at git.icculus.org. Yesterday coolo moved the svn tree to trunk/qt4 and I did not want to do a git-svn clone from scratch because then I would need to start my published repo from scratch.

doener on #git helped me. I had no idea how this works, but it worked. So I blog it in case I need it again in the future. May be useful if you have the same problem:

```
git clone git://git.icculus.org/duncan/yast2-qt4
cd yast2-qt4
git-svn init http://svn.opensuse.org/svn/yast/trunk/qt4/
git svn fetch -r42760 svn
git branch tmp git-svn
git-filter-branch --parent-filter "echo -p $(git rev-parse master)" tmp
git reset --hard tmp
git update-ref refs/remotes/git-svn master
find -name .rev_db* | xargs rm
git svn rebase
```
