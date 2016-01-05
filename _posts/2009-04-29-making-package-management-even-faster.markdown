---
layout: post
title: Making package management even faster?
date: '2009-04-29'
tags:
- newsuse
- software
- suse
- yum
- zypper
---

Joshua Bahnsen wrote [on the yum mailing list][1] (summarizing):

> Is there support for compression types other than gzip for the yum metadata? For example, bzip2 or LZMA?  
> I am keeping track of 16 RHEL channels, using createrepo with the standard gzip I am totaling 1.4 GB of metadata. Compressing those same XML documents with LZMA yields a total of 140 MB. That's 10x savings overall, I think that's worth a look.

James Antill asked some questions, and also pointed some issues:

> These we can probably try and help with, but we've been asking and waiting for 12+ months for RHN and CentOS to move to generating .sqlite files server side. So I wouldn't bet that we can help in the general case, quickly. Plus any client side support for lzma probably wouldn't get into 5.x until at least 5.5 (more likely 5.6 or 5.7). So realistically you are targeting Fedora and 6.x for a change like this.

For Fedora the main issue is that they use sqlite version of the metadata generated on the server side, therefore they are more interested in how those files compress.

openSUSE still downloads the raw metadata. Mostly because our sat-solver is really fast converting it to the solv cache. We have thought about shipping solv files in the metadata some day, but we still haven't look at that in detail.

However I wanted to see how it could make a difference for openSUSE by using lzma when compressing the metadata. I took the 11.0 update repository, as it has quite a lot of stuff in it, which makes it big to download when it changes.

For that, only a minimal patch to /usr/bin/repo2solv.sh is needed. You can get that [patch here][3].

Here are my metadata size results:

 ![]({{ site.baseurl }}/assets/images/posts/media_httpspreadsheet_jtcym-scaled500.jpg)

I also looked to how fast the parsing of the metadata goes. For that I parsed each file 3 times and averaged each time component. I dropped disk caches before.

 ![]({{ site.baseurl }}/assets/images/posts/media_httpspreadsheet_fsave-scaled500.jpg)

As you can see, the repo is 4x smaller and parses 2x faster, which is still very nice(the 10x gains seems to be for files like other.xml which we don't download).

You can get the data from [here][2].

A change like this is still challenging:

* We need upstream support to keep repos compatible  
* It can only be done in Factory, and people need to update the software management stack before the format is changed.

What do you think?

[1]: http://lists.baseurl.org/pipermail/yum/2009-April/022560.html  
[2]: http://spreadsheets.google.com/ccc?key=pbhIDzxcltze0yu1aSoUjFw  
[3]: http://www.pastie.org/461773

