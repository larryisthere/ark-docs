---
description: Node SDK 主要用于服务端 Node 应用，如 Node Web 应用的后台服务。
---

# Node JS SDK

Node JS SDK 集成前请先下载 SDK

{% hint style="info" %}
SDK Releases包下载：  
Github地址\(推荐\)：[https://github.com/analysys/ans-node-sdk/releases](https://github.com/analysys/ans-node-sdk/releases)  
Gitee地址：[https://gitee.com/Analysys/ans-node-sdk/releases](https://gitee.com/Analysys/ans-node-sdk/releases)  
Releases中含有更新说明请您阅读，接口使用请参考本文档。
{% endhint %}

| 文件名 | 功能描述 | 是否必须 |
| :--- | :--- | :--- |
| AnalysysAgent\_NodeJS\_SDK.es6.js | 基础模块 | ES6必须 |
| AnalysysAgent\_NodeJS\_SDK.cjs.js | 基础模块 | 非ES6必须 |
| AnalysysAgent\_NodeJS\_SDK\_LogCollecter.es6.js | 日志收集功能 | ES6可选 |
| AnalysysAgent\_NodeJS\_SDK\_LogCollecter.cjs.js | 日志收集功能 | 非ES6可选 |

注意：请您根据自身业务需求来引用相关的SDK。

## 集成 SDK

SDK目前提供了以下方集成方式

### **NPM引入**

**使用NPM方式引入 ，安装 ans-node-sdk**

```javascript
npm install ans-node-sdk --save

// es6 
import AnalysysAgent from "ans-node-sdk"
// 落文件功能，不需要则不用引入
import LogCollector  from 'ans-node-sdk/sdk/AnalysysAgent_NodeJS_SDK_LogCollecter.cjs.js';

// 非es6 
var AnalysysAgent = require("ans-node-sdk");
// 落文件功能，不需要则不用引入
var LogCollector = require('ans-node-sdk/sdk/AnalysysAgent_NodeJS_SDK_LogCollecter.cjs.js');
```

###  **手动引入**

**SDK目前提供了两个版本 非es6（cjs） 和 es6 两个版本**

```javascript
// es6 
import AnalysysAgent from "./sdk/AnalysysAgent_NodeJS_SDK.es6.js"
// 落文件功能，不需要则不用引入
import LogCollector  from './sdk/AnalysysAgent_NodeJS_SDK_LogCollecter.es6.js';

// 非es6 
var AnalysysAgent = require("./sdk/AnalysysAgent_NodeJS_SDK.cjs.js");
// 落文件功能，不需要则不用引入
var LogCollector = require('./sdk/AnalysysAgent_NodeJS_SDK_LogCollecter.cjs.js');
```

## 2. SDK 初始化

### 2.1 获取配置信息

```javascript
let AgentConfig ={
        appId : 项目对应的appkey,
        uploadURL:数据接收地址,
        debugMode:debug模式,
        postNumber:上传条数的设置,
        postTime:自动上传间隔时间,
        logCollector:new LogCollector({   //落文件功能增加的参数
            gerFold:落地文件存放的路径,
            gerRule:落地文件存放的格式
        })
    }
```

### 2.2 初始化接口

在程序启动时，调用构造函数 new AnalysysAgent\(AgentConfig\) 初始化 Node SDK 实例。如下：

```cpp
// es6 
import AnalysysAgent from './sdk/AnalysysAgent_NodeJS_SDK.es6.js';
import LogCollector from './sdk/AnalysysAgent_NodeJS_SDK_LogCollecter.es6.js';
var analysys = new AnalysysAgent(AgentConfig);

// 非es6
let AnalysysAgent = require('./sdk/AnalysysAgent_NodeJS_SDK.cjs.js');
let LogCollector = require('./sdk/AnalysysAgent_NodeJS_SDK_LogCollecter.es6.js');
var analysys = new AnalysysAgent(AgentConfig);
```

#### 2.2.1 实时收集器接口

用户每触发一次上传，则立即上传数据至接收服务器：

```cpp
postNumber:0
```

#### 2.2.2 批量收集器接口

用户触发上传，该收集器将先缓存数据，直到数量达到用户设置的阈值或者用户设置的等待时间，才会触发真正的上传：

```cpp
postNumber:batchNum
postTime:batchSecnBatchNum：
```

batchNum：批量发送数量，默认值：0条,实时上传  
batchSec：批量发送等待时间\(毫秒\)，默认值：0 毫秒

#### 2.2.3 落地文件收集器

该收集器可以把用户触发的事件经过封装处理成标准的JSON写入本地文件中。支持多进程同时写入同一文件，默认为同步一条条写入文件，可以指定是否异步批量写入以及批量写入的条数和时间间隔，批量写入的条数和时间间隔就是 SDK 初始化设置的批量条数和时间间隔。

```cpp
    let AgentConfig ={
        appId : 项目对应的appkey,
        debugMode:debug模式,
        postNumber:上传条数的设置（批量写入的条数设置）,
        postTime:自动上传间隔时间 （批量写入的时间间隔）,
        logCollector:new LogCollector({   //落文件功能增加的参数
            gerFold:落地文件存放的路径,
            gerRule:落地文件存放的格式
        })
    }
```

gerFold：数据保存的目录  
gerRule：文件切分规则，（可选 "H" 和 "D" ）默认 H，按小时切割，D 按天切割。

假如启用了落地文件收集器，配置中的uploadURL将不再生效。

至此 SDK初始化完成，调用 SDK 提供的接口已可正常采集用户数据了。  
说明：程序结束前需要调用 `flush()` 接口，该接口可以把本地缓存的尚未发送至数据接收服务器或未写入本地的数据发送到数据接收服务器或写入本地文件。

## 3.基础接口介绍

### 3.1 Debug 模式

Debug 主要用于开发者测试，接口如下：

```cpp
debugMode:debug
```

debug：debug 模式,Number,默认关闭状态。有以下三种枚举值：

`0`：表示关闭 Debug 模式  
`1`：表示打开 Debug 模式，但该模式下发送的数据仅用于调试，不计入平台数据统计  
`2`：表示打开 Debug 模式，该模式下发送的数据可计入平台数据统计

{% hint style="info" %}
注意：发布版本时debug模式设置为\`0\`。
{% endhint %}

### 3.2 统计事件

事件跟踪，设置事件名称和事件详细信息。接口如下：

```javascript
track(distinctId, isLogin, eventName, properties,platfrom);
track(distinctId, isLogin, eventName, properties,platform,upLoadTime);
```

* distinctId:自定义设备身份标识,长度大于 0 且小于 255字符
* isLogin:用户 ID 是否是登录 ID
* eventName:事件名称,以字母或 `$` 开头，可包含字母、数字、下划线和 `$`，字母不区分大小写，`$`开头为预置事件,不支持乱码和中文,最大长度 99字符
* properties:事件属性,最多包含 100条,且 key 以字母或 `$` 开头，可包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件属性,最大长度 99字符,不支持乱码和中文,value 类型约束\(String/Number/boolean/list/数组\)，若为字符串,最大长度255字符。没有请设置`null`。
* platform：平台类型,建议内容范围：JS、WeChat、Android、iOS, 并且支持自定义
* upLoadTime: 用户自定义时间戳\(带毫秒的13位时间戳\)

```cpp
// 浏览商品
var distinctId = "1234";
var isLogin = false;
var eventName = "ViewProduct";
var platform = "Android";
var trackPropertie = {
    "$ip":"112.112.112.112"        //IP地址
};
var bookList = ["Javascript权威指南"];
trackPropertie.productName = bookList;  
trackPropertie.productType = "Js书籍";
trackPropertie.producePrice = 80;		  
trackPropertie.shop = "xx网上书城";     
analysys.track( distinctId, isLogin,eventName, trackPropertie,platform);
// 或者也可以使用自定义的时间戳
var uploadTime = 1569859200000;
analysys.track( distinctId, isLogin,eventName, trackPropertie,platform,uploadTime);
```

### 3.3 用户关联

用户 ID 关联接口。将 aliasId和 distinctId关联，计算时会认为是一个用户的行为。该接口是在 distinctId发生变化的时候调用，来告诉 SDK distinctId变化前后的 ID 对应关系。该场景一般应用在用户注册/登录的过程中。比如：一个匿名用户浏览商品，系统为其分配的distinctId = "1234567890987654321"，随后该匿名用户进行注册，系统为其分配了新的注册 ID，aliasId = "ABCDEF123456789"，此时就需要调用 alias 接口对两个 ID 进行关联。接口如下：

```cpp
alias(aliasId, distinctId,platform);
alias(aliasId, distinctId,platform,upLoadTime);
```

* aliasId：用户注册 ID，长度大于 0，且小于 255字符
* distinctId：用户匿名ID，长度大于 0，且小于 255字符
* platform：平台类型,建议内容范围：JS、WeChat、Android、iOS, 并且支持自定义
* upLoadTime：用户自定义时间戳\(带毫秒的13位时间戳\)

示例：匿名用户浏览商品到注册会员

```cpp
// 匿名ID
var distinctId = "1234567890987654321";
var platform = "Android"
...
...
...
//用户注册登录
var registerId = "ABCDEF123456789";
analysys.alias(registerId,distinctId,platform);
// 或者也可以使用自定义的时间戳
var uploadTime = 1569859200000;
analysys.alias(registerId,distinctId,platform,uploadTime);
```

### 3.4 用户属性设置

SDK提供以下接口供用户设置用户的属性，比如用户的年龄/性别等信息。

用户属性是一个标准的K-V结构，K和V均有相应的约束条件，如不符合则丢弃该次操作。 参数约束:

属性名称

以字母或$开头，可包含字母、数字、下划线和$，字母不区分大小写，$开头为预置事件属性,最大长度99字符,不支持乱码和中文

属性值

支持部分类型：string/number/boolean/集合/数组; 若为字符串,则最大长度255字符; 若为数组或集合,则最多包含100条,且key约束条件与属性名称一致,value最大长度255字符

设置单个或多个属性，如用户所在城市，用户昵称，用户头像信息等。如果之前存在，则覆盖，否则，新创建。接口如下：

```javascript
profileSet(distinctId, isLogin, properties, platform);
profileSet(distinctId, isLogin, properties, platform,upLoadTime)
```

* distinctId: 自定义设备身份标识,长度大于0且小于255字符
* isLogin: 用户ID是否是登录 ID
* properties: 事件属性，没有请设置`null`。
* platform：平台类型,建议内容范围：JS、WeChat、Android、iOS, 并且支持自定义
* upLoadTime: 用户自定义时间戳\(带毫秒的13位时间戳\)

示例：用户注册后设置用户的注册信息属性

```cpp
var registerId = "ABCDEF123456789";
var isLogin = true;
var platform  = "Android"
//用户信息
var profiles = {
    $city:"北京",         //城市
    $province:"北京",     //省份
    nickName:"昵称123",   //昵称
    userLevel:0,         //用户级别
    userPoint:0          //用户积分
};
var interestList = ["户外活动","足球赛事","游戏"];
profiles.interest = interestList;//用户兴趣爱好
analysys.profileSet(registerId, isLogin, profiles,platform);
// 或者也可以使用自定义的时间戳
var uploadTime = 1569859200000;
analysys.profileSet(registerId, isLogin, profiles,platform,uploadTime);
```

## 4.更多接口

### 4.1 用户属性

#### 4.1.1设置用户固有属性

只在首次设置时有效的属性。如：用户的注册时间。如果被设置的用户属性已存在，则这条记录会被忽略而不会覆盖已有数据，如果属性不存在则会自动创建。接口如下：

```cpp
profileSetOnce(distinctId, isLogin, properties,platform);
profileSetOnce(distinctId, isLogin, properties,platform,upLoadTime);
```

* distinctId: 自定义设备身份标识,长度大于0且小于255字符
* isLogin: 用户ID是否是登录 ID
* properties: 事件属性，没有请设置`null`。
* platform：平台类型,建议内容范围：JS、WeChat、Android、iOS, 并且支持自定义
* upLoadTime: 用户自定义时间戳\(带毫秒的13位时间戳\)

示例：要统计用户注册时间

```cpp
var registerId = "ABCDEF123456789";
var isLogin = true;
var platform  = "Android"
var profileAge = {
    registerTime:"20180101101010"
};
analysys.profileSetOnce(registerId, isLogin, profileAge,platform);
// 或者也可以使用自定义的时间戳
var uploadTime = 1569859200000;
analysys.profileSetOnce(registerId, isLogin, profileAge,platform,uploadTime);

```

#### 4.1.2 设置用户属性相对变化值

设置用户属性的单个相对变化值\(相对增加,减少\)，只能对数值型属性进行操作，如果这个Profile之前不存在,则初始值为0。接口如下：

```cpp
profileIncrement(distinctId, isLogin, properties,platform);
profileIncrement(distinctId, isLogin, properties,platform,upLoadTime);
```

* distinctId: 自定义设备身份标识,长度大于0且小于255字符
* isLogin: 用户ID是否是登录 ID
* properties: 事件属性，没有请设置`null`。
* platform：平台类型,建议内容范围：JS、WeChat、Android、iOS, 并且支持自定义
* upLoadTime: 用户自定义时间戳\(带毫秒的13位时间戳\)

示例：用户注册初始积分为0，在用户购买商品后，用户的积分增加20，则调用该接口，用户的积分变为0+20=20了：

```cpp
var registerId = "ABCDEF123456789";
var isLogin = true;
var platform = "Android";
var profile = {
    userPoint:20
};
analysys.profileIncrement(registerId, isLogin, profile,platform);
// 或者也可以使用自定义的时间戳
var uploadTime = 1569859200000;
analysys.profileIncrement(registerId, isLogin, profile,platform,uploadTime);
```

#### 4.1.3 增加列表类型的属性

为列表类型的属性增加一个或多个元素，如：用户新增兴趣爱好，接口如下：

```cpp
profileAppend(distinctId, isLogin, properties,platform);
profileAppend(distinctId, isLogin, properties,platform, upLoadTime);
```

* distinctId: 自定义设备身份标识,长度大于0且小于255字符
* isLogin: 用户ID是否是登录 ID
* properties: 事件属性，没有请设置`null`。
* platform：平台类型,建议内容范围：JS、WeChat、Android、iOS, 并且支持自定义
* upLoadTime: 用户自定义时间戳\(带毫秒的13位时间戳\)

示例：用户初始填写的兴趣爱好为\["sports"，"football"，"game"\]，调用该接口追加\["study"，"BodyBuilding"\]，则用户的爱好变为\["sports"，"football"，"game"，"study"，"BodyBuilding"\]

```cpp
var registerId = "ABCDEF123456789";
var isLogin = true;
var platform = "Android";
var interestList = ["户外活动","足球赛事","游戏"];
var profile = {
    interest:interestList     //用户兴趣爱好
};
analysys.profileAppend(registerId, isLogin, profile,platform);
// 或者也可以使用自定义的时间戳
var uploadTime = 1569859200000;
analysys.profileAppend(registerId, isLogin, profile,platform,uploadTime)
```

#### 4.1.4 删除设置的属性值

删除设置的属性值。接口如下：

```cpp
profileUnSet(distinctId, isLogin, property,platform);
profileUnSet(distinctId, isLogin, property,platform, upLoadTime);
profileDelete(distinctId, isLogin,platform);
profileDelete(distinctId, isLogin, platform,upLoadTime);
```

* distinctId: 自定义设备身份标识,长度大于0且小于255字符
* isLogin: 用户ID是否是登录 ID
* property：事件属性
* platform：平台类型,建议内容范围：JS、WeChat、Android、iOS, 并且支持自定义
* upLoadTime: 用户自定义时间戳\(带毫秒的13位时间戳\)

```cpp
示例1： 要删除已经设置的用户昵称这一用户属性
var registerId = "ABCDEF123456789";
var isLogin = true;
var platform = "Android";
var property = "nickName";
// 删除单个用户属性
analysys.profileUnSet( registerId, isLogin,property,platform);
// 或者也可以使用自定义的时间戳
var uploadTime = 1569859200000;
analysys.profileUnSet( registerId, isLogin,property,platform,uploadTime);


```

示例2：要清除已经设置的所有用户属性


var registerId = "ABCDEF123456789";
var isLogin = true;
var platform = "Android"
// 清除所有属性
analysys.profileDelete(registerId,isLogin,platform);
// 或者也可以使用自定义的时间戳
var uploadTime = 1569859200000;
analysys.profileDelete(registerId,isLogin,platform,uploadTime); 
```

###  4.2 通用属性

如果某个事件的属性，在所有事件中都会出现，则这个属性可以做为通用属性，通过将该属性设置为事件通用属性，则设置后每次发送事件的时候都会带有该属性。比如用户浏览/购买商品过程中的用户等级就可以作为通用属性。

通用属性是一个标准的 K-V 结构，K 和 V 均有相应的约束条件，如不符合则丢弃该次操作。

约束条件如下:

**属性名称**

以字母或 $ 开头，可包含字母、数字、下划线和 $，字母不区分大小写，$ 开头为预置事件属性,最大长度99字符,不支持乱码和中文。

**属性值**

支持部分类型：string/number/boolean/集合/数组；

若为字符串,则最大长度 255字符；

若为数组或集合,则最多包含 100条，且 key 约束条件与属性名称一致，value 最大长度 255字符。

#### 4.2.1 注册通用属性

以用户浏览/购买商品的过程中发生的事件为例，用户的级别/积分就可以作为通用的属性，通过把用户级别/积分注册为通用属性，就可以避免在每次收集事件属性的时候都要手工设置该属性值。接口如下：

```cpp
registerSuperProperty(key,value)
registerSuperProperties(params);
```

* key：单个属性的key
* value：单个属性的value
* params：设置多个属性

示例：

```cpp
// 设置单个通用属性
var key = "super"; //用户级别
var value = 0; //用户积分
analysys.registerSuperProperty(key,value);
// 设置多个通用属性
var superPropertie = {
    userLevel:0,
    userPoint:0
};
analysys.registerSuperProperties(superPropertie);
```

####  4.2.2 删除通用属性

如果要删除某个通用属性或者删除全部的通用属性，可以分别调用 unRegisterSuperProperty或 clearSuperAttributes接口。具体接口定义如下：

```cpp
unRegisterSuperProperty(key);
clearSuperProperties();
```

* key：属性名称

示例：

```cpp
示例1：删除设置的用户积分属性
// 删除单个通用属性
analysys.unRegisterSuperProperty("userPoint");
```

示例2：清除所有已经设置的通用属性


// 清除所有通用属性
analysys.clearSuperProperties();
```

#### 4.2.3 获取通用属性

由属性名称查询获取单条通用属性，或者获取全部的通用属性。接口如下：

```cpp
getSuperProperty(key);
getSuperProperties();
```

* Key：属性名称

示例：

```cpp
示例1：查看已经设置的 userLevel 通用属性

// 获取单个通用属性
analysys.getSuperProperty("userLevel");
```

示例2：查看所有已经设置的通用属性

// 获取所有通用属性
analysys.getSuperProperties();
```

### 4.3 刷新缓存

```cpp
analysys.flush();
```

立即发送所有收集的数据到服务器。

## 3.SDK示例

```cpp
var appId = "1234";
var upLoadURL = "http://192.168.0.3:8089";
var analysys = new AnalysysAgent({
    appId : appId,
    uploadURL:upLoadURL,
    debugMode:2,
    postNumber:10,
    postTime:10000
});
try {
    var distinctId = "1234567890987654321";
    var platform = "Android";
    //浏览商品
    var trackPropertie = {
        $ip:"112.112.112.112"    //IP地址
    };
    var bookList = ["Javascript权威指南"];
    trackPropertie.productName = bookList;  //商品列表
    trackPropertie.productType = "Js书籍"; //商品类别
    trackPropertie.producePrice = 80;		  //商品价格
    trackPropertie.shop = "xx网上书城";     //店铺名称
    analysys.track(distinctId,true,"ViewProduct", trackPropertie,platform);
    //用户注册登录
    var registerId = "ABCDEF123456789";
    analysys.alias(registerId,distinctId,platform);//设置公共属性
    var superPropertie = {
        sex:"male",          //性别
        age:23               //年龄
    };     
    analysys.registerSuperProperties(superPropertie);//用户信息
    var profiles = {
        $city:"北京",         //城市
        $province:"北京",     //省份
        nickName:"昵称123",   //昵称
        userLevel:0,         //用户级别
        userPoint:0          //用户积分
    };
    var interestList = ["户外活动","足球赛事","游戏"];
    profiles.interest =  interestList;//用户兴趣爱好
    analysys.profileSet(registerId, true,profiles,platform);//用户注册时间
    var profile_age = {
        registerTime:"20180101101010"
    };
    analysys.profileSetOnce(registerId, true, profile_age,platform);//重新设置公共属性
    analysys.clearSuperProperties();

    superPropertie = {
        userLevel:0,   //用户级别
        userPoint:0    //用户积分
    };
    analysys.registerSuperProperties(superPropertie);//再次浏览商品

    trackPropertie.$ip = "112.112.112.112"; //IP地址
    var bookList = ["Javascript权威指南"];
    trackPropertie.productName = bookList;  //商品列表
    trackPropertie.productType = "Js书籍";//商品类别
    trackPropertie.producePrice =  80;		  //商品价格
    trackPropertie.shop = "xx网上书城";     //店铺名称
    analysys.track(registerId, true,"ViewProduct", trackPropertie,platform);//订单信息

    trackPropertie.orderId = "ORDER_12345";
    trackPropertie.price = 80;
    analysys.track(registerId, true,"Order",trackPropertie,platform);//支付信息

    trackPropertie.orderId = "ORDER_12345";
    trackPropertie.productName = "Javascript权威指南";
    trackPropertie.productType =  "Js书籍";
    trackPropertie.producePrice = 8;
    trackPropertie.shop = "xx网上书城";
    trackPropertie.productNumber = 1;
    trackPropertie.price = 80;
    trackPropertie.paymentMethod = "AliPay";
    analysys.track( registerId, true,"Payment", trackPropertie,platform);
} catch (err) {
    analysys.flush();
}

```

