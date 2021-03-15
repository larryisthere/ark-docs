# iOS Hybrid模式

## 1. 基本配置

iOS App 中如需加载 H5 页面，需要同时集成iOS SDK与JS SDK。

### 1.1 集成 iOS SDK

集成方式查看[iOS SDK 使用说明](https://docs.analysys.cn/ark/integration/sdk/ios)

### 1.2 集成 JS SDK

集成方式查看[JS SDK 使用说明](https://docs.analysys.cn/ark/integration/sdk/js)

{% hint style="info" %}
版本提醒：需使用 iOS SDK 4.5.2及 JS SDK 4.5.1以上版本
{% endhint %}

## 2. 代码集成

### 2.1 SDK配置WKWebViewConfiguration接口

初始化WKWebView的configuration对象时，会注入Hybrid方法并配置相关信息。 接口如下：

```objectivec
+ (void)setAnalysysAgentHybrid:(WKWebViewConfiguration *)config；
```

代码参考：

```text
WKWebViewConfiguration config = [[WKWebViewConfiguration alloc] init];
[AnalysysAgent setAnalysysAgentHybrid:config];
```

### 2.2 注销JS消息监听

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

