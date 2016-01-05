---
layout: post
title: GPLv3 draft. C++ CRTP
date: '2007-03-29'
tags:
- open-source
- patents
- software
---

A new discussion draft of the GNU GPL has been released. [Go here to see more][1]. Note the end of the Patents section in the changes guide:

> specifically granted to recipients of the covered work under this License[, unless you entered into that arrangement, or that patent license was granted, prior to March 28, 2007].

I heard for first time about the [Curiously Recurring Template Pattern][3] when I read the [Eigen library][2] webpage. Now I found [this example][4] by Bruce Eckel that makes it much clearer:

//: generics/Mixins.cpp  
  
#include  
  
#include  
  
#include  
  
using namespace std;

template\<class T\> class TimeStamped : public T {  
  
&nbsp;&nbsp;long timeStamp;  
  
public:  
  
&nbsp;&nbsp;TimeStamped() { timeStamp = time(0); }  
  
&nbsp;&nbsp;long getStamp() { return timeStamp; }  
  
};

template\<class T\> class SerialNumbered : public T {  
  
&nbsp;&nbsp;long serialNumber;  
  
&nbsp;&nbsp;static long counter;  
  
public:  
  
&nbsp;&nbsp;SerialNumbered() { serialNumber = counter++; }  
  
&nbsp;&nbsp;long getSerialNumber() { return serialNumber; }  
  
};

// Define and initialize the static storage:  
  
template\<class T\> long SerialNumbered\<T\>::counter = 1;

int main() {  
  
&nbsp;&nbsp;TimeStamped\<SerialNumbered\<string\> \> mixin1, mixin2;  
  
&nbsp;&nbsp;mixin1.append("test string 1"); // A string method  
  
&nbsp;&nbsp;mixin2.append("test string 2");  
  
&nbsp;&nbsp;cout \<\< mixin1 \<\< " " \<\< mixin1.getStamp() \<\< " " \<\<  
  
&nbsp;&nbsp;&nbsp;&nbsp;mixin1.getSerialNumber() \<\< endl;  
  
&nbsp;&nbsp;cout \<\< mixin2 \<\< " " \<\< mixin2.getStamp() \<\< " " \<\<  
  
&nbsp;&nbsp;&nbsp;&nbsp;mixin2.getSerialNumber() \<\< endl;  
  
}

The effect you get is basically what you get with [Ruby Mixins][5].

[1]: http://gplv3.fsf.org/gpl3-dd3-guide  
 [2]: http://bjacob.livejournal.com/tag/kde+eigen  
 [3]: http://en.wikipedia.org/wiki/Curiously_Recurring_Template_Pattern  
 [4]: http://www.artima.com/weblogs/viewpost.jsp?thread=132988  
 [5]: http://www.juixe.com/techknow/index.php/2006/06/15/mixins-in-ruby/

