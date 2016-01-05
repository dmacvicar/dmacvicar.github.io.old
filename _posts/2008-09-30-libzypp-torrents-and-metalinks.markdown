---
layout: post
title: libZYpp, torrents and metalinks
date: '2008-09-30'
tags:
- newsuse
- software
- suse
- zypp
---

Using only http for transfering files will be a thing from the past very soon. We know all the problems distributing content to large audiences companies face, and [Peter Poeml][1] knows it very well too.

So Peter mentored [Gerard Farràs][2] during [the last Summer of Code][3], and the result was a ZYpp media handler that uses [aria2][4] to download files, instead of curl. (May be aria uses curl for http then, I have no idea ;-) ).

> aria2 is a utility for downloading files. The supported protocols are HTTP(S), FTP, BitTorrent (DHT, PEX, MSE/PE), and Metalink. It can download a file from multiple sources/protocols and tries to utilize your maximum download bandwidth. It even supports downloading a file from HTTP(S)/FTP and BitTorrent at the same time, while the data downloaded from HTTP(S)/FTP is uploaded to the BitTorrent swarm. Using Metalink's chunk checksums, aria2 automatically validates chunks of data while downloading a file like BitTorrent.

The benefits is that aria knows about bittorrent, metalinks, etc, so this open the door for much better download failover in the future.

As we are aready on beta phase and we can't switch such an important component, I [merged the code][5], but disabled. That means ZYpp still uses curl, unless you enable the environment variable ZYPP\_ARIA=1. That way you can play with it while it matures for 11.2 or something. There should be lot of features missing in the aria2 handler, so please have that in mind.

Thanks to Peter and Gerard for the handler.

[1]: http://en.opensuse.org/User:Poeml  
 [2]: http://bloc.gerardfarras.com/  
 [3]: http://code.google.com/soc/  
 [4]: http://aria2.sourceforge.net/  
 [5]: http://lists.opensuse.org/zypp-devel/2008-09/msg00142.html

