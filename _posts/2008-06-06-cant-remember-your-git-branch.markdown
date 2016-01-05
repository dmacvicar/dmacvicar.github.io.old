---
layout: post
title: can't remember your git branch?
date: '2008-06-06'
tags:
- git
- newsuse
- scm
- software
- suse
---

I always have the problem remembering in which branch I am, and typing git branch all the time sucks, so just add this to your profile:

```
if [-e /etc/bash_completion.d/git] ; then
  source /etc/bash_completion.d/git
  export PS1='$(ppwd l)u@h:w$(__git_ps1 "(%s)")> '
fi
```

And your prompt will look like this if you are in a git checkout:

```
dmacvicar@piscola:/space/git/suse/zypp(master)>
```

