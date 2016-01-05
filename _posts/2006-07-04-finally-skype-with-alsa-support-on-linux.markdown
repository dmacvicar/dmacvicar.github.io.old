---
layout: post
title: Finally, Skype with ALSA support on Linux
date: '2006-07-04'
tags:
- open-source
- software
---

Skype, the proprietary voice over internet application that works :-) has just released version 1.3 beta, which was probably the feature most Linux users were waiting.

Skype dissapointed most Linux users when the Windows (now 2.5) version got out of sync with the Linux one (today, 1.3beta). Which is really nonsense, as Skype is done in Qt, they could keep one source tree and release for all platforms.

The explanation seemed to be that one of the subsystem handling the audio is from a third party, and it did not support ALSA, but that was denied in the forum by other users, Skype was just not using the last version. Not an excuse, there are various decent [multiplatform audio layers][2] out there.

After realizing Skype 1.2 did not work on my laptop because it was not using ALSA and OSS emulation sucked and distorted the conversations, I almost went away. I have a [Openwengo][3] account and money on it, and I am still thinking what will I do when my Skype credit is over. OpenWengo keeps one tree for all platforms and is open source. Let's see how fast Skype managed to add video to Skype for Linux and stop this Skype for Linux and Skype for Windows idiocy and release just Skype in 1 package per platform.

[1]: http://www.skype.com/download/skype/linux/13beta.html  
 [2]: http://www.portaudio.com  
 [3]: http://www.openwengo.org

