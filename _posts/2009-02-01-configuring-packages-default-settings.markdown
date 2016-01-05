---
layout: post
title: Configuring package's default settings
date: '2009-02-01'
tags:
- debian
- newsuse
- software
- suse
- yast
---

The last weeks I heard twice about the idea of having something like [debconf][1] to configure package default settings. Now I just notices a [blog post from Lars about it][2].

When such ideas appear, I always pay attention, because there we may miss some points:

* Look out of the box. We end with our own custom way to do things, sometimes really coupled with YaST (which is very different to \*integrated\* with YaST).  
* No research is done about existing solutions, their mistakes and advantages.  
* One weakness is not enough to invent something new. People forget the missing feature fast. Compatibility brings more value than one missing feature: You gain tools and existing knowledge.  
\*\* For example, we switched to the updateinfo.xml format, which does not support dependencies. People complained, but till now nobody has missed them. However the value is that you can use tools like [Bodhi][3] with openSUSE.  
* You can still innovate, and add value, by integrating it with your already valuable stuff.

So lets put this feature in perspective. The main use case (extracted from Lars post is):

* Allow the package to define settings  
* Ask for those settings at installation (or later).

There is even a "mockup" or how it could look like in a new xml format.

Lets [look at debconf][1], just an overview, to see what it does:

* It supports questions with the following types: string boolean select multiselect note text password  
* The myth that it break non-interactive install is not completely true. There is a noninteractive "frontend".  
* It has frontends for command line, KDE, Gnome, and a ncurses/dialog like.  
* Packages provide templates, like:

```
Template: foo/like_debian
Type: boolean
Description: Do you like Debian?
 We'd like to know if you like the Debian GNU/Linux system.

Template: foo/why_debian_is_great
Type: note
Description: Poor misguided one. Why are you installing this package?
 Debian is great. As you continue using Debian, we hope you will
 discover the error in your ways.
</pre>

* Packages provide scripts. Script get a minimal and simple API to get the values of the answered templates, and apply them. So you can write them in sh and perl (and may be others).

<pre class="prettyprint">
#!/bin/sh -e

# Source debconf library.
. /usr/share/debconf/confmodule

# Do you like debian?
db_input medium foo/like_debian || true
db_go

# Check their answer.
db_get foo/like_debian
if ["$RET" = "false"]; then
    # Poor misguided one..
    db_input high foo/why_debian_is_great || true
    db_go
fi
</pre>

* There is even the concept of "shared settings":

<pre class="prettyprint">
Template: shared/window-manager
Type: select
Choices: ${choices}
Description: Select the default window manager.
 Select the window manager that will be started by default when X
 starts.
```

So, the question is, if till now nobody has additional requirements, why invent a new xml format, or force people to code wizards in ycp?

* Why not use exactly the \_SAME\_ template format. Hey packager! You can copy the templates from Debian, just as you copy patches from Gentoo today ;-)  
* Why not add the glue with YaST in the script part (so the scripts would be sh, perl or ycp with a \_MINIMAL\_ api)

If the template format is the same, we could also expect Fedora people could also start using it (of course we haven't yet talked to them, and then we ask ourselves why every distribution has its own \<insert-whatever-here).

If the scripts are integrated with YaST, other distributions can implement them in a different way, but with the same minimal API.

Also, hiding the whole YaST infrastructure keeps the concept simple and prevent packagers to start doing evil stuff from these scripts, and even better: the templates are not programs, or ycp maps, but documentation/knowledge which happens to be read from YasT code.

[1]: http://www.fifi.org/doc/debconf-doc/tutorial.html  
[2]: http://lizards.opensuse.org/2009/01/27/writing-wrapper-packages-for-server-applications-or-a-generic-yast-module  
[3]: https://fedorahosted.org/bodhi

