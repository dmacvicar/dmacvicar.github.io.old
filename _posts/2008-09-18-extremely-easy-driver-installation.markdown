---
layout: post
title: Extremely easy driver installation
date: '2008-09-18'
tags:
- newkde
- newsuse
- packagekit
- software
- suse
- zypp
---

We have something really cool in.

## How it works, usecase and experience.

The usecase. I have a webcam, but it does not work, because it requires the quickcam-kmp-default package. But I don't know that.

![The webcam]( [http://www.suse.de/~dmacvicar/driverinstall/webcamphoto1-s.jpg)]({{ site.baseurl }}/assets/webcamphoto1-s.jpg)

![Desktop]( [http://www.suse.de/~dmacvicar/driverinstall/driver-install1-s.png)]({{ site.baseurl }}/assets/driver-install1-s.png)

You are in your desktop. You can see in the tray the applet telling you that there are security updates to install.

![Tray Icon]( [http://www.suse.de/~dmacvicar/driverinstall/driver-install-tray-icon-updates....]({{ site.baseurl }}/assets/driver-install-tray-icon-updates.png)

Now I connect the webcam to the computer:

![Webcam connected]( [http://www.suse.de/~dmacvicar/driverinstall/webcamphoto2-s.jpg)]({{ site.baseurl }}/assets/webcamphoto2-s.jpg)

Notice the tray icon. It went from "There are security updates available" to a "hardware" icon (we will add a more visible notification too).

![Tray Icon]( [http://www.suse.de/~dmacvicar/driverinstall/driver-install-tray-icon.png)]({{ site.baseurl }}/assets/driver-install-tray-icon.png)

Now you click on the icon and you see:

![Install Dialog]( [http://www.suse.de/~dmacvicar/driverinstall/driver-install4.png)]({{ site.baseurl }}/assets/driver-install4.png)

You click install, and after 10 seconds quickcam-kmp-default is installed.

### See it live

Do you want to see it live?. I did a recording of the process as a [flash movie][3].

## Background

Since openSUSE 10.1, [ZYpp][5] has the ability to recommend packages based on drivers and other useful system information. Packages can supplement any namespace, which is in turn evaluated at solving time. This allows to automatic select drivers on installation, based on the machine hardware, for example.

You could also plug new hardware, and call

# zypper up

And that would recommend you to install the right drivers.

However this functionality was not used to its own potential. What we really wanted here was to recommend packages when hardware was plugged.

With [PackageKit][4], filling that gap was possible, as we can easily talk to [ZYpp][5] from the desktop over dbus, using an abstracted interface.

So in the last weeks, [Stefan Haas][1] implemented support for this in our PackageKit ZYpp backend. [Thomas Goettlicher][2] added the needed glue in the kupdater applet. That is, listening to added devices events, and calling PackageKit to let ZYpp recommend new hardware.

Yesterday I sat to see it working. Some small one liners prevented it to work, but after some tweaking in the PackageKit backend, it worked really well. Thanks for everyone putting the pieces together.

[1]: http://en.opensuse.org/User:Haass  
 [2]: http://en.opensuse.org/User:Tgoettlicher  
 [3]: http://www.suse.de/~dmacvicar/driverinstall/drivers.swf  
 [4]: http://www.packagekit.org  
 [5]: http://en.opensuse.org/ZYPP

