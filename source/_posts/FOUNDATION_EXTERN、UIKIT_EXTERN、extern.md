---
title: FOUNDATION_EXTERN、UIKIT_EXTERN、extern
urlname: pabkvo
date: 2018-05-30 14:04:11 +0800
tags: [iOS]
categories: [移动端]
---

1. `FOUNDATION_EXTERN 是可以兼容 C++的 extern 的宏`
   <!-- more -->
   FOUNDATION_EXPORT
   FOUNDATION_IMPORT
   这两个是用来兼容 win32 应用程序的，当然这个宏我们在 iOS 编程中一般是很少用到的
   FOUNDATION_EXTERN 是在 Foundation 框架里面 NSObjCRuntime.h 中定义的。
   NSString 是在 Foundation 框架里面 NSString.h 中定义的。
   所以用 FOUNDATION_EXTERN 修饰 NSString。
   使用：
   FOUNDATION_EXTERN NSString *const CDRequestKeyVersion;
   NSString *const CDRequestKeyVersion = @"v2/";
1. `UIKIT_EXTERN,是经过处理的 extern。`
   简单来说：就是将函数修饰为兼容以往 C 编译方式的、具有 extern 属性(文件外可见性)、public 修饰的方法或变量库外仍可见的属性。
   UIKIT_EXTERN 是在 UIKit 框架里面 UIKitDefines.h 中定义的。
   CGFloat 在 CoreGraphics 框架里面 CGBase.h 中定义的。
   用 UIKIT_EXTERN 修饰 CGFloat。
   NSInteger 是在 usr/include 框架里面 NSObjCRuntime.h 中定义的。
   用 UIKIT_EXTERN 修饰 NSInteger。
   使用：
   UIKIT_EXTERN const CGFloat MJRefreshLabelLeftInset;
   const CGFloat MJRefreshLabelLeftInset = 25;
1. `extern 可以在多处声明，但是只能在一处实现`
   extern
   extern const CGFloat QMUIImagePreviewViewControllerCornerR
   adiusAutomaticDimension;
   const CGFloat QMUIImagePreviewViewControllerCornerRadiusAutomaticDimension = -1;
