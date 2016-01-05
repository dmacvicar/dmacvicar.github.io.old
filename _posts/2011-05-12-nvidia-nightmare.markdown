---
layout: post
title: NVIDIA nightmare
date: '2011-05-12'
tags:
- buildservice
- newsuse
- nouveau
- nvidia
- opensuse
- suse
---

NVIDIA continues to be a nightmare on my T410 hardware.

The proprietary driver does not resume correctly after the second suspend to ram. Not sure if it has something to do with the dock.

The nouveau driver continues to be affected by the [famous bug 26980](https://bugs.freedesktop.org/show_bug.cgi?id=26980 "Freedesktop bug 26980")&nbsp;which has been affecting for a while a set of GPUs, producing random freezes.

There was some hope [in this comment](https://bugzilla.redhat.com/show_bug.cgi?id=625187#c29 "Possible fix/patch"), which points to [this patch](http://cgit.freedesktop.org/nouveau/linux-2.6/patch/?id=25c68aef4e6abcc3c10f593fc565c342ebe2ded8 "possible fix")&nbsp;and stattes that using 2.6.38 with that patch improves the situation. I am trying to build Kernel:stable + this patch [here](https://build.opensuse.org/project/show?project=home%3Admacvicar%3Akernel "Duncan custom kernel").

&nbsp;

