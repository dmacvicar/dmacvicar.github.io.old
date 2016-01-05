---
layout: post
title: yast2-ruby-bindings user interface support
date: '2007-09-19'
tags:
- newkde
- ruby
---

I just commited support for using the user interface (yes, that is Qt, GTK or ncurses) from the ruby language:

```
require 'yast'
ui = YaST::Module.new("UI")
YaST::Ui::init("qt")
include YaST::Ui

t = HBox( Label("Welcome to Ruby!"), PushButton("Push me") )

puts "#{t.to_s} #{t.class}"

ui.OpenDialog(t)
ui.UserInput()
```

Result:

 ![]({{ site.baseurl }}/assets/images/posts/media_httpimg215image_hzgic-scaled500.png)
