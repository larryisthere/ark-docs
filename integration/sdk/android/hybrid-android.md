# Android Hybrid模式

## 1. SDK集成

Android App 中如需加载 H5 页面，需要同时集成Android SDK与JS SDK。

### 1.1 集成 Android SDK

集成方式查看[Android SDK 使用说明](./)

### 1.2 集成 JS SDK

集成方式查看[JS SDK 使用说明](https://docs.analysys.cn/ark/integration/sdk/js)

{% hint style="info" %}
版本提醒：需使用 Android SDK 4.5.2及 JS SDK 4.5.1以版本
{% endhint %}

## 2. 代码集成

### 2.1 设置Hybrid模式

在初始化 WebView 后，调用setAnalysysAgentHybrid\(\)接口设置Hybrid模式

 接口如下：

```text
// 设置以注入方式实现Hybrid
public static void setAnalysysAgentHybrid(Object webView);
```

* webView ：WebView 对象

### 2.3 重置Hybrid模式

在页面关闭的时候调用。 接口如下：

```text
public static void resetAnalysysAgentHybrid(Object webView);
```

* webView ：WebView对象

示例：

```text
 @Override
  protected void onDestroy() {
    super.onDestroy();
    AnalysysAgent.resetAnalysysAgentHybrid(mWebView);
    mWebView.destroy();
  }
```

## 

