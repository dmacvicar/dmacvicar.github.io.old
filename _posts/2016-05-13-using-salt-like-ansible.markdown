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

![Salt 0mq]({{ site.baseurl }}/assets/images/posts/2016-05-13-using-salt-like-ansible/salt-0mq.png)

For example, you can create more scalable topoligies using the concept of syndics:

![Salt syndic]({{ site.baseurl }}/assets/images/posts/2016-05-13-using-salt-like-ansible/salt-syndic.png)

Or manage dumb devices with the concept of Salt proxies:

![Salt 0mq]({{ site.baseurl }}/assets/images/posts/2016-05-13-using-salt-like-ansible/salt-proxy.png)

The power of all the concepts Salt introduces is huge, and it is definitely [worth to learn them](https://docs.saltstack.com/en/getstarted/).

However, for a small DevOp team collaborating via git, the model of running Ansible from their workstations to a bunch of nodes defined in a text file is very attractive.

Salt allows you to do this too. It is called `salt-ssh`. So lets take [this Ansible tutorial](https://serversforhackers.com/an-ansible-tutorial) and show how you would do the same with `salt-ssh`.

## Install

>This means there's usually a "central" server running Ansible commands, although there's nothing particularly special about what server Ansible is installed on. Ansible is "agentless" - there's no central agent(s) running. We can even run Ansible from any server; I often run Tasks from my laptop.

The salt package is made of various components, among others:

* `salt`: the framework, libraries, modules, etc.
* `salt-minion`: the minion daemon, runs on the managed hosts.
* `salt-master`: the master daemon, runs on the management server.
* `salt-ssh`: a tool to manage servers over ssh.

If you want to run Salt like Ansible, you only need to install `salt` and `salt-ssh` in your machine (the machine where you want to run tasks from).

Salt is available out of the box on [openSUSE Leap and Tumbleweed](https://www.opensuse.org/) so if you are using them just type:

```console
zypper in salt-ssh
```
For other platforms, please refer to the [install section of the Salt documentation](https://docs.saltstack.com/en/latest/topics/installation/).

### Self contained setup

It is common to put all the project in a single folder. In Ansible you can put the `hosts` file in a folder, and the playbooks in a subfolder. To accomplish this with `salt-ssh`.

* Create a folder for your project, eg: `~/Project`.
* Create a file named `Saltfile` in your `~/Project`.

```yaml
salt-ssh:
    config_dir: etc/salt
    max_procs: 30
    wipe_ssh: True
```

Here we tell Salt that the configuration directory is now relative to the folder. You can name it as you want, but I prefer myself to stick to the same conventions, so `/etc/salt` becomes `~/Project/etc/salt`.

Then create `~/Project/etc/salt/master`:

```yaml
root_dir: .
file_roots:
  base:
    - srv/salt
pillar_roots:
  base:
    - srv/pillar
```

And create both trees:

```console
mkdir -p srv/salt
mkdir -p srv/pillar
```

Salt will also create a `var` directory for the cache inside the project tree, unless you choose a different path. What I do is to put `var` inside `.gitignore`.

## Managing servers

>Ansible has a default inventory file used to define which servers it will be managing. After installation, there's an example one you can reference at /etc/ansible/hosts.

The equivalent file in `salt-ssh` is [`/etc/salt/roster`](https://docs.saltstack.com/en/latest/topics/ssh/roster.html).

>That's good enough for now. If needed, we can define ranges of hosts, multiple groups, reusable variables, and use [other fancy setups](http://docs.ansible.com/intro_inventory.html), including [creating a dynamic inventory](http://docs.ansible.com/intro_dynamic_inventory.html).

Salt can also provide the roster with [custom modules](https://docs.saltstack.com/en/latest/ref/roster/all/index.html#all-salt-roster). Funnily enough, [`ansible`](https://docs.saltstack.com/en/latest/ref/roster/all/salt.roster.ansible.html#module-salt.roster.ansible) is one of them.

As I am using a self-contained setup, I create `~/Project/etc/salt/roster`:

```yaml
node1:
  host: node1.example.com
node2:
  host: node2.example.com
```

## Basic: Running Commands

>Ansible will assume you have SSH access available to your servers, usually based on SSH-Key. Because Ansible uses SSH, the server it's on needs to be able to SSH into the inventory servers. It will attempt to connect as the current user it is being run as. If I'm running Ansible as user vagrant, it will attempt to connect as user vagrant on the other servers.

`salt-ssh` is not very different here. Either you already have access to the server, otherwise it will optionally ask you for the password and deploy the generated key-pair `etc/salt/pki/master/ssh/salt-ssh.rsa.pub` to the host so that you have access to it in the future.

So, in the Ansible tutorial, you did:

```console
$ ansible all -m ping
127.0.0.1 | success >> {
    "changed": false,
    "ping": "pong"
}
```

The equivalent in `salt-ssh` would be:

```
salt-ssh '*' test.ping
node1:
    True
node2:
    True
```

Just like the Ansible tutorial covers, `salt-ssh` also has options to change the user, output, roster, etc. Refer to `man salt-ssh` for details.

## Modules

>Ansible uses "modules" to accomplish most of its Tasks. Modules can do things like install software, copy files, use templates and much more.

The equivalent in Salt is called "modules" as well. There are two types of modules: [Execution modules](https://docs.saltstack.com/en/latest/ref/modules/) and [State modules](https://docs.saltstack.com/en/latest/ref/states/writing.html). Execution modules are *imperative actions* (think of *install!*). State modules are used to build idempotent declarative state (think of *installed*).

There are two execution modules worth to mention:

* The `cmd` module, which you can use to run shell commands when you want to accomplish something that is not provided by a built-in execution module.
* The `state` module, which is the execution module that allows to apply state modules and more complex composition of states, known as `sls` files.

>If we didn't have modules, we'd be left running arbitrary shell commands like this:
>```console
>ansible all -s -m shell -a 'apt-get install nginx'
>```











