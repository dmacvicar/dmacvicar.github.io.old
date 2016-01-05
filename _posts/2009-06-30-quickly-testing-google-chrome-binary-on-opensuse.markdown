---
layout: post
title: Quickly testing Google Chrome binary on openSUSE
date: '2009-06-30'
tags:
- google
- newsuse
- suse
---

Get the deb package from the [developer release site][1].

Unpack the deb:

```
ar x google-chrome-unstable_current_i386.deb
lzma -d data.tar.lzma
tar xpvf data.tar
```

Now you got a opt directory in the current directory with the full tree. Go to the Chrome directory.

```
cd opt/google/chrome
```

Symlink the libraries:

```
ln -s /usr/lib/libnss3.so ./libnss3.so.1d
ln -s /usr/lib/libnssutil3.so ./libnssutil3.so.1d
ln -s /usr/lib/libsmime3.so ./libsmime3.so.1d
ln -s /usr/lib/libssl3.so ./libssl3.so.1d
ln -s /usr/lib/libplds4.so ./libplds4.so.0d
ln -s /usr/lib/libplc4.so ./libplc4.so.0d
ln -s /usr/lib/libnspr4.so ./libnspr4.so.0d
```

Run it and enjoy!. Be sure to read [about privacy features of this preview release][2].

```
LD_LIBRARY_PATH=$PWD ./google-chrome
```

[1]: http://www.google.com/chrome/intl/en/eula_dev.html?dl=unstable_i386_deb  
 [2]: http://www.google.com/chrome/intl/en/privacy_linux.html

