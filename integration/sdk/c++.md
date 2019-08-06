# C++ SDK

C++ SDK 主要用于服务端 C++ （也称CPP、C plus plus）应用，如 C++ 应用的后台服务。集成前请先登录[Github下载源码](https://github.com/analysys/argo-sdk-cpp)

{% hint style="info" %}
C++ 版本需要standand 11及以上。SDK字符串编码请使用UTF-8编码,如果需要使用https协议请参考HttpSender.cpp 276行
{% endhint %}

## 1. 第三方开源库

### 1.1 CURL

使用CURL版本：curl-7.64.0，用于url网络发送。

### 1.2 ZLIB

使用ZLIB版本：zlib-1.2.11，网络发送数据加密。

## 2. SDK 初始化

### 2.1 获取配置信息

需获取的配置信息为：

数据接收地址：http://host:port

### 2.2 初始化接口

在程序启动时，调用构造函数 AnalysysCPlusPlusSdk \(\) 初始化 C++ SDK 实例。如下：

```cpp
std::string APP_ID = "1234";
std::string ANALYSYS_SERVICE_URL = "http://192.168.220.167:8089";
AnalysysCPlusPlusSdk analysys;
analysys.init(ANALYSYS_SERVICE_URL, APP_ID);
analysys.syncCollecter();
```

\* APP\_ID:网站获取的App key

\* ANALYSYS\_SERVICE\_URL: url

### 2.3 实时收集器接口

用户每触发一次上传，则立即上传数据至接收服务器：

```cpp
void syncCollecter();
```

### 2.4 批量收集器接口

用户触发上传，该收集器将先缓存数据，直到数量达到用户设置的阈值或者用户设置的等待时间，才会触发真正的上传：

```cpp
void batchCollecter(int nBatchNum=20, int nBatchSec = 10);
```

nBatchNum：批量发送数量，默认值：20条

nBatchSec：批量发送等待时间\(秒\)，默认值：10秒

### 2.5 Debug 模式

Debug 主要用于开发者测试，接口如下：

```cpp
void setDebugMode(int debug);
```

\* debug：debug 模式,枚举类型,默认关闭状态。有以下三种枚举值：

\* DEBUG.CLOSE：表示关闭 Debug 模式

\* DEBUG.OPENNOSAVE：表示打开 Debug 模式，但该模式下发送的数据仅用于调试，不计入平台数据统计

\* DEBUG.OPENANDSAVE：表示打开 Debug 模式，该模式下发送的数据可计入平台数据统计

{% hint style="info" %}
注意：发布版本时debug模式设置为\`DEBUG.CLOSE\`。
{% endhint %}

### 2.6 统计事件

事件跟踪，设置事件名称和事件详细信息。接口如下：

```cpp
void track(const std::string& strDistinctId, bool bIsLogin, const std::string& strEventName, const JObject& jAttributes, const std::string& strPlatform);
```

\* strDistinctId 用户 ID,长度大于 0 且小于 255字符

\* bIsLogin 用户 ID 是否是登录 ID

\* strEventName 事件ID

\* jAttributes 事件属性,最多包含 100条,且 key 以字母或 $ 开头，可包含字母、数字、下划线和 $，字母不区分大小写，$ 开头为预置事件属性, 最大长度 125字符,不支持乱码和中文,value 类型约束\(String/Number/boolean/list/数组\)，若为字符串,最大长度255字符

```cpp
//浏览商品
JObject trackPropertie;
trackPropertie["$ip"] = "112.112.112.112";
JObject bookList;
bookList[0] = "Thinking in Java";
trackPropertie["productName"] = bookList;
trackPropertie["productType"] = "Java book";
trackPropertie["producePrice"] = (long long)80;
trackPropertie["shop"] = "xx shop";
analysys.track(distinctId, false, "ViewProduct", trackPropertie, "JS");
```

### 2.7 用户关联

用户 ID 关联接口。将 strAliasId和 strDistinctId关联，计算时会认为是一个用户的行为。该接口是在 strDistinctId发生变化的时候调用，来告诉 SDK strDistinctId变化前后的 ID 对应关系。该场景一般应用在用户注册/登录的过程中。比如：一个匿名用户浏览商品，系统为其分配的strDistinctId = "1234567890987654321"，随后该匿名用户进行注册，系统为其分配了新的注册 ID，strAliasId = "ABCDEF123456789"，此时就需要调用 alias 接口对两个 ID 进行关联。接口如下：

```cpp
void alias(const std::string& strAliasId, const std::string& strDistinctId, const std::string& strPlatform);
```

\* strAliasId：用户注册 ID，长度大于 0，且小于 255字符

\* strDistinctId：用户匿名ID，长度大于 0，且小于 255字符

\* strPlatform：平台类型,内容范围：JS、WeChat、Android、iOS

示例：匿名用户浏览商品到注册会员

```cpp
string registerId = "ABCDEF123456789";
analysys.alias(registerId, distinctId, "JS");
```

### 2.8 用户属性设置

SDK提供以下接口供用户设置用户的属性，比如用户的年龄/性别等信息。

用户属性是一个标准的K-V结构，K和V均有相应的约束条件，如不符合则丢弃该次操作。 参数约束:

属性名称

以字母或$开头，可包含字母、数字、下划线和$，字母不区分大小写，$开头为预置事件属性,最大长度125字符,不支持乱码和中文

属性值

支持部分类型：string/number/boolean/集合/数组; 若为字符串,则最大长度255字符; 若为数组或集合,则最多包含100条,且key约束条件与属性名称一致,value最大长度255字符

设置单个或多个属性，如用户所在城市，用户昵称，用户头像信息等。如果之前存在，则覆盖，否则，新创建。接口如下：

```cpp
void profileSet(const std::string& strDistinctId, bool bIsLogin, const JObject& jAttributes, const std::string& strPlatform);
```

\* strDistinctId: 用户ID,长度大于0且小于255字符

\* bIsLogin: 用户ID是否是登录 ID

\* jAttributes: 事件属性

\* strPlatform: 平台类型,内容范围：JS、WeChat、Android、iOS

示例：用户注册后设置用户的注册信息属性

```cpp
JObject profiles;
profiles["$city"] = "beijin";
profiles["$province"] = "beijin";
profiles["nickName"] = "nickeName123";
profiles["userLevel"] = (long long)0;
profiles["userPoint"] = (long long)0;
JObject interestList;
interestList[0] = "sports";
interestList[1] = "football";
interestList[2] = "game";
profiles["interest"] = interestList;
analysys.profileSet(registerId, true, profiles, "JS");
```

### 2.9 设置用户固有属性

只在首次设置时有效的属性。如：用户的注册时间。如果被设置的用户属性已存在，则这条记录会被忽略而不会覆盖已有数据，如果属性不存在则会自动创建。接口如下：

```cpp
void profileSetOnce(const std::string& strDistinctId, bool bIsLogin, const JObject& jAttributes, const std::string& strPlatform);
```

\* strDistinctId: 用户ID,长度大于0且小于255字符

\* bIsLogin: 用户ID是否是登录 ID

\* jAttributes: 事件属性

\* strPlatform: 平台类型,内容范围：JS、WeChat、Android、iOS

示例：要统计用户注册时间

```cpp
//用户注册时间
JObject profile_age;
profile_age["registerTime"] = "20180101101010";
analysys.profileSetOnce(registerId, true, profile_age, "JS");
```

### 2.10 设置用户属性相对变化值

设置用户属性的单个相对变化值\(相对增加,减少\)，只能对数值型属性进行操作，如果这个Profile之前不存在,则初始值为0。接口如下：

```cpp
void profileIncrement(const std::string& strDistinctId, bool bIsLogin, const JObject& proAttributes, const std::string& strPlatform);
```

\* strDistinctId: 用户ID,长度大于0且小于255字符

\* bIsLogin: 用户ID是否是登录 ID

\* proAttributes: 事件属性

\* strPlatform: 平台类型,内容范围：JS、WeChat、Android、iOS

示例：用户注册初始积分为0，在用户购买商品后，用户的积分增加20，则调用该接口，用户的积分变为0+20=20了：

```cpp
std::string registerId = "ABCDEF123456789";
bool isLogin = true;
std::string platform = "Android";
JObject jProfile;
jProfile["userPoint"]=(long long )20;
analysys.profileIncrement(registerId, isLogin, jProfile, platform);
```

### 2.11 增加列表类型的属性

为列表类型的属性增加一个或多个元素，如：用户新增兴趣爱好，接口如下：

```cpp
void profileAppend(const std::string& strDistinctId, bool bIsLogin, const JObject& proAttributes, const std::string& strPlatform);
```

\* strDistinctId: 用户ID,长度大于0且小于255字符

\* bIsLogin: 用户ID是否是登录 ID

\* proAttributes: 事件属性

\* strPlatform: 平台类型,内容范围：JS、WeChat、Android、iOS

示例：用户初始填写的兴趣爱好为\["sports"，"football"，"game"\]，调用该接口追加\["study"，"BodyBuilding"\]，则用户的爱好变为\["sports"，"football"，"game"，"study"，"BodyBuilding"\]

```cpp
std::string registerId = "ABCDEF123456789";
bool isLogin = true;
std::string platform = "Android";
JObject profile;
JObject interestList;
interestList [0] = "study";
interestList [1] = "BodyBuilding";
profile["interest"]= interestList;
analysys.profileAppend(registerId, isLogin, profile, platform);
```

### 2.12 删除设置的属性值

删除设置的属性值。接口如下：

```cpp
void profileUnSet(const std::string& strDistinctId, bool bIsLogin, const std::string& strProperty, const std::string& strPlatform);
void profileDelete(const std::string& strDistinctId, bool bIsLogin, const std::string& strPlatform) ;
```

\* strDistinctId: 用户ID,长度大于0且小于255字符

\* bIsLogin: 用户ID是否是登录 ID

\* strProperty: 事件属性

\* strPlatform: 平台类型,内容范围：JS、WeChat、Android、iOS

```cpp
示例1：删除单个用户属性
std::string registerId = "ABCDEF123456789";
bool isLogin = true;
std::string platform = "Android";
// 删除当前用户单个属性值
analysys.profileUnSet(registerId, isLogin, "nickName", platform);
示例2：删除已经设置的所有用户属性
std::string registerId = "ABCDEF123456789";
bool isLogin = true;
std::string platform = "Android";
// 删除当前用户所有属性值
analysys.profileDelete(registerId, isLogin, platform);
```

###  2.13 通用属性

如果某个事件的属性，在所有事件中都会出现，则这个属性可以做为通用属性，通过将该属性设置为事件通用属性，则设置后每次发送事件的时候都会带有该属性。比如用户浏览/购买商品过程中的用户等级就可以作为通用属性。

通用属性是一个标准的 K-V 结构，K 和 V 均有相应的约束条件，如不符合则丢弃该次操作。

约束条件如下:

属性名称

以字母或 $ 开头，可包含字母、数字、下划线和 $，字母不区分大小写，$ 开头为预置事件属性,最大长度 125字符,不支持乱码和中文

属性值

支持部分类型：string/number/boolean/集合/数组;

若为字符串,则最大长度 255字符;

若为数组或集合,则最多包含 100条,且 key 约束条件与属性名称一致,value 最大长度 255字符

#### 2.13.1 注册通用属性

以用户浏览/购买商品的过程中发生的事件为例，用户的级别/积分就可以作为通用的属性，通过把用户级别/积分注册为通用属性，就可以避免在每次收集事件属性的时候都要手工设置该属性值。接口如下：

```cpp
void registerSuperAttributes(const JObject& attrAttributes);
```

\* attrAttributes：设置多个属性

示例：

```cpp
// 设置多个通用属性
JObject superPropertie;superPropertie ["userLevel"]= 0;
//用户级别
 superPropertie["userPoint"]= 0;
//用户积分
analysys.registerSuperProperties(superPropertie);
```

####  2.13.2 删除通用属性

如果要删除某个通用属性或者删除全部的通用属性，可以分别调用 unRegisterSuperProperty或 clearSuperAttributes接口。具体接口定义如下：

```cpp
void unRegisterSuperProperty(const std::string& strKey);
void clearSuperAttributes();
```

\* strKey：属性名称

示例：

```cpp
// 删除单个通用属性
analysys.unRegisterSuperProperty ("userPoint");
// 清除所有通用属性
analysys.clearSuperAttributes ();
```

#### 2.13.3 获取通用属性

由属性名称查询获取单条通用属性，或者获取全部的通用属性。接口如下：

```cpp
const JObject& getSuperAttributes();
JObject getSuperProperty(const std::string& strKey);
```

\* strKey：属性名称

示例：

```cpp
// 获取单个通用属性
analysys.getSuperProperty("userLevel");
// 获取所有通用属性
analysys. getSuperAttributes ();
```

### 2.14 刷新缓存

```cpp
bool flush();
```

立即发送所有收集的数据到服务器。

## 3. C++ SDK示例

```cpp
string APP_ID = "1234";
string ANALYSYS_SERVICE_URL = "http://192.168.220.167:8089";
AnalysysCPlusPlusSdk analysys;
analysys.init(ANALYSYS_SERVICE_URL, APP_ID);
analysys.syncCollecter();
string distinctId = "1234567890987654321";
try
{
//浏览商品
JObject trackPropertie;
trackPropertie["$ip"] = "112.112.112.112";
JObject bookList;
bookList[0] = "Thinking in Java";
trackPropertie["productName"] = bookList;
trackPropertie["productType"] = "Java book";
trackPropertie["producePrice"] = (long long)80;
trackPropertie["shop"] = "xx shop";
analysys.track(distinctId, false, "ViewProduct", trackPropertie, "JS");
//用户注册登录
string registerId = "ABCDEF123456789";
analysys.alias(registerId, distinctId, "JS");
//设置公共属性
JObject superPropertie;
superPropertie["sex"]= "male";
superPropertie["age"]= (long long)23;
analysys.registerSuperAttributes(superPropertie);
//用户信息
JObject profiles;
profiles["$city"] = "beijin";
profiles["$province"] = "beijin";
profiles["nickName"] = "nickeName123";
profiles["userLevel"] = (long long)0;
profiles["userPoint"] = (long long)0;
JObject interestList;
interestList[0] = "sports";
interestList[1] = "football";
interestList[2] = "game";
profiles["interest"] = interestList;
analysys.profileSet(registerId, true, profiles, "JS");
//用户注册时间
JObject profile_age;
profile_age["registerTime"] = "20180101101010";
analysys.profileSetOnce(registerId, true, profile_age, "JS");
//重新设置公共属性
analysys.clearSuperAttributes();
superPropertie = (long long)1;
superPropertie["userLevel"]=(long long)0; //用户级别
superPropertie["userPoint"]=(long long)0; ///用户积分
analysys.registerSuperAttributes(superPropertie);
//再次浏览商品
trackPropertie = false;
trackPropertie["$ip"]="112.112.112.112"; //IP地址
JObject abookList;
abookList[0]="Thinking in Java";
trackPropertie["productName"]=bookList; //商品列表
trackPropertie["productType"]="Java book";//商品类别
trackPropertie["producePrice"]= (long long)80;//商品价格
trackPropertie["shop"]= "xx shop"; //店铺名称
analysys.track(registerId, true, "ViewProduct", trackPropertie, "JS");
//订单信息
trackPropertie = (long long)1;
trackPropertie["orderId"]= "ORDER_12345";
trackPropertie["price"]=(long long)80;
analysys.track(registerId, true, "Order", trackPropertie, "JS");
//支付信息
trackPropertie = (long long)1;
trackPropertie["orderId"]= "ORDER_12345";
trackPropertie["productName"]= "Thinking in Java";
trackPropertie["productType"]= "Java book";
trackPropertie["producePrice"]= (long long)80;
trackPropertie["shop"]= "xx shop";
trackPropertie["productNumber"]=(long long) 1;
trackPropertie["price"]= (long long)80;
trackPropertie["paymentMethod"]= "AliPay";
analysys.track(registerId, true, "Payment", trackPropertie, "JS");
analysys.batchCollecter();
analysys.track(registerId, true, "Payment", trackPropertie, "JS");
analysys.flush();
}
catch (SDKException& e)
{
 std::string strErr = e.what();
 cout << strErr << endl;
}
```

