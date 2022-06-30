---
title: xib适配不同屏幕（等比缩放）
urlname: ts13hy
date: 2018-12-07 13:36:56 +0800
tags: [xib,iOS,iOS适配,Xcode]
categories: [移动端]
---

基本所有的 UI 效果图都是以 iPhone6 屏幕为基准来设计的，在 4.7 的屏幕上没有问题，但是到其它屏幕上，就不是 UI 图上的效果了。各个控件的上、左、下、右、宽、高约束都不是想要的比例了。这个时候就要从 xib 拉出 NSLayoutConstraint 属性来动态设置数值了。
怎样才能达到按照一个屏幕尺寸去做开发，使其它所有屏幕都是自动等比缩放呢？

<!-- more -->

**这里主要提供一种利用 xib 做开发，做到适配不同屏幕的方法。**

- 关键字 IBInspectable 出现新的可编辑属性 和 User Defined Runtime Attributes

新建一个 NSLayoutConstraint 的类别

NSLayoutConstraint.h

```objc

#import <UIKit/UIKit.h>

NS_ASSUME_NONNULL_BEGIN

@interface NSLayoutConstraint (ItemEdge)

// IBInspectable 出现新的可编辑属性 和 User Defined Runtime Attributes
@property (nonatomic, assign) IBInspectable BOOL adapterScreen;

@end

NS_ASSUME_NONNULL_END
```

NSLayoutConstraint.m

```objc

#import "NSLayoutConstraint+ItemEdge.h"

@implementation NSLayoutConstraint (ItemEdge)

@dynamic adapterScreen;

static char associateLengthKey;

- (BOOL)adapterScreen {

    return [(NSNumber *)objc_getAssociatedObject(self, &associateLengthKey) boolValue];
}
- (void)setAdapterScreen:(BOOL)adapterScreen {
    objc_setAssociatedObject(self, &associateLengthKey, @(adapterScreen), OBJC_ASSOCIATION_RETAIN_NONATOMIC);
    if (adapterScreen) {

        switch (self.firstAttribute) {

            case NSLayoutAttributeTop: {

                self.constant = CDREALVALUE_HEIGHT(self.constant);

                if (CD_IS_IPHONE_X && [self.identifier isEqualToString:@"T"]) {
                    self.constant += 24;
                }
                break;
            }
            case NSLayoutAttributeBottom: {

                self.constant = CDREALVALUE_HEIGHT(self.constant);
                if (CD_IS_IPHONE_X && [self.identifier isEqualToString:@"B"]) {
                    self.constant += 34;
                }
                break;
            }
            case NSLayoutAttributeHeight: {

                self.constant = CDREALVALUE_HEIGHT(self.constant);

                break;
            }
            case NSLayoutAttributeLeft:
            case NSLayoutAttributeRight:
            case NSLayoutAttributeTrailing:
            case NSLayoutAttributeLeading:
            case NSLayoutAttributeWidth: {

                self.constant = CDREALVALUE_WIDTH(self.constant);
                break;
            }
            default:
            break;
        }
    }
}

@end
```

#### 上面的代码牵扯到一些宏定义

1. CD_IS_IPHONE_X ：判断机型是否使带刘海的手机。
1. CDREALVALUE_HEIGHT() 根据基准机型等比缩放约束的高度。
1. CDREALVALUE_WIDTH() 根据基准机型等比缩放约束的宽度。

#### 怎样在 xib 内使用

- 约束完成后点中 NSLayoutConstraint 约束
- 将 Adapter Screen 属性改为 on，这样就会进入到 setAdapterScreen 方法内，对每个约束进行你要要的缩放。

![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602937059903-d8860b81-dcc3-4801-bf46-93c5f7c2f0ea.png#align=left&display=inline&height=577&margin=%5Bobject%20Object%5D&originHeight=577&originWidth=400&size=0&status=done&style=none&width=400)
