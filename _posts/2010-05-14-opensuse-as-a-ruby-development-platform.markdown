---
layout: post
title: openSUSE as a ruby development platform
date: '2010-05-14'
tags:
- newsuse
- opensuse
- ruby
- software
- suse
---

  

# openSUSE is a gem for ruby development

In this post I would like to show what openSUSE has to offer to ruby enthusiasts.

The latest openSUSE version ships with ruby 1.8.7, rubygems 1.3.1 and rails 2.3.2. The latest two being not so recent, here is where the openSUSE project shines. Say hello to [devel:languages:ruby](http://download.opensuse.org/repositories/devel:/languages:/ruby) and [devel:languages:ruby:extensions](http://download.opensuse.org/repositories/devel:/languages:/ruby:/extensions) build service projects.

The first is a project containing a more recent 1.8.7 ruby (p249 vs p72 in 11.2). However, as a build service project, it is built on top of multiple targets so you can add this repository not only to 11.2 but also to SLE and older openSUSE releases.

In this project you will also find a ruby19 package which is nothing else than ruby 1.9.1 p376 installable in parallel with 1.8.7.

The [devel:languages:ruby:extensions](http://download.opensuse.org/repositories/devel:/languages:/ruby:/extensions) contains ruby libraries and gems. For example you can find rails 2.3.5 there. Libraries are usually packaged as ruby-something and gems are packaged as rubygem-something. Gem packages have some nice attributes:

- They are visible by the software management stack (rpm ZYpp, and all the ZYpp integrated tools: PackageKit, YaST, etc)
- They are visible to the gem tool. The rpm is installed where the gem tools expects to find it
- They are the best choice if you want distribute a fully packaged application, or an appliance using [SUSE Studio](http://www.susestudio.com/) 
- You can easily create them from a standard gem using the gem2rpm-opensuse script included in the [rubygem-gem2rpm](http://download.opensuse.org/repositories/devel:/languages:/ruby) package

To add the gem repository to your 11.2 system, just do (as root):

```
zypper ar 
http://download.opensuse.org/repositories/devel:/languages:/ruby:/extensions/openSUSE_11.2 rubygems
```

# Living on the edge

While the ruby environment provided by openSUSE is great, you may want to go one step further. What comes to my head:

- If you have both ruby and ruby19, you would need to have a different package for each ruby interpreter
- Sometimes you want to try a really experimental virtual machine, however openSUSE offers no package for it (eeer, and experimental VMs make packaging difficult, especially if the developers are MacOS users whose build script download prebuilt binaries of LLVM over the internet eeeek!)
- If you run or develop applications in the same machine, you may want to isolate the gem environment for each applications
- You may want quickly to test a program with different interpreters.
- You may not have root access! And sadly quacking like an administrator won’t give you superpowers.

## Enter rvm, the ruby version manager

rvm is a nice tool that can quickly compile ruby interpreters from source and switch between them, all without root access (you can also set the current interpreter to the “system” one). Gems you install for one interpreter are isolated from the other interpreters, and you can create “gem sets”.

While rvm can be installed really easily:

```
bash < <( curl http://rvm.beginrescueend.com/releases/rvm-install-head )
```

for the HEAD version in github or

```
bash < <( curl http://rvm.beginrescueend.com/releases/rvm-install-latest )
```

You can also install it the “openSUSE way”, if you added the devel:languages:ruby:extensions repository:

```
zypper install rubygem-rvm
```

Once you have it installed, you will need to run rvm-install (as user) and edit your shell profile so that the environment is set correctly.

Then you can start using it. To see all the interpreters available in your system:

```
rvm list
```

To see all the ones known to rvm:

```
rvm list --all
```

Build 1.9 from source:

```
rvm install ruby-1.9.2-head
```

Build JRuby:

```
rvm install jruby
```

Use jruby:

```
rvm use jruby
```

Then you can “double check”:

```
ruby -v
```

jruby 1.4.0 (ruby 1.8.7 patchlevel 174) (2009-11-02 69fbfa3) (Java HotSpot(TM) 64-Bit Server VM 1.6.0\_20) [amd64-java]

## Combine rvm with the powerful bundler

Of all the new ruby tools, the ones I find most useful is the [bundler](http://github.com/carlhuda/bundler), which was created as a rails independent solution to make ruby applications specify the gems they need in their environment.

You can create a Gemfile in the top level directory of your application:

Something like.

```
source 'http://gemcutter.org'
gem "rails", "3.0.0.beta3"
gem "runt", :git => "http://github.com/tevio/runt.git"
```

As you can see, you can specify gems from git repositories, or use specific branches or versions. This is really nice for deployment, as you can get the working environment really easy.

```
bundle install
```

Would install the missing gems. It would reuse system gems, etc. If you are using rvm, it would install them in the interpreter specific gems.

The bundler has more advanced features, like the ability to take all the gems and “bundle” them directly “in” the application, but I leave that as an exercise to the reader.

# Conclusions

So that finishes this post. I hope you could see how openSUSE looks like for a ruby developer.

I would like to first thanks [Marcus Rückert (a.k.a darix)](http://news.opensuse.org/2008/03/01/people-of-opensuse-marcus-rueckert/) as he was the main brain behind the rubygem packages. Also big thanks to the [YaST](http://en.opensuse.org/YaST), [Klaus](http://kkaempf.blogspot.com/), [SUSE Studio](http://www.susestudio.com/) and [Build Service](http://build.opensuse.org/) teams, whose development resulted in many package updates and contributions.

