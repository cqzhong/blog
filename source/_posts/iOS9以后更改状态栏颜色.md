---
title: iOS9以后更改状态栏颜色
urlname: dgzoyl
date: 2019-03-19 12:58:06 +0800
tags: [iOS,StatusBarStyle]
categories: [移动端]
---

```objc
@property (nonatomic, assign) UIStatusBarStyle statusBarStyle;

- (void)viewWillAppear:(BOOL)animated {
    [super viewWillAppear:animated];

    self.statusBarStyle = UIStatusBarStyleDefault;
    [self setNeedsStatusBarAppearanceUpdate];
}
- (UIStatusBarStyle)preferredStatusBarStyle {
    return self.statusBarStyle;
}
```
