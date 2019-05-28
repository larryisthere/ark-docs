# 微信小程序 SDK

## 微信小程序 SDK 使用说明

微信小程序 SDK 适用于原生微信小程序，集成前请先[下载 SDK](https://ark.analysys.cn/sdk/v2/analysys_paas_WX_v4.2.2_20190525.zip)

| js文件 | 功能描述 | 是否必须 |
| :---: | :---: | :---: |
| AnalysysAgent\_WX\_SDK.min.js | 基础模块SDK | 必须 |

注意：请您根据自身业务需求来引用相关的SDK。

### 快速集成

如果您是第一次使用易官方舟产品，可以通过阅读本文快速了解此产品

#### 1. 集成 SDK

在app.js文件的顶部引入SDK。

#### 2. 设置初始化接口

通过初始化代码的配置参数配置您的AppKey。

#### 3. 设置上传地址

通过初始化代码的配置参数uploadURL设置您上传数据的地址。

#### 4. 配置上传地址域名

登录微信公众平台，将上传地址域名配置到request 合法域名中。

#### 5. 设置需要采集的页面或事件

通过手动埋点，设置需要采集的页面或事件。

#### 6. 打开 Debug 模式查看日志

通过设置Ddebug模式，开/关 log 查看日志。

通过以上6步您即可验证SDK是否已经集成成功。更多接口说明请您查看API文档。

## 集成配置

### 集成 SDK

将 AnalysysAgent\_WX\_SDK.min.js 文件放到小程序的目录下

![ ](http://imguserradar.analysys.cn/fangzhou/img/2018/09/201809191614101827.png)

在小程序的 app.js 文件中的第一行加入以下代码:

```javascript
let AnalysysAgent = require("./build/AnalysysAgent_WX_SDK.min.js")
AnalysysAgent.appkey = "/*设置为实际APPKEY*/" //APPKEY
```

在各个 Page 内通过以下代码获取 AnalysysAgent\_WX\_SDK 全局函数:

```javascript
let app = getApp();
let AnalysysAgent = app.AnalysysAgent;
```

请注意:

```text
1.将 appkey 的值填入您具体的项目 appkey

2.目录为您所引入微信小程序 SDK 的具体目录
```

### 配置参数

* _appkey_\(必须\) 在网站获取的 AppKey
* _debugMode_ 设置调试模式：0 - 关闭调试模式\(默认\)；1 - 开启调试模式，数据不入库；2 - 开启调试模式，数据入库
* _uploadURL_\(必须\) 自定义上传地址
* _auto_ 设置打开/关闭自动采集页面：false - 关闭自动采集；true - 开启自动采集\(默认\)
* _autoProfile_ 设置是否追踪新用户的首次属性：false - 不追踪新用户的首次属性；true - 追踪新用户的首次属性\(默认\)
* _encryptType_ 设置是否对上传数据加密：0 - 对上传数据不加密\(默认\)；1 - 对上传数据AES加密

#### appkey

appkey 在网站获取的 AppKey。

* value 在网站获取的 AppKey。类型:String。取值长度 1 - 255字符。

```javascript
// 设置key，77a52s552c892bn442v721为样例数据，请根据实际情况替换相应内容
AnalysysAgent.appkey = "77a52s552c892bn442v721"
```

#### debugMode

debugMode 调试模式为接入 JS SDK 后进行数据调试的主要手段。可实时验证 JS SDK 数据监测的正确与否。

* 0 关闭调试模式\(默认\)。类型：Number。
* 1 开启调试模式，数据不入库。类型：Number。
* 2 开启调试模式，数据入库。类型：Number。

```javascript
//开启调试模式且数据不入库
AnalysysAgent.debugMode = 1
//开启调试模式且数据入库
AnalysysAgent.debugMode = 2
//关闭调试模式
AnalysysAgent.debugMode = 0
//或删除该段代码
```

或删除 debugMode 参数。

#### uploadURL

uploadURL 为自定义上传地址，参数设置后，所有事件信息将上传到该地址。

* value 类型：String。数据上传地址，格式为 scheme://host + :port\(不包含/后的内容\)。scheme 必须以 http:// 或 https:// 开头，host 只支持域名和 IP，取值长度 1 - 255字符，port 端口号必须携带

```javascript
//设置自定义上传地址为 scheme://host + :port
AnalysysAgent.uploadURL = "/*设置为实际地址*/"
```

#### auto

auto 为设置打开/关闭自动采集页面的参数。可根据自身需要进行更改。

* true 开启自动采集页面打开事件\(默认\)。类型：Boolean。
* false 关闭自动采集页面打开事件。类型：Boolean。

```javascript
//关闭自动采集页面打开事件，关闭后可使用微信小程序 SDK的API中的手动发送页面打开数据方法，来发送页面打开状态的数据。
AnalysysAgent.auto = false
//开启自动采集页面打开事件。
AnalysysAgent.auto = true //或删除该行代码。
```

#### autoProfile

autoProfile 为设置是否追踪新用户的首次属性。可根据自身需要进行更改。

* true 追踪新用户的首次属性\(默认\)。类型：Boolean。
* false 不追踪新用户的首次属性。类型：Boolean。

```javascript
//不追踪新用户的首次属性，新用户首次打开网站不上传新用户的首次属性。
AnalysysAgent.autoProfile = false
//追踪新用户的首次属性，新用户首次打开网站上传新用户的首次属性。
AnalysysAgent.autoProfile = true//或删除该行代码。
```

#### encryptType

encryptType 为设置数据上传时的加密方式,目前只支持 AES 加密，如不设置此参数，数据上传不加密。。可根据自身需要进行更改。

* 0 对上传数据不加密\(默认\)。类型：Number。
* 1 对上传数据 AES 加密。类型：Number。

```javascript
//对上传数据不加密。
AnalysysAgent.encryptType = 0//或删除该行代码。
//对上传数据AES加密。
AnalysysAgent.encryptType = 1
```

### 域名配置

登录微信公众平台，设置&gt;开发设置&gt;服务器域名&gt;request 合法域名，加入您所配置的 `https` 域名：`https://xxx.xxx.xxx`

![ ](http://imguserradar.analysys.cn/fangzhou/img/2018/09/201809191614109548.jpg)

## 基础模块介绍

### 统计页面接口介绍

页面跟踪，SDK 默认设置跟踪所有页面，支持自定义页面信息。接口如下：

```javascript
AnalysysAgent.pageView(pageName);
AnalysysAgent.pageView(pageName, properties);
```

* pageName：页面标识，为字符串，取值长度 1 - 255字符
* properties：页面信息，properties 最多包含 100条，且 key 以字母或 `$` 开头，包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件/属性，不支持乱码和中文，取值长度 1 - 255字符，value 支持类型：String/Number/boolean/JSON/内部元素为String的Array，若为字符串，取值长度 1 - 255字符

示例：

```javascript
// 正在开展某个活动，需要统计活动页面；
AnalysysAgent.pageView("活动页");

......

// 访问手机活动页面，活动页面内容为优惠出售iPhone手机，手机价格为5000元
var properties ={
    "commodityName": "iPhone",
    "commodityPrice": 8000
}

AnalysysAgent.pageView("商品页", properties);
```

### 统计事件接口

用户行为追踪，可以设置自定义属性。接口如下：

```javascript
AnalysysAgent.track(eventName, eventInfo)
```

* eventName：事件名称，以字母或 `$` 开头，包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件/属性，不支持乱码和中文，取值长度 1 - 99字符
* eventInfo：自定义属性，用于对事件描述。eventInfo 最多包含 100条，且 key 以字母或 `$` 开头，包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件/属性，不支持乱码和中文，取值长度 1 - 125字符，value 支持类型：String/Number/boolean/JSON/内部元素为String的Array，若为字符串，取值长度 1 - 255字符

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

### 设备ID与用户关联

用户 id 关联接口。将需要绑定的用户ID 和设备ID进行关联，计算时会认为是一个用户的行为。接口如下：

```javascript
AnalysysAgent.alias(aliasId, originalId);
```

* aliasId：新的唯一用户 id。 取值长度 1 - 255字符,支持类型：String
* originalId：待关联的设备ID，可以是现在使用也可以是历史使用的设备ID,不局限于本地正使用的设备ID。 可以为空值，若为空时使用本地的设备ID。取值长度 1 - 255 字符（如无特殊需求，不建议设置），支持类型：String

示例：

```javascript
// 登陆账号时调用，只设置当前登陆账号即可和之前行为打通
AnalysysAgent.alias("sanbo");

......

//现在登陆账号是zhangsan，和历史上的 lisi是一个人。 此时不会关心登陆 zhangsan前的用户是谁
AnalysysAgent.alias("zhangsan", "lisi");
```

### 设备ID设置

唯一设备ID标识设置，接口如下：

```javascript
AnalysysAgent.identify(distinctId);
```

* distinctId：唯一身份标识，取值长度 1 - 255字符,支持类型：String

示例:

```text
// 设置设备ID为`fangke009901`,注意此方法需要在初始化之后优先调用
AnalysysAgent.identify("fangke009901");
```

### 设备ID获取

获取用户通过identify接口设置或自动生成的id，优先级如下： 用户设置的id &gt; 代码自动生成的id

接口如下：

```javascript
AnalysysAgent.getDistinctId();
```

示例:

```javascript
// 获取匿名id
var distinctId = AnalysysAgent.getDistinctId();
```

### 用户属性设置

> 用户属性是一个标准的 K-V 结构，K 和 V 均有相应的约束条件，如不符合则丢弃该次操作。

约束条件如下：

> **属性名称**

```text
以字母或`$`开头，包含字母、数字、下划线和`$`，字母不区分大小写，`$`开头为预置事件/属性，则取值长度 1 - 125 字符，不支持乱码和中文
```

> **属性值**

```text
支持部分类型：String/Number/Boolean/JSON/内部元素为String的Array；若为字符串，则取值长度 1 - 255字符；若为 Array 或 JSON，则最多包含 100条，且 key 约束条件与属性名称一致，value 取值长度 1 - 255字符
```

#### 设置用户固有属性

设置用户的固有属性，只在首次设置时有效的属性。 如：应用的激活时间、首次登录时间等。如果被设置的用户属性已存在，则这条记录会被忽略而不会覆盖已有数据，如果属性不存在则会自动创建。接口如下：

```javascript
AnalysysAgent.profileSetOnce(propertyName, propertyValue);

AnalysysAgent.profileSetOnce(property);
```

* propertyName ：属性名称，约束见[属性名称](wx.md#1.1)
* propertyValue ：属性值，约束见[属性值](wx.md#2.1)
* property ： 属性列表，约束见[属性名称](wx.md#1.1)，[属性值](wx.md#2.1)

示例：

```javascript
// 设置用户激活时间
AnalysysAgent.profileSetOnce("activationTime",1521280551929);

// 设置用户性别和出生时间
var setOnceProfile = {
    "birth": 548798705000,
    "sex": "male"
}
AnalysysAgent.profileSetOnce(setOnceProfile);
```

#### 设置用户属性

给用户设置单个或多个属性，如果之前不存在，则新建，否则覆盖。接口如下：

```javascript
//设置单个用户属性
AnalysysAgent.profileSet(propertyName, propertyValue);
//设置多个用户属性
AnalysysAgent.profileSet(property);
```

* propertyName ：属性名称，约束见[属性名称](wx.md#1.1)
* propertyValue ：属性值，约束见[属性值](wx.md#2.1)
* property ：属性列表，约束见[属性名称](wx.md#1.1)，[属性值](wx.md#2.1)

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

#### 设置用户属性相对变化值

设置用户属性的相对变化值\(相对增加，减少\)，只能对数值型属性进行操作，如果这个 Profile之前不存在，则初始值为0。接口如下：

```javascript
AnalysysAgent.profileIncrement(propertyName, propertyNumber)

AnalysysAgent.profileIncrement(property);
```

* propertyName：属性名称，约束见[属性名称](wx.md#1.1)
* propertyValue：属性值，约束见[属性值](wx.md#2.1)
* property：属性列表，约束见[属性名称](wx.md#1.1)，[属性值](wx.md#2.1)

示例：

```javascript
//用户增加一岁
AnalysysAgent.profileIncrement("age",1);

......

//用户玩某个游戏时间增加一年，游戏等级增加2
var incrementProfile = {
    "gameAge":1,
    "gameRating":2
}
AnalysysAgent.profileIncrement(incrementProfile);
```

#### 增加列表类型的属性

用户列表属性增加元素。接口如下：

```javascript
//增加单个属性
AnalysysAgent.profileAppend(propertyName, propertyValue);
//增加多个属性
AnalysysAgent.profileAppend(propertyValue);
//增加多个属性
AnalysysAgent.profileAppend(propertyName, propertyValue);
```

* propertyName：属性名称，约束见[属性名称](wx.md#1.1)
* propertyValue：属性值，约束见[属性值](wx.md#2.1)

示例：

```javascript
// 增加用户爱好 music
AnalysysAgent.profileAppend("hobby", "Music");

// 增加多个用户属性
var map = {
    "hobby": "PlayBasketball",
    "sports": "Run"
};
AnalysysAgent.profileAppend(mContext, map);

//增加多个用户爱好
var list = ["PlayBasketball", "music"];
AnalysysAgent.profileAppend("hobby", list);
```

#### 删除设置的属性

删除已设置的用户属性。接口如下：

```javascript
AnalysysAgent.profileUnset(propertyName);
AnalysysAgent.profileDelete();
```

* propertyName：属性名称，约束见[属性名称](wx.md#1.1)

示例：

```javascript
// 删除单个用户属性
AnalysysAgent.profileUnset( "age");

// 清除所有属性
AnalysysAgent.profileDelete();
```

### 通用属性

> 通用属性是每次上传事件信息都会带有的属性，通用属性是一个标准的 K-V 结构，K 和 V 均有相应的约束条件，如不符合则丢弃该次操作。

约束条件如下:

> **属性名称**

```text
以字母或`$`开头，包含字母、数字、下划线和`$`，字母不区分大小写，`$`开头为预置事件/属性，则取值长度 1 - 125 字符，不支持乱码和中文
```

> **属性值**

```text
支持部分类型：String/Number/boolean/JSON/内部元素为String的Array；若为字符串，则取值长度 1 - 255字符；若为 Array 或 JSON,则最多包含 100条，且 key 约束条件与属性名称一致，value 取值长度 1 - 255字符
```

#### 注册通用属性

某一个体，在固定范围内，持续拥有的属性，每次数据上传都会携带。接口如下:

```javascript
AnalysysAgent.registerSuperProperty(superPropertyName , superPropertyValue );

AnalysysAgent.registerSuperProperties(superProperty);
```

* superPropertyName：属性名称，约束见[属性名称](wx.md#1)
* superPropertyValue：属性值，约束见[属性值](wx.md#2)
* superProperty：属性列表，约束见[属性名称](wx.md#1)，[属性值](wx.md#2)

示例：

```javascript
// 在某视频平台，购买一年会员，该年内只需设置一次即可
AnalysysAgent.registerSuperProperty("member","VIP");

......

// 小明在20岁的时候，购买了一年腾讯会员
var property = {
    "platform":"TX",
    "age":"20",
    "member":"VIP",
    "user":"xiaoming"
}
AnalysysAgent.registerSuperProperties(property);
```

#### 删除通用属性

根据属性名称，删除已设置过的通用属性。接口如下：

```javascript
//删除单个通用属性
AnalysysAgent.unRegisterSuperProperty(superPropertyName);
//清除所有通用属性
AnalysysAgent.clearSuperProperties();
```

* superPropertyName：属性名称，约束见[属性名称](wx.md#1)

示例：

```javascript
// 删除已经设置的用户年龄属性
AnalysysAgent.unRegisterSuperProperty("age");

......

// 清除所有已经设置的通用属性
AnalysysAgent.clearSuperProperties();
```

#### 获取通用属性

查询获取通用属性。接口如下：

```javascript
// 获取单个通用属性
AnalysysAgent.getSuperProperty(superPropertyName);
// 获取所有的通用属性
AnalysysAgent.getSuperProperties();
```

* superPropertyName：属性名称，约束见[属性名称](wx.md#1)

示例：

```javascript
// 查看已经设置的"member"的通用属性
AnalysysAgent.getSuperProperty("member");

......

// 查看所有已经设置的通用属性
AnalysysAgent.getSuperProperties();
```

### 清除本地设置

清除本地现有的设置（包括 id 和通用属性）重新开始统计。接口如下：

```javascript
AnalysysAgent.reset();
```

示例：清除本地现有的设置，包括id和通用属性

```javascript
// 删除设置的id和通用属性
AnalysysAgent.reset();
```

