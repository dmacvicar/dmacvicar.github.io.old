---
layout: post
title: Moving openSUSE Factory to aria2c (and new features)
date: '2009-02-23'
tags:
- newsuse
- software
- suse
- yast
---

You may remember, openSUSE 11.1 shipped with experimental (and by default disabled) software management stack improved HTTP download support, based on aria2c, which gives additional capabilities to take advantage of our famous [download infrastructure][1], on which [Peter][2] has been working on.

The ultimate goal of this is to provide a pleasant experience to our advanced users tracking Factory and doing frequent dist-upgrades ([feature 305634][3]), and this includes other cool improvements, like [the one announced some days ago][7], based on [Stephan Kulow][8] and Michael Matz's build compare techniques, and of course the one I describe here: robust mirror handling and improved download experience ([feature 302923][4]).

The next step for the aria2c backend is to enable it as a default. Everything was working more or less smooth, except file existence detection, which is needed for the repository probing components. This part of the code was implemented as a "always true" stub.

As the MediaCurl code was already a bit messy, duplication was started to be everywhere, and actually MediaAria2c already had quite a lot of code duplicated from MediaCurl. However, a big refactoring required studying the code in deep.

Thanks to the Asian restaurant near the office, which did not clean their surroundings from the ice created from the past snowy days in Nürnberg, I had to stay some days at home (medical order) due to a very ugly fall that made my left arm useless (except for some typing), which gave me the quiet opportunity to study/read that code and figure out a solution.

I tried tons of different things. Even using curl command line from the aria backend only to check file existence, but at the end the problems I encountered (and figuring out some missing features from aria2c) pushed me in the direction of making the aria2c backend a particular case of the curl backend, and therefore I could reuse features from curl, without worrying about reentrant access to media attach points or other stuff. This reduced the code of the aria2c backend considerably. MediaCurl is now much smaller (about 200 lines of duplicated code removed), so we can add range support (I will explain this in a future post) to the API.

On the way to this refactoring, the following features/options (zypp.conf) got implemented:

```
##
## Maximum number of concurrent connections to use per transfer
## This setting is only used if more than one is possible
## Setting it to a reasonable number avoids flooding servers
##
# download.max_concurrent_connections = 2
</pre>

The option above only works with the aria2c backend. Not with pure curl.

<pre class="prettyprint">
##
## Sets the minimum download speed (bytes per second)
## until the connection is dropped
## This can be useful to prevent security attacks on hosts by
## providing updates at very low speeds.
##
## 0 means no limit
##
# download.min_download_speed = 0
</pre>

The option above can be used to prevent a (very theoretical) attack described in [this report][6], where the bad guys will send data very slowly to the client to keep it unpatched. This option should work with both aria2c and curl backends.

Also, we implemented the following options for those people sharing a connection or needing to limit the connection usage somehow:

<pre class="prettyprint">
## Maximum download speed (bytes per second)
## 0 means no limit
# download.max_download_speed = 0
</pre>

Also, the following option is useful for people doing the dist-upgrade unattended, so they can set it to try hard, or just to keep trying without interaction:

<pre class="prettyprint">
## Number of tries per download which will be
## done without user interaction
## 0 means no limit (use with caution)
# download.max_silent_tries = 5
```

These features are in libzypp 6.1.0, Factory has 5.11.0 right now. I plan to submit to Factory in the following days (after basic testing). Once aria2c is the default, it can be disabled using ZYPP\_ARIA2C=0 which will give you the basic curl back.

Of course, this may result in new bugs or problems. So be prepared!

[1]: http://www.mirrorbrain.org  
 [2]: http://en.opensuse.org/User:Poeml  
 [3]: https://features.opensuse.org/305634  
 [4]: https://features.opensuse.org/302923  
 [5]: https://features.opensuse.org/303532  
 [6]: http://www.cs.arizona.edu/people/justin/packagemanagersecurity/attacks-on-package-managers.html  
 [7]: http://news.opensuse.org/2009/02/05/more-efficient-factory-development/  
 [8]: http://en.opensuse.org/User:Coolo

