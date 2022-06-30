---
title: iOS团队协作之Xcode模版、代码片段(新)
urlname: vrvv9o
date: 2021-07-23 21:00:00 +0800
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
open /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/Templates/File\ Templates/iOS/Source
```

参考 Cocoa Touch Class.xctemplate 内的 Xcode 模版

- 创建文件夹：PG Class.xctemplate， 仿照 Cocoa Touch Class.xctemplate 内的 TemplateInfo 创建 TemplateInfo.plist 文件，理解 plist 文件字段的含义，尤其注意 options 字段， 创建有 xib 和没有 xib 的基类。
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

- 把自定义好的 Xcode 模版 PG Class.xctemplate，放到 Xcode 模版路径下，无需重启 Xcode，直接新建文件，即可看到“PG Class”的模板可供选择，如下图

![xctemplate4.png](https://cdn.nlark.com/yuque/0/2021/png/1028501/1627011066334-2c7cc9a2-fe68-42db-9e65-810a05bd0f94.png#clientId=u6e343f58-ef5a-4&from=drop&id=u0f414f48&margin=%5Bobject%20Object%5D&name=xctemplate4.png&originHeight=1042&originWidth=1460&originalType=binary∶=1&size=178846&status=done&style=none&taskId=ub5618670-a53a-4022-a4ad-e42e650b4a4)

#### Xcode 模版项目 [PG_Xcode_Templates](https://gitee.com/cqzhong/pg_-xcode_-templates)

​

进入 Xcode 模板存放路径：

```bash
cd /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/Templates/File\ Templates/iOS/Source
```

将项目拉取下来，并命名为 `PG Class.xctemplate`（文件夹名自由定义，后缀名不可变），注意这个目录是系统目录，需要 root 权限才能修改，所以所有 git 命令都需要加 `sudo`。

```bash
sudo git clone https://gitee.com/cqzhong/pg_-xcode_-templates.git PG\ Class.xctemplate
```

无需重启 Xcode，直接新建文件，即可看到“PG Class”的模板可供选择。
​

#### Xcode 代码片段项目 [CodeSnippets](https://gitee.com/cqzhong/code-snippets)

Xcode 的 Code Snippets 文件存放于 `~/Library/Developer/Xcode/UserData/CodeSnippets` 目录，只要直接把 `*.codesnippets` 文件放到这个目录下（若没有则自己创建），重启 Xcode 即可生效。

为了方便更新，建议直接将 iOS CodeSnippets clone 到这个目录内（注意，不是在 CodeSnippets 里创建一个目录，这里不支持子目录）：

```bash
cd ~/Library/Developer/Xcode/UserData
#
git clone https://gitee.com/cqzhong/code-snippets.git CodeSnippets
```
