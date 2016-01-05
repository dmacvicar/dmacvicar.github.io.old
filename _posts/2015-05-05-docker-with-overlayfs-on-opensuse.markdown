---
layout: post
title: Docker with overlayfs on openSUSE
date: '2015-05-05'
categories:
- Software
tags:
- docker
---

Docker works better with an union filesystem, but the most popular one (aufs) is not included in the mainline kernel because of stability issues.

openSUSE has been shipping the Device Mapper driver until now, but the most promising driver seems to be the one based on the overlayfs union filesystem from [SUSE developer Miklos Szeredi](https://git.kernel.org/cgit/linux/kernel/git/mszeredi/vfs.git/log/?h=overlayfs-next). Overlayfs made it upstream, and the kernel 4.0 has all the pieces that docker needs to make it work as a storage backend.

kernel 4.0.0 arrived in the last days to openSUSE Tumbleweed. So provided you already ran "zypper dup" you can switch to this backend with:

Delete all images and containers:

`docker rm $(docker ps -a -q)
docker rmi $(docker images -q)`

Edit `/etc/sysconfig/docker` and add a -s option:

```text
systemctl restart docker
```

