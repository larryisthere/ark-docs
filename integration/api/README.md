# API

## 1. API介绍

易观方舟提供一系列的开放API, 方便方舟用户及生态合作伙伴利用API开展基于方舟分析模型的应用程序开发、细粒度的数据应用、自定义的分析模型与OLAP分析，多系统数据源融合治理等。

方舟API目前有四个大类：

* [分析类API](api-analytics/)：用于获取各种分析模型结果，主要包事件分析、留存分析、转化漏斗、属性分析等常用分析模型，主要用于在其他渠道查看方舟统计分析或者对统计指标做二次开发的场景，例如客户自己的BI系统、展示大屏等。
* [用户类API](api-cohort/)：主要包括用户分群的创建、删除，以及获取单个分群的用户明细，获取单个用户的用户属性等，适用于对接客户自己的消息推送系统或者对人群数据进行二次分析等场景。
* [项目类API](api-manage-project/)：主要包括在项目级别下的一些常用功能类API，例如获取项目元事件、属性等基础信息，以及给项目成员分配权限等，常用于对接客户自己的用户权限体系，以便能够简化对成员进行方舟项目授权等手工操作。
* [平台类API](api-manage-enterprise/)：主要包括项目创建、项目数据流的启停、企业成员管理等，主要用于对接客户自己的业务系统，可以通过API 实现对方舟项目和用户的管理和维护。

{% hint style="info" %}
API 章节涉及较多的技术细节，适用于有相关开发经验的开发者参考。

如果对文档内容有疑惑，可以咨询您的项目经理获取相应的技术协助。
{% endhint %}

## 2.调用方法

API 采用标准的 HTTP 方式，调用的 URL 为:

```text
http://{$WEB_URL}/{$API_URL}
```

**$WEB\_URL** ：和访问方舟产品的 URL一致。

**$API\_URL** ：具体的 API 地址。

{% hint style="info" %}
如果私有化部署的过程中修改了Nginx的默认配置，或通过CDN等访问方舟，则请咨询相关人员获取配置信息。
{% endhint %}

## 3.接口认证

所有接口调用时都需要在header中带上认证信息，接口认证分为平台接口认证和项目接口认证两种。

### 2.1 项目接口认证

用于指定项目的相关查询和操作，每个项目的AccessKey只能访问当前项目API，和项目标识appKey一起使用。参数名称使用 token，**非**平台管理API ****的接口都使用项目认证。示例如下：

```text
Request Headers
    Content-Type: application/json
    token: {$API_ACCESSKEY}
    appKey: {$APP_KEY}
```

**$APP\_KEY**为项目标识，对应方舟系统中的AppKey。

**$API\_ACCESSKEY** 为项目API调用的验证标识。可在方舟中【管理】 - 【项目概览】模块中获取，截图如下：

![&#x9879;&#x76EE;&#x6982;&#x89C8;](../../.gitbook/assets/image-20190912174738856.png)

### 2.2 平台接口认证

主要用于项目权限之上的相关查询和操作，参数名称使用 **xtoken**，在 [平台管理API](api-manage-enterprise/) ****的接口中有使用。示例如下：

```text
Request Headers
    Content-Type: application/json
    xtoken: {$SUPER_TOKEN}
```

**$SUPER\_TOKEN** 为企业级接口授权码。在【4.5.3版本】此之前版本联系运维获取，4.5.3版本可在方舟中【企业平台管理】 - 【企业概览】模块中获取，截图如下：

![&#x4F01;&#x4E1A;&#x6982;&#x89C8;](../../.gitbook/assets/image%20%28266%29.png)

## 4.调用示例

获取到 $WEB\_URL 地址和token后，就可以使用任意HTTP Client进行 API 调用，非特殊说明都使用RequestBody方式传参，例如使用 curl工具访问事件分析，例子如下：

```java
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X POST --data '{
    "measures":[
        {
            "filter":{
                "conditions":[
                    {
                        "expression":"event.$Anything.$platform",
                        "function":"EQ",
                        "params":[
                            "JS"
                        ]
                    }
                ],
                "relation":"and"
            },
            "expression":"event.$Anything",
            "aggregator":"TRIGGER_USER_COUNT"
        }
    ],
    "filter":{
        "conditions":[
            {
                "expression":"event.$Anything.$platform",
                "function":"EQ",
                "params":[
                    "JS",
                    "iOS",
                    "Android"
                ]
            }
        ],
        "relation":"AND"
    },
    "byFields":[
        {
            "expression":"event.$Anything.$screen_width",
            "buckets":[
                600,
                800
            ]
        }
    ],
    "crowds":[
        "$ALL"
    ],
    "fromDate":"2019-06-18",
    "toDate":"2019-06-20",
    "samplingFactor":1,
    "useCache":true,
    "unit":"DAY"
}' http://127.0.0.1:4005/uba/api/events/analyze
```

## 5. 接口状态码说明

API调用是否成功都是通过HTTP状态码来识别，接口直接返回结果结构体，以下是系统对应的状态码。

| HttpStatus | HttpMessage | 描述 |
| :--- | :--- | :--- |
| 200 | OK | 接口调用成功 |
| 400 | Bad Request | 请求失败，如参数为空 |
| 401 | Unauthorized | 认证失败，如token/appKey错误 |
| 404 | Not Found | 接口不存在 |
| 415 | Unsupported Media Type | HTTP协议的错误，如Content-Type |
| ~~498~~ | ~~Request method not supported~~ | ~~HTTP请求方法不支持【在4.5.1版本取消，用 400代替】~~ |
| ~~499~~ | ~~The maximum limit has been reached~~ | ~~超出限制，如项目个数超出限额，分群个数超出限额【在4.5.1版本取消，用 501代替】~~ |
| 500 | Internal Server Error | 接口处理异常 |
| 501 | Not Implemented | 接口未完成操作，如项目个数超出限额，分群个数超出限额，对象已存在【在4.5.1版本中新增】 |
| 503 | Service Unavailable | 连接服务不可用，如presto连接异常，数据库连接异常 |

{% hint style="info" %}
HttpStatus 不等于 200时，会返回json格式的结果，结果中包含code、msg、data三部分。
{% endhint %}

* HttpStatus=400时，会返回详细的错误内容（data），data中会罗列具体的错误参数。
* 在HttpStatus=501时，可根据接口返回的code区分具体的错误信息，如下：

| HttpStatus | code | 描述 |
| :--- | :--- | :--- |
| 501 | 501 | 资源超出业务限制，如项目个数超出限额，分群个数超出限额 |
| 501 | 50101 | 对象已存在，如注册用户邮箱重复、分群名称重复 |
| 501 | 50102 | 对象已存在，但是已被禁用 |
| 501 | 50103 | PRESTO 资源不足，调用接口前可以通过接口获取当前presto使用情况 |
| 501 | 50104 | 查询并发数达到上限 |

以下是Postman调用结果示例：

![API&#x8C03;&#x7528;&#x72B6;&#x6001;&#x793A;&#x4F8B;](../../.gitbook/assets/image%20%2875%29.png)

## 6. 指定操作用户

管理类API涉及到新增/修改/删除，为了区分谁操作，在接口传参中都需要带当前操作人对应的邮箱，如果不用区分谁操作，默认为一个方舟中存在的邮箱即可。参数名为loginUser，通过Path方式传参。示例如下：

```java
?loginUser=操作用户的邮箱地址
```

{% hint style="info" %}
以上内容没有解答我的问题？[点击我进入方舟论坛去反馈](https://www.analysysdata.com/forum/index) 🚀
{% endhint %}

