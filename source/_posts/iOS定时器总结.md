---
title: iOS定时器总结
urlname: poyvtz
date: 2019-10-11 11:41:00 +0800
tags: [iOS,定时器]
categories: [移动端]
---

iOS 中常用的定时器有三种，分别是 NSTimer，CADisplayLink 和 GCD。

<!-- more -->

#### NSTimer

两种方式创建

创建方式 1

```objc
NSTimer *timer = [NSTimer scheduledTimerWithTimeInterval:2 target:self selector:@selector(test) userInfo:nil repeats:YES];
    // 停止定时器
    [timer invalidate];
     timer == nil
```

创建方式 2

```objc
NSTimer *timer = [NSTimer timerWithTimeInterval:2 target:self selector:@selector(test) userInfo:nil repeats:YES];
    // 将定时器添加到runloop中，否则定时器不会启动
    [[NSRunLoop mainRunLoop] addTimer:timer forMode:NSRunLoopCommonModes];
    // 停止定时器
    [timer invalidate];
    timer == nil
```

方式 1 会自动将创建的定时器以默认方式添加到当前线程 runloop 中，而无需手动添加。但是在此种模式下，当滚动屏幕时 runloop 会进入另外一种模式，定时器会暂停，为了解决这种问题，可以像方式 2 那样把定时器添加到 NSRunLoopCommonModes 模式下。

方式 1 和方式 2 在设置后都会在间隔设定的时间（本例中设置为 2s）后执行 test 方法，如果需要立即执行可以使用下面的代码。

[time fire];

不过，NSTimer 相对来说是不精确的，参考苹果官方文档介绍 [timer](https://developer.apple.com/documentation/foundation/timer)

**问题**
1.NSTimer 加在 main runloop 中，模式是 NSDefaultRunLoopMode，main 负责所有主线程事件，例如 UI 界面的操作，复杂的运算，这样在同一个 runloop 中 timer 就会产生阻塞。 2.模式的改变。主线程的 RunLoop 里有两个预置的 Mode：kCFRunLoopDefaultMode 和 UITrackingRunLoopMode。
当你创建一个 Timer 并加到 DefaultMode 时，Timer 会得到重复回调，但此时滑动一个 ScrollView 时，RunLoop 会将 mode 切换为 TrackingRunLoopMode，这时 Timer 就不会被回调，并且也不会影响到滑动操作。所以就会影响到 NSTimer 不准的情况。
PS:DefaultMode 是 App 平时所处的状态，rackingRunLoopMode 是追踪 ScrollView 滑动时的状态。

**解决办法**

```objc
方案1.在主线程中进行NSTimer操作，但是将NSTimer实例加到main runloop的特定mode（模式）中。避免被复杂运算操作或者UI界面刷新所干扰。
NSTimer *timer = [NSTimer timerWithTimeInterval:1 target:self selector:@selector(test) userInfo:nil repeats:YES];
[[NSRunLoop currentRunLoop] addTimer:timer forMode:NSRunLoopCommonModes];

方案2.在子线程中进行NSTimer的操作，再在主线程中修改UI界面显示操作结果；
- (void)timer2 {
    NSThread *thread = [[NSThread alloc] initWithTarget:self selector:@selector(newThread) object:nil];
    [thread start];
}
- (void)newThread {
    @autoreleasepool {
    [NSTimer scheduledTimerWithTimeInterval:1.0 target:self selector:@selector(showTime) userInfo:nil repeats:YES];
    [[NSRunLoop currentRunLoop] run];
      }
}
```

#### CADisplayLink

```objc
CADisplayLink *displayLink = [CADisplayLink displayLinkWithTarget:self selector:@selector(test:)];
    // 将创建的displaylink添加到runloop中，否则定时器不会执行
    [displayLink addToRunLoop:[NSRunLoop mainRunLoop] forMode:NSDefaultRunLoopMode];

    // 停止定时器
    [displayLink invalidate];
    displayLink = nil;
当把CADisplayLink对象add到runloop中后，selector就能被周期性调用，类似于重复的NSTimer被启动了；执行invalidate操作时，CADisplayLink对象就会从runloop中移除，selector调用也随即停止，类似于NSTimer的invalidate方法
```

**注意点：**

iOS 并不能保证能以每秒 60 次的频率调用回调方法，这取决于：

1、CPU 的空闲程度

如果 CPU 忙于其它计算，就没法保证以 60HZ 执行屏幕的绘制动作，导致跳过若干次调用回调方法的机会，跳过次数取决 CPU 的忙碌程度。

2、执行回调方法所用的时间

如果执行回调时间大于重绘每帧的间隔时间，就会导致跳过若干次回调调用机会，这取决于执行时间长短。

总结：

从原理上不难看出，CADisplayLink 使用场合相对专一，适合做界面的不停重绘，比如视频播放的时候需要不停地获取下一帧用于界面渲染。

#### GCD 定时器

一次性定时

```objc
dispatch_time_t timer = dispatch_time(DISPATCH_TIME_NOW, 1.0 * NSEC_PER_SEC);

 dispatch_after(timer, dispatch_get_main_queue(), ^(void){

        NSLog(@"GCD-----%@",[NSThread currentThread]);

    });
```

重复执行的定时器

```objc
{
    //0.创建队列
    dispatch_queue_t queue = dispatch_get_main_queue();
    //1.创建GCD中的定时器
    /*
     第一个参数:创建source的类型 DISPATCH_SOURCE_TYPE_TIMER:定时器
     第二个参数:0
     第三个参数:0
     第四个参数:队列
     */
    dispatch_source_t timer = dispatch_source_create(DISPATCH_SOURCE_TYPE_TIMER, 0, 0, queue);

    //2.设置时间等
    /*
     第一个参数:定时器对象
     第二个参数:DISPATCH_TIME_NOW 表示从现在开始计时
     第三个参数:间隔时间 GCD里面的时间最小单位为 纳秒
     第四个参数:精准度(表示允许的误差,0表示绝对精准)
     */
    dispatch_source_set_timer(timer, DISPATCH_TIME_NOW, 1.0 * NSEC_PER_SEC, 0 * NSEC_PER_SEC);

    //3.要调用的任务
    dispatch_source_set_event_handler(timer, ^{
        NSLog(@"GCD-----%@",[NSThread currentThread]);
    });

    //4.开始执行
    dispatch_resume(timer);

    //
    self.timer = timer;
}
```

注意的地方： 此处注意一定要强引用定时器 ，否则定时器执行到 } 后将会被释放，无定时效果。GCD 定时器时间非常精准，最小的定时时间可以达到 1 纳秒，所以用在非常精确的定时场合。

NSObject 的方法也有类似功能的方法

```objc

- (void)performSelector:(SEL)aSelector withObject:(nullable id)anArgument afterDelay:(NSTimeInterval)delay;
```
