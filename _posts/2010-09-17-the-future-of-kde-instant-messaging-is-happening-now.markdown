---
layout: post
title: The future of KDE instant messaging is happening now
date: '2010-09-17'
tags:
- software
---

 ![]({{ site.baseurl }}/assets/mainwindowimages/posts/1.png)

Almost nine years passed since the first lines of Kopete code started to [take form, in a remote country in the south part of the globe](http://websvn.kde.org/trunk/KDE/kdenetwork/kopete/kopete/main.cpp?view=markup).

Still today, looking at an old Kopete screenshot has a special meaning for me. I had so much fun. I learned hundred of tricks and certainly it shaped me as a developer. Working with brilliant hackers across the world brought this multicultural curiosity. Both things combined resulted in myself living in a different country, married to a woman from yet another one, and having friend parties where almost everyone was from a different place, and working in a company involved in this great hobby.

However the world was different by then. At that point the discussion was whether you ICQed or you sent viruses via MSN. And the most difficult challenge was to get file transfer done right. Nothing of that matters anymore.

Kopete was initially very innovative, at least in its goals: to communicate with people, while leaving the IM network as a channel. We brought the concepts of “metacontacts” (bad naming), but basically you say people in your contact list, no matter if they were available on MSN, ICQ, or both.

Today I have a telephone with internet 24/7 in my pocket and I can IM on the bus. I don’t choose IM networks as a soccer team, but rely on them because I have friends on various of them. Just like I use twitter for “geeky stuff” while Facebook is a more “relaxed” environment.

Sadly, at some point Kopete lost leadership, when all the strong opinionated and grumpy developers that shaped its core heart got busy with other stuff, while people contributing to it were mostly interested in features like being able to have sub-sub-sub-sub-sub-groups, or notifications per-contact-per-network-perl-daytime or concepts like identities which made sense but that were not designed, transforming Kopete in a kind of a very sophisticated “IM App Construction Kit” but at the same time some of its configuration looks like an IDE, which wording that not every user would understand: identity, account, contact, metacontact, etc.

The focus was on supporting every single feature of every single IM system and to expose it in the user interface somehow. Which was at the same time not easy, as we implemented various protocols from scratch, and keeping up with features or even protocol changes as lot of manpower.

I tried to motivate myself to do development again, but every proposal on the mailing list just seems a completely different direction of what I would even had energy to discuss. Yes. I don’t get motivated even to discuss if adding one tab per IM network to the user interface makes sense or not.

Linux gained a promising technology when [Telepathy](http://telepathy.freedesktop.org/wiki/) started to mature. While Telepathy is yet another abstraction layer, it is one built in a way that has proven to be successful on the Linux Desktop, by providing dbus based services that are desktop independent. And it has reached the point where is even used in production devices (AFAIK Nokia), and has features like conferencing and multimedia in its design. Not to mention the protocols code is reused across applications.

There is a group of KDE hackers bringing this forward for KDE under a project called [Real Time Communication and Collaboration](http://community.kde.org/Real-Time_Communication_and_Collaboration), partially sponsored by [Collabora](http://www.collabora.co.uk/).

I personally think this is the future of Kopete. IM not as an app, but as a desktop level service where the chat window and the contact list is just something using this service. It fulfills the vision we tried to achieve lot of times with Kopete: like when KMail contact status was implemented, or KAddresbook integration.

A future where people is people (and no metacontacts), and the focus is communicating, listening and seeing them, and not sorting and classifying them. I believe this project has the chance to achieve that.

As any big application, you can’t kill it by rewriting it from scratch. Kopete is awesome and will serve us for a while. The strategy this team is taking by building blocks one by one in the most simple way, sometimes reusing Kopete pieces where possible, is the right one. And I have the feeling they will coexist in parallel for some time.

This group of hackers is [meeting right now in Collabora offices for a 1st sprint](http://community.kde.org/Telepathy/Events/TelepathySprint1) (Hello Will!). They deserve all the support from the community. Because they are writing the future of KDE instant messaging.

