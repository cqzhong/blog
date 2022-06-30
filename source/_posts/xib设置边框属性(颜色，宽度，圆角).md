---
title: xib设置边框属性(颜色，宽度，圆角)
urlname: qgvzcy
date: 2018-09-07 22:36:56 +0800
tags: [xib,iOS,Xcode]
categories: [移动端]
---

- storyboard 或者 Xib 给 View 设置边框属性(颜色，宽度，圆角)

<!-- more -->

![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602937133027-453adb17-42a9-4b72-b5f1-c7cadffa93b3.png#align=left&display=inline&height=718&margin=%5Bobject%20Object%5D&originHeight=718&originWidth=1134&size=0&status=done&style=none&width=1134)

- 写一个 CALayer 类别，用于给 xib 设置边框颜色

```objc

#import <QuartzCore/QuartzCore.h>
    #import <UIKit/UIKit.h>
    @interface CALayer (JKBorderColor)

@property(nonatomic, assign) UIColor \*jk_borderColor;
    @end



#import "CALayer+JKBorderColor.h"

@implementation CALayer (JKBorderColor)

-(void)setJk_borderColor:(UIColor \*)jk_borderColor{
    self.borderColor = jk_borderColor.CGColor;
    }

- (UIColor\*)jk_borderColor {
    return [UIColor colorWithCGColor:self.borderColor];
    }

@end
```
