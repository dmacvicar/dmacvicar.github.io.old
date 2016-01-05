---
layout: post
title: Easy development & test environment for Linux kernels
date: '2007-05-28'
tags:
- newsuse
- suse
---

I really enjoy learning new things, and I spend most of my computer time digging into stuff. My productive times always coincide with the quality of my development environment.

If you can't keep up with a easy and own system to checkout, develop, compile and try code, it is very likely you will spend more time doing meta-development.

I have been always interested into learning more about the kernel. I would conform myself with having a sandbox where easily modify and try things, so if one day I feel like doing something, I could just sit and start.

We are going to use [QEMU][qemu], a PC emulator, which is quite nice because:

* It can launch kernels directly. Uses some magic to load the kernel in the emulator and boot it, so it is not needed to setup a tftp server with a kernel.  
* It doesn't require kernel modules, except if you need more speed, then you can use kqemu.  
* It is free

The fact QEMU can launch a kernel directly, means we can launch the image we just compiled.

To get the Linux kernel source do:

```
git-clone git://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux-2.6.git linux
```

To avoid having to create a image of the system to use as a root disk all the time, we can just mount a directory on our own machine via NFS.

```
make xconfig
```

Be sure to enable nfsroot option, which at the same time requires kernel ip level autoconfiguration. Also be sure to put your network card in the kernel, not as a module, otherwise you will need to create a initrd image too.

```
make bzimage
 make modules
```

We have everything ready to boot the kernel. But we need a root filesystem too. We will use the [Kiwi][kiwi] tool to generate a custom root filesystem. With kiwi you can generate a root filesystem and then turn it into a LiveDVD, a USB stick distro or a Xen image. For us the first stage is enough. See the website for info how to get SUSE packages for Kiwi

Edit /usr/share/kiwi/image/netboot/suse-10.2/config.xml and now create the image:

```
kiwi --root /space/nfsroot/work --prepare /usr/share/kiwi/image/netboot/suse-10.2
```

When trying this, I got a strange error. Looking at work.log, the problem was when kiwi called smart to install the packages, at commit time, the rpm database did not exist.

```
Committing transaction...^M
 error: cannot open Packages index using db3 - No such file or directory (2)^M
 error: cannot open Packages database in work/var/lib/rpm
```

So I edited /usr/share/kiwi/modules/KIWIManager.pm and near line 630 I added these two lines:

```
print $fd "mkdir -p $root/var/lib/rpmn";
 print $fd "rpm --root $root --initdbn";
```

I am not sure if it the right fix. I will ask on the mailing list later. But the workaround worked for me. The final section of the code looks now like:

```
# Add package manager to package list
 #------------------------------------------
 push (@packs,$manager);
 #==========================================
 # Create screen call file
 #------------------------------------------
 print $fd "smart update @channelListn";
 print $fd "mkdir -p $root/var/lib/rpmn";
 print $fd "rpm --root $root --initdbn";
 print $fd "test $? = 0 && smart install @packs @installOptsn";
 print $fd "echo $? > $screenCall.exitn";
 print $fd "rm -f $root/etc/smart/channels/*n";
```

Edit /etc/exports to share that directory via NFS:

```
/space/nfsroot/work (rw,root_squash,sync)
```

Now, go to your kernel source directory and launch it:

```
qemu -kernel arch/i386/boot/bzImage
 -append
  "nfsroot=192.168.0.101:/space/nfsroot/work,rw
   ip=::::diskless:eth0:dhcp root=/dev/nfs"
 -hda /dev/zero -net nic -net user
```

Final result:

 ![]({{ site.baseurl }}/assets/bootvb3.png)

I still can't login on the machine, but that is a minor issue. Some error messages about read-only filesystem too.

Things I haven't figured yet:

* How to install modules to the root filesystem (something like make modules\_install DESTDIR=/space/image)

[qemu]: http://fabrice.bellard.free.fr/qemu/  
 [kiwi]: http://en.opensuse.org/Build_Service/KIWI

