---
layout: post
title: yum and ZYpp speed / memory usage
date: '2008-05-17'
tags:
- newsuse
- software
- suse
- yast
- yum
- zypp
---

Michael Zucchi [complains][1] about yum memory usage, and points python as guilty.

> Yum isn’t so yummy after-all. Re-enabled python so i could run yum. Wow 120mb of vm to install a couple of packages. Not bad considering the box only has 128mb. This is crap.  
>   
> Hmm, should I try xubuntu - or will it be just as crappy and bloated and blighted by python poo?

Since our efforts to make the ZYpp really fast, by incorporating and integrating Michael Schroeder's sat solver together with Michael Matz's great work on the solv files and data storage, I never took the time to make a "quick comparison" on speed or memory usage. So lets have a quick look.

These are my repositories:

```
Software configuration management (openSUSE_10.3)
10.3 - Main Repository (NON-OSS)
10.3 - Packman
openSUSE-10.3-Updates
Virtualization:VirtualBox
home:dgollub
KDE:KDE3
Mozilla based projects (openSUSE_10.3)
ZYPP SVN Builds (openSUSE_10.3)
ZYPP SVN Builds (openSUSE_10.3)
home:prusnak
10.3 - VideoLan
openSUSE.org tools (openSUSE_10.3)
SUSE Feature Tracking Tool (openSUSE_10.3)
psmt's Home Project (openSUSE_10.3)
openSUSE:10.3
Duncan Mac-Vicar SUSE rpms (openSUSE_10.3)
Latest YaST svn snapshots (openSUSE_10.3)
building/openSUSE_10.3
```

All these repos together are about 41.000 packages.

What I did was to symlink ZYpp repositories to the yum repo path so they use the same repos.

```
# rm -rf /etc/yum.repos.d/
# ln -s /etc/zypp/repos.d /etc/yum.repos.d
```

\*NOTE:\* I tested with yum 3.2.4. I know 3.2.14 is available, but that is what I had installed when doing the test. After doing this tests I upgraded to 3.2.14 but it did not accept my .repo file because the character ":" in repo names. However the changelog of yum since 3.2.4 shows:

If using latest yum would invalidate this numbers (not as in 1 second, but as in an order of magnitude), let me know and I will repeat them when I make them work with my repo files.

\*\*Update 14.05.2008 : I did add yum 3.2.14. However it performed even worse, except for memory usage.\*\*

\*\*Update 15.05.2008 : added smart 0.52 numbers\*\*

libzypp is the one you see in factory since some days: 4.21.1.

yum and ZYpp behave differently, as yum downloads and parses filelists.xml and other.xml which we ignore. There fore I skipped the download metadata part and just timed the cache building process.

```
# yum clean dbcache
...
19 sqlite files removed

# time yum makecache
...
Metadata Cache Created

real 9m41.036s
user 2m34.766s
sys 0m11.545s
```

Almost 10 minutes. As this time includes parsing the two big files we ignore. I did it again, pressing Ctrl-C After yum finished with the primary data, which is what ZYpp uses:

```
# time yum makecache
...
Exiting on user cancel

real 4m6.730s
user 0m34.058s
sys 0m3.080s
```

Now, zypper's turn:

```
# time zypper ref -B
...
All repositories have been refreshed.

real 0m18.472s
user 0m16.029s
sys 0m2.024s
```

So yum takes 13 times the ZYpp needs technically (primary 1:1 comparison), but 30 times the time the end user sees.

Now, installing a package. Times were measured till the "continue? yes/no" prompt, or till the first interactive question.

```
# time yum install fate
...
Is this ok [y/N]: n
Exiting on user Command
Complete!

real 0m19.143s
user 0m14.057s
sys 0m1.920s
```

zypper's turn:

```
# time zypper in fate
...
Continue? [YES/no]: n

real 0m9.796s
user 0m8.509s
sys 0m0.624s
```

This time, ZYpp is only twice as fast as yum. Only ;-)

What happens when you want to upgrade your packages?

```
# time yum upgrade
...
real 0m45.152s
user 0m36.894s
sys 0m7.476s
```

(Note: yum did not even found a solution here).

```
# time zypper update
...
Continue? [YES/no]: n

real 0m8.988s
user 0m7.820s
sys 0m0.596s
```

yum needs 4 times the time ZYpp needs to calculate the upgrade.

\*\*Update 14.05.2008 : I was comparing update to upgrade, I fixed those numbers in the chart. However, I don't have the update value for the old yum.\*\*

Summary:

 ![]({{ site.baseurl }}/assets/images/posts/media_httpspreadsheet_vgtuz-scaled1000.jpg)

Now, how much memory does each one need? For this, I just tested the install command with one package using valgrind massif, a heap profiler.

yum memory usage:

[![yum memory usage][2]][2]

ZYpp memory usage:

[![zypp memory usage][3]][3]

\*\*Update 14.05.2008 : memory usage chart for yum 3.2.14\*\*

[![yum 3.2.14 memory usage][4]][4]

\*\*Update 15.05.2008 : memory usage chart for smart 0.52\*\*

[![smart 0.52 memory usage][5]][5]

Here you can appreciate ZYpp goes a little bit over 20M, while yum goes over 180M, so yum uses about 9 times more memory. \*\*Update 14.05.2008 : yum 3.2.14 uses around 160 in the worst point of time.\*\*

 ![]({{ site.baseurl }}/assets/images/posts/media_httpspreadsheet_iahnc-scaled500.jpg)

I would be interested in tracking cpu usage too, but that will come later. What do you think about it?

[1]: http://blogs.gnome.org/zucchi/2008/05/10/linux-is-bloated/  
 [2]: http://files.opensuse.org/opensuse/en/e/ea/Yum-in-massif.png  
 [3]: http://files.opensuse.org/opensuse/en/6/65/Zypper-in-massif.png  
 [4]: http://files.opensuse.org/opensuse/en/2/26/Yum-3214-massif.png  
 [5]: http://files.opensuse.org/opensuse/en/f/fa/Smart-in-massif.png

