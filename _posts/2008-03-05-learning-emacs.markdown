---
layout: post
title: Learning emacs
date: '2008-03-05'
tags:
- development
- emacs
---

I am not good at keyboard shortcuts. I started using mice very early. Therefore anytime I post a video recording my screen, I get more comments about my stupid ways to clear the screen or move the cursor, than about the content itself.

However, I like to change my habits to be more efficient and I keep trying new stuff, and old stuff too.

I tried learning vim, and I was successful in using it during the last years for console usage and basic file editing. However, I was never able to remember more than opening/saving files and deleting lines. I was using Kate for most of my coding needs. I tried switching from Kate to vim for coding like three or four times without any success.

During this trip, and brought some emacs tutorials with me, in order to give it a try. I was expecting something horrible, but surprise, after a few hours, I could do much more than I was able to do with vim, and I had no big problems with the ctrl-something key shortcuts (I have yet to try on my Microsoft Natural keyboard though).

I am now in the process of setting up some packages I need (code browsing and completion) and till now I am very happy. There are a couple of things that really suck:

* Setting up the environment sucks because you have to do it from scratch. There are no sane defaults for anything. No thing that ask you what do you do, and setup something that makes sense to start with.

* XEmacs and Emacs features.They forked one from the other and at some point XEmacs had more features, but now both have things the other hasn't and you can't tell exactly what it is.

* XEmacs and Emacs user interface. I discarded XEmacs because it has this horrible tcl/tk look and feel, like using its own widgets over xlib. It had a experimental gtk user interface based on 1.x, but its status is not really clear. At least our package does not enable it. There are screenshots of Emacs running on OS X that look very sweet. Sad that all efforts of bringing Qt to emacs are more or less dead.

For now, I choose GNU Emacs, just because I have some sense of aesthetics and usability. XEmacs is \_and\_ look like 1970. With GNU Emacs I only have to suffer of the gtk "original" file dialog but I suffer from it in lot of gtk-infected programs (starting with Firefox) so it is not very serious. I can live with it, and I am sure I can use KDE file dialog by using some lisp magic and the kdialog command.

Other pending issues:

* setting up cedet/ecb to get auto completion and code browsing.

* setting up git mode and learn how to use it

* integrate cmake mode and a sane way to use the compile commands out of the source tree

May be next time I record a video, you will see me using ctrl-L instead of typing "clear" ;-)

