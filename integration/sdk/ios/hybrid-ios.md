# iOS Hybrid SDK

## 1. 基本配置

iOS App 中如需加载 H5 页面，需要同时集成IOS SDK与JS SDK。

### 1.1 集成 iOS SDK

集成方式查看[iOS SDK 使用说明](https://github.com/larryisthere/ark-docs/tree/03211ca894b85a2ac80a6540af9a600714d71d2c/docs/manual/sdk_ios.md)

### 1.2 集成 JS SDK

集成方式查看[JS SDK 使用说明](https://docs.analysys.cn/ark/integration/sdk/js)

## 2. 代码集成

### 2.1 SDK监听拦截URL

H5 页面触发事件时，会把事件发往 iOS App 端，App SDK 端接收到数据后保存并上报。 接口如下：

```objectivec
+ (BOOL)setHybridModel:(id)webView request:(NSURLRequest *)request;
```

* webView: `UIWebView`或`WKWebView`对象
* request: `NSURLRequest`请求对象
* return: 若 js 触发的是 AnalysysAgent 的事件，则`setHybridModel:request:`方法会返回 YES，完成统计；否则返回 NO，不处理。

{% hint style="info" %}
注意:若项目中需要设置`UserAgent，`则需要使用追加方式，请勿覆盖调用setHybridMode接口设置的"**AnalysysAgent/Hybrid**"标识
{% endhint %}

代码参考：

```text
// 获取已有 UserAgent
NSString *userAgent = [webView stringByEvaluatingJavaScriptFromString:@"navigator.userAgent"];
userAgent = [userAgent stringByAppendingString:@"追加信息"];
//  注册到NSUserDefaults
[[NSUserDefaults standardUserDefaults] registerDefaults:@{@"UserAgent": userAgent}];
```

示例：

#### 2.1.1 UIWebView

实现`UIWebView`的`UIWebViewDelegate`方法，并调用统计接口。

```objectivec
- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {

    if ([AnalysysAgent setHybridModel:webView request:request]) {
        NSLog(@"AnalysysAgent 统计完成");
        return NO;
    }

    return YES;
}
```

#### 2.1.2 WKWebView

实现`WKWebView`的`WKNavigationDelegate`代理方法，并调用统计接口。

```objectivec
- (void)webView:(WKWebView *)webView decidePolicyForNavigationAction:(WKNavigationAction *)navigationAction decisionHandler:(void (^)(WKNavigationActionPolicy))decisionHandler {

    if ([AnalysysAgent setHybridModel:webView request:navigationAction.request]) {
        NSLog(@"AnalysysAgent 统计完成");
        decisionHandler(WKNavigationActionPolicyCancel);
        return;
    }

    decisionHandler(WKNavigationActionPolicyAllow);
}
```

### 2.2 重置UserAgent

在`UIWebView`/`WKWebView`结束使用、置为nil或加载`UIWebView`/`WKWebView`的页面`dealloc`时调用。 接口如下：

```objectivec
+ (void)resetHybridModel;
```

示例：

```objectivec
-(void)dealloc {
    [AnalysysAgent resetHybridModel];
}
```

## 3 接口介绍

### 3.1 iOS 接口

与[iOS SDK 使用说明](https://github.com/larryisthere/ark-docs/tree/03211ca894b85a2ac80a6540af9a600714d71d2c/docs/manual/sdk_ios.md)中的接口介绍相同。

### 3.2 JS SDK 接口

与[JS SDK 使用说明](https://github.com/larryisthere/ark-docs/tree/03211ca894b85a2ac80a6540af9a600714d71d2c/docs/manual/sdk_js.md)中的接口介绍相同。

