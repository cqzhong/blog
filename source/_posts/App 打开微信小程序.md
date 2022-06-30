---
title: App 打开微信小程序
urlname: hd47zy
date: 2018-11-30 15:06:53 +0800
tags: [iOS,微信小程序]
categories: [移动端]
---

1. 导入 sdk

```objc
po 'WechatOpenSDK'
```

<!-- more -->

2. 设置 URL Types

![](https://cdn.nlark.com/yuque/0/2020/png/1028501/1602931807565-84637c6b-e690-48a5-9541-dfa62692cb25.png#align=left&display=inline&height=791&margin=%5Bobject%20Object%5D&originHeight=791&originWidth=1032&size=0&status=done&style=none&width=1032)

3. weixin 加入到 LSApplicationQueriesSchemes 白名单
4. App Transport Security Settings、Allow Arbitrary Loads 打开
5. 入口类

```objc
#import "WXApi.h"
@interface CDAppDelegate : UIResponder <UIApplicationDelegate, WXApiDelegate>
@property (strong, nonatomic) UIWindow *window;
@end

#import "CDAppDelegate.h"

#import "CDViewController.h"

@implementation CDAppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {

    [WXApi registerApp:@"wx9bda0261a717aedb"];

//    [WXApi registerApp:@"wx425878e5ac948779"];
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];

    self.window.backgroundColor = [UIColor whiteColor];
     self.window.rootViewController = [CDViewController new];

    [self.window makeKeyAndVisible];
    return true;
}

- (BOOL)application:(UIApplication *)application handleOpenURL:(NSURL *)url {

    return  [WXApi handleOpenURL:url delegate:self];
}

- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation {

    return [WXApi handleOpenURL:url delegate:self];
}

-(void) onReq:(BaseReq*)req {

}

-(void) onResp:(BaseResp*)resp {

    if ([resp isKindOfClass:[WXLaunchMiniProgramResp class]]) {

        WXLaunchMiniProgramReq *miniProgramReq = (WXLaunchMiniProgramReq *)resp;

        NSString *string = miniProgramReq.extMsg;

        // 对应JsApi navigateBackApplication中的extraData字段数据


        NSLog(@"%@", string);

    }

}
@end
```
