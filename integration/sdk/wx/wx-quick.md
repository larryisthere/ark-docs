# 快速集成

通过以下方式可以快速完成SDK的集成体验方舟，更多方式请查SDK集成文档

## 集成配置

将 AnalysysAgent\_WX\_SDK.min.js 文件放到小程序的目录下

![](../../../.gitbook/assets/image%20%287%29.png)

## 获取上报地址 <a id="huo-qu-shang-bao-di-zhi"></a>

登录易观方舟系统后点击”管理“——”数据接入管理“——”集成SDK接入数据”——“获取数据接收地址”

点击后即可获取您的数据上报地址。

![&#x83B7;&#x53D6;&#x6570;&#x636E;&#x63A5;&#x6536;&#x5730;&#x5740;](https://docs.analysys.cn/_imgs/477863553390137972.png?alt=media&token=d48d296e-4c24-4a3b-8c13-33f701dd82b6)

## SDK接入及初始化

{% tabs %}
{% tab title="微信原生接入" %}
```javascript
//标准版本开启全埋点接入方式示例：
// app.js
import AnalysysAgent from  './build/AnalysysAgent_WX_SDK.es6.min.js';
//设置您的APPKEY
AnalysysAgent.appkey = "/*设置为实际APPKEY*/" 
//设置您的上报地址
AnalysysAgent.uploadURL = '/*设置为方舟项目上报的地址*/'
AnalysysAgent.autoTrack = true
```
{% endtab %}

{% tab title="taro 框架接入" %}
```javascript
//框架（taro）开启全埋点接入方式示例：
//app.jsx
let AnalysysAgent = require("./build/AnalysysAgent_WX_SDK.min.js")
AnalysysAgent.appkey = "/*设置为实际APPKEY*/" //APPKEY
AnalysysAgent.uploadURL = '/*设置为方舟项目上报的地址*/'
AnalysysAgent.autoTrack = true
```
{% endtab %}

{% tab title="uniapp 框架接入" %}
```javascript
//框架（uniapp）开启全埋点接入方式示例：
// main.js
import AnalysysAgent from './sdk/AnalysysAgent_WX_SDK.es6.min.js';
import Vue from 'vue'
import App from './App'

AnalysysAgent.appkey = "/*设置为实际APPKEY*/" //APPKEY
AnalysysAgent.uploadURL = '/*设置为方舟项目上报的地址*/'
AnalysysAgent.autoTrack = true//框架（mpvue）开启全埋点接入方式示例：
```
{% endtab %}

{% tab title="mpvue 框架接入" %}
```javascript
//main.js
import AnalysysAgent from './sdk/AnalysysAgent_WX_SDK.es6.min.js';
import Vue from 'vue'
import App from './App'

AnalysysAgent.appkey = "/*设置为实际APPKEY*/" //APPKEY
AnalysysAgent.uploadURL = '/*设置为方舟项目上报的地址*/'
AnalysysAgent.autoTrack = true//框架（wepy）开启全埋点接入方式示例：
```
{% endtab %}

{% tab title="mpvue 框架接入" %}
```javascript
// app.wpy
import AnalysysAgent from  './sdk/AnalysysAgent_WX_SDK.min.js';
AnalysysAgent.appkey = "/*设置为实际APPKEY*/" //APPKEY
AnalysysAgent.uploadURL = '/*设置为方舟项目上报的地址*/'
AnalysysAgent.autoTrack = true
```
{% endtab %}
{% endtabs %}

## 自定义事件接口

自定义事件接口，可以设置自定义属性。接口如下：

```javascript
AnalysysAgent.track(eventName, eventInfo)
```

* eventName：事件ID，以字母或 `$` 开头，包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件/属性，不支持乱码和中文，取值长度 1 - 99字符
* eventInfo：自定义属性，用于对事件描述。eventInfo 最多包含 100条，且 key 以字母或 `$` 开头，包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件/属性，不支持乱码和中文，取值长度 1 - 99字符，value 支持类型：String/Number/boolean/内部元素为String的Array，若为字符串，取值长度 1 - 255字符

示例：

```javascript
// 添加事件
AnalysysAgent.track("back");

......

// 用户购买手机
var eventInfo = {
    "type":"Phone",
    "name":"Apple iPhone8",
    "money":4000,
    "count":1
}
AnalysysAgent.track("buy", eventInfo);
```

## 账号关联

用户关联的主要作用是打通用户登录前后的行为，做过用户关联的用户在登录前后的行为在方舟系统里面会被认为是一个用户。**建议在用户注册成功或者登录成功后客户端需要调用 alias 接口。**

```javascript
AnalysysAgent.alias(aliasId);
```

* aliasId：需要关联的用户ID。 取值长度 1 - 255字符,支持类型：String

示例：

```javascript
// 登陆账号时调用，只设置当前登陆账号即可和之前行为打通
AnalysysAgent.alias("sanbo");
```

## 设置用户属性

给用户设置单个或多个属性，如果之前不存在，则新建，否则覆盖。接口如下：

```javascript
//设置单个用户属性
AnalysysAgent.profileSet(propertyName, propertyValue);
//设置多个用户属性
AnalysysAgent.profileSet(property);
```

* propertyName ：属性名称，约束见属性名称
* propertyValue ：属性值，约束见属性值
* property ： 属性列表，约束见属性名称，属性值

示例：

```javascript
//设置用户的邮箱地址为yonghu@163.com
AnalysysAgent.profileSet("Email", "yonghu@163.com");

......

// 设置用户的邮箱和微信
var property = {
    "Email" : "yonghu@163.com",
    "WeChatID" : "weixinhao"
}
AnalysysAgent.profileSet(property);
```

## 验证数据

开启Debug模式后，用户可以通过微信开发者工具中的控制台中查看tag为`[analysys]`的Log日志

![](../../../.gitbook/assets/image%20%2885%29.png)

