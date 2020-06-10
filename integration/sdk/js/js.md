---
description: 标准版本适用方舟4.3.3以上版本
---

# JS SDK标准版

## JS SDK 使用说明

JS SDK 用于由 HTML 、 Css 及 Javascript 制作成的网站，集成前请先下载 SDK

{% hint style="info" %}
SDK Releases包下载：  
Github地址\(推荐\)：[https://github.com/analysys/ans-javascript-sdk/releases](https://github.com/analysys/ans-javascript-sdk/releases)  
Gitee地址：[https://gitee.com/Analysys/ans-javascript-sdk/releases](https://gitee.com/Analysys/ans-javascript-sdk/releases)  
Releases中含有更新说明请您阅读，接口使用请参考本文档。
{% endhint %}

| js文件 | 功能描述 | 是否必须 | 服务端版本 |
| :---: | :---: | :---: | :--- |
| AnalysysAgent\_JS\_SDK.min.js | 基础模块SDK | 非ES6必须 | 全部 |
| AnalysysAgent\_JS\_SDK.es6.min.js | 基础模块SDK | ES6必须 | 全部 |
| AnalysysAgent\_JS\_SDK.amd.min.js | 基础模块SDK | AMD必须 | 全部 |
| AnalysysAgent\_JS\_SDK\_VISUAL.min.js | 可视化模块SDK | 可选 | 全部 |
| AnalysysAgent\_JS\_SDK\_HEATMAP.min.js | 热图模块SDK | 可选 | 4.3.0及以上 |
| AnalysysAgent\_Encrypt.min.js | 加密模块SDK | 非ES6可选 | 全部 |
| AnalysysAgent\_Encrypt.es6.min.js | 加密模块SDK | ES6可选 | 全部 |
| AnalysysAgent\_Encrypt.amd.min.js | 加密模块SDK | AMD可选 | 全部 |
| AnalysysAgent\_GBK.min.js | GBK转码模块SDK | 非ES6可选 | 全部 |
| AnalysysAgent\_GBK.es6.min.js | GBK转码模块SDK | ES6可选 | 全部 |
| AnalysysAgent\_GBK.amd.min.js | GBK转码模块SDK | AMD可选 | 全部 |
| AnalysysAgent\_PageViewStayTime.min.js | 页面访问时长模块SDK | 非ES6可选 | 4.4.1及以上 |
| AnalysysAgent\_PageViewStayTime.es6.min.js | 页面访问时长模块SDK | ES6可选 | 4.4.1及以上 |
| AnalysysAgent\_PageViewStayTime.amd.min.js | 页面访问时长模块SDK | AMD可选 | 4.4.1及以上 |

### 快速集成

如果您是第一次使用易观方舟产品，可以通过阅读本文快速了解此产品

#### 1. 选择集成方式

目前我们提供了异步集成、同步集成等多种集成方式

#### 2. 设置初始化接口

通过初始化代码的配置参数配置您的 AppKey

#### 3. 设置上传地址

通过初始化代码的配置参数 uploadURL 设置您上传数据的地址。

#### 4. 设置需要采集的页面或事件

通过手动埋点，设置需要采集的页面或事件。

#### 5. 打开Debug模式查看日志

通过设置 Ddebug 模式，开/关 log 查看日志。

通过以上5步您即可验证 SDK 是否已经集成成功。更多接口说明请您查看 API 文档。

## 集成配置

{% hint style="info" %}
**有多种集成方式：**

**1 同步加载**：将 SDK 作为用户网站资源，与用户网站资源一同根据页面结构从上到下按照顺序加载，直至资源加载都完毕后，运行相关 JS 代码；

**2 异步加载**：不参与用户网站资源加载，等待用户网站资源加载完毕，运行 JS 代码时，动态插入script 标签，进行 SDK 的加载，加载完毕后运行 JS 相关代码。

**二者比较**：

* 异步加载晚于同步加载，所以异步加载统计到的启动与页面打开事件要稍晚于同步加载；时间差 ≈ 异步加载时SDK加载时间
* 对于页面打开后快速跳转的网页，建议使用同步加载，因为该类网页有可能会在异步加载SDK未结束前跳转网页
{% endhint %}

{% tabs %}
{% tab title="异步集成（推荐）" %}
将以下JS代码复制到您所需分析页面中的`<head>`和`</head>`标签之间。只要保证在以下代码之下即可。无需等待`window.onload`之后再执行。

```javascript
<script>
    (function(c) {
        window.AnalysysAgent = window.AnalysysAgent || {}
        var a = window.AnalysysAgent || {}
        var ans = ['identify', 'alias', 'reset', 'track', 'profileSet', 'profileSetOnce', 'profileIncrement', 'profileAppend', 'profileUnset', 'profileDelete', 'registerSuperProperty', 'registerSuperProperties', 'unRegisterSuperProperty', 'clearSuperProperties', 'getSuperProperty', 'getSuperProperties', 'pageView', 'getDistinctId']
        a['config'] = c
        a['param'] = []
        function factory (b) {
          return function () {
            a['param'].push([b, arguments])
            return window.AnalysysAgent
          }
        }
        for (var i = 0; i < ans.length; i++) {
          a[ans[i]] = factory(ans[i])
        }
        if (c.name) {
          window[c.name] = a
        }
        var date = new Date();
        var time = new String(date.getFullYear()) + new String(date.getMonth() + 1) + new String(date.getDate());

        var d = document,
            c = d.createElement('script'),
            n = d.getElementsByTagName('script')[0];
        c.type = 'text/javascript';
        c.async = true;
        c.id = 'ARK_SDK';
        c.src = '/*设置为JS SDK实际存放地址*/' +'?v=' +time; //JS SDK存放地址
        n.parentNode.insertBefore(c, n);
    })({
        appkey: '/*设置为实际APPKEY*/', //APPKEY
        uploadURL: '/*设置为实际地址*/'//上传数据的地址
    })
</script>

```
{% endtab %}

{% tab title="同步集成" %}
将以下 JS 代码复制到您所需分析页面中的`<head>`和`</head>`标签之间。无需等待`window.onload`之后再执行。

```javascript
//建议在script标签设置id为ARK_SDK,该ID用来引导可视化模块与热图模块的加载
<script type="text/javascript" id="ARK_SDK" src="/*设置为JS SDK实际存放地址*/"></script>
//初始化JS SDK
<script>
window.AnalysysAgent.init({
    appkey: '/*设置为实际APPKEY*/', //APPKEY
    uploadURL: '/*设置为实际地址*/'//上传数据的地址
})
</script>
```
{% endtab %}

{% tab title="ES6方式集成" %}
从 npm 获取 SDK ， npm install ans-javascript-sdk。也可以自行下载SDK。

```javascript
// 从 npm 获取 sdk

import  AnalysysAgent from 'ans-javascript-sdk/SDK/AnalysysAgent_JS_SDK.es6.min.js'

//自行下载SDK

//import AnalysysAgent from '/*设置为JS SDK ES6模块实际存放地址*/'

AnalysysAgent.init({
    appkey: '/*设置为实际APPKEY*/',//APPKEY
    uploadURL: '/*设置为实际地址*/',//上传数据的地址
    SDKFileDirectory: '/*设置为可视化与热图模块SDK实际存放目录*/'//可视化与热图模块SDK存放目录。
})


```
{% endtab %}

{% tab title="CommonJS 规范集成" %}
从 npm 获取 sdk ， `npm install ans-javascript-sdk`

```javascript
var AnalysysAgent = require('ans-javascript-sdk')
AnalysysAgent.init({
    appkey: '/*设置为实际APPKEY*/',//APPKEY
    uploadURL: '/*设置为实际地址*/',//上传数据的地址
    SDKFileDirectory: '/*设置为可视化与热图模块SDK实际存放目录*/'//可视化与热图模块SDK存放目录。
})

```
{% endtab %}

{% tab title="AMD 规范集成（以 RequireJS 为例）" %}
自行下载SDK。获取AnalysysAgent\_JS\_SDK.amd.min.js，假设该文件放到与 require.js 同一目录中

```javascript
requirejs(["./AnalysysAgent_JS_SDK.amd.min"], function(AnalysysAgent) {
           AnalysysAgent.init({
                appkey: '/*设置为实际APPKEY*/',//APPKEY
                uploadURL: '/*设置为实际地址*/',//上传数据的地址
                SDKFileDirectory: '/*设置为可视化与热图模块SDK实际存放目录*/'//可视化与热图模块SDK存放目录。
            })
        });

```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
请不要修改以上初始化代码中的代码逻辑，可能会造成SDK无法正常运行。
{% endhint %}

### 配置参数

* _appkey_\(必须\) 在网站获取的 AppKey
* _debugMode_ 设置调试模式：0 - 关闭调试模式\(默认\)；1 - 开启调试模式，数据不入库；2 - 开启调试模式，数据入库
* _uploadURL_\(必须\) 设置上传数据接口
* _visitorConfigURL_\(如使用可视化埋点，则必须\) 设置可视化配置获取接口
* _name_ 设置 JS SDK 全局对象别名
* _auto_ 设置自动采集页面打开事件：false - 关闭自动采集；true - 开启自动采集\(默认\)
* _autoTrack_ 设置是否启用全埋点功能：false - 不启用全埋点功能\(默认\)；true - 启用全埋点功能（SDK 版本 4.4.0 及以后支持）
* _autoHeatmap_ 设置是否启用热图功能：false - 不启用热图功能\(默认\)；true - 启用热图功能
* autoWebstay 在开启热图功能\(autoHeatmap设置为true\)时，设置是否追踪页面滚动行为：false - 在开启热图功能\(autoHeatmap设置为true\)时，不追踪页面滚动行为；true - 在开启热图功能\(autoHeatmap设置为true\)时，追踪页面滚动行为\(默认\)
* _hash_ 设置检测 url hash 变化：false - 关闭监测url hash变化；true - 开启监测url hash变化\(默认\)
* _autoProfile_ 设置是否追踪新用户的首次属性：false - 不追踪新用户的首次属性；true - 追踪新用户的首次属性\(默认\)
* _encryptType_ 设置是否对上传数据加密：0 - 对上传数据不加密\(默认\)；1 - 对上传数据进行AES 128位ECB加密；2 对上传数据进行AES 128位CBC加密
* pageProperty 设置自动采集时页面自定义属性；类型：Object
* pageViewBlackList 设置页面统计黑名单；类型：String/内部元素为String或Function的Array/Function；（SDK 版本 4.4.0 及以后支持）
* heatMapBlackList 设置热图统计黑名单；类型：String/内部元素为String或Function的Array/Function；（SDK 版本 4.4.0 及以后支持）
* autoClickBlackList 设置全埋点统计黑名单；类型：String/内部元素为String或Function的Array/Function；（SDK 版本 4.4.0 及以后支持）
* SDKFileDirectory 设置可视化模块SDK与热图模块SDK存放目录。类型：String；（SDK 版本 4.4.0 及以后支持）
* _sendType_ 设置上传日志方式。'img' - 使用image标签的图片链接地址上传日志\(默认\)；'post'-使用Ajax中的post请求上传日志
* _webstayDuration_ 设置追踪页面滚动行为时，最大停留时长。默认值：5小时。类型：Number。单位：毫秒
* _cross\_subdomain 设置在二级域名下存储cookie：false - 在自身域名下存储cookie；true - 在二级域名下存储cookie\(默认\)_

#### appkey

appkey 为在网站获取的 AppKey。

* value 在网站获取的 AppKey。类型:String。取值长度 1 - 255 字符。

```javascript
// 设置key，77a52s552c892bn442v721为样例数据，请根据实际情况替换相应内容
{
    appkey:"77a52s552c892bn442v721"
}
```

#### debugMode

debugMode debug 模式，默认关闭状态。发布版本时 debugMode 模式设置为`0`

* 0 关闭调试模式\(默认\)。类型：Number。
* 1 开启调试模式，数据不入库。类型：Number。
* 2 开启调试模式，数据入库。类型：Number。

```javascript
//开启调试模式且数据不入库
{
    debugMode:1
}
//开启调试模式且数据入库
{
    debugMode:2
}
//关闭调试模式
{
    debugMode:0 //或删除debugMode参数
}
```

或删除 debugMode 参数。

#### uploadURL

uploadURL 为自定义上传地址，参数设置后，所有事件信息将上传到该地址。

* value 类型：String。数据上传地址，格式为 scheme://host + :port\(不包含/后的内容\)。scheme 必须以 http:// 或 https:// 开头，host 只支持域名和 IP，取值长度 1 - 255字符，port 端口号必须携带

```javascript
//设置自定义上传地址为 scheme://host + :port
{
    uploadURL:"/*设置为实际地址*/"
}
```

#### visitorConfigURL

visitorConfigURL 为获取可视化埋点配置信息的服务器地址，参数设置后，将从该服务器地址获取可视化埋点配置信息。

* value 类型：String。获取可视化埋点配置信息的服务器地址，格式为 scheme://host + :port\(不包含/后的内容\)。scheme 必须以 http:// 或 https:// 开头，host 只支持域名和 IP，取值长度 1 - 255字符，port 端口号必须携带

```javascript
//设置自定义上传地址为 scheme://host + :port
{
    visitorConfigURL:"/*设置为实际地址*/"
}
```

#### name

name 为设置 JS SDK全局对象的别名。可根据自身所需进行更改。

* value 类型：String。取值长度 1 - 255字符。

```javascript
//设置JS SDK全局对象的别名为analysys，之后可在全局使用analysys来调用JS SDK所有的API方法。
{
    name:"analysys"
}
```

#### auto

auto 为设置打开/关闭自动采集页面的参数。可根据自身需要进行更改。

* true 开启自动采集页面打开事件\(默认\)。类型：Boolean。
* false 关闭自动采集页面打开事件。类型：Boolean。

```javascript
//关闭自动采集页面打开事件，关闭后可使用JS SDK的API中的手动发送页面打开数据方法，来发送页面打开状态的数据。
{
    auto:false
}
//开启自动采集页面打开事件。
{
    appkey:"77a52s552c892bn442v721",
    auto:true//或删除auto参数。
}
```

#### hash

针对单页面网站的 $pageview 的采集，在将 hash 设置 true 后，可以自动监测 URL 的变化，改变时自动上报 $pageview 事件。

* true，默认，类型：Boolean。
* false，默认值，类型：Boolean。

```javascript
//关闭自动采集监测 URL 变化，关闭后可使用JS SDK的手动上报页面浏览的方法上报 $pageview 事件。
{
    hash:false
}
//开启自动采集监测 URL 变化。
{
    hash:true
}
```

#### autoProfile

autoProfile 为设置是否追踪新用户的首次属性。可根据自身需要进行更改。

* true 追踪新用户的首次属性\(默认\)。类型：Boolean。
* false 不追踪新用户的首次属性。类型：Boolean。

```javascript
//不追踪新用户的首次属性，新用户首次打开网站不上传新用户的首次属性。
{
    autoProfile:false
}
//追踪新用户的首次属性，新用户首次打开网站上传新用户的首次属性。
{
    autoProfile:true//或删除autoProfile参数。
}
```

#### autoWebstay

autoWebstay 为设置是否追踪页面滚动行为。可根据自身需要进行更改。

* true 追踪页面滚动行为。类型：Boolean。
* false 不追踪页面滚动行为\(默认\)。类型：Boolean。

```javascript
//不追踪页面滚动行为。
{
    autoWebstay:false//或删除autoWebstay参数。
}
//追踪页面滚动行为。
{
    autoWebstay:true
}
```

#### autoTrack

autoTrack 为设置是否启用全埋点功能。可根据自身需要进行更改。true 启用热图功能。类型：Boolean。

* true 启用全埋点功能。类型：Boolean。
* false 不启用全埋点功能\(默认\)。类型：Boolean。

```text
//不启用全埋点功能。
{
    autoTrack:false//或删除autoTrack参数。
}
//启用全埋点功能。
{
    autoTrack:true
}
```

#### autoHeatmap

autoHeatmap 为设置是否启用热图功能。可根据自身需要进行更改。

* true 启用热图功能。类型：Boolean。
* false 不启用热图功能\(默认\)。类型：Boolean。

```text
//不启用热图功能。
{
    autoHeatmap:false//或删除autoHeatmap参数。
}
//启用热图功能。
{
    autoHeatmap:true
}
```

#### encryptType

encryptType 为设置数据上传时的加密方式,目前只支持 AES 加密，如不设置此参数，数据上传不加密。。可根据自身需要进行更改。

* 0 对上传数据不加密\(默认\)。类型：Number。
* 1 对上传数据 AES 加密。类型：Number。
* 2 对上传数据进行AES 128位CBC加密。类型：Number。

```javascript
//对上传数据不加密。
{
    encryptType:0//或删除encryptType参数。
}
//对上传数据AES加密。
{
    encryptType:1
}
//对上传数据AES加密。
{
    encryptType:2
}

```

#### pageProperty

pageProperty 为设置自动采集时页面自定义属性。可根据自身需要进行增加。

* 类型：JSON。

```javascript
//设置自动采集时页面自定义属性
{
    pageProperty:{
        title:'首页'
    }
}
```

#### pageViewBlackList

pageViewBlackList 为设置页面统计黑名单。根据设置不在采集设置中的页面内所有的页面事件，包含用户手动调用pageView API。（SDK 版本 4.4.0 及以后支持） 可根据自身需要进行增加。

* 类型：String/内部元素为String或Function的Array/Function。

```javascript
//设置页面地址`url`为https://xxx.xx.com不进行页面事件采集
{
    pageViewBlackList:'https://xxx.xx.com'
}
//设置页面地址`url`为https://xxx.xx.com或https://xxx.xx.com/x不进行页面事件采集
{
    pageViewBlackList:['https://xxx.xx.com','https://xxx.xx.com/x']
}
//设置页面地址`url`内包含'index'的页面不进行页面事件采集
{
    pageViewBlackList: function() {
        var url = window.location.href
        if(url.indexOf('index') > -1){
            return true
        }
        return false
    }
}
//设置页面地址`url`内包含'index'或页面地址`url`为https://xxx.xx.com的页面不进行页面事件采集
{
    pageViewBlackList: [
    'https://xxx.xx.com',
    function() {
        var url = window.location.href
        if(url.indexOf('index') > -1){
            return true
        }
        return false
    }]
}

```

#### heatMapBlackList

heatMapBlackList 为设置热图统计黑名单。根据设置不在采集设置中的页面内所有的热图事件。（SDK 版本 4.4.0 及以后支持） 可根据自身需要进行增加。

* 类型：String/内部元素为String或Function的Array/Function。

```javascript
//设置页面地址`url`为https://xxx.xx.com不进行热图事件采集
{
    heatMapBlackList:'https://xxx.xx.com'
}
//设置页面地址`url`为https://xxx.xx.com或https://xxx.xx.com/x不进行热图事件采集
{
    heatMapBlackList:['https://xxx.xx.com','https://xxx.xx.com/x']
}
//设置页面地址`url`内包含'index'的页面不进行热图事件采集
{
    heatMapBlackList: function() {
        var url = window.location.href
        if(url.indexOf('index') > -1){
            return true
        }
        return false
    }
}
//设置页面地址`url`内包含'index'或页面地址`url`为https://xxx.xx.com的页面不进行热图事件采集
{
    heatMapBlackList: [
    'https://xxx.xx.com',
    function() {
        var url = window.location.href
        if(url.indexOf('index') > -1){
            return true
        }
        return false
    }]
}

```

#### autoClickBlackList

autoClickBlackList 为设置全埋点统计黑名单。根据设置不在采集设置中的页面内所有的全埋点事件。（SDK 版本 4.4.0 及以后支持） 可根据自身需要进行增加。

* 类型：String/内部元素为String或Function的Array/Function。

```javascript
//设置页面地址`url`为https://xxx.xx.com不进行全埋点事件采集
{
    autoClickBlackList:'https://xxx.xx.com'
}
//设置页面地址`url`为https://xxx.xx.com或https://xxx.xx.com/x不进行全埋点事件采集
{
    autoClickBlackList:['https://xxx.xx.com','https://xxx.xx.com/x']
}
//设置页面地址`url`内包含'index'的页面不进行全埋点事件采集
{
    autoClickBlackList: function() {
        var url = window.location.href
        if(url.indexOf('index') > -1){
            return true
        }
        return false
    }
}
//设置页面地址`url`内包含'index'或页面地址`url`为https://xxx.xx.com的页面不进行全埋点事件采集
{
    autoClickBlackList: [
    'https://xxx.xx.com',
    function() {
        var url = window.location.href
        if(url.indexOf('index') > -1){
            return true
        }
        return false
    }]
}

```

#### SDKFileDirectory

SDKFileDirectory 设置可视化模块SDK与热图模块SDK存放目录。可根据自身需要进行增加。（SDK 版本 4.4.0 及以后支持）

* 类型：JSON。

```javascript
//设置可视化模块SDK与热图模块SDK存放目录
{
    SDKFileDirectory:'/*设置为JS SDK 可视化模块SDK与热图模块SDK实际存放目录*/'
}

```

#### sendType

sendType 为设置日志上传方式。可根据自身需要进行增加。

* 类型：String。

```javascript

//设置为使用image标签的图片链接地址上传日志(默认)。
{
    sendType:'img'//或删除sendType参数。
}
//使用Ajax中的post请求上传日志。
{
    sendType:'post'
}

```

{% hint style="info" %}
注意：img类型支持需要方舟4.2.2版本支持，如您使用的方舟版本低于4.2.2版本请更换发送方式
{% endhint %}

#### webstayDuration

webstayDuration 为设置追踪页面滚动行为时，最大停留时长。可根据自身需要进行增加。默认值：5小时

* 类型：Number
* 单位：毫秒

```javascript
//设置追踪页面滚动行为时，最大停留时长。
{
    webstayDuration:50000 //设置追踪页面滚动行为时，最大停留时长为50秒
}

```

#### cross\_subdomain

cross\_subdomain 为_设置在二级域名下存储cookie：false - 在自身域名下存储cookie；true - 在二级域名下存储cookie\(默认\)。例如：一个用户在_同一浏览器中访问_a.analysys.cn与b.analysys.cn两个域，_cross\_subdomain为true时，该用户为一个用户。cross\_subdomain为false时，该用户为两个用户。

* 类型：Boolean

```javascript
//在自身域名下存储cookie。
{
    cross_subdomain:false//或删除autoWebstay参数。
}
//在二级域名下存储cookie。
{
    cross_subdomain:true//或删除cross_subdomain参数。
}

```

## 基础模块介绍

### 统计页面接口介绍

页面跟踪，SDK 默认设置跟踪所有页面，支持自定义页面信息。接口如下：

```javascript
AnalysysAgent.pageView(pageName);
AnalysysAgent.pageView(pageName, callback);
AnalysysAgent.pageView(pageName, properties);
AnalysysAgent.pageView(pageName, properties, callback);    
```

* pageName：页面标识，为字符串，取值长度 1 - 255字符。
* properties：页面信息，为K-V键值对，最多包含100条，且`key`是以字母开头的字符串，**必须由** 字母、数字、下划线组成，字母不区分大小写，**不支持** 乱码、中文、空格等，长度范围1-99字符。value支持类型：String/Number/Boolean/JSON/内部元素为String的Array，若为字符串，长度范围1-255字符。
* callback：数据发送完毕后的回调函数，支持类型：Function\(需SDK版本4.4.1及以后支持\)

示例：

```javascript
// 正在开展某个活动，需要统计活动页面；
AnalysysAgent.pageView("活动页");

......

// 访问手机活动页面，活动页面内容为优惠出售iPhone手机，手机价格为5000元
var properties ={
    "commodityName": "iPhone",
    "commodityPrice": 8000,
    "commodityType": function(){
        return 'phone';
    }
}

AnalysysAgent.pageView("商品页", properties);

......

// 访问手机活动页面，活动页面内容为优惠出售iPhone手机，手机价格为5000元，并返回数据发送完毕状态
var properties ={
    "commodityName": "iPhone",
    "commodityPrice": 8000,
    "commodityType": function(){
        return 'phone';
    }
}

var callback = function(){
    console.log('数据发送完毕')
}
AnalysysAgent.pageView("商品页", properties);
```

### 统计事件接口

用户行为追踪，可以设置自定义属性。接口如下：

```javascript
AnalysysAgent.track(eventName);
AnalysysAgent.track(eventName, eventInfo);
AnalysysAgent.track(eventName, eventInfo, callback);
```

* eventName：自定义事件ID标识，以字母开头的字符串，**必须由**字母、数字、下划线组成，`$` 开头为预置事件/属性，**不支持**乱码、中文、空格等，长度范围1-99字符。
* properties：自定义属性，K-V键值对，用于对事件的描述。最多包含100条，且`key`是以字母开头的字符串，**必须由** 字母、数字、下划线组成，字母不区分大小写，**不支持** 乱码、中文、空格等，长度范围1-99字符。value支持类型：String/Number/Boolean/JSON/内部元素为String的Array，若为字符串，长度范围1-255字符。
* callback：数据发送完毕后的回调函数，支持类型：Function\(需SDK版本4.4.1及以后支持\)

示例：

```javascript
var callback = function() {
    console.log('数据发送完毕')
}
// 添加事件
AnalysysAgent.track("back");

......

// 添加事件，并返回数据发送完毕
AnalysysAgent.track("back", callback);

......

// 用户购买手机
var eventInfo = {
    "type":"Phone",
    "name":"Apple iPhone8",
    "money":4000,
    "count":1,
    "userID": function(){
        return '123456'
    }
}
AnalysysAgent.track("buy", eventInfo);

......

// 用户购买手机，并返回数据发送完毕
var eventInfo = {
    "type":"Phone",
    "name":"Apple iPhone8",
    "money":4000,
    "count":1,
    "userID": function(){
        return '123456'
    }
}

AnalysysAgent.track("buy", eventInfo, callback);
```

### 匿名ID与用户关联

用户关联的主要作用是打通用户登录前后的行为，以及多屏登录后的行为。做过用户关联的用户在登录前后的行为在方舟系统里面会被认为是一个用户。方舟系统目前支持 一台设备只能绑定一个用户 ID，一个用户 ID 只能绑定一台设备。设备和用户 ID 绑定后，就无法再和其他用户或者设备进行绑定。例如一个用户的设备 ID 是 ABC 用户的登录 ID 是 123，绑定成功后会对应同一个 ID，这样在统计或者分析时会被认为是一个用户。**在用户注册成功或者登录成功后客户端需要调用 alias 接口**，建议埋点时观看下 [方舟 SDK 接入视频](https://ark.analysys.cn/video-list.html) 接口描述如下：

用户 id 关联接口。将需要绑定的用户ID 和匿名ID进行关联，计算时会认为是一个用户的行为。接口如下：

```javascript
AnalysysAgent.alias(aliasId);
AnalysysAgent.alias(aliasId, callback);
```

* aliasId：需要关联的用户ID。 取值长度 1 - 255字符,支持类型：String
* callback：数据发送完毕后的回调函数，支持类型：Function\(需SDK版本4.4.1及以后支持\)

示例：

```javascript
// 登陆账号时调用，只设置当前登陆账号即可和之前行为打通
AnalysysAgent.alias("sanbo");

// 登陆账号时调用，只设置当前登陆账号即可和之前行为打通, 并返回数据发送完毕
var callback = function() {
    console.log('数据发送完毕')
}
AnalysysAgent.alias("sanbo", callback);
```

### 用户属性设置

> 用户属性是一个标准的 K-V 结构，K 和 V 均有相应的约束条件，如不符合则丢弃该次操作。

约束条件如下：

> **属性名称**

```text
以字母开头的字符串，必须由字母、数字、下划线组成，字母不区分大小写，$ 开头为预置事件/属性，不支持乱码、中文、空格等，长度范围1-99字符。
```

> **属性值**

```text
支持部分类型：String/Number/Boolean/JSON/内部元素为String的Array；若为字符串，则取值长度 1 - 255字符；若为 Array 或 JSON，则最多包含 100条，且 key 约束条件与属性名称一致，value 取值长度 1 - 255字符
```

#### 设置用户固有属性

设置用户的固有属性，只在首次设置时有效的属性。 如：应用的激活时间、首次登录时间等。如果被设置的用户属性已存在，则这条记录会被忽略而不会覆盖已有数据，如果属性不存在则会自动创建。接口如下：

```javascript
AnalysysAgent.profileSetOnce(propertyName, propertyValue);
AnalysysAgent.profileSetOnce(propertyName, propertyValue, callbck);
AnalysysAgent.profileSetOnce(property);
AnalysysAgent.profileSetOnce(property, callbck);
```

* propertyName ：属性名称，约束见[属性名称](js.md#1.1)
* propertyValue ：属性值，约束见[属性值](js.md#2.1)
* property ： 属性列表，约束见[属性名称](js.md#1.1)，[属性值](js.md#2.1)
* callback：数据发送完毕后的回调函数，支持类型：Function\(需SDK版本4.4.1及以后支持\)

示例：

```javascript
var callback = function(){
    console.log('数据上报完毕')
}
// 设置用户激活时间
AnalysysAgent.profileSetOnce("activationTime", 1521280551929);

// 设置用户激活时间，并返回数据发送完毕
AnalysysAgent.profileSetOnce("activationTime", 1521280551929, callback);

// 设置用户性别和出生时间
var setOnceProfile = {
    "birth": 548798705000,
    "sex": "male",
    "userID": function(){
        return '123456'
    }
}
AnalysysAgent.profileSetOnce(setOnceProfile);

// 设置用户性别和出生时间，并返回数据发送完毕
AnalysysAgent.profileSetOnce(setOnceProfile, callback);
```

#### 设置用户属性

给用户设置单个或多个属性，如果之前不存在，则新建，否则覆盖。接口如下：

```javascript
//设置单个用户属性
AnalysysAgent.profileSet(propertyName, propertyValue);
AnalysysAgent.profileSet(propertyName, propertyValue, callback);

//设置多个用户属性
AnalysysAgent.profileSet(property);
AnalysysAgent.profileSet(property, callback);
```

* propertyName ：属性名称，约束见[属性名称](js.md#1.1)
* propertyValue ：属性值，约束见[属性值](js.md#2.1)
* property ：属性列表，约束见[属性名称](js.md#1.1)，[属性值](js.md#2.1)
* callback ：数据发送完毕后的回调函数，支持类型：Function\(需SDK版本4.4.1及以后支持\)

示例：

```javascript
var callback = function(){
    console.log('数据上报完毕')
}
//设置用户的邮箱地址为yonghu@163.com
AnalysysAgent.profileSet("Email", "yonghu@163.com");

//设置用户的邮箱地址为yonghu@163.com，并返回数据发送完毕
AnalysysAgent.profileSet("Email", "yonghu@163.com", callback);

......

// 设置用户的邮箱和微信
var property = {
    "Email" : "yonghu@163.com",
    "WeChatID" : "weixinhao",
    "userID": function(){
        return '123456'
    }
}
AnalysysAgent.profileSet(property);
// 设置用户的邮箱和微信，并返回数据发送完毕
AnalysysAgent.profileSet(property, callback);
```

#### 设置用户属性相对变化值

设置用户属性的相对变化值\(相对增加，减少\)，只能对数值型属性进行操作，如果这个 Profile之前不存在，则初始值为0。接口如下：

```javascript
AnalysysAgent.profileIncrement(propertyName, propertyNumber)
AnalysysAgent.profileIncrement(propertyName, propertyNumber, callback)
AnalysysAgent.profileIncrement(property);
AnalysysAgent.profileIncrement(property, callback);
```

* propertyName：属性名称，约束见[属性名称](js.md#1.1)
* propertyValue：属性值，约束见[属性值](js.md#2.1)
* property：属性列表，约束见[属性名称](js.md#1.1)，[属性值](js.md#2.1)
* callback ：数据发送完毕后的回调函数，支持类型：Function\(需SDK版本4.4.1及以后支持\)

示例：

```javascript
var callback = function(){
    console.log('数据上报完毕')
}
//用户增加一岁
AnalysysAgent.profileIncrement("age", 1);

//用户增加一岁，并返回数据发送完毕
AnalysysAgent.profileIncrement("age", 1, callback);
......

//用户玩某个游戏时间增加一年，游戏等级增加2,游戏金币加1000
var incrementProfile = {
    "gameAge":1,
    "gameRating":2,
    "gageMoney": function(){
        return 1000
    }
}
AnalysysAgent.profileIncrement(incrementProfile);
//用户玩某个游戏时间增加一年，游戏等级增加2，并返回数据发送完毕
AnalysysAgent.profileIncrement(incrementProfile, callback);
```

#### 增加列表类型的属性

用户列表属性增加元素。接口如下：

```javascript
//增加多个属性
AnalysysAgent.profileAppend(propertyName, propertyValue);
AnalysysAgent.profileAppend(propertyName, propertyValue, callback);
```

* propertyName：属性名称，约束见[属性名称](js.md#1.1)
* propertyValue：属性值，约束见[属性值](js.md#2.1)

示例：

```javascript
//增加多个用户爱好
var list = ["PlayBasketball", "music"];
// var list = function(){
//     return ["PlayBasketball", "music"];
// }
AnalysysAgent.profileAppend("hobby", list);

//增加多个用户爱好，并返回数据发送完毕
var callback = function(){
    console.log('数据上报完毕')
}

AnalysysAgent.profileAppend("hobby", list, callback);
```

#### 删除设置的属性值

删除已设置的用户属性值。接口如下：

```javascript
//  删除当前用户单个属性值
AnalysysAgent.profileUnset(propertyName);
AnalysysAgent.profileUnset(propertyName, callback);

//  删除当前用户所有属性值
AnalysysAgent.profileDelete();
AnalysysAgent.profileDelete(callback);
```

* propertyName：属性名称，约束见[属性名称](js.md#1.1)
* callback ：数据发送完毕后的回调函数，支持类型：Function\(需SDK版本4.4.1及以后支持\)

示例：

```javascript
var callback = function(){
    console.log('数据上报完毕')
}
// 删除单个用户属性
AnalysysAgent.profileUnset( "age");

// 删除单个用户属性，并返回数据发送完毕
AnalysysAgent.profileUnset( "age", callback);

// 清除所有属性
AnalysysAgent.profileDelete();

// 清除所有属性，并返回数据发送完毕
AnalysysAgent.profileDelete(callback);
```

### 匿名ID设置

唯一匿名ID标识设置，接口如下：

```javascript
AnalysysAgent.identify(distinctId);
AnalysysAgent.identify(distinctId, callback);
```

* distinctId：自定义设备身份标识，取值长度 1 - 255字符,支持类型：String
* callback ：数据处理完毕后的回调函数，支持类型：Function\(需SDK版本4.4.1及以后支持\)

示例:

```javascript
var callback = function(){
    console.log('数据处理完毕')
}
// 设置匿名ID为`fangke009901`,注意此方法需要在初始化中调用
AnalysysAgent.identify("fangke009901");

// 设置匿名ID为`fangke009901`,注意此方法需要在初始化中调用，并返回数据处理完毕
AnalysysAgent.identify("fangke009901", callback);
```

### 匿名ID获取

获取用户通过identify接口设置或自动生成的id，优先级如下： 用户设置的id &gt; 代码自动生成的id

接口如下：

```javascript
AnalysysAgent.getDistinctId();
AnalysysAgent.getDistinctId(callback);
```

* callback ：数据处理完毕后的回调函数，支持类型：Function\(需SDK版本4.4.1及以后支持\)

示例:

```javascript
// 获取匿名id
var distinctId = AnalysysAgent.getDistinctId();

// 获取匿名id，并返回数据处理完毕
var callback = function(id){
    console.log('数据处理完毕，匿名id:' + id)
}

AnalysysAgent.getDistinctId(callback);
```

### 通用属性

> 通用属性是每次上传事件信息都会带有的属性，通用属性是一个标准的 K-V 结构，K 和 V 均有相应的约束条件，如不符合则丢弃该次操作。

约束条件如下:

> **属性名称**

```text
以字母开头的字符串，必须由字母、数字、下划线组成，字母不区分大小写，$ 开头为预置事件/属性，不支持乱码、中文、空格等，长度范围1-99字符。
```

> **属性值**

```text
支持部分类型：String/Number/boolean/JSON/内部元素为String的Array；若为字符串，则取值长度 1 - 255字符；若为 Array 或 JSON,则最多包含 100条，且 key 约束条件与属性名称一致，value 取值长度 1 - 255字符
```

#### 注册通用属性

某一个体，在固定范围内，持续拥有的属性，每次数据上传都会携带。接口如下:

```javascript
AnalysysAgent.registerSuperProperty(superPropertyName, superPropertyValue );
AnalysysAgent.registerSuperProperty(superPropertyName, superPropertyValue, callback );
AnalysysAgent.registerSuperProperties(superProperty);
AnalysysAgent.registerSuperProperties(superProperty, callback);
```

* superPropertyName：属性名称，约束见[属性名称](js.md#1)
* superPropertyValue：属性值，约束见[属性值](js.md#2)
* superProperty：属性列表，约束见[属性名称](js.md#1)，[属性值](js.md#2)
* callback ：数据处理完毕后的回调函数，支持类型：Function\(需SDK版本4.4.1及以后支持\)

示例：

```javascript
var callback = function(id){
    console.log('数据处理完毕，匿名id:' + id)
}

// 在某视频平台，购买一年会员，该年内只需设置一次即可
AnalysysAgent.registerSuperProperty("member","VIP");

......

// 在某视频平台，购买一年会员，该年内只需设置一次即可，并返回数据处理完毕
AnalysysAgent.registerSuperProperty("member", "VIP", callback);

......

// 小明在20岁的时候，购买了一年腾讯会员
var property = {
    "platform":"TX",
    "age":"20",
    "member":"VIP",
    "user":"xiaoming",
    "userID": function(){
        return '123456'
    }
}
AnalysysAgent.registerSuperProperties(property);

......

// 小明在20岁的时候，购买了一年腾讯会员，并返回数据处理完毕
AnalysysAgent.registerSuperProperties(property, callback);
```

#### 删除通用属性

根据属性名称，删除已设置过的通用属性。接口如下：

```javascript
//删除单个通用属性
AnalysysAgent.unRegisterSuperProperty(superPropertyName);
AnalysysAgent.unRegisterSuperProperty(superPropertyName, callback);

//清除所有通用属性
AnalysysAgent.clearSuperProperties();
AnalysysAgent.clearSuperProperties(callback);
```

* superPropertyName：属性名称，约束见[属性名称](js.md#1)
* callback ：数据处理完毕后的回调函数，支持类型：Function\(需SDK版本4.4.1及以后支持\)

示例：

```javascript
var callback = function(){
    console.log('数据处理完毕')
}
// 删除已经设置的用户年龄属性
AnalysysAgent.unRegisterSuperProperty("age");

......

// 删除已经设置的用户年龄属性，并返回数据处理完毕
AnalysysAgent.unRegisterSuperProperty("age", callback);

......

// 清除所有已经设置的通用属性
AnalysysAgent.clearSuperProperties();

......

// 清除所有已经设置的通用属性，并返回数据处理完毕
AnalysysAgent.clearSuperProperties(callback);
```

#### 获取通用属性

查询获取通用属性。接口如下：

```javascript
// 获取单个通用属性
AnalysysAgent.getSuperProperty(superPropertyName);
AnalysysAgent.getSuperProperty(superPropertyName, callback);

// 获取所有的通用属性
AnalysysAgent.getSuperProperties();
AnalysysAgent.getSuperProperties(callback);
```

* superPropertyName：属性名称，约束见[属性名称](js.md#1)
* callback ：数据处理完毕后的回调函数，支持类型：Function\(需SDK版本4.4.1及以后支持\)

示例：

```javascript
var callback = function(value){
    console.log('数据处理完毕, 获取到的通用属性：', value)
}
// 查看已经设置的"member"的通用属性
AnalysysAgent.getSuperProperty("member");

......

// 查看已经设置的"member"的通用属性，并返回数据处理完毕
AnalysysAgent.getSuperProperty("member", callback);

......

// 查看所有已经设置的通用属性
AnalysysAgent.getSuperProperties();
......

// 查看所有已经设置的通用属性，并返回数据处理完毕
AnalysysAgent.getSuperProperties(callback);

```

### 获取预置属性

获取预置属性。接口如下：

```javascript
AnalysysAgent.getPresetProperties();
AnalysysAgent.getPresetProperties(callback);
```

* callback ：数据处理完毕后的回调函数，支持类型：Function\(需SDK版本4.4.1及以后支持\)

示例：获取预置属性

```javascript
var callback = function(value){
    console.log('数据处理完毕, 获取到的预置属性：', value)
}

// 获取预置属性
var presetProperties = AnalysysAgent.getPresetProperties();

console.log('预置属性:', presetProperties)

// 获取预置属性，并返回数据处理完毕
AnalysysAgent.getPresetProperties(callback);
```

### 清除本地设置

清除本地现有的设置（包括 id 和通用属性）重新开始统计。接口如下：

```javascript
AnalysysAgent.reset();
AnalysysAgent.reset(callback);
```

* callback ：数据处理完毕后的回调函数，支持类型：Function\(需SDK版本4.4.1及以后支持\)

示例：清除本地现有的设置，包括id和通用属性

```javascript
// 删除设置的id和通用属性
AnalysysAgent.reset();

// 删除设置的id和通用属性，并返回数据处理完毕
var callback = function(){
    console.log('数据处理完毕')
}
AnalysysAgent.reset(callback);

```



