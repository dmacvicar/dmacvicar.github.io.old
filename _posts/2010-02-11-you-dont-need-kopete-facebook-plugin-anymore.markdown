---
layout: post
title: You don't need Kopete Facebook plugin anymore
date: '2010-02-11'
tags:
- facebook
- software
---

In May 2008, [Facebook announced](http://developers.facebook.com/news.php?blog=1&story=110) that they were planning to add [XMPP](http://xmpp.org/) (a.k.a Jabber), the standard messaging protocol behind Google Talk and other chat programs, to their Facebook Chat solution.

In May 2009, seeing that nothing happened, [I announced that I was working](http://duncan.mac-vicar.com/blog/archives/541) on Facebook support for Kopete and released a prototype on [github](http://github.com/dmacvicar/kopete-facebook). The plugin was not perfect, and it was talking to Facebook using non-standard ways (including html scrapping!), but allowed people to see their contacts and chat.

Yesterday, [Facebook finally announced XMPP support](http://blog.facebook.com/blog.php?post=297991732130). This means various things:

- You can connecto to Facebook chat using any XMPP compatible program: Kopete (using the standard Jabber plugin)
- My Kopete plugin is not longer needed and will be deprecated

To setup it with Kopete just add a Jabber account like this:

 ![]({{ site.baseurl }}/assets/kopete-facebook-xmpp-1-267x300.png)

Kopete preferences for Facebook chat

This is a great move from Facebook. As [Lars already mentioned](http://twitter.com/larsmb/status/8146517566), the Web 2.0 sites have brought lot of innovation and fresh wind to the Web. However, they have ignored interoperability a lot, and he is right, you need “connectors” to get your data.

The Web 2.0 has changed the way users store their data. Now it is everywhere. Without good interoperability we are only adding complexity to users.

If your website implements contacts, don’t forget to add a url with a http accessible vCard list. If your site implement events, provide an url with iCal entries. Google has done a good job with [Google Calendar](http://calendar.google.com/) and Google Mail. Worth to mention the urls with calendar/contact entries can be “secret urls” which contain a long random string, but require no authentication, which makes it easy to add to your desktop mashup utilities, organizers, plasmods, etc.

Facebook still could do more. They invented a whole new email system. But they forgot to offer IMAP/SMTP interoperability with it. I am not sure either (feel free to correct me) whether I can generate a secret url with iCal entries of Facebook events as well.

Anyways, big thanks to whoever is responsible of getting this done. You did a big favor to the Internet itself.

As for Kopete. As protocols start to use XMPP, the need of hiding XMPP for the end user arises. The account wizard should display the services known by name, and do the XMPP setup with the known preferences. May be something I can work on now that I don’t need to maintain the protocol anymore. And I almost forget: we need a way to migrate current users of the plugin.

