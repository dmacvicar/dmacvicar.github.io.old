---
layout: post
title: Web-izing YaST
date: '2009-05-26'
tags:
- newsuse
- rails
- software
- suse
- yast
---

As you may know, are working on making [YaST functionality accessible via the web][4]. With this we mean not only browser. The current prototype has two parts: a generic web service (REST like API) and a web browser client.

Stefan Schubert [announced last week][1] a new snapshot for developers. You can find packages [in the build service project][2]. The packages are named yast2-webservice and yast2-webclient.

This is very early code. It is not fast, and the web client is not yet using all user interface possibilities that ajax gives, but there are we going :-).

If you are an experienced user, and you get it running, you may be interested in getting it running from source code by reading [our development web page][3]. Both the webservice and the webclient are rails applications. Developing modules is also easy! Just show up in irc (freenode) #yast.

[1]: http://lists.opensuse.org/yast-devel/2009-05/msg00008.html  
[2]: http://download.opensuse.org/repositories/YaST:/Web  
[3]: http://en.opensuse.org/YaST/Web/Development  
[4]: http://en.opensuse.org/YaST/Web

