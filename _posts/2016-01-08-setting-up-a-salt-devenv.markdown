---
layout: post
title: Setting up a Salt development environment
date: '2016-01-08'
categories:
- Software
tags:
- SUSE
- Saltstack
published: false 
---

I needed to work in [Salt](https://github.com/saltstack)'s code so I followed the "[Installing Salt for Development](https://docs.saltstack.com/en/latest/topics/development/hacking.html)" guide.

```console
mkdir /space/virtualenv/salt
virtualenv --system-site-packages /space/virtualenv/salt

git clone https://github.com/saltstack/salt
git remote add upstream https://github.com/saltstack/salt
git fetch --tags upstream

pip install M2Crypto    # Don't install on Debian/Ubuntu (see below)
pip install pyzmq PyYAML pycrypto msgpack-python jinja2 psutil
pip install -e ./salt   # the path to the salt git clone from above

mkdir -p /path/to/your/virtualenv/etc/salt
cp ./salt/conf/master ./salt/conf/minion /path/to/your/virtualenv/etc/salt/

user: duncan
root_dir: /space/virtualenv/salt

user: duncan
root_dir: /space/virtualenv/salt
master: localhost
id: saltdev

cd /path/to/your/virtualenv
salt-master -c ./etc/salt -d
salt-minion -c ./etc/salt -d
salt-key -c ./etc/salt -L
salt-key -c ./etc/salt -A
salt -c ./etc/salt '*' test.ping

https://github.com/saltstack/salt-testing.git

