---
layout: post
title: openSUSE Build Service on Android
date: '2010-06-28'
tags:
- android
- newsuse
- software
- suse
---

 ![]({{ site.baseurl }}/assets/obsdroid2.png)

 ![]({{ site.baseurl }}/assets/obsdroidimages/posts/1.png)

Release fast, release early. That is what I am trying this time so don’t get too excited. I only added one feature. Yes one.

You can list the pending submit requests related to you. Nothing else. And not with a very pretty layout

 ![]({{ site.baseurl }}/assets/images/posts/icon_smile.gif)

but it will get better.

However, I have an infrastructure in place that will allow me to consume the API very easily, and I will push new versions every time I add something.

What I want to add next?

- Improve the user interface so that it looks pretty
- Ability to act on a request and see the diff (colored!)
- Explore projects and packages
- A service that polls for build failures and show notifications. Same for requests.

I don’t plan to implement the whole user interface in a phone, but mostly the parts where a phone makes a good tool for the build service. Do you have an idea? Screen mockup? Share it with me!

 ![]({{ site.baseurl }}/assets/obsdroid3.png)

To try it, just search “opensuse” in the market. The code is available [here](http://gitorious.org/opensuse/obs-client-android).

