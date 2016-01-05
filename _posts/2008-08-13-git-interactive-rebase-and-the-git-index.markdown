---
layout: post
title: git interactive rebase and the git index
date: '2008-08-13'
tags:
- git
- software
---

Federico posts about [git interactive rebase][1] to squash commits with build errors together with the fix, which is a nice feature.

However, git design of "commit often" does not mean you need to commit whenever you modify one line, that is a bit overkill.

The git index can be used to incrementally add your work to the point it is really worth committing, and actually you can also interactively pick stuff to be added to the index for later committing just as you can interactively rebase.

I recommend reading the article [Embrace the Git Index][2] to know more. Also this [blog post][3] mentions the topic.

On the same topic, does anyone knows if it is possible to install [gitorious][5] or its fork [gitlab][4] on a separate server to the machine actually hosting the repositories (and therefore having the web application push the configuration from the database to the other server via ssh or something? ).

[1]: http://www.gnome.org/~federico/news-2008-08.html#git-rebase-interactive  
[2]: http://www.jdl.com/papers/Embrace_The_Git_Index.pdf  
[3]: http://codemac.net/blog/18/  
[4]: http://gitorious.org/projects/gitlab  
[5]: http://gitorious.org/projects/gitorious

