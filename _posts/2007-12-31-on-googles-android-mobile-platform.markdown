---
layout: post
title: On Google's Android mobile platform
date: '2007-12-31'
tags:
- android
- eclipse
- java
- mobile
---

I have spent quite some time studying and testing [Android][1], the new mobile platform from Google.

It is hard to understand people complaining about Java fragmentation because Google's technical decisions like not using a standard virtual machine.

Just at the same time, I was trying to create an application for Blackberry. Oh yes, Blackberry uses the standard J2ME plus some proprietary APIs. But they of course did not think about Eclipse and instead offer a crappy self-made Windows IDE that nobody likes. There are hacks to use the blackberry APIs (a .jar file) inside Eclipse or Netbeans, but things are not easy if Research in Motion (Blackberry makers) offers the files inside a Windows .exe setup.  
I did not even try to install the Sun stuff. It was not clear what I needed to download. Which VM? which Eclipse plugin? Which emulator? Where are the docs.

Forget about that. Enter Android.

After downloading the files and extracting them to a directory, I installed the Eclipse plugin adding an remote update site to the configuration. Then I configured the plugin to look for the Android SDK in the directory where I extracted it.

Working with Eclipse is a pleasure. I always become depressed when working with Eclipse because it reminds me how crappy is the life of a C++ programmer when it comes to decent tools. Real time compilation and completion would make me happy in the C++ world.

The documentation is good. Not at the level of Trolltech's Qt docs but enough. There are some examples in the site, and the amount of [Android related projects][3] on code.google.com hosting is impressive. This makes easy learning by looking different examples.

I like the design of the framework, based on concepts such as Activity, Intent, Content (modeled after URIs), etc. The user interface API looks like a normal toolkit, but the XML way of creating layouts is not intuitive. One simple google search and I found [Droiddraw][2], a tool to draw screens and generate XML files from them.

The android sqlite API sucks. I figured out how to wrap a database to expose a content provider url based API, by looking other examples, and at the end I was copy pasting code. Where is [DRY][4] ?. However the content provider API is really nice and a clever way to expose data across applications.

Other APIs are really good. Location services, Google apps views, and the way you can use XML files as resources (layouts, strings, etc) is also interesting. Android generates a Java class called R, which you use to access the resources, this allows you to autocomplete the resources in Eclipse.

The emulator rocks, I press "run" in Eclipse and it works, and debugging support is also complete. The emulator is based on qemu. It support different skins and can simulate network speeds and latencies.

In conclusion, Android provides a easy to use framework that work out of the box on your Linux box. It integrates with Eclipse, and the amount of existing code for the platform is enough to start learning.

I will post again as soon as I discover more ;-)

[1]: http://code.google.com/android/  
[2]: http://www.droiddraw.org/  
[3]: http://code.google.com/hosting/search?q=android&btn=Search+Projects  
[4]: http://en.wikipedia.org/wiki/Don't\_repeat\_yourself

