---
description: 快应用小程序 框架版 SDK 使用说明
---

# 快应用 SDK

快应用小程序SDK集成前请先下载SDK

{% hint style="info" %}
SDK Releases包下载：  
Github地址\(推荐\)：[https://github.com/analysys/ans-QuickApp-sdk/releases](https://github.com/analysys/ans-QuickApp-sdk/releases)  
Releases中含有更新说明请您阅读，接口使用请参考本文档。
{% endhint %}

| js文件 | 功能描述 | 是否必须 |
| :---: | :---: | :---: |
| AnalysysAgent\_QuickApp\_SDK.custom.min.js | 基础模块SDK | 二选一 |
| AnalysysAgent\_QuickApp\_SDK.es6.custom.min.js | es6基础模块SDK | 二选一 |
| AnalysysAgent\_encryption.min.js | 加密模块 | 非必须 |
| AnalysysAgent\_encryption.es6.min.js | es6加密模块 | 非必须 |

### 快速集成

如果您是第一次使用易观方舟产品，可以通过阅读本文快速了解此产品

**1. 集成 SDK**

在app.js文件的顶部引入SDK。

**2. 设置初始化接口**

通过初始化代码的配置参数配置您的AppKey。

**3. 设置上传地址**

通过初始化代码的配置参数uploadURL设置您上传数据的地址。

**4. 设置需要采集的页面或事件**

通过手动埋点，设置需要采集的页面或事件。

**5. 打开 Debug 模式查看日志**

通过设置Ddebug模式，开/关 log 查看日志。

**6. 调用快应用启动事件**

在app.ux 设定 AnalysysAgent 为全局函数方便调用

```javascript
import AnalysysAgent from './util/sdk/AnalysysAgent_QuickApp_SDK.custom.es6.min.js'

export default{
    onCreate() {
        AnalysysAgent.appkey = "/*设置为实际APPKEY*/" //APPKEY
        AnalysysAgent.uploadURL = "/*设置为实际地址*/"
        this.AnalysysAgent = AnalysysAgent
    }
}
```

在入口文件首页 page.ux文件中调用启动事件

```javascript
export default{
    onShow () {
        this.$app.AnalysysAgent.appStart(this.$page);
    }
}
```

其中 this.$page 为页面参数，包含UTM、场景值等，为必传字段，否则统计将会缺少字段上报。 utm

```javascript
 public: {
    // 获取页面参数，快应用通过public定义 key 名相同的属性获取外部参数;如果参数 key 未被声明，public 不会新增这个属性，即获取不到参数值。
    // 不同的厂商对该能力可能有不同限制，使用前请和相应厂商确认。
    // https://doc.quickapp.cn/tutorial/platform/deeplink.html  
    campaign_id: null,
    utm_source:null,
    utm_medium:null,
    utm_term:null,
    utm_content:null,
    utm_campaign:null
  },
```

**7. 调用快应用统计页面事件**

在每一个页面的入口js文件中调用快应用统计页面事件

```javascript
export default{
    onShow () {
        this.$app.AnalysysAgent.pageView('首页');//页面名称可自定义。
    }
}
```

通过以上7步您即可验证SDK是否已经集成成功。更多接口说明请您查看API文档。

## 集成配置

### 集成 SDK

将 AnalysysAgent\_QuickApp\_SDK.es6.min.js 文件放到快应用的目录下

![](../../.gitbook/assets/wechatimg736.jpg)

在快应用的 app.ux 文件中的第一行加入以下代码:

```javascript
import AnalysysAgent from './util/sdk/AnalysysAgent_QuickApp_SDK.custom.es6.min.js'
export default{
    onCreate()
        AnalysysAgent.appkey = "/*设置为实际APPKEY*/" //APPKEY
        AnalysysAgent.uploadURL = "/*设置为实际地址*/"
    }
}
```

假如需要加密模块

```javascript
import AnalysysAgent from './util/sdk/AnalysysAgent_QuickApp_SDK.custom.es6.min.js'
import AnalysysEncryption from './util/sdk/AnalysysAgent_encryption.es6.min.js'
export default{
    onCreate()
        AnalysysAgent.appkey = "/*设置为实际APPKEY*/" //APPKEY
        AnalysysAgent.uploadURL = "/*设置为实际地址*/"
        AnalysysAgent.encrypt = AnalysysEncryption
    }
}
```

es6版本不是每个框架都能用，不能使用es6的请使用如下代码:

```javascript
let AnalysysAgent = require('./util/sdk/AnalysysAgent_QuickApp_SDK.custom.min.js')
export default{
    onCreate()
        AnalysysAgent.appkey = "/*设置为实际APPKEY*/" //APPKEY
        AnalysysAgent.uploadURL = "/*设置为实际地址*/"
    }
}
```

假如需要加密模块

```javascript
let AnalysysAgent = require('./util/sdk/AnalysysAgent_QuickApp_SDK.custom.min.js')
let AnalysysEncryption = require('./util/sdk/AnalysysAgent_encryption.min.js')
export default{
    onCreate()
        AnalysysAgent.appkey = "/*设置为实际APPKEY*/" //APPKEY
        AnalysysAgent.uploadURL = "/*设置为实际地址*/"
        AnalysysAgent.encrypt = AnalysysEncryption
    }
}
```

在各个 Page 内通过以下代码获取 AnalysysAgent\_QuickApp\_SDK 全局函数:

```javascript
this.$app.AnalysysAgent;
```

请注意:

```text
1.将 appkey 的值填入您具体的项目 appkey

2.目录为您所引入快应用 SDK 的具体目录
```

### 配置参数

* _appkey_\(必须\) 在网站获取的 AppKey
* _debugMode_ 设置调试模式：0 - 关闭调试模式\(默认\)；1 - 开启调试模式，数据不入库；2 - 开启调试模式，数据入库
* _uploadURL_\(必须\) 自定义上传地址
* _autoProfile_ 设置是否追踪新用户的首次属性：false - 不追踪新用户的首次属性；true - 追踪新用户的首次属性\(默认\)
* _encryptType_ 设置是否对上传数据加密：0 - 对上传数据不加密\(默认\)；1 - 对上传数据进行AES 128位ECB加密；2 对上传数据进行AES 128位CBC加密
* _allowTimeCheck_ 设置是否开启时间校准：false\(默认\) - 关闭时间校准；true - 开启时间校准
* _maxDiffTimeInterval_ 设置最大时间校准分为：30s\(默认\) ，当设置的时间差值小于他，将不开启校准。否则将会进行时间校准。
* _requestDataType_ 设置返回值类型，一般不用设置，当方舟返回密文且上报是加密的时候需要设置成 text，默认 json。

**appkey**

appkey 在网站获取的 appkey。

* value 在网站获取的 appkey。类型:String。取值长度 1 - 255字符。

```javascript
// 设置key，77a52s552c892bn442v721为样例数据，请根据实际情况替换相应内容
AnalysysAgent.appkey = "77a52s552c892bn442v721"
```

**debugMode**

debugMode 调试模式为接入 钉钉 SDK 后进行数据调试的主要手段。可实时验证 钉钉 SDK 数据监测的正确与否。

* 0 关闭调试模式\(默认\)。类型：Number。
* 1 开启调试模式，数据不入库。类型：Number。
* 2 开启调试模式，数据入库。类型：Number。

```javascript
//开启调试模式且数据不入库
AnalysysAgent.debugMode = 1
//开启调试模式且数据入库
AnalysysAgent.debugMode = 2
//关闭调试模式 或删除该段代码
AnalysysAgent.debugMode = 0
```

**uploadURL**

uploadURL 为自定义上传地址，参数设置后，所有事件信息将上传到该地址。

* value 类型：String。数据上传地址，格式为 [https://host\(不包含/后的内容\)。host](https://host%28不包含/后的内容%29。host) 只支持域名，取值长度 1 - 255字符，不可携带端口号

```javascript
//设置自定义上传地址为 https://host
AnalysysAgent.uploadURL = "/*设置为实际地址*/"
```

**autoProfile**

autoProfile 为设置是否追踪新用户的首次属性。可根据自身需要进行更改。

* true 追踪新用户的首次属性\(默认\)。类型：Boolean。
* false 不追踪新用户的首次属性。类型：Boolean。

```javascript
//不追踪新用户的首次属性，新用户首次打开网站不上传新用户的首次属性。
AnalysysAgent.autoProfile = false
//追踪新用户的首次属性，新用户首次打开网站上传新用户的首次属性。
AnalysysAgent.autoProfile = true//或删除该行代码。
```

**encryptType**

encryptType 为设置数据上传时的加密方式,目前只支持 AES 加密，如不设置此参数，数据上传不加密。可根据自身需要进行更改。

* 0 对上传数据不加密\(默认\)。类型：Number。
* 1 对上传数据进行AES 128位ECB加密。类型：Number。
* 2 对上传数据进行AES 128位CBC加密。类型：Number。

```javascript
//对上传数据不加密。
AnalysysAgent.encryptType = 0//或删除该行代码。
//对上传数据AES加密。
AnalysysAgent.encryptType = 1
```

**allowTimeCheck**

allowTimeCheck 为设置是否开启时间校准，开启时间校准在debug 1或者 2 的情况下会有相关提示。

* false 关闭时间校准\(默认\)。类型：Boolean。
* true 开启时间校准。类型：Boolean。

```javascript
//关闭时间校准。
AnalysysAgent.allowTimeCheck = false//或删除该行代码。
//开启时间校准。
AnalysysAgent.allowTimeCheck = true
```

**maxDiffTimeInterval**

maxDiffTimeInterval 为设置不校准时间的最大时间差值。当客户端时间和服务端时间相差在此区间内，将不进行时间校准，否则将进行时间校准。

value：类型 Number 。默认值 30。单位：秒。

```javascript
//设置最大 允许时间 。
AnalysysAgent.maxDiffTimeInterval = 20 // 当服务端和客户端的时间差超过 20s 将进行时间校准
```

## 基础模块介绍

### 启动事件接口

启动事件 appStart\(options\),快应用SDK启动事件需要手动调用，而且只能调用一次。

```javascript
AnalysysAgent.appStart(options);
```

* options：options为快应用 页面获取到的参数，包括query、url等,不同框架，不同方式获取，请开发者根据使用的框架获取。

> 原生
>
> ```javascript
>  onshow(){
>         const params = this.$pages
>         AnalysysAgent.appStart(params);
>  }
> ```
>
> 框架taro:
>
> ```javascript
> componentDidShow () {
>         const params = this.$router.params
>         AnalysysAgent.appStart(params);
> }
> ```

### 统计页面接口

页面跟踪，快应用SDK需要手动设置跟踪所有页面，支持自定义页面信息。接口如下：

```javascript
AnalysysAgent.pageView(pageName);
AnalysysAgent.pageView(pageName, properties);
```

* pageName：页面标识，为字符串，取值长度 1 - 255字符
* properties：页面信息，为K-V键值对，最多包含 100条，且key是以字母开头的字符串，必须由 字母、数字、下划线组成，字母不区分大小写，不支持 乱码、中文、空格等，长度范围1-99字符；value支持类型：String/Number/Boolean/JSON/内部元素为String的Array，若为字符串，长度范围1-255字符。

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

* eventName：自定义事件ID标识，以字母开头的字符串,必须由字母、数字、下划线组成，$ 开头为预置事件/属性，不支持乱码、中文、空格等，长度范围1-99字符。
* eventInfo：自定义属性，K-V键值对，用于对事件的描述。最多包含100条，且key是以字母开头的字符串，必须由 字母、数字、下划线组成，字母不区分大小写，不支持 乱码、中文、空格等，长度范围1-99字符；value支持类型：String/Number/Boolean/JSON/内部元素为String的Array，若为字符串，长度范围1-255字符。

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

### 注册页面事件通用属性

注册应用中所有页面通用属性，设置后当次快应用启动后所有页面都拥有该属性，直至该快应用关闭。接口如下：

```javascript
AnalysysAgent.appProperty(properties)
```

* properties：页面信息，K-V键值对，最多包含100条，且key是以字母开头的字符串,必须由 字母、数字、下划线组成，字母不区分大小写，不支持 乱码、中文、空格等，长度范围1-99字符；value支持类型：String/Number/Boolean/JSON/内部元素为String的Array，若为字符串，长度范围1-255字符。

示例：

```javascript
// 设置被分享页面所属分享群ID
var properties ={
    openGId:'123456789'
}

AnalysysAgent.appProperty(properties);
```

### 采集分享按钮点击事件

采集分享按钮点击事件，只采集分享按钮的点击事件，不区分分享是否成功。方法返回对象（toShareProperties）。接口如下：

```javascript
AnalysysAgent.share(toShareProperties,trackProperties);
```

* toShareProperties\(可选\)，分享属性，包括自定义title等。
* trackProperties（可选），分享事件自定义属性。K-V键值对，最多包含100条，且key是以字母开头的字符串，必须由字母、数字、下划线组成，字母不区分大小写，不支持乱码、中文、空格等，长度范围1-99字符；value支持类型：String/Number/Boolean/JSON/内部元素为String的Array，若为字符串，长度范围1-255字符。

示例：

```javascript
share(){
    let _this = this;
    // 手动采集
    share.share({
        type: '用户自定义',
        data: '用户自定义',
        success: function(data) {
            _this.$app.AnalysysAgent.share()   // 注意
        },
        fail: function(data, code) {
            console.log(`handling fail, code = ${code}`)
        }
    })
}
```

### 匿名ID与用户关联

用户 id 关联接口。将需要绑定的 'aliasId' 和 设备ID 进行关联，计算时会认为是一个用户的行为。接口如下：

```javascript
AnalysysAgent.alias(aliasId);
```

* aliasId：新的唯一用户 id。 取值长度 1 - 255字符,支持类型：String

示例：

```javascript
// 登陆账号时调用，只设置当前登陆账号即可和之前行为打通
AnalysysAgent.alias("sanbo");
```

### 匿名ID设置

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

### 匿名ID获取

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

> 用户属性是一个标准的 K-V 结构，K 和 V 均有相应的约束条件，如不符合则日志提醒，但是还是会原文上报。

约束条件如下：

> **属性名称**

```text
以字母开头的字符串，必须由字母、数字、下划线组成，字母不区分大小写，$ 开头为预置事件/属性，不支持乱码、中文、空格等，长度范围1-99字符。
```

> **属性值**

```text
支持部分类型：String/Number/Boolean/内部元素为String的Array；若为字符串，则取值长度 1 - 255 字符；若为数组或集合，则最多包含 100条，且 key 约束条件与属性名称一致，value 取值长度 1 - 255 字符
```

**设置用户固有属性**

设置用户的固有属性，只在首次设置时有效的属性。 如：应用的激活时间、首次登录时间等。如果被设置的用户属性已存在，则这条记录会被忽略而不会覆盖已有数据，如果属性不存在则会自动创建。接口如下：

```javascript
AnalysysAgent.profileSetOnce(propertyName, propertyValue);

AnalysysAgent.profileSetOnce(property);
```

* propertyName ：属性名称，约束见[属性名称](./#1.1)
* propertyValue ：属性值，约束见[属性值](./#2.1)
* property ： 属性列表，约束见[属性名称](./#1.1)，[属性值](./#2.1)

示例：

```javascript
// 设置用户激活时间
AnalysysAgent.profileSetOnce("activationTime", "2018-06-18 18:18:18.188");

// 设置用户性别和出生时间
var setOnceProfile = {
    "birth": "2018-06-18 18:18:18.188",
    "sex": "male"
}
AnalysysAgent.profileSetOnce(setOnceProfile);
```

**设置用户属性**

给用户设置单个或多个属性，如果之前不存在，则新建，否则覆盖。接口如下：

```javascript
//设置单个用户属性
AnalysysAgent.profileSet(propertyName, propertyValue);
//设置多个用户属性
AnalysysAgent.profileSet(property);
```

* propertyName ：属性名称，约束见[属性名称](./#1.1)
* propertyValue ：属性值，约束见[属性值](./#2.1)
* property ：属性列表，约束见[属性名称](./#1.1)，[属性值](./#2.1)

示例：

```javascript
//设置用户的邮箱地址为yonghu@163.com
AnalysysAgent.profileSet("Email", "yonghu@163.com");

......

// 设置用户的邮箱和ID
var property = {
    "Email" : "yonghu@163.com",
    "ID" : "zhanghao"
}
AnalysysAgent.profileSet(property);
```

**设置用户属性相对变化值**

设置用户属性的相对变化值\(相对增加，减少\)，只能对数值型属性进行操作，如果这个 Profile之前不存在，则初始值为0。接口如下：

```javascript
AnalysysAgent.profileIncrement(propertyName, propertyNumber)

AnalysysAgent.profileIncrement(property);
```

* propertyName：属性名称，约束见[属性名称](./#1.1)
* propertyValue：属性值，约束见[属性值](./#2.1)
* property：属性列表，约束见[属性名称](./#1.1)，[属性值](./#2.1)

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

**增加列表类型的属性**

用户列表属性增加元素。接口如下：

```javascript
//增加单个属性
AnalysysAgent.profileAppend(propertyName, propertyValue);
//增加多个属性
AnalysysAgent.profileAppend(propertyValue);
//增加多个属性
AnalysysAgent.profileAppend(propertyName, propertyValue);
```

* propertyName：属性名称，约束见[属性名称](./#1.1)
* propertyValue：属性值，约束见[属性值](./#2.1)

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

**删除设置的属性值**

删除已设置的用户属性值。接口如下：

```javascript
AnalysysAgent.profileUnset(propertyName);
AnalysysAgent.profileDelete();
```

* propertyName：属性名称，约束见[属性名称](./#1.1)

示例：

```javascript
//  删除当前用户单个属性值
AnalysysAgent.profileUnset( "age");

//  删除当前用户所有属性值
AnalysysAgent.profileDelete();
```

### 通用属性

> 通用属性是每次上传事件信息都会带有的属性，通用属性是一个标准的 K-V 结构，K 和 V 均有相应的约束条件，如不符合则日志提醒，但是还是会原文上报。

约束条件如下:

> **属性名称**

```text
以字母开头的字符串，必须由字母、数字、下划线组成，字母不区分大小写，$ 开头为预置事件/属性，不支持乱码、中文、空格等，长度范围1-99字符。
```

> **属性值**

```text
支持部分类型：String/Number/boolean/内部元素为String的Array；若为字符串，则取值长度 1 - 255 字符；若为数组或集合，则最多包含 100条，且 key 约束条件与属性名称一致，value 取值长度 1 - 255 字符
```

**注册通用属性**

某一个体，在固定范围内，持续拥有的属性，每次数据上传都会携带。接口如下:

```javascript
AnalysysAgent.registerSuperProperty(superPropertyName , superPropertyValue );

AnalysysAgent.registerSuperProperties(superProperty);
```

* superPropertyName：属性名称，约束见[属性名称](./#1)
* superPropertyValue：属性值，约束见[属性值](./#2)
* superProperty：属性列表，约束见[属性名称](./#1)，[属性值](./#2)

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

**删除通用属性**

根据属性名称，删除已设置过的通用属性。接口如下：

```javascript
//删除单个通用属性
AnalysysAgent.unRegisterSuperProperty(superPropertyName);
//清除所有通用属性
AnalysysAgent.clearSuperProperties();
```

* superPropertyName：属性名称，约束见[属性名称](./#1)

示例：

```javascript
// 删除已经设置的用户年龄属性
AnalysysAgent.unRegisterSuperProperty("age");

......

// 清除所有已经设置的通用属性
AnalysysAgent.clearSuperProperties();
```

**获取通用属性**

查询获取通用属性。接口如下：

```javascript
// 获取单个通用属性
AnalysysAgent.getSuperProperty(superPropertyName);
// 获取所有的通用属性
AnalysysAgent.getSuperProperties();
```

* superPropertyName：属性名称，约束见[属性名称](./#1)

示例：

```javascript
// 查看已经设置的"member"的通用属性
AnalysysAgent.getSuperProperty("member");

......

// 查看所有已经设置的通用属性
AnalysysAgent.getSuperProperties();
```

### 获取预置属性

获取预置属性，接口如下：

```javascript
AnalysysAgent.getPresetProperties();
```

示例：获取预置属性

```javascript
// 获取预置属性
var presetProperties =  AnalysysAgent.getPresetProperties();
console.log('预置属性:',presetProperties)
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

