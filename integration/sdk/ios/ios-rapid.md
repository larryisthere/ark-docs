# 快速集成

通过以下方式可以快速完成SDK的集成，更多方式请查SDK集成文档

## 集成配置

{% tabs %}
{% tab title="Cocoapods集成" %}
1. 安装CocoaPods
2. 工程目录下创建`Podfile`文件，并添加`pod 'AnalysysAgent'`，示例如下：

```objectivec
platform :ios, '8.0'
use_frameworks!

target 'YourApp' do
    pod 'AnalysysAgent'
end
```

   3. 关闭Xcode，在工程目录下执行`pod install`或`pod install --verbose --no-repo-update`，完成后打开xxx.xcworkspace工程
{% endtab %}

{% tab title="手动集成" %}
登录GitHub或者Gitee获取SDK Releases 包

打开Xcode

1. 选择 工程 - 右键 - `Add Files to "ProjectName"`
2. 选择 `AnalysysAgent.framework`文件
3. 勾选 `Copy items if needed`、`Create groups`- `Add` 完成添加类库
4. 添加`AnalysysAgent.bundle`资源文件：`Targets`-&gt;`ProjectName` -&gt; `Build Phases` -&gt; `Copy Bundle Resources` -&gt; 添加文件
5. 若使用手动集成，请添加如下依赖框架：

选择工程 - `Targets` - “项目名称” - `Build Phase` - `Link Binary With Libraries` 依赖库如下：

| 类库 | 用途 |
| :--- | :--- |
| `AdSupport.framework` | 广告标识 |
| `CoreTelephony.framework` | 运营商信息 |
| `SystemConfiguration.framework` | 网络状态 |
| `libz.tbd` | 数据压缩（Xcode7以下:libz.dylib） |
| `libicucore.tbd` | websocket可视化连接 |
| `libsqlite3.tbd` | 数据存储 |

* 为支持SDK中category，请在`Targets` - “项目名称” - `Build Settings` - `Other Linker Flags`，添加`-ObjC`选项（注意大小写）
* 若使用 `Swift` 语言工程开发集成，则除上述配置外还需要添加桥接文件，该文件可以使用以下两种方式之一创建：

  * 在工程中添加 `XXX-Bridging-Header.h` 文件（`XXX`为工程名称），并在 `Build Setting` - `Objective-C Bridging Header` 中添加桥接文件路径。
  * 在工程中创建 Objective-C 文件，系统会提示是否创建桥接文件，选择创建即可生成桥接文件

  创建完成之后，在 `XXX-Bridging-Header.h` 文件并引入类库：

  ```text
  #import <AnalysysAgent/AnalysysAgent.h>
  ```
{% endtab %}
{% endtabs %}

## 获取上报地址

登录已经易观方舟系统点击”管理“——”数据接入管理“——”集成SDK接入数据”——“获取数据接收地址”

点击后即可获取您的数据上报地址。

![&#x83B7;&#x53D6;&#x6570;&#x636E;&#x63A5;&#x6536;&#x5730;&#x5740;](../../../.gitbook/assets/wx20200427-181748-2x.png)

## 基础模块介绍

以下接口生效依赖于基础SDK模块，需集成基础SDK相关`AnalysysAgent.framework`文件，请正确集成。

### 初始化接口

在Xcode工程文件`~AppDelegate.m` 中导入头文件`"#import <AnalysysAgent/AnalysysAgent.h>"`。接口如下：

{% hint style="info" %}
请确保初始化SDK在 \(BOOL\)application:\(UIApplication \)application didFinishLaunchingWithOptions:\(NSDictionary\)launchOptions中主线程初始化
{% endhint %}

```objectivec
+ (AnalysysAgent *)startWithConfig:(AnalysysAgentConfig *)config;
```

### 设置上传地址

自定义上传地址，接口设置后，所有事件信息将上传到该地址。接口如下：

```objectivec
+ (void)setUploadURL:(NSString *)uploadURL;
```

### Debug 接口

Debug 接口主要用于开发者测试。可以开/关日志，查看tag为`[analysys]`的Log日志。接口如下：

```objectivec
+ (void)setDebugMode:(AnalysysDebugMode)debugMode;
```

### 初始化示例代码

```objectivec
    //  导入SDK  
#import <AnalysysAgent/AnalysysAgent.h>
    //在主线程中开始初始化代码
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    //  AnalysysAgent 配置信息
    //  设置key，77a52s552c892bn442v721为样例数据，请根据实际情况替换相应内容
    AnalysysConfig.appKey = @"77a52s552c892bn442v721";
    // 设置渠道
    AnalysysConfig.channel = @"App Store";
    //  使用配置初始化SDK
    [AnalysysAgent startWithConfig:AnalysysConfig];
    
    //开启DeBug模式，并在控制台输出日志
    #ifdef DEBUG
        [AnalysysAgent setDebugMode:AnalysysDebugButTrack];
    #else
    //关闭Debug模式，并关闭控制台输入日志
        [AnalysysAgent setDebugMode:AnalysysDebugOff];
    #endif

    //  设置上传地址，http://example.com为您上报地址

    [AnalysysAgent setUploadURL:@"htts://example.com"];
     // 开启全埋点
    [AnalysysAgent setAutoTrackClick:YES];

    return YES;
}
```

## 自定义事件接口

自定义事件接口，可以设置自定义属性。接口如下：

```objectivec
+ (void)track:(NSString *)event;

+ (void)track:(NSString *)event properties:(NSDictionary *)properties;
```

* event：事件ID，以字母或 `$` 开头，只能包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件/属性，最大长度 99字符，不支持乱码和中文
* properties：自定义属性，用于对事件描述。properties最多包含100对，且 key 以字母或 `$` 开头，只能包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件/属性，key长度 1 - 99 字符且不支持乱码和中文；value 支持类型：`NSString`/`NSNumber`/`NSArray<NSString *>`/`NSSet<NSString *>`/`NSDate`/`NSURL`，若为字符串，取值长度为1-255个字符

代码示例：

{% tabs %}
{% tab title="objective-c" %}
```objectivec
//  收藏事件
[AnalysysAgent track:@"collect"];

....

// 用户购买手机
NSMutableDictionary *properties = [NSMutableDictionary dictionary];
properties[@"type"] = @"Phone";
properties[@"name"] = @"iPhone XS Max";
properties[@"money"] = [NSNumber numberWithFloat:9599.0];
properties[@"count"] = @1;
[AnalysysAgent track:@"pay" properties:properties];
```
{% endtab %}

{% tab title="Swift" %}
```objectivec
let properties = ["type": "Phone",
                  "name": "iPhone XS Max",
                 "money": 9599.0,
                 "count": 1] as [String: Any]
AnalysysAgent.track("pay", properties: properties)
```
{% endtab %}
{% endtabs %}



## 账号关联

用户关联的主要作用是打通用户登录前后的行为，做过用户关联的用户在登录前后的行为在方舟系统里面会被认为是一个用户。**建议在用户注册成功或者登录成功后客户端需要调用 alias 接口。**

用户账号关联接口。

```objectivec
+ (void)alias:(NSString *)aliasId;
```

* aliasId：需要关联的用户ID。 取值长度为1-255个字符

代码示例：

{% tabs %}
{% tab title="objective-c" %}
```objectivec
//登陆账号时调用，只设置当前登陆账号即可和之前行为打通
[AnalysysAgent alias:@"sanbo"];
```
{% endtab %}

{% tab title="Swift" %}
```objectivec
//登陆账号时调用，只设置当前登陆账号即可和之前行为打通
AnalysysAgent.alias("zhangsan")
```
{% endtab %}
{% endtabs %}

## 设置用户属性

给用户设置单个或多个属性，如果之前不存在，则新建，否则覆盖。接口如下：

```objectivec
+ (void)profileSet:(NSDictionary *)property;

+ (void)profileSet:(NSString *)propertyName value:(id)propertyValue;
```

* propertyName：属性名称，约束见[属性名称](./#userPropertyKey)
* propertyValue：属性值，约束见[属性值](./#userPropertyValue)
* property：属性列表，约束见[属性名称](./#userPropertyKey)，[属性值](./#userPropertyValue)

代码示例：

{% tabs %}
{% tab title="objective-c" %}
```objectivec
// 设置用户的 `Job` 是 `Engineer`
[AnalysysAgent profileSet:@"Job" propertyValue:@"Engineer"];
```
{% endtab %}

{% tab title="Swift" %}
```
// 统计用户昵称和爱好信息
NSDictionary *properties = @{@"nickName":@"小叮当",@"hobby":@[@"Singing", @"Dancing"]};
[AnalysysAgent profileSet:properties];
//Swift代码示例：
AnalysysAgent.profileSet(["nickName": "小叮当", "hobby": ["Singing", "Dancing"]])
```
{% endtab %}
{% endtabs %}



### 验证数据

用户可以通过Xcode的控制台中查看tag为`[analysys]`的Log日志

![Xcode&#x4E2D;&#x7684;&#x65E5;&#x5FD7;](../../../.gitbook/assets/image%20%28280%29.png)



