---
layout: post
title: The natural order of things...
date: '2008-09-29'
tags:
- facebook
- gnome
- google
- software
- webservices
- yast
---

## Polyglot programmers

In the book "The Productive Programmer", written by Neal Ford, there is a nice chapter that talks about an old topic: languages, and defines [Polyglot programming][1] as the kind of tendency in the industry to manage the complexity of current applications.

It is tempting to think that one language keeps the complexity low, but that is exactly the reason why languages keep coming. They usually try to become general purpose languages and start to get bloated or inheriting nonsense stuff.

While thinking on this topic. I started to remember lot of episodes in my life as a software developer. Having learned lot of my base in the KDE community, I was quite natural to think in the way Neal explains. In KDE people write since the beginning of the platform a good bunch of the static user interface descriptions in domain specific languages, which are transformed to more general languages (C++) at compilation time.

The very basics of Qt is C++ plus a "platform" and some specific add ons (another domain specific language for component communication) which are compiled into support code at compilation time. Code generation using DSL's goes beyond that: Configuration for applications can be specified in [a simple XML schema for the configuration domain][9] and most of the code is generated. Therefore consistent, robust and bug free. For a KDE guy, this sounds natural (however it could be extended much more), and all the platform is like the book "Ola's Pyramid", where the lower levels are done using general languages (like C) and upper layers are done using more domain specific languages. We miss the intermediate part of scripting languages, but I am sure Plasma will be the first component following that natural order of things.

At that time I was learning, and the KDE "way" became natural. At the same time, that was the time when I discarded the Gnome platform completely. Mostly because I heard lot of FUD against KDE decisions at that time, lot of attacks and ignores towards a more common world, and then the world kinda inverted itself and used to be criticized stuff like use of preprocessor to add features to the language (Qt's moc) : [Welcome Vala?][14] or the ignored dcop bus : [Enter dbus][15], Object orientation (\_try\_ to enter GObject ugh)... are now part of the "natural order of things". I was kind of confused ( [famous hackers claiming they won't program apps in C again][11], uh? ).

As a side note, some developers, like Cornelius Schumacher and André Duffeck played with this concept of [generating code much further][8]. Like mixing models and user interfaces.

## polyglot YaST

As a YaST developer, I keep thinking in the current code base, the directions we are taking, etc. The development field is full of new silver bullets, languages and things that make hard to keep the big picture if you are following previously unknown fields.

Surprisingly YaST is a very good example in the whole model. We abstract the low level system with a C++ made platform, and run a [domain specific language called YCP][12] on top, and have [another domain specific language][13] to manage the system configuration. It is a very good example of a complex platform that manages complexity using different languages, and it is a very mature platform at the same time. Of course there are lot of things that can be improved:

* No usage of convention over configuration: There is an abuse of "skeletons", that is, generating trees and code, putting it into version control, instead of doing most things magically by convention. (Very similar to the difference of rails to Java use of XML).

* Defining user interfaces using code is ok as long as the code is not where it should not be. And the code base has this problem. Having an external DSL for this helps forcing you to separate things.

* Hard to bind to the platform: Writing components is boring and over engineered. It works for complex cases (like defining your own type of component functions), but fails for the simple case. Try doing a ruby extension. It is easy. I think this is something we should improve. There are some basic support like adding some type information (using a DSL) on the headers and parsing that.

* We should check and recheck that we are not repeating ourselves. Duplicated functionality, declarations and implementations.

### Code generation for services

Now that we are walking towards the world of services, Web and such, it is time for new ideas. It is interesting to see big players trying to solve difficult problems, and coming up with natural solutions, like the case of Google with their [protocol buffers][5], and Facebook with [Thrift][7] ([paper][4]).

Thrift is more interesting, as it comes from the situation of having to develop different services that need to communicate without needing to care much about what was used to build that component (SOA approach, which is not very different to what we are solving with dcop-ng (aka dbus) in the desktop scenario), but imagine dbus is your network, and thrift is the Qt dbus binding generator or dbusxml2something.

> Thrift is a software framework for scalable cross-language services development. It combines a powerful software stack with a code generation engine to build services that work efficiently and seamlessly between C++, Java, Python, PHP, and Ruby. Thrift was developed at Facebook and released as open source.  
>   
> Thrift allows you to define data types and service interfaces in a simple definition file. Taking that file as input, the compiler generates code to be used to easily build RPC clients and servers that communicate seamlessly across programming languages.

Now that schubi has been doing quite progress on exposing some YaST functionality via http (result of the last workshop), I am really curious if we can define our platform, and tackle some problems by defining very domain specific tools that can save us from repetition. Will see.

[1]: http://www.erubycon.com/images/polyglot_programming.pdf  
 [2]: http://productiveprogrammer.com/wiki/index.php/Main_Page  
 [3]: http://memeagora.blogspot.com/2006/12/polyglot-programming.html  
 [4]: http://developer.facebook.com/thrift/thrift-20070401.pdf  
 [5]: http://code.google.com/apis/protocolbuffers/docs/overview.html  
 [6]: http://developer.facebook.com/thrift/  
 [7]: http://incubator.apache.org/thrift/  
 [8]: http://www.lst.de/~cs/kode/index.html  
 [9]: http://developer.kde.org/documentation/tutorials/kconfigxt/kconfigxt.html  
 [10]: http://phonon.kde.org/  
 [11]: http://www.gnome.org/~federico/news-2007-06.html#18  
 [12]: http://forgeftp.novell.com/yast/doc/SL11.1/tdg/Book-YCPLanguage.html  
 [13]: http://forgeftp.novell.com/yast/doc/SL11.0/scr/index.html  
 [14]: http://live.gnome.org/Vala  
 [15]: http://dbus.freedesktop.org/

