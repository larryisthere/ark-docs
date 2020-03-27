# 用户档案

## 1 单用户属性明细

根据用户ID获取用户属性数据。

### 1.1 接口地址

> 【POST】 /uba/api/cohort/users/{xwho}

{% hint style="info" %}
xwho 表示用户ID，取用户属性ID为xwho的值，传参方式：path variable，非request body
{% endhint %}

### 1.2 请求参数示例

```java
{
    //【选填】这里表示不支持，默认获取方舟系统所有可用的用户属性
    "properties":[]
}
```

> **properties**：指定需要的用户属性列，传入用户属性ID，可以通过方舟系统或者 [元数据管理](../api-manage-project/api-meta.md)-[用户属性](../api-manage-project/api-meta.md#1-huo-qu-yong-hu-shu-xing) 接口获取用户属性列表。不指定默认查询方舟系统【元数据管理 - 用户属性】中 可见 的所有用户属性。
>
> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。

### 1.3 返回结果示例

```java
{
        "$imei": null,
        "chuangke_topic": null,
        "$first_visit_language": "zh-cn",
        "$city": null,
        "$email": null,
        "$platform": "JS",
        "chuangke_region": null,
        "chuangke_createtime": null,
        "$country": null,
        "$first_visit_time": 1557403591272,
        "phone": null,
        "$province": null,
        "$signup_time": null,
        "$mac": null,
        "company_code": null,
        "xwho": "JSd650856937040a530ff54fdaab4e56d7d650",
        "email": null
    }
```

### 1.4 接口调用示例

```java
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X POST --data '{
    "properties":[]
}' http://127.0.0.1:4005/uba/api/cohort/users/JSd650856937040a530ff54fdaab4e56d7d650
```

