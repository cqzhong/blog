---
title: mPaaS 设置状态栏颜色
urlname: itipqf
date: 2020-12-14 00:00:00 +0800
tags: [mPaaS,iOS]
categories: [mPaaS]
---

#### 方法一

- 1、修改 `info.plist` 中 `View controller-based status bar appearance` 设置为 `NO`
- 2、在需要修改状态栏颜色的页面添加以下代码

<!-- more -->

```objc
#pragma clang diagnostic push
#pragma clang diagnostic ignored "-Wdeprecated-declarations"
    [[UIApplication sharedApplication] setStatusBarStyle:UIStatusBarStyleLightContent];
#pragma clang diagnostic pop
```

#### 方法二

- 1、修改 `info.plist` 中 `View controller-based status bar appearance` 设置为 `YES`
- 2、在需要修改状态栏颜色的页面添加以下代码

```objc
@property (nonatomic, assign) UIStatusBarStyle statusBarStyle;

self.statusBarStyle = UIStatusBarStyleLightContent;
    [self setNeedsStatusBarAppearanceUpdate];

- (UIStatusBarStyle)preferredStatusBarStyle {
    return self.statusBarStyle;
}
```
