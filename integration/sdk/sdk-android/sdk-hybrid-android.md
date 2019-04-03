# Android Hybrid SDK

## 1. 基本配置

Android App中如需加载H5页面，可在H5页面中以如下方式集成[AnalysysAgent\_Hybrid\_JS\_SDK](https://ark.analysys.cn/sdk/v2/analysys_paas_Hybrid_v4.2.0.1_20190401.zip%20)。

### 1.1 集成 Android SDK

集成方式查看[Android SDK 使用说明](./)

### 1.2 配置JS SDK

PAAS JS SDK地址为: `http://ark.analysys.cn/sdk/v2/AnalysysAgent_Hybrid_JS_SDK.min.js`，请在接入PAAS JS SDK时更改为实际PAAS JS SDK地址

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

### 2.1 设置UserAgent

在初始化 WebView 后，调用setHybridModel\(\)接口设置UserAgent。 注意:如项目中需要自定义设置UA，请追加UA。将setHybridModel\(\)接口放到 部分UserAgent设置请 接口如下：

```text
// 设置UA
public static void setHybridModel(Context context, Object webView);
```

* context ：应用上下文对象
* webView ：WebView 对象

示例：

```text
public class WebViewDemo extends AppCompatActivity {
  private WebView mWebView;
  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main_webview_demo);
    mWebView = (WebView) findViewById(R.id.wv_main);
    mWebView.loadUrl("file:///android_asset/index.html");
    mWebView.setWebViewClient(new MyWebviewClient());
    mWebView.getSettings().setJavaScriptEnabled(true);
    // 设置UserAgent
    AnalysysAgent.setHybridModel(this, mWebView);
  }
}
```

### 2.2 SDK监听拦截URL

当调用`设置UserAgent`后，H5 页面触发事件时，会把事件发往 App 端，App SDK 端接收到数据后保存并上报。 接口如下：

```text
public static void interceptUrl(Context context, String url, Object webView)；
```

* context ： 应用上下文对象
* url ：URL地址
* webView ：WebView对象

示例：

```text
 class MyWebviewClient extends WebViewClient {
    @Override
    public void onPageFinished(WebView view, String url) {
      Log.d("analysys.hybrid", "onPageFinished url:" + url);
    }
    @Override
    public boolean shouldOverrideUrlLoading(WebView view, String url) {
      // 设置URL拦截
      AnalysysAgent.interceptUrl(WebViewDemo.this, url, view);
      return false;
    }
  }
```

### 2.3 重置UserAgent

撤销在“设置UserAgent”接口中对UserAgent做的修改重置UserAgent，在页面关闭的时候调用。 接口如下：

```text
public static void resetHybridModel(Context context, Object webView);
```

* context ：应用上下文对象
* webView ：WebView对象

示例：

```text
 @Override
  protected void onDestroy() {
    super.onDestroy();
    mWebView.removeAllViews();
    mWebView.destroy();
    AnalysysAgent.resetHybridModel(this, mWebView);
  }
```

## 3 接口介绍

### 3.1 Android 接口

与[Android SDK 使用说明](./)中的接口介绍相同。

### 3.2 JS SDK 接口

与[JS SDK 使用说明](../sdk-js.md)中的接口介绍相同。

[![ ](https://imguserradar.analysys.cn/fangzhou/img/2019/01/201901151711159657.jpeg)](https://ark.analysys.cn/view/sign/signup.html?campaign_id=2111486795&utm_campaign=文档注册&utm_medium=自媒体&utm_source=文档&utm_content=&utm_term=)

