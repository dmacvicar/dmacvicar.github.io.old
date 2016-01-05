---
layout: post
title: enhancerepo 0.4.1 released
date: '2010-02-26'
tags:
- newsuse
- software
- suse
---

You can find the packages in my [home project](http://download.opensuse.org/repositories/home:/dmacvicar/).

# Changes

- Gemified. It is now available as a ruby gem. The rpm package is now therefore rubygem-enhancerepo
- Changed command line option parsing: for multiple items in an option, don’t use commas anymore: –generate-update pkg1,pkg2 becomes –generate-update pkg1 pkg2
- Support for product metadata generation from release packages. Enhancerepo will scan \*-release rpm packages and insert product.xml information in the metadata.
- Bugfixes for delta and updates generation
- Improved output

# Thanks

This release was possible thanks to contributions and testing from Michael Calmer and Jordi Massaguer Pla

# Next release

A 0.4.2 release will follow shortly, because 0.4.1 was queued from some time already. At least 2 features are planned for 0.4.2/0.4.3

- Merge splitting of any metadata patch from Jordi Massaguer Pla
- Merge pattern conversion patch from Michael Calmer

# About Enhancerepo

[Enhancerepo](http://en.opensuse.org/Enhancerepo) is a tool to edit and index rpm-md (a.k.a yum) software repositories. It is developed in [gitorious](http://www.gitorious.org/opensuse/enhancerepo).

## Main features:

- Reindex metadata
- Generate delta rpms from different package versions, and the corresponding metadata
- Generate updates (patches) metadata from different package versions
- Insert keywords and other properties
- Create product descriptions from -release packages
