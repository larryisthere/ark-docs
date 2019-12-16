---
description: JS SDK 其他模块介绍
---

# 其他模块

## 加密模块

加密模块SDK，根据用户初始化SDK时参数encryptType的值对上报日志进行对应加密。  
使用加密模块SDK时，需确保证该插件在基础SDK文件前引入

```javascript
//同步集成
<script type="text/javascript" src="/*设置为非ES6加密模块SDK实际存放地址*/"></script>
...//其他SDK代码


//ES6集成
import '设置为ES6加密模块SDK实际存放地址'
...//其他SDK代码
```

## GBK转码模块SDK

GBK转码模块SDK，针对符合UTM的参数值如为GBK/GB2312编码格式，进行UTF-8编码转换。  
使用加密模块SDK时，需确保证该插件在基础SDK文件前引入

```javascript
//同步集成
<script type="text/javascript" src="/*设置为非ES6GBK转码模块SDK实际存放地址*/"></script>
...//其他SDK代码


//ES6集成
import '设置为ES6GBK转码模块SDK实际存放地址'
...//其他SDK代码
```

## 可视化埋点介绍

### 接入 JS SDK 可视化模块

将 `AnalysysAgent_JS_SDK_VISUAL.min.js` 放到与 JS SDK`AnalysysAgent_JS_SDK.min.js` 文件存放的同一文件目录中。或通过：`SDKFileDirectory`接口指定SDK目录。

### 设置请求埋点配置地址

```javascript
{
    visitorConfigURL:"/*设置为实际地址*/"
}
```

### 允许 iframe 加载

使用可视化埋点功能需要使用 `iframe` 来加载您的网站。如果您的网站禁止了 `iframe` 加载，就无法使用可视化埋点功能，需要在服务器配置中设置 `X-Frame-Options` 允许 `iframe` 加载。

```javascript
//配置nginx中 X-Frame-Options 响应头
//您的网站为https协议，https://example.com/为您访问的方舟的域名
add_header X-Frame-Options: Allow-From https://example.com
//您的网站为http协议，http://example.com为您访问的方舟的域名
add_header X-Frame-Options: Allow-From http://example.com
```

{% hint style="info" %}
注意：

X-Frame-Options更多使用方式请参考：[X-Frame-Options 响应头](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/X-Frame-Options)

可视化埋点时候需要方舟与埋点页面协议头相同。否则会被浏览器拒绝，无法进行可视化埋点。
{% endhint %}



### window对象

以下 window 对象中的属性被复写后将导致可视化埋点功能无法正常使用。

```javascript
window.top
window.parent
window.name
window.location
```

## 热图模块介绍

将 `AnalysysAgent_JS_SDK_HEATMAP.min.js` 放到与 JS SDK`AnalysysAgent_JS_SDK.min.js` 文件存放的同一文件目录中，展示热图的时会自动调用。或通过：`SDKFileDirectory`接口指定SDK目录。

