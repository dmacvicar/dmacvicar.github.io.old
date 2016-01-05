---
layout: post
title: Introducing osc-plugin-overview
date: '2009-04-07'
tags:
- newsuse
- software
- suse
---

You maintain some packages in your repository, which you package from some upstream source. You want to quickly compare versions across your development project, Factory, and upstream. You want to configure what is the package list and to display packages only if they are outdated.

You want to configure a view writing myview.ini to ~/.osc-overview which says something like:

[someview]  
 repos=obs://devel:languages:ruby:extensions  
 ,gem://gems.rubyforge.org  
 packages=$1  
 filter.older=$1

And you want to type "osc overview myview" and get back something like:

![table][2]

You can configure as many package data sources as you want for a view. No limit for 2 columns. You can specify from which repo the package list comes from  
using the $x macros. Same with the older version filter.

Then osc-plugin-overview may be what you are looking for, or even better, it may be soon, when we add more features. See more complete description, where to download it  
and planned features in [its wiki webpage][1].

This is the first release so it includes support for getting information from build service projects, submit requests against projects, upstream gem servers and that is. I expect to add stuff like customizable output (machine readable, patchnfo, changes, diff) and other sources like local directories and upstream ftp servers. Help welcomed!

[1]: http://en.opensuse.org/Build_Service/osc_plugins/Overview  
[2]: http://www.suse.de/~dmacvicar/screenshots/osc-plugin-overview/osc-plugin-overview-table.png

