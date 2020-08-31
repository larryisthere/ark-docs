# SDK 指南

易观方舟支持全端数据接入，接入前准备事项详见[《接入前准备》](../prepare/)

易观方舟SDK已全部开源，详见：[Github上的易观方舟SDK](https://github.com/analysys)

## 客户端 SDK 

客户端 SDK 用于客户端，采集用户与应用界面产生交互的行为，这些行为只会在客户端发生，而不会传输到服务器端，常见的比如页面打开、按钮点击等。

| 客户端 SDK 类型 | 适用范围 |
| :--- | :--- |
| [Android SDK](android/) | 用于 Android 原生和 Hybrid App |
| [iOS SDK](ios/) | 用于 iOS 原生和 Hybrid App |
| [JS SDK](js/) | 用于 Web 网站和 H5 页面 |
| [微信小程序 SDK](wx/) | 用于微信小程序 |
| [支付宝小程序 SDK](alipay/) | 用于支付宝小程序 |
| [百度小程序 SDK](baidu.md) | 用于百度小程序 |
| [字节跳动小程序 SDK](bytedance.md) | 用于字节跳动小程序 |
| [钉钉小程序 SDK ](dingtalk/) | 用于钉钉小程序 |
| [Flutter SDK](https://docs.analysys.cn/ark/integration/sdk/flutter-sdk) | 用与Flutter开发的应用 |
| [Reactnative SDK](https://docs.analysys.cn/ark/integration/sdk/reactnative-sdk) | 用与Reactnative开发的应用 |

## **服务端 SDK**

服务端 ****SDK 应用于服务端，采集用户行为产生的业务结果数据，比如注册成功、支付成功等，是在程序与数据交互的界面埋点，能够更准确的记录业务数据的改变。

| 服务端 SDK 类型 | 适用范围 |
| :--- | :--- |
| [Java SDK](java.md) | 用于服务端 Java 应用，如 Java Web 应用的后台服务 |
| [Python SDK](python.md) | 用于服务端 Python 应用，如 Python Web 应用的后台服务 |
| [PHP SDK](php.md) | 用于服务端 PHP 应用，如 PHP Web 应用的后台服务 |
| [C++ SDK](c++.md) | 用于服务端 C++ （也称CPP、C plus plus）应用，如 C++ 应用的后台服务 |
| [C\# SDK](dotnet.md) | 用于服务端或客户端 C\# 应用 |
| [Node JS SDK](node-sdk.md) | 用于服务端 Node 应用，如 Node Web 应用的后台服务 |
| [Lua SDK](lua-sdk.md) | 用于服务端 Lua 应用 |
| [Golang SDK](golang-sdk.md) | 用于服务端 Golang 应用 |

### 应用场景

**1 采集业务结果数据比客户端 SDK 更准确**

业务结果数据指注册、订单、支付等，会存在于业务数据库中的数据。相比客户端 SDK 更准确的原因有：1. 网络传输造成的丢数  2. 客户端SDK尚未上送数据时，页面已经跳转，造成一定比例的数据丢失，尤其是支付等会跳转到第三方服务再跳转回来的场景，数据丢失的比例会更大等等。

**2 关键业务变动，但用户没有行为**

有些数据客户端 SDK 无法采集到，比如货到付款数据、超时不支付订单自动取消数据、物流状态变化数据等，但这些数据对用户行为的业务结果分析至关重要，就可以使用服务端 SDK

**3 其他业务系统关键数据的同步**

比如CRM中客户跟进状态变化的数据，B2B企业客户付费情况变化的数据等

{% hint style="info" %}
数据量较小时，可采用服务端SDK，该场景还可使用 Restful-API 或导入工具实现
{% endhint %}

**4 业务系统历史数据导入**

比如历史注册、历史订单、历史支付等

{% hint style="info" %}
数据量较小时，可采用服务端SDK，该场景还可使用 Restful-API 或导入工具实现

另外需注意：历史数据导入要先于通过埋点采集新发生的数据
{% endhint %}

### **注意事项**

#### **1 基础配置**

1. 数据接收地址：[`http://host:port`](http://hostport/)
2. AppKey，确定数据上报的项目

**更多具体配置详见各个SDK的文档**

#### **2 必报数据**

{% hint style="info" %}
**一句话概括：**

**什么人，在什么时间，从什么地方来，在什么环境下，做了什么事，有什么结果**
{% endhint %}

**什么人**

1. isLogin：用户 ID 是否是登录 ID；采集业务结果数据时，该值一般为“是”，即“true”
2. aliasId：用户注册 ID，长度大于 0，且小于 255字符；需从客户端传递到服务端
3. distinctId：自定义设备身份标识，长度大于 0 且小于 255字符

**在什么时间**

xwhen：用户自定义时间戳（带毫秒的13位时间戳）

**从什么地方来**

session\_id：由方舟客户端SDK生成，用于识别流量来源，从客户端传递到服务端后，才能在方舟渠道分析/Session分析等分析模型中，按来源类属性细分时，查看到不同来源下的相关数据

{% hint style="info" %}
如果分析渠道转化目标的事件比如注册成功等，是在服务端进行埋点的，一定要注意 session\_id 的传递
{% endhint %}

**在什么环境下**

platform：平台类型，支持自定义

{% hint style="info" %}
为保证与用户发生行为的平台对应，便于更好的让数据还原真实用户行为，建议内容范围为：JS、WeChat、Android、iOS、Alipay、Baidu、ByteDance、DingTalk、QQMini、Quickapp 方舟客户端 SDK 所支持的平台，此时，需从客户端传递到服务端。
{% endhint %}

**做了什么事**

eventName：自定义事件 ID 标识（需与事件设计方案保持一致）

以字母开头的字符串，必须由字母、数字、下划线组成，$ 开头为预置事件/属性，不支持乱码、中文、空格等，长度范围1～100字符

#### **有什么结果**

properties：自定义事件属性

最多包含 100条（需与事件设计方案保持一致），且key是以字母开头的字符串，必须由字母、数字、下划线组成，字母不区分大小写，不支持乱码、中文、空格等，长度范围1～100字符；value 类型约束\(String/Number/boolean/list/数组\)，若为字符串，最大长度 255 字符。

{% hint style="danger" %}
所有语言类型的服务端 SDK 使用时，上述字段均为必填字段，并遵循上述使用方法；否则，会出现数据缺失或无法对应的情况，导致数据无法反映真实用户行为，基于这样的数据进行的分析，无法帮助业务成长。
{% endhint %}

## **数据导入**

{% page-ref page="../import/" %}

## 常见问题

{% page-ref page="sdk-faq/" %}

