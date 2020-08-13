---
description: Flutter SDK 使用说明
---

# Flutter SDK

Flutter SDK 主要用于Flutter语言开发的应用.

{% hint style="info" %}
SDK Releases包下载：  
Github地址\(推荐\)：[https://github.com/analysys/ans-flutter-sdk/releases](https://github.com/analysys/ans-flutter-sdk/releases)  
Gitee地址：[https://gitee.com/Analysys/ans-flutter-sdk/releases](https://gitee.com/Analysys/ans-flutter-sdk/releases)  
Releases中含有更新说明请您阅读，接口使用请参考本文档。
{% endhint %}

## 移动端iOS & Android Flutter插件使用说明

方舟argo\_flutter\_plugin插件主要对移动端iOS和Android两个平台常用接口的封装，支持常用埋点事件的统计上报。

## 插件安装

在 Flutter 项目下 `pubspec.yaml` 文件添加 `argo_flutter_plugin` 依赖，配置如下：

```text
dependencies:
  ## 易观方舟flutter插件
  argo_flutter_plugin: any

安装插件：

flutter packages get

```

## SDK初始化

### Android 端

建议在应用 Application 中调用 SDK 初始化接口 init\(\)， 配置 AppKey、Channel

```text
import com.analysys.AnalysysAgent;
import com.analysys.AnalysysConfig;
import com.analysys.EncryptEnum;

import io.flutter.app.FlutterApplication;

public class AnalysysApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        
        // 初始化SDK
        AnalysysConfig config = new AnalysysConfig();
        config.setAppKey("77a52s552c892bn442v721");
        config.setChannel("豌豆荚");
        AnalysysAgent.init(this, config);
        
        // 设置 打开 debug 模式 0：关闭 Debug 1：打开 Debug数据仅调试 2：打开Debug数据计入平台统计
 注意：若设置其他值则不生效,使用默认值
        AnalysysAgent.setDebugMode(this, 2);
        // 设置自定义上传地址为 scheme://host + :port
        AnalysysAgent.setUploadURL(mContext,"/*设置为实际地址*/");
    }
}
```

### iOS 端

在Xcode工程文件`~AppDelegate.m` 中导入头文件`#import <AnalysysAgent/AnalysysAgent.h>`，并在入口函数`- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions`中进行SDK初始化：

```text
//  导入SDK
#import <AnalysysAgent/AnalysysAgent.h>

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
     //  初始化SDK
    AnalysysConfig.appKey = <#appkey#>;
    AnalysysConfig.channel = @"App Store";
    [AnalysysAgent startWithConfig:AnalysysConfig];
    
    #ifdef DEBUG
        [AnalysysAgent setDebugMode:AnalysysDebugButTrack];
    #endif
    //  设置上传地址
    [AnalysysAgent setUploadURL:@"https://url:port"];

    return YES;
}
```

## 插件使用

在所需代码dart文件中引入插件：

`import 'package:argo_flutter_plugin/argo_flutter_plugin.dart';`

## API接口

### 统计事件

事件跟踪，设置事件名称和事件详细信息。接口如下：

```dart
Map<String, Object> properties = {"productCategory": "iPhone X", "price": 6000};
AnalysysAgent.track("purchase", properties);
```

### 用户属性设置

SDK提供以下接口供用户设置用户的属性，比如用户的年龄/性别等信息。

```dart
Map<String, Object> properties = {"nickName":"小叮当", "hobby": ["Singing", "Dancing"]};
AnalysysAgent.profileSet(properties);
```

### 通用属性设置

如果某个事件的属性，在所有事件中都会出现，则这个属性可以做为通用属性，通过 registerSuperProperties\(\) 将该属性设置为事件通用属性，则设置后每次发送事件的时候都会带有该属性。比如用户浏览/购买商品过程中的用户等级就可以作为通用属性。

```dart
Map<String, Object> properties = {"birthday": "2010-01-01"};
AnalysysAgent.registerSuperProperties(properties);
```

### 用户关联

用户 ID 关联接口。将 用户ID和匿名ID关联，计算时会认为是一个用户的行为。该接口是在匿名ID发生变化的时候调用，来告诉 SDK 匿名ID变化前后的 ID 对应关系。该场景一般应用在用户注册/登录的过程中。比如：一个匿名用户浏览商品，系统为其分配的distinctId = "1234567890987654321"，随后该匿名用户进行注册，系统为其分配了新的注册 ID，aliasId = "ABCDEF123456789"，此时就需要调用 alias 接口对两个 ID 进行关联。接口如下：

```dart
AnalysysAgent.alias('A');
```

其他接口请参iOS& Android 接口

