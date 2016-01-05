---
layout: post
title: bicho 0.0.3 released
date: '2011-12-09'
categories:
- Software
tags:
- bugzilla
- ruby
- suse
---

[Bicho](http://github.com/dmacvicar/bicho "Bicho on github") is a ruby gem implementing access to bugzilla. It is a library but comes with a simple command line client.

This [release](http://rubygems.org/gems/bicho "Bicho on rubygems.org") fixes some bugs and adds support for named queries.

From the [API](http://rubydoc.info/github/dmacvicar/bicho/v0.0.3/frames "Bicho API docs"), you can give a bug number or named query, or a combination of many of them:

or from the command line

```
bicho -b https://user:pw@bugzilla.domain.com show query-name
```

If you are using Novell's bugzilla, Bicho includes a plugin that automatically authenticates using your .oscrc credentials.

Be sure to also checkout [Klaus's](http://twitter.com/#!/kkaempf "Klaus twitter") [bugzilla adapter for data-mapper](https://github.com/openSUSE/dm-bugzilla-adapter "dm-bugzilla-adapter"), which is also powered by Bicho.

