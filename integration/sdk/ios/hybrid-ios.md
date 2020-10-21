# iOS Hybrid模式

## 1. 基本配置

iOS App 中如需加载 H5 页面，需要同时集成iOS SDK与JS SDK。

### 1.1 集成 iOS SDK

集成方式查看[iOS SDK 使用说明](https://docs.analysys.cn/ark/integration/sdk/ios)

### 1.2 集成 JS SDK

集成方式查看[JS SDK 使用说明](https://docs.analysys.cn/ark/integration/sdk/js)

{% hint style="info" %}
版本提醒：需使用 iOS SDK 4.5.2及 JS SDK 4.5.1以版本
{% endhint %}

## 2. 代码集成

### 2.1 SDK配置WKWebViewConfiguration接口

初始化WKWebView的configuration对象时，会注入Hybrid方法并配置相关信息。 接口如下：

```objectivec
+ (void)setAnalysysAgentHybrid:(WKWebViewConfiguration *)config scriptMessageHandler:(id)handler;
```

* config: `WKWebViewConfiguration`对象
* id: `script message handler`对象

代码参考：

```text
#import <WebKit/WebKit.h>

@interface WeakScriptMessageDelegate : NSObject

@property (nonatomic, weak) id scriptDelegate;

- (instancetype)initWithDelegate:(id)scriptDelegate;

@end

@implementation WeakScriptMessageDelegate

- (instancetype)initWithDelegate:(id)scriptDelegate {
    self = [super init];
    if (self) {
        _scriptDelegate = scriptDelegate;
    }
    return self;
}

- (void)userContentController:(WKUserContentController *)userContentController didReceiveScriptMessage:(WKScriptMessage *)message {
    [self.scriptDelegate userContentController:userContentController didReceiveScriptMessage:message];
}

@end

@interface ANSWKWebViewController ()<WKNavigationDelegate,WKUIDelegate, WKScriptMessageHandler>

@property (nonatomic,strong) WeakScriptMessageDelegate *scriptMessage;

@end

@implementation ANSWKWebViewController

- (WKWebViewConfiguration *)configuration {
    if (!_configuration) {
        _configuration = [[WKWebViewConfiguration alloc] init];
        [AnalysysAgent setAnalysysAgentHybrid:_configuration scriptMessageHandler:self.scriptMessage];
    }
    return _configuration;
}

@end
```

### 2.2 监听JS消息

在`WKWebView 回调的协议方法监听回调。接口如下:`

```text
+ (void)setAnalysysAgentHybridScriptMessage:(WKScriptMessage *)scriptMessage;
```

示例：

```text
#pragma mark - WKScriptMessageHandler

- (void)userContentController:(WKUserContentController *)userContentController didReceiveScriptMessage:(WKScriptMessage *)message {
    [AnalysysAgent setAnalysysAgentHybridScriptMessage:message];
}
```

### 2.3 注销JS消息监听

在`WKWebView`结束使用、置为nil或加载`WKWebView`的页面`dealloc`时调用。 接口如下：

```objectivec
+ (void)resetAnalysysAgentHybrid:(WKWebViewConfiguration *)config;
```

示例：

```objectivec
- (void)dealloc {
    [AnalysysAgent resetAnalysysAgentHybrid:self.configuration];
}
```

