---
layout: post
title: Control Center in System Settings?
date: '2008-12-17'
tags:
- newkde
- newsuse
- qt
- software
- suse
- yast
---

  

## Comments

After posting [A refactoring journey: the control center](http://duncan.mac-vicar.com/blog/archives/444) yesterday, I got some comments quickly.

Of course I got some comments like “The category icons are too small” “Spacing is wrong”. When it was clearly stated at the end of the post:

> Note that none of these screenshots mean “this is how the control center will look”. The focus is still on the infrastructure that enables us to try more radical approaches. For example, the old control center look and feel is trivial to emulate.

However, there was another series of comments:

> Why don’t you add the yast options into the KDE settings?

I think the answer to this question is the same as the inverse. Why should we?

```
ls /usr/share/applications/YaST2/ | wc -l
85
```

There are 85 modules in my system. Well, not all visible. I don’t have all available installed either. But, would you like to have suddenly 85 icons in this view?

![KDE4 System Settings]({{ site.baseurl }}/assets/images/posts/1.png)

## What do others do?

Now, Marçal Juan comment:

> Just think in Apple and Microsoft approach… it’s all in the “System settings” not two independent apps.

This is simply not the case. I wonder why Marçal thinks they are mixed.

I think there is a simple reason why they are separate. If you had to put so many items, the control center can’t be read by our brain in one shot anymore . “but lets create sub-items””… yeah, and then you are again back in the kcontrol tree times, where finding something was hard if you don’t know at which deep-level it was. Yeah, our readers/commenters solve any usability problem by making things a tree or adding tabs :-) .

Actually, Apple has a System Preferences panel:

![leopard preferences]({{ site.baseurl }}/assets/images/posts/leopard-preview-prefs-24.png)

For server related settings, they have a separate console (Still note how User accounts is present on both):

![leopard server preferences]({{ site.baseurl }}/assets/images/posts/131454-leopardserver1.jpg)

Not only Apple. Marçal is also wrong with Microsoft, which has a control panel:

![windows control panel]({{ site.baseurl }}/assets/images/posts/control-panel.gif)

And also a server console (this time with a completely different look:

![windows server panel]({{ site.baseurl }}/assets/images/posts/SBS_ServerManagementDiskManagement.jpg)

## Why?

I think the temptation to merge them comes from two sources:

- 

The fact that most Linux user tend to mix the server/advanced administration and basic tasks a lot, and therefore they think their usecase is the common one.

- 

This makes them think that everyones wants to go trough the basic settings when configuring some advanced service.

- 

Actually the [last YaST survey revealed](http://files.opensuse.org/opensuse/en/9/91/YaST-SurveySummary_11172007.pdf) that the only tasks an user does often is software management and in second place, network management. All the rest is done seldom.

- 

The fact that Linux lacks some basic administration tools upstream or integrated in the desktop, which YaST provides right now.

I think the last two ponts are relevant. The real problem is not whether you have all icons in one control panel, but where are those icons. Is software management a basic task or it goes together with virtualization settings? That is the problem. Windows and Apple provide software management and network management in the basic settings. However, I think this is only a problem for those specific YaST modules that are used to often, and both cases have a solution.

In the case of Software Management, PackageKit already offers basic software management integrated with the desktop. I do have an icon “Add/remove software” in my KDE System Settings (KPackageKit provides it). The case of networking is also solved since long time, and most desktop users configure their network in the system tray with knetworkmanager.

Any other configuration that becomes common for a desktop user has to be integrated with the desktop in the basic settings, or even further, in the right context (for example, enabling sharing a folder by right clicking the folder instead of going to the system settings ).

Right now we have some technical limitations. We can’t just put the YaST modules in system settings without doing some changes in that code, and we can’t offer all functionality so it can be integrated in the right context. But we are moving into that direction, and that really does not means we have to go and put 80 new icons and ruin the KDE4 control center concept or reintroduce hard to remember categories.

We will do some experiments about it anyway.

**Update 1:** _As Stano points out, it may make sense to offer access to YaST in the “Computer Administration” category of the System Settings, not module per module, but as one icon. (then launch YaST control center, or embed it). What we can’t do is to embed the modules themselves in the system settings (we did this in the past and was problematic as modules have wizard semantics)._

