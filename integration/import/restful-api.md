---
description: 通过 Restful API 可以将历史数据通过网络上报到易观方舟，这种方式适用于不方便登录方舟服务器，并且数据量不大的情况。
---

# 接口导入

## 数据接口

### 功能描述

调用该接口，把符合特定格式的数据以POST的方式上报至数据接收服务器。接收服务器对上报的数据进行校验，不符合格式的返回相应的错误提示。易观SDK就是通过该API进行数据上报。

上报后的数据会先暂存在 Kafka 中，流处理引擎大约会以3000条/秒的速度将数据落库并可用于查询，该过程性能受服务器影响，但偏差一般不会太大。

### 接口协议

HTTP\(S\)，POST方式

### 请求地址

```text
http(s)://host:port/up
```

### 请求数据

请求的 Body 体里面存放具体要上报的数据，数据明文为 JsonArray 的形式。上报的数据明文示例如下：

```javascript
[{
    "appid": "demo",
    "xwho": "8c0eebf0-2383-44bc-b8ba-a5c719fc6194",
    "xwhat": "confirmOrder",
    "xwhen": 1532514947857,
    "xcontext": {
        "$channel": "豌豆荚",
        "$app_version": "4.0.4.001",
        "$model": "MI 6X",
        "$os": "Android",
        "$os_version": "8.1.0",
        "$lib": "Android",
        "$platform": "Android",
        "$is_login": false,
        "$lib_version": "4.0.4",
        "$debug": 2,
        "$importFlag": 1     //说明：$importFlag字段是专门用作导数
    }
}]
```

### 数据编码

请使用 UTF-8 编码。上报数据可以使用明文上报，也可以对数据进行压缩/编码处理后进行上报。压缩/编码过程具体为：先进行Gzip压缩，然后进行Base64编码，最后把编码后的数据直接放到Body体里面上报即可。

### 应答格式

* 上报成功：{"code":200}
* 上报失败：{"code":500}
* 上报数据格式错误：{"code":xxx, "msg":"xxxxx"}，返回的应答消息中包含"msg"字段，内容为具体的异常信息。

API调用参考示例

```text
String uploadUrl = "http://ip:port/up";
StringBuffer buffer = new StringBuffer();
buffer.append("[{");
buffer.append("\"appid\":\"demo\",");
buffer.append("\"xwho\":\"8c0eebf0-2383-44bc-b8ba-a5c719fc6194\",");
buffer.append("\"xwhat\":\"confirmOrder\",");
buffer.append("\"xwhen\":1532514947857,");
buffer.append("\"xcontext\":{");
buffer.append("\"$channel\":\"豌豆荚\",");
buffer.append("\"$app_version\":\"4.0.4.001\",");
buffer.append("\"$model\":\"MI6X\",");
buffer.append("\"$os\":\"Android\",");
buffer.append("\"$os_version\":\"8.1.0\",");
buffer.append("\"$lib\":\"Android\",");
buffer.append("\"$platform\":\"Android\",");
buffer.append("\"$is_login\":false,");
buffer.append("\"$lib_version\":\"4.0.4\",");
buffer.append("\"$debug\":2,");
buffer.append("\"$importFlag\":1");
buffer.append("}");
buffer.append("}]");
String uploadJsonData = buffer.toString();

/*// 对数据进行压缩/编码(可选)
String gzipUploadJsonData = GZIPInputStreamUtil.encode(uploadJsonData); // GZIP压缩
String base64UploadJsonData = Base64Util.encode(gzipUploadJsonData);    // Base64编码
uploadJsonData = base64UploadJsonData;*/

CloseableHttpClient httpclient = HttpClients.createDefault();
    try {
        HttpPost httpPost = new HttpPost(uploadUrl);
        StringEntity req = new StringEntity(uploadJsonData);
        req.setContentEncoding("UTF-8");
        httpPost.setEntity(req);
        httpclient.execute(httpPost);
        } catch (Exception e) {
            exceptionHandle(e);
        } finally {
            try {
                httpclient.close();
            } catch (IOException e) {
                logger.errot(e);
            }
        }
```

{% hint style="info" %}
没有特殊需求的情况下，不建议使用该API直接上报数据。推荐使用SDK进行数据上报，SDK有完善的数据校验等功能，更安全可靠。
{% endhint %}

## 数据格式及约束

数据上报需要符合一定的格式以及规范。服务器会对上报的数据做有效性校验，不符合规范的数据会返回给调用方错误信息。

### 字段说明

 **上报报文必须包含`xwho`/`xwhat`/`xwhen`/`appid`/`xcontext`这几个key，具体含义如下：**

appid：字符串，等价于一个产品，推荐跨平台使用。

{% hint style="danger" %}
appid为该产品特有的标识，非法值或恶意篡改的值上报至服务器会返回错误。

Appid等于`Appkey`可以在统计平台-项目管理中获取
{% endhint %}



xwho：字符串，长度大于0且小于255字符。不可以为中文。 用户定义的用户唯一性标准 xwhat：字符串， 需要符合Java命名规则： 开头约束:字母或者$ 字符类型:大小写字母、数字、下划线和 $ 最大长度是99字符。不可以为中文。任何主体主动/被动触发事件的的事件名除内置预定义事件外，其他事件全部来源用户。主要分为三种类型: 用户事件\(Profile/User\)，可以修改；普通事件\(Event/Track\) ，不允许修改；预置事件，不建议用户覆盖，但是可以强行覆盖。 

xwhen：数字型\(Long类型\)，事件触发的时间。默认使用本地时间，但是需要将时区信息放到xcontext中。

xcontext：JSON格式，数据结构是KV结构。主要包含: 描述信息。所有事件xcontext中必须包含五个字段: $platform、$lib、$is\_login、$lib\_version、$debug 以及 $importFlag。

$importFlag：数值型，值为1，必填。用于告诉方舟该条数据是历史数据，不做时间窗口校验并且允许进行查询。

$lib：字符串，SDK类型，选项有:Java、Python、JS、Node、PHP、Wechat、Andorid、iOS。

$platform：字符串，平台信息，和lib相同；也可自定义添加平台信息。

$lib\_version：字符串，SDK版本号

$debug： 数值型，是否为DEBUG模式。 

> 三个选项：
>
> 0==非debug
>
> 1==debug不入库
>
> 2==debug入库

$is\_login：布尔值，用于标识该事件是否有已登录用户产生

### 支持类型

上报报文的Json数据结构中的Key值类型如下：

符合java命名规则： 开头约束:字母或者$ 字符类型:大小写字母、数字、下划线和 $ 最大长度125字符 上报报文的Json数据结构中的value值类型如下：

`Number` ：int/long/double/float 

`Boolean` ：数据传输中使用true/false,后端处理转换为0,1 

`String`：字符串,后端支持时间格式: DATE/DATETIME 

`Array`：类型 List、数组\(这种格式的:\[\]\),传输给服务器全部转化为JSONArray

### 约束条件

上报的json中的key以及xwhat的value值需要符合Java命名规则。

`key/xwhat的value约束`: 以字母或$开头，可包含大小写字母/数字/\_/$，最大长度125字符，不支持乱码和中文

`xwhat`的value值最大长度是99字符

`id`限制：长度大于0且小于255字符

`xcontext`约束：

`key约束：`符合java命名规则： 开头约束:字母或者$ 字符类型:大小写字母、数字、下划线和 $ 最大长度125字符

`value约束：` 类型约束\(String/Number/boolean/list/数组\)，若为字符串，最大长度255

`个数约束：` xcontext中承载的属性设置最大个数是300个。数组集合约束 数组或集合内最多包含100条。 若为字符串数组或集合，每条最大长度255个字符。

### 预置事件

事件名称，即xwhat值。可以理解成：某一个体在某一时间点的状态。 当触发相应接口时，只发送相关数据，没有设备信息。用户属性是一个标准的K-V结构，K和V均有相应的约束条件，如不符合则丢弃该次操作。 

> **参数约束**
>
> 属性名：以字母或$开头,可包含大小写字母/数字/\_/$,最大长度125字符,不支持乱码和中文
>
> 属性值：支持部分类型：String/Number/boolean/集合/数组; 若为字符串,则最大长度255字符; 若为数组或集合,则最多包含100条,且key约束条件与属性名称一致,value最大长度255字符。

#### 用户类型事件

有以下几种类型：

`$profile_set`: 给用户设置单个或多个属性，比如用户的年龄/性别等信息。如果之前不存在，则新建，否则覆盖。

上传报文示例：

```javascript
[{
    "xwhen": 1532756553883,
    "xcontext": {
        "$lib": "Android",
        "$debug": 0,
        "$is_login": true,
        "userPoint": 0,
        "userLevel": 0,
        "interest": ["户外活动", "足球赛事", "游戏"],
        "nickName": "昵称123",
        "$lib_version": "4.0.3",
        "$platform": "Android"
    },
    "appid": "1234",
    "xwho": "ABCDEF123456789",
    "xwhat": "$profile_set"
}]
```

 `$profile_set_once:`设置用户的固有属性，只在首次设置时有效的属性。如：用户的注册时间。如果被设置的用户属性已存在，则这条记录会被忽略而不会覆盖已有数据，如果属性不存在则会自动创建。上传报文示例：

```javascript
[{
	"xwhen": 1532756655466,
	"xcontext": {
		"$lib": "Android",
		"$debug": 0,
		"$is_login": true,
		"registerTime": "20180101101010",
		"$lib_version": "4.0.3",
		"$platform": "Android"
	},
	"appid": "1234",
	"xwho": "ABCDEF123456789",
	"xwhat": "$profile_set_once"
}]
```

`$profile_unset`: 删除某个已设置的用户属性。上传报文示例：

```javascript
[{
	"xwhen": 1532757129288,
	"xcontext": {
		"$lib": "Android",
		"$debug": 0,
		"$is_login": true,
		"nickName": "",
		"$lib_version": "4.0.3",
		"$platform": "Android"
	},
	"appid": "1234",
	"xwho": "ABCDEF123456789",
	"xwhat": "$profile_unset"
}]
```

`$profile_increment`:设置用户属性的相对变化值\(相对增加，减少\)，只能对数值型属性进行操作，携带参数必须为 Number类型。如果这个Profile之前不存在,则初始值为0。上传报文示例：

```javascript
[{
	"xwhen": 1532756825695,
	"xcontext": {
		"$lib": "Android",
		"$debug": 0,
		"$is_login": true,
		"userPoint": 20,
		"$lib_version": "4.0.3",
		"$platform": "Android"
	},
	"appid": "1234",
	"xwho": "ABCDEF123456789",
	"xwhat": "$profile_increment"
}]
```

`$profile_append`: 用户列表属性增加元素，上行协议的值为Array结构。为列表类型的属性增加一个或多个元素，如：用户新增兴趣爱好。上传报文示例：

```javascript
[{
	"xwhen": 1532757076341,
	"xcontext": {
		"$lib": "Android",
		"$debug": 0,
		"$is_login": true,
		"interest": ["户外活动", "足球赛事", "游戏"],
		"$lib_version": "4.0.3",
		"$platform": "Android"
	},
	"appid": "1234",
	"xwho": "ABCDEF123456789",
	"xwhat": "$profile_append"
}]
```

$profile\_delete:删除全部已设置的用户属性。上传报文示例：

```javascript
[{
	"xwhen": 1532757239032,
	"xcontext": {
		"$lib": "Android",
		"$debug": 0,
		"$is_login": true,
		"$platform": "Android",
		"$lib_version": "4.0.3"
	},
	"appid": "1234",
	"xwho": "ABCDEF123456789",
	"xwhat": "$profile_delete"
}]
```

身份关联：该场景一般应用在用户注册/登录的过程中。比如：一个匿名用户浏览商品，系统为其分配的DistinctId = "1234567890987654321"，随后该匿名用户进行注册，系统为其分配了新的注册ID，aliasId = "ABCDEF123456789"，此时就需要调用alias接口对两个ID进行关联。

`$alias`:关联身份, 计算时会认为是一个用户的行为。上传报文示例：

```javascript
[{
	"xwhen": 1532757366067,
	"xcontext": {
		"$lib": "Android",
		"$debug": 0,
		"$is_login": true,
		"$original_id": "1234567890987654321",
		"$lib_version": "4.0.3",
		"$platform": "Android"
	},
	"appid": "1234",
	"xwho": "ABCDEF123456789",
	"xwhat": "$alias"
}]
```

#### 系统启动/页面事件

$pageview：浏览页面，打开页面时触发

```javascript
[{
	"xwhen": 1532757366067,
	"xcontext": {
		"$lib": "Android",
		"$debug": 0,
		"$is_login": true,
		"$original_id": "1234567890987654321",
		"$lib_version": "4.0.3",
		"$platform": "Android"
	},
	"appid": "1234",
	"xwho": "ABCDEF123456789",
	"xwhat": "$alias"
}]
```

 `$startup`：安装后，首次启动事件。上传报文示例：

```javascript
[{
	"appid": "1234",
	"xwho": "ABCDEF123456789",
	"xwhat": "$startup",
	"xwhen": 1532687468414,
	"xcontext": {
		"$is_first_time": false,
		"$is_from_background": false,
		"$channel": "测试",
		"$manufacturer": "Xiaomi",
		"$app_version": "1.0",
		"$model": "MIX 2S",
		"$os": "Android",
		"$os_version": "8.0.0",
		"$network": "WIFI",
		"$screen_width": 1080,
		"$screen_height": 2030,
		"$brand": "Xiaomi",
		"$is_first_day": true,
		"$lib": "Android",
		"$platform": "Android",
		"$is_login": false,
		"$lib_version": "4.0.4",
		"$debug": 2
	}
}]
```

### 数据上报示例

以下示例是追踪一个匿名用户浏览商品，然后注册系统后再次浏览商品，然后生成订单，最后支付的流程

```java
String appid = "1234";
String service_url = "http://host:port/up;
String distinctId = "1234567890987654321"; //用户匿名ID
//浏览商品
StringBuffer buffer = new StringBuffer();
buffer.append("[{");
buffer.append("   \"xwho\":\"" + distinctId + "\",");
buffer.append("   \"xwhen\":\"" + System.currentTimeMillis() + "\",");
buffer.append("   \"xwhat\":\"ViewProduct\",");
buffer.append("   \"appid\":\"" + appid + "\",");
buffer.append("   \"xcontext\":{");
buffer.append("       \"$ip\":\"112.112.112.112\",");
buffer.append("       \"productName\":[\"Thinking in Java\"],");
buffer.append("       \"productType\":\"Java书籍\",");
buffer.append("       \"producePrice\":80,");
buffer.append("       \"shop\":\"xx网上书城\",");
buffer.append("       \"$platform\":\"Android\",");
buffer.append("       \"$lib\":\"Android\",");
buffer.append("       \"$is_login\":false,");
buffer.append("       \"$lib_version\":\"0.1.0\",");
buffer.append("       \"$debug\":0");
buffer.append("   }");
buffer.append("}]");
postDataToServer(service_url, buffer.toString());
//用户注册登录
buffer = new StringBuffer();
String registerId = "ABCDEF123456789";
buffer.append("[{");
buffer.append("   \"xwho\":\"" + registerId + "\",");
buffer.append("   \"xwhen\":\"" + System.currentTimeMillis() + "\",");
buffer.append("   \"xwhat\":\"$alias\",");
buffer.append("   \"appid\":\"" + appid + "\",");
buffer.append("   \"xcontext\":{");
buffer.append("       \"$platform\":\"Android\",");
buffer.append("       \"$lib\":\"Android\",");
buffer.append("       \"$is_login\":true,");
buffer.append("       \"$lib_version\":\"0.1.0\",");
buffer.append("       \"$debug\":0,");
buffer.append("       \"$original_id\":\"" + distinctId + "\"");
buffer.append("   }");
buffer.append("}]");
postDataToServer(service_url, buffer.toString());
//用户信息
buffer = new StringBuffer();
buffer.append("[{");
buffer.append("   \"xwho\":\"" + registerId + "\",");
buffer.append("   \"xwhen\":\"" + System.currentTimeMillis() + "\",");
buffer.append("   \"xwhat\":\"$profile_set\",");
buffer.append("   \"appid\":\"" + appid + "\",");
buffer.append("   \"xcontext\":{");
buffer.append("       \"$platform\":\"Android\",");
buffer.append("       \"$lib\":\"Android\",");
buffer.append("       \"$is_login\":true,");
buffer.append("       \"$lib_version\":\"0.1.0\",");
buffer.append("       \"$debug\":0,");
buffer.append("       \"nickName\":\"昵称123\",");
buffer.append("       \"userLevel\":0,");
buffer.append("       \"userPoint\":0,");
buffer.append("       \"interest\":[\"户外活动\",\"足球赛事\",\"游戏\"]");
buffer.append("   }");
buffer.append("}]");
postDataToServer(service_url, buffer.toString());
//用户注册时间
buffer = new StringBuffer();
buffer.append("[{");
buffer.append("   \"xwho\":\"" + registerId + "\",");
buffer.append("   \"xwhen\":\"" + System.currentTimeMillis() + "\",");
buffer.append("   \"xwhat\":\"$profile_set_once\",");
buffer.append("   \"appid\":\"" + appid + "\",");
buffer.append("   \"xcontext\":{");
buffer.append("       \"$platform\":\"Android\",");
buffer.append("       \"$lib\":\"Android\",");
buffer.append("       \"$is_login\":true,");
buffer.append("       \"$lib_version\":\"0.1.0\",");
buffer.append("       \"$debug\":0,");
buffer.append("       \"registerTime\":\"20180101101010\"");
buffer.append("   }");
buffer.append("}]");
postDataToServer(service_url, buffer.toString());

//再次浏览商品
buffer = new StringBuffer();
buffer.append("[{");
buffer.append("   \"xwho\":\"" + registerId + "\",");
buffer.append("   \"xwhen\":\"" + System.currentTimeMillis() + "\",");
buffer.append("   \"xwhat\":\"ViewProduct\",");
buffer.append("   \"appid\":\"" + appid + "\",");
buffer.append("   \"xcontext\":{");
buffer.append("       \"$ip\":\"112.112.112.112\",");
buffer.append("       \"productName\":[\"Thinking in Java\"],");
buffer.append("       \"productType\":\"Java书籍\",");
buffer.append("       \"producePrice\":80,");
buffer.append("       \"shop\":\"xx网上书城\",");
buffer.append("       \"$platform\":\"Android\",");
buffer.append("       \"$lib\":\"Android\",");
buffer.append("       \"$is_login\":true,");
buffer.append("       \"$lib_version\":\"0.1.0\",");
buffer.append("       \"$debug\":0");
buffer.append("   }");
buffer.append("}]");
postDataToServer(service_url, buffer.toString());
//订单信息
buffer = new StringBuffer();
buffer.append("[{");
buffer.append("   \"xwho\":\"" + registerId + "\",");
buffer.append("   \"xwhen\":\"" + System.currentTimeMillis() + "\",");
buffer.append("   \"xwhat\":\"Order\",");
buffer.append("   \"appid\":\"" + appid + "\",");
buffer.append("   \"xcontext\":{");
buffer.append("       \"orderId\":\"ORDER_12345\",");
buffer.append("       \"price\":80,");
buffer.append("       \"$platform\":\"Android\",");
buffer.append("       \"$lib\":\"Android\",");
buffer.append("       \"$is_login\":true,");
buffer.append("       \"$lib_version\":\"0.1.0\",");
buffer.append("       \"$debug\":0");
buffer.append("   }");
buffer.append("}]");
postDataToServer(service_url, buffer.toString());
//购买商品
buffer = new StringBuffer();
buffer.append("[{");
buffer.append("   \"xwho\":\"" + registerId + "\",");
buffer.append("   \"xwhen\":\"" + System.currentTimeMillis() + "\",");
buffer.append("   \"xwhat\":\"Payment\",");
buffer.append("   \"appid\":\"" + appid + "\",");
buffer.append("   \"xcontext\":{");
buffer.append("       \"orderId\":\"ORDER_12345\",");
buffer.append("       \"price\":80,");
buffer.append("       \"productNumber\":\"AliPay\",");
buffer.append("       \"paymentMethod\":80,");
buffer.append("       \"productName\":[\"Thinking in Java\"],");
buffer.append("       \"productType\":\"Java书籍\",");
buffer.append("       \"producePrice\":80,");
buffer.append("       \"$platform\":\"Android\",");
buffer.append("       \"$lib\":\"Android\",");
buffer.append("       \"$is_login\":true,");
buffer.append("       \"$lib_version\":\"0.1.0\",");
buffer.append("       \"$debug\":0");
buffer.append("   }");
buffer.append("}]");
postDataToServer(service_url, buffer.toString());
```

以上各个流程对应的上报的JSON明文如下：

浏览商品：

```javascript
[{
	"xwho": "1234567890987654321",
	"xwhen": "1532683122298",
	"xwhat": "ViewProduct",
	"appid": "1234",
	"xcontext": {
		"$ip": "112.112.112.112",
		"productName": ["Thinking in Java"],
		"productType": "Java书籍",
		"producePrice": 80,
		"shop": "xx网上书城",
		"$platform": "Android",
		"$lib": "Android",
		"$is_login": false,
		"$lib_version": "0.1.0",
		"$debug": 0
	}
}]
```

用户注册登录：

```javascript
[{
	"xwho": "ABCDEF123456789",
	"xwhen": "1532683122298",
	"xwhat": "$alias",
	"appid": "1234",
	"xcontext": {
		"$platform": "Android",
		"$lib": "Android",
		"$is_login": true,
		"$lib_version": "0.1.0",
		"$debug": 0,
		"$original_id": "1234567890987654321"
	}
}]
```

用户信息：

```javascript
[{
	"xwho": "ABCDEF123456789",
	"xwhen": "1532683122298",
	"xwhat": "$profile_set",
	"appid": "1234",
	"xcontext": {
		"$platform": "Android",
		"$lib": "Android",
		"$is_login": true,
		"$lib_version": "0.1.0",
		"$debug": 0,
		"nickName": "昵称123",
		"userLevel": 0,
		"userPoint": 0,
		"interest": ["户外活动","足球赛事","游戏"]
	}
}]
```

用户注册时间：

```javascript
[{
	"xwho": "ABCDEF123456789",
	"xwhen": "1532683122298",
	"xwhat": "$profile_set_once",
	"appid": "1234",
	"xcontext": {
		"$platform": "Android",
		"$lib": "Android",
		"$is_login": true,
		"$lib_version": "0.1.0",
		"$debug": 0,
		"registerTime": "20180101101010"
	}
}]
```

再次浏览商品：

```javascript
[{
	"xwho": "ABCDEF123456789",
	"xwhen": "1532683122298",
	"xwhat": "ViewProduct",
	"appid": "1234",
	"xcontext": {
		"$ip": "112.112.112.112",
		"productName": ["Thinking in Java"],
		"productType": "Java书籍",
		"producePrice": 80,
		"shop": "xx网上书城",
		"$platform": "Android",
		"$lib": "Android",
		"$is_login": true,
		"$lib_version": "0.1.0",
		"$debug": 0
	}
}]
```

订单信息：

```javascript
[{
	"xwho": "ABCDEF123456789",
	"xwhen": "1532683122298",
	"xwhat": "Order",
	"appid": "1234",
	"xcontext": {
		"orderId": "ORDER_12345",
		"price": 80,
		"$platform": "Android",
		"$lib": "Android",
		"$is_login": true,
		"$lib_version": "0.1.0",
		"$debug": 0
	}
}]
```

购买商品：

```javascript
[{
	"xwho": "ABCDEF123456789",
	"xwhen": "1532683122298",
	"xwhat": "Payment",
	"appid": "1234",
	"xcontext": {
		"orderId": "ORDER_12345",
		"price": 80,
		"productNumber": "AliPay",
		"paymentMethod": 80,
		"productName": ["Thinking in Java"],
		"productType": "Java书籍",
		"producePrice": 80,
		"$platform": "Android",
		"$lib": "Android",
		"$is_login": true,
		"$lib_version": "0.1.0",
		"$debug": 0
	}
}]
```

{% hint style="info" %}
以上内容没有解答我的问题？[点击我进入方舟论坛去反馈](https://www.analysysdata.com/forum/index) 🚀
{% endhint %}

