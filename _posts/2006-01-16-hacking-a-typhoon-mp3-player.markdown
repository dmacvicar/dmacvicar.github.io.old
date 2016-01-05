---
layout: post
title: Hacking a Typhoon MP3 player
date: '2006-01-16'
categories:
  - Software
---

I got a [hard disk based mp3 player](http://www.typhoon.de/en/art.php?p=751). The cool thing is that it can play [ogg files](http://en.wikipedia.org/wiki/Ogg) and also works as a standard usb-storage device. It has its problem though. Like most player, the jukebox database is completely proprietary.

Emailing the company did not work as it was a rebranded product. Emailing the original makers resulted only in getting ignored.

![]({{ site.baseurl }}/assets/83090017fp.jpg)

After playing with [khexedit](http://docs.kde.org/stable/en/kdeutils/khexedit/introduction.html) for a while I was able to figure the simple fixed field length format of the binary database. I wrote a test program using Qt and Taglib (thanks Scott, it rocks, so simple...) to recreate the database for my collection. I haven’t however yet figured the index files. Also, I have no idea how to read the FAT short names so I have to mount has VFAT to copy and keep the nice names, but as MSDOS to generate the database and see the shortnames the player stores in the database. Once I figure out the indexes, it should work.

If it does, I plan to write an [amaroK](http://amarok.kde.org) media device plugin (no idea how to resolve the mount issue here).
