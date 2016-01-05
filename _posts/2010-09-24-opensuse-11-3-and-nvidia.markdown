---
layout: post
title: openSUSE 11.3 and nVidia
date: '2010-09-24'
tags:
- newsuse
- nvidia
- opensuse
- suse
---

I was going nuts because I could not get a sane configuration for my laptop, which has a nVidia NVS 3100M card.

The laptop is connected most of the time to a docking station, which has a external monitor. I want to use the external monitor when the machine is on the dock, and the internal LCD when it is not. No dual head.

openSUSE 11.3 ships with the nouveau driver. It has wonderful RandR support. KRandTray tool detected both displays and allowed me to turn one off and the other on with 3 mouse clicks. It was not perfect, as I had to do it on dock/undock, but also at boot time.

Sadly, nouveau is buggy. At the beginning I was affected by a [random freeze bug](https://bugzilla.redhat.com/show_bug.cgi?id=596330) which forced me to login from my phone to reboot the system. Deactivating acceleration made this go away, but then randomly you will see in your syslog:

```
[drm] nouveau 0000:01:00.0: no space while unhiding cursor
```

And the system will get extremely slow. Reboot needed. This was happening too often. My Linux experience completely ruined. I tried various kernels including Kernel:HEAD, but no luck. [Known bug](https://bugzilla.redhat.com/show_bug.cgi?id=541628), there is a report in Novell bugzilla too.

On the other hand, the proprietary nvidia driver seemed to work well. But KRandTray did not showed two displays. The problem is that the nVidia driver has a proprietary system to support dual head configurations.

You could run the nvidia-settings tool as root (!!) and change the monitor, but this was a very complicated process. My use case is: I want to hack in a different room, 2 clicks and undock.

Today I found googling a tip that made it work like I expected. You need to configure metamodes. Yes, I have a xorg.conf again. My Screen section looks like this:

```
Section "Screen"
    Identifier "Screen0"
    Device "Device0"
    Monitor "Monitor0"
    DefaultDepth 24
    SubSection "Display"
        Depth 24
    EndSubSection
    Option "TwinView" "1"
    Option "TwinViewXineramaInfoOrder" "DFP-0"
    Option "metamodes" "DFP-0: null, DFP-2: nvidia-auto-select +0+0;
               DFP-0: nvidia-auto-select +0+0, DFP-2: null"
EndSection
```

Each metamode disables one monitor. After relogin I noticed KRandTray still showed one display. But I realized I had two possible resolutions, and selecting between them switched the monitors!. Even less clicks than before, where I had to enable one and disable the other.

Lets see how stable it runs

 ![]({{ site.baseurl }}/assets/images/posts/icon_smile.gif)

.

You can even bind the switching to hotkeys using xrandr calls:

```
xrandr -s 0
xrandr -s 1
```

