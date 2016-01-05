---
layout: post
title: Cool things in a cold day
date: '2007-11-14'
tags:
- germany
- java
- newkde
- open-source
---

Nice surprise today:

 ![]({{ site.baseurl }}/assets/images/posts/media_httpfarm3static_afbyi-scaled500.jpg)

* ITO time spent on YaST. No success yet with [Wt][4]. It does not provide something like Qt's QSocketNotifier, or glib's g\_io\_add\_watch which integrate themselves with the event loop. Did a hack with a standard select and a timeout, did not work. Even worse, Wt crashes on processEvents().

* Will hosted a hack session on Saturday. While I did not get any code done, I got motivated by the Kopete 4.x state to continue working on it at home. Yesterday I commited my chat window participants view code for Kopete. It simplifies the code and the signal battle a lot. I still have to fix some issues.

* The [Android stuff][1] is so cool. Read this post about [Dalvik: how Google routed around Sun's IP-based licensing restrictions on Java ME][5]. The [Activity, Services and Intents][2] model seems natural for other scenarios, not only mobiles.

[1]: http://code.google.com/android/index.html  
[2]: http://code.google.com/android/intro/lifecycle.html  
[3]: http://code.google.com/android/reference/android/app/Activity.html  
[4]: http://www.webtoolkit.eu/wt/  
[5]: http://www.betaversion.org/~stefano/linotype/news/110/

