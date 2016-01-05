---
layout: post
title: Wireless card
date: '2006-10-19'
tags:
- newsuse
- open-source
- suse
---

Some years ago I did some quick google and bought a D-Link wireless card "supported by Linux". It wasn't. Mine was a "special edition" of the model with a different chipset. Compiling after each upgrade and binary firmware was needed to use it. It was not \*that bad\*. Then when I moved to germany I lost it somewhere.

Ok, this time I decided to get another one, so I googled and concluded ralink was the best option. I found a card with ralink chipset in ebay. Got the card. Spend one night upgrading some packages to factory (kernel, networkmanager, dependencies ). Inserted the card, Uh oh. error loading firmware. Firmware? ok. More google. There are ralinks and ralinks. The good one is rt2400/rt2500 and I got a rt61. Dammit. Ok. At least there is a driver. Copied the firmware. And networkmanager detected a.... \*wired card?\*.

No luck trying to use it with WEP from command line.

So I went to the website and compiled the last released driver by the rt2x000 project. Loaded the module and (now it is ra0 instead of wlan1) and networkmanager sees the card. The kernel complains:

> linux kernel: ra0 (WE) : Driver using old /proc/net/wireless support, please fix driver !

Ok. So that is why networkmanager does not see the SUSE version of the driver. Because it is fixed and NetworkManager isn't? or is it hal?. Doesn't matter, lets try to connect to my router. No luck.

I am trying to get a prism2 or orinoco card now.

