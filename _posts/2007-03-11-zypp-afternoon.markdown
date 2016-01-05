---
layout: post
title: zypp afternoon
date: '2007-03-11'
---

pterjan asks himself [why rpm -qa is so slow][1] and then [answers himself][2]. The reason checks digests and signatures by default when reading. I wonder what behavior you get by default when reading from the db with the API.

Redhat magazine features [the history of rpm][3].

Saturday afternoon spent on zypp redesign.

As the metadata cache concept evolve, I need at least prototypes of the future milestones concepts to test and play.

One of the parts zypp needs a major redesign is source handling, because this affects annoying bugs like refreshing the sources when you just need to create the object. This is due because the same component is responsible of downloading, and creating the resolvables.

The original concept of the redesign was splitting sources into three components:

* Downloaders: take an URL and download the metadata. Sounds piececake, but most metadata require some parsing in order to know what you need to download.  
* Parsing: parse raw metadata and fill the cache via the store API.  
* The Source itself. Construct the resolvables from the store, using the query API.

During FOSDEM I was able to end with a working MediaSetAccess class which is independant from the SourceImpl class.

MediaSetAccess is a thin layer over the Media API that knows about multiple medias, and can be used by any other component. It allows to attach a media verifier to each media number.

Today I got two more prototypes. A Fetcher class, that allows you to enqueue a bunch of remote files. You can define places where those files can already be cached. Then you can start the task of transfering from the network all files that can't be retrieved directly from the defined caches. The main purpose of Fetcher is at some point allow parallel downloads without affecting the other components.

The [SourceType]Downloader class is specific to a source type, and encapsulates the logic of getting metadata files to know which other files are needed, and uses a Fetcher to get those files as they are known.

 ![]({{ site.baseurl }}/assets/images/posts/media_httpimg147image_dmbsx-scaled500.png)

I am still unsure if some responsabilities should go in MediaSetAccess or in the Fetcher class, but I will figure that out later, as I replicate at least one complete Downloader.

[1]: http://fasmz.org/~pterjan/blog/?date=20050914  
 [2]: http://fasmz.org/~pterjan/blog/?date=20051208  
 [3]: http://www.redhatmagazine.com/2007/02/08/the-story-of-rpm/

