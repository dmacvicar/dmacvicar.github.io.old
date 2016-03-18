---
layout: post
title: Not sweet but salty, SUSE Manager 3 Technical Backstage Part I
date: '2016-03-16'
categories:
- Software
tags:
- java
comments: true
hide: true
---

## Introduction

During the last year we have been working on SUSE Manager 3, the next release of [SUSE's Systems Management product](https://www.suse.com/products/suse-manager/), which includes additional capabilities around Configuration Management and Complaince. This article details this journey from the team's perspective that may be of interest of product enthusiasts and developers.

## Sweet did not last

In mid-2014 I wrote about [SUSE Manager 2.1]({% post_url 2014-06-11-suse-manager-2-1 %}). It was an important release for us because at that point we became very active upstream, to the point that on an usual day, a considerable chunk of the open pull requests came from SUSE, including a refreshed mobile-enabled user interface.

But the world is changing, and management offerings are taking new innovative directions, especially around Configuration Management and Compliance. This trend was in several dimensions quite radical for our product, based on the [Spacewalk](http://spacewalk.redhat.com/) project, which has been managing Linux system with the same paradigm since 2008.

Deploying a management solution is a considerable investement for the customer: operation, training, processes, etc. Telling the customer you will completely replace their deployed solution because new trends appeared means you are throwing all these efforts to the trash.

On the other hand, it is very easy to find excuses to rewrite software from scratch and there are [well-known industry voices giving advice](http://www.joelonsoftware.com/articles/fog0000000069.html) on the topic.

So we set ourselves the challenge, can we evolve SUSE Manager iteratively into this new world without hurting our customer investments?.

## Choosing a configuration management engine

After we decided that we would bring this new world into the existing SUSE Manager, we had very clear that we would not write a configuration management engine ourselves: there were multiple opensource projects that were up to the task. We also understood that customers could be already invested into one of them, and that is fine: we were looking for the one that will be *tightly* integrated into SUSE Manager and whose design match or vision and which implementation become our engine.

And here is where things got even less sweet: they got *salty!*

We ended choosing [Salt](http://saltstack.com/community/). The reasons are endless: From its vibrant community to the "aha!" moment you get when understanding its internals and seeing the vision behind the architecture. It is a complete toolkit to build datacenter automation, wrapped in usable tools with sane defaults that work out-of-the-box.

> SaltStack platform or Salt is a Python-based open source configuration management software and remote execution engine. Supporting the "Infrastructure as Code" approach to deployment and cloud management, it competes primarily with Puppet, Chef, and Ansible. (Source: [Wikipedia](https://en.wikipedia.org/wiki/Salt_(software))]

> SaltStack software orchestrates the build and ongoing management of any modern infrastructure. SaltStack is also the most scalable and flexible configuration management software for event-driven automation of CloudOps, ITOps and DevOps. (Source: [SaltStack](http://saltstack.com/))

We did not get the same impression from the other tools we evaluated. It was clear that Salt fit into the SUSE Manager present and future.

The fact that Salt implements configuration management (states) on top of the [execution engine (state.apply)](https://docs.saltstack.com/en/latest/ref/modules/all/salt.modules.state.html), and that the executions modules are first class, we could offer almost all features we already had in SUSE Manager and naturally extend to the Configuration Management part. On top of that, Salt is writen in python, and the current Spacewalk client stack and partly the server stack are also Python components.

Things only got better from there. During all this time we got really crazy about Salt. Our sysadmins are using it as well and a community was formed internally that went beyond our product. Not only with sysadmins and engineers but even our Product Manager writes [beacons](https://docs.saltstack.com/en/latest/topics/beacons/).

### Suminator

The first serious dogfooding of Salt we did was the birth of *Suminator*. [Silvio](https://github.com/moio) combined Vagrant with Salt to build a small tool that allowed us to build release and development versions of SUSE Manager and vanilla Spacewalk servers and clients in all possibly code stream and operating system combinations. Very soon it became the tool of choice for developers to work in the codebase.

This tool is still tied to our internal build service and package repositories, but we hope we find time to make it useful to others as well.

>If you want to play with openSUSE and Salt using [Vagrant](https://www.vagrantup.com/), I have published [a repository](https://github.com/dmacvicar/salt-opensuse-playground) that will get you started.

### SUSECon 2015
The awesomeness of Salt started to click at various levels. The idea of orchestration built on the concept of [Event-Driven Infrastructure](https://docs.saltstack.com/en/getstarted/event/index.html) plus the support for [dumb devices](https://docs.saltstack.com/en/latest/topics/proxyminion/index.html) derived in the great SUSECon demo.

![SUSECon 1]({{ site.baseurl }}/assets/images/posts/2016-03-16-susemanager-3-backstage/susecon-susemanager-1.png)

At SUSECon we showed that with the concept of reactive infrastructure, it was trivial to react to changes in configuration drift, in this case, using an inotify beacon that had "knowledge" about the managed files on that system, and make SUSE Manager react. On top of that we interacted with [smart lights](http://www2.meethue.com) via [proxy minions](https://docs.saltstack.com/en/latest/topics/proxyminion/index.html), just like you will have to do once the [IoT](https://en.wikipedia.org/wiki/Internet_of_Things) takes over the world.

![SUSECon 1]({{ site.baseurl }}/assets/images/posts/2016-03-16-susemanager-3-backstage/susecon-susemanager-2.png)

On top of that we showed that we could achieve all using the power of opensource that SUSE has been doing for almost 25 years. Support for [Hue lights in Salt](https://docs.saltstack.com/en/develop/ref/proxy/all/salt.proxy.philips_hue.html) was already upstream by the time of that demonstration.

## Refreshing our platform

Working on a new release means the oppotunity to refresh the platforms and technologies you use, and to look for better alternatives for some of them.

* We keep rebasing and picking up enhancements from Spacewalk [upstream](https://github.com/spacewalkproject/spacewalk).
* A mature codebase does not mean you should not get rid of code. Eg: here is a pull request from the team to [remove 30k lines](https://github.com/spacewalkproject/spacewalk/pull/280) of code that did not make much sense nowadays.

With the Salt and Compliance work there was going to be new code writen, and that is an opportunity for choosing the right platforms and frameworks.

* From SLE-11-SP3 to SLES-12-SP1
* From Tomcat 6.x to Tomcat 8.x
* From Java 7 to Java 8
* We started to use [Spark](http://sparkjava.com/) for server-side on the Java side.
* We started to use [React](https://facebook.github.io/react/) for client side.

## Integrating Salt into SUSE Manager

The first attempt as done as part of [Hackweek 11](https://hackweek.suse.com/11/projects/514). A protoype known as [Saltwalk](https://github.com/SUSE/spacewalk-saltstack) was born.

This protoype (a simple python reactor) helped figuring out what the bulk of the work would be, the non-trivial parts and what decisions we needed to take to move forward.

The basic architecture of a reactor that handles Salt events and interact with Spacewalk was in place. What we needed now is a way for Spacewalk to interact with Salt.

![SUSE Manager 3 Salt Architecture 1]({{ site.baseurl }}/assets/images/posts/2016-03-16-susemanager-3-backstage/suma-salt-architecture.png)

### salt-netapi-client

For the interaction of SUSE Manager with Salt, a [Salt client library](https://github.com/SUSE/salt-netapi-client) for Java was created, which allows to consume Salt functionality through [salt-api](https://docs.saltstack.com/en/latest/ref/cli/salt-api.html).

Months after the [original announcement](https://groups.google.com/forum/#!topic/salt-users/YdMgcUWiWw8), `salt-netapi-client` keeps being the best option available to interact with [Salt from Java](http://suse.github.io/salt-netapi-client/docs/master/overview-summary.html).

>We pointed applicants to [our open positions](https://attachmatehr.silkroad.com/epostings/index.cfm?fuseaction=app.allpositions&company_id=15495&version=6) to [salt-netapi-client](https://github.com/SUSE/salt-netapi-client) as a challenge. Various contributors to the library became SUSE Manager team members!.

### Becoming a Salt master

When the decision of using Salt was clear, it was decided that we would do the integration code on the Java side of SUSE Manager, and so Saltwalk stayed as a protoype, and the functionality was implemented in Java.

At this point, SUSE Manager default installation was at the same time a full fledged [Salt master](https://docs.saltstack.com/en/latest/ref/configuration/master.html) server.

>A consequence of this is that you can enjoy Salt on openSUSE out of the box. The [Salt package](https://software.opensuse.org/package/salt) is kept updated on [Tumbleweed](https://en.opensuse.org/Portal:Tumbleweed) and [Leap](https://software.opensuse.org/421/en), which fits very well with the fact that openSUSE is available in various Cloud Providers Eg. [#1](https://cloud.google.com/compute/docs/operating-systems/linux-os#opensuse) out of the box.

### Registering minions

The next step was to make SUSE Manager aware of minions. First by registering them as they appeared (after `salt-key --accept` was done), so that they show up together with traditional systems:

![Minion Clients Updates 1]({{ site.baseurl }}/assets/images/posts/2016-03-16-susemanager-3-backstage/minion-clients-2.png)

After we did work in order to retrieve the inventory data using Salt itself, the details page of minions were also available:

![Minion Overview 1]({{ site.baseurl }}/assets/images/posts/2016-03-16-susemanager-3-backstage/minion-overview-1.png)

Once this was working we improved on it allowing to operate the `salt-key` functionality directly from the user interface. Once a minion key needs to be accepted you would see it in the overview page:

![Minion Pending 1]({{ site.baseurl }}/assets/images/posts/2016-03-16-susemanager-3-backstage/pending-minions-1.png)

And from there you can accept which ones to onboard:

![Minion Onboarding 1]({{ site.baseurl }}/assets/images/posts/2016-03-16-susemanager-3-backstage/minion-onboarding-1.png)

### Configuration Management with SUSE Manager

While you can patch, install packages in the same way you did with traditional clients, there are two main differences:

* What you do is instantaneous (unless you schedule for later)
* Instead of doing imperative actions (eg. install this package), you can also use states to define "what should be installed" in a declarative way using the [power of States](https://docs.saltstack.com/en/latest/topics/tutorials/starting_states.html).
* You can write plain `sls` data in custom states

![State Catalog 1]({{ site.baseurl }}/assets/images/posts/2016-03-16-susemanager-3-backstage/states-catalog-1.png)

Additionally, SUSE Manager also has a higher level state user interface for packages. With this user interface you can search packages in the assigned channels.

![Minion Package Stats 1]({{ site.baseurl }}/assets/images/posts/2016-03-16-susemanager-3-backstage/package-states-1.png)

#### Common states for organizations and groups

SUSE Manager allows to apply states from the State Catalog to Organizations and Groups. Every system belonging to those entities [will be subject to those states](https://docs.saltstack.com/en/latest/ref/states/top.html).

![Minion Custom States 1]({{ site.baseurl }}/assets/images/posts/2016-03-16-susemanager-3-backstage/minion-custom-states-1.png)

## Massive command execution

The `Remote Commands` page in the `Salt` section gives you a web based version of `salt '*' cmd.run`. You can preview which minions will be affected with the target before sending the commands and then see the results in real time:

![Remote Commands 1]({{ site.baseurl }}/assets/images/posts/2016-03-16-susemanager-3-backstage/remote-commands-1.png)

## Being part of the ecosystem

Making Salt an important part of SUSE Manager does not end there. In our industry being an Enterprise distributor of open-source software only works if you are part of it.

* SUSE already has around 600 commits from 5+ developers in Salt upstream
* The SUSE Manager team is hiring so that we can do more work upstream and help shape Salt's future
* SUSE will be gold sponsor at Saltconf 2016

![Saltconf Sponsors 1]({{ site.baseurl }}/assets/images/posts/2016-03-16-susemanager-3-backstage/saltconf-sponsor-1.png)

## The future

As you can see, SUSE Manager with Salt is a powerful duo which allows you to continue managing your infrastructure with the same tool, be 100% backward compatible and start taking advantages of declarative configuration management and the orchestration capabilities of Salt, while keeping everything you have already deployed untouched.

We are very excited about the posibilities here, which will be guided by feedback from our customers and synergies we have with Salt and other SUSE technologies.

## To be continued

Configuration Management is only one of the features that will arrive SUSE Manager 3. Expect to hear from the powerful Subscription Compliance and Topology Awareness in future posts.