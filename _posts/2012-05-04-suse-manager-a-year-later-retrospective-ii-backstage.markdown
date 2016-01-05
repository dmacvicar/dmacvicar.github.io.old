---
layout: post
title: SUSE Manager, a year later retrospective II (Backstage)
date: '2012-05-04'
categories:
- Software
---

_[I posted a retrospective](http://duncan.mac-vicar.com/2012/05/03/suse-manager-a-year-later-retrospective/) of what we did for our customers in [SUSE Manager](http://www.suse.com/de-de/products/suse-manager/) during the last year. This is a continuation of that post, focusing on what is going behind the scenes._

### Testing

At SUSE we love to test a lot. We sell mission-critical Linux right?. If a package .spec file manages to pass our very strict suite of checks we have enabled in the [build service](//build.opensuse.org), that is only the first step. We also do testing at various other levels. Our internal [Jenkins instance](http://jenkins-ci.org) has hundreds of jobs testing the code SUSE contributes before it its packaged.

For SUSE Manager we had the challenge of various inter-dependent components we were at the beginning not very familiar with. We decided to test the end-user functionality and model the tests as you would do when using Behavior-Driven-Development, describing every test-case in a human-friendly way. The test-case implementation prepares some of the required environment based on the test-case descriptions.

One of the first things we open-sourced was [this testsuite](https://github.com/SUSE/spacewalk-testsuite-base) we created based on [Cucumber](http://cukes.info/), Capybara and Selenium.

For every commit in our git tree we use Jenkins to send packages to the [build service](http://www.open-build-service.org). A few minutes later the build service not only rebuilds the changed packages but also creates the appliance images. A job takes those and deploy them on a server and a client.

After that the testsuite is launched and more than 200 regression and feature tests are ran against the server.

![]({{ site.baseurl }}/assets/testsuite-images/posts/1.png)

We also added screenshots for failed tests. If a test fails, we can see exactly where the problem was, and fix it.

![]({{ site.baseurl }}/assets/images/posts/testsuite-2.png)

At the beginning, this test-suite allowed us to build an agile process by assimilating a big code-base we were not very familiar with at the beginning. Today, where we are already contributing to the upstream project, the test-suite is one of the building blocks of our continuous integration process. Allowing us to have a shippable product every night and add features with more confidence. For our customers it means enjoying SUSE Manager with the SUSE level of quality they are used to.

### Security

SUSE products are continuously audited for security problems. [Thomas](http://thetoms-random-thoughts.blogspot.de/2011/06/suse-manager-security-update.html/) has done a great job in the last years introducing security into the development process and educating developers about the topic. We found some problems and they were reported upstream using the standard procedures, most of the time with patches attached.

### Upstream

You may already know that SUSE Manager is built from the [Spacewalk](http://spacewalk.redhat.com/) project source code. When we shipped, we had done a lot of porting work which resulted on a lot of patches. It was a challenge for us at that point to become contributors to the project. Engineering and Project Management in products based on open-source components involve a lot of trade-offs between custom patches and up-streaming, between re-basing and forking. None is right. You play with them in order to satisfy your schedules, lower the overhead of custom patches (fork-debt) and adding features without sacrificing stability.

We are very happy with the results. We have contributed back everything that was possible and feel very welcome in the upstream community. We are very grateful to the Spacewalk developers: Miroslav Suchý, Jan Pazdziora, Thomas Lestach, Stephen Herr and others, who reviewed our patches and gave very insightful feedback.

Of course our git tree is different. There are patches that need some work in order to be up-streamed. But I think we made clear that we want to support the project. Where possible we are trying to upstream patches before we release them into our tree.

_I hope I was able to give you an insight on how SUSE Manager is being developed. We have more topics to share! may be in a future post._

