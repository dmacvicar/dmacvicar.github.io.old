---
layout: post
title: Trying Rust (language) on openSUSE
date: '2014-01-16'
categories:
- Software
tags:
- golang
- llvm
- opensuse
- rust
- suse
---

I am always playing with new languages. I love learning the thinking and philosophy behind them. Usually I throw them away after one evening (mostly mee-too's), but there are some that are very interesting and get explored a bit further, and some are of course "adopted" into your toolbox.

The last ones that I spend quite some time with were:

- Scala, which I used as base language for [some courses](http://duncan.mac-vicar.com/2014/01/09/2013-learning-retrospective-software/)
- Go, which I used with the intention of writing servers for the Raspberry Pi in combination with Javascript browser apps

I was expecting to find in [Go](http://golang.org/) the "right" native-compiled language. Go is simple, has great (integrated) tooling, very pragmatic syntax, etc. But it has a bunch of warts that just made me very uncomfortable when translating ideas into code:

- It does not support generics. Lists of voids (interface{})? no thanks.
- For a language having first class functions. the functional aspects of is close to zero. After you did some ruby is hard to think again without using expressions and functional constructs like map/select/etc. "if" as a statement where you assign nil to the variable first... nope

At that time Mozilla's [Rust](http://www.rust-lang.org "Rust")&nbsp;was still &nbsp;immature and I did not manage to build a decent package to start playing with it.

Rust is a nice language. It is a bit noisy from the syntax, but it has been improved a lot in the latest releases. Garbage collected pointers (@) are gone and moved to the standard library which means there is now only ~ and &. But it is a language that got a lot (almost everything) "right":

- For having lot of features, it is very lightweight if you use the basics
- It has decent integration with C
- It has good [support for concurrency](http://static.rust-lang.org/doc/master/rust.html#tasks) using light tasks and channels
- Supports something more flexible than classes, called [traits](http://static.rust-lang.org/doc/master/rust.html#traits)
- The toolchain is modern and very integrated
- It does not reinvent everything. No own build system, and that is probably why it fits so well with Linux packaging
- It uses [LLVM](http://llvm.org/) as the backend, so we can expect good portability and getting all the optimization work from the LLVM guys for free
- It has the feeling of ruby: object-oriented, with some functional sugar
- [Pattern matching](http://static.rust-lang.org/doc/master/rust.html#match-expressions)!

But 0.9 was just released and this time I found some [very](https://build.opensuse.org/package/show?project=home%3Aradekmi&package=rust) decent [.spec files](https://build.opensuse.org/package/show?project=home%3AIgnotusp%3Adevel%3Alanguages%3Arust&package=rust) to start with. I improved those with:

- Do not install compiler related libraries to /usr/lib
- Make parallel installation possible using /etc/alternatives

This is very similar how the Java package is structured. I also built a package for rust-bindgen, which uses [clang](http://clang.llvm.org/) to parse headers and generate bindings for C libraries.

So, grab one of the [repositories](http://download.opensuse.org/repositories/home:/dmacvicar:/rust/)&nbsp;and:

[code language="bash"]  
zypper ar&nbsp;http://download.opensuse.org/repositories/home:/dmacvicar:/rust/openSUSE\_13.1/home:dmacvicar:rust.repo  
zypper ref  
zypper install rust

Then continue with the [tutorial](http://doc.rust-lang.org/doc/0.9/tutorial.html) and the [manual](http://doc.rust-lang.org/doc/0.9/rust.html). Happy learning!.

