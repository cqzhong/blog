---
title: ifdef 宏定义
urlname: ky14hg
date: 2018-08-10 11:55:05 +0800
tags: [宏定义,iOS]
categories: [移动端]
---

```objectivec
#ifdef LUMBERJACK


#else /* LUMBERJACK */


#endif /* LUMBERJACK */
```

<!-- more -->

```objectivec
#ifdef CDTEST

    NSLog(@"CDTEST");

#elif ADHOC
    NSLog(@"ADHOC");

#elif DEBUG

    NSLog(@"DEBUG");

#else
    NSLog(@"release");

#endif

#if defined(DEBUG) || defined(CDTEST)

    NSLog(@"DEBUG, CDTEST");

#else
    NSLog(@"release");
#endif
```

```objc
#if (APP_Type == 0)
// Debug
#define a @"000000"

#elif (APP_Type == 1)
// Release
#define a @"111111"

#elif (APP_Type == 2)
// TestA
#define a @"222222"

#elif (APP_Type == 3)
// TestB
![Uploading CA914A5C-31B3-48FB-A5F1-71CCB3B37B80_595336.png . . .]
#define a @"333333"
```

```objectivec
#if defined (__i386__) || defined (__x86_64__)
    //模拟器下执行
#else
    //真机下执行

#endif
```

```objectivec
#ifdef CDTHEME_COLOR

    NSLog(@"\n\n\n\n CDTHEME_COLOR 被宏定义过 \n\n\n\n\n\n\n");

#else

    NSLog(@"\n\n\n\n CDTHEME_COLOR 未被宏定义过 \n\n\n\n\n\n\n");

#endif
```
