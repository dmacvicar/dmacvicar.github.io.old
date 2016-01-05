---
layout: post
title: ZYpp 6.2.1, no mirror will stop you
date: '2009-03-03'
tags:
- newsuse
- software
- suse
- yast
- zypp
---

Two weeks ago [I posted about making aria2c][1] the default transfer mechanism for our software management stack.

With libzypp 6.1.0 in Factory, this is already the case. With the first community round of testing, we got quite a lot of bug reports and feedback. Some stuff did not work because the implementation was wrong, while others just showed us that aria2 worked differently as we thought.

The upcoming 6.2.1 addresses the following issues:

* implemented speed indicators (bnc#475506). You should see transfer speed in applications that report it (like zypper).  
* implemented progress indicators correctly (bnc#473846). You should see progress reporting now in a consistent basis.

* fixed broken pipe when looking for aria2c which caused a fallback to curl. (bnc#480930)

* implement saving and reading mirror stats data in /var/cache/zypp/aria2.stats. This means libzypp now learns which mirrors are faster and more reliable for you and remembers it. Every usage adds more information to the "learning".

* handle failover correctly (bnc#481115). This means libzypp tries harder to get the file from alternate mirrors.  
* various improvements in error and report handling. General code review and minor fixes.

How to test?

Peter added an interesting "test feature". If a request happens with a header X-Broken-Mirrors then the openSUSE redirector will send you broken mirrors. So edit your root's aria2.conf:

cat ~/.aria2/aria2.conf  
 header=X-Broken-Mirrors: true

And observe zypper.log. Even if errors are reported, libzypp should try other mirrors and get the file successfully.

You can go more extreme, and use "X-Broken-Mirrors: only", which will give you only bad servers. Even in this case you will see libzypp try hard to get the file before giving up. How hard to try is configurable, see [last post for more information][1].

Big thanks to [Peter Poeml][4] and Tatsuhiro Tsujikawa for all the feedback and support, and of course

And with this, [feature 302923][2] is done ;-) and the rest is tracked as bugs.

[1]: http://duncan.mac-vicar.com/blog/archives/507  
[2]: https://features.opensuse.org/302923  
[3]: http://en.opensuse.org/Libzypp/Failover  
[4]: http://en.opensuse.org/User:Poeml

