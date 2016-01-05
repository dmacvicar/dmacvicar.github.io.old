---
layout: post
title: Installation testing reloaded
date: '2008-10-30'
tags:
- newsuse
- software
- suse
- yast
---

### Background

My colleagues use awesome tricks to do nfs boots with custom inst-sys and other magical procedures in order to test a modified installation. I find those tricks cool but that I tend to forget them after a few days.

During 11.0 release cycle, Coolo had to test the new installation theming. He explained me his method, using [update disks][1].

Update disks exist since longs time. AFAIK they were originally designed for drivers, but today they can insert almost any kind of file to the instsys. That was what coolo did: had his own tree, with an updated installation css theme, packed it, and then run qemu, passing the updatedisk url (which he served from his own machine) IIRC he passed an iso as a cdrom to qemu.

I find this method pretty cool. But I still had the feeling it could be automated more:

* qemu supports booting directly from a kernel and initrd, so I could use the http repositories and download those instead of needing a iso.  
* I could append kernel parameters telling qemu about them  
* I could automatically repack my local directory with updated files and avoid running mkcramfs by hand (latter [Steffen][3] told me I could even use cpio)  
* Why bothering with apache in my machine? As qemu can't access the driverdisk image from my local disk, I could start a embedded web server on demand and tell linuxrc about it.

So after lot of questions to Steffen and some code reading of Ludwig awesome "setupgrubfornfsinstall", here is the first version that works:

git clone git://git.opensuse.org/people/dmacvicar/startinstall.git

### Usage

The structure of the driver update disk is something like this:

```
./linux/suse/i386-11.0/dud.config
./linux/suse/i386-11.0/inst-sys
./linux/suse/i386-11.0/inst-sys/usr
./linux/suse/i386-11.0/inst-sys/usr/share
./linux/suse/i386-11.0/inst-sys/usr/share/YaST2
./linux/suse/i386-11.0/inst-sys/usr/share/YaST2/theme
./linux/suse/i386-11.0/inst-sys/usr/share/YaST2/theme/openSUSE
./linux/suse/i386-11.0/inst-sys/usr/share/YaST2/theme/openSUSE/wizard
./linux/suse/i386-11.0/inst-sys/usr/share/YaST2/theme/openSUSE/wizard/logo.png
./linux/suse/i386-11.0/inst-sys/usr/share/YaST2/theme/openSUSE/wizard/installation.qss
```

In this case, I only want to replace the logo and css file. The important part is the directory with the architecture and version. dud.config has:

```
UpdateName: openSUSE 11.0 DUD
UpdateID: 193a399fca4b4da9
UpdatePriority: 100
```

Which I guess are random values for our specific case ;-) This directory tree is available at /foo/driverupdate on my machine.

Now you need a http or ftp repository (I will add more features later...). In the example, it will be http://distros.foo.com/openSUSE-11.0-RC4-DVD/i386/DVD1

Now just run it:

```
ruby startinstall.rb --tree /foo/driverupdate http://distros.foo.com/openSUSE-11.0-RC4-DVD/i386/DVD1
```

startinstall.rb will download the kernel, initrd, pack the driverdisk, start a (webbrick based) webserver offering this driverdisk and launch qemu with the right parameters:

```
qemu -kernel /tmp/kernel20081030-5149-1yn5upf-0
 -initrd /tmp/initrd20081030-5149-uquvmh-0
 -append
   "install=http://distros.foo.com/openSUSE-11.0-RC4-DVD/i386/DVD1
    driverupdate=http://mymachine.suse.de:9999/updatedisk20081030-5149-ogaqpl-0 "
 -hda /dev/zero
 -no-reboot
 -net nic,macaddr=00:BE:C3:B9:4C:BD,model=pcnet
 -m 666
 -net user
 -usb -usbdevice tablet
```

After booting, you will see your modified (see the logo) installation:

![Screenshot]( [http://files.opensuse.org/opensuse/en/0/0a/Yast2-dud.png)]({{ site.baseurl }}/assets/Yast2-dud.png)

I will keep a small page for this tool [here][4].

[1]: http://ftp.suse.com/pub/people/hvogel/Update-Media-HOWTO/html/  
 [2]: http://en.opensuse.org/Linuxrc  
 [3]: http://lizards.opensuse.org/author/snwint/  
 [4]: http://files.opensuse.org/opensuse/en/0/0a/Yast2-dud.png  
 [5]: http://en.opensuse.org/YaST/Development/Tools/StartInstall

