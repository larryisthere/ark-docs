---
description: 该 SDK 适用于ReactNative 跨平台项目。
---

# ReactNative SDK

## 集成RN模块

### 1、安装SDK RN模块

通过npm集成SDK RN模块

```text
npm install react-native-analysys
```

### 2、链接SDK RN模块

{% hint style="info" %}
React Native 0.60 及以上版本会 autolinking，不需要执行下边的 react-native link 命令。
{% endhint %}

```text
react-native link react-native-analysys
```

### 3、配置 package.json

在 React Native 项目里的 package.json 文件的 script 模块里增加如下配置

```text
"scripts": {
      "postinstall": "node node_modules/react-native-analysys/ansHook.js -run"
}
```

### 4、执行 npm  命令 <a id="id-.ReactNativev1.15-&#x6267;&#x884C;npm&#x547D;&#x4EE4;"></a>

```text
npm install
```

## 集成Android端

### 1、初始化sdk，版本要求4.5.7及以上，参考官方文档[https://docs.analysys.cn/integration/sdk/android](https://docs.analysys.cn/integration/sdk/android)

### 2、设置接口相关Module

```text
public class MainApplication extends Application implements ReactApplication {

  private final ReactNativeHost mReactNativeHost =
      new ReactNativeHost(this) {
        ...
        @Override
        protected List<ReactPackage> getPackages() {
          List<ReactPackage> packages = new PackageList(this).getPackages();
          packages.add(new RNAnalysysAgentPackage());
          return packages;
        }
        ...
      };

  @Override
  public ReactNativeHost getReactNativeHost() {
    return mReactNativeHost;
  }
}
```

## iOS端

### 集成方舟SDK

React Native 0.60 及以上版本可以通过 CocoaPods 的方式引用 RNAnalysysAgentModule 插件及 易观方舟SDK；React Native 0.60 以下版本需要使用手动方式添加。

#### 方式一：CocoaPods 集成

1. 将`npm install`下载后的文件：项目目录/node\_modules/react-native-analysys文件拷贝至ios工程下（一般为.xcodeproj同级目录），在该目录下创建Podfile文件，并配置RN插件，如下示例：

```text
platform :ios, '8.0'
use_frameworks!

target 'YourApp' do
    pod 'RNAnalysysAgentModule', :path => 'react-native-analysys/'
end
```

1. 关闭Xcode，在工程目录下执行`pod install`或`pod install --verbose --no-repo-update`，完成后打开xxx.xcworkspace工程

#### 方式二：手动引入

1. 下载[方舟SDK](https://github.com/analysys/ans-ios-sdk/releases)并导入iOS工程中
2. 将`npm install`下载后的`RNAnalysysAgentModule`插件（路径：项目目录/node\_modules/react-native-analysys\_test/sdk/ios）导入工程
3. 勾选 Copy items if needed、Create groups- Add 完成添加类库
4. 添加依赖库:选择工程 - Targets - “项目名称” - Build Phase - Link Binary With Libraries ：`libz.tbd、libicucore.tbd、libsqlite3.tbd`

### 初始化SDK

在Xcode工程文件`~AppDelegate.m` 中导入头文件`"#import <AnalysysAgent/AnalysysAgent.h>"`，配置 SDK 相关内容。

```text
#import <AnalysysAgent/AnalysysAgent.h>

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    //  设置key，77a52s552c892bn442v721为样例数据，请根据实际情况替换相应内容
    //  AnalysysAgent 配置信息
    AnalysysConfig.appKey = @"77a52s552c892bn442v721";
    // 设置渠道
    AnalysysConfig.channel = @"App Store";
    // 设置追踪新用户的首次属性
    AnalysysConfig.autoProfile = YES;
    // 设置上传数据使用AES加密，需添加加密模块
    //  AnalysysConfig.encryptType = AnalysysEncryptAES;
    //  使用配置初始化SDK
    [AnalysysAgent startWithConfig:AnalysysConfig];
    
    #if DEBUG
    [AnalysysAgent setDebugMode:AnalysysDebugButTrack];
#else
    [AnalysysAgent setDebugMode:AnalysysDebugOff];
#endif
    //  设置上报地址
    [AnalysysAgent setUploadURL:@"https://url:port""];
    
    ...
  return YES;
}

@end
```

## React Native 中 JS 使用

在 js 中获取 `RNAnalysysAgentModule` 模块

```text
//  易观统计模块
import AnalysysAgent from "react-native-analysys";
```

### 接口调用

在相关需要进行统计的部分进行埋点。以点击购买事件为例：

```text
//  事件名称为：buy(购买)  事件附加属性为：ptype(产品分类): iPhone; model(型号): iPhone X
var properties = {
    'ptype': 'iPhone',
    'model': 'Apple iPhoneX'
}
AnalysysAgent.track('buy',properties)
```

