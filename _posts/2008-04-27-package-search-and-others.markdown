---
layout: post
title: Package search and others
date: '2008-04-27'
tags:
- newsuse
- packagemanagement
- suse
- yast
- zypp
---

### YaST ideas and research

We moved the summer of code ideas, and other projects to the [YaST/Research][9] page. We would like to find all the experiments that are lying around in the tmp and experimental branches and put them together in that page so previous attempts and knowledge is not lost.

### Installation Branding

Some information on branding the installation is available on [YaST/Tips#Branding\_the\_installation][1].

### Package groups

The first thing that caught my attention in the rpm world was [package groups][8]. It is a 4 deep level tree which has become pretty useless with time. They are also not consistent across rpm distributions. I agree that they are interesting metadata (because they contain keywords), but nothing you want to display in a user interface.

So I started to ask people around. Do you use the groups tree?. "YES" was the first reply. I was surprised. Were my initial guesses wrong?. "I use it to select the zzzAll group, which is the only way to select all packages". Interesting point, any effort to change this should take that into account.

Luckily all the answers were the same. The famous "zzzAll" group was the only reason why people used that filter.

So when looking for a way to make that screen more easy to the eyes, [PackageKit][6] groups seemed like the way to go, as they are already in use in some applications.

That is how package groups looked like before:

[![rpm groups before][2]][3]

After matching the rpm groups to PackageKit groups, this is the result. Easy.

[![rpm groups after][4]][5]

Notice that "All packages" is still there. Also there is a recommended and suggested groups. The interesting thing is, that if you look at other applications, like Gnome PackageKit install, which is a braindead application (no features) compared to YaST, but still, now they look pretty consistent with each other:

![packagekit search][7]

Before you cry for your old 4 level deep tree, you can still reach the groups via search. Search will look into the groups field and also the generated keywords in the metadata (tags).

### Package search

Now uses the sat solver APIs to query the solv files, which is much faster and we have transparent access to more attributes.

[1]: http://en.opensuse.org/YaST/Tips#Branding_the_installation  
[2]: http://files.opensuse.org/opensuse/en/thumb/d/d3/Rpmgroups-before.png/800px-Rpmgroups-before.png  
[3]: http://files.opensuse.org/opensuse/en/d/d3/Rpmgroups-before.png  
[4]: http://files.opensuse.org/opensuse/en/thumb/0/02/Rpmgroups-after.png/800px-Rpmgroups-after.png  
[5]: http://files.opensuse.org/opensuse/en/0/02/Rpmgroups-after.png  
[6]: http://www.packagekit.org/  
[7]: http://www.packagekit.org/img/gpk-application-search.png  
[8]: http://forgeftp.novell.com/susesdk/docs/SUSE%20Package%20Conventions/spc_rpm_groups.html  
[9]: http://en.opensuse.org/YaST/Research

