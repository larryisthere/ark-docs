---
description: 方舟5.3.3版本中新增维度表管理相关API。
---

# 维度表管理

## 1. 获取维度表字段‌

获取某个维度表下的所有字段。

### 1.1 接口地址

> 【GET】 /uba/api/project/dimensions/properties

### 1.2 请求参数示例

```java
//【必填】通过urlPath传参，table为维度表名称
table=?
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。

### 1.3 返回结果示例

```java
[
    {
        //字段名
        "id": "dim_product_id",
        //数据类型
        "dataType": "number",
        //字段显示名称
        "name": null,
        //字段说明
        "remark": null,
        //是否启用 1为启动 0为禁用
        "enable": 1,
        //是否为关联字段 1为关联字段 0为非关联字段
        "relation": 1
    },
    {
        "id": "name",
        "dataType": "string",
        "name": null,
        "remark": null,
        "enable": 1,
        //0表示非关联字段
        "relation": 0
    },
    {
        "id": "price",
        "dataType": "number",
        "name": null,
        "remark": null,
        "enable": 1,
        "relation": 0
    }
]
```

### 1.4 接口调用示例

```text
curl -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" http://127.0.0.1:4005/uba/api/project/dimensions/properties?table=dim_product
```

## 2. 更新维度表

更新维度表中的数据，维度表必须是已经创建成。接口支持新增列和往表中添加数据。

### 2.1 接口地址

> 【POST】 /uba/api/project/dimensions/update

### 2.2 请求参数示例

```java
{
    //【必填】维度表名称，维度表必须存在
    "table":"dim_product",
    //【必填】需要上报数据的维度表字段和字段对应的类型，map结构，key为字段名，value为字段类型
    "columns":{
        //dim_product_id为字段名 number为数据类型
        "dim_product_id":"number",
        "name":"string",
        "price":"number"
    },
    //【必填】维度属性值，可以多条
    "rows":[
        {
            //一行值为map结构，key为字段名，value为字段对应的值，dim_product_id为列名，1为dim_product_id字段的值
            "dim_product_id":1,
            "name":"华为meta8",
            "price":4999
        },
        {
            "dim_product_id":11,
            "name":"小米 K30",
            "price":2699
        }
    ]
}
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。
>
> **columns：**维度表中的字段名和字段数据类型。
>
> 参数为map结构，key为字段名，value为字段对应的数据类型。数据类型支持：
>
> * **string**：字符串；
> * **number**：数值，包含整数和小数点数据；
> * **boolean**：布尔，只包含 true/false；
> * **datetime**：日期，如yyyy-MM-dd HH:mm:ss.SSS 或yyyy-MM-dd HH:mm:ss 或yyyy-mm-dd；
> * **array&lt;string&gt;**：集合，字符串集合。
>
> **rows：**要插入到维度表中的数据，一行一条记录，每条记录用map存储，key为字段名，value为字段对应的值，关联字段值不能为空。rows中的字段在columns中必须定义。

{% hint style="info" %}
columns中必须包含维度表关联字段，并且关联字段在rows中的值不能为空； 

如果数据库中已有的维度表字段数据类型和columns传入的不一致，会提示参数错误； 

如果rows中指定的column在columns中没有指定字段的类型，会提示参数错误。
{% endhint %}

### 2.3 返回结果示例

```text
{
  "success":0
}
```

### 2.4 接口调用示例

```haskell
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X POST --data '{
    "table":"dim_product",
    "columns":{
        "dim_product_id":"number",
        "name":"string",
        "price":"number"
    },
    "rows":[
        {
            "dim_product_id":1,
            "name":"华为meta8",
            "price":4999
        },
        {
            "dim_product_id":11,
            "name":"小米 K30",
            "price":2699
        }
    ]
}' http://127.0.0.1:4005/uba/api/project/dimensions/update
```

## 3 创建维度表‌

创建维度表，维度表必须不存在，第一次创建维度表时需要指定表名、关联字段、表字段并且至少包含一条初始化数据。

### 3.1 接口地址

> 【POST】 /uba/api/project/dimensions

### 3.2 请求参数示例

```java
{
    //【必填】维度表名称，维度表必须存在
    "table":"dim_product",
    //维度表展示名称，不能超过200个字符
    "showName":"产品属性",
    //关联字段
    "relations":[
        "dim_product_id"
    ],
    //备注信息，不能超过200个字符
    "remark":"",
    //【必填】需要上报数据的维度表字段和字段对应的类型，map结构，key为字段名，value为字段类型
    "columns":{
        //dim_product_id为字段名 number为数据类型
        "dim_product_id":"number",
        "name":"string",
        "price":"number"
    },
    //【必填】维度属性值，可以多条
    "rows":[
        {
            //一行值为map结构，key为字段名，value为字段对应的值，dim_product_id为列名，1为dim_product_id字段的值
            "dim_product_id":1,
            "name":"华为meta8",
            "price":4999
        },
        {
            "dim_product_id":11,
            "name":"小米 K30",
            "price":2699
        }
    ]
}
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。
>
> **relations**：关联字段，可以指定多个，最多不能超过
>
> **columns：**维度表中的字段名和字段数据类型。
>
> 参数为map结构，key为字段名，value为字段对应的数据类型。数据类型支持：
>
> * **string**：字符串；
> * **number**：数值，包含整数和小数点数据；
> * **boolean**：布尔，只包含 true/false；
> * **datetime**：日期，如yyyy-MM-dd HH:mm:ss.SSS 或yyyy-MM-dd HH:mm:ss 或yyyy-mm-dd；
> * **array&lt;string&gt;**：集合，字符串集合。
>
> **rows：**要插入到维度表中的数据，一行一条记录，每条记录用map存储，key为字段名，value为字段对应的值，关联字段值不能为空。rows中的字段在columns中必须定义。

{% hint style="info" %}
columns中必须包含维度表关联字段，并且关联字段在rows中的值不能为空； 

如果数据库中已有的维度表字段数据类型和columns传入的不一致，会提示参数错误； 

如果rows中指定的column在columns中没有指定字段的类型，会提示参数错误。
{% endhint %}

### 3.3 返回结果示例

```text
{
  "success":0
}
```

### 3.4 接口调用示例

```haskell
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X POST --data '{
    "table":"dim_product",
    "showName":"商品维度表",
    "relations":[
        "dim_product_id"
    ],
    "columns":{
        "dim_product_id":"number",
        "name":"string",
        "price":"number"
    },
    "rows":[
        {
            "dim_product_id":1,
            "name":"华为meta8",
            "price":4999
        },
        {
            "productId":11,
            "name":"小米 K30",
            "price":2699
        }
    ]
}' http://127.0.0.1:4005/uba/api/project/dimensions
```

## 4. 清空维度表‌

清空维度表中的所有记录。清空后维度表和字段都还存在，页面还可继续使用，但是表中没有任何一条记录，数据不可恢复。

### 4.1 接口地址

> 【DELETE】 /uba/api/project/dimensions/clear

### 4.2 请求参数示例

```text
//【必填】通过urlPath传参，table为维度表名称
table=?
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。

### 4.3 返回结果示例

```text
{
  "success":0
}
```

### 4.4 接口调用示例

```text
curl -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X DELETE http://127.0.0.1:4005/uba/api/project/dimensions/clear?table=dim_product
```

## 5. 删除维度表‌

删除维度表。删除后维度表和维度表的记录都会被删除，不可恢复。删除后维度表将不存在，页面也不能再继续使用。

### 5.1 接口地址

> 【DELETE】 /uba/api/project/dimensions/drop

### 5.2 请求参数示例

```text
//【必填】通过urlPath传参，table为维度表名称
table=?
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。

### 5.3 返回结果示例

```text
{
  "success":0
}
```

### 5.4 接口调用示例

```text
curl -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X DELETE http://127.0.0.1:4005/uba/api/project/dimensions/drop?table=dim_product
```

