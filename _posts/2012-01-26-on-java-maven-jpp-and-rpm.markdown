---
layout: post
title: On Java, Maven, JPP and rpm
date: '2012-01-26'
categories:
- Software
tags:
- fedora
- java
- maven
- rpm
- suse
---

Java on Linux has been always a "special" topic. They don't mix well.

The mindset of Linux distributions is very different to the Java world when it comes to build software. This is understandable as they have different requirements.

In the Java world, there is the concept of artifacts. You build org.foo.bar:bar-moo:1.1 once and it stays there forever, archived for anyone to use it. Tools like maven and ivy allow developers to specify in their source tree the specific dependencies of their components and those are grabbed from the network, the software built and then publish the output as a new artifact that others can grab.

Linux distributions on the other hand, bootstrap the complete stack from source. They don't take the binary artifact from upstream but build it, and then use the binary they built to build the next. This seems to work pretty well for C, C++, and for Ruby, Python, let's say it "works".

When it comes to package Java software, Linux distributors find themselves in the following situation:

- If a buildable source tarball is provided then you are lucky.
- If the buildable tarball is provided, it will either include a directory full of binary jars (the build dependencies) or it will have a very automatic build system grabbing them from the network.

This clashes with Linux in various edges:

- **Linux distributions have normally one version of each component**. The Java method works well if you bundle your dependencies inside your application, but not if everything is a reusable component. I have mixed feelings here. I think bundling your dependencies in the application for everything that is not part of the "base" system is the right approach. Updating them can break the application and trying to control this via QA only moves QA from the application developers to the distribution itself. When I ship apache-commons-collection as part of my Foobar Java application as a Java package, I am inviting everyone to use it, forcing myself to give support for it out of the context of the application.
- 

**Distributions needs to build from source**. Even if you get rid of the above requirement and you bundle all your dependencies, distributions want to build everything from source. This has technical and legal reasons. SUSE build system does very complex checks on every package that it builds. Those checks are part of the quality we sell to our customers. Other reasons are legal: I am still trying, for example, to build the [Play!](http://www.playframework.org) framework. Even if it is BSD, [it includes some .jars inside of unknown origin](http://groups.google.com/group/play-framework/browse_thread/thread/377549fe950175e5/463c8e762f38a49a?lnk=gst&q=duncan#463c8e762f38a49a). What would happen if one of these jars results to be proprietary?. Michael Vyskocil had a [similar issue with openproj and its bundled dependencies](http://sourceforge.net/mailarchive/forum.php?thread_name=200808121025.47048.mvyskocil%40suse.cz&forum_name=openproj-developers).

Another reason to build from source is support. Enterprise distributions sell support and if a customer has a problem, we will fix it on our own and not wait for upstream to release a new version. Having a standardized way to build from source with our own fixes allows us to serve our customers. We can bundle jars in our application, but if a bug traces back to a jar we included, we would need to change the complete build description of the product in order to take this component. If we are able to rebuild this component at all. It already happened to us once with an XML-RPC library. And we were glad that it could be fixed by adding a patch to the rpm build description.

- **Grabbing dependencies from the network** just does not work. All packages are built without network access for security reasons.

Because the Linux distributions know that they are not the center of the universe, they adapted. At the beginning things where still ok. Ant was very popular and basically you recursively packaged all build dependencies until you could build your package, in the same way:

- unpack
- delete all binary jars
- set CLASSPATH to the jars grabbed by the packaged build dependencies
- call ant
- install the jars

Something like this:

`
%prep
%setup -q`

# remove all third party jars  
find . -iname '\*.jar' | xargs rm -rf

%build  
export CLASSPATH=$(build-classpath foo)  
ant

Until this was true, the world was still fine. ant needed bootstrapping, but this was doable.

Until Maven...

Maven is at the same time revolutionary and one of the biggest atrocities I have seen when building software.

On the positive side:

- it defined a common convention for modules: groupId, artifactId, version.
- it defined a standard layout for the source tree
- it started a wave of convention over configuration that Java was always lacking

on the negative side:

- it requires itself to build itself
- it can't build much itself, so it requires plugins to build anything
- plugins require maven to be built, plus more dependencies, which are usually require maven to build, plus... plugins.

All the above means that maven basically requires all the software it is supposed to build. Not the best design for a build system.

To make things worse, maven grabs dependencies from the network, which is what is disabled in our distro build process.

Fedora has done quite a progress providing a maven stack, by improving extending on the conventions the [JPackage project](http://www.jpackage.org/) started for maven packages. This is implemented using what is called a ["dependency map"](http://fedoraproject.org/wiki/Java/JPPMavenReadme).

The approach works by installing some xml files per-package that map the maven artefact identifiers (groupId, artifactId) to a local jar in the system. Then maven itself is patched to include a resolver for artefacts with some properties:

- ignores multiple versions (usually in Linux you have one version installed)
- resolves the artefact names to local installed jars. Every package uses macros to add stuff to the dependency map

What I don't like for this approach is:

Why would anyone in their sane mind use XML files to create mappings to files when you are in a UNIX-like OS and you have the filesystem and symbolic links?.

It is very explicit. It does not rely on a simple convention.

A second issue is how packages are built. This is SUSE specific. Fedora can bootstrap packages with circular dependencies by introducing a binary package A, build other dependencies until it can build a real A. Once a package is built, it stays in the buildsystem frozen as an artefact (just like the Java world).

In the [openSUSE Build Service](http://build.opensuse.org), the repository is always ready to bootstrap. For circular dependencies you create a package A-bootstrap that provides A and set the project config to prefer A. When A does not exist, A-bootstrap is grabbed, but as soon as A is there, it is preferred and used. When a package changes the packages depending on it are rebuilt automatically. This approach has several advantages, but makes hard to bootstrap a collection of packages where everything depends on everything.

In openSUSE, we have successfully build many maven dependent packages in the Java:base project without having a maven package by using the maven ant plugin to generate a tarball with ant build files.

This method does not work for every package, specially when files are generated, then one needs also to include those. But they may be good enough for solving our specific bootstrapping problem. The question is how many bootstrap packages would we need.

Another idea is to use package with binary jars for bootstrapping.

Fedora is not very happy with the current situation either, and they have been researching adding native support to [Koji to build maven packages.](http://fedoraproject.org/wiki/KojiMavenSupport)

In any case, I think there is room for improvement everywhere. I think the Maven infrastructure can be simplified taking into account that what maven contributed to the world was a (now) popular way to identify a module, and this is now being used also outside of Maven. [Apache Ivy](http://ant.apache.org/ivy/), [SBT](https://github.com/harrah/xsbt/wiki), [Gradle](http://gradle.org/), etc all support maven-style repositories and support refering to an artefact as groupId:artifactId:version.

Why not instead of a depmap just have:

```
/usr/share/java/foo.jar
/usr/share/java/org.bar/foo.jar -> /usr/share/java/foo.jar
/usr/share/java/org.bar/foo.pom
```

And have the Maven patched resolved to just look there?

If you need parallel versions, then just

```
/usr/share/java/foo1.jar
/usr/share/java/foo2.jar
/usr/share/java/org.bar/1.0/foo.jar -> /usr/share/java/foo1.jar
/usr/share/java/org.bar/2.0/foo.jar -> /usr/share/java/foo2.jar
/usr/share/java/org.bar/1.0/foo.pom
/usr/share/java/org.bar/2.0/foo.pom
```

Or use the standard alternatives:

```
/usr/share/java/foo.jar -> /etc/alternatives/foo.jar
```

The resolver would first look for the specific version described in the .pom file as /usr/share/java/$groupId/$version/$artifactId.ext. If it is not found, it could fallback to just look for /usr/share/java/$groupId/$artifactId.ext. This supports most cases where we just have one version for the system and exceptions for some packages where providing a specific version in parallel is also required. If the same jar is also known under a different groupId, well, then you create another symlink.

Then, build-classpath is enhanced so that in addition of being able to say 'build-classpath commons-logging' you can also call 'build-classpath commons-logging:commons-logging'. Identify every module by this convention.

The same with Provides: java(commons-logging:commons-logging). Fedora is already doing this as mvn(..), but is this maven specific?.

Why do we need xml files with maps, fragments of XML files that need to be updated at install and uninstall time?.

### Looking for a new solution...

I discussed this with Fedora developers [Alexander Kurtakov](http://fedoraproject.org/wiki/User:Akurtakov) and [Stanislav Ochotnicky](http://fedoraproject.org/wiki/User:Sochotni) and they mostly agreed with my concerns. They pointed me to Carlo de Wolf's work [on a similar solution](https://github.com/wolfc/fedora-maven), but using a standard maven repository layout.

Carlo's solution does not touch maven but is implemented as a plugin that gets loaded using a custom config file that is used when you call the wrapper script fmvn instead of mvn (for Fedora-Maven).

The whole solution as they described it has some extras like [macros to symlink](http://fpaste.org/iDer/) the maven repository artifacts so that they can be found as artifacts in the [JPP](http://fedoraproject.org/wiki/Java/JPPMavenReadme) layout. I am not sure if we need this. What I like from the solution alone:

- It does what you expect: uses only the local repository and ignores versions (uses latest) if the requested version is not found.
- It does not require macros. We need to build stuff on released distros and it is no fun to introduce new rpm macros.
- It does not require patching maven. fmvn is a separate package, providing the plugins and the wrapper script.
- As soon as Carlo gets "mvn install" working, there is no need to manually install the jar/pom in the spec file. Just calling "fmvn install" should build and install it.

I have been playing with Carlo's plugins and it looks very promising. Fedora would need time to switch to a solution like this, but at SUSE we don't have maven in our stack so we have nothing to lose and at the same time we can help serving as a test bed.

### The current plan...

Not having the need to patch maven allows us to use a vanilla build of Maven for bootstrapping.

maven-bootstrap (upstream binary release, Provides: maven)  
fmvn-bootstrap (binary jars built locally with maven, Provides: fmvn)

_Note: If you have more than one package with the same capability and want to use it in (Build)Requires, you will need to setup "Prefer:" in prjconf._  
  
We would like to build now maven using fmvn. Here is where the circular dependencies start. We need maven (provided by maven-bootstrap) and it dependencies, like plexus and a big bunch of maven plugins.

Here is where [pom2spec](https://github.com/dmacvicar/pom2spec) comes to the rescue. This script allows to quickly create bootstrap packages from search.maven.org. It is based on [Pascal Bleser's script](http://lists.opensuse.org/opensuse-packaging/2011-08/msg00073.html).

So lets say I need a bootstrap package for maven-compiler-plugin:

`
org.apache.maven.plugins:maven-compiler-plugin : using version 2.3.2
Writing maven-compiler-plugin-bin.spec
Done
Downloading maven-compiler-plugin-2.3.2.pom...
######################################################################## 100.0%
Downloading maven-compiler-plugin-2.3.2.jar...
######################################################################## 100.0%
t http://repo1.maven.org/maven2/org/apache/maven/plugins/maven-compiler-plugin/2.3.2/maven-compiler-plugin-2.3.2.pom
_ http://repo1.maven.org/maven2/org/apache/maven/plugins/maven-compiler-plugin/2.3.2/maven-compiler-plugin-2.3.2.jar
`

Which generates the following files:

`
maven-compiler-plugin-2.3.2.jar
maven-compiler-plugin-2.3.2.pom
maven-compiler-plugin-bin.spec
`

If I build it, I get an rpm with the following layout:

`
/usr/share/java/maven-compiler-plugin.jar
/usr/share/maven/repository/org/apache/maven/plugins/maven-compiler-plugin/maven-compiler-plugin-2.3.2.jar
/usr/share/maven/repository/org/apache/maven/plugins/maven-compiler-plugin/maven-compiler-plugin-2.3.2.pom
`

_/usr/share/java/maven-compiler-plugin.jar_ is just a symlink to the real jar. This layout is enough for fmvn to find the artifact and also for legacy packages to just use build-class-path. It would still be better to enhance build-class-path to also accept groupId:artifactId keys and return the path to the jar.

The -bin suffix is to allow then the real package (built from source) to coexist in the same repository. The package with the -bin suffix also "Provides:" the package without the suffix so it can be used by dependent packages. Actually both "Provide:" _java(org.apache.maven.plugins:maven-compiler-plugin)_ which is what a package that depends on it should "BuildRequire:".

Once Carlo's resolver works with "mvn install" I will try to build a repository following this method.

