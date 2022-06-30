---
title: strong、weak、assign和copy
urlname: vw7zyu
date: 2018-01-05 09:20:00 +0800
tags: [iOS]
categories: [移动端]
---

说明并比较关键词：strong、weak、assign 和 copy

<!-- more -->

- strong 表示指向并拥有该对象。其修饰的对象引用计数会增加 1。该对象只要引用计数不为 0，就不会被销毁。当然，强行将其设为 nil 也可以销毁它。
- weak 表示指向但不拥有该对象。其修饰的对象引用计数不会增加。无需手动设置，该对象会自行在内存中被销毁。
- assign 主要用于修饰基本数据类型，如 NSInteger 和 CGFloat，这些数值主要存在栈中。
- weak 一般用来修饰对象，assign 一般用来修饰基本数据类型。原因是 assign 修饰的对象被释放后，指针的地址依然存在，造成“野指针”，在堆上容易造成崩溃。而栈上的内存系统会自动处理，不会造成“野指针”。
- copy 与 strong 类似。不同之处是，strong 的复制是多个指针指向同一个地址，而 copy 的复制是每次会在内存中复制一份对象，指针指向不同的地址。copy 一般用在修饰有对应可变类型的不可变对象上，如 NSString，NSArray，NSDictionary。

在 Objective-C 中，基本数据的默认你关键字是 atomic，readwrite 和 assign；普通属性的默认关键字是 atmoic，readwrite 和 strong。
