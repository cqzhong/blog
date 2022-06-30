---
title: iOS 计算两点距离、点间角度、线间角度
urlname: snb1do
date: 2018-03-05 11:46:52 +0800
tags: [iOS]
categories: [移动端]
---

#### 1、如果枚举没有<<就不能组合使用

<!-- more -->

```objectivec
1 << n 代表:2的n次方:
//1 << 16 代表:2的16次方
 UIControlEventEditingDidBegin = 1 << 16,
//1 << 17 代表:2的17次方
 UIControlEventEditingChanged  = 1 << 17,
//1 << 18 代表:2的18次方
 UIControlEventEditingDidEnd  = 1 << 18,
//1 << 19 代表:2的19次方
 UIControlEventEditingDidEndOnExit  = 1 << 19,

 原来这样的枚举可以组合使用, 那苹果官方是怎么知道我们多个条件组合使用了呢 ?

 NSUInteger controlEvents = UIControlEventEditingDidBegin | UIControlEventValueChanged | UIControlEventEditingDidEnd;
    /**
    //通过 & 符号来判断是否包含:
    UIControlEventEditingDidBegin,
    UIControlEventValueChanged,
    UIControlEventEditingDidEnd
     */
    if (controlEvents & UIControlEventEditingDidBegin) {
        NSLog(@"UIControlEventEditingDidBegin");
    }else if (controlEvents & UIControlEventValueChanged) {
        NSLog(@"UIControlEventValueChanged");
    }else if (controlEvents & UIControlEventEditingDidEnd) {
        NSLog(@"UIControlEventEditingDidEnd");
    }
```

#### 2、iOS 计算两点距离、点间角度、线间角度

```objc
#include <math.h>

#define pi 3.14159265358979323846
#define degreesToRadian(x) (pi * x / 180.0)
#define radiansToDegrees(x) (180.0 * x / pi)
CGFloat distanceBetweenPoints (CGPoint first, CGPoint second) {
CGFloat deltaX = second.x - first.x;
CGFloat deltaY = second.y - first.y;
return sqrt(deltaX*deltaX + deltaY*deltaY );
};
CGFloat angleBetweenPoints(CGPoint first, CGPoint second) {
CGFloat height = second.y - first.y;
CGFloat width = first.x - second.x;
CGFloat rads = atan(height/width);
return radiansToDegrees(rads);
//degs = degrees(atan((top - bottom)/(right - left)))
}
CGFloat angleBetweenLines(CGPoint line1Start, CGPoint line1End, CGPoint line2Start, CGPoint line2End) {

CGFloat a = line1End.x - line1Start.x;
CGFloat b = line1End.y - line1Start.y;
CGFloat c = line2End.x - line2Start.x;
CGFloat d = line2End.y - line2Start.y;

CGFloat rads = acos(((a*c) + (b*d)) / ((sqrt(a*a + b*b)) * (sqrt(c*c + d*d))));

return radiansToDegrees(rads);

}
```
