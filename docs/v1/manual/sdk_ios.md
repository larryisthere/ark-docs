# iOS SDK 集成指南

适用于iOS应用，支持Hybrid APP。

## 1. 下载SDK

最新版本：[AnalysysFangzhou_iOS_SDK_V3.7.9](https://ark.analysys.cn/sdk/AnalysysFangzhou_iOS_Idfa_SDK_V3.7.9.zip)

> 更新时间：2018/08/01
>
> 更新内容：  
>
>   1) 性能优化
>

## 2. 准备环境导入SDK

解压下载文件AnalysysFangzhou_iOS_Idfa_SDK_xxxx，将对应版本文件夹下的EGMonitor文件导入您的项目工程中    

### 2.1 基本配置

1）选择工程根目录->TARGETS->Build Phases->Link Binary With Libraries，检查是否存在EGMonitorLib.a类库，若不存在则点击“+”，Add Other..选择EGMonitor下的libEGMonitor.a，点击确定   

2）添加系统依赖库如下（路径同1）：
    
类库                          | 说明             | 
------------------------------|------------------|
CoreTelephony.framework      | 用于读取手机运营商信息   | 
SystemConfiguration.framework      | 用于判断网络状态   | 
libz.tbd      | 用于数据处理   | 
CoreLocation.framework      | 读取地理位置信息（读取地理位置）   | 
AdSupport.framework      |  广告标识（参考‘[IDFA说明](#idfa)’）  | 

如下图所示：

![示例图片](http://file1.analysys.cn/fangzhou/ceb1ee253353887cd2d1a84d84057f2d_1383x360.png)

### 2.2 iOS 9之适配ATS

iOS 9.0中新增App Transport Security（简称ATS）特性, 主要使原来请求的时候用到的HTTP，都转向TLS1.2协议进行传输。这也意味着所有的HTTP协议都强制使用了HTTPS协议进行传输。     
如果我们在iOS 9.0以后直接进行HTTP请求是会收到如下错误提示：        
> App Transport Security has blocked a cleartext HTTP (http://) resource load since it is insecure. Temporary exceptions can be configured via your app's Info.plist file.   

系统会告诉我们不能直接使用HTTP进行请求，需要在Info.plist新增一段用于控制ATS的配置：

```xml
<key>NSAppTransportSecurity</key>
<dict>
 <key>NSAllowsArbitraryLoads</key>
 <true/>
</dict>
```
即：在工程目录中->TARGETS->Info新增配置如下:

![ATS](http://file1.analysys.cn/fangzhou/1aec4bf1cedd8428eddc8faeb1ae4b58_1392x396.png)

> 苹果规定 从2017年1月1日起,新提交的 app 不允许使用NSAllowsArbitraryLoads来绕过ATS(全称：App Transport Security)的限制。  

### 2.3 地理位置信息配置
在plist文件中添加NSLocationWhenInUseUsageDescription或NSLocationAlwaysUsageDescription配置：   
找到info.plist文件-->右击-->Open As-->Source Code-->在尾部的`</dict>`标签之前添加并修改对应描述信息（选择一个即可）：  

```xml
<key>NSLocationUsageDescription</key>
<string>使用位置描述说明</string>
```
```xml
<key>NSLocationWhenInUseUsageDescription</key>
<string>app使用期间获取GPS的描述</string>
```
或者

```xml
 <key>NSLocationAlwaysUsageDescription</key>
 <string>app一直获取GPS的描述</string>
```

![GPS](http://file1.analysys.cn/fangzhou/6328d1987a0b00f1c5e33028286973f3_1392x395.png)

### <span id = "URLScheme">2.4 URL Scheme配置
该配置用于其他App或H5页面唤醒本App时的数据统计。     
在工程目录->TARGETS->Info->URL Types，添加相应的URL Scheme。identifier: EGMonitor；URL Scheme：h5sdk。如下图所示：     

![URL Scheme](http://file1.analysys.cn/fangzhou/3e3930e642f52ffb3a2ac2c559d56dc8_1393x381.png)

### 2.5 其他配置
#### 2.5.1 加载静态库中的类别(category)    
工程-->TARGETS-->Build Setting-->搜索Other Linker Flags-->添加 "-ObjC"，如下图所示：

![Category](http://file1.analysys.cn/fangzhou/eab5931f644ac565a8168acf5befb5c7_1396x462.png)

##  3. 初始化SDK
导入头文件 #import "EGMonitor.h"
在 ~AppDelegate.m 中配置SDK相关内容。代码示例如下：

```objectivec

#import "EGMonitor.h"     
   
-(BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {    
    // Override point for customization after application launch.
    
    EGConfigInstance.appKey = @"易观AppKey";
    EGConfigInstance.channelId = @"App Store";    //  渠道标识
    EGConfigInstance.debugEnable = YES; // 若为Release状态，则置为NO
    EGConfigInstance.autoPositionEnable = YES;  // 是否允许自动获取地理位置信息，若为YES参考上方《2.3地理位置信息配置》
    [EGMonitor startWithConfigure:EGConfigInstance];  // 初始化SDK    
    ...   
    
    return YES;
}
   
```


**注意事项：**
> 1）请将初始化方法写在根视图初始化前，防止统计遗漏；   
2）EGConfigInstance为SDK参数配置的实例类，EGMonitorConfig类中标注有'必填项'属性必须正确填写，非必填使用可查看具体属性的注释信息；   
> 3）appKey为添加应用后获取到的AppKey，一个AppKey仅能用于一个App，同一应用不同平台需分别添加并获取AppKey；    
> 4）channelId的值为应用的渠道标识。默认为 @"App Store"；    
> 5）debugEnable设置是否调试模式。默认非调试模式，即：debugEnable = NO；当设置debugEnable = YES时，SDK会实时上传日志信息，供开发者查看埋点是否正确，上线时记得设置debugEnable = NO。   
 
## 4. 重要API接口说明 
### 4.1 采集页面事件信息 
说明：页面统计区分iOS原生页面和H5页面两种。原生页面即：使用UIViewController产生的页面；H5页面即：混合模式开发，使用了web开发，嵌入了UIWebView或WKWebView页面。   

具体需要采集哪些页面事件信息，请登录[方舟官网](http://ark.analysys.cn/)：我的应用--管理--对应应用<回数状态>列--预埋点事件梳理 tab页面，添加对应页面事件信息和属性信息（[属性数据格式说明](#attrType)）。    

![页面配置信息](http://file1.analysys.cn/fangzhou/c961d29501de23390d63010e5d531f03_1824x247.png)

参数：  

* @param pageIdentifier 页面标识，“预埋点事件梳理”事件类型为“页面”对应的“事件ID”   
* @param attributes 页面属性，当前“事件ID”对应的属性信息。
* @param urlStr 页面自定义属性，可以为当前页面的路径 

示例：  

```objectivec
-(void)viewWillAppear:(BOOL)animated {
    [super viewWillAppear:animated];
    
    //  页面开始访问，pageIdentifier为页面的“事件ID”
    [EGMonitor beginLogPage:@"LoginPage"];
}
           
-(void)viewWillDisappear:(BOOL)animated {
    [super viewWillDisappear:animated];
    
    //  使用 endLogPage:attributes:urlStr: 添加自定义属性信息，自定义属性key为“属性ID”
    NSMutableDictionary *attr = [NSMutableDictionary dictionary];
    attr[@"Type"] = @"QQ";   //  登录方式
    attr[@"Tag"] = @[@"电子商品", @"消耗品"];   //  页面标签
    attr[@"HasStock"] = @YES;    //  是否有库存
    [EGMonitor endLogPage:@"LoginPage" attributes:attr urlStr:@"/Detail/ProductDetail"];
    
    //  若不含其它属性，则可使用endLogPage: 
	//  [EGMonitor endLogPage:@"LoginPage"];
}    
```

**注意事项：** 
> 开始和结束接口页面标识必须相同，否则会导致统计数据不准确。  
> 若要统计每一个详情页面的情况（如：所有详情页为同一个类），则可通过urlStr参数设置具体页面标识，如：page/detail?id=1001；或添加属性值的方式区分。    


### 4.2 采集点击事件信息
具体需要采集哪些点击事件信息，请登录[方舟官网](http://ark.analysys.cn/)：我的应用--管理--对应应用<回数状态>列--预埋点事件梳理 tab页面，添加对应事件信息和属性信息（[属性数据格式说明](#attrType)）。      

![事件配置信息](http://file1.analysys.cn/fangzhou/d3176624ff75fd5767e2ef0cf911cd48_1834x343.png)

参数：  

* @param eventId 事件标识，“预埋点事件梳理”事件类型为“点击事件”对应的“事件ID”    
* @param attributes  事件属性，当前“事件ID”对应的属性信息。（[属性类型说明](#attrType)）   

示例：   

```objectivec     
//	属性key值为事件的“属性ID”
NSMutableDictionary *attr = [NSMutableDictionary dictionary];
attr[@"PayType"] = @"Alipay";   //  支付方式
attr[@"Price"] = @160.8;    //  价格
attr[@"HasStock"] = @YES;    //  是否有库存
attr[@"DeliveryDate"] = @"2018-01-01 12:30:00";    //  送达日期  注：日期类型必须转为字符串类型
[EGMonitor event:@"Purchase" attributes:attr];

//	若不含属性，则可以使用：
//  [EGMonitor event:@"Purchase"];    
```

### 4.3 采集用户登录退出信息  
在用户登入之时，具体需要上传哪些用户属性，请登录[方舟官网](http://ark.analysys.cn/)：我的应用--管理--对应应用<回数状态>列--预上传用户属性梳理 tab页面，添加对应用户属性信息（[属性数据格式说明](#attrType)）。   

![页面配置信息](http://file1.analysys.cn/fangzhou/5a2313e65bde50c3dee9fbaad8cf717e_1818x345.png)     

#### 4.3.1 用户登录统计
参数：  

* @param userInfo EGUserInfo实例 
  
示例：   

```objectivec    
EGUserInfo *userInfo = [[EGUserInfo alloc] init];
userInfo.userId = @"8866"; //   真实用户id，必填项
userInfo.userName = @"张三";
userInfo.provider = @"Wechat";
userInfo.email = @"zhangsan@163.com";
userInfo.phoneNumber = @"18688886666";
userInfo.userSex = @"man";
userInfo.birthday = @"1995-10-10";
userInfo.qq = @"1188998866";
userInfo.weChat = @"18688886666";
userInfo.weiBo = @"analysys.163.com";
//  自定义属性部分，key为“用户属性ID”
userInfo.customerAttributes[@"Province"] = @"北京";   //  省份
userInfo.customerAttributes[@"Southerners"] = @YES; //  是否南方
userInfo.customerAttributes[@"Age"] = @18;  //  年龄
userInfo.customerAttributes[@"Weight"] = @60.5; //  体重

[EGMonitor signIn:userInfo];    
```

#### 4.3.2 用户退出统计   
说明：当用户退出帐号或进行帐号切换时调用  
参数：无  
示例：  

```
[EGMonitor signOff];    
```

#### 4.3.3 用户信息修改   
说明：修改已登录用户的信息     

* @param userInfo EGUserInfo实例.   
示例：  

```objectivec

EGUserInfo *userInfo = [[EGUserInfo alloc] init];
userInfo.userId = @"8866";  //  已存在用户id
userInfo.provider = @"Alipay";
userInfo.phoneNumber = @"15899998888";
//  修改用户属性，key为“用户属性ID”。若属性不存在则新增（参考"预上传用户属性梳理"）；存在则修改。
userInfo.customerAttributes[@"Weight"] = @65.3;
userInfo.customerAttributes[@"Animal"] = @"dragon";
    
[EGMonitor editUser:userInfo];
```

### <span id = "Hybrid">4.4 采集Hybrid页面、事件信息   
如果需要对应用内使用UIWebView或WKWebView加载的易观H5相关页面进行数据统计，请使用该模块进行集成。     
具体页面及事件的预埋点信息，请参考4.1和4.2中对应的链接中配置。请登录[方舟官网](http://ark.analysys.cn/)：我的应用--管理--对应应用<回数状态>列--开始集成或查看日志--预埋点事件梳理 tab页面，添加对应页面和事件埋点信息和属性信息   

#### 4.4.1 H5页面配置  
导入JS-SDK:

> 最新版本：[AnalysysFangzhou_JS_APP_SDK](http://ark.analysys.cn/sdk/AnalysysFangzhou_JS_APP_SDK.min.js) 


将JS-SDK下载到相应目录;

将以下代码修改目录后,复制到您所需分析页面中的 < head > 和 < /head > 标签之间即可。安装成功后，所有网址下的行为数据都将会被收集

```html
<script src="AnalysysFangzhou_JS_APP_SDK.min.js" async ></script>
```

**fz.pageTAG({Key:value,...})**

说明：设置页面属性


参数：
* Object

> 为JSON格式，加入相关KEY与VALUE值，如只有KEY值无具体VALUE值，不会上传该字段。


示例：

```html
fz.pageTAG({"页面属性01":"属性01"});  
```

**fz.open()**

说明：用以Url地址不变的情况下，检测模块切换状况。

> 如有改变页面Title行为，请在该方法之前使用JS改变页面Title，后使用该方法。


示例：
```html
fz.open();
```

**fz.event({EI:””,EPD:{}})**

说明：用户自定义事件

参数：
* EI 事件ID
* EPD 事件属性

> 1)EI 事件ID可在方舟产品中的元事件管理进行预定义。

> 2)EPD 为JSON格式。

示例：

```html
fz.event({
  EI:"ID01",
  EPD:{
    "event01":"事件01"
  }
  });  
```

**fz.setUInfo({"UID":"","UN":"","UPRO":"","EM":"","PN":"","SEX":"","BIY":"","QQ":"","WED":"","WBD":"","UPD":{},})**

说明：设置访问用户的个体信息。

参数：
* UID         用户ID
* UN          用户名称
* UPRO        用户来源
* EM          用户邮箱
* PN          用户电话
* SEX         用户性别
* BIY         用户生日
* QQ          用户QQ号
* WED         用户微信号
* WBD         用户微博号
* UPD         用户自定义属性字典

> 1)UPD 为JSON格式。

示例：

```html
fz.setUInfo({
   "UID":"123456",
   "UN":"用户01",
   "UPRO":"直接注册",
   "EM":"123456@qq.com",
   "PN":"18600001234",
   "SEX":"1",
   "BIY":"1988-01-01",
   "QQ":"123456",
   "WED":"18600001234",
   "WBD":"123456@qq.com",
   "UPD":{"自定义属性01":"属性01"}
}); 
```

**fz.delUInfo()**
说明：删除通过setUInfo接口所设置的访问用户的个体信息。

示例：

```html
fz.delUInfo();
```

**fz.openAPP(scheme)**
说明：仅用于使用JS SDK唤醒集成了方舟的iOS/Android SDK的应用。

参数：   

* scheme 为安装了方舟的iOS/Android SDK的应用安装相关文档所配置的scheme

示例：

```html
fz.openAPP("h5sdk");
```

#### 4.4.2 App内方法调用  
说明：将H5页面嵌入App内，可以使用UIWebView或WKWebView（iOS 8.0以后）。若app仅支持8.0之后版本，建议使用WKWebView控件，效率更高。    
参数：  

* @param request request请求对象  

**UIWebView示例**  

```objectivec
//  实现UIWebViewDelegate的方法
-(BOOL)webView:(UIWebView*)webView shouldStartLoadWithRequest:(NSURLRequest*)request navigationType:(UIWebViewNavigationType)navigationType {
    if ([EGMonitor h5InfoStatEnterWithRequest:request]) {
        NSLog(@"H5统计完成");
    }
    return YES;
}
```

**WKWebView示例**

```objectivec
//  iOS 8.0之后新增WKWebView是一个高性能的WebView解决方案，实现WKNavigationDelegate代理方法：
-(void)webView:(WKWebView*)webView decidePolicyForNavigationAction:(WKNavigationAction*)navigationAction decisionHandler:(void (^)(WKNavigationActionPolicy))decisionHandler {
    if (condition) { // 填写具体跳转条件
        if ([EGMonitor h5InfoStatEnterWithRequest:navigationAction.request]) {
            NSLog(@"H5统计完成");
        }
        decisionHandler(WKNavigationActionPolicyAllow);
    } else {
        decisionHandler(WKNavigationActionPolicyCancel);
    }
}
```

### 4.5 采集地理位置信息  
说明：若用户配置EGConfigInstance.autoPositionEnable = YES;则调用以下接口无效。   
参数：   

* @param latitude 维度值    
* @param longitude 经度值  
* @param location CLLocation对象

示例： 

```
[EGMonitor setLatitude:39.983687 longitude:116.461477];
```
或

```
[EGMonitor setLocation:cllocation];
```

### 4.6 采集App唤醒信息
说明：可以通过配置URL Scheme统计当前App被第三方App调起相关数据(请确保[URL Scheme配置](#URLScheme))。若通过H5页面调起App，请参考[4.4 采集Hybrid页面、事件信息](#Hybrid)。   
参数：

* @param url NSURL对象
* @param options sourceApplication/options
* @return 若集成H5-SDK后可返回部分自定义信息
 
示例：    

```objectivec
//  兼容iOS 9.0之前版本
- (BOOL)application:(UIApplication *)application handleOpenURL:(NSURL *)url {
    NSDictionary *custPara = [EGMonitor wakeupWithOpenURL:url options:nil];
    
    return YES;
}
//  兼容iOS 9.0之前版本
- (BOOL)application:(UIApplication *)application openURL:(NSURL *)url sourceApplication:(NSString *)sourceApplication annotation:(id)annotation {
    NSDictionary *custPara = [EGMonitor wakeupWithOpenURL:url options:sourceApplication];
    
    return YES;
}
//  iOS 9.0及之后版本
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<NSString*, id> *)options {
    NSDictionary *custPara = [EGMonitor wakeupWithOpenURL:url options:options];
    
    return YES;
}
```

### 4.7 消息推送
#### 4.7.1 推送基本设置
说明：下载最新版极光或百度 推送SDK并集成。  
将对应推送厂家官网注册并申请的AppKey和Master Secret信息回填、绑定到方舟《推送配置》模块。如下图所示： 
![页面配置信息](http://file1.analysys.cn/fangzhou/f660a4672b1534efbdb5c633c6dc8825_1764x232.png)

参数：    
* @param provider 使用的第三方类型。若同时使用多个推送服务则可多次调用   
* @param pushId 第三方推送使用的真正标识推送的id，如：极光的registrationID，百度的channel_id  

示例厂商 |  SDK版本|
:--------:|:-----------|
极光 | JPush iOS SDK v3.0.6 |
百度 | iOS V1.4.9|

<!--个推 | 版本号：iOS-2.0.0.0 |
小米 | 2.2.7|-->
 
**极光示例**  

```objectivec
//  注册APNs成功并上报DeviceToken
- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {

  [JPUSHService registerDeviceToken:deviceToken];
  
  [JPUSHService registrationIDCompletionHandler:^(int resCode, NSString *registrationID) {
    if(resCode == 0){
      //  将极光registrationID回传方舟SDK
      [EGMonitor setPushProvider:PushProviderJG pushId:registrationID];
    } else {
      NSLog(@"registrationID获取失败，code：%d",resCode);
    }
  }];
}
@end
```

**百度示例**  

```objectivec
//  注册APNs成功并上报DeviceToken
- (void)application:(UIApplication *)app didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
    
    [BPush registerDeviceToken:deviceToken];
    
    [BPush bindChannelWithCompleteHandler:^(id result, NSError *error) {
        // 网络错误
        if (error) {
            return;
        }
        
        if (result) {
            if ([result[@"error_code"]intValue] != 0) {
                return;
            }
            NSString *channel_id = [BPush getChannelId];
            //  将百度ChannelId回传方舟SDK
            [EGMonitor setPushProvider:PushProviderBaiDu pushId:channel_id];
        }
    }];
}
```

<!--**个推示例**  
```objectivec
//  注册APNs成功并上报DeviceToken
- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
 
    NSString *token = [[deviceToken description] stringByTrimmingCharactersInSet:[NSCharacterSet characterSetWithCharactersInString:@"<>"]];
    token = [token stringByReplacingOccurrencesOfString:@" " withString:@""];
    NSLog(@"\n>>>[DeviceToken Success]:%@\n\n", token);
	
    // 向个推服务器注册deviceToken
    [GeTuiSdk registerDeviceToken:token];
}
	
- (void)GeTuiSdkDidRegisterClient:(NSString *)clientId {
    //  将个推clientId回传方舟SDK
    [EGMonitor setPushProvider:PushProviderGT pushId:clientId];
}
	
@end
```

**小米示例** 
	
```objectivec
//  注册APNs成功并上报DeviceToken
 - (void)application:(UIApplication *)app didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
    // 注册APNS成功, 注册deviceToken
    [MiPushSDK bindDeviceToken:deviceToken];
 }
 
 - (void)miPushRequestSuccWithSelector:(NSString *)selector data:(NSDictionary *)data {
    if ([selector isEqualToString:@"bindDeviceToken:"]) {
        // 获取regId
        NSString *xmRegId = data[@"regid"];
        [EGMonitor setPushProvider:PushProviderXiaoMi pushId:xmRegId];
    }
}
	
```-->


#### 4.7.2 推送效果统计
1) 当用户收到推送消息时，调用收到推送消息接口上报消息推送成功事件；  
2) 若用户触发点击通知行为，则调用通知点击接口上报上报事件。    

目前推送通知共包含以下四种行为：
     
- 跳转应用指定页面：若使用UITabBarController、UINavigationController及其子类作为window.rootViewController的应用，则调用pushViewController:方法到相应页面；若使用UIViewController及其子类时，则调用presentViewController:方式模态推出，需用户在推出页面中添加取消按钮，并调用dismissViewControllerAnimated:方法返回页面。同时会将用户在页面中自定义参数信息传入被推出页面的属性中；    
- 打开链接：>=iOS 9.0应用内浏览链接；否则使用浏览器打开链接；   
- 打开应用：仅调起应用；
- 自定义行为：将自定义参数返回开发者。 

参数：  

* @param userInfo 将推送回调的userInfo字典回传

注意：       
在iOS 10之前App在前台收到通知时无法在通知栏显示，iOS 10之后App前台时可通过回调completionHandler(UNNotificationPresentationOptionAlert);方法在通知栏显示。    
若用户需要在iOS 10之前系统前台自定义UI展示收到的通知，则在收到通知时调用以下统计方法：

```objectivec
//  App前台 收到推送消息，追踪"App 消息推送"事件
NSDictionary *activityInfo = [EGMonitor receivedRemoteNotification:userInfo];

//  处理点击通知，记录"App通知 点击通知"事件
[EGMonitor clickedRemoteNotification:userInfo];
```

**极光示例**  

```objectivec
//  >= iOS 10 前台收到通知消息    
- (void)jpushNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(NSInteger))completionHandler {
    NSDictionary * userInfo = notification.request.content.userInfo;
    if([notification.request.trigger isKindOfClass:[UNPushNotificationTrigger class]]) {
        [JPUSHService handleRemoteNotification:userInfo];
        
        //  收到推送消息，追踪"App 消息推送"事件    
        NSDictionary *activityInfo = [EGMonitor receivedRemoteNotification:userInfo];
    }
    completionHandler(UNNotificationPresentationOptionBadge|UNNotificationPresentationOptionSound|UNNotificationPresentationOptionAlert); 
}
      
//  >= iOS 10 后台点击进入    
- (void)jpushNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void (^)())completionHandler {
    NSDictionary *userInfo = response.notification.request.content.userInfo;
    if([response.notification.request.trigger isKindOfClass:[UNPushNotificationTrigger class]]) {
        [JPUSHService handleRemoteNotification:userInfo];
        
        //  点击通知栏打开消息，记录"App 点击通知"事件
        [EGMonitor clickedRemoteNotification:userInfo];
    }
    completionHandler();  // 系统要求执行这个方法
}

//  收到通知(>= iOS 7 <iOS 10,默认前台)    
//  后台通知需设置Capabilities 开启Remote notifications    
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler {
    NSLog(@"iOS7及以上系统，收到通知:%@", [self logDic:userInfo]);
    [JPUSHService handleRemoteNotification:userInfo];
    completionHandler(UIBackgroundFetchResultNewData);
    
    if (application.applicationState == UIApplicationStateActive) {
        //  App前台 收到推送消息，追踪"App 消息推送"事件
        NSDictionary *activityInfo = [EGMonitor receivedRemoteNotification:userInfo];
    } else if (application.applicationState == UIApplicationStateBackground) {
        //  App前台 收到推送消息，追踪"App 消息推送"事件
        NSDictionary *activityInfo = [EGMonitor receivedRemoteNotification:userInfo];
    } else {
        //  点击通知栏打开消息，记录"App 点击通知"事件
        [EGMonitor clickedRemoteNotification:userInfo];
    }
}

```

<!--**个推示例**   

```objectivec
#pragma mark - APP运行中接收到通知(推送)处理 - iOS 10以下版本收到推送
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))completionHandler {
 
    [GeTuiSdk handleRemoteNotification:userInfo];
    completionHandler(UIBackgroundFetchResultNewData);
    
    if (application.applicationState == UIApplicationStateActive) {
        //  App前台 收到推送消息，追踪"App 消息推送"事件
        NSDictionary *activityInfo = [EGMonitor receivedRemoteNotification:userInfo];
    } else if (application.applicationState == UIApplicationStateBackground) {
        //  App前台 收到推送消息，追踪"App 消息推送"事件
        NSDictionary *activityInfo = [EGMonitor receivedRemoteNotification:userInfo];
    } else {
        //  点击通知栏打开消息，记录"App 点击通知"事件
        [EGMonitor clickedRemoteNotification:userInfo];
    }
}

#pragma mark - iOS 10中收到推送消息
#if __IPHONE_OS_VERSION_MAX_ALLOWED >= __IPHONE_10_0
//  iOS 10: App在前台获取到通知
- (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions))completionHandler {
    NSDictionary *userInfo = notification.request.content.userInfo;
    
    if([notification.request.trigger isKindOfClass:[UNPushNotificationTrigger class]]) {
        //  收到推送消息，追踪"App 消息推送"事件
        NSDictionary *activityInfo = [EGMonitor receivedRemoteNotification:userInfo];
    }
    // 根据APP需要，判断是否要提示用户Badge、Sound、Alert
    completionHandler(UNNotificationPresentationOptionBadge | UNNotificationPresentationOptionSound | UNNotificationPresentationOptionAlert);
}

//  iOS 10: 点击通知进入App时触发
- (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void (^)())completionHandler {
    NSDictionary *userInfo = response.notification.request.content.userInfo;    
    if([response.notification.request.trigger isKindOfClass:[UNPushNotificationTrigger class]]) {
		// [ GTSdk ]：将收到的APNs信息传给个推统计
        [GeTuiSdk handleRemoteNotification:response.notification.request.content.userInfo];
        
        //  点击通知栏打开消息，记录"App 点击通知"事件
        [EGMonitor clickedRemoteNotification:userInfo];
    }
    completionHandler();
}
#endif

/** SDK收到透传消息回调 */
- (void)GeTuiSdkDidReceivePayloadData:(NSData *)payloadData andTaskId:(NSString *)taskId andMsgId:(NSString *)msgId andOffLine:(BOOL)offLine fromGtAppId:(NSString *)appId {
    // [ GTSdk ]：汇报个推自定义事件(反馈透传消息)
    [GeTuiSdk sendFeedbackMessage:90001 andTaskId:taskId andMsgId:msgId];
    
    // 数据转换
    NSString *payloadMsg = nil;
    if (payloadData) {
        payloadMsg = [[NSString alloc] initWithBytes:payloadData.bytes length:payloadData.length encoding:NSUTF8StringEncoding];

        //  收到推送消息，追踪"App 消息推送"事件
        NSDictionary *dic = [EGMonitor receivedRemoteNotification:payloadMsg];
    }
}
```-->

**百度示例** 

```objectivec
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler {
    
    completionHandler(UIBackgroundFetchResultNewData);
    
    if (application.applicationState == UIApplicationStateActive) {
        //  App前台 收到推送消息，追踪"App 消息推送"事件
        NSDictionary *activityInfo = [EGMonitor receivedRemoteNotification:userInfo];
    } else if (application.applicationState == UIApplicationStateBackground) {
        //  App前台 收到推送消息，追踪"App 消息推送"事件
        NSDictionary *activityInfo = [EGMonitor receivedRemoteNotification:userInfo];
    } else {
        //  点击通知栏打开消息，记录"App 点击通知"事件
        [EGMonitor clickedRemoteNotification:userInfo];
    }
}
```

<!--**小米示例** 

```objectivec
- ( void )application:( UIApplication *)application didReceiveRemoteNotification:( NSDictionary *)userInfo {
    [MiPushSDK handleReceiveRemoteNotification :userInfo];
     
	if (application.applicationState == UIApplicationStateActive) {
	        //  App前台 收到推送消息，追踪"App 消息推送"事件
	        NSDictionary *activityInfo = [EGMonitor receivedRemoteNotification:userInfo];
	        [self showToastWithPara:activityInfo];
	    } else if (application.applicationState == UIApplicationStateBackground) {
	        //  App后台 收到推送消息，追踪"App 消息推送"事件
	        NSDictionary *activityInfo = [EGMonitor receivedRemoteNotification:userInfo];
	        [self showToastWithPara:activityInfo];
	    } else {
	        //  App非活动状态
	        //  点击通知栏打开消息，记录"App 点击通知"事件
	        [EGMonitor clickedRemoteNotification:userInfo];
	    }
 }

// iOS10 应用在前台收到通知
- (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions))completionHandler {
    NSDictionary *userInfo = notification.request.content.userInfo;
    if([notification.request.trigger isKindOfClass:[UNPushNotificationTrigger class]]) {
        [MiPushSDK handleReceiveRemoteNotification:userInfo];
        
        //  收到推送消息，追踪"App 消息推送"事件
        NSDictionary *activityInfo = [EGMonitor receivedRemoteNotification:userInfo];
    }
    // 根据APP需要，判断是否要提示用户Badge、Sound、Alert
    completionHandler(UNNotificationPresentationOptionBadge | UNNotificationPresentationOptionSound | UNNotificationPresentationOptionAlert);
}

// 点击通知进入应用
- (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void (^)())completionHandler {
    NSDictionary *userInfo = response.notification.request.content.userInfo;
    if([response.notification.request.trigger isKindOfClass:[UNPushNotificationTrigger class]]) {
        [MiPushSDK handleReceiveRemoteNotification:userInfo];
        
        //  点击通知栏打开消息，记录"App 点击通知"事件
        [EGMonitor clickedRemoteNotification:userInfo];
    }
    completionHandler();
}

```
-->

## <span id = "attrType">5. 属性数据类型定义
若在页面、事件、用户信息中需添加额外的信息，则可使用带属性参数的接口，同时需在官网您的app中填写“预埋点事件梳理”，并填写对应的附带属性信息，以便进行后续数据处理。

附加属性限制：

* 属性key：必须为`NSString`类型，且长度为255个字符以内
* 属性value：类型必须为以下类型：`NSString`/`NSNumber`/`NSArray`/`BOOL`/`NSDate`，若value为`NSString`类型或`NSArray`类型，其字符长度必须为255个字符以内，且NSArray数组长度不超过100。

若不满足属性限制，数据将会被丢弃。

以下列表列举出了目前支持的所有类型：

数据类型  | iOS说明  |  限制  | 示例 | 备注 | 
-----------|------------|------|------|------|   
字符串      | NSString   | 最长不超过255个字节 | "Analysys" | -- | 
数值型      | double/int/CGFloat等   | -9E15 到 9E15 小数点后最多保留3位 | 10 或 15.5 | 由于属性放在字典中，所以数值类型一般都会使用以下两种方式进行包装：</br>1. attr[@"Age"] = @18; </br>2. attr[@"Price"] = [NSNumber numberWithFloat:100.0]; | 
布尔值      | BOOL   | -- | YES 或 NO | 与数值类型一样，布尔值也需要包装，一般都会使用以下两种方式进行包装：</br> 1.attr[@"HasStock"] = @YES; </br> 2.attr[@"HasStock"] = [NSNumber numberWithBool:NO]; | 
日期时间      | NSDate   | 时间类型,年取值范围是 [1900, 2099]，格式为`yyyy-MM-dd HH:mm:ss`或`yyyy-MM-dd HH:mm:ss.SSS` | "2018-02-09 14:42:18.044" | 如商品7日后送达：</br>attr[@"DeliveryDate"] = [NSDate dateWithTimeIntervalSinceNow:7\*24\*60\*60]; | 
字符串集合    |  NSArray / NSMutableArray  | 数组中所有元素均为字符串类型，且去重后个数不超过100 | [@"Music", @"Dance"] | 与数值类型一样，数组一般使用以下两种方式：</br> 1. attr[@"Hobby"] = @[@"Music", @"Dance"]; </br> 2. NSMutableArray *hobbies = [NSMutableArray arrayWithObjects:@"Music",@"Dance", nil];</br> attr[@"Hobby"] = hobbies; | 

注：
	以上例子以Objective-C为例，swift请使用相应格式转换。
	

## 6. 开启Debug模式

至此SDK已经集成完毕，为了可以快速在页面上看到应用回传的日志，来校验埋点的正确性和完整性，引入了Debug模式。  

```
EGConfigInstance.debugEnable = YES;
```
在此模式下，数据将发送到测试服务器，能够实时看到日志；   

默认debugEnable = No，正式发布APP时，切记确保为No。   

## 7. 验证SDK集成是否正确

通过以下几步完成数据准确性和完整性的验证：

1）开启Debug模式，在真机上运行测试应用，对相关埋点的地方点击操作；   
2）搜索Xcode控制台中查看包含`【 eguanLog 】`字符串，可以看到初始化/发送/发送成功或失败的对应日志；
3）系统会默认校验回传日志是否满足在预埋点事件和预上传用户属性中梳理的需求；将在元事件管理和用户属性管理页面显示校验结果；
4）根据结果提示做相应调整，当状态都为 √ 的时候，表示集成正确，关闭Debug模式，即可发布应用（不同渠道包注意填写不同的渠道ID）。


## <span id = "idfa">附：IDFA说明</span>
若使用易观方舟iOS SDK默认版本会获取IDFA(identifier for advertising)，使开发者更精准的识别用户，提高追踪数据的准确性和稳定性，尤其是对含有广告服务的应用用户追踪有很大的作用。  
但根据AppStore的应用审核规定，拒绝上架采集IDFA但未集成任何广告服务的应用。   
为此，易观方舟同时提供两个版本的iOS SDK，分别是采集IDFA的默认版本和不采集IDFA版本。  

* 若您希望采集IDFA，追踪广告带来的行为，但您的应用未集成任何广告服务时，返回顶部选择下载默认版本。

> 注意：在提交Appstore审核时按照以下方式选择IDFA选项，以防止应用因获取IDFA被AppStore拒绝（若您的应用本身含有广告服务，提交时按应用本身情况选择即可）。
 
![替代文字](http://file1.analysys.cn/fangzhou/3.png)

* 若您不希望采集IDFA，可以点击下载[不含IDFA版](https://ark.analysys.cn/sdk/AnalysysFangzhou_iOS_NoIdfa_SDK_V3.7.9.zip)，集成时可能存在两种情况：  

1)  未集成过默认版本SDK，则在集成不含IDFA版时，无需导入框架AdSupport.framework，其余集成方法同上。  

2)  已集成默认版本SDK，但需要更换成不含IDFA版时，请同时移除应用内的相关库文件: AdSupport.framework。   


