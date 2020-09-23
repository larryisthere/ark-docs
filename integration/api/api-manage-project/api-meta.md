---
description: 方舟4.3.5版本中新增API
---

# 元数据管理

## 1. 获取用户属性

获取用户上报成功并且在计划中的用户属性，含系统预置但页面上隐藏的用户属性。

### 1.1 接口地址

> 【GET】 /uba/api/meta/userProperties

### 1.2 请求参数示例

无。

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。

### 1.3 返回结果示例

```java
[
    {
        //用户属性ID，唯一
        "id":"xwho",
        //用户属性名称，用于页面展示
        "name":"用户ID",
        //是否可用 1为启用 0为禁用 （通过方舟系统可以控制用户属性的可用性）
        "enable":1,
        //是否可见 1为可见 0为不可见（用于方舟系统，系统预置但不用于分析的属性在页面上被隐藏了）
        "visible":1,
      	//【4.5.0中新增】是否预置属性 1为预置属性 0为自定义属性
      	"preset": 1,
      	//【4.5.0中新增】数据类型，有string、boolean、number、datetime、array<string>五种
        "dataType": "string",
      	//【4.5.0中新增】是否有字典 1为有字典 0为未上传字典
        "dict": 0
    },
  	{
        "id": "company",
        "name": "所在公司",
        "enable": 1,
      	//页面不可见
        "visible": 0,
      	//自定义属性
        "preset": 0,
        "dataType": "string",
      	//有字典
        "dict": 1
    }
]
```

### 1.4 接口调用示例

```java
curl -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" http://127.0.0.1:4005/uba/api/meta/userProperties
```

## 2. 获取元事件列表

获取在埋点方案中并且已经回数的事件列表，含被禁用事件。

### 2.1 接口地址

> 【GET】 /uba/api/meta/event

### 2.2 请求参数示例

```java
//【选填 5.1.0086版本新增】通过urlPath传参
type=event&enable=1
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。

#### **2.2.1 入参说明**

| 参数名称 | 类型 | 必填 | 说明 | 枚举 |
| :--- | :--- | :--- | :--- | :--- |
| type | String | N | 事件类型，可获取所有和指定类型【5.1】 | all/event/virtual |
| enable | Integer | N | 是否回数：1为已回数 0为未回数【5.1】 | 0/1 |

### 2.3 返回结果示例

```java
[
    {
        //事件ID，唯一
        "id":"$startup",
        //事件名称，用于页面展示
        "name":"启动",
        //是否可用 1为启用 0为禁用 （通过方舟系统可以控制事件的可用性）
        "enable":1,
        //备注，描述事件的作用
        "remark":"APP启动 / 打开网站",
      	//【4.5.0中新增】是否预置事件 1为预置事件 0为自定义事件
      	"preset": 1,
        //【4.6中新增】回数平台，多个值之间逗号隔开
        "platform": "Android,iOS,JS",
         //【5.1.0086中新增】事件类型
         "type":"event",
      		//【5.1.0086中新增】如果是虚拟事件，则会返回虚拟事件的创建规则
      	 "content":null
    },
    {
        "id":"$end",
        "name":"关闭",
      	//禁用
        "enable":0,
        "remark":"APP关闭 / 关闭网页",
      	"preset": 1,
        "platform": "Android,iOS",
        "type":"event",
      	"content":null
    },
  	{
        "id": "login",
        "name": null,
        "enable": 1,
        "remark": null,
      	//自定义
        "preset": 0,
        "platform": "Android",
        //虚拟事件
        "type":"virtual",
        //虚拟事件的创建规则
        "content":{}
    }
]
```

如果是虚拟事件，则会返回虚拟事件的创建规则，格式如下：

```java
{
    "events": [
        {
            //包含的事件
            "expression": "event.$startup",
            //事件需要满足的条件
            "filter": {
                "conditions": [
                    {
                        "expression": "event.$startup.$platform",
                        "function": "EQ",
                        "params": [
                            "JS"
                        ]
                    }
                ],
                "relation": "AND"
            }
        },
        {
            "expression": "event.$pageview"
        }
    ],
    "relation": "OR"
}
```

### 2.4 接口调用示例

```java
curl -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" http://127.0.0.1:4005/uba/api/meta/event
```

## 3. 获取事件属性

获取事件的属性列表，包含**事件自定义属性**和**通用属性**两种，自定义属性需要自己埋点上报，通用属性由方舟系统自动采集。

### 3.1 接口地址

> 【GET】 /uba/api/meta/eventProperties

### 3.2 请求参数示例

```java
//【必填】通过urlPath传参
eventId=?
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。

### 3.3 返回结果示例

```java
[
    {
        //事件属性ID
        "id":"$is_first_time",
        //事件属性名称，用于页面展示
        "name":null,
        //是否可用 1为启用 0为禁用 （通过方舟系统可以控制事件属性的可用性）
        "enable":1,
        //是否通用属性，1为通用属性 0为事件定义属性
        "global":0,
      	//【4.5.0中新增】是否事件预置属性 1为预置属性 0为自定义属性
      	"preset": 1,
      	//【4.5.0中新增】数据类型，有string、boolean、number、datetime、array<string>五种
        "dataType": "boolean",
      	//【4.5.0中新增】是否有字典 1为有字典 0为未上传字典
      	"dict": 0
    },
    {
        "id":"$platform",
        "name":"平台",
        //属性可用
        "enable":1,
        //通用属性（通用属性表示每个事件都有这个属性）
        "global":1,
      	"preset":1,
        "dataType": "string",
        "dict": 0
    }
]
```

> **global**： 0为事件自定义属性，1为通用属性。

### 3.4 接口调用示例

```java
curl -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" http://127.0.0.1:4005/uba/api/meta/eventProperties?eventId=%24startup
```

## 4. 创建虚拟属性

上报的元事件无法直接满足分析需求，组要组合使用事件时，可以使用虚拟事件。虚拟事件可以组合多个元事件，每个元事件下也可以设置过滤条件，事件之间关系为或。在 5.1.0221 版本中新增。

### 4.1 接口地址

> 【POST】 /uba/api/meta/event/virtual

### 4.2 请求参数示例

```java
{
    //【必填】虚拟事件ID，唯一，并且不能和元事件ID重名，由字母、数字和下划线组成，不能以数字开头，总长度不超过100个字符
    "id":"vactive",
    //虚拟事件名称，长度不能超过 50
    "name":"活跃用户",
    //虚拟事件说明，长度不能超过 100
    "remark":"使用过APP或者浏览过web网站的用户",
    //【必填】虚拟事件包含的元事件
    "events":[
        {
            //包含的元事件 不含条件
            "expression":"event.$startup"
        },
        {
            //包含的元事件 含条件
            "expression":"event.$pageview",
            //事件需要满足的条件（平台=JS的页面浏览事件）
            "filter":{
                "conditions":[
                    {
                        //条件表达式
                        "expression":"event.$pageview.$platform",
                        "function":"EQ",
                        "params":[
                            "JS"
                        ]
                    }
                ],
                "relation":"AND"
            }
        }
    ]
}
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。

### 4.3 返回结果示例

```javascript
{"success":0}
```

### 4.4 接口调用示例

```haskell
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X POST --data '{
    "id":"$vactive",
    "name":"活跃用户",
    "remark":"使用过APP或者浏览过web网站的用户",
    "events":[
        {
            "expression":"event.$startup"
        },
        {
            "expression":"event.$pageview"
        }
    ]
}' http://127.0.0.1:4005/uba/api/meta/event/virtual
```

## 5. 修改虚拟属性

支持修改虚拟事件的名称、描述、规则内容。在 5.1.0221 版本中新增。

虚拟事件修改规则是只能修改自己创建的虚拟事件，如果API不带loginUser参数只能修改通过API创建并且未带loginUser的虚拟事件。如果带loginUser参数，就能修改对应用户创建的虚拟事件。

### 5.1 接口地址

> 【PUT】 /uba/api/meta/event/virtual

### 5.2 请求参数示例

```java
{
    //【必填】虚拟事件ID，因为事件ID不允许修改，所以ID会作为修改条件
    "id":"vactive",
    //虚拟事件名称，长度不能超过 50，如果不需要修改名称，可以不传此列
    "name":"活跃用户",
    //虚拟事件说明，长度不能超过 100，如果不需要修改描述，可以不传此列
    "remark":"使用过APP或者浏览过web网站的用户",
    //虚拟事件包含的元事件，如果规则不需要发生改变，可以不传
    "events":[
        {
            //包含的元事件 不含条件
            "expression":"event.$startup"
        },
        {
            //包含的元事件 含条件
            "expression":"event.$pageview"
        }
    ]
}
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。

### 5.3 返回结果示例

```java
{"success":0}
```

### 5.4 接口调用示例

```java
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X PUT --data '{
    "id":"$vactive",
    "name":"活跃用户"
}' http://127.0.0.1:4005/uba/api/meta/event/virtual
```

