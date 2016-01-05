---
layout: post
title: Freedom and Privacy
date: '2006-06-21'
tags:
- freedom
- society
- tech
---

>  "I worry about my child and the Internet all the time, even though she's too young to have logged on yet. Here's what I worry about.  
 I worry that 10 or 15 years from now, she will come to me and say 'Daddy, where were you when they took freedom of the press away from the Internet?'"  
>   
>  --Mike Godwin, Electronic Frontier Foundation

Since some time I am spending lot if time trying and following some projects related to privacy, mostly inspired by daily news about how corporations and some countries try to control what their citizens do and not do.

The situation in countries like China and USA is becoming somewhat dangerous. Some topics are being used now as a excuse to control the population like real communist dictatorships. The excuse is always what the "bad guys" will do with those tools. Give me a break.

Other source of pressure come from [the guys][18] that were unable to keep with change and lost their business of selling CDs at high margins at expenses of artists, and now they pretend to keep their business at expenses of people freedom instead of being [creative with their business model][19].

Another point is the storage of historic user data. Most people trust Google, I do too, but Google is a massive data center with data from the whole world, and it is becoming a threat only because where it is located.

After some research and surfing, I came to the following projects that will help you keep the goverment's nose out of your business. I don't really need those tools because I don't have anything anyone would be interested into (hey, I am even lacking new ideas those days :-P ), but just the pleasure to transmit data without the soviets being able to trace (ok, or at least you make it hard to) it, is a reason enough to support those tools.

### Filesharing

There are a cuple of nice projects here. But also most of them are useless. Freenet is probably the most known, but it really looks more like a research toy than a real application. For a nice comparision [go here][3].

[GNUnet][4] has nice features, but lacks a good KDE front end to use it. Also, it is slow and that makes it not usable.  
Another alternative is [MUTE][5], which seems more user oriented. There is a nice KDE frontend called [Kommute][6].

![kommute screenshot][7]

MUTE and Kommute are not really easy to compile and setup for newbies. I already packaged GNUnet for SUSE users, and you can find the package in the [filesharing repo][9] in the build service. Kommute and MUTE will follow soon in the same repo.

### Data privacy

Not too much to dig around. [TrueCrypt][11] is open source, has a nice license, and provides nice features like two levels of [plausible deniability][11], which might be useful in case a user is required to reveal the password. SUSE users can find it in the [privacy repo][8] in the OpenSUSE build service.

Another option is [EncFS][12] which uses the FUSE (filesystem in userspace) layer. The nice thing is that you can encrypt a directory without needed to make a volume, but file sizes and number of files are still there. The packages included is some distributions, including SUSE 10.1.

### Anonymous surfing

This project is the one that surprises me most, because it works. Based on the Onion routing algorithm originally developed by the U.S. Navy (heh) this project is now supported by the [Electronic Frontier Foundation][13] (they provide some [legal information about it][14]), the [TOR][14] project provides anonymous surfing.

What is really nice is the already existant tools. A few minutes of surfing led me to [FoxyProxy][16], a firefox extension that makes Tor usage with firefox piececake. Also, I found [TorK][17], a KDE tool to configure Tor.

![TorK][2]

Tor is already in the privacy repo mentioned above, and expect TorK soon.

As I said, most of this tools are quite useless for me now, but I will try to keep supporting spreading them and making them easy to use.

[1]: http://img90.imageshack.us/img90/7663/torkmain3ne.png  
 [2]: http://img90.imageshack.us/img90/7663/torkmain3ne.th.png  
 [3]: http://www.planetpeer.de/wiki/index.php  
 [4]: http://gnunet.org/  
 [5]: http://mute-net.sourceforge.net/  
 [6]: http://kommute.sourceforge.net/  
 [7]: http://img144.imageshack.us/img144/6902/screenshot1small7mc.jpg  
 [8]: http://repos.opensuse.org/security:/privacy/SUSE_Linux_10.1/  
 [9]: http://repos.opensuse.org/filesharing/SUSE_Linux_10.1/  
 [10]: http://en.wikipedia.org/wiki/TrueCrypt  
 [11]: http://en.wikipedia.org/wiki/Plausible_deniability  
 [12]: http://arg0.net/wiki/encfs  
 [13]: http://www.eff.org  
 [14]: http://tor.eff.org/eff/tor-legal-faq.html.en  
 [15]: http://tor.eff.org  
 [16]: https://addons.mozilla.org/firefox/2464/  
 [17]: http://tork.sourceforge.net/wiki/index.php/Main_Page  
 [18]: http://www.riaa.com  
 [19]: http://www.apple.com

