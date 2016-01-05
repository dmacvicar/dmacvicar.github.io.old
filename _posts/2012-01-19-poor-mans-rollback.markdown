---
layout: post
title: Poor man's rollback
date: '2012-01-19'
categories:
- Software
tags:
- opensuse
- suse
- zypp
- zypper
---

By [Andrew Wafaa](http://www.wafaa.eu/)'s [request](https://twitter.com/#!/awafaa/status/160030399135883265).

Save the package list:

`
dmacvicar@piscola:~> rpm -qa --queryformat="%{name}\n" > 1
`

Do something... like uninstalling what was cool last week and it is not cool anymore:

`
dmacvicar@piscola:~> sudo zypper rm erlang
Loading repository data...
Reading installed packages...
Resolving package dependencies...`

The following packages are going to be REMOVED:  
 erlang rabbitmq-server

2 packages to remove.  
After the operation, 51.3 MiB will be freed.  
Continue? [y/n/?] (y): y  
Removing rabbitmq-server-2.2.0-1.2 [done]  
Removing erlang-R14B-1.2 [done]  
There are some running programs that use files deleted by recent upgrade. You may wish to restart some of them. Run 'zypper ps' to list these programs.

Save the new state:

`
dmacvicar@piscola:~> rpm -qa --queryformat="%{name}\n" > 2
`

Now you need to know that zypper accepts + and - in its input. You can install and uninstall packages in one go:

`
zypper in -- +pkg1 -pkg2 +pkg3 ...
`

So we can diff both files:

`
dmacvicar@piscola:~> diff -u 1 2
--- 1	2012-01-19 17:23:26.640180000 +0100
+++ 2	2012-01-19 17:24:43.196248000 +0100
@@ -420,7 +420,6 @@
 gnome-themes-accessibility
 libeet1
 icc-profiles-mini
-rabbitmq-server
 kde4-filesystem
 gpg-pubkey
 libiptcdata-lang
@@ -3561,7 +3560,6 @@
 perl-Config-General
 PolicyKit-devel
 gtk2-engine-aurora
-erlang
 libeet-devel
 cyrus-sasl-gssapi
 libimobiledevice2
`

Close to what we need. We remove the context lines by using -u0 and we remove the 3 first lines:

`
dmacvicar@piscola:~> diff -u0 1 2 | grep -Ev '^(@@|\+\+|--)'
-rabbitmq-server
-erlang
`

Now feed zypper with this to get your packages back:

`
zypper in -- $(diff -u0 2 1 | grep -Ev '^(@@|\+\+|--)' | xargs)
`

Of course this only will work if you have all repositories. It is also useful to sync packages across computers (like you get a new laptop and need to setup it in a similar way).

openSUSE 12.1 rollback is implemented using btrfs via snapper, plus a zypp plugin that records a snapshot on every commit. It should be possible to write a Poor's man version by recording the package list on every commit and then performing the above operation to go one or more steps back.

