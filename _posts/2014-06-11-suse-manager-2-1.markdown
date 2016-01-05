---
layout: post
title: SUSE Manager 2.1
date: '2014-06-11'
categories:
- Software
tags:
- linux
- spacewalk
- suse
- susemanager
comments: true
---

Back in March, Christian Stankowic [analysed Spacewalk 2.1 and its new user interface](http://blog.christian-stankowic.de/?p=5862&lang=en "First sight at Spacewalk 2.1") look and feel. He asked himself how SUSE Manager would look like:

> I really appreciate this update! The new interface looks more clean and well-designed than the elderly look. I’m really interested to see what the implementation in&nbsp;SUSE Manager&nbsp;will look like and whether&nbsp;Red Hat Satellite&nbsp;will also get a new design.&nbsp; ![:)]({{ site.baseurl }}/assets/images/posts/icon_smile.gif)

Well, now you can see it yourself, as [SUSE Manager 2.1 is out!](https://www.suse.com/company/press/2014/6/new-suse-manager-to-simplify-improve-linux-server-lifecycle-management.html)

![SUSE Manager Systems Systems Details Overview]({{ site.baseurl }}/assets/images/posts/suse-manager-systems-systems-details-overview.png)

![]({{ site.baseurl }}/assets/images/posts/screenshot-from-2014-06-10-234145.png)

New features include, among others:

- A slick setup wizard to guide administrators through the basic steps needed to configure a fully operational SUSE Manager: Proxy, Novell Mirror Credentials, SUSE Products.  
 ![SUSE Manager Admin Setup Wizard Mirror Credentials]({{ site.baseurl }}/assets/images/posts/suse-manager-admin-setup-wizard-mirror-credentials.png)
- Action chaining that lets administrators bundle and execute related management actions in one step
- Unattended bare-metal provisioning that allows customers to power on and off and reboot bare-metal systems via the IPMI (Intelligent Platform Management Interface) protocol.
- OpenScap (the open source implementation of SCAP – Security Content Automation Protocol), a standardized approach to maintaining enterprise system security.
- CVE Auditing. This feature goes beyond telling you pending patches&nbsp;but instead assisting you to assign the right content to your systems:&nbsp;what vulnerabilities affect you where you haven't yet assigned the right channels. For example, you may have an affected system in production. CVE Auditing may tell you that you can fix the security issue by assigning the stage channel to a system.  
 ![CVE Audit]({{ site.baseurl }}/assets/images/posts/cve-audit.png)
- Locks packages on the server which are then enforced on the client side (eg. if you login via ssh to the client).

And of course, most of the work is [already merged upstream](https://github.com/spacewalkproject/spacewalk/pulls).


