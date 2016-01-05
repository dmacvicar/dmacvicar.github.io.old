---
layout: post
title: Introducing enhancerepo 0.3
date: '2008-09-26'
tags:
- newsuse
- rpmmd
- software
- suse
- yum
- zypp
---

## Introduction

You may know that we are slowly heading to use the [rpmmd][1] format as the default one. We already do for the build service since the beginning, and the only remaining part is the media.

Therefore, we have been following an strategy of, at the same time, extending rpmmd with our own extensions, but at the same time, talk to the yum people in order to think in more long term about changes in the format.

Right now, in the openSUSE 11.1Beta code, you find the [following extensions][2]:

* updateinfo.xml (patches, same Format as Fedora)  
* deltainfo.xml (delta-rpms, same format as yum-presto, different name)  
* suseinfo.xml (repository extra data)  
* susedata.xml (package extra data, like eulas and disk usage per mount point)

Apart of that, we sign our repositories.

Even for testing our own stuff, it became tedious to add extra metadata to repositories created with [createrepo][3].

## enhancerepo

[Enhancerepo][5] allows you to inject all the extra metadata to repositories in an easy way. It takes care of updating the index and compressing the files.

### Features

* Sign repositories  
* Generate eulas from text files with certain name conventions (package.eula)  
* Generate keywords from text files with certain name conventions (package.eula)  
* Add disk usage per mount point information  
* Add repository expiration time (for outdated mirror autodetection)  
* (expermental and incomplete) generation of updateinfo.xml

### Usage

```
Usage
-----
enhancerepo [OPTION] ... DIR

-h, --help:

   show help

--sign keyid

   Generates signature for the repository
   using key keyid

--updates

   Add updates from *.updates files
   and generate updateinfo.xml

SUSE specific repository data (suseinfo.xml):
--expire time

  Set repository expiration hint
  (Can be used to detect dead mirrors)

--repo-product prodname:

   Adds product compatibility information

--repo-keyword keyword

   Tags repository with keyword

SUSE specific package data (susedata.xml)
--eulas

   Drop packagename.eula files and
   the attributes will be added to susedata.xml

--keywords

   Drop packagename.keywords files and
   the attributes will be added to susedata.xml

--disk-usage

   Add disk usage information
   the attributes will be added to susedata.xml

Note: your .eula or .keywords file will be added to a package if it
matches its name. If you want to add the attributes to a specific
package, name the file name-version, name-version-release or
name-version-release.arch

DIR: The repo base directory ( where repodata/ directory is located )
```

### Download

Packages available [here][4].

```
git clone git://git.opensuse.org/projects/enhancerepo
```

[1]: http://en.opensuse.org/Standards/Rpm_Metadata  
 [2]: http://en.opensuse.org/Standards/Rpm_Metadata#Proposed_Extensions  
 [3]: http://linux.duke.edu/projects/metadata/  
 [4]: http://software.opensuse.org/search?q=enhancerepo  
 [5]: http://en.opensuse.org/Enhancerepo

