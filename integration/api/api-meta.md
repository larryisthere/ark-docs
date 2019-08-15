# 元数据管理

## 1. 获取用户属性

获取用户上报成功并且在计划中的用户属性，含系统预置但页面上隐藏的用户属性。

### 1.1 接口地址

> 【GET/POST】 /uba/api/meta/userProperties

### 1.2 请求参数说明

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



