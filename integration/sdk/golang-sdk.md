---
description: Golang SDK 使用说明
---

# Golang SDK

Golang SDK 主要用于服务端 Golang 应用.

{% hint style="info" %}
SDK Releases包下载：  
Github地址\(推荐\)：[https://github.com/analysys/ans-lua-sdk/releases](https://github.com/analysys/ans-go-sdk/releases)   
Gitee地址：[https://gitee.com/Analysys/ans-lua-sdk/releases/](https://gitee.com/Analysys/ans-go-sdk/releases/)  
Releases中含有更新说明请您阅读，接口使用请参考本文档。
{% endhint %}

## 1.集成准备

### 1.1下载 SDK

```text
//获取SDK文件从GitHub上获取
go get github.com/analysys/ans-go-sdk


//或更新本地已经存在的
go get -u github.com/analysys/ans-go-sdk 
```

### 1.2 **代码引入**

```text
import ans "github.com/analysys/ans-go-sdk"
```

## 2. SDK 初始化

### 2.1 获取配置信息

需获取的配置信息为：

* 数据接收地址：`http://host:port`

### 2.2 初始化接口

在程序/模块启动时初始化 Lua SDK 实例。如下：

```go
ans.InitAnalysysAgent(Collector lib.Collector, appid string, debugMode int)
```

* Collector :上报类，实时上传、批量上传 或者 同步落数据到本地、批量落数据到本地
* appid : 方舟获取的项目唯一标示
* debugMode:debug 模式,int。有以下三种枚举值：
  * `0`：表示关闭 Debug 模式
  * `1`：表示打开 Debug 模式，但该模式下发送的数据仅用于调试，不计入平台数据统计
  * `2`：表示打开 Debug 模式，该模式下发送的数据可计入平台数据统计

{% hint style="info" %}
注意：发布版本时debug模式设置为`0`。
{% endhint %}

示例：

```text
AnalysysAgent := ans.InitAnalysysAgent(Collector, appid, debugMode)
```

#### 2.2.1 实时收集器

用户每触发一次上传，该收集器则立即上传数据至接收服务器：

```go
InitSyncCollecter(uploadURL string)
```

* uploadURL:服务上报地址

示例：

```text
// 初始化实时 syncConsumer
syncCollector := ans.InitSyncCollector(uploadURL)
AnalysysAgent := ans.InitAnalysysAgent(syncCollector, appid, debugMode)
```

#### 2.2.2 批量收集器

用户触发上传，该收集器将先缓存数据，直到数量达到用户设置的阈值，才会触发真正的上传:

```lua
InitBatchCollector(uploadURL string,postNumber int)
```

* uploadURL:服务上报地址
* postNumber:批量发送数量，满足条数发送

示例：

```text
//初始化批量收集器 
batchCollector := InitBatchCollector(uploadURL,postNumber)
AnalysysAgent := ans.InitAnalysysAgent(batchCollector, appid, debugMode)
```

#### 2.2.3 **实时落日志到本地**

用户每触发一次上传，该收集器则立即落数据至本地设置的存放地址:

```lua
InitSyncLogCollector(gerFolder string,gerRule string)
```

* gerFolder:文件存放的路径
* gerRule:文件的切割规则，"D" 按天分割, "H",按小时分割 ,传"" ,则默认按小时切割

示例

```text
// 初始化实时落日志到本地
syncLogCollector := InitSyncLogCollector(gerFolder,gerRule)
AnalysysAgent := ans.InitAnalysysAgent(syncLogCollector, appid, debugMode)
```

#### 2.2.4 **批量落日志到本地**

用户触发上传，该收集器将先缓存数据，直到数量达到用户设置的阈值，才会触发真正的落数据至本地设置的存放地址:

```lua
InitBatchLogCollector(gerFolder string,gerRule string,postNumber int)
```

* gerFolder:文件存放的路径
* gerRule:文件的切割规则，"D" 按天分割, "H",按小时分割 ,传"" ,则默认按小时切割
* postNumber:批量落日志的数量，满足条数落日志到本地

示例

```text
// 初始化批量落日志到本地
batchLogCollector := InitBatchLogCollector(gerFolder,gerRule,postNumber)
AnalysysAgent := ans.InitAnalysysAgent(batchLogCollector, appid, debugMode)
```

至此 SDK初始化完成，调用 SDK 提供的接口已可正常采集用户数据了。  
说明：程序结束前需要调用 `flush()` 接口，该接口可以把本地缓存的尚未发送至数据接收服务器或未写入本地的数据发送到数据接收服务器或写入本地文件。

## 3. 基础接口介绍

### 3.1 统计事件

事件跟踪，设置事件名称和事件详细信息。接口如下：

```lua
Track(distinctId string, isLogin bool , eventName string, properties map[string]interface{},platform string,upLoadTime int);
```

* distinctId：用户 ID,长度大于 0 且小于 255字符
* isLogin：用户ID 是否是登录 ID
* eventName：事件名称,以字母或 `$` 开头，可包含字母、数字、下划线和 `$`，字母不区分大小写，`$`开头为预置事件,不支持乱码和中文,最大长度 99字符
* properties: 事件属性,最多包含 100条,且 key 以字母或 `$` 开头，可包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件属性,最大长度 99字符,不支持乱码和中文,value 类型约束\(String/Int/Bool/slice\[\]string\)，若为字符串,最大长度255字符
* platfrom: 平台类型,建议内容范围：JS、WeChat、Android、iOS, 并且支持自定义
* upLoadTime: 用户自定义时间戳\(带毫秒的13位时间戳\),如要使用当前时间，传入ans.CurrentTime\(\)即可

示例：用户浏览商品

```lua
// 浏览商品
distinctId := "1234"
isLogin := false
eventName := "ViewProduct"
trackPropertie := map[string]interface{}{
    "$ip":"112.112.112.112",        //IP地址
}
bookList := []string{"Go语言编程"}
trackPropertie["productName"] = bookList  
trackPropertie["productType"] = "Go书籍"
trackPropertie["producePrice"] = 80;          
trackPropertie["shop"] = "xx网上书城" 
platform:= "Android" 
uploadTime:= ans.CurrentTime()  
AnalysysAgent.Track(distinctId,isLogin,eventName,trackPropertie,platform,uploadTime);
```

### 3.2 用户关联

用户 ID 关联接口。将 用户ID和匿名ID关联，计算时会认为是一个用户的行为。该接口是在匿名ID发生变化的时候调用，来告诉 SDK 匿名ID变化前后的 ID 对应关系。该场景一般应用在用户注册/登录的过程中。比如：一个匿名用户浏览商品，系统为其分配的distinctId = "1234567890987654321"，随后该匿名用户进行注册，系统为其分配了新的注册 ID，aliasId = "ABCDEF123456789"，此时就需要调用 alias 接口对两个 ID 进行关联。接口如下：

```lua
Alias(aliasId string, distinctId string,platform string, upLoadTime int);
```

* aliasId：用户注册 ID，长度大于 0，且小于 255字符
* distinctId：用户匿名ID，长度大于 0，且小于 255字符
* platfrom: 平台类型,建议内容范围：JS、WeChat、Android、iOS, 并且支持自定义
* upLoadTime: 用户自定义时间戳\(带毫秒的13位时间戳\),如要使用当前时间，传入ans.CurrentTime\(\)即可

示例：匿名用户浏览商品到注册会员

```lua
// 匿名ID
distinctId := "1234567890987654321"
//用户注册登录
registerId := "ABCDEF123456789"
platform := "Android"
uploadTime := 1585122647079
AnalysysAgent.Alias(registerId,distinctId,platform,uploadTime);
```

### 3.3 用户属性设置

SDK提供以下接口供用户设置用户的属性，比如用户的年龄/性别等信息。

> 用户属性是一个标准的K-V结构，K和V均有相应的约束条件，如不符合则丢弃该次操作。 参数约束:

* **属性名称**

  以字母或`$`开头，可包含字母、数字、下划线和`$`，字母不区分大小写，`$`开头为预置事件属性,最大长度99字符,不支持乱码和中文

* **属性值**

  支持部分类型：string/int/bool/\[\]string切片; 若为字符串,则最大长度255字符; 若为数组或集合,则最多包含100条,且key约束条件与属性名称一致,value最大长度255字符

设置单个或多个属性，如用户所在城市，用户昵称，用户头像信息等。如果之前存在，则覆盖，否则，新创建。接口如下：

```lua
ProfileSet(distinctId string, isLogin bool, properties map[string]interface{},platform string, upLoadTime int);
```

* distinctId: 用户ID,长度大于0且小于255字符
* isLogin: 用户ID是否是登录 ID
* properties: 事件属性
* platfrom: 平台类型,建议内容范围：JS、WeChat、Android、iOS, 并且支持自定义
* upLoadTime: 用户自定义时间戳\(带毫秒的13位时间戳\),如要使用当前时间，传入ans.CurrentTime\(\)即可

示例：用户注册后设置用户的注册信息属性

```lua
registerId := "ABCDEF123456789"
isLogin := true
//用户信息
profiles := map[string]interface{}{
    "$city":"北京",         //城市
    "$province":"北京",     //省份
    "nickName":"昵称123",   //昵称
    "userLevel":0,         //用户级别
    "userPoint":0,          //用户积分
}
interestList := []string{"户外活动","足球赛事","游戏"}
profiles["interest"] = interestList//用户兴趣爱好
platform := "Android"
currentTime := ans.CurrentTime() //当前时间
AnalysysAgent.ProfileSet(registerId, isLogin, profiles,platform,currentTime);
```

## 4. 更多接口

### 4.1 用户属性

#### 4.1.1 设置用户固有属性

只在首次设置时有效的属性。如：用户的注册时间。如果被设置的用户属性已存在，则这条记录会被忽略而不会覆盖已有数据，如果属性不存在则会自动创建。接口如下：

```java
ProfileSetOnce(distinctId, isLogin, properties,platform,upLoadTime);
```

* distinctId: 用户ID,长度大于0且小于255字符
* isLogin: 用户ID是否是登录 ID
* properties: 事件属性
* platform: 平台类型,建议内容范围：JS、WeChat、Android、iOS, 并且支持自定义
* xwhen: 用户自定义时间戳\(带毫秒的13位时间戳\),如要使用当前时间，传入ans.CurrentTime\(\)即可

示例：要统计用户注册时间

```lua
registerId := "ABCDEF123456789"
isLogin := true
platform := "Android"
profileAge := map[string]interface{}{
   "registerTime":20180101101010,
}
uploadTime := 1569859200000
AnalysysAgent.ProfileSetOnce(registerId,isLogin,profileAge,platform,uploadTime);
```

#### 4.1.2 设置用户属性相对变化值

设置用户属性的单个相对变化值\(相对增加,减少\)，只能对数值型属性进行操作，如果这个Profile之前不存在,则初始值为0。接口如下：

```lua
ProfileIncrement(distinctId string, isLogin string, properties map[string]int,platform string,upLoadTime int);
```

* distinctId: 用户ID,长度大于0且小于255字符
* isLogin: 用户ID是否是登录 ID
* properties: 事件属性
* platform: 平台类型,建议内容范围：JS、WeChat、Android、iOS, 并且支持自定义
* xwhen: 用户自定义时间戳\(带毫秒的13位时间戳\),如要使用当前时间，传入ans.CurrentTime\(\)即可

示例：用户注册初始积分为0，在用户购买商品后，用户的积分增加20，则调用该接口，用户的积分变为0+20=20了：

```lua
registerId := "ABCDEF123456789"
isLogin := true
platform := "Android"
profile := map[string]int{
    "userPoint":20,
};
uploadTime := 1569859200000;
AnalysysAgent.ProfileIncrement(registerId, isLogin, profile,platform,uploadTime);
```

#### 4.1.3 增加列表类型的属性

为列表类型的属性增加一个或多个元素，如：用户新增兴趣爱好，接口如下：

```lua
ProfileAppend(distinctId string, isLogin string, properties map[string]interface{},platform string, upLoadTime int);
```

* distinctId: 用户ID,长度大于0且小于255字符
* isLogin: 用户ID是否是登录 ID
* properties: 事件属性
* platform: 平台类型,建议内容范围：JS、WeChat、Android、iOS, 并且支持自定义
* xwhen: 用户自定义时间戳\(带毫秒的13位时间戳\),如要使用当前时间，传入ans.CurrentTime\(\)即可

示例：用户初始填写的兴趣爱好为\["户外活动"，"足球赛事"，"游戏"\]，调用该接口追加\["学习"，"健身"\]，则用户的爱好变为\["户外活动"，"足球赛事"，"游戏"，"学习"，"健身"\]

```lua
registerId := "ABCDEF123456789"
isLogin := true
platform := "Android"
interestList := []{"户外活动","足球赛事","游戏"}
profile := map[string]interface{}{
    "interest":interestList,     //用户兴趣爱好
};
uploadTime := 1569859200000
AnalysysAgent.ProfileAppend(registerId, isLogin, profile,platform,uploadTime);
```

#### 4.1.4 删除设置的属性值

删除设置的属性值。接口如下：

```java
ProfileUnSet(distinctId string, isLogin bool, property string,platform string, upLoadTime int);
ProfileDelete(distinctId string, isLogin bool , platform string,upLoadTime int);
```

* distinctId: 用户ID,长度大于0且小于255字符
* isLogin: 用户ID是否是登录 ID
* propertie: 事件属性
* platform: 平台类型,建议内容范围：JS、WeChat、Android、iOS, 并且支持自定义
* xwhen: 用户自定义时间戳\(带毫秒的13位时间戳\),如要使用当前时间，传入ans.CurrentTime\(\)即可

示例：

```lua
--示例1： 要删除已经设置的用户昵称这一用户属性
registerId := "ABCDEF123456789"
isLogin := true
platform := "Android"
property := "nickName"
uploadTime := 1569859200000;
AnalysysAgent.ProfileUnSet(registerId,isLogin,property,platform,uploadTime);

--示例2：要删除已经设置的所有用户属性
registerId := "ABCDEF123456789"
isLogin := true
platform := "Android"
uploadTime := 1569859200000
// 清除所有属性
AnalysysAgent.ProfileDelete(registerId,isLogin,platform,uploadTime); 
```

### 4.2 通用属性

如果某个事件的属性，在所有事件中都会出现，则这个属性可以做为通用属性，通过 registerSuperProperties\(\) 将该属性设置为事件通用属性，则设置后每次发送事件的时候都会带有该属性。比如用户浏览/购买商品过程中的用户等级就可以作为通用属性。

> 通用属性是一个标准的 K-V 结构，K 和 V 均有相应的约束条件，如不符合则丢弃该次操作。

约束条件如下:

* **属性名称**

  以字母或 `$` 开头，可包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件属性,最大长度 99字符,不支持乱码和中文

* **属性值**
  * 支持部分类型：string/int/bool/\[\]string切片;
  * 若为字符串,则最大长度 255字符;
  * 若为数组或集合,则最多包含 100条,且 key 约束条件与属性名称一致,value 最大长度 255字符

#### 4.2.1 注册通用属性

以用户浏览/购买商品的过程中发生的事件为例，用户的级别/积分就可以作为通用的属性，通过把用户级别/积分注册为通用属性，就可以避免在每次收集事件属性的时候都要手工设置该属性值。接口如下：

```lua
RegisterSuperProperty(key string,value interface{})
RegisterSuperProperties(params map[string]interface{});
```

* key 单个属性的key
* value 单个属性的value
* params：设置多个属性

示例：

```lua
// 设置单个通用属性
key := "super" //用户级别
value := 0 //用户积分
AnalysysAgent.RegisterSuperProperty(key,value)
// 设置多个通用属性
superPropertie :=map[string]interface{} {
    "userLevel":0,
    "userPoint":0,
};
AnalysysAgent.RegisterSuperProperties(superPropertie);
```

#### 4.2.2 删除通用属性

如果要删除某个通用属性或者删除全部的通用属性，可以分别调用 unregisterSuperProperty 或 clearSuperProperties 接口。具体接口定义如下：

```lua
UnRegisterSuperProperty(key string);
ClearSuperProperties();
```

* key：属性名称

示例

```lua
--示例1：删除设置的用户积分属性
// 删除单个通用属性
AnalysysAgent.UnRegisterSuperProperty("userPoint");

--示例2：清除所有已经设置的通用属性
// 清除所有通用属性
AnalysysAgent.ClearSuperProperties();

```

#### 4.2.3 获取通用属性

由属性名称查询获取单条通用属性，或者获取全部的通用属性。接口如下：

```java
GetSuperProperty(key string);
GetSuperProperties();
```

* key：属性名称

示例：

```lua
-- 示例1：查看已经设置的 userLevel 通用属性
// 获取单个通用属性
AnalysysAgent.GetSuperProperty("userLevel");

--示例2：查看所有已经设置的通用属性
// 获取所有通用属性
AnalysysAgent.GetSuperProperties();

```

### 4.3  刷新缓存

立即发送所有收集的信息到服务器。

```java
AnalysysAgent.Flush();
```

## 5. SDK 使用样例

```lua
import (
    "ans"
)
appId := "1234"
upLoadURL := "https://arksdk.analysys.cn:4089"
postNumber := 2
debugMode := 2
timestr := strconv.FormatInt(time.Now().UnixNano()/1e6, 10)
currentTime, _ := strconv.Atoi(timestr)
batchCollector := ans.InitBatchCollector(upLoadURL, postNumber)
AnalysysAgent := ans.InitAnalysysAgent(batchCollector, appId, debugMode)
// 初始化完成
distinctId := "1234567890987654321"
platform := "Android"
//浏览商品
trackPropertie := map[string]interface{}{
    "$ip": "112.112.112.112", //IP地址
}
bookList := []string{"Go语言编程"}
trackPropertie["productName"] = bookList //商品列表
trackPropertie["productType"] = "Go书籍"   //商品类别
trackPropertie["producePrice"] = 80      //商品价格
trackPropertie["shop"] = "xx网上书城"        //店铺名称
AnalysysAgent.Track(distinctId, true, "ViewProduct", trackPropertie, platform, currentTime)
//用户注册登录
registerId := "ABCDEF123456789"
AnalysysAgent.Alias(registerId, distinctId, platform, currentTime) //设置公共属性
superPropertie := map[string]interface{}{
    "sex": "male", //性别
    "age": 23,     //年龄
}
AnalysysAgent.RegisterSuperProperties(superPropertie) //用户信息
profiles := map[string]interface{}{
    "$city":     "北京",    //城市
    "$province": "北京",    //省份
    "nickName":  "昵称123", //昵称
    "userLevel": 0,       //用户级别
    "userPoint": 0,       //用户积分
}
interestList := []string{"户外活动", "足球赛事", "游戏"}
profiles["interest"] = interestList                                         //用户兴趣爱好
AnalysysAgent.ProfileSet(registerId, true, profiles, platform, currentTime) //用户注册时间
profile_age := map[string]interface{}{
    "registerTime": "20180101101010",
}
AnalysysAgent.ProfileSetOnce(registerId, true, profile_age, platform, currentTime)
//重新设置公共属性
AnalysysAgent.ClearSuperProperties()

superPropertie = map[string]interface{}{
    "userLevel": 0, //用户级别
    "userPoint": 0, //用户积分
}
AnalysysAgent.RegisterSuperProperties(superPropertie)
//再次浏览商品
trackPropertie["$ip"] = "112.112.112.112" //IP地址
bookList = []string{"Go语言编程"}
trackPropertie["productName"] = bookList //商品列表
trackPropertie["productType"] = "Go书籍"   //商品类别
trackPropertie["producePrice"] = 80      //商品价格
trackPropertie["shop"] = "xx网上书城"        //店铺名称
AnalysysAgent.Track(registerId, true, "ViewProduct", trackPropertie, platform, currentTime)
//订单信息
trackPropertie["orderId"] = "ORDER_12345"
trackPropertie["price"] = 80
AnalysysAgent.Track(registerId, true, "Order", trackPropertie, platform, currentTime)
//支付信息
trackPropertie["orderId"] = "ORDER_12345"
trackPropertie["productName"] = "Go语言编程"
trackPropertie["productType"] = "Go书籍"
trackPropertie["producePrice"] = 8
trackPropertie["shop"] = "xx网上书城"
trackPropertie["productNumber"] = 1
trackPropertie["price"] = 80
trackPropertie["paymentMethod"] = "AliPay"
AnalysysAgent.Track(registerId, true, "Payment", trackPropertie, platform, currentTime)
AnalysysAgent.Flush()
```

