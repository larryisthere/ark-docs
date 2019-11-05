---
description: 方舟4.5.1版本中新增API
---

# 属性分析

## **1. 接口地址**

> 【POST】 /uba/api/userAttributes/analyze

## **2. 请求参数示例**

接口请求参数，更多参数说明参照 查询API 中的 [通用参数](./#2-tong-yong-can-shu) 说明。

```java
{
    // 【必填】查询指标，含用户数和某用户属性的去重数等
    "measure":{
        //在获取某个属性的聚合结果时候才需要填
        "expression":"",
        //【必填】聚合操作符 这里表示用户数
        "aggregator":"USER_COUNT"
    },
    //【必填】用户分群
    "crowds":[
        "$ALL"
    ],
    //使用缓存 这里表示是
    "useCache":true,
    "limit":50,
    // 针对于指标的过滤条件，非必填，只能选择用户属性
    "filter":{
        "conditions":[
            {
                "expression":"user.xwho",
                "function":"NOT_NULL",
                "params":[
​
                ]
            }
        ],
        "relation":"AND"
    },
    //按XX维度查看，可以输入多个，可以为空，只能选择用户属性
    "byFields":[
        {
            "expression":"user.$city"
        }
    ]
}
```

**特殊说明：**

1、参数在示例中未标注必填的表示选填。

2、请求参数可以通过方舟产品生成，具体参照 [接口请求参数获取](./#3-jie-kou-qing-qiu-can-shu-kuai-jie-huo-qu)。

3、接口不支持日期对比和分群对比。

4、属性分析中的所有表达式都为用户属性表达式，只支持用户属性的查看、过滤。

**认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。

### **2.1 聚合表达式说明**

> **aggregator：**聚合表达式，支持查看总用户数或某个属性的聚合结果。
>
> * 不需要表达式查看总体的聚合操作符如下：
>   * **USER\_COUNT** ：用户数。
> * 用户属性表达式支持的聚合操作符如下：
>   * **REMOVE\_DUMPLICATE** ：表示去重数，支持所有数据类型。
>   * **SUM**：总和，选定属性的属性值求和，例如加入购物车的商品金额之和，只支持数值类型。
>   * **AVG**：均值，选定属性的属性值算数平均数，例如加入购物车的商品金额均值，只支持数值类型。
>   * **MAX**： 最大值，选定属性的属性值最大值，例如加入购物车的商品最大金额，只支持数值类型。
>   * **MIN** ：最小值，选定属性的属性值最小值，例如加入购物车的商品最小金额，只支持数值类型。
>   * **AVG\_PER**： 人均值，选定属性的属性值人均值，例如人均加入购物车的商品金额，只支持数值类型。

## **3. 返回结果示例**

```java
//一、无细分维度（查看用户数）
{
    "rows": [
        {
            //指标查询结果
            "value": 88399,
            "byValue": []
        }
    ],
    "measure": "USER_COUNT",
    "byFields": [],
    "reportUpdateTime": "2019-10-09 11:19:40"
}
​
//二、有细分维度
{
    //查询结果
    "rows": [
        {
            //维度对应的指标值
            "value": 27206,
            //维度分类
            "byValue": [
                "2018-05-07 00:00:00"
            ]
        },
        {
            "value": 7683,
            "byValue": [
                "2018-05-08 00:00:00"
            ]
        },
        {
            "value": 5859,
            "byValue": [
                "2018-05-10 00:00:00"
            ]
        },
        {
            "value": 5277,
            "byValue": [
                "2018-05-09 00:00:00"
            ]
        },
        {
            "value": 3548,
            "byValue": [
                "2018-05-11 00:00:00"
            ]
        }
    ],
    //查询的指标
    "measure": "USER_COUNT",
    //按照首次访问时间查看
    "byFields": [
        "user.$first_visit_time"
    ],
    "reportUpdateTime": "2019-10-09 11:24:34"
}
```

## **4. 接口调用示例**

```java
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X POST --data '{
    "measure":{
        "aggregator":"USER_COUNT"
    },
    "useCache":true,
    "limit":50,
    "crowds":[
        "$ALL"
    ],
    "byFields":[
​
    ]
}' http://127.0.0.1:4005/uba/api/userAttributes/analyze
```

