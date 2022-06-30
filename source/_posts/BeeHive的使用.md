---
title: BeeHive的使用
urlname: hsr3zy
date: 2019-03-23 14:06:53 +0800
tags: [iOS,BeeHive]
categories: [移动端]
---

### 一、 BeeHive 介绍

- BeeHive 是阿里巴巴公司开源的一个 iOS 框架，这个框架是 App 模块化编程的框架一种实现方案，吸收了 Spring 框架 Service 的理念来实现模块间的 API 解耦。

<!-- more -->

- BeeHive 这个名字灵感来源于蜂窝。蜂窝是世界上高度模块化的工程结构，六边形的设计能带来无限扩张的可能。所以就用了这个名字作为开源项目的名字。

### 二、BeeHive 使用

- 新建一个 AppDelegate 继承 BHAppDelegate
- 在 main.m 文件内修改 AppDelegate

```c
int main(int argc, char * argv[]) {

    @autoreleasepool {
        return UIApplicationMain(argc, argv, nil, NSStringFromClass([HPAppDelegate class]));
    }
}
```

- 在 BHAppDelegate 内引用 BeeHive

```objectivec
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {


    [BHContext shareInstance].application = application;

    [BHContext shareInstance].launchOptions = launchOptions;


    /*

     配置环境 BHEnvironmentType

     给出定义四种环境，缺点是环境多余四种了不好扩充

     */
#if DEBUG

    [BHContext shareInstance].env = BHEnvironmentDev;
#else

    [BHContext shareInstance].env = BHEnvironmentProd;

#endif



    [BeeHive shareInstance].enableException = true;

    [[BeeHive shareInstance] setContext:[BHContext shareInstance]];


    [super application:application didFinishLaunchingWithOptions:launchOptions];


 id <HPMainServiceProtocol> mainService = [[BeeHive shareInstance] createService:@protocol(HPMainServiceProtocol)];
    /*

     模块化主要是 BaseServiceProtocol 和服务操作，不涉及UI。

     Protocol定义该模块内的方法。在service内实现该Protocol定义的方法

    */
    UIViewController *vc = [mainService getController];

    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];

    [self.window setBackgroundColor:[UIColor whiteColor]];

    self.window.rootViewController = vc;

    [self.window makeKeyAndVisible];

    return YES;

}
```

### 模块注册

1、每一个模块对应一个 Protocol ， service，Moudle

2、所有的模块都遵守 BHModuleProtocol 协议
3、可以使用 BH_EXPORT_MODULE(NO) 是否异步加载 Moudle (该方法是在 +load 方法内调用, 需要写到 [@implementation ](/implementation) 和 [@end ](/end) 之间)
4、也可以使用 [@BeeHiveMod(BaseMoudle) ](/BaseMoudle) 来注册 Moudlle (该方法是在应用启动前, 加载符号文件时候调用)
5、没个 service 文件中 都必须导入该服务的协议。

```objectivec

#import "HPMainServiceProtocol.h"

BeeHiveService(HPMainServiceProtocol, HPMainMoudleService)

@interface HPMainMoudleService () <HPMainServiceProtocol>


@end
```

6、在每个 Moudle 内去实现自定义处理事件，例如可以独立出一个 推送模块

```objectivec

@implementation APNSMoudle


//如果不去设置Level默认是Normal

//basicModuleLevel不去实现默认Normal

- (void)basicModuleLevel;

//越大越优先

- (NSInteger)modulePriority;


- (BOOL)async


- (void)modSetUp:(BHContext *)context {

}

- (void)modDidRegisterForRemoteNotifications:(BHContext *)context {
}

- (void)modDidFailToRegisterForRemoteNotifications:(BHContext *)context {

}

- (void)modDidReceiveRemoteNotification:(BHContext *)context{
}

- (void)modDidEnterBackground:(BHContext *)context {

}

- (void)modWillEnterForeground:(BHContext *)context {


}

- (void)modDidBecomeActive:(BHContext *)context {

    BOOL isOpenNotify = [self isAllowedNotification];

    if (isOpenNotify) {//推送被关闭

        [[UIApplication sharedApplication] unregisterForRemoteNotifications];

    } else { //已经开启推送

        [[UIApplication sharedApplication] registerForRemoteNotifications];
    }

}
//MARK: 判断不同系统下用户是否在设置界面关闭了推送

- (BOOL)isAllowedNotification {

    // YES关闭。NO未关闭

    UIUserNotificationSettings *setting = [[UIApplication sharedApplication] currentUserNotificationSettings];

    return (setting.types== UIUserNotificationTypeNone) ? YES : NO;

}
-(void)modWillResignActive:(BHContext *)context {



}

- (void)modOpenURL:(BHContext *)context{



    NSDictionary *options = context.openURLItem.options;



    NSURL *url = context.openURLItem.openURL;



    BOOL result = [[UMSocialManager defaultManager]  handleOpenURL:url options:options];

    if (!result) {

        // 其他如支付等SDK的回调

        if ([url.host isEqualToString:@"safepay"])  {

        }
    }

}

@end
```
