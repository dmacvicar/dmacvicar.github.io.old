---
layout: post
title: 'zypper feature: source and automatic build dependencies install'
date: '2008-08-18'
tags:
- packagekit
- software
- zypp
- zypper
---

There are features that are present since some time, but people just don't know about it. This time I will mention one that is very useful to developers.

Imagine you are a contributor to foo, which is also available to the distribution as foo.rpm. However, you install the distro from packages, but compile your foo source tree by hand. Compiling foo requires lot of packages.

zypper to the rescue. If foo is in the distribution, make sure you have the main distribution repository added, plus the source repository. Source packages have a nice feature: their Requires, are the BuildRequires of the binary package.

The source install command (si) has a -d option to install only the dependencies, and not the source package itself. So lets say we want to compile PackageKit, lets install all the compile dependencies:

# sudo zypper si -d PackageKit  
 Reading installed packages...

The following NEW packages are going to be installed:  
 opensp openjade gtk-doc docbook\_3 docbook-dsssl-stylesheets sqlite-devel

Then I can go and compile:

PackageKit\> ./autogen.sh --libdir=/usr/lib64  
 --prefix=/usr --enable-developer --with-security-framework=dummy  
 --sysconfdir=/etc --with-dbus-sys=/etc/dbus-1/system.d  
 --enable-zypp --with-default-backend=zypp

Note, you \*do\* need the source repository added in order for this to work, otherwise zypper will not be able to find a matching src.rpm package to read the dependencies from.

