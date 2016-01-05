---
layout: post
title: kvm setup for laptops with NetworkManager using bridges or openvswitch and
  NAT
date: '2012-03-12'
categories:
- Software
tags:
- suse
---

On my workstation I have a static network setup: I don't give an ip to eth0 but configure dhcp to give the ip to a bridge br0 attached to eth0. Then qemu-kvm creates tap devices attached to the bridge, getting ip's in the same network as the host.

On my laptop I run NetworkManager, which does not play well with bridges. It seems that in other distributions you can tell the network configuration to use NetworkManager for certain interfaces only (which is still not exactly what I want). After lot of reading, I found a configuration that fits my needs.

The idea is to create a bridge, and instead of having it attached to eth0 (which is controlled by NetworkManager), we create the bridge in a separate network and use NAT to have the VMs access the internet.

I [found a script by Amos Kong that setups the network](http://patchwork.test.kernel.org/patch/2687/) (adapted to the paths of brctl in openSUSE):

Then you need a pair of scripts for qemu, which are called with the tap device as a parameter.

```bash
#!/bin/sh
switch='br0'
/sbin/ifconfig $1 0.0.0.0 down
/sbin/brctl delif ${switch} $1
```

Then you run the VM like:

here until the submit request is accepted.

Then change on the setup script the brctl addbr line to use ovs-vsctl:

[sourcecode lang="bash" highlight="4"]  
add\_br()  
{  
 echo "add new private bridge"  
 ovs-vsctl add-br $brname  
 echo 1 \> /proc/sys/net/ipv6/conf/$brname/disable\_ipv6  
 echo 1 \> /proc/sys/net/ipv4/ip\_forward  
 /sbin/brctl stp $brname on  
 /sbin/brctl setfd $brname 0  
 ...

And the for the qemu scripts, use ovs-vsctl add-port instead of brctl addif:

