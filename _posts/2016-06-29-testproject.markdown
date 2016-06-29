---
layout: post
title: "ios在添加单元测试工程，release模式link error"
date: "2016-06-29 11:47:38 +0800"
---
最近在尝试使用ios工程自带的Test进行底层代码的单元测试，遇到个很奇怪的问题，就是在debug模式下运行主工程正常运行，但是到release模式下就会报错
如下：

{%highlight  java%}
Undefined symbols for architecture i386:
  "_OBJC_CLASS_$_SomeClassUnderTest", referenced from:
      objc-class-ref in SomeTest.o
{%endhighlight%}

真机的时候便是

{%highlight  java%}
Undefined symbols for architecture arm64:
  "_OBJC_CLASS_$_SomeClassUnderTest", referenced from:
      objc-class-ref in SomeTest.o
{%endhighlight%}

一开始是以为我使用的是pod工程没有link到某些pod类库中的代码导致，可是后来将test工程添加了pod库，依旧编译报错

想想问题的根本是debug下可以运行、但是release下运行了，那就是工程在debug跟release之间有区别

而test工程是依赖于主工程的，体现在test工程general-》Testing-》Host Application

所以将主工程的debug、release存在差异的地方查看一遍，发现在Symbols Hidden by Default设置项
两者存在差异，索性都设置为NO

再次在release模式下运行工程，编译通过

但是有个奇怪的问题就是为什么我编译的是主工程，却引来了test工程的错误呢，这个是在那个地方触发的还没有查明原因
