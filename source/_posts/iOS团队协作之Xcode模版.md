---
title: iOS团队协作之Xcode模版
urlname: sdu8nd
date: 2018-08-06 08:56:59 +0800
tags: [iOS,Xcode]
categories: [移动端]
---

当我们在 Xcode 里新建文件时，Xcode 默认提供了多种文件模板，并且不同的 class，模板里默认提供的内容也不一样。

<!-- more -->

#### 为什么要自定义 Xcode 模版

在团队开发过程中， 我们要遵守一定的代码规范，例如控制器中需要用#pragma mark -来分割各个方法

```objc

#pragma mark - Life Cycle Methods

#pragma mark - Intial Methods

#pragma mark - Network Methods

#pragma mark - Target Methods

#pragma mark - Public Methods

#pragma mark - Private Method

#pragma mark - External Delegate

#pragma mark - Setter Getter Methods
```

但是如果每次创建类，都要一个一个的去添加这些代码，就会浪费很多时间。所以这个时候我们就要用到自定义 Xcode 模版。使用 Xcode 模版对于一个团队而言，能加快不少效率。

#### 自定义 Xcode 模版

- 打开 Xcode 模板存放路径：

```bash
open /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/Templates/File\ Templates/Source
```

参考 Cocoa Touch Class.xctemplate 内的 Xcode 模版

- 创建文件夹：CD Class.xctemplate， 仿照 Cocoa Touch Class.xctemplate 内的 TemplateInfo 创建 TemplateInfo.plist 文件，理解 plist 文件字段的含义，尤其注意 options 字段， 创建有 xib 和没有 xib 的基类。
- 模版是利用几个系统的预处理宏定义，包括

```objc
___FILEBASENAMEASIDENTIFIER___ ：类名

___VARIABLE_cocoaTouchSubclass___：基类名

___FILENAME___：文件名

___PROJECTNAME___：工程名

___FULLUSERNAME___：用户名

___DATE___：当前日期

___COPYRIGHT___：版权声明
```

- 把自定义好的 Xcode 模版 CD Class.xctemplate，放到 Xcode 模版路径下，无需重启 Xcode，直接新建文件，即可看到“CD Class”的模板可供选择，如下图

![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602936698225-345d30b3-207e-41a1-95a0-fb2cd93f3417.png#height=938&id=OOKLF&originHeight=938&originWidth=1418&originalType=binary∶=1&size=0&status=done&style=none&width=1418)
