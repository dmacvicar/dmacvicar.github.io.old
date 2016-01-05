---
layout: post
title: Easy packaging of ruby gems for openSUSE
date: '2009-03-17'
tags:
- newsuse
- ruby
- software
- suse
---

Every distribution has its own conventions to package scripting languages addons (perl modules, ruby gems, etc) as native packages. This allows packages to depend on those addons honoring the package dependencies, and at the same time, look like the addon was installed the scripting language tool (cpan, gem).

openSUSE keeps a [repository of gems][2], thanks to [Marcus Rueckert][5].

For ruby, David Lutterkort ([augeas][4] lead developer) created a [nice tool][1] to help converting gems into spec files.

I created a template for openSUSE-like rubygem-\* packages, and David [committed it upstream][3].

So, to create a package for a gem:

Fetch it:

gem fetch foo

Convert it:

gem2rpm -t opensuse.spec.template ./foo-1.1.gem \> rubygem-foo.spec

Build and tweak it. Make sure everything is alright. Some gems work out of the box, some not, but still gem2rpm saves 90% of the effort. Consider submitting it to the [project][2] if you are willing to keep it up to date ;-)

[1]: http://rubyforge.org/projects/gem2rpm  
[2]: http://download.opensuse.org/repositories/devel:/languages:/ruby:/extensions  
[3]: http://gem2rpm.rubyforge.org/svn/trunk/opensuse.spec.template  
[4]: http://www.augeas.net  
[5]: http://en.opensuse.org/User:Darix

