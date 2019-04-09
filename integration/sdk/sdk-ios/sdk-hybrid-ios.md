# iOS Hybrid SDK

## 1. 基本配置

iOS App 中如需加载 H5 页面，可在 H5 页面中以如下方式集成[AnalysysAgent\_Hybrid\_JS\_SDK](https://ark.analysys.cn/sdk/v2/analysys_paas_Hybrid_v4.2.0.1_20190401.zip%20)。

### 1.1 集成 iOS SDK

集成方式查看[iOS SDK 使用说明](https://github.com/larryisthere/ark-docs/tree/03211ca894b85a2ac80a6540af9a600714d71d2c/docs/manual/sdk_ios.md)

### 1.2 配置JS SDK

JS SDK 地址为: `https://ark.analysys.cn/sdk/v2/AnalysysAgent_Hybrid_JS_SDK.min.js`，请在接入JS SDK时更改为实际 JS SDK 地址

将以下JS代码复制到您所需分析页面中的`<head>`和`</head>`标签之间。

```javascript
<script>
    (function(config) {
        window.AnalysysAgent = window.AnalysysAgent || []
        window.AnalysysAgent.methods = 'identify alias reset track profileSet profileSetOnce profileIncrement profileAppend profileUnset profileDelete registerSuperProperty registerSuperProperties unRegisterSuperProperty clearSuperProperties getSuperProperty getSuperProperties pageView debugMode auto appkey name uploadURL hash visitorConfigURL autoProfile autoWebstay encryptType pageProperty duplicatePost'.split(' ');

        function factory(b) {
            return function() {
                var a = Array.prototype.slice.call(arguments);
                a.unshift(b);
                window.AnalysysAgent.push(a);
                return window.AnalysysAgent;
            }
        };
        for (var i = 0; i < AnalysysAgent.methods.length; i++) {
            var key = window.AnalysysAgent.methods[i];
            AnalysysAgent[key] = factory(key);
        }
        for (var key in config) {
            AnalysysAgent[key](config[key])
        }
        var date = new Date();
        var time = new String(date.getFullYear()) + new String(date.getMonth() + 1) + new String(date.getDate());

        var d = document,
            c = d.createElement('script'),
            n = d.getElementsByTagName('script')[0];
        c.type = 'text/javascript';
        c.async = true;
        c.id = 'devSDK';
        c.src = window.location.protocol+'//sdk.analysys.cn/AnalysysAgent_Hybrid_JS_SDK.min.js?' + time //PAAS JS SDK地址
        n.parentNode.insertBefore(c, n);
    })({
        appkey: '' //APPKEY
    })
</script>
```

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
#### 您也可以场景自行设置UA标识"**AnalysysAgent/Hybrid**"，来完成相应需求
{% endhint %}

注意事项： 若项目中需要使用`UserAgent`，则需要使用追加方式，而非直接覆盖已有`UserAgent`

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

[![ ](https://imguserradar.analysys.cn/fangzhou/img/2019/01/201901151711159657.jpeg)](https://ark.analysys.cn/view/sign/signup.html?campaign_id=2111486795&utm_campaign=文档注册&utm_medium=自媒体&utm_source=文档&utm_content=&utm_term=)

