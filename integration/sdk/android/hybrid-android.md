# Android Hybrid模式

## 1. SDK集成

Android App 中如需加载 H5 页面，需要同时集成Android SDK与JS SDK。

### 1.1 集成 Android SDK

集成方式查看[Android SDK 使用说明](./)

### 1.2 集成 JS SDK

集成方式查看[JS SDK 使用说明](https://docs.analysys.cn/ark/integration/sdk/js)

## 2. 代码集成

### 2.1 设置UserAgent

在初始化 WebView 后，调用setHybridModel\(\)接口设置UserAgent

 接口如下：

```text
// 设置UA
public static void setHybridModel(Context context, Object webView);
```

* context ：应用上下文对象
* webView ：WebView 对象

{% hint style="info" %}
注意:若项目中需要设置`UserAgent，`则需要使用追加方式，请勿覆盖调用setHybridMode接口设置的"**AnalysysAgent/Hybrid**"标识
{% endhint %}

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
    mWebView.getSettings().setJavaScriptEnabled(true);
    // 设置UserAgent
    AnalysysAgent.setHybridModel(this, mWebView);
    // 设置WebViewClient
    mWebView.setWebViewClient(new MyWebviewClient());
  }
}
```

{% hint style="info" %}
若项目中已经设置WebViewClient且不能被替换和修改，要启用Hybrid模式必须先调用setHybridModel，然后设置新的WebViewClient，如示例代码
{% endhint %}

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

## 

