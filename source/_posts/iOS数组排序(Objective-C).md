---
title: iOS数组排序(Objective-C)
urlname: sl9cqs
date: 2018-10-14 12:58:06 +0800
tags: [数组排序,iOS]
categories: [移动端]
---

#### iOS 数组排序的几种方法

<!-- more -->

```javascript
1）
self.products = [myProducts sortedArrayUsingComparator:^NSComparisonResult(id  _Nonnull obj1, id  _Nonnull obj2) {
  SKProduct *pro1 = (SKProduct *)obj1;
  SKProduct *pro2 = (SKProduct *)obj2;
  return pro1.price.integerValue < pro2.price.integerValue ? NSOrderedAscending : NSOrderedDescending;
}];

（2）比较大小排序  从小到大排序
  NSArray *sortedArray = [keyArr sortedArrayUsingComparator:^NSComparisonResult(NSString *obj1, NSString *obj2) {
        if ([obj1 intValue] < [obj2 intValue]) {

            return NSOrderedAscending;
        } else {
            return NSOrderedDescending;
        }
    }];

（3） 系统自带方法
NSArray *arrays = [moneyArrays sortedArrayUsingSelector:@selector(compare:)];

（4）NSArray 快速求总和 最大值 最小值 和 平均值

NSArray *array = [NSArray arrayWithObjects:@"2.0", @"2.3", @"3.0", @"4.0", @"10", nil];
CGFloat sum = [[array valueForKeyPath:@"@sum.floatValue"] floatValue];
CGFloat avg = [[array valueForKeyPath:@"@avg.floatValue"] floatValue];
CGFloat max =[[array valueForKeyPath:@"@max.floatValue"] floatValue];
CGFloat min =[[array valueForKeyPath:@"@min.floatValue"] floatValue];
NSLog(@"%fn%fn%fn%f",sum,avg,max,min);
```
