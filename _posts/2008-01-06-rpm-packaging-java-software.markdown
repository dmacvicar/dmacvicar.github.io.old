---
layout: post
title: rpm packaging java software
date: '2008-01-06'
tags:
- java
- packaging
- rpm
---

You may know, that rpm supports (is that the right name?) "named" dependencies. That is, you can require arbitrary strings other packages provide, no matter if that arbitrary string is a package or not.

Take curl as an example, lets see what curl provides:

```
# rpm -q --provides libcurl4
 libcurl.so.4()(64bit)
 libcurl4 = 7.16.4-16.2
```

You see the package provides the libcurl.so.4()(64bit) symbol. Now lets explore the requires:

```
# rpm -q --requires libcurl4
curl-ca-bundle
/sbin/ldconfig
/sbin/ldconfig
rpmlib(PayloadFilesHavePrefix) <= 4.0-1
rpmlib(CompressedFileNames) <= 3.0.4-1
libc.so.6()(64bit)
libc.so.6(GLIBC_2.2.5)(64bit)
libc.so.6(GLIBC_2.3)(64bit)
libc.so.6(GLIBC_2.3.4)(64bit)
libc.so.6(GLIBC_2.4)(64bit)
libcrypto.so.0.9.8()(64bit)
libdl.so.2()(64bit)
libdl.so.2(GLIBC_2.2.5)(64bit)
libidn.so.11()(64bit)
libssl.so.0.9.8()(64bit)
libz.so.1()(64bit)
rpmlib(PayloadIsBzip2) <= 3.0.5-1
```

curl depends on zlib. But it is not needed to have the package zlib installed. Any package "providing" the symbol "libz.so.1()(64bit)" will fulfill the requirement. If you have a megapackage-bundle rpm which includes lot of libraries, it could also fulfill curl's requirement.

Advantages:

* They are generated automatically from elf binaries, perl modules, etc. AFAIK Debian depend only on packages. If the depending package stops providing a library, you will not know until you get an error.

* You don't need to maintain the requirements manually and the spec file only lists what you need to build the package.

* The requirements are more granular. If you depend on a big package, you are not really needed all the stuff. Therefore there is no need to split packages in thousands of smaller pieces.

* Don't couple the libraries with package names. AFAIK Debian does not support this. And a virtual package called "zengine" or whatever would need to be created so packages need to depend on it.

Disadvantages:

* More noise. Metadata gets bigger.

* More processing needed by the solver, as there are much more provides and requires.

This technique is not only used with native libraries, but with other languages. Look at mono:

```
# rpm -q --provides mono-web
mono-web-forms
mono-web-services
mono-remoting
mono(Mono.Http) = 1.0.5000.0
mono(Mono.Http) = 2.0.0.0
mono(System.Runtime.Remoting) = 1.0.5000.0
mono(System.Runtime.Remoting) = 2.0.0.0
mono(System.Runtime.Serialization.Formatters.Soap) = 1.0.5000.0
mono(System.Runtime.Serialization.Formatters.Soap) = 2.0.0.0
mono(System.Web) = 1.0.5000.0
mono(System.Web) = 2.0.0.0
mono(System.Web.Services) = 1.0.5000.0
mono(System.Web.Services) = 2.0.0.0
```

Did I mention that they are also used in kernel modules to match driver compatibility? The kernel package provides certain interface names, with a version which is actually some kind of hash of the signature of those interfaces. So a driver will depend on those.

```
# rpm -q --provides kernel-default
kernel-default-nongpl
kernel = 2.6.22.9-0.4
k_deflt
k_numa
k_smp
smp
kernel-smp
kernel(drivers_ata) = 8c8c26cd48be2c29
kernel(drivers_char_tpm) = c2f46bb4192faaf6
kernel(fs_nfsd) = 5302e5e83fcef713
kernel(drivers_media_dvb_frontends) = 6c12beb7312724c0
kernel(drivers_video_matrox) = 9d4717fb264df90d
kernel(drivers_block_paride) = c3185e6447e90578
...
```

Now, lets get back to Java. I tried to package jruby, microemu and other small things. The amount of jars you need to build, which are not yet available in the distribution is amazing. You give up after three iterations. And the binary tarball with the jars is there as a temptation.

However, binary jars sometimes include everything you need to \_run\_ the program. Including many java packages (namespaces) from external projects. For example, microemu includes nanoxml and asm.

However, the information that current java packages carry is minimal:

```
# rpm -q --provides ant
apache-ant
ant = 1.7.0-30
```

Even source packages, usually include the full source, but all binary jar's required to build the source in a lib directory. We don't want to include these jars in the rpm package (our goal is to include only what we compiled). But if these jars are not available in the distribution, we also don't know the name of the package providing those jars.

However, we could build the source using those jars, and then make the rpm require the java namespaces the jar requires by examining the resulting jar. And therefore also the spec file could remove the namespaces included in the resulting jar which don't belong to that package, and in that way we don't provide the rpm package version's version of those namespaces.

So, I discussed this with [Pascal][2]. He hacked a bash line to find the provides quicker than me ;-)

```
for jar in *.jar; do
  jar tf "$jar"|grep '.class$'|sed 's|/[^/]*.class$||'|sort -u|while read package; do
    echo "Provides: java(${package////.}) = %{version}-%{release}"
  ; done
; done
```

Which for ant, results in:

```
Provides: java(org.apache.tools.ant) = %{version}-%{release}
Provides: java(org.apache.tools.ant.dispatch) = %{version}-%{release}
Provides: java(org.apache.tools.ant.filters) = %{version}-%{release}
Provides: java(org.apache.tools.ant.filters.util) = %{version}-%{release}
Provides: java(org.apache.tools.ant.helper) = %{version}-%{release}
Provides: java(org.apache.tools.ant.input) = %{version}-%{release}
Provides: java(org.apache.tools.ant.listener) = %{version}-%{release}
... much more
```

However, we still need the requires information. We discussed about using [jarjar][4]. But Pascal hacked a [better way][3] using [jdepend][1]. The output looks like:

```
Provides: java(org.springframework.dao)
Requires: java(org.springframework.core)
Provides: java(org.springframework.dao.support)
Requires: java(org.apache.commons.logging)
Requires: java(org.springframework.beans.factory)
Requires: java(org.springframework.dao)
Requires: java(org.springframework.util)
Provides: java(org.springframework.transaction)
Requires: java(org.springframework.core)
Provides: java(org.springframework.transaction.annotation)
Requires: java(org.springframework.transaction.interceptor)
Provides: java(org.springframework.transaction.interceptor)
Requires: java(org.aopalliance.aop)
Requires: java(org.aopalliance.intercept)
Requires: java(org.apache.commons.logging)
Requires: java(org.springframework.aop)
Requires: java(org.springframework.aop.framework)
Requires: java(org.springframework.aop.framework.adapter)
Requires: java(org.springframework.aop.support)
Requires: java(org.springframework.aop.target)
Requires: java(org.springframework.beans.factory)
Requires: java(org.springframework.beans.propertyeditors)
...
```

The missing piece would be stripping unwanted namespaces from the final compiled jar. This can be done using either jarjar or manually by unpacking the jar, deleting some directories and repacking it.

This would allow for much easier building of java packages from source without needing the full dependency tree prepackaged. And build those later one by one, and in a much more granular way. It would also help when building directly from binary jars, resulting in much more granular rpms.

What do you think? What other things can be improved when packaging java software?

[1]: http://clarkware.com/software/JDepend.html  
[2]: http://dev-loki.blogspot.com/  
[3]: http://linux01.gwdg.de/~pbleser/files/rpm/java/FindRequires.java  
[4]: http://code.google.com/p/jarjar/

