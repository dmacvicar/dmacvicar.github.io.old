---
layout: post
title: 'A better Java: Scala or Xtend?'
date: '2012-09-30'
categories:
- Software
tags:
- android
- eclipse
- java
- scala
- xtend
comments: true
---

I have been playing with two languages recently: [Scala](http://www.scala-lang.org/) and [Xtend](http://www.eclipse.org/xtend/).

Xtend and Scala have some similarities, but don't let this make you think they are that similar. Both are JVM based languages offering a refresh over Java, but the itch they focus on is different and the culture behind them is even more different.

Xtend focuses on fixing Java so that it is good enough to do the stuff you are already doing with Java without so much pain. It is like bringing Java to the state where C# is, which is something Sun and Oracle haven't been able to do. Xtend compiles to Java source code, not bytecode. If you use it on eclipse, you will see a xtend-gen folder with the generated code, which is then in turn compiled to bytecode. Everything is transparent to the developer. Xtend is built on top of [Xtext, a framework to write Domain Specific Languages](http://www.eclipse.org/Xtext/) and get Java/IDE support for free (example: a [Cucumber-like DSL](http://www.jnario.org/))

Scala is also a "better Java", but it focuses more on providing a "Scalable Language". A language that can be used on different paradigms or as Domain Specific Languages (eg. the [Play! web framework](http://www.playframework.org/) features type-safe HTML templates thanks to Scala). It mixes heavily the object oriented paradigm with functional programming. The functional aspect of Scala aims to provide support to designing concurrent programs easily. This goes beyond simple lambdas and the philosophy aims of immutable objects and expressions over statements, immutable collections and actors in the library.

For developers just looking for a refresh, both provide:

### Optional Semicolons

Yes. No need to write them in most cases.

### val and var

In Scala:

```scala
val String someVariable = "This will not change"
var String someVariable = "This can change later"
```

While "val" is not different from "final", I really like this syntax. In scala it is heavily used to make your brain always think if you really need  
to change a variable later. You soon realize that you don't, and most variables are calculations that need to be initialized once.

### Type inference

In the example above, you don't need to specify the types. They will be infered:

```scala
val lambda = [String s | s.length]
```

In Scala

```scala
final JTextField textField = new JTextField();
textField.addActionListener(new ActionListener() {
  @Override
  public void actionPerformed(ActionEvent e) {
    textField.setText("Something happened!");
  }
});
```

Can be described using a lambda in Xtend like:

```scala
val textField = new JTextField
textField.addActionListener [
  textField.text = "Something happened!"
]
```

Lambdas are closures, so they take variables from the current scope. Lambdas are not only useful for callbacks. I do miss ruby's each/map/collect in Java:

```scala
val charCount = strings.map[s|s.length].reduce[sum, size | sum + size]
```

### Extending libraries

Xtend has a really cool feature called extensions.

You can find about it in the guide.

Scala on the other hand has something called implicit convertions. They can be used with a pattern called ["pimp-my-library"](https://wiki.scala-lang.org/display/SYGN/Pimp-my-library) to extend existing APIs:

For example, to add a method headOr to the List class, one first create a "wrapper" class with the method:

```scala
implicit def listExtensions[A](xs : List[A]) = new ListExtensions(xs)
```

And then this should work:

```scala
implicit def function2ViewOnClickListener(f: View => Unit) : View.OnClickListener = {
   new View.OnClickListener() {
     def onClick(view: View) {
       f(view)
     }
   }
}
```

Then this works:

There is more stuff in both languages I am not going to spend time on, but you can go to the respective documentation.

For Xtend go to [the documentation](http://www.eclipse.org/xtend/documentation.html) or this [document called "20 Facts about Xtend"](http://jnario.org/org/jnario/jnario/documentation/20FactsAboutXtendSpec.html).

For Scala. I bought ["the book"](http://www.amazon.com/dp/0981531644/ref=cm_sw_r_tw_dp_-xHzqb03X9HX8). However you can also find more in [the documentation site](http://docs.scala-lang.org/).

### IDE support

I first tried Scala a year ago (can't remember) and the [IDE](http://scala-ide.org/) support was so bad that I did not go further. This is no longer true. There has been quite a lot of investment in it lately and it is good enough already.

Xtend is part of Eclipse now. Therefore you can expect good IDE support. I found some glitches and weird messages, but in general it works fine.

### Android

I tried both languages on Android.

Scala worked fine, but you have to use ProGuard to reduce the size of the application by removing unused methods and classes.

[This guide](http://blog.andresteingress.com/2011/09/20/programming-android-with-scala/) was a good start. However I hit weird errors with the Treeshaker Proguard plugin I was using. [This Stackoverflow answer](http://stackoverflow.com/questions/9924015/eclipse-android-scala-made-easy-but-still-does-not-work/11084146#11084146) put me back on the right track with the right ProGuard plugin.

I don't feel confortable with Scala on Android. Without an external tool to trim the jar the generated code goes over the limit of methods that can be handled, even when developing. Not only that. The scala runtime library is 8.7M:

```java
setContentView(R$layout::activity_main)
```

This is a syntax issue. It is documented. But it was just too unexpected for me.

There is another issue you should know about. No debugging. The Dalvik VM does not support JSR-45 which is why debugging Xtend (and other Xbase languages) [doesn't work](https://bugs.eclipse.org/bugs/show_bug.cgi?id=386725).

Once I accepted not being able to debug. I ran my program:

```text
zip -d /space/sw/eclipse/plugins/org.eclipse.xtend.lib_2.4.0.v201208210511.jar about.html
deleting: about.html
zip -d /space/sw/eclipse/plugins/org.eclipse.xtext.xbase.lib_2.4.0.v201208210511.jar about.html
```

Run again:

Also, a good tip is to change the "xtend-gen" folder in the Xtend settings to use the "gen" folder Android already uses to dump generated files from resources and others.

After that, everything worked fine and I got my application running on Android. The size of the Xtend and Xbase libraries is 7K and 90K. guava is the heaviest dependency with 1.2M. But nothing compared to Scala. My application was 2M installed without proguard processing (which happens by default in release mode).

### Criticism

#### Scala

In my opinion, Scala is much more mature. The syntax is well thought. I was annoyed for using [] for generics and () to index arrays until I saw the explanation in the book. I am enjoying reading the book and learning the "Scala way".

Scala has a unified type system. Unlike Java, everything is an object. You can write:

read this post. Or you look at [scala.collections.immutable.List](http://www.scala-lang.org/api/current/scala/collection/immutable/List.html) to look for more information about the type:

I know there is a reason for this. I know at some point a page in the Scala book will explain it. But the step curve is not easy. I was writing very basic code and I was asking myself how to do lot of stuff. The biggest problem in my opinion is that Scala, being "Scalable", allows to write the code in various ways. One is the Scala way. The other is not. Sometimes one is the intuitive one. Sometimes the other is to slow.

An example of this is the [Quick Pimp Library Pattern](http://www.decodified.com/scala/2010/12/02/the-quickpimp-pattern.html). This allows you to write the explicit conversion easier by not having to create a "wrapper" type.

see the link) this is slow. There was also some speed issue with "for" loops if you write the "for" in the wrong way (I forgot the details).

This gives me a feeling of the Scala culture very similar to what some subworld of C++ doing heavy template meta-programming, policy based design gives to me. I like boost. But I have to be very awake to decode the signatures and understand how to use a library. The design and the quality is top-notch, but it is an advanced device. You need to invest quite a bunch of time on it and make sure those features will actually pay off. I am not against a language that requires some CS background and a type system that requires you to think a bit. But if you are going to master it, you have to be aware of the cost/benefits.

#### Xtend

Xtend was a pleasure to write and read. I got used to it very fast. The documentation is a single page guide. The type system is the same as Java.

However, Xtend is not fully mature yet (despite version being 2.x). I was playing with the [JFugue library](http://www.jfugue.org/) and I tried to do this:

does not have character literals yet. The solution:

```scala
button.onClickListener = [
  this.textView.text = "Foo"
]
```

But it did not compile! The simplest example did not work. Disclaimer: I was not using the released version. But I was not using the nightly builds either. I was using the milestones. I got a "Incompatible types" error. I updated the eclipse plugin to the current milestone, restarted Eclipse, and everything was working. (Facepalm).

### Conclusions

Both are great languages. If you are doing servers and services, I'd pick Scala. But be prepared to invest some time learning the culture behind it.  
Learning Scala gives you the most from the [Play! Framework](http://www.playframework.org/). It can also be used from Java, but it is not the same.

If you are only looking for a Java refresh. Go with Xtend. You will learn it in one night and will benefit from the value it provides. Once the quirks with Android get resolved (especially the debugger), it is a great addition to Android development. Being an Eclipse project means the IDE will be a first citizen and releases will be stable enough to ship with Eclipse itself.

