---
layout: post
title: openSUSE clients with Spacewalk
date: '2011-05-06'
tags:
- newsuse
- opensuse
- suse
- susemanager
- zypp
---

 ![SUSE Manager screenshot]({{ site.baseurl }}/assets/images/posts/40_system_groups.png)

My team has been busy the last months with the release of [SUSE Manager](http://www.novell.com/products/suse-manager/), which was received with [very good reviews](http://www.eweek.com/c/a/Linux-and-Open-Source/Novell-SUSE-Manager-12-Taps-Red-Hat-Technology-to-Rein-In-Enterprise-Linux-Servers-815712/). There is lot of room for improvements, some of them specific to SUSE products/integration but others in Spacewalk itself.

There is lot of work to do and lot of [patches being reviewed](http://www.mail-archive.com/search?a=1&l=spacewalk-devel%40redhat.com&haswords=PATCH+%22%40suse.de%22&from=&notwords=&subject=&datewithin=1d&date=&order=relevance&search=Search). Lot of them are [already upstream](http://miroslav.suchy.cz/spacewalk/gitstat/changelog-find.php?search_opt=3&search3=suse&submit=1&page=).

One common question is: if I already have a Spacewalk server, how do I setup openSUSE clients?

Thanks to Michael Calmer, we submitted the required packages to a [repository in the openSUSE Build Service](http://download.opensuse.org/repositories/systemsmanagement:/spacewalk:/1.4/openSUSE_11.4). You can find the instructions looking for the SUSE section in the ["Registering Clients" page of the official Spacewalk wiki.](https://fedorahosted.org/spacewalk/wiki/RegisteringClients)

