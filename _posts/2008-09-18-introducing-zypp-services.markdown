---
layout: post
title: Introducing ZYpp services
date: '2008-09-18'
tags:
- newsuse
- suse
- zypp
---

## Credits

The following cool stuff would have not been possible without the hard work of [Jan Kupec](http://jniq.blogspot.com/), [Michael Andres](http://lizards.opensuse.org/author/mlandres/) and Michael Calmer, who implemented and tested this stuff. The original concept dates back from ZLM and the Novell Customer Center.

## Usecase #1: The project with multiple repositories and layers

Imagine the following usecase (with this one I am using some real request from our KDE guys)

The build service provides a KDE4 repository. Which in turn requires the Qt4 repository, because is built on openSUSE 11.0 + the new Qt4 repo.

When looking at this problem, repository dependencies is what comes to head in the first place. But forget about them. If package dependencies are complicated right now, imagine adding a secondary (and duplicated) layer of information. Packages already know their dependencies.

Now imagine our KDE guys can provide an URL, which you add to zypper. And this url returns a dynamic list of repositories. And zypper adds and remove repositories based on the information returned by this url on every refresh.

## Usecase #2: Update repositories based on the customer

This is actually where services where originated. Services were present in Novell ZenWorks. How it works?

The service url nu.novell.com is added to the system. But in this url also a customer id is present as a http username. When you registered, Novell knows the product this customer is linked to, and can return a dynamic list of update repositories based on the customer preferences, products and entitlements and the customer does not need to keep them in sync.

Now that we don’t have Zenworks in the base system, we still want this cool functionality for our customers.

Technically, this even allows us to offer hotfixes to L3 supported customers on the fly!

## Usecase #3: Dynamic repository collections

You are a build service user, and you have an account, and of course you have a list of watched projects you are interested to. What if you could keep your system repositores in sync with your watched project list.

Or what if the build service could offer a service based on keywords or other data: like http://build.opensuse.org/services/mostpopular/repo/repoindex.xml would contain dynamically the 15 most popular repositories. You add that service, and then ZYpp does the work for you of adding new popular repositories, and remove the old ones.

## Quick test

For a quick test, I needed a service provider, and I have none. I could have used our [Subscription Management Tool](http://www.novell.com/linux/smt/), which can act as a local proxy replacement of a Novell Update service provider, but I decided to implement a quick and dirty service that could be used as an example for other people that have nice ideas for this stuff.

So I created a rails application, a very simple one, that returns a dynamic [repository index](http://en.opensuse.org/Standards/Repository_Index_Service) by searching for the “emacs” keyword using the [opensuse-community search API](http://api.opensuse-community.org/searchservice/Search/Simple/openSUSE_110/emacs).

```
map.connect ':controller/repoindex.xml', :action => 'index'
```

Requesting my new service http://localhost:3000/repo/repoindex.xml returns me the following.

# zypper ls  
# | Alias | Name | Enabled | Refresh | Type  
---+-----------------------------+---------------------------------------------------------------------+---------+---------+-------  
1 | myservice | myservice | Yes | No | ris  
2 | FATE | FATE | Yes | Yes | rpm-md  
3 | KDE\_KDE4\_Factory\_Desktop | KDE 4.1.x Packages (openSUSE\_11.0) | Yes | Yes | rpm-md  
4 | KDE\_KDE4\_Factory\_Extra-Apps | The (default) Factory KDE 4 desktop with extra apps (openSUSE\_11.0) | Yes | Yes | rpm-md  
5 | KDE\_Qt | Current Qt 4.x packages (openSUSE\_11.0) | Yes | No | rpm-md  
6 | Virtualization:VirtualBox | VirtualBox | Yes | No | rpm-md  
7 | YaST\_SVN | Latest YaST svn snapshots (openSUSE\_11.0) | Yes | Yes | rpm-md  
8 | devel\_languages\_python | Python and Python Modules (openSUSE\_11.0) | Yes | Yes | rpm-md  
9 | home:jnweiger | home:jnweiger | Yes | Yes | rpm-md  
10 | home\_dmacvicar\_packagekit | PackageKit experiments (openSUSE\_11.0) | Yes | No | rpm-md  
11 | network\_utilities | Network Utilities (openSUSE\_11.0) | Yes | Yes | rpm-md  
12 | openSUSE-11.0 | openSUSE-11.0 | Yes | No | yast2  
13 | openSUSE-11.0-NONOSS | openSUSE-11.0-NONOSS | Yes | No | yast2  
14 | openSUSE-11.0-Update | openSUSE-11.0-Update | Yes | No | rpm-md  
15 | openSUSE\_Tools | openSUSE.org tools (openSUSE\_11.0) | Yes | No | rpm-md  
16 | outdated | outdated | Yes | No | rpm-md  
17 | packman | packman | Yes | Yes | rpm-md  
18 | rpms | rpms | Yes | No | rpm-md  
19 | ruby | Ruby | No | No | rpm-md  
20 | zypp\_svn | ZYPP SVN Builds (openSUSE\_11.0) | Yes | Yes | rpm-md  

Do you see that all the added repositories are “hidden” behind the added service.

If you want to see them, then you need to list the repositories instead:

[sourcecode wraplines="false"]  
# zypper lr  
# | Alias | Name | Enabled | Refresh  
---+-------------------------------------+---------------------------------------------------------------------+---------+--------  
1 | Education\_desktop\_openSUSE\_11.0 | Education\_desktop\_openSUSE\_11.0 | No | Yes  
2 | FATE | FATE | Yes | Yes  
3 | KDE\_KDE4\_Factory\_Desktop | KDE 4.1.x Packages (openSUSE\_11.0) | Yes | Yes  
4 | KDE\_KDE4\_Factory\_Extra-Apps | The (default) Factory KDE 4 desktop with extra apps (openSUSE\_11.0) | Yes | Yes  
5 | KDE\_Qt | Current Qt 4.x packages (openSUSE\_11.0) | Yes | No  
6 | M17N\_openSUSE\_11.0 | M17N\_openSUSE\_11.0 | No | Yes  
7 | Virtualization:VirtualBox | VirtualBox | Yes | No  
8 | YaST\_SVN | Latest YaST svn snapshots (openSUSE\_11.0) | Yes | Yes  
9 | \_distribution\_11.0\_repo\_oss\_suse | \_distribution\_11.0\_repo\_oss\_suse | No | Yes  
10 | devel\_languages\_python | Python and Python Modules (openSUSE\_11.0) | Yes | Yes  
11 | home:jnweiger | home:jnweiger | Yes | Yes  
12 | home\_beyerle\_openSUSE\_11.0 | home\_beyerle\_openSUSE\_11.0 | No | Yes  
13 | home\_dmacvicar\_packagekit | PackageKit experiments (openSUSE\_11.0) | Yes | No  
14 | home\_dsteuer\_openSUSE\_11.0 | home\_dsteuer\_openSUSE\_11.0 | No | Yes  
15 | home\_hmacht\_openSUSE\_11.0 | home\_hmacht\_openSUSE\_11.0 | No | Yes  
16 | home\_jefferyfernandez\_openSUSE\_11.0 | home\_jefferyfernandez\_openSUSE\_11.0 | No | Yes  
17 | home\_marcinzalewski\_openSUSE\_11.0 | home\_marcinzalewski\_openSUSE\_11.0 | No | Yes  
18 | home\_maw\_bzr\_openSUSE\_11.0 | home\_maw\_bzr\_openSUSE\_11.0 | No | Yes  
19 | home\_sjcundy\_openSUSE\_11.0 | home\_sjcundy\_openSUSE\_11.0 | No | Yes  
20 | home\_tiwai\_openSUSE\_11.0 | home\_tiwai\_openSUSE\_11.0 | No | Yes  
21 | network\_utilities | Network Utilities (openSUSE\_11.0) | Yes | Yes  
22 | openSUSE-11.0 | openSUSE-11.0 | Yes | No  
23 | openSUSE-11.0-NONOSS | openSUSE-11.0-NONOSS | Yes | No  
24 | openSUSE-11.0-Update | openSUSE-11.0-Update | Yes | No  
25 | openSUSE\_Tools | openSUSE.org tools (openSUSE\_11.0) | Yes | No  
26 | outdated | outdated | Yes | No  
27 | packman | packman | Yes | Yes  
28 | rpms | rpms | Yes | No  
29 | ruby | Ruby | No | No  
30 | server\_mail\_openSUSE\_11.0 | server\_mail\_openSUSE\_11.0 | No | Yes  
31 | zypp\_svn | ZYPP SVN Builds (openSUSE\_11.0) | Yes | Yes

There you have them all. You should also note that service configuration is very similar to repository configuration. Only services.d intead of repos.d.

## Conclusion

As you can see, services implement a kind of “black box” repository, that allows subscription to dynamic repository sets. The concept is simple, optional (you can not use them at all), and the potential is very big, if people start to offer them.

