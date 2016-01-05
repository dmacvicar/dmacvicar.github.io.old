---
layout: post
title: mounting from scripts
date: '2007-01-23'
---

I had a script that mounted my usb hard disk and backup some folders to it. However from one day to another HAL refused to mount automatically a device listed in fstab, and I was required to have it listed there so I could run the backup script as user. If the device was listed in fstab, then KDE automatic mounting when clicking on the icon did not work anymore.

But KDE is cool, so another solution was possible. I removed the fstab entry, and the script now calls the media manager directly via dcop:

dcop kded mediamanager mount  
 "/org/freedesktop/Hal/devices/volume\_uuid\_1839914b\_3f70\_4e47\_97a9\_497074d9732d"  
 rsync -a --delete --stats --progress  
 /home/duncan/Documents /media/extdisk/backup  
 dcop kded mediamanager unmount  
 "/org/freedesktop/Hal/devices/volume\_uuid\_1839914b\_3f70\_4e47\_97a9\_497074d9732d"

Now everything works fine.

It was not easy to figure out the problem was the device being listed in fstab. When it was listed, if you click the icon, the media manager complained a non sense permissions message. HAL logs showed the true.

