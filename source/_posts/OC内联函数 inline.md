---
title: OC内联函数 inline
urlname: an9gmh
date: 2019-07-12 12:46:52 +0800
tags: [iOS,内联函数]
categories: [移动端]
---

在 iOS 开发过程中经常会使用 static inline 关键字组合

<!-- more -->

#### Static 关键字理解

##### Static 修饰局部变量

- 当 static 关键字修饰局部变量时，只会初始化一次且在程序中只有一份内存；
- 关键字 static 不可以改变局部变量的作用域，但可延长局部变量的生命周期（直到程序结束才销毁）。

##### Static 修饰全局变量

- 当 static 关键字修饰全局变量时，作用域仅限于当前文件，外部类是不可以访问到该全局变量的（即使在外部使用 extern 关键字也无法访问）。

#### inline 内联函数

```objc

static inline CGFloat
removeFloatMin(CGFloat floatValue) {
    return floatValue == CGFLOAT_MIN ? 0 : floatValue;
}


static inline CGFloat
flatSpecificScale(CGFloat floatValue, CGFloat scale) {
    floatValue = removeFloatMin(floatValue);
    scale = scale ?: ScreenScale;
    CGFloat flattedValue = ceil(floatValue * scale) / scale;
    return flattedValue;
}
```

虽然 static inline 修饰的是函数.但它在这里就是宏的作用,你可以将 removeFloatMin 当作一个宏.
当然 inline 函数与宏有区别,inline 可以:

- 解决函数调用效率的问题；
- 函数之间调用，是内存地址之间的调用，当函数调用完毕之后还会返回原来函数执行的地址。函数调用有时间开销，内联函数就是为了解决这一问题。
- 不用 inline 修饰的函数, 汇编时会出现 call 指令.调用 call 指令就是就需要： 1.将下一条指令的所在地址入栈 2.并将子程序的起始地址送入 PC（于是 CPU 的下一条指令就会转去执行子程序）.

**inline 优点**

- inline 函数避免了普通函数的,在汇编时必须调用 call 的缺点:取消了函数的参数压栈，减少了调用的开销,提高效率.所以执行速度确比一般函数的执行速度要快.
- 集成了宏的优点,使用时直接用代码替换(像宏一样); 1.避免了宏的缺点:需要预编译.因为 inline 内联函数也是函数,不需要预编译. 2.编译器在调用一个内联函数时，会首先检查它的参数的类型，保证调用正确。然后进行一系列的相关检查，就像对待任何一个真正的函数一样。这样就消除了它的隐患和局限性。 3.可以使用所在类的保护成员及私有成员。

#### inline 使用总结

- 内联函数只是我们向编译器提供的申请,编译器不一定采取 inline 形式调用函数.
- 内联函数不能承载大量的代码.如果内联函数的函数体过大,编译器会自动放弃内联.
- 内联函数内不允许使用循环语句或开关语句.
- 内联函数的定义须在调用之前.
