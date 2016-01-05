---
layout: post
title: How to gain root in a HTC Hero (aka G2 Touch) phone
date: '2011-04-06'
tags:
- software
---

_DISCLAIMER_: I am not recommending you to root your phone and if you are not sure what for or what are the consequences of doing it (including your warranty), then don’t do it. This post is only an explanation on _how_ to do it. Also I am not responsible for any damage to your phone, including bricking it.

First, download Android SDK 1.5 from [here](http://developer.android.com/sdk/1.5_r3/index.html), unpack it and go to the tools/ directory.

My first problem:

```
./adb shell
error: device not found
```

```
./adb devices                        
List of devices attached
```

Reading around I found out how to fix it, and then realized it was documented [here](http://developer.android.com/guide/developing/device.html)

Create a file /etc/udev/rules.d/90-android.rules with the following content:

```
SUBSYSTEM=="usb", SYSFS{idVendor}=="0bb4", MODE="0666"
SUBSYSTEM=="usb",ATTR{idVendor}=="0bb4",ATTR{idProduct}=="0c02",SYMLINK+="android_adb"
SUBSYSTEM=="usb",ATTR{idVendor}=="0bb4",ATTR{idProduct}=="0c01",SYMLINK+="android_fastboot"
```

And restart udev by doing killall -HUP udevd, then unplug and plug the phone.

Now, get [Recovery-RA-HERO-v1.2.2.img](http://rapidshare.com/files/281026171/Recovery-RA-HERO-v1.2.2.img) and [fastboot](http://member.america.htc.com/download/RomCode/ADP/fastboot.zip) (unpack and chmod +x this one), and put them somewhere, or as this example assumes, in the same tools/ directory.

Now boot the recovery image:

```
./adb shell reboot bootloader
./fastboot boot Recovery-RA-HERO-v1.2.2.img
```

You can use the NDroid Backup option to backup your phone to the SDCard.

Get [Superuser.zip](http://forum.xda-developers.com/attachment.php?attachmentid=211569&d=1249225060) (requires registration in the xda-developers phorum) and unpack it. You will get two files from there: “Superuser.apk” and “su”.

```
./adb shell mount /system
./adb push su /system/bin/
./adb shell chmod 4755 /system/bin/su
./adb push Superuser.apk /system/app/
./adb shell reboot
```

Now confirm that it worked:

```
./adb shell
su
```

(Click Always allow on the phone)

And that is!.

If you need to restore your NDroid backup, I haven’t tried, but you can do it from the recovery image or manually pointing it to the images:

```
fastboot flash boot /boot.img
fastboot flash system /system.img
fastboot flash userdata /data.img
fastboot reboot
```

