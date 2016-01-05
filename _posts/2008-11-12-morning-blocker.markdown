---
layout: post
title: 'morning: blocker'
date: '2008-11-12'
tags:
- Life
- newsuse
- software
- suse
- yast
- zypp
---

### Morning

Did not start the day in a good way. I buy [mobicards][2] every month so I have full access to the public transport system. I buy them via the internet, so I get an email when it is about to run out, and with one click I get the form to order another one by mail (even with the date range pre filled).

Today I took the straßenbahn (tram), and suddenly the guy next to me transformed itself in 1 second from a civilian to a public transport control employee, and asked me for my ticket. I took my mobicard from my pocket and showed it to him. He nods but after a second he comes back and ask "let me see it again". He looks again and points out it starts on 12.11 and today was 11.11. No problem, last month one is behind that one (the whole year cards are there). He looks the old one and points out "This one ends at 10.11". I could not believe it, I did not even realize. Usually I just click OK-OK when buying them. I got a ticket. Luckily the checkbox marked was not "pay 40€" but "Go to the headquarters to clarify" (which still means they could fine me). Of course that means wasting half a morning not counting the time I lost already over the tram (not being able to get out in the right station). If I get fined after buying mobicards every month for years it would be really ironic.

### ZYpp

In the office we had the typical "first media" blocker. Luckily [Michael][3] found the bug really quickly. Later I wanted to test the fix using linuxrc magic driverupdate tricks I have learned in the past weeks. However it did not work: Linuxrc can use rpms as a container format, but still expects the driverupdate directory structure. Bah, world was not that cool as I thought. Met with Michael and Steffen about it. Steffen realized that it would not be a bad idea to try the rpm first as a driverupdate, and if no driverupdate is there just unpack it in the system (and promised to implement it) That sounds good, easy testing. Happy.

### YaST

#### autoYaST

Katarina wrote a really nice [tutorial][1] that explains how use autoYaST to apply a configuration to a running system. Uhm. Looks like an XML API. Wow. Couldn't we use it for our YaST web service?. Talked with schubi about it. While the command line interface has higher level, exposing this interface would make sense too, because it will offer automatically almost all autoYaST power.

#### Partitioner

The new YaST partitioner, on which [Katarina][8], [Arvin][7], [Matthias][6] and [Martin][9] worked really hard during the Code11 cycle [got a pretty nice review here][5]. Give it a look. And give the guys feedback. I am sure lots of things can still be improved!.

#### YaST releases

[Stano][11] is reviving the discussion about whether to make [independent of openSUSE YaST releases][10]. I talked about this topic with Zonker just after he started as community manager.

I think it would be a pretty good move. Also a big challenge. It would mean a challenge not only for us the YaST teams but also for other internal and external stakeholders.

Right now a new YaST package submission (same with libzypp) is more or less tied with the development of features or some project manager sitting next to your chair to get a blocker fixed for an openSUSE release. Things would change if the distribution's team would need to take the last released YaST and take patches, forward ports, backports, etc.

Also it would be a challenge for us. Most of YaST testing happens on a openSUSE release. We have lot of ideas on how to improve our automated testing and this would require develop them further. Also I think an approach like this would facilitate making YaST less distro-dependent.

Another point I like of the approach is that it would force us to have a better process to define our core-platform (ir order to be able to release it separately). Right now I see it a bit chaotic: APIs are born in modules, and as soon as they are used by more than one module, they land in the wildcard yast2 package. Something I really don't find very convenient. APIs should be reviewed with more care and packaged by the area the API covers.

Still and interesting topic and material to innovate!

[1]: http://hedgehogpainter.livejournal.com/2568.html  
 [2]: http://www.vgn.de/mobicard/  
 [3]: http://lizards.opensuse.org/author/mlandres/  
 [4]: http://lizards.opensuse.org/author/snwint/  
 [5]: http://ostatic.com/176402-blog/opensuse-11-1s-new-partitioning-module  
 [6]: http://www.linkedin.com/pub/1/9b0/b93  
 [7]: http://lizards.opensuse.org/author/aschnell/  
 [8]: http://hedgehogpainter.livejournal.com  
 [9]: http://en.opensuse.org/User:Mschmidkunz  
 [10]: http://lizards.opensuse.org/2008/11/07/yast-releases-independent-of-opensuse-releases/  
 [11]: http://lizards.opensuse.org/author/visnov/

