---
description: 'C# SDK 主要用于服务端或客户端 C# 应用。'
---

# C\# SDK

{% hint style="info" %}
SDK Releases包下载：  
Github地址\(推荐\)：[https://github.com/analysys/ans-DotNet-sdk/releases](https://github.com/analysys/ans-DotNet-sdk/releases)  
Gitee地址：[https://gitee.com/Analysys/ans-DotNet-sdk/releases](https://gitee.com/Analysys/ans-DotNet-sdk/releases)  
Releases中含有更新说明请您阅读，接口使用请参考本文档。
{% endhint %}

## 1. 集成 SDK

以 Visual Studio 为例：将需要的动态链接库Analysys.dll拷贝到本地工程目录下；在 vs 中右键工程引用-&gt;添加引用-&gt;程序集-&gt;浏览，选择Analysys.dll点击 添加，即添加引用成功。

{% hint style="info" %}
`说明`：该版本动态链接库基于 .netframework4.0 进行编译的，无需其他的第三方库，如果需要其他framework的版本，请联系我们。
{% endhint %}

## 2. SDK 初始化

### 2.1 获取配置信息

需获取的配置信息为：

* 数据接收地址：`http://host:port`

### 2.2 初始化接口

在程序启动时（如 static void Main\(string\[\] args\) 方法中），调用构造函数 new AnalysysDotNetSdk\(Collecter\) 初始化 DotNet SDK 实例。如下：

```java
private const string APP_KEY = "APPKEY";
private const string ANALYSYS_SERVICE_URL = @"http://host:port";
AnalysysDotNetSdk analysys = new AnalysysDotNetSdk(new SyncCollecter(ANALYSYS_SERVICE_URL), APP_KEY);;
```

* APPKEY：网站获取的 Key
* SyncCollecter：为实时事件收集器

事件收集器还提供批量收集器和本地落文件收集器，用户可以按照自己需求指定。

#### 2.2.1 实时收集器

用户每触发一次上传，该收集器则立即上传数据至接收服务器：

```java
public SyncCollecter(String serverUrl);
```

* serverUrl：数据接收地址

#### 2.2.2 批量收集器

用户触发上传，该收集器将先缓存数据，直到数量达到用户设置的阈值或者用户设置的等待时间，才会触发真正的上传：

```java
public BatchCollecter(String serverUrl);
public BatchCollecter(String serverUrl, int batchNum);
public BatchCollecter(String serverUrl, int batchNum, long batchSec);
```

* serverUrl：数据接收地址
* batchNum：批量发送数量，默认值：20条
* batchSec：批量发送等待时间\(秒\)，默认值：10秒

#### 2.2.3 落地文件收集器

该收集器可以把用户触发的事件经过封装处理成标准的JSON写入本地文件中。支持多线程/多进程同时写入同一文件，默认为同步一条条写入文件，可以指定是否异步批量写入以及批量写入的条数和时间间隔。

```java
public LogCollecter(String logFolder);

public LogCollecter(String logFolder, boolean async);

public LogCollecter(String logFolder, GeneralRule rule);

public LogCollecter(String logFolder, GeneralRule rule, boolean async);

public LogCollecter(String logFolder, GeneralRule rule, boolean async, int batchNum, long batchSec);

```

* logFolder：数据保存的目录
* async： 是否异步批量保存\(默认为false，如果设置为true则默认20条或者10秒进行落地操作\)
* rule: 文件存储规则。当传值为"GeneralRule.DAY"时，文件按日写入数据（即每日生成一个文件），当传值为"GeneralRule.HOUR"时，文件按小时写入数据（即每小时生成一个文件）。默认规则为按小时
* batchNum： 批量保存数量，默认值：20条
* batchSec： 批量保存等待时间\(秒\)，默认值：10秒

至此 SDK初始化完成，调用 SDK 提供的接口已可正常采集用户数据了

{% hint style="info" %}
> 1. 程序结束前建议调用`Flush();`接口，确保不会丢失内存缓存的数据。
> 2. 在多线程环境中时，推荐使用单例模式初始化 AnalysysDotNetSdk，开发者可以在不同的线程间重复使用 AnalysysDotNetSdk 实例用于记录数据。
{% endhint %}

## 3. 基础接口介绍

### 3.1 Debug 模式

Debug 主要用于开发者测试，接口如下：

```java
public void setDebugMode(DEBUG debug);
```

* debug：debug 模式,枚举类型,默认关闭状态。有以下三种枚举值：

  * `DEBUG.CLOSE`：表示关闭 Debug 模式
  * `DEBUG.OPENNOSAVE`：表示打开 Debug 模式，但该模式下发送的数据仅用于调试，不计入平台数据统计
  * `DEBUG.OPENANDSAVE`：表示打开 Debug 模式，该模式下发送的数据可计入平台数据统计

  注意：发布版本时debug模式设置为\`DEBUG.CLOSE\`。

### 3.2 统计事件

事件跟踪，设置事件名称和事件详细信息。接口如下：

```java
public void Track(string distinctId, bool isLogin, string eventName, Dictionary<string, object> properties, string platform);
public void Track(string distinctId, bool isLogin, string eventName, Dictionary<string, object> properties, string platform, string xwhen);
```

* distinctId：用户 ID,长度大于 0 且小于 255字符
* isLogin：用户 ID 是否是登录 ID
* eventName：事件ID,以字母或 `$` 开头，可包含字母、数字、下划线和 `$`，字母不区分大小写，`$`开头为预置事件,不支持乱码和中文,最大长度 99字符
* properties: 事件属性,最多包含 100条,且 key 以字母或 `$` 开头，可包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件属性,最大长度99字符,不支持乱码和中文,value 类型约束\(String/Number/boolean/list/数组\)，若为字符串,最大长度255字符
* platform：平台类型,内容范围：JS、WeChat、Android、iOS
* xwhen: 用户自定义时间戳\(带毫秒的13位时间戳\)

示例：用户浏览商品

```java
//浏览商品
string distinctId = "1234567890987654321";
string platForm = "DotNet"; //DotNet平台
analysys.SetDebugMode(DEBUG.CLOSE); //设置debug模式
//浏览商品
Dictionary<string, object> trackPropertie = new Dictionary<string, object>();
trackPropertie["$ip"] = "122.122.122.122"; //IP地址
List<string> bookList = new List<string>();
bookList.Add("Thinking in Java");
trackPropertie["productName"] = bookList; //商品列表
trackPropertie["productType"] = "Java书籍"; //商品类别
trackPropertie["producePrice"] = 80; //商品价格
trackPropertie["shop"] = "xx网上书城"; //店铺名称
analysys.Track(distinctId, false, "ViewProduct", trackPropertie, platForm);
```

### 3.3 用户关联

用户 ID 关联接口。将 aliasId 和 distinctId 关联，计算时会认为是一个用户的行为。该接口是在 distinctId 发生变化的时候调用，来告诉 SDK distinctId 变化前后的 ID 对应关系。该场景一般应用在用户注册/登录的过程中。比如：一个匿名用户浏览商品，系统为其分配的distinctId = "1234567890987654321"，随后该匿名用户进行注册，系统为其分配了新的注册 ID，aliasId = "ABCDEF123456789"，此时就需要调用 alias 接口对两个 ID 进行关联。接口如下：

```java
public void Alias(string AliasId, string distinctId, string platform);
public void Alias(string AliasId, string distinctId, string platform, string xwhen);
```

* aliasId：用户注册 ID，长度大于 0，且小于 255字符
* distinctId：用户匿名ID，长度大于 0，且小于 255字符
* platform：平台类型,内容范围：JS、WeChat、Android、iOS
* xwhen: 用户自定义时间戳\(带毫秒的13位时间戳\)

示例：匿名用户浏览商品到注册会员

```java
// 匿名ID
String distinctId = "1234567890987654321";
String platform = "DotNet";
...
...
...
//用户注册登录
String registerId = "ABCDEF123456789";
analysys.alias(registerId, distinctId, platform);

// 或者也可以使用自定义的时间戳
String myxWhen = "1569859200000";
analysys.alias(registerId, distinctId, platform, myxWhen);
```

### 3.4 用户属性设置

SDK提供以下接口供用户设置用户的属性，比如用户的年龄/性别等信息。

> 用户属性是一个标准的K-V结构，K和V均有相应的约束条件，如不符合则丢弃该次操作。 参数约束:

* **属性名称**

  以字母或`$`开头，可包含字母、数字、下划线和`$`，字母不区分大小写，`$`开头为预置事件属性,最大长度99字符,不支持乱码和中文

* **属性值**

  支持部分类型：string/number/boolean/集合/数组; 若为字符串,则最大长度255字符; 若为数组或集合,则最多包含100条,且key约束条件与属性名称一致,value最大长度255字符

设置单个或多个属性，如用户所在城市，用户昵称，用户头像信息等。如果之前存在，则覆盖，否则，新创建。接口如下：

```java
public void ProfileSet(string distinctId, bool isLogin, Dictionary<string, object> properties, string platform);
public void ProfileSet(string distinctId, bool isLogin, Dictionary<string, object> properties, string platform, string xwhen);
```

* distinctId: 用户ID,长度大于0且小于255字符
* isLogin: 用户ID是否是登录 ID
* properties: 事件属性
* platform: 平台类型,内容范围：JS、WeChat、Android、iOS
* xwhen: 用户自定义时间戳\(带毫秒的13位时间戳\)

示例：用户注册后设置用户的注册信息属性

```aspnet
Dictionary<string, object> profiles = new Dictionary<string, object>();
profiles["$city"] = "北京"; //城市
profiles["$province"] = "北京"; //省份
profiles["nickName"] = "昵称123"; //昵称
profiles["userLevel"] = 0; //用户级别
profiles["userPoint"] = 0; //用户积分
List<string> interestList = new List<string>();
interestList.Add("户外活动");
interestList.Add("足球赛事");
interestList.Add("游戏");
profiles["interest"] = interestList; //用户兴趣爱好
analysys.ProfileSet(registerId, true, profiles, platForm);

```

## 4. 更多接口

### 4.1 用户属性

#### 4.1.1 设置用户固有属性

只在首次设置时有效的属性。如：用户的注册时间。如果被设置的用户属性已存在，则这条记录会被忽略而不会覆盖已有数据，如果属性不存在则会自动创建。接口如下：

```csharp
public void ProfileSetOnce(string distinctId, bool isLogin, Dictionary<string, object> properties, string platform);
public void ProfileSetOnce(string distinctId, bool isLogin, Dictionary<string, object> properties, string platform, string xwhen);
```

* distinctId: 用户ID,长度大于0且小于255字符
* isLogin: 用户ID是否是登录 ID
* properties: 事件属性
* platform: 平台类型,内容范围：JS、WeChat、Android、iOS
* xwhen: 用户自定义时间戳\(带毫秒的13位时间戳\)

示例：要统计用户注册时间

```java
string registerId = "ABCDEF123456789";
bool isLogin = true;
string platform = "DotNet";
//用户注册时间
Dictionary<string, object> profile_age = new Dictionary<string, object>();
profile_age["registerTime"] = "20180101101010";
analysys.ProfileSetOnce(registerId, true, profile_age, platForm);

// 或者也可以使用自定义的时间戳
String myxWhen = "1569859200000";
analysys.profileSetOnce(registerId, isLogin, profile_age, platform, myxWhen);
```

#### 4.1.2 设置用户属性相对变化值

设置用户属性的单个相对变化值\(相对增加,减少\)，只能对数值型属性进行操作，如果这个Profile之前不存在,则初始值为0。接口如下：

```csharp
public void profileIncrement(string distinctId, bool isLogin, Dictionary<string, object> properties, string platform);
public void profileIncrement(string distinctId, bool isLogin, Dictionary<string, object> properties, string platform, string xwhen);
```

* distinctId: 用户ID,长度大于0且小于255字符
* isLogin: 用户ID是否是登录 ID
* properties: 事件属性
* platform: 平台类型,内容范围：JS、WeChat、Android、iOS
* xwhen: 用户自定义时间戳\(带毫秒的13位时间戳\)

示例：用户注册初始积分为0，在用户购买商品后，用户的积分增加20，则调用该接口，用户的积分变为0+20=20了：

```java
String registerId = "ABCDEF123456789";
boolean isLogin = true;
String platform = "DotNet";
Map<String,Object> profile = new HashMap<>();
profile.put("userPoint",20);
analysys.profileIncrement(registerId, isLogin, profile, platform);

// 或者也可以使用自定义的时间戳
String myxWhen = "1569859200000";
analysys.profileIncrement(registerId, isLogin, profile, platform, myxWhen);
```

#### 4.1.3 增加列表类型的属性

为列表类型的属性增加一个或多个元素，如：用户新增兴趣爱好，接口如下：

```csharp
public void profileAppend(string distinctId, bool isLogin, Dictionary<string, object> properties, string platform);
public void profileAppend(string distinctId, bool isLogin, Dictionary<string, object> properties, string platform, string xwhen);
```

* distinctId: 用户ID,长度大于0且小于255字符
* isLogin: 用户ID是否是登录 ID
* properties: 事件属性
* platform: 平台类型,内容范围：JS、WeChat、Android、iOS
* xwhen: 用户自定义时间戳\(带毫秒的13位时间戳\)

示例：用户初始填写的兴趣爱好为\["户外活动"，"足球赛事"，"游戏"\]，调用该接口追加\["学习"，"健身"\]，则用户的爱好变为\["户外活动"，"足球赛事"，"游戏"，"学习"，"健身"\]

```csharp
String registerId = "ABCDEF123456789";
boolean isLogin = true;
    String platform = "DotNet";
Map<String,Object> profile = new HashMap<>();
List<String> interestList = new ArrayList<String>();
interestList.add("户外活动");
interestList.add("足球赛事");
interestList.add("游戏");
profile.put("interest", interestList);//用户兴趣爱好
analysys.profileAppend(registerId, isLogin, profile, platform);

// 或者也可以使用自定义的时间戳
String myxWhen = "1569859200000";
analysys.profileAppend(registerId, isLogin, profile, platform, myxWhen);;
```

#### 4.1.4 删除设置的属性值

删除设置的属性值。接口如下：

```csharp
public void ProfileUnSet(string distinctId, bool isLogin, string property, string platform);
public void ProfileUnSet(string distinctId, bool isLogin, string property, string platform, string xwhen);
public void ProfileDelete(string distinctId,bool isLogin, string platform);
public void ProfileDelete(string distinctId,bool isLogin, string platform, string xwhen)
```

* distinctId: 用户ID,长度大于0且小于255字符
* isLogin: 用户ID是否是登录 ID
* propertie: 事件属性
* platform: 平台类型,内容范围：JS、WeChat、Android、iOS
* xwhen: 用户自定义时间戳\(带毫秒的13位时间戳\)

示例1： 要删除已经设置的用户昵称这一用户属性

```java
String registerId = "ABCDEF123456789";
boolean isLogin = true;
String platform = "DotNet";
// 删除单个用户属性
analysys.profileUnSet(registerId, isLogin, "nickName", platform);

// 或者也可以使用自定义的时间戳
String myxWhen = "1569859200000";
analysys.profileUnSet(registerId, isLogin, "nickName", platform, myxWhen);
```

示例2：要删除已经设置的所有用户属性

```java
String registerId = "ABCDEF123456789";
boolean isLogin = true;
String platform = "DotNet";
// 清除所有属性
analysys.profileDelete(registerId, isLogin, platform);

// 或者也可以使用自定义的时间戳
String myxWhen = "1569859200000";
analysys.profileDelete(registerId, isLogin, platform, myxWhen);;
```

### 4.2 通用属性

如果某个事件的属性，在所有事件中都会出现，则这个属性可以做为通用属性，通过 registerSuperProperties\(\) 将该属性设置为事件通用属性，则设置后每次发送事件的时候都会带有该属性。比如用户浏览/购买商品过程中的用户等级就可以作为通用属性。

> 通用属性是一个标准的 K-V 结构，K 和 V 均有相应的约束条件，如不符合则丢弃该次操作。

约束条件如下:

* **属性名称**

  以字母或 `$` 开头，可包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件属性,最大长度 99字符,不支持乱码和中文

* **属性值**
  * 支持部分类型：string/number/boolean/集合/数组;
  * 若为字符串,则最大长度 255字符;
  * 若为数组或集合,则最多包含 100条,且 key 约束条件与属性名称一致,value 最大长度 255字符

#### 4.2.1 注册通用属性

以用户浏览/购买商品的过程中发生的事件为例，用户的级别/积分就可以作为通用的属性，通过把用户级别/积分注册为通用属性，就可以避免在每次收集事件属性的时候都要手工设置该属性值。接口如下：

```csharp
public void registerSuperProperties(Map<String, Object> params);
```

* params：设置多个属性

示例：

```csharp
// 设置多个通用属性
Dictionary<string, object> superPropertie = new Dictionary<string, object>();
superPropertie.put("userLevel", 0); //用户级别
superPropertie.put("userPoint", 0); //用户积分
analysys.registerSuperProperties(superPropertie);
```

#### 4.2.2 删除通用属性

如果要删除某个通用属性或者删除全部的通用属性，可以分别调用 unregisterSuperProperty 或 clearSuperProperties 接口。具体接口定义如下：

```csharp
public void unRegisterSuperProperty(String key);
public void clearSuperProperties();
```

* key：属性名称

示例1：删除设置的用户积分属性

```csharp
// 删除单个通用属性
analysys.unRegisterSuperProperty("userPoint");

示例2：清除所有已经设置的通用属性
// 清除所有通用属性
analysys.clearSuperProperties();
```

#### 4.2.3 获取通用属性

由属性名称查询获取单条通用属性，或者获取全部的通用属性。接口如下：

```csharp
public Object getSuperProperty(String key);
public Map<String, Object> getSuperProperties();
```

* key：属性名称

示例1：查看已经设置的 userLevel 通用属性

```csharp
// 获取单个通用属性
analysys.getSuperProperty("userLevel")

示例2：查看所有已经设置的通用属
// 获取所有通用属
analysys.getSuperProperties();
```

### 4.3  刷新缓存

立即发送所有收集的信息到服务器。

```java
analysys.flush();
```

## 5. SDK 使用样例

```java
//AnalysysDotNetSdk analysys = new AnalysysDotNetSdk(new SyncCollecter(ANALYSYS_SERVICE_URL), APP_KEY);
//批量
// AnalysysDotNetSdk analysys = new AnalysysDotNetSdk(new BatchCollecter(ANALYSYS_SERVICE_URL), APP_KEY);
//落文件
AnalysysDotNetSdk analysys = new AnalysysDotNetSdk(new LogCollecter(@"SAVE_DIR"), APP_KEY);
try {
    string distinctId = "1234567890987654321";
    string platForm = "DotNet"; //DotNet平台
    analysys.SetDebugMode(DEBUG.CLOSE); //设置debug模式
    //浏览商品
    Dictionary<string, object> trackPropertie = new Dictionary<string, object>();
    trackPropertie["$ip"] = "122.122.122.122"; //IP地址
    List<string> bookList = new List<string>();
    bookList.Add("Thinking in Java");
    trackPropertie["productName"] = bookList; //商品列表
    trackPropertie["productType"] = "Java书籍"; //商品类别
    trackPropertie["producePrice"] = 80; //商品价格
    trackPropertie["shop"] = "xx网上书城"; //店铺名称
    analysys.Track(distinctId, false, "ViewProduct", trackPropertie, platForm);

    //用户注册登录
    string registerId = "ABCDEF123456789";
    analysys.Alias(registerId, distinctId, platForm);

    //设置公共属性
    Dictionary<string, object> superPropertie = new Dictionary<string, object>();
    superPropertie["sex"] = "male"; //性别
    superPropertie["age"] = 23; //年龄
    analysys.RegisterSuperProperties(superPropertie);
    //用户信息
    Dictionary<string, object> profiles = new Dictionary<string, object>();
    profiles["$city"] = "北京"; //城市
    profiles["$province"] = "北京"; //省份
    profiles["nickName"] = "昵称123"; //昵称
    profiles["userLevel"] = 0; //用户级别
    profiles["userPoint"] = 0; //用户积分
    List<string> interestList = new List<string>();
    interestList.Add("户外活动");
    interestList.Add("足球赛事");
    interestList.Add("游戏");
    profiles["interest"] = interestList; //用户兴趣爱好
    analysys.ProfileSet(registerId, true, profiles, platForm);

    //用户注册时间
    Dictionary<string, object> profile_age = new Dictionary<string, object>();
    profile_age["registerTime"] = "20180101101010";
    analysys.ProfileSetOnce(registerId, true, profile_age, platForm);

    //重新设置公共属性
    analysys.ClearSuperProperties();
    superPropertie.Clear();
    superPropertie = new Dictionary<string, object>();
    superPropertie["userLevel"] = 0; //用户级别
    superPropertie["userPoint"] = 0; //用户积分
    analysys.RegisterSuperProperties(superPropertie);

    //再次浏览商品
    trackPropertie.Clear();
    trackPropertie["$ip"] = "122.122.122.122"; //IP地址
    List<string> abookList = new List<string>();
    abookList.Add("Thinking in Java");
    trackPropertie["productName"] = bookList; //商品列表
    trackPropertie["productType"] = "Java书籍"; //商品类别
    trackPropertie["producePrice"] = 80; //商品价格
    trackPropertie["shop"] = "xx网上书城"; //店铺名称
    analysys.Track(registerId, true, "ViewProduct", trackPropertie, platForm);

    //订单信息
    trackPropertie.Clear();
    trackPropertie["orderId"] = "ORDER_12345";
    trackPropertie["price"] = 80;
    analysys.Track(registerId, true, "Order", trackPropertie, platForm);

    //支付信息
    trackPropertie.Clear();
    trackPropertie["orderId"] = "ORDER_12345";
    trackPropertie["productName"] = "Thinking in Java";
    trackPropertie["productType"] = "Java书籍";
    trackPropertie["producePrice"] = 80;
    trackPropertie["shop"] = "xx网上书城";
    trackPropertie["productNumber"] = 1;
    trackPropertie["price"] = 80;
    trackPropertie["paymentMethod"] = "AliPay";
    analysys.Track(registerId, true, "Payment", trackPropertie, platForm);
}
catch (Exception e) {
    Console.WriteLine(e);
}
    analysys.Flush();
```

