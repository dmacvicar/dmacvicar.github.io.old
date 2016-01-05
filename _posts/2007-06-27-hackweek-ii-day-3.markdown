---
layout: post
title: Hackweek II (Day 3)
date: '2007-06-27'
tags:
- ruby
---

So today SCR (system configuration repository) works from Ruby. Also if you call Ruby from YCP and the Ruby module calls YaST, Ruby can't find the library. I fixed that.

```
require 'yast'

m = YaST::Module.new("SCR")
m.Execute(".target.bash", "firefox")
```

Will launch firefox

```
require 'yast'

m = YaST::Module.new("SCR")
modules = m.Read(".proc.modules")
modules.each do | k, v |
  puts "#{k}:"
  v.each do | a, b |
    puts " #{a} - #{b}"
  end
end
```

Will output:

```
de_cd:
    size - 40608
    used - 0
cdrom:
    size - 36896
    used - 2
ehci_hcd:
    size - 34956
    used - 0
... (more)
```

