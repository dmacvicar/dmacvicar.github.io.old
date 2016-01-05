---
layout: post
title: 'Facebook support: First milestone reached'
date: '2009-05-20'
tags:
- facebook
- newkde
- newsuse
- software
- suse
---

So, I have been working some weeks on this, and today I reached the first "usable" point. Screenshot:

![facebook screenshot]( [http://userbase.kde.org/images.userbase/9/95/Kopete-fb-4-c.png)]({{ site.baseurl }}/assets/Kopete-fb-4-c.png)

As you may know, Facebook has a chat service. For me at least is slowly becoming the place where I have more people talking to me, and as you may also guess, the value of social systems is very tied to the number of users.

Sadly, Facebook guys where not smart enough as the Google guys and brought yet another damn protocol to this protocol overpopulated world. Then came the worst part. They announced something that was not there and [promised Jabber support][1]. One year later nothing has yet happened.

For a such popular service, one starts to think whether waiting another year is worth for a protocol that is so popular. As I wanted it myself now, at some point I decided I was willing to implement it even if a Jabber version was available later.

We already have the problem that users expect to see Google talk in the Kopete list, because developers don't figure out that grandma does not know what Jabber/XMPP is. So a good improvement would be adding the concept of "services" where we could add a protocol by just saying "it is just jabber, but with this server settings, this logo and this name". That path would allow for a easy move to other XMPP protocols later.

But for Facebook, no more wait. Yesterday I was able to use it for first time to chat, so I am blogging about it.

Next steps:

* Add more error handling  
* Fix a bug in the contacts status when they go offline  
* Put it into kopete or playground svn  
* Make an openSUSE package ;-)  
* Cleanup. I started over the testbed plugin and it added some stuff that probably I don't need  
* Proxy support. I coded the engine using [QNetworkAccessManager][2] so it is KDE independent. Only the Kopete plugin is KDE based, so I haven't looked into proxy support and other stuff

Other stuff with less priority:

* Adding contacts from the client  
* Configuration (there is no much to configure)

[1]: http://www.google.com/search?q=facebook+jabber  
[2]: http://doc.trolltech.com/qnetworkaccessmanager.html

