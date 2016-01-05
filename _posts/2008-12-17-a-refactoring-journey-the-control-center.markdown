---
layout: post
title: 'A refactoring journey: the control center'
date: '2008-12-17'
tags:
- newkde
- newsuse
- qt
- software
- suse
- yast
---

It is almost Christmas and things are getting more quiet and there is now some time to look at pending TODOs. I mean all those things that did not make into 11.1 because priorities.

## The problem

The Qt control center is an ancient piece of code, and it still is a Qt 3.x application.

You may know how it looks:

![original][12]

We already have a [feature for a reorganization of the control center][7]. [Thomas Göttlicher][15], [Jens Daniel Schmidt][16], [Jörg Kress][14] and [Martin Schmidkunz][16] [worked last year on proposals][8]. I want to leave look and feel out of this post for now. (When it comes to which color to use to paint the walls, everyone has an opinion). I wanted to focus on:

* [The fact that it still is a Qt 3.x][9] application  
* The code is not easy to port to Qt 4.x, even more difficult to change the look and feel (which would help for [the feature mentioned above][7] )

The main reason of why the code became so inflexible is the mixing of presentation with data items everywhere. Yes, not only the control center suffers from this problem but also the Qt package selector.

Qt 4.x has, among a lot of unmatched features, the ability to use a [Model/View paradigm][18]. This means, you describe a model, and then you just plug it into a view, and it will be displayed according to the widget's context, instead of sub classing widgets just to hold data on the user interface.

I have to admit that it is not the first time I look into Qt's Model/View, but in order to understand it, one needs to dig in some real life problem because it has some concepts like indexes, views, delegates, etc, which start to make sense once one adapts it to the specific problem.

So, my challenge was to add a model architecture to the control center in order to make then trivial to let other people choose the wall color ;-) and also enable other innovations. For example [Stano][19] hacked a ycp script to retrieve summary information from the configuration (I can't wait to integrate that).

## Step 0. Find out where are we.

Thomas Göttlicher already did [some research on using the KDE4 system-settings style views][11]. This shown that some widgets could be reused (like KCategorizedView), but also showed that pieces like the SystemSettingsModel are too complex for what we need.

## Step 1. Compile old control center on Qt 4.x

I took the old control center, [branched it][10] and started the boring work to getting it to compile with Qt 4.x. This involved setting cmake up as I did not wanted to waste time figuring how to convert an autotools project to Qt 4.x.

In this stage one sees much design decisions that either help or hurt when porting. Also opportunities, for example the control center had its own .ini file parser, which I replaced with the QSettings class. Note, QSettings is there since Qt 3.3.

At the same time I got it compiling in Qt 4.x, I extracted some of the module listing code to an QAbstractItemModel and created a small test program for it that I could run in parallel.

My model could be plugged into a view and display the groups, with one line of code:

![groups model][1]

After getting familiar with the code, and learning it, I decided it was almost impossible to plug my model in the current code. Why did I tried then? Because [reading code is always harder than writing it, therefore there is always a tendency to rewrite from scratch][13], and I wanted to be really sure it was not the "reading code effect".

## Step 2: start with a cleaner base

I took [Thomas codebase][11] and compiled it. I realized it was easier. I only needed to get rid of the systemsettings code. I only needed the KCategorizedView part, or even a plain QListView if I had the right model.

Once the code was removed, I needed a modules model (analog to the groups model). After writting some code, I felt like I was writing the same thing again: a model that lists what it is in .desktop files. Closed both models and focused on a DesktopFilesModel as a base. Once this was done, the groups and modules model was a few lines of code (mainly setting the right configure time YaST paths). So I had a modules model too:

![groups model][2]

## Step 3: KCategorizedView

This component worked out of the box when I plugged my model:

![one category][3]

Except that it only displayed a hardcoded category. One needs to enhance the model to reply to the CategoryDisplayRole, which required to start thinking better about the structure of the models and add utility methods to query groups for modules given an index and similar methods.

At the same time once you see it on the screen you get a feeling when the view is querying the model (speed and debug lines), which resulted in a basic cache infrastructure for the base .desktop files model.

The result:

![categories][4]

## Step 4: Profit from the new structure

At this point I realized there were too many items on the screen, but also I realized how trivial was to add a docking at the right with the group list, or how trivial it would be to set the proxy model to filter certain groups. Just for playing, I added a search bar:

![search][5]

and then a groups list dock (which you can move around the main window):

![group list][6].

## Now what?

Note that none of these screenshots mean "this is how the control center will look". The focus is still on the infrastructure that enables us to try more radical approaches. For example, the old control center look and feel is trivial to emulate.

Will it be the old? the new? configurable? We don't know yet. There is still work to do before we focus on the look and feel.

[1]: http://www.suse.de/~dmacvicar/screenshots/control-center-qt4/1-modulegroupsmodel.png  
 [2]: http://www.suse.de/~dmacvicar/screenshots/control-center-qt4/2-modulesmodel.png  
 [3]: http://www.suse.de/~dmacvicar/screenshots/control-center-qt4/3-onecategory.png  
 [4]: http://www.suse.de/~dmacvicar/screenshots/control-center-qt4/4-categories.png  
 [5]: http://www.suse.de/~dmacvicar/screenshots/control-center-qt4/5-searchbar.png  
 [6]: http://www.suse.de/~dmacvicar/screenshots/control-center-qt4/6-dockgroups.png  
 [7]: https://features.opensuse.org/?rm=feature_show&id=302306  
 [8]: http://en.opensuse.org/YaST/Development/New_Control_Center/  
 [9]: https://features.opensuse.org/?rm=feature_show&id=305550  
 [10]: http://svn.opensuse.org/svn/yast/branches/tmp/dmacvicar/control-center-qt4/  
 [11]: http://svn.opensuse.org/svn/yast/branches/tmp/tgoettlicher/yast2cc_rewrite/  
 [12]: http://www.suse.de/~dmacvicar/screenshots/control-center-qt4/0-original.png  
 [13]: http://www.joelonsoftware.com/articles/fog0000000069.html  
 [14]: http://joshs.littlecornerofthe.net/blog/  
 [15]: http://en.opensuse.org/User:Tgoettlicher  
 [16]: http://en.opensuse.org/User:Jdsn  
 [17]: http://en.opensuse.org/User:Mschmidkunz  
 [18]: http://doc.trolltech.com/4.4/model-view-programming.html  
 [19]: http://en.opensuse.org/User:Visnov

