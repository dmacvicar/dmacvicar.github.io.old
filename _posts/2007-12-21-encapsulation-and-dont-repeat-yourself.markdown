---
layout: post
title: Encapsulation and don't repeat yourself
date: '2007-12-21'
tags:
- ruby
- software
---

Richard Moore [comment][1] on [Access Anxiety][2] explains why Encapsulation is a good thing. I think both are missing the point. Nobody doubts that encapsulation is a good thing, and Michael Feathers tries to present ruby way as a more "relaxed" thing.

> The author was comparing the 'ruby style' of direct access to member variables with the getter/setter pattern common in Java code

So what is the ruby style really?

The annoying thing about java setters is that they are functions. So you either start with them (which is overkill for simple things) or don't use them and then you are lost when you want to add things like Richard showed.

Ruby on the other hand, makes it easy, because you can't expose member variables, but you can very easily create default accessors.

Michael Feathers example is wrong. Usually you start by actually offering a simple default accessor.

```
class GlazeObject
  attr_accessor :store, :formatter

  def initialize
    @store = DefaultStore.new
    @formatter = DefaultFormatter.new
  end
end
```

Now, if you need something more special, like Richard example:

```
public void setFormatter(Formatter formatter) {
        this.formatter = formatter;
        this.expensive = doCalculation(formatter);
    }
```

You can just remove attr\_accessor :formatter and add to the class:

```
class GlazeObject
  def formatter
    return @formatter
  end

  def formatter=(f)
    @formatter = f;
     @expensive = doCalculation(f);
  end
end
```

What is the difference? In ruby, for client code consuming this class, you always see obj.formatter for both cases. In java you have to start from the beginning copy-pasting repetitive accessor code if you don't want to start changing obj.formatter to obj.getFormatter() and obj.formater = something to obj.setFormatter(something).

C# has a similar feature, called properties.

```
public class Person
{
  private int _age;
  public int age
  {
   get
   {
     return _age;
   }
   set
   {
     _age = value;
   }
  }
}
```

No matter if we start with a public variable called age, we can move it to setters and getters (which are the providers of encapsulation without having to change the operation mode from variables to functions.

[1]: http://www.kdedevelopers.org/node/3163  
 [2]: http://beautifulcode.oreillynet.com/2007/12/access_anxiety.php

