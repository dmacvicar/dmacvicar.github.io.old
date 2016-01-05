---
layout: post
title: ecryptfs. spca5xx packages.
date: '2006-10-22'
tags:
- freedom
- Life
- newkde
- newsuse
- suse
---

Yesterday we enjoyed a nice party at Will's place. I was definitely [defeated on Karaoke by Lisann][2].

spca5xx webcam drivers package builds again for all SUSE versions, including factory.

 ![]({{ site.baseurl }}/assets/images/posts/media_httpimg181image_ebafj-scaled500.png)

Today I found about [eCryptfs][1]:

>  eCryptfs (SourceForge page) is a POSIX-compliant enterprise-class stacked cryptographic filesystem for Linux. It is derived from Erez Zadok's Cryptfs, implemented through the FiST framework for generating stacked filesystems. eCryptfs extends Cryptfs to provide advanced key management and policy features. eCryptfs stores cryptographic metadata in the header of each file written, so that encrypted files can be copied between hosts; the file will be decryptable with the proper key, and there is no need to keep track of any additional information aside from what is already in the encrypted file itself. Think of eCryptfs as a sort of ``gnupgfs.''

I haven't tried packaging it yet. But could be a nice addition to the [security:privacy][3] repository in the build service. Also you may want to give a look to [TPM keyrings][4].

[1]: http://ecryptfs.sourceforge.net/  
 [2]: http://www.flickr.com/photo_zoom.gne?id=275308852&size=s  
 [3]: http://build.opensuse.org/project/show?project=security%3Aprivacy  
 [4]: http://trousers.sourceforge.net/tpm_keyring2/quickstart.html

