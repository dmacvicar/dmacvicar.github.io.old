---
layout: post
title: The attack of the Droid Chamaleon
date: '2017-04-23'
categories:
- Software
tags:
- linux
- containers
- android
comments: true
---

![Robot Chamaleon]({{ site.baseurl }}/assets/images/posts/anbox/robot-chamaleon.png)
<small>([Robot Chamaleon](http://rahulsonic7.blogspot.de/2015/11/robot-chameleon-software-3ds-max.html) by Rahul Kumar)</small>

You may have seen the series ["Make Windows green again"](https://www.suse.com/communities/blog/make-windows-green-part-1/) from my colleague [Hannes Kuehnemund](https://twitter.com/hakuehnemund), which shows you how to run openSUSE as a sub-system inside Windows without using virtualization.

Windows users now get the world of Linux at their disposal, using a compatibility layer for the POSIX API and loading of Linux binaries. I do the same in reverse. I am able to flawlessly run two guitar pedalboard client software under Linux using [Wine](https://www.winehq.org/). It keeps getting better!.

![Architecture]({{ site.baseurl }}/assets/images/posts/anbox/wine-zoom.png)

Wine covers one of the two interesting platforms Linux users would like to have access to. There are two others: Mac, for which is there is a [project in the same spirit](https://www.darlinghq.org/) but not there yet when it comes to run graphical applications, and Android, the mobile platform by Google.

What if I tell you you can run Android in openSUSE, and not using virtualization?.

# Anbox

[Anbox](http://anbox.io) is a new project with the goal to run Android applications under a "regular" Linux system.

Unlike other solutions like Shashlik or Genimobile, it does not use an emulator, but it uses an approach similar to the one used to bring Android applications to ChromeOS, it uses container technology to confine the Android applications, and a bridge to simulate some of the services that Android needs at runtime.

Anbox is still in alpha stage, but it can already run some stuff. It is distributed upstream using [Snap](https://snapcraft.io/). While I understand the reasons, giving that the package uses system-wide services, needs privileges and installs/loads kernel modules, I did not feel comfortable with this approach.

It was not trivial, but after reading a lot of the installer source-code and some tips from the Archilinux PKGBUILD that installs directly from git I have managed to create an openSUSE package for Anbox and its build dependencies.

# Anbox Architecture

Anbox consists of two services, the `anbox-container-manager`, that runs as root and manages the containers, and a `anbox-session-manager`, which runs as a systemd user service for each user wanting to run Android apps.

![Architecture]({{ site.baseurl }}/assets/images/posts/anbox/anbox-architecture.png)

# Installing Anbox

First, add my repository:

```console
$ sudo zypper ar obs://home:dmacvicar:anbox/openSUSE_Tumbleweed anbox
Adding repository 'anbox' .......................................................................................[done]
Repository 'anbox' successfully added
...
```

Now install it:

```console
$ sudo zypper in anbox
Loading repository data...
Reading installed packages...
Resolving package dependencies...

The following 3 NEW packages are going to be installed:
  anbox anbox-kmp-default libdbus-cpp5
```

Before you start the container manager, and because of a bug in my package, you need to manually setup the Anbox network. As root run:

```console
/usr/lib/anbox/anbox-bridge.sh start
```

This should not be necessary in the future, but if you look at the `anbox-container-manager.service` systemd file, I have commented the lines that do this:

```console
[Service]
#ExecStartPre=/usr/lib/anbox/anbox-bridge.sh start
ExecStart=/usr/bin/anbox container-manager --privileged
#ExecStopPost=/usr/lib/anbox/anbox-bridge.sh stop
```

Only because `dnsmasq` does not stay alive if `android-bridge.sh` is run from systemd. Probably easy to fix, just not a priority yet.

Now you can start the container manager:

```console
$ systemctl start anbox-container-manager
```

Let's check it is running:

```console
$ systemctl status anbox-container-manager
● anbox-container-manager.service - Anbox Container Manager
   Loaded: loaded (/usr/lib/systemd/system/anbox-container-manager.service; disabled; vendor preset: disabled)
   Active: failed (Result: exit-code) since Sat 2017-04-22 19:20:02 CEST; 5s ago
  Process: 10621 ExecStart=/usr/bin/anbox container-manager --privileged (code=exited, status=1/FAILURE)
 Main PID: 10621 (code=exited, status=1/FAILURE)
      CPU: 16ms
```

The container manager fails because it can't find the Android image. This is expected.

Unfortunately, while building Anbox in the [Build Service](https://build.opensuse.org/) is possible, building the Android image itself is another story. Building Android requires 40G of disk space and it is possibly an order of magnitude more challenging to do it while following the strict building rules of a classic Linux distribution.

So, before starting the Anbox Container Manager, get the Android image and make sure it is in /var/lib/anbox/android.img`.

```console
$ sudo wget -O /var/lib/anbox/android.img http://build.anbox.io/android-images/2017/04/12/android_1_amd64.img
```

Now you can start the container manager:

```console
$ systemctl start anbox-container-manager
```

And now it should be running without issues:

```console
$ systemctl status anbox-container-manager
● anbox-container-manager.service - Anbox Container Manager
   Loaded: loaded (/usr/lib/systemd/system/anbox-container-manager.service; disabled; vendor preset: disabled)
   Active: active (running) since Sat 2017-04-22 19:20:37 CEST; 1s ago
 Main PID: 10725 (anbox)
    Tasks: 9 (limit: 4915)
   Memory: 8.9M
      CPU: 17ms
   CGroup: /system.slice/anbox-container-manager.service
           └─10725 /usr/bin/anbox container-manager --privileged
```

Now you need to start the session manager, as a normal user:

```console
$ systemctl start --user anbox-session-manager
```

```console
$ systemctl status --user anbox-session-manager
● anbox-session-manager.service - Anbox session manager
   Loaded: loaded (/usr/lib/systemd/user/anbox-session-manager.service; disabled; vendor preset: enabled)
   Active: active (running) since Sat 2017-04-22 19:21:12 CEST; 3s ago
 Main PID: 10927 (anbox)
   CGroup: /user.slice/user-1000.slice/user@1000.service/anbox-session-manager.service
           └─10927 /usr/bin/anbox session-manager
```

Now you are ready to run Anbox.

# Running Anbox

Once the session manager is installed, you should see anbox in the launcher (together with other Android applications):

![Anbox launch]({{ site.baseurl }}/assets/images/posts/anbox/anbox-launch.png)

Running the "Anbox" entry should start the application manager:

![Anbox launch]({{ site.baseurl }}/assets/images/posts/anbox/anbox-opensuse.png)

Check if network is working by running the webview:

![Anbox Google]({{ site.baseurl }}/assets/images/posts/anbox/anbox-google.png)

There is, Android running on openSUSE.

And it is not emulated, you can see Android processed running together with other apps, just issolated in a container:

![Anbox launch]({{ site.baseurl }}/assets/images/posts/anbox/anbox-containers.png)

# Installing Software

To install software, you can use the adb tool included in the [Android SDK](https://developer.android.com/studio/index.html). You can download just the command-line tools at the end of the page.

`adb` is in the `platform-tools` directory where you unpacked the SDK.

```console
cd android-sdk/platform-tools
```

```console
$ ./adb devices -l
List of devices attached
XB5A20ZXXX             device usb:1-2 product:D5503 model:D5503 device:D5503
emulator-6663          device product:anbox_desktop_x86_64 model:Anbox device:x86_64
```

The first device is my phone, connected via USB Debugging. The second is Anbox itself.

As there is no Google Play! in the base Android image, I will install [F-Droid](https://f-droid.org/) to get some opensource apps:

```console
$ ./adb -s emulator-6663 install /home/duncan/Downloads/FDroid.apk
Success
```

![F-Droid]({{ site.baseurl }}/assets/images/posts/anbox/anbox-fdroid.png)

If you want to install an app you already have in your phone, get the list of all packages and their paths:

```console
$ ./adb -s CB5A20ZPX7 shell pm list packages
```

Then use the path to get the apk:

```console
$ ./adb -s CB5A20ZPX7 pull /data/app/com.starfinanz.smob.android.sbanking-2/base.apk /tmp/banking.apk
```

# Conclusion

Anbox is not in production quality yet. However, what they have achieved is already impressive.

openSUSE should keep an eye on it, as the level of integration could in theory provide transparent running of Android application and games under Linux.

There are other issues to solve which are not technical, like the Google Play! store. Upstream is [aware of those](http://anbox.io/#faq).
