---
layout: post
title: Modernizing Spacewalk's user interface
date: '2013-10-30'
categories:
- Software
tags:
- bootstrap
- css
- html5
- less
- spacewalk
- suse
- suse manager
comments: true
---

[Spacewalk](http://spacewalk.redhat.com/ "Spacewalk home page") is [SUSE Manager](https://www.suse.com/products/suse-manager/ "SUSE Manager home page") upstream project. We base our product on Spacewalk's codebase and [contribute features and fixes back](http://goo.gl/0xmYWv "gmane search").

![Screenshot from 2013-10-30 09:42:09]({{ site.baseurl }}/assets/images/posts/screenshot-from-2013-10-30-094209.png)

There are lot of advantages of working in a mature codebase. At the same time, you get to work everyday with legacy code.

We want SUSE Manager to look great and behave like a modern web application. We want it to survive a rapidly changing environment that gives you new challenges like mobile. Our main goals were:

- Better mobile responsiveness
- A standard and documented CSS framework as a base
- Easier customization of the CSS (e.g. [LESS](http://lesscss.org/ "LESS"))
- Deprecate the [Prototype](http://prototypejs.org/) library and use [JQuery](http://jquery.com/)
- Nicer icons

The topic was first discussed as ["what could we do"](https://www.redhat.com/archives/spacewalk-devel/2013-June/msg00003.html "Future of the development stack?") in order to keep everything current and relevant. Then [we presented a quick prototype](https://www.redhat.com/archives/spacewalk-devel/2013-July/msg00024.html "Twitter Bootstrap: Standardizing the CSS framework?") migrating the user interface to something based on [Twitter Bootstrap](http://getbootstrap.com/2.3.2/ "Bootstrap 2.3.2"). This is what the prototype looked like:

![Image]({{ site.baseurl }}/assets/images/posts/manager_bootstrap_default_1.jpg)

The idea was well received. There were (valid) concerns. Spacewalk is a huge application, and a refactoring like this was going to be an intrusive change.

For our department workshop, we assembled a squad and did a very succesful first iteration. We managed to port the main structure and also get a first Spacewalk "black theme". Some of us continued for the [SUSE Hackweek](https://hackweek.suse.com/ "SUSE Hackweek"). After that we integrated the development into our main sprints and at some point we started [pushing our branch to upstream](https://git.fedorahosted.org/cgit/spacewalk.git/log/?h=bootstrap-css "bootstrap-css branch").

What do we have right now?

- A mobile responsive layout based on [Bootstrap 3](http://getbootstrap.com/ "Bootstrap 3"), the [most popular](http://www.ostraining.com/blog/webdesign/bootstrap-boom/ "Bootstrap Boom") CSS framework.
- Scalable font-based icons powered by [Font Awesome 4](http://fortawesome.github.io/Font-Awesome/ "Font Awesome")
- A beautiful Spacewalk "black theme"
- Most of the code was refactored towards HTML5, current CSS, jQuery and LESS.

![]({{ site.baseurl }}/assets/images/posts/laptop-mobile.jpg)

![what-we-have-screenshot-002]({{ site.baseurl }}/assets/images/posts/what-we-have-screenshot-002.jpg?w=300)](http://duncan.files.wordpress.com/2013/10/images/posts/what-we-have-screenshot-002.jpg) [![what-we-have-screenshot-001]({{ site.baseurl }}/assets/images/posts/what-we-have-screenshot-001.jpg)

![]({{ site.baseurl }}/assets/images/posts/spacewalk-mobile.jpg)

More detailed screenshots of how it looked before on mobile [1](http://duncan.files.wordpress.com/2013/10/screenshot_2013-10-30-10-08-29.png "mobile before 1") & [2](http://duncan.files.wordpress.com/2013/10/screenshot_2013-10-30-10-09-04.png "mobile before 2"). How it looks now: [1](http://duncan.files.wordpress.com/2013/10/screenshot_2013-10-30-10-07-13.png "mobile after 1") & [2](http://duncan.files.wordpress.com/2013/10/screenshot_2013-10-30-10-07-42.png "mobile after 2") & [3](http://duncan.files.wordpress.com/2013/10/screenshot_2013-10-30-10-07-50.png "mobile after 3") & [4](http://duncan.files.wordpress.com/2013/10/screenshot_2013-10-30-10-07-56.png "mobile after 4")

There are still some items in our backlog:

- Un-styled pages here and there
- Cosmetic details due to "misbehaving" html (manual breaks, inline styles)
- Custom tags unit tests (WIP)
- Perl-part data list widget (but the layout is there)
- Replacing gifs with icons (WIP)

Now our main goal is a first upstream merge so that we can continue working on this without blocking other features where we want to take advantage of the base user interface.

I hope you enjoyed this report. I will follow up with a more technical post detailing some refactorings and how we automated some tedious changes.

