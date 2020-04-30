# 快速集成

通过以下方式可以快速完成SDK的集成，更多方式请查SDK集成文档

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
    (function(config) {
        window.AnalysysAgent = window.AnalysysAgent || [];
        window.AnalysysAgent.methods = 'identify alias reset track profileSet profileSetOnce profileIncrement profileAppend profileUnset profileDelete registerSuperProperty registerSuperProperties unRegisterSuperProperty clearSuperProperties getSuperProperty getSuperProperties pageView getDistinctId getPresetProperties'.split(' ');

        function factory(b) {
            return function () {
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
            if (!AnalysysAgent[key]) AnalysysAgent[key] = factory(key);
            AnalysysAgent[key](config[key]);
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
<script>
    (function(config) {
        window.AnalysysAgent = window.AnalysysAgent || [];
        window.AnalysysAgent.methods = 'identify alias reset track profileSet profileSetOnce profileIncrement profileAppend profileUnset profileDelete registerSuperProperty registerSuperProperties unRegisterSuperProperty clearSuperProperties getSuperProperty getSuperProperties pageView getDistinctId getPresetProperties'.split(' ');

        function factory(b) {
            return function () {
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
            if (!AnalysysAgent[key]) AnalysysAgent[key] = factory(key);
            AnalysysAgent[key](config[key]);
        }
    })({
        appkey: '/*设置为实际APPKEY*/', //APPKEY
        uploadURL: '/*设置为实际地址*/'//上传数据的地址
    })
</script>

//引用JS SDK文件的script标签必须在初始化代码之下

//建议在script标签设置id为ARK_SDK,该ID用来引导可视化模块与热图模块的加载
<script type="text/javascript" id="ARK_SDK" src="/*设置为JS SDK实际存放地址*/"></script>

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
自行下载SDK。获取AnalysysAgent\_JS\_SDK.min.js，假设该文件放到与 require.js 同一目录中

```javascript
requirejs(["./AnalysysAgent_JS_SDK.min"], function(AnalysysAgent) {
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

## 获取上报地址

登录易观方舟系统后点击”管理“——”数据接入管理“——”集成SDK接入数据”——“获取数据接收地址”

点击后即可获取您的数据上报地址。

![&#x83B7;&#x53D6;&#x6570;&#x636E;&#x63A5;&#x6536;&#x5730;&#x5740;](../../../.gitbook/assets/wx20200427-181748-2x.png)

## 基础模块介绍

### 初始化方法

* appkey\(必须\) 在网站获取的 AppKey
* _debugMode_ 设置调试模式：0 - 关闭调试模式\(默认\)；1 - 开启调试模式，数据不入库；2 - 开启调试模式，数据入库
* _uploadURL_\(必须\) 设置上传数据接口
* _autoTrack_ 设置是否启用全埋点功能：false - 不启用全埋点功能\(默认\)；true - 启用全埋点功能（SDK 版本 4.4.0 及以后支持）

### 初始化示例代码

{% tabs %}
{% tab title="异步集成初始化示例代码" %}
```javascript
<script>
    (function(config) {
        window.AnalysysAgent = window.AnalysysAgent || [];
        window.AnalysysAgent.methods = 'identify alias reset track profileSet profileSetOnce profileIncrement profileAppend profileUnset profileDelete registerSuperProperty registerSuperProperties unRegisterSuperProperty clearSuperProperties getSuperProperty getSuperProperties pageView getDistinctId getPresetProperties'.split(' ');

        function factory(b) {
            return function () {
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
            if (!AnalysysAgent[key]) AnalysysAgent[key] = factory(key);
            AnalysysAgent[key](config[key]);
        }
        var date = new Date();
        var time = new String(date.getFullYear()) + new String(date.getMonth() + 1) + new String(date.getDate());

        var d = document,
            c = d.createElement('script'),
            n = d.getElementsByTagName('script')[0];
        c.type = 'text/javascript';
        c.async = true;
        c.id = 'ARK_SDK';
    //首先您需要下载SDk，替换其中的地址为您本地目录绝对路径或者相对路径
        c.src = '/*设置为JS SDK实际存放地址*/' +'?v=' +time; //JS SDK存放地址
        n.parentNode.insertBefore(c, n);
    })({
    //设置APPKEY，
    //  设置key，77a52s552c892bn442v721为样例数据，请根据实际情况替换相应内容
        appkey: '77a52s552c892bn442v721', 
    //设置数据上报地址，请替换http://example.com为您上报地址
        uploadURL: 'htts://example.com'//上传数据的地址
    //开启Debug模式，上线时请设置0或删除代码
        debugMode:2 
    //开启全埋点
        autoTrack:true
    })
</script>
```
{% endtab %}
{% endtabs %}

## 自定义事件接口

用户行为追踪，可以设置自定义属性。接口如下：

```javascript
AnalysysAgent.track(eventName);
AnalysysAgent.track(eventName, eventInfo);
AnalysysAgent.track(eventName, eventInfo, callback);
```

* eventName：事件ID，以字母或 `$` 开头，包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件/属性，不支持乱码和中文，取值长度 1 - 99字符
* properties：自定义属性，用于对事件描述。properties 最多包含 100条，且 key 以字母或 `$` 开头，包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件/属性，不支持乱码和中文，取值长度 1 - 99字符，value 支持类型：String/Number/boolean/JSON/内部元素为String的Array，若为字符串，取值长度 1 - 255字符
* callback：数据发送完毕后的回调函数，支持类型：Function\(需SDK版本4.4.1及以后支持\)

代码示例：

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

## 账号关联

用户关联的主要作用是打通用户登录前后的行为，做过用户关联的用户在登录前后的行为在方舟系统里面会被认为是一个用户。

**建议在用户注册成功、登录成功后、或在检测登录状态的业务代码中需要调用 alias 接口。**

接口如下：

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

## 设置用户属性

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

## 验证数据

开启Debug模式后，可以通过在浏览器的开发者工具中查看相应上报日志

![&#x901A;&#x8FC7;&#x6D4F;&#x89C8;&#x5668;&#x4E0A;&#x5F00;&#x53D1;&#x8005;&#x5DE5;&#x5177;&#x67E5;&#x770B;](../../../.gitbook/assets/image%20%2868%29.png)

* 检查上报地址是否正确
* 检查是否发送成功
* 检查事件名称及内容与预期相同



