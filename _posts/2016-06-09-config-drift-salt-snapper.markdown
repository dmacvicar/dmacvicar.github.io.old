---
layout: post
title: Managing configuration drift with Salt and Snapper
date: '2016-06-09'
categories:
- Software
tags:
- salt
comments: true
hide: true
---

## Introduction

That some configuration management tools originate in the DevOps space and their inmense popularity there has as a result that even when they declaratively manage configuration, they are tailored towards deployment of new servers using this configuration but not torwards the auditing of existing servers.

For example, lets imagine a server with the following state:

```yaml
/etc/motd:
  file.managed:
    - source: salt://common/motd
```

If we apply this state (in `test` mode) on a non-compliant server:

```console
$ salt minion1 state.apply test=True
minion1:
----------
          ID: /etc/motd
    Function: file.managed
      Result: None
     Comment: The file /etc/motd is set to be changed
     Started: 10:06:05.021643
    Duration: 30.339 ms
     Changes:
              ----------
              diff:
                  ---
                  +++
                  @@ -1 +1 @@
                  -Have a lot of fun...
                  +This is my managed motd

Summary for minion1
------------
Succeeded: 1 (unchanged=1, changed=1)
Failed:    0
------------
Total states run:     1
```

Salt is able to tell us that there is a file that deviates from the configuration. And we can easily fix it by just removing `test=True`.

Now, lets say an intruder adds a malicious entry to `/etc/hosts`:

```
192.168.1.34    www.google.com
```

If we re run our state in test mode:

```console
$ salt minion1 state.apply test=True
minion1:
----------
          ID: /etc/motd
    Function: file.managed
      Result: None
     Comment: The file /etc/motd is set to be changed
     Started: 10:12:11.518105
    Duration: 29.479 ms
     Changes:
              ----------
              diff:
                  ---
                  +++
                  @@ -1 +1 @@
                  -Have a lot of fun...
                  +This is my managed motd

Summary for minion1
------------
Succeeded: 1 (unchanged=1, changed=1)
Failed:    0
------------
Total states run:     1
```

It did not found anything. This is expected. This rule is not in the configuration.

## Creating new systems vs auditing existing systems

This model works fine in the DevOps world where the culture is to take a random Linux image from the internet and use it as a base to deploy systems from scratch. As long as all tests pass, replacing the underlaying image is not a problem. Only what is explicitly defined is evaluated against the configuration and defined as a drift.

When meeting enterprise customers who are starting to use configuration management to improve the control on their infrastructure, it turns out their expectations where different. "If I use Salt, will it tell me when somebody makes a change to the system?". "Ugh.. no... well depends...".

## Baselines

That was the point that I started to think on how we could use the state system do a more generic auditing, but how to do it without ruining the experience of working with states until it all clicked together: implicit state can be done explicit by using another state. When the customer said "any change", it was in reality saying "any change against my defined configuration" plus "any change since my last working configuration".

So, we needed a way to manage "last working configuration" and turns out SUSE is where [Snapper](http://snapper.io) originated and Snapper is nowadays available in most distributions.

Snapper is a set of tools over snapshots (mostly btrfs, but also works on others like ext4 if you have the required kernel/tool patches). Think of it of what docker did to containers, snapper does to snapshots. It adds the required workflows, terminology and tools to make them usable.

It also turns out that my system already has some snapshots, because just like I can manually take one, tools like YaST and zypper take snapshots before and after doing operations. You can even select previous snapshots from the bootloader and boot into the previous working system.

What if I could describe a state in Salt that said: "Nothing deviates from this snapshots, except....".

## Let's do it

So during this year Department workshop I paired with [Pablo](https://github.com/meaksh) and our project had the following steps:

* Complete the Salt execution module to expose the basic snapper operations you can do from the command line. Example:

```console
salt minion1 snapper.create_snapshot
```

* Create a generic way for sysadmins to do Salt operations which can be reverted. We implemented this as a meta-call (a call taking another call as a parameter) `snapper.run`. So you can do something like:

```console
$ salt minion2 snapper.run function=file.append args='["/etc/motd", "some text"]'
minion2:
    Wrote 1 lines to "/etc/motd"
```

This will generate a snapshot before running the command, run the command and then take a snapshot afterwards, also adding metadata about the Salt job that did the change:

```console
...
pre    | 21 |       | Thu Jun  9 10:34:36 2016 | root | number  | salt job 20160609103437556668 | salt_jid=20160609103437556668
post   | 22 | 21    | Thu Jun  9 10:34:37 2016 | root | number  | salt job 20160609103437556668 | salt_jid=20160609103437556668
```

Because in Salt, state is implemented as a method `state.apply` or state.highstate`, calling `snapper.run function=state.apply` means you can rollback a failed `state.apply`.

And of course we not only exposed `snapper.diff` which takes the snapshot number but also a `snapper.diff_jid` which tells you what a Salt job changed:

```console
$ salt minion2 snapper.diff_jid 20160609103437556668
minion2:
    ----------
    /etc/motd:
        --- /.snapshots/21/snapshot/etc/motd
        +++ /.snapshots/22/snapshot/etc/motd
        @@ -1 +1,2 @@
         Have a lot of fun...
        +some text
```

* And finally, allowing a system administrator to use snapshots as a baseline to apply state. Lets take the original example with the malicious user modifying `/etc/hosts', we will add a snapper state rule:

```yaml
my_baseline:
  snapper.baseline_snapshot:
    - number: 20
    - ignore:
      - /var/log
      - /var/cache

/etc/motd:
  file.managed:
      - source: salt://common/motd
```

Now we apply the state in test mode again:

```console
$ salt minion1 state.apply test=True
minion1:
----------
          ID: my_baseline
    Function: snapper.baseline_snapshot
      Result: None
     Comment: 1 files changes are set to be undone
     Started: 12:20:24.899848
    Duration: 1051.996 ms
     Changes:
              ----------
              files:
                  ----------
                  /etc/hosts:
                      ----------
                      actions:
                          - modified
                      comment:
                          text file
                      diff:
                          --- /etc/hosts
                          +++ /.snapshots/21/snapshot/etc/hosts
                          @@ -22,5 +22,3 @@
                           ff02::3         ipv6-allhosts


                          -192.168.1.34    www.google.com
                          -
----------
          ID: /etc/motd
    Function: file.managed
      Result: None
     Comment: The file /etc/motd is set to be changed
     Started: 12:20:25.953348
    Duration: 20.425 ms
     Changes:
              ----------
              diff:
                  ---
                  +++
                  @@ -1 +1 @@
                  -Have a lot of fun...
                  +This is my managed motd

Summary for minion1
------------
Succeeded: 2 (unchanged=2, changed=2)
Failed:    0
------------
Total states run:     2
```

Exactly what we expect!.

# Conclusions

So with this you can use your configuration management to manage your state against a defined state and on top of that we give you the tooling to inspect and rollback configuration changes.

We will continue adding the missing pieces to give the administrators full overview and control over their running systems.

You can find our current work [in this github repository](https://github.com/SUSE/salt-snapper-module). We plan of course to send it upstream once the design and implementation settles down.



