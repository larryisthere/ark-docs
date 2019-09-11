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

> **默认参数**：每个接口都必传token和appKey两个参数，详情见 [默认参数](./#11-mo-ren-can-shu)。

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
        "visible":1
    },
    {
        "id":"$xiaomi",
        "name":"小米ID",
        "enable":1,
        "visible":0
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

无。

> **默认参数**：每个接口都必传token和appKey两个参数，详情见 [默认参数](./#11-mo-ren-can-shu)。

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
        "remark":"APP启动 / 打开网站"
    },
    {
    "id":"$end",
    "name":"关闭",
    "enable":1,
    "remark":"APP关闭 / 关闭网页"
}
]
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

> **默认参数**：每个接口都必传token和appKey两个参数，详情见 [默认参数](./#11-mo-ren-can-shu)。

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
        //，这里表示事件自定义属性
        "global":0
    },
    {
        "id":"$platform",
        "name":"平台",
        //属性可用
        "enable":1,
        //通用属性
        "global":1
    }
]
```

> **global**： 0为事件自定义属性，1为通用属性。

### 3.4 接口调用示例

```java
curl -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" http://127.0.0.1:4005/uba/api/meta/eventProperties?eventId=%24startup
```

