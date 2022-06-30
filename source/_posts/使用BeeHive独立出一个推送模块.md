---
title: 使用BeeHive独立出一个推送模块
urlname: fi2nn9
date: 2019-03-24 13:06:53 +0800
tags: [iOS,BeeHive,推送]
categories: [移动端]
---

废话不多说，直接上代码。这里以阿里推送 pod 'AlicloudPush', '~> 1.9.8' 为例

- CDAPNSModule.h

<!-- more -->

```objc

/**
 * 基类 Moudle
 * 每创建一个模块时, 需要继承该类, 并在Moudle 头文件中, 引入该模块的所有暴露的头文件
 * 可以使用 BH_EXPORT_MODULE(NO) 是否异步加载 Moudle (该方法是在 +load 方法内调用, 需要写到 @implementation 和 @end 之间)
 * 也可以使用 @BeeHiveMod(BaseModule) 来注册 Moudlle (该方法是在应用启动前, 加载符号文件时候调用)
 */

#define MOUDLE_REGISTER(moudle_imp) BeeHiveMod(moudle_imp)

#define MOUDLE_EXPORT(async) BH_EXPORT_MODULE(async)


@interface CDAPNSModule : NSObject  <BHModuleProtocol>

@end
```

- CDAPNSModule.m

```objc

#import "CDAPNSModule.h"
#import <CloudPushSDK/CloudPushSDK.h>

#ifdef NSFoundationVersionNumber_iOS_9_x_Max
#import <UserNotifications/UserNotifications.h>
#endif

static NSString *const CCPDidChannelConnectedSuccess = @"CCPDidChannelConnectedSuccess";
static NSString *const CCPDidReceiveMessageNotification = @"CCPDidReceiveMessageNotification";

//@BeeHiveMod(CDAPNSModule)

@interface CDAPNSModule () <UNUserNotificationCenterDelegate>

@end

@implementation CDAPNSModule

+ (void)load {
    // 使用动态注册的方法。
    [BeeHive registerDynamicModule:[self class]];
}
//BH_EXPORT_MODULE(NO)

//- (NSInteger)modulePriority {
//    return CGFLOAT_MAX;
//}
//- (void)basicModuleLevel {
//
//}
- (void)modInit:(BHContext *)context {

}
- (void)modSetUp:(BHContext *)context {

    // APNs注册，获取deviceToken并上报
    [self registerAPNS:context.application];
    // 初始化SDK
    [self initCloudPush];
    //     监听推送通道打开动作
    [self listenerOnChannelOpened];
    //     监听推送消息到达
    [self registerMessageReceive];
    [CloudPushSDK sendNotificationAck:context.launchOptions];

}
- (void)modDidRegisterForRemoteNotifications:(BHContext *)context {

    // 注册APNS成功, 注册deviceToken
//    NSString *deviceId = [[NSString alloc] initWithData:context.notificationsItem.deviceToken encoding:NSUTF8StringEncoding];
//    NSString *deviceTokenString = [[[[context.notificationsItem.deviceToken description] stringByReplacingOccurrencesOfString: @"<" withString: @""]stringByReplacingOccurrencesOfString: @">" withString: @""]stringByReplacingOccurrencesOfString: @" " withString: @""];
//
//    CDDLog(@"token---:%@",deviceTokenString);
    [CloudPushSDK registerDevice:context.notificationsItem.deviceToken withCallback:^(CloudPushCallbackResult *res) {
        if (res.success) {
            CDDLog(@"Register deviceToken success, deviceToken: %@", [CloudPushSDK getApnsDeviceToken]);
            CDDLog(@"Register deviceToken success, deviceToken: %@", [CloudPushSDK getApnsDeviceToken]);
            CDDLog(@"Register getDeviceId success, getDeviceId: %@", [CloudPushSDK getDeviceId]);
        } else {
            CDDLog(@"Register deviceToken failed, error: %@", res.error);
        }
    }];
}
- (void)modDidFailToRegisterForRemoteNotifications:(BHContext *)context {

    // 注册APNS失败. optional
    CDDLog(@"did Fail To Register For Remote Notifications With Error: %@", context.notificationsItem.notificationsError);
}
- (void)modDidEnterBackground:(BHContext *)context {

    [self clearBageNum:context.application];
}
- (void)modWillEnterForeground:(BHContext *)context {

    [self clearBageNum:context.application];
}
//MARK: 打开通知
//iOS (3_0, 10_0) App 处于前台,如果收到 远程通知 则调用该处理方法
- (void)modDidReceiveRemoteNotification:(BHContext *)context {

    //打开激活app
    //    NSDictionary *userInfoDict1 = [context.launchOptions objectForKey:UIApplicationLaunchOptionsRemoteNotificationKey];

    UIApplicationState state = [UIApplication sharedApplication].applicationState;
    if (state == UIApplicationStateActive) {

        return;
    }
    //在前台,在后台
    NSDictionary *userInfoDict = context.notificationsItem.userInfo;

    [self parsingTheSkipInfo:userInfoDict];
    context.application.applicationIconBadgeNumber = 0;
    [CloudPushSDK sendNotificationAck:context.notificationsItem.userInfo];
}
//MARK: -跳转规则
- (void)parsingTheSkipInfo:(NSDictionary *)userInfoDict {

}
//- (void)modDidBecomeActive:(BHContext *)context {
//
//    BOOL isOpenNotify = [self isAllowedNotification];
//    if (isOpenNotify) {//推送被关闭
//        [[UIApplication sharedApplication] unregisterForRemoteNotifications];
//    } else { //已经开启推送
//        [[UIApplication sharedApplication] registerForRemoteNotifications];
//    }
//}
//-(void)modWillResignActive:(BHContext *)context {
//
//}
//MARK: APNs Register
/**
向APNs注册，获取deviceToken用于推送

@param application application description
*/
- (void)registerAPNS:(UIApplication *)application {

    if (@available(iOS 10.0, *)) {

        // 请求推送权限
        [[UNUserNotificationCenter currentNotificationCenter] requestAuthorizationWithOptions:UNAuthorizationOptionAlert | UNAuthorizationOptionBadge | UNAuthorizationOptionSound completionHandler:^(BOOL granted, NSError * _Nullable error) {

            if (granted) {
                // 向APNs注册，获取deviceToken
                dispatch_async(dispatch_get_main_queue(), ^{

                    [application registerForRemoteNotifications];
                });
            } else {
                dispatch_async(dispatch_get_main_queue(), ^{

                    [application registerForRemoteNotifications];
                });
            }
        }];

        [self getNotificationSettingStatus];

    } else {
#pragma clang diagnostic push
#pragma clang diagnostic ignored"-Wdeprecated-declarations"
        [application registerUserNotificationSettings:
         [UIUserNotificationSettings settingsForTypes:
          (UIUserNotificationTypeSound | UIUserNotificationTypeAlert | UIUserNotificationTypeBadge)
                                           categories:nil]];
        dispatch_async(dispatch_get_main_queue(), ^{

            [application registerForRemoteNotifications];
        });
#pragma clang diagnostic pop
    }
}
/**
*  主动获取设备通知是否授权(iOS 10+)
*/
- (void)getNotificationSettingStatus NS_AVAILABLE_IOS(10_0) {
    [[UNUserNotificationCenter currentNotificationCenter] getNotificationSettingsWithCompletionHandler:^(UNNotificationSettings * _Nonnull settings) {
        if (settings.authorizationStatus == UNAuthorizationStatusAuthorized) {
            CDDLog(@"User authed.");
        } else {
            CDDLog(@"User denied.");
        }
    }];
}
    ///MARK: App处于前台时收到通知(iOS 10+)
- (void)modWillPresentNotification:(BHContext *)context NS_AVAILABLE_IOS(10_0) {

    if ([context.notificationsItem.notification.request.trigger isKindOfClass:[UNPushNotificationTrigger class]]) {

        //        [self handleiOS10Notification:context.notificationsItem.notification];
    }context.notificationsItem.notificationPresentationOptionsHandler(UNNotificationPresentationOptionSound | UNNotificationPresentationOptionAlert | UNNotificationPresentationOptionBadge);
}
//MARK: 触发通知动作时回调，比如点击、删除通知和点击自定义action(iOS 10+)
- (void)modDidReceiveNotificationResponse:(BHContext *)context NS_AVAILABLE_IOS(10_0) {

    if ([context.notificationsItem.notificationResponse.notification.request.trigger  isKindOfClass:[UNPushNotificationTrigger class]]) {

        [self handleiOS10Notification:context.notificationsItem.notificationResponse.notification];
    }
    context.notificationsItem.notificationCompletionHandler();
}
//MARK: 处理iOS 10通知(iOS 10+)
- (void)handleiOS10Notification:(UNNotification *)notification NS_AVAILABLE_IOS(10_0) {

    UNNotificationRequest *request = notification.request;
    UNNotificationContent *content = request.content;
    NSDictionary *userInfo = content.userInfo;

    [self parsingTheSkipInfo:userInfo];
    [CloudPushSDK sendNotificationAck:userInfo];
}
#pragma mark SDK Init
- (void)initCloudPush {

#if defined(DEBUG) || defined(CDTEST)

    [CloudPushSDK turnOnDebug];
#endif

    // 请从控制台下载AliyunEmasServices-Info.plist配置文件，并正确拖入工程
    [CloudPushSDK autoInit:^(CloudPushCallbackResult *res) {
        if (res.success) {
            CDDLog(@"Push SDK init success, deviceId: %@.", [CloudPushSDK getDeviceId]);
        } else {
            CDDLog(@"Push SDK init failed, error: %@", res.error);
        }
    }];
}
#pragma mark Channel Opened

/**
 消息模块监听
 */
- (void)listenerOnChannelOpened {
    [[NSNotificationCenter defaultCenter] addObserver:self
                                             selector:@selector(onChannelOpened:)
                                                 name:CCPDidChannelConnectedSuccess
                                               object:nil];
}
/**
推送通道打开回调

@param notification 消息内容
*/
- (void)onChannelOpened:(NSNotification *)notification {
    CDDLog(@"消息通道建立成功");
}
//MARK: 推送消息
- (void)registerMessageReceive {
    [[NSNotificationCenter defaultCenter] addObserver:self
                                             selector:@selector(onMessageReceived:)
                                                 name:CCPDidReceiveMessageNotification
                                               object:nil];
}
/**
处理到来推送消息

@param notification 消息内容
*/
- (void)onMessageReceived:(NSNotification *)notification {

//        CCPSysMessage *message = [notification object];
//        CDDLog(@"接收到的消息内容：%@",[[NSString alloc] initWithData:message.title encoding:NSUTF8StringEncoding]);
}
- (void)clearBageNum:(UIApplication *)application {
    [CloudPushSDK syncBadgeNum:0 withCallback:^(CloudPushCallbackResult *res) {

    }];

    dispatch_async(dispatch_get_main_queue(), ^{

        [application setApplicationIconBadgeNumber:0];
        [application cancelAllLocalNotifications];
    });
}
//MARK: 判断不同系统下用户是否在设置界面关闭了推送
- (BOOL)isAllowedNotification {
    // true关闭。false关闭
    UIUserNotificationSettings *setting = [[UIApplication sharedApplication] currentUserNotificationSettings];
    return (setting.types== UIUserNotificationTypeNone) ? YES : NO;
}


@end
```

- 手动注册推送

```objectivec
[[BHModuleManager sharedManager] registerDynamicModule:[CDAPNSModule class] shouldTriggerInitEvent:true];
```
