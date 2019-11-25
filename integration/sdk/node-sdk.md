---
description: Node SDK 主要用于服务端 Node 应用，如 Node Web 应用的后台服务。
---

# Node JS SDK

集成前请先[下载 SDK](https://ark.analysys.cn/sdk/v2/analysys_paas_Node_4.0.1_20191115.zip)

| 文件名 | 功能描述 | 是否必须 |
| :--- | :--- | :--- |
| AnalysysAgent\_NodeJS\_SDK.es6.js | SDK集成文件 | 二选一 |
| AnalysysAgent\_NodeJS\_SDK.cjs.js | SDK集成文件 | 二选一 |

## 1. 集成 SDK

SDK目前提供了两个版本 非es6（cjs） 和 es6 两个版本

### **1.1代码引入**

```javascript
// es6 方法引入
import AnalysysAgent from './sdk/AnalysysAgent_NodeJS_SDK.es6.js';
// cjs 方法引入
let AnalysysAgent = require('./sdk/AnalysysAgent_NodeJS_SDK.cjs.js');
```

## 2. SDK 初始化

### 2.1 获取配置信息

```javascript
let AgentConfig = {
    appId : 项目对应的appkey,
    uploadURL:数据接收地址,
    platform:应用的平台,
    debugMode:debug模式,
    postNumber:上传条数的设置,
    isLogin:是否是登陆状态,
    postTime:自动上传间隔时间
}
```

### 2.2 初始化接口

在程序启动时，调用构造函数 new AnalysysAgent\(AgentConfig\) 初始化 Node SDK 实例。如下：

```cpp
var analysys = new AnalysysAgent(AgentConfig);
```

### 2.3 实时收集器接口

用户每触发一次上传，则立即上传数据至接收服务器：

```cpp
postNumber:0
```

### 2.4 批量收集器接口

用户触发上传，该收集器将先缓存数据，直到数量达到用户设置的阈值或者用户设置的等待时间，才会触发真正的上传：

```cpp
postNumber:batchNum
postTime:batchSecnBatchNum：
//批量发送数量，默认值：20条
```

batchNum：批量发送数量，默认值：0条,实时上传  
batchSec：批量发送等待时间\(毫秒\)，默认值：0 毫秒

至此 SDK初始化完成，调用 SDK 提供的接口已可正常采集用户数据了。  
说明：程序结束前需要调用 `flush()` 接口，该接口可以把本地缓存的尚未发送至数据接收服务器的数据发送到数据接收服务器。

### 2.5 Debug 模式

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

### 2.6 统计事件

事件跟踪，设置事件名称和事件详细信息。接口如下：

```javascript
track(distinctId, isLogin, eventName, properties);
track(distinctId, isLogin, eventName, properties,upLoadTime);
```

* distinctId 用户 ID,长度大于 0 且小于 255字符
* isLogin 用户 ID 是否是登录 ID
* eventName 事件名称,以字母或 `$` 开头，可包含字母、数字、下划线和 `$`，字母不区分大小写，`$`开头为预置事件,不支持乱码和中文,最大长度 99字符
* properties 事件属性,最多包含 100条,且 key 以字母或 `$` 开头，可包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件属性,最大长度 99字符,不支持乱码和中文,value 类型约束\(String/Number/boolean/list/数组\)，若为字符串,最大长度255字符
* upLoadTime: 用户自定义时间戳\(带毫秒的13位时间戳\)

```cpp
// 浏览商品
var distinctId = "1234";
var isLogin = false;
var eventName = "ViewProduct";
var trackPropertie = {
    "$ip":"112.112.112.112"        //IP地址
};
var bookList = ["Javascript权威指南"];
trackPropertie.productName = bookList;  
trackPropertie.productType = "Js书籍";
trackPropertie.producePrice = 80;		  
trackPropertie.shop = "xx网上书城";     
analysys.track( distinctId, isLogin,eventName, trackPropertie,11111111111111111);
```

### 2.7 用户关联

用户 ID 关联接口。将 aliasId和 distinctId关联，计算时会认为是一个用户的行为。该接口是在 distinctId发生变化的时候调用，来告诉 SDK distinctId变化前后的 ID 对应关系。该场景一般应用在用户注册/登录的过程中。比如：一个匿名用户浏览商品，系统为其分配的distinctId = "1234567890987654321"，随后该匿名用户进行注册，系统为其分配了新的注册 ID，aliasId = "ABCDEF123456789"，此时就需要调用 alias 接口对两个 ID 进行关联。接口如下：

```cpp
alias(aliasId, distinctId);
alias(aliasId, distinctId, upLoadTime);
```

* aliasId：用户注册 ID，长度大于 0，且小于 255字符
* distinctId：用户匿名ID，长度大于 0，且小于 255字符
* upLoadTime：用户自定义时间戳\(带毫秒的13位时间戳\)

示例：匿名用户浏览商品到注册会员

```cpp
// 匿名ID
var distinctId = "1234567890987654321";
...
...
...
//用户注册登录
var registerId = "ABCDEF123456789";
analysys.alias(registerId,distinctId,11111111111111111);
```

### 2.8 用户属性设置

SDK提供以下接口供用户设置用户的属性，比如用户的年龄/性别等信息。

用户属性是一个标准的K-V结构，K和V均有相应的约束条件，如不符合则丢弃该次操作。 参数约束:

属性名称

以字母或$开头，可包含字母、数字、下划线和$，字母不区分大小写，$开头为预置事件属性,最大长度125字符,不支持乱码和中文

属性值

支持部分类型：string/number/boolean/集合/数组; 若为字符串,则最大长度255字符; 若为数组或集合,则最多包含100条,且key约束条件与属性名称一致,value最大长度255字符

设置单个或多个属性，如用户所在城市，用户昵称，用户头像信息等。如果之前存在，则覆盖，否则，新创建。接口如下：

```javascript
profileSet(distinctId, isLogin, properties);
profileSet(distinctId, isLogin, properties, upLoadTime);
```

* distinctId: 用户ID,长度大于0且小于255字符
* isLogin: 用户ID是否是登录 ID
* properties: 事件属性
* upLoadTime: 用户自定义时间戳\(带毫秒的13位时间戳\)

示例：用户注册后设置用户的注册信息属性

```cpp
var registerId = "ABCDEF123456789";
var isLogin = true;
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
analysys.profileSet(registerId, isLogin, profiles);
```

### 2.9 设置用户固有属性

只在首次设置时有效的属性。如：用户的注册时间。如果被设置的用户属性已存在，则这条记录会被忽略而不会覆盖已有数据，如果属性不存在则会自动创建。接口如下：

```cpp
profileSetOnce(distinctId, isLogin, properties);
profileSetOnce(distinctId, isLogin, properties, upLoadTime);
```

* distinctId: 用户ID,长度大于0且小于255字符
* isLogin: 用户ID是否是登录 ID
* properties: 事件属性
* upLoadTime: 用户自定义时间戳\(带毫秒的13位时间戳\)

示例：要统计用户注册时间

```cpp
var registerId = "ABCDEF123456789";
var isLogin = true;
var profileAge = {
    registerTime:"20180101101010"
};
analysys.profileSetOnce(registerId, true, profileAge);

```

### 2.10 设置用户属性相对变化值

设置用户属性的单个相对变化值\(相对增加,减少\)，只能对数值型属性进行操作，如果这个Profile之前不存在,则初始值为0。接口如下：

```cpp
profileIncrement(distinctId, isLogin, properties);
profileIncrement(distinctId, isLogin, properties, upLoadTime);
```

* distinctId: 用户ID,长度大于0且小于255字符
* isLogin: 用户ID是否是登录 ID
* properties: 事件属性
* upLoadTime: 用户自定义时间戳\(带毫秒的13位时间戳\)

示例：用户注册初始积分为0，在用户购买商品后，用户的积分增加20，则调用该接口，用户的积分变为0+20=20了：

```cpp
var registerId = "ABCDEF123456789";
var isLogin = true;
var profile = {
    userPoint:20
};
analysys.profileIncrement(registerId,isLogin,profile);
```

### 2.11 增加列表类型的属性

为列表类型的属性增加一个或多个元素，如：用户新增兴趣爱好，接口如下：

```cpp
profileAppend(distinctId, isLogin, properties);
profileAppend(distinctId, isLogin, properties, upLoadTime);
```

* distinctId: 用户ID,长度大于0且小于255字符
* isLogin: 用户ID是否是登录 ID
* properties: 事件属性
* upLoadTime: 用户自定义时间戳\(带毫秒的13位时间戳\)

示例：用户初始填写的兴趣爱好为\["sports"，"football"，"game"\]，调用该接口追加\["study"，"BodyBuilding"\]，则用户的爱好变为\["sports"，"football"，"game"，"study"，"BodyBuilding"\]

```cpp
var registerId = "ABCDEF123456789";
var isLogin = true;
var interestList = ["户外活动","足球赛事","游戏"];
var profile = {
    interest:interestList     //用户兴趣爱好
};
analysys.profileAppend(registerId, isLogin, profile);
```

### 2.12 删除设置的属性值

删除设置的属性值。接口如下：

```cpp
profileUnSet(distinctId, isLogin, property);
profileUnSet(distinctId, isLogin, property, upLoadTime);
profileDelete(distinctId, isLogin);
profileDelete(distinctId, isLogin, upLoadTime);
```

* distinctId: 用户ID,长度大于0且小于255字符
* isLogin: 用户ID是否是登录 ID
* properties: 事件属性
* upLoadTime: 用户自定义时间戳\(带毫秒的13位时间戳\)

```cpp
示例1： 要删除已经设置的用户昵称这一用户属性
var registerId = "ABCDEF123456789";
var isLogin = true;
var property = "nickName";
// 删除单个用户属性
analysys.profileUnSet( registerId, isLogin,property);
```

示例2：要清除已经设置的所有用户属性

```js
var registerId = "ABCDEF123456789";
var isLogin = true;
// 清除所有属性
analysys.profileDelete(registerId,isLogin);
```

###  2.13 通用属性

如果某个事件的属性，在所有事件中都会出现，则这个属性可以做为通用属性，通过将该属性设置为事件通用属性，则设置后每次发送事件的时候都会带有该属性。比如用户浏览/购买商品过程中的用户等级就可以作为通用属性。

通用属性是一个标准的 K-V 结构，K 和 V 均有相应的约束条件，如不符合则丢弃该次操作。

约束条件如下:

**属性名称**

以字母或 $ 开头，可包含字母、数字、下划线和 $，字母不区分大小写，$ 开头为预置事件属性,最大长度 125字符,不支持乱码和中文。

**属性值**

支持部分类型：string/number/boolean/集合/数组；

若为字符串,则最大长度 255字符；

若为数组或集合,则最多包含 100条，且 key 约束条件与属性名称一致，value 最大长度 255字符。

#### 2.13.1 注册通用属性

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

####  2.13.2 删除通用属性

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

```js
// 清除所有通用属性
analysys.clearSuperProperties();
```

#### 2.13.3 获取通用属性

由属性名称查询获取单条通用属性，或者获取全部的通用属性。接口如下：

```cpp
getSuperProperty(key);
getSuperProperties();;
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

### 2.14 刷新缓存

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
    platform:"Node",
    debugMode:2,
    postNumber:10,
    isLogin:false,
    postTime:10000
});
try {
    var distinctId = "1234567890987654321";
    //浏览商品
    var trackPropertie = {
        $ip:"112.112.112.112"    //IP地址
    };
    var bookList = ["Javascript权威指南"];
    trackPropertie.productName = bookList;  //商品列表
    trackPropertie.productType = "Js书籍"; //商品类别
    trackPropertie.producePrice = 80;		  //商品价格
    trackPropertie.shop = "xx网上书城";     //店铺名称
    analysys.track(distinctId,true,"ViewProduct", trackPropertie);
    //用户注册登录
    var registerId = "ABCDEF123456789";
    analysys.alias(registerId,distinctId);//设置公共属性
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
    analysys.profileSet(registerId, true,profiles);//用户注册时间
    var profile_age = {
        registerTime:"20180101101010"
    };
    analysys.profileSetOnce(registerId, true, profile_age);//重新设置公共属性
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
    analysys.track(registerId, true,"ViewProduct", trackPropertie);//订单信息

    trackPropertie.orderId = "ORDER_12345";
    trackPropertie.price = 80;
    analysys.track(registerId, true,"Order",trackPropertie);//支付信息

    trackPropertie.orderId = "ORDER_12345";
    trackPropertie.productName = "Javascript权威指南";
    trackPropertie.productType =  "Js书籍";
    trackPropertie.producePrice = 8;
    trackPropertie.shop = "xx网上书城";
    trackPropertie.productNumber = 1;
    trackPropertie.price = 80;
    trackPropertie.paymentMethod = "AliPay";
    analysys.track( registerId, true,"Payment", trackPropertie);
} catch (err) {
    analysys.flush();
}

```

