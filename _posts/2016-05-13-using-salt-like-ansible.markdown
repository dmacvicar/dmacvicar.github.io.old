---
layout: post
title: Using Salt like Ansible
date: '2016-05-13'
categories:
- Software
tags:
- salt
comments: true
hide: true
---

## Introduction

If you have been paying attention to the configuration management field, you probably heard of Salt and Ansible.

When at SUSE we chose Salt [as the configuration management engine for SUSE Manager 3]({% post_url 2016-03-16-susemanager-3-backstage %}), some people asked me "Why not Ansible?".

The first part of the answer had to do that the master-minion architecture of Salt has as consequence a lot of interesting features and synergies with the way SUSE Manager operates: real-time management, event-bus, etc.

For example, you can create more scalable topoligies using the concept of syndics:

![Salt 0mq]({{ site.baseurl }}/assets/images/posts/2016-05-13-using-salt-like-ansible/salt-syndic.png)

Or manage dumb devices with the concept of Salt proxies:

![Salt 0mq]({{ site.baseurl }}/assets/images/posts/2016-05-13-using-salt-like-ansible/salt-proxy.png)

Now, for a small DevOp team collaborating via git, the model of running Ansible from their workstations to a bunch of nodes defined in a text file is very attractive.

Salt allows you to do this too. It is called `salt-ssh`. So lets take [this Ansible tutorial](https://serversforhackers.com/an-ansible-tutorial) and show how you would do the same with `salt-ssh`.





