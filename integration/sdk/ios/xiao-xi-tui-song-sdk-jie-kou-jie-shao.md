---
description: 以下接口生效依赖于推送模块，需集成推送相关AnalysysPush.framework文件，请正确集成。
---

# 消息推送SDK接口介绍

## 简介

推送消息是一种常用的运营方法。在合适的时间把合适的内容通过合适的方式推送给合适的人群，不仅能促进用户活跃，也能极大提升用户对产品的体验。 易观推送现在支持极光、个推、百度、小米四个三方平台。支持通过易观开发者平台设置以下三种类型的推送消息内容:

* 下发通知，点击通知，打开宿主 App
* 下发通知，点击通知，打开宿主 App 的特定页面
* 下发通知，点击通知，触发打开特定的 URL 页面

首先，开发者需要在 App 中集成第三方推送系统的 SDK，并在 App 初始化过程中获取设备推送 ID，并保存在方舟分析的用户信息中。以下简要说明集成第三方推送系统的集成方式

## 设置设备推送 ID

该接口用于设置推送平台（provider）提供的设备推送ID。

```objectivec
+ (void)setPushProvider:(AnalysysPushProvider)provider pushID:(NSString *)pushID;
```

* provider 推送提供方标识，目前只 `AnalysysPushProvider` 枚举中的类型
* pushID 第三方推送标识。如：极光的 `registrationID`，个推的 `clientId`，百度的 `channelid`，小米的 `xmRegId`

调用方法，以极光为例：

```objectivec
[AnalysysAgent setPushProvider:AnalysysPushJiGuang pushID:@"191e35f7e01617e5181"];
```

Swift示例:

```swift
AnalysysAgent.setPushProvider(.jiGuang, pushID: "191e35f7e01617e5181")
```

### 极光推送

请下载最新的极光推送 SDK，并根据[《iOS SDK 集成指南》](https://docs.jiguang.cn/jpush/client/iOS/ios_guide_new/)将SDK集成至开发者App中。并集成并初始化方舟SDK。

在 iOS App 中，首先获取 APNs 的 Device Token，然后注册极光推送，成功后极光推送会返回 `registrationID`，将此 `registrationID` 回传方舟 SDK 即可。

```objectivec
@interface AppDelegate () <JPUSHRegisterDelegate>

@end
```

```objectivec
#import <AnalysysAgent/AnalysysAgent.h>
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {

    // 初始化极光SDK后，获取 registrationID 并回传
    [JPUSHService registrationIDCompletionHandler:^(int resCode, NSString *registrationID) {
        if(resCode == 0){
            //  回传
            [AnalysysAgent setPushProvider:AnalysysPushJiGuang pushID:registrationID];
        } else{
            NSLog(@"registrationID获取失败，code：%d",resCode);
        }
    }];
}

//  获取 APNS Token 值
- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {

    //  向极光注册 deviceToken
    [JPUSHService registerDeviceToken:deviceToken];
}
```

### 百度推送

请下载最新的百度推送 SDK，并根据[《百度Push服务SDK用户手册（iOS版）》](http://push.baidu.com/doc/ios/api)将 SDK 集成至开发者App中。并集成并初始化方舟SDK。

在 iOS App 中，首先获取 APNs 的 Device Token，然后注册百度推送，成功后百度推送会返回 `channelid`，将此 `channelid` 回传方舟 SDK 即可。

```objectivec
@interface AppDelegate () <UIApplicationDelegate,UNUserNotificationCenterDelegate>

@end
```

```objectivec
#import <AnalysysAgent/AnalysysAgent.h>
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {

    // 初始化百度SDK后，获取 channelid 并回传
    [BPush bindChannelWithCompleteHandler:^(id result, NSError *error) {
        if (error) {
            return ;
        }
        if (result) {
            if ([result[@"error_code"]intValue]!=0) {
                return;
            }
            NSString *channelid = [BPush getChannelId];
            //  回传
            [AnalysysAgent setPushProvider:AnalysysPushBaiDu pushID:channelid];
        }
    }];
}

//  获取 APNS Token 值
- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {

    //  向百度注册 deviceToken
    [BPush registerDeviceToken:deviceToken];
}
```

### 小米推送

请下载最新的小米推送 SDK，并根据[《小米推送服务iOS客户端SDK使用指南》](https://dev.mi.com/console/doc/detail?pId=98)将 SDK 集成至开发者 App 中。并集成并初始化方舟SDK。

在 iOS App 中，首先获取 APNs 的 Device Token，然后注册小米推送，成功后小米推送会返回 `regid`，将此 `regid` 回传方舟 SDK 即可。

```objectivec
@interface AppDelegate () <MiPushSDKDelegate,UNUserNotificationCenterDelegate>

@end
```

```objectivec
#import <AnalysysAgent/AnalysysAgent.h>
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // 只启动APNs.
    [MiPushSDK registerMiPush:self];
}

//  获取 APNS Token 值
- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {

    //  向小米注册 deviceToken
    [MiPushSDK bindDeviceToken:deviceToken];
}

#pragma mark *** MiPushSDKDelegate ***
- (void)miPushRequestSuccWithSelector:(NSString *)selector data:(NSDictionary *)data {
    if ([selector isEqualToString:@"bindDeviceToken:"]) {
        NSString *xmRegId = data[@"regid"];
        //  回传
        [AnalysysAgent setPushProvider:AnalysysPushXiaoMi pushID:xmRegId];
    }
}
```

### 个推推送

请下载最新的个推推送 SDK，并根据[《iOS端 &gt; Xcode集成》](http://docs.getui.com/getui/mobile/ios/xcode/)将 SDK 集成至开发者 App 中。并集成并初始化方舟 SDK。

在 iOS App 中，首先获取 APNs 的 Device Token，然后注册个推推送，成功后个推推送会返回 `clientId`，将此 `clientId` 回传方舟 SDK 即可。

```objectivec
@interface AppDelegate () <UIApplicationDelegate, GeTuiSdkDelegate, UNUserNotificationCenterDelegate>

@end
```

```objectivec
#import <AnalysysAgent/AnalysysAgent.h>
//  获取 APNS Token 值
- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {

    //  向个推注册 deviceToken
    [GeTuiSdk registerDeviceToken:token];
}

#pragma mark *** GeTuiSdkDelegate ***
/** SDK启动成功返回cid */
- (void)GeTuiSdkDidRegisterClient:(NSString *)clientId {
    //  回传
    [AnalysysAgent setPushProvider:AnalysysPushGeTui pushID:clientId];
}
```

## 统计活动推广信息

追踪记录 provider 对应平台的消息推送的信息。

接口如下：

```objectivec
+ (void)trackCampaign:(id)userInfo isClick:(BOOL)isClick;

+ (void)trackCampaign:(id)userInfo isClick:(BOOL)isClick userCallback:(void(^)(id campaignInfo))userCallback;
```

* userInfo：推送携带的参数信息
* isClick：YES：用户点击通知；NO：接收到消息通知
* userCallback：将解析后的用户下发活动信息回调用户

三方推送平台 SDK 集成及示例代码

首先，开发者需要在 App 中集成第三方推送系统的 SDK，并在 App 初始化过程中获取设备推送 ID，并保存在方舟分析的用户信息中。目前易观 SDK 支持极光、百度、小米、个推四家第三方推送统计的支持。以下简要说明集成第三方推送系统的集成方式。

### 极光推送

```objectivec
#pragma mark *** JPUSHRegisterDelegate ***

// >= iOS 10 Support , App Forground
- (void)jpushNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(NSInteger))completionHandler {
    NSDictionary * userInfo = notification.request.content.userInfo;

    if([notification.request.trigger isKindOfClass:[UNPushNotificationTrigger class]]) {
        //  收到推送消息，追踪"App 消息推送"事件
        [AnalysysAgent trackCampaign:userInfo isClick:NO userCallback:^(id campaignInfo) {
            NSLog(@"此处可根据需要处理数据 : %@",campaignInfo);
        }];

        [JPUSHService handleRemoteNotification:userInfo];
    }
    completionHandler(UNNotificationPresentationOptionBadge | UNNotificationPresentationOptionSound | UNNotificationPresentationOptionAlert); // 需要执行这个方法，选择是否提醒用户，有Badge、Sound、Alert三种类型可以选择设置
}

// iOS 10 Support, App Background
- (void)jpushNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void (^)())completionHandler {
    NSDictionary *userInfo = response.notification.request.content.userInfo;

    if([response.notification.request.trigger isKindOfClass:[UNPushNotificationTrigger class]]) {
        //  点击通知栏打开消息，记录"App 点击通知"事件
        [AnalysysAgent trackCampaign:userInfo isClick:YES userCallback:^(id campaignInfo) {
            NSLog(@"此处可根据需要处理数据 : %@",campaignInfo);
        }];

        [JPUSHService handleRemoteNotification:userInfo];
    }
    completionHandler();
}

//  >= iOS 7 <iOS 10
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler {
    [JPUSHService handleRemoteNotification:userInfo];

    completionHandler(UIBackgroundFetchResultNewData);

    [self handleRemoteNotificationWithApplication:application userInfo:userInfo];
}

// < iOS 7
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
    [JPUSHService handleRemoteNotification:userInfo];

    [self handleRemoteNotificationWithApplication:application userInfo:userInfo];
}
```

### 百度推送

```objectivec
#pragma mark *** UNUserNotificationCenterDelegate ***
// >= iOS 10 Support , App Forground
- (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions))completionHandler {
    NSDictionary * userInfo = notification.request.content.userInfo;

    if([notification.request.trigger isKindOfClass:[UNPushNotificationTrigger class]]) {
        [BPush handleNotification:userInfo];
        //  收到推送消息，追踪"App 消息推送"事件
        [AnalysysAgent trackCampaign:userInfo isClick:NO userCallback:^(id campaignInfo) {
            NSLog(@"此处可根据需要处理数据 : %@",campaignInfo);
        }];
    }

    // 根据APP需要，判断是否要提示用户Badge、Sound、Alert
    completionHandler(UNNotificationPresentationOptionBadge | UNNotificationPresentationOptionSound | UNNotificationPresentationOptionAlert);
}

// iOS 10 Support, App Background
- (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void (^)())completionHandler {
    NSDictionary *userInfo = response.notification.request.content.userInfo;

    if([response.notification.request.trigger isKindOfClass:[UNPushNotificationTrigger class]]) {
        //  点击通知栏打开消息，记录"App 点击通知"事件
        [AnalysysAgent trackCampaign:userInfo isClick:YES userCallback:^(id campaignInfo) {
            NSLog(@"此处可根据需要处理数据 : %@",campaignInfo);
        }];

        [BPush handleNotification:userInfo];
    }
    completionHandler();
}

//  >= iOS 7 <iOS 10
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler {
    [BPush handleNotification:userInfo];

    completionHandler(UIBackgroundFetchResultNewData);

    [self handleRemoteNotificationWithApplication:application userInfo:userInfo];
}

// < iOS 7
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
    [BPush handleNotification:userInfo];

    [self handleRemoteNotificationWithApplication:application userInfo:userInfo];
}
```

### 小米推送

```objectivec
#pragma mark *** UNUserNotificationCenterDelegate ***
// >= iOS 10 Support , App Forground
- (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions))completionHandler {
    NSDictionary * userInfo = notification.request.content.userInfo;

    if([notification.request.trigger isKindOfClass:[UNPushNotificationTrigger class]]) {
        [MiPushSDK handleReceiveRemoteNotification:userInfo];
        //  收到推送消息，追踪"App 消息推送"事件
        [AnalysysAgent trackCampaign:userInfo isClick:NO userCallback:^(id campaignInfo) {
            NSLog(@"此处可根据需要处理数据 : %@",campaignInfo);
        }];
    }

    // 根据APP需要，判断是否要提示用户Badge、Sound、Alert
    completionHandler(UNNotificationPresentationOptionBadge | UNNotificationPresentationOptionSound | UNNotificationPresentationOptionAlert);
}

// iOS 10 Support, App Background
- (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void (^)())completionHandler {
    NSDictionary *userInfo = response.notification.request.content.userInfo;

    if([response.notification.request.trigger isKindOfClass:[UNPushNotificationTrigger class]]) {
        //  点击通知栏打开消息，记录"App 点击通知"事件
        [AnalysysAgent trackCampaign:userInfo isClick:YES userCallback:^(id campaignInfo) {
            NSLog(@"此处可根据需要处理数据 : %@",campaignInfo);
        }];

        [MiPushSDK handleReceiveRemoteNotification:userInfo];
    }
    completionHandler();
}

//  >= iOS 7 <iOS 10
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler {
    [MiPushSDK handleReceiveRemoteNotification:userInfo];

    completionHandler(UIBackgroundFetchResultNewData);

    [self handleRemoteNotificationWithApplication:application userInfo:userInfo];
}

// < iOS 7
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo {
    [MiPushSDK handleReceiveRemoteNotification:userInfo];

    [self handleRemoteNotificationWithApplication:application userInfo:userInfo];
}
```

### 个推推送

```objectivec
// iOS 10 Support, App Background
- (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void (^)())completionHandler {
    NSDictionary *userInfo = response.notification.request.content.userInfo;

    if([response.notification.request.trigger isKindOfClass:[UNPushNotificationTrigger class]]) {
        //  点击通知栏打开消息，记录"App 点击通知"事件
        [AnalysysAgent trackCampaign:userInfo isClick:YES userCallback:^(id campaignInfo) {
            NSLog(@"此处可根据需要处理数据 : %@",campaignInfo);
        }];

        [GeTuiSdk handleRemoteNotification:userInfo];
    }
    completionHandler();
}

//  >= iOS 7 <iOS 10
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler {
    [GeTuiSdk handleRemoteNotification:userInfo];

    completionHandler(UIBackgroundFetchResultNewData);

    [self handleRemoteNotificationWithApplication:application userInfo:userInfo];
}

#pragma mark *** GeTuiSdkDelegate ***
/** SDK收到透传消息回调 */
- (void)GeTuiSdkDidReceivePayloadData:(NSData *)payloadData andTaskId:(NSString *)taskId andMsgId:(NSString *)msgId andOffLine:(BOOL)offLine fromGtAppId:(NSString *)appId {
    // [ GTSdk ]：汇报个推自定义事件(反馈透传消息)
    [GeTuiSdk sendFeedbackMessage:90001 andTaskId:taskId andMsgId:msgId];

    // 数据转换
    NSString *payloadMsg = nil;
    if (payloadData) {
        payloadMsg = [[NSString alloc] initWithBytes:payloadData.bytes length:payloadData.length encoding:NSUTF8StringEncoding];

        //  若App在前台时用户做弹框等处理，请适当调整isClick:参数（是否用户点击）
        [AnalysysAgent trackCampaign:payloadMsg isClick:NO userCallback:^(id campaignInfo) {
            NSLog(@"此处可根据需要处理数据 : %@",campaignInfo);
        }];
    }
}
```

注：iOS 10.0以下回调方法统一调用的方法：

```objectivec
//  <iOS 10 版本统一处理通知回调方法
- (void)handleRemoteNotificationWithApplication:(UIApplication *)application userInfo:(NSDictionary *)userInfo{
    if (application.applicationState == UIApplicationStateActive) {
        //  App前台 收到推送消息，追踪"App 消息推送"事件
        [AnalysysAgent trackCampaign:userInfo isClick:NO userCallback:^(id campaignInfo) {
            NSLog(@"此处可根据需要处理数据 : %@",campaignInfo);
        }];

    } else if (application.applicationState == UIApplicationStateBackground) {
        //  App后台 收到推送消息，追踪"App 消息推送"事件
        [AnalysysAgent trackCampaign:userInfo isClick:NO userCallback:^(id campaignInfo) {
            NSLog(@"此处可根据需要处理数据 : %@",campaignInfo);
        }];
    } else {
        //  App非活动状态
        //  点击通知栏打开消息，记录"App 点击通知"事件
        [AnalysysAgent trackCampaign:userInfo isClick:YES userCallback:^(id campaignInfo) {
            NSLog(@"此处可根据需要处理数据 : %@",campaignInfo);
        }];
    }
}
```

