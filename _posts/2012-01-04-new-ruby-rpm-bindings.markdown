---
layout: post
title: new ruby RPM bindings
date: '2012-01-04'
categories:
- Software
tags:
- ffi
- linux
- rpm
- ruby
- suse
---

The original [ruby-rpm](http://rubygems.org/gems/ruby-rpm "ruby-rpm") bindings were originally written around the year 2002 for the Kondara Linux distribution. [David Lutterkort](http://twitter.com/#!/lutterkort) adopted them to power various systems management pieces written in ruby, and later I did a couple of releases.

After openSUSE 12.1 was released, the gem stopped building against the current rpm (4.9.x) and something needed to be done. After studying the code a lot I figured out:

- API compatibility was important, as the goal was to keep some software running.
- I did not want to add more #ifdefs to the code, as it was already supporting ancient rpm versions.
- I wanted to avoid C where ruby could be used instead
- I wanted [good documentation](http://rubydoc.info/github/dmacvicar/ruby-rpm-ffi/master/frames) (I had added some to ruby-rpm in the latest releases)

I decided to start fresh: target rpm 4.9.x first and later see if older rpms can be supported. I want to introduce an early release of the [new rpm gem](http://rubygems.org/gems/rpm).

### What does this milestone implement?

- Querying rpm database
- Querying packages

### How is this gem different to the original ruby-rpm?

- It is written in pure ruby, and uses FFI to access librpm
- It is documented
- It will be compatible with ruby-rpm. The testsuite is a continuation of the original
- It is MIT licensed instead of GPL. [Kenta Murata](http://twitter.com/#!/mrkn) gave me permission to&nbsp;re-license&nbsp;all the code I studied while writing the pure-ruby version

### What is missing?

- Only rpm 4.9.0 is supported for now. May be older versions work. I do know that **4.4.x does not work.**
- Not all APIs are covered yet, for example the RPM::Source class or methods to execute transactions (install, remove).

### What did I learn on the way

#### The RPM API and compatibility

The RPM API is quite scary. Part of it because the field it covers but also older rpm versions exposed lot of unnecesary stuff to the API. Comparing the API across versions shows that [Panu](http://laiskiainen.org/)&nbsp;is doing an awesome job cleaning it up.

Because the mess in rpm 4.4.x, supporting older rpm versions will not be trivial. I realized very late that functions like headerNew are not even exposed as symbols in 4.4.x.

#### FFI is great, but it is not there yet

FFI is [advocated as a better way to access native code](https://github.com/ffi/ffi/wiki/why-use-ffi) from ruby interpreters. The true is that ruby never had any API to do that, and when you write ruby C extensions you are just playing with the MRI interpreter guts.

With FFI, each interpreter provides the FFI API and implements it. For example, JRuby may use JNI, and MRI may implement it as a C extension using the API we all already know.

However, after being unable to run the gem on rubinius because [its FFI does not implement enums](https://github.com/rubinius/rubinius/issues/682), I realized the C compatibility layer most interpreters provide may be even more mature than FFI itself.

Also, because you are accessing the library symbols, you inherit another set of problems, like the&nbsp;inability to refer to anything that is not a symbol, like macros.

It is still better, but it needs time to mature. I am happy that I can write ruby code that interacts with the operating system without having to tie the code to an specific interpreter or platform.

Go grab the [source code](https://github.com/dmacvicar/ruby-rpm-ffi) and send me a pull request!.

