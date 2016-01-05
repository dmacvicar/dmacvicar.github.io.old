---
layout: post
title: Genius tablet under Linux
date: '2007-01-25'
tags:
- newkde
- newsuse
- suse
---

I just made a old Genius tablet I got from my grandfather to work under Linux.

 ![]({{ site.baseurl }}/assets/images/posts/media_httpimg489image_qzgyt-scaled500.jpg)

The Xorg driver is [here][1]. I packaged it for openSUSE [in my personal repository][1] in the build service (x11-input-wizardpen and x11-input-wizardpen-tools).

Add a udev rule so the device gets always the same file in /dev, you can find those attributes in /proc/bus/input/devices

duncan@linux:~\> cat /etc/udev/rules.d/10-tablet.rules  
 KERNEL=="event\*", SYSFS{idProduct}=="0003"  
 , SYSFS{idVendor}=="5543", SYMLINK+="input/tablet

Add to /etc/X11/xorg.conf

Section "InputDevice"  
 Identifier "WizardPen Tablet"  
 Driver "wizardpen"  
 Option "Device" "/dev/input/tablet"  
 Option "TopX" "0"  
 Option "TopY" "2517"  
 Option "BottomX" "31859"  
 Option "BottomY" "32762"  
 Option "MaxX" "31859"  
 Option "MaxY" "32762"  
 EndSection

Then add to the ServerLayout section:

InputDevice "Mouse[1]" "CorePointer"  
 InputDevice "WizardPen Tablet" "AlwaysCore"

Restart, drawing with Krita is really fun!

[1]: http://software.opensuse.org/download/home:/dmacvicar/  
 [2]: http://www.stud.fit.vutbr.cz/~xhorak28/index.php?page=WizardPen_Driver

