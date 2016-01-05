---
layout: post
title: Realtime syntax checking with emacs
date: '2011-08-30'
categories:
- Software
tags:
- c
- emacs
- ruby
- suse
---

_ **Note: this post is a web cache recover of a November 2010 post that got lost with the blog crash. I updated it to add C/C++ autocompletion.** _

One of the nice features of fat IDEs is that you get real time syntax checking. Some languages make it easy, some not.

For example Eclipse has access to the compiler as a service inside the IDE, and it checks the code as you type, even suggesting fixes.

I started to research what could be done on the emacs side to get a better experience, as when I am coding, I am usually thinking at the same time I write, and this means I make more syntax errors than the average guy.

So I found [flymake](http://flymake.sourceforge.net/ "flymake"), which is included by default in the emacs package. Copy pasting some snippets from the [emacs wiki](http://www.emacswiki.org/emacs/FlyMake "Emacs Wiki") I got really nice real time checking for ruby:

![]({{ site.baseurl }}/assets/ruby-images/posts/1.png)

Going forward with python was easy. A flymake extension that uses pyflakes (a python checking program) was already available.

![]({{ site.baseurl }}/assets/python-images/posts/1.png)

It was with C/C++ where things started to get more complicated. Flymake has support for those hardcoded so that make with a special target is invoked. I wanted to check the current file only. Also gcc syntax errors are not that good.

So I started to modify the extensions I had seen to use [clang (and clang++)](http://clang.llvm.org/ "clang") from the [LLVM](http://www.llvm.org "LLVM Project") project. Once it worked, I got nice error messages on C/C++ files:

![]({{ site.baseurl }}/assets/c-images/posts/1.png)

I was so happy to have this working on C++ that I needed more to challenge my new acquired mastery. So I decided to try it with [ycp](http://doc.opensuse.org/projects/YaST/openSUSE11.1/tdg/Book-YCPLanguage.html "YCP language"). Not that I code with it that often, but I have colleagues who do. After adapting the extensions, here is the result:

![]({{ site.baseurl }}/assets/ycp-images/posts/1.png)

You can find all the required files on my [emacs setup repository](https://github.com/dmacvicar/duncan-emacs-setup "Duncan emacs setup"). Follow custom.el which goes to custom-$lang.el to site-lisp/flymake-$lang.el.

**Update 20.11.2010:**

Thanks to [this post](http://mnemonikk.org/2010/11/05/using-flymake-to-check-erb-templates/) I managed to get also Rails .erb templates working. Those are more tricky as they can’t be parsed directly by ruby, but they have to go first through erb -x and then through ruby -c. I ported the script to the style of loading (init and load functions) I was already using.

**Update 30.08.2011:**

Thanks to [this autocomplete extension](https://github.com/brianjcj/auto-complete-clang), I was able to setup autocompletion using clang for C/C++. Here is how it looks like:

![]({{ site.baseurl }}/assets/images/posts/c-2.png)

Next! ruby autocompletion.

