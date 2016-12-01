---
layout: post
title: Building Docker images with plain Salt
date: '2016-07-11'
categories:
- Software
tags:
- salt
- docker
comments: true
---

So [Hackweek 14](https://hackweek.suse.com/) is over. It started during the [openSUSE Conference 2016](https://events.opensuse.org/conference/oSC16) on Friday June 24 and continued all over the following week.

I had worked on [integrating snapshots with Salt]({% post_url 2016-06-09-config-drift-salt-snapper %}) with [Pablo](https://github.com/meaksh) just some weeks before that and I was waiting for the openSUSE Conference to get the chance to show [Thomas](https://twitter.com/thatch45) what we had done in order to get feedback and figure out next steps.

A few days before the Conference Redhat [did a press release](https://www.redhat.com/en/about/press-releases/red-hat-launches-ansible-native-container-workflow-project) that caught my attention: a framework to build container images with [Ansible](https://www.ansible.com). Yes, that makes a lot of sense. My head started immediately to think all day long about the challenges to build something like that: Installing the configuration management tool without leaving it there, etc. I got curious and started poking at the README.

On one hand, it was not what I was expecting (well, at least, for a Press Release or Tech Preview). It still "generated" Dockerfiles, relied on Ansible to be installed in some way, wich was "templated" into Dockerfiles, and it was of course a new tool.

On the other hand, it was pure inspiration: I remembered why I like Salt so much. I knew that with Salt [I wouldn't need to build a "new tool"]({% post_url 2016-05-18-using-salt-like-ansible %}). I'd only need to write a module and connect some pieces, and that makes my feature distributed, accessible, deployable, etc. I'd not need to interact with Docker directly, but only with the [Salt execution module](https://docs.saltstack.com/en/latest/ref/modules/all/salt.modules.dockerng.html) for it. The best part: I had a Hackweek project!.

I used the chance that [Thomas](https://twitter.com/thatch45) was at the openSUSE Conference to ask him some details about `salt-thin` and explain him rough ideas.

The feature went more or less like expected:

* A Docker image is basically another image, modified.
* The problem can be reduced to run a Salt state run inside of the container, modulo problems.
  * The image does not have Salt.
  * After the State run, we can't leave Salt there.
  * The container does not have connectivity with the Salt master.
  * Pillars may be templated against grains which come from the container.

![Diagram]({{ site.baseurl }}/assets/images/posts/2016-07-11-building-docker-images-with-plain-salt/diagram-short.png)

After tackling the problems one by one, you can factor some stuff out:

* If `dockerng.build_sls` needs to apply state on a new container and commit it, why not allow to call state on a running container?. `dockerng.sls` was born.
* If we are going to call `state.sls` and `grains.items` on the container, why not allow to call *any* module in a container?. `dockerng.call` was born.

On Friday I was able to give the following Lightning Talk:

<iframe width="560" height="315" src="https://www.youtube.com/embed/2znjgf9Q7J0" frameborder="0" allowfullscreen></iframe>

The result is:

* You can build images using your own `salt://` tree modules only needing Python on the base image. And yes, you can consume pillar data.
* You can execute modules on containers. Which will be interesting to see how it can be used for auditing (eg. [HubbleStack](https://github.com/HubbleStack)).

I prepared a [pull request](https://github.com/saltstack/salt/pull/34484), which had the best reception I ever got on a pull request:

![Awesome]({{ site.baseurl }}/assets/images/posts/2016-07-11-building-docker-images-with-plain-salt/awesome.png)

There are some details to polish and I hope it can be merged soon.

