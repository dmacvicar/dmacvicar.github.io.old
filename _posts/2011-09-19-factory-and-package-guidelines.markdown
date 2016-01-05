---
layout: post
title: Factory and package guidelines
date: '2011-09-19'
categories:
- Software
tags:
- suse
---

I see some changes going into Factory that apply current packaging guidelines to packages.

To summarize some of the cleanups you can find as submit requests: (no guarantee that those are actual guidelines. [Check yourself](http://en.opensuse.org/Portal:Packaging))

- Remove redundant information:  

```diff
-# norootforbuild
```

- Remove AutoReqProv: on (I guess because it is on by default)  

```diff
-Authors:
----------
- Bob Esponja
- Peter Parker
```

- New macro for parallel builds:  

```diff
+%check
+
make check
```

- Don't clean the builroot yourself:  

```diff
-%clean
-rm -rf $RPM_BUILD_ROOT
-
```

