# iOS Hybrid SDK 使用说明

## 1. 基本配置

iOS App 中如需加载 H5 页面，可在 H5 页面中以如下方式集成[AnalysysAgent_Hybrid_JS_SDK](https://ark.analysys.cn/sdk/v2/analysys_Hybrid_v4.1.2_20181228.zip)。

### 1.1 集成 iOS SDK

集成方式查看[iOS SDK 使用说明](./sdk_ios.md)

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

H5 页面触发事件时，会把事件发往 iOS App 端，App SDK 端接收到数据后保存并上报。
接口如下：

```objectivec
+ (BOOL)setHybridModel:(id)webView request:(NSURLRequest *)request;
```

* webView: `UIWebView`或`WKWebView`对象
* request: `NSURLRequest`请求对象
* return: 若 js 触发的是 AnalysysAgent 的事件，则`setHybridModel:request:`方法会返回 YES，完成统计；否则返回 NO，不处理。

<font color="red">注意事项：</font>
若项目中需要使用`UserAgent`，则需要使用追加方式，而非直接覆盖已有`UserAgent`。代码参考：

```
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

在`UIWebView`/`WKWebView`结束使用、置为nil或加载`UIWebView`/`WKWebView`的页面`dealloc`时调用。
接口如下：

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

与[iOS SDK 使用说明](./sdk_ios.md)中的接口介绍相同。

### 3.2 JS SDK 接口

与[JS SDK 使用说明](./sdk_js.md)中的接口介绍相同。

[![ ](https://imguserradar.analysys.cn/fangzhou/img/2019/01/201901151711159657.jpeg)](https://ark.analysys.cn/view/sign/signup.html?campaign_id=2111486795&utm_campaign=%E6%96%87%E6%A1%A3%E6%B3%A8%E5%86%8C&utm_medium=%E8%87%AA%E5%AA%92%E4%BD%93&utm_source=%E6%96%87%E6%A1%A3&utm_content=&utm_term=)