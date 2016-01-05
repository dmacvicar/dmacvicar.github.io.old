---
layout: post
title: Easy packaging of ruby gems for openSUSE, part II
date: '2009-03-19'
tags:
- newsuse
- ruby
- software
- suse
---

I was in the need of learning about osc plugins, because I want to rewrite some scripts that I use to track maintenance updates to the software management stack. Learning osc plugins meant learning python at the same time.

I was inspired by the osc gnome todo functionality, which shows something like this:

vuntz@lyon ~/work/opensuse/tmp/\>osc gnome todo  
 Downloading data in a cache. It might take a few seconds...  
 Package | openSUSE:Factory | GNOME:Factory | Upstream  
 ---------+------------------+------------------+-------------  
 anjuta | 2.23.91 | 2.23.91 | 2.24.0.1 (r)  
 at-spi | 1.23.92 | 1.23.92 | 1.24.0  
 gdm | 2.23.92 | 2.23.92 (s) | 2.24.0 (s)  
 gedit | 2.23.93 | 2.23.93 | 2.24.0

So, in my last post, I wrote how to [easily package gems for openSUSE][1], saving at least 99% of the work. However, how do you know that you need to package a new gem? Well, here is osc ruby todo, in its first 0.0.0.0.1 release:

duncan@tarro:/osc-plugins\> osc ruby todo  
 126 gems in build service projects  
 Retrieving upstream gem information...  
 + libxml-ruby upstream: 1.1.2 bs: 0.9.5  
 + radiant upstream: 0.7.1 bs: 0.6.9  
 + launchy upstream: 0.3.3 bs: 0.3.2  
 + hpricot upstream: 0.7 bs: 0.6.161  
 + net-ssh upstream: 2.0.11 bs: 2.0.8

So, it connects to gems.rubyforge.org and tell you which gems are outdated. Do you want to keep openSUSE a nice ruby platform? help me. The following features are waiting:

* track ruby package versions  
* track other gem repositories, like the rails one or github  
* also compare it with version in factory  
* nice tables  
* caching  
* Use [gem2rpm and the openSUSE template][1] to automatically create the specs for outdated gems

You can download it from [git.opensuse.org][2], just symlink the .py file to ~/.osc-plugins. It requires rpm-python and Python 2.6 or newer.

My ultimate goal is to get a similar one for zypp/yast that allows to compare our Head repositories with Factory and maintenance repos, show diffs and make submit requests.

So how does it works? How to create your own osc plugin? Look at the source code of the osc-\* projects at [git.opensuse.org][3], You will realize it is as easy as:

* Open a .py file  
* Create a do\_something method, adding the documentation and checking input and arguments  
* Use the meta\_ API and urllib to get data from the build service  
* Use makeurl to build the right authenticated urls to do the requests.

[1]: http://duncan.mac-vicar.com/blog/archives/520  
[2]: http://git.opensuse.org/?p=projects/osc-plugins-ruby.git;a=summary  
[3]: http://git.opensuse.org

