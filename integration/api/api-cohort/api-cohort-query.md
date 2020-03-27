---
description: 方舟4.3.4版本中新增API
---

# 分群查询

## 1. 获取用户分群下的用户明细

获取某个分群下的用户明细

### 1.1 接口地址

> 【POST】  /uba/api/cohort/users

### 1.2 请求参数示例

```java
{
    // 【必填】分群code
    "cohortCode":"arkfq_3",
    //需要获取的用户条数
    "limit":2,
    //【选填】指定需要查询的用户属性列，传用户属性ID
    "properties":["xwho", "distinct_id", "$imei", "$first_visit_language", "$signup_time"],
  	//【4.6中新增】【选填】当前页，从1开始，不需要分页不用传值
  	"page":1
    //【4.6中新增】【选填】每页大小，和page配合使用，值不能大于limit
  	"pageSize"：1000，
}
```

> **limit**：获取用户分群下的用户条数，需要传正整数，不传默认为1000。limit为需要获取的总条数，如果有分页信息会用来控制总页数。
>
> **properties**：指定需要的用户属性列，传入用户属性ID，可以通过方舟系统或者 [元数据管理](../api-manage-project/api-meta.md)-[用户属性](../api-manage-project/api-meta.md#1-huo-qu-yong-hu-shu-xing) 接口获取用户属性列表。不指定默认查询方舟系统【元数据管理 - 用户属性】中 可见 的所有用户属性。
>
> **page**：为空，则不需要分页信息，接口支持返回limit条数；不为空，接口返回pageSize条数。分页从1开始。
>
> **pageSize**：page有值时，不能为空，对应每页条数。pageSize不能大于limit。
>
> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。

### 1.3 返回结果示例

```java
{
    //分群下总用户条数
    "count":46,
    //【4.6中新增】当前页
    "page": 1,
    //结果返回的用户条数
    "size":2,
    //用户详情
    "users":[
        {
            "$imei":null,
            "$first_visit_language":"zh-cn",
            "distinct_id":4255875062385414000,
            "$signup_time":null,
            "xwho":"JSd650856937040a530ff54fdaab4e56d7d650"
        },
        {
            "$imei":null,
            "$first_visit_language":"zh-cn",
            "distinct_id":-4635244755532264000,
            "$signup_time":null,
            "xwho":"JS1c6d11e3e67bf0dc02030d3fd393310b1c6d"
        }
    ]
}
```

### 1.4 接口调用示例

```java
 curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X POST --data '{
    "cohortCode":"arkfq_3",
    "limit":2,
    "properties":["xwho", "distinct_id", "$imei", "$first_visit_language", "$signup_time"]
}' http://127.0.0.1:4005/uba/api/cohort/users
```

