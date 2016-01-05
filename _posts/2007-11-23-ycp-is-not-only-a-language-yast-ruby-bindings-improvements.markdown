---
layout: post
title: ycp is not only a language. YaST ruby bindings improvements
date: '2007-11-23'
tags:
- open-source
- ruby
---

After quite lot of hacking nights, I managed to get some yast2 ruby bindings improvements working.

You might know that ycp is not only a language, but a component technology. Every component using the YaST2 technology can be reused from other YaST components no matter what language they are written in, as long as there are bindings for that. That was the original intention of the ruby bindings.

So for example, there is a component call Storage, which is written in the ycp language. The component (well, it is really a namespace, but YaST sees it as a namespace component) is located in /usr/share/YaST2/modules/Storage.ycp (.ybc for the bytecode compiled version) and offers some medium level functions that YaST uses.

This component needs low level access to the system, for which there is another module called Libstorage ( located in /usr/share/YaST2/modules/LibStorage.pm ) which in this case is written in perl which is a language YaST supports as long as you follows some simple rules.

The reason is because libstorage is a C++ library, and there is no easy way to bind it to the YaST environment, but as YaST supports perl, you can generate perl bindings for the C++ library and all the library functionality will be available to other components in YaST.

So repeat after me. ycp is not only a language. It is a communication protocol, used by YaST component framework.

So, with the ruby bindings, you where able to do:

```
require 'yast'

m = YaST::Module.new("Storage")
dp = m.GetDiskPartition("/dev/sda1")
dp.each do | key, value |
  puts "#{key} #{value}"
end
```

But I was never happy with the syntax. The idea was that you construct a dynamic proxy for a YaST component and then call methods on it. This had some disadvantages:

* If the component did not exist, you did not get an error till the method call  
* It was not possible to do introspection to the component, because the methods call where implemented using the method\_missing hook. So basically the proxy responded to all methods and only raised an error if the equivalent call to the YaST side was not possible.

So I decided to look for a new approach. This is the result till now:

```
require 'yast'
require 'ycp/storage'

dp = YCP::Storage::GetDiskPartition("/dev/sda1")
dp.each do | key, value |
  puts "#{key} #{value}"
end
```

Oh yes, you notice. First, there is no need to instantiate the module, a classic require will do it. The bindings modify the built-in require method, so if you require something under ycp/, a module is created on the fly. A real module, a real symbol. You can also import it manually using YCP::import

Now, you can do this:

```
puts YCP::Storage.methods
```

When the module is imported, the symbols are declared in the module, so you can ask for them in the module. I still need to work on other symbol types, but it should be straight forward.

The last improvement is that I simplified the C part of the bindings a lot, and moved most of the magic to pure ruby code, which makes easier to work with.

Next improvement, the other way around, making nicer how to call ruby components from any other component.

