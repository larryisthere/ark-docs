---
description: mPaaS架构应用如何使用SDK采集数据
---

# mPaaS SDK

## Android端

将SDK中的aar文件拷贝到`portal`和`bundle`工程的`libs`文件夹中，`portal`为`implementation`，`bundle`为`compileOnly`

```text
portal：implementation fileTree(dir: 'libs', include: ['*.aar'])
bundle：compileOnly fileTree(dir: 'libs', include: ['*.aar'])
```

{% hint style="info" %}
SDK版本需要4.5.4以上
{% endhint %}

在`portal`工程中添加`provider`

```text
<provider
    android:name="com.analysys.database.AnsContentProvider"
    android:authorities="[应用包名].AnsContentProvider"
    android:enabled="true"
    android:exported="false"
    tools:replace="android:authorities"/>
```

将全埋点插件添加到`portal`和`bundle`工程中

```text
项目根build.gradle:
dependencies {
        ...
        classpath 'cn.com.analysys:analysys-allgro-plugin:1.1.2'
 }
 
项目模块build.gradle:
apply plugin: 'com.analysys.android.plugin'
```

在 `LauncherActivityAgent` 的`preInit`函数中初始化方舟sdk

```text
@Override
public void preInit() {
    super.preInit();

    // 初始化方舟sdk
    initAnalysys();

    // 如果使用了H5容器，需要调用此接口初始化Hybrid相关功能
    AnalysysMpaas.init();
}

public static final int DEBUG_MODE = 2;
public static final String APP_KEY = "heatmaptest0916";
public static final String UPLOAD_URL = "http://192.168.220.105:8089";
private static final String SOCKET_URL = "ws://192.168.220.105:9091";
private static final String CONFIG_URL = "http://192.168.220.105:8089";

private void initAnalysys() {
    Context ctx = getApplicationContext();
    AnalysysAgent.setDebugMode(ctx, DEBUG_MODE);
    //  设置 debug 模式，值：0、1、2
    AnalysysConfig config = new AnalysysConfig();
    // 设置key(目前使用电商demo的key)
    config.setAppKey(APP_KEY);
    // 设置渠道
    config.setChannel("AnalysysDemo");
    // 设置追踪新用户的首次属性
    config.setAutoProfile(true);
    // 设置使用AES加密
    config.setEncryptType(EncryptEnum.AES);
    // 设置服务器时间校验
    config.setAllowTimeCheck(true);
    // 时间最大允许偏差为5分钟
    config.setMaxDiffTimeInterval(5 * 60);
    // 开启渠道归因
    config.setAutoInstallation(true);
    // 热图数据采集（默认关闭）
    config.setAutoHeatMap(true);
    // pageView自动上报总开关（默认开启）
    config.setAutoTrackPageView(true);
    // fragment-pageView自动上报开关（默认关闭）
    config.setAutoTrackFragmentPageView(true);
    // 点击自动上报开关（默认关闭）
    config.setAutoTrackClick(true);

    config.setAutoTrackCrash(true);

    config.setAutoTrackDeviceId(true);
    // 初始化
    AnalysysAgent.init(ctx, config);
    AnalysysAgent.setUploadNetworkType(AnalysysAgent.AnalysysNetworkType.AnalysysNetworkWIFI);
    // 设置数据上传/更新地址
    AnalysysAgent.setUploadURL(ctx, UPLOAD_URL);
    // 设置 WebSocket 连接 Url
    AnalysysAgent.setVisitorDebugURL(ctx, SOCKET_URL);
    // 设置配置下发Url
    AnalysysAgent.setVisitorConfigURL(ctx, CONFIG_URL);
}
```

## iOS端

在项目工程Podfile（示例配置如下）所在目录下`pod install`，安装ANSMpaasPlugin

```text
source "https://code.aliyun.com/mpaas-public/podspecs.git"
source 'https://github.com/CocoaPods/Specs.git'
source 'https://github.com/analysys/ANSMpaasPlugin.git'
 
mPaaS_baseline '10.1.68'  # 请将 x.x.x 替换成真实基线版本
mPaaS_version_code 22   # This line is maintained by MPaaS plugin automatically. Please don't modify.
platform :ios, '9.0'
 
target 'ANSMpaasPlugin_Example' do
   
  remove_pod "mPaaS_MBProgressHud"
   
  mPaaS_pod 'mPaaS_Nebula'
  pod 'ANSMpaasPlugin'
  pod 'AnalysysAgent'
end
```

{% hint style="info" %}
SDK版本需要4.5.4以上
{% endhint %}

在`DTFrameworkInterface`分类\(例如:DTFrameworkInterface+ANSMpaasPlugin\_Example\)中导入mPaaS SDK 及 iOS SDK

```text
#import <AnalysysAgent/AnalysysAgent.h>
#import <ANSMpaasPlugin/AnalysysJsApi4EventList.h>
#import <ANSMpaasPlugin/AnalysysJsApi4Hybird.h>
#import <ANSMpaasPlugin/AnalysysJsApi4Property.h>
#import <ANSMpaasPlugin/AnalysysJsApi4Track.h>

@implementation DTFrameworkInterface (ANSMpaasPlugin_Example)
...

- (BOOL)shouldLogReportActive
{
    return YES;
}

- (NSTimeInterval)logReportActiveMinInterval
{
    return 1800;
}

- (BOOL)shouldLogStartupConsumption
{
    return YES;
}

...

@end
```

在`DTFrameworkInterface`分类\(例如:DTFrameworkInterface+ANSMpaasPlugin\_Example\)中 application:beforeDidFinishLaunchingWithOptions:方法内加载自定义插件配置

```text
NSBundle *bundle = [NSBundle bundleForClass:[AnalysysJsApi4EventList class]];
NSString *pluginsJsapisPath = [bundle pathForResource:@"AnalysysWKWebPlugins.bundle/AnalysysWKWebConfig.plist" ofType:nil];
[MPNebulaAdapterInterface initNebulaWithCustomPresetApplistPath:presetApplistPath customPresetAppPackagePath:appPackagePath customPluginsJsapisPath:pluginsJsapisPath];
```

在`DTFrameworkInterface`分类\(例如:DTFrameworkInterface+ANSMpaasPlugin\_Example\)中 application:afterDidFinishLaunchingWithOptions:方法内初始化`AnalysysAgent`

```text
//  部分设置在SDK初始化前设置
[AnalysysAgent setAutomaticCollection:YES];
[AnalysysAgent setAutomaticHeatmap:YES];
[AnalysysAgent setAutoTrackClick:YES];
AnalysysConfig.appKey = @"heatmaptest0916";
AnalysysConfig.channel = @"App Store";
AnalysysConfig.autoProfile = YES;
AnalysysConfig.autoTrackCrash = NO;
AnalysysConfig.autoInstallation = YES;
AnalysysConfig.autoTrackDeviceId = YES;
AnalysysConfig.encryptType = AnalysysEncryptAESCBC128;
AnalysysConfig.allowTimeCheck = YES;
AnalysysConfig.maxDiffTimeInterval = 5 * 60;
[AnalysysAgent startWithConfig:AnalysysConfig];
#if DEBUG
    [AnalysysAgent setDebugMode:AnalysysDebugButTrack];
#else
    [AnalysysAgent setDebugMode:AnalysysDebugOff];
#endif
     
    [AnalysysAgent setUploadURL:@"http://192.168.220.105:8089"];
     
#if DEBUG
    [AnalysysAgent setVisitorDebugURL:@"ws://192.168.220.105:9091"];
#endif
    [AnalysysAgent setVisitorConfigURL:@"http://192.168.220.105:8089"];
 
 
//H5容器配置
// 定制容器
    [MPNebulaAdapterInterface shareInstance].nebulaVeiwControllerClass = [H5WebViewController class]; //设置H5容器基类
    [MPNebulaAdapterInterface shareInstance].nebulaWebViewClass = [H5WKWebView class];
    [MPNebulaAdapterInterface shareInstance].nebulaUserAgent = @"mPaaS/Portal";//设置H5容器UserAgent
    [MPNebulaAdapterInterface shareInstance].nebulaUseWKArbitrary = YES; //开启 WKWebview
    [MPNebulaAdapterInterface shareInstance].nebulaCommonResourceAppList = @[@"77777777"];// 设置全局资源包
    [MPNebulaAdapterInterface shareInstance].nebulaNeedVerify = NO; // 关闭离线包验签，正式版本请开启验签
     
    // 更新离线包
    [[MPNebulaAdapterInterface shareInstance] requestAllNebulaApps:^(NSDictionary *data, NSError *error) {
        NSLog(@"[mpaas] nebula rpc data :%@", data);
    }];
```



