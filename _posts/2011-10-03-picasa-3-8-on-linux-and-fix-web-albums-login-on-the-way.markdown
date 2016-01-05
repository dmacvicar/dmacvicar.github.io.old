---
layout: post
title: Picasa 3.8 on Linux (and fix web albums login on the way)
date: '2011-10-03'
categories:
- Software
tags:
- google
- linux
- picasa
- suse
---

Today I found myself with Picasa for Linux (3.0 beta) not allowing me to login to web albums, even if I could login without problems from the web browser.

After googling a bit, it seems that Picasa 3.0 does not work anymore due to some Google+ related changes. On the other hand it looks like Picasa on Linux is abandoned &nbsp;ie: 3.0 vs 3.8 on Windows.

Picasa 3.8 has some [interesting new features](http://picasa.google.com/support/bin/answer.py?hl=en&answer=93773). Why not a Linux version? Picasa for Linux is no more than Picasa for Windows bundled with [wine](http://www.winehq.org/) plus some minor changes. Ok, lets do one ourselves.

I started by unpacking the original rpm and replacing the "Picasa3" folder in "Program Files" with the tarred content of the "Program Files/Picasa3" resulting from installing Picasa 3.8 with wine. That worked, but it requires you to create this tarball.

Then I went one step further: why not trying installing the newer Picasa 3.8 inside the build section of the .spec file? Thanks to the [wpkg project](http://wpkg.org/Picasa) I figured how to run the installer in unattended mode. The Build Service Tips & Tricks page [explains how to run something that requires an X server using XVfb](http://en.opensuse.org/openSUSE:Build_Service_Tips_and_Tricks#Building_a_package.2C_which_needs_an_X-Server_during_build).

So the spec file first unpacks the original Linux rpm in the builroot. Then runs the Windows-based installer in unattended mode using a temporary wine prefix, and then copies the new installed Picasa over to the buildroot, replacing the files in the original rpm. We use the official rpm as a base because it contains a custom wine and some other Linux integrations, however it would be worth to see if it behaves better with newer wine versions.

You can find the resulting [.spec file in my home project](https://build.opensuse.org/package/show?package=picasa&project=home%3Admacvicar).

I can't redistribute the original rpm and the windows installer, so I include a fetch.sh (just like the spec file for the nvidia driver does) that will fetch those binary files.

To build it:

`
osc co home:dmacvicar picasa
# get the files I can't redistribute
./fetch
osc build openSUSE_Factory
`

Now install it and you should have Picasa 3.8 on Linux, which also solves the issue of login into Picasaweb:

![]({{ site.baseurl }}/assets/images/posts/about-picassa.png)

