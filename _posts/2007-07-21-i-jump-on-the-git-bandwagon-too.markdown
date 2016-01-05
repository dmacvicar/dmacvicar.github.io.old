---
layout: post
title: I jump on the git bandwagon too!
date: '2007-07-21'
tags:
- software
---

It was not my intention to use [git][1]. All started when I was looking for some way of doing commits to subversion repositories while being offline and at the same time creating local branches.

I tried [SVK][4]. At the beginning it seemed fine. But soon after I started to face it's limitations:

* depot / checkout separation is sick. Detach a checkout just to move it around? no thanks

* Something (probably me?) went wrong and I ended up overwriting Michael's commits in ZYpp subversion.

* To share and move commits between machines, I would need to mirror the depot of the other machine in my laptop.

* slow

* packaging it was not fun

So, during aKademy in Glasgow, while having some italian food with some [trolls][6] and KDE people, [Simon][5] told me he was using git to interact with perforce and subversion.

Back in Nürnberg, I started to try it. First I tried tracking KDE4 subversion repository using [git-svn][2] ([tutorial][3]). Then ZYpp. It works really well, especially with cmake (it forces you to compile out of the source tree), so you can do workflows like this:

* being in master, sync your master branch with subversion.

* ok, now you have to implement feature X.

* create a local branch, checkout it, work, compile from your build dir

* do a couple of commits.

* Somebody comes to your office, to discuss some bug. Your feature is half broken so you want to discuss using the clean master. Checkout master.

* The nice thing is, checkouts are in the same working copy, so if you recompile from the build dir, only the files changed between branches will be recompiled. In other system, every checkout has to be in a different directory (except svn switch which is crap).

* you switch back to your feature branch and continue working and commiting.

* when it works, merge it to master. git-svn dcommit will send it back to subversion.

Till now, I am really satisfied with it. I will keep blogging about it as I learn more.

[1]: http://git.or.cz  
 [2]: http://www.kernel.org/pub/software/scm/git/docs/git-svn.html  
 [3]: http://utsl.gen.nz/talks/git-svn/intro.html  
 [4]: http://svk.bestpractical.com/  
 [5]: http://people.kde.org/tron.html  
 [6]: http://trolltech.com/

