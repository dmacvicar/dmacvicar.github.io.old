---
layout: post
title: Be aware of the Garbage Collector when accessing C/C++ from Ruby
date: '2012-07-31'
categories:
- Software
tags:
- libyui
- ruby
- suse
comments: true
---

When you wrap C/C++ code into a language like Ruby which has a garbage collector, you have to be very careful because the GC knows about the Ruby objects referencing to each other, but not about the underlying C objects. You need to manually hint the GC that two Ruby objects are connected by their underlying pointers so that the GC does not deallocate one, freeing all the pointer chain and leaving another Ruby object with a dangling pointer.

As an example: You have a Ruby r1 object that points to a 'Widget' '*w1', and r2 points to 'Widget' '*w2'. The parent of w2 is w1, and if you delete the w1 pointer all children will be deleted.

In Ruby's mark & sweep GC this is achieved by implementing the 'mark()' hook correctly and calling 'rb_gc_mark()' from there. For the example above it would be something like:

```c++
void
ui_widget_mark(YWidget *wg)
{
  // ...
  // mark our child _ruby_ objects by finding the C
  // ptrs and looking the ruby counterparts in the hash
  for (YWidgetListConstIterator it = wg->childrenBegin();
       it != wg->childrenEnd();
       ++it) {
    YWidget *child = *it;
    VALUE rb_child = widget_object_map_for(child);
    if (!NIL_P(rb_child))
      rb_gc_mark(rb_child);
  }
}
```

This may be sometimes tricky, and you may need to keep some extra metadata in the binding's code in order to figure the dependencies between objects.

For example libyui keeps a dialog stack, but does not provide access to it. You can't delete the dialogs in a different order than the stack one. However if you have Ruby references to multiple dialogs, the GC may kick starting by any dialog. You have to tell the GC to mark all parent dialogs when marking a given dialog corresponding Ruby object. As I don't have access to the stack I have to keep my own stack (implemented with a list) in order to implement 'mark()' correctly.