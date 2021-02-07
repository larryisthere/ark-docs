---
description: 方舟5.2版本中新增分布分析API
---

# 分布分析

## **1. 接口地址**

> 【POST】 /uba/api/distributions/analyze

## **2. 请求参数示例**

接口请求参数，更多参数说明参照 查询API 中的 [通用参数](./#2-tong-yong-can-shu) 说明。

```java
{
    //【必填】 时间范围-开始时间
    "fromDate":"2019-06-17",
    //【必填】 时间范围-结束时间
    "toDate":"2019-06-18",
    // 【必填】基础指标 可以输入多个
    "measure":{
        //【必填】事件/事件属性表达式
        "expression":"event.$Anything",
        //【必填】聚合操作符
        "aggregator":"TOTAL_COUNT"
    },
    // 针对于所有步骤事件的过滤条件，非必填
    "filter":{
        "conditions":[
            {
                "expression":"event.$Anything.$platform",
                "function":"EQ",
                "params":[
                    "JS",
                    "Android"
                ]
            }
        ],
        "relation":"AND"
    },
    //【必填】用户分群
    "crowds":[
        "$ALL"
    ],
    //抽样 这里表示全量
    "samplingFactor":1,
    //【选填】 unit分为按照全部、日（DAY）、周（WEEK）、月（MONTH）查看，为空时表示按照全部查看
    "unit":"",
    //按XX维度查看，只支持单个维度，不传表示不按维度细分
    "byField":{
        "expression":"event.$Anything.$platform"
    },
    //是否使用缓存
    "useCache":true,
    "limit":50
}
```

{% hint style="info" %}
**特殊说明：**

1、参数在示例中未标注必填的表示选填。

2、请求参数可以通过方舟产品生成，具体参照 [接口请求参数获取](./#3-jie-kou-qing-qiu-can-shu-kuai-jie-huo-qu)。

3、细分维度只支持单维度查看，不支持分群对比。

**认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。
{% endhint %}

### **2.1 聚合表达式说明**

> **aggregator：**聚合表达式，根据指标表达式是事件和属性而不同。
>
> * 事件表达式支持的聚合操作符如下：
>   * **TOTAL\_COUNT**： 次数，行为触发次数。
>   * **DISTRIBUTION\_TIME**：时间，分布时间点。
> * 属性表达式支持的聚合操作符如下：
>   * **REMOVE\_DUMPLICATE** ：表示去重数，支持所有数据类型。
>   * **SUM**：总和，选定属性的属性值求和，例如加入购物车的商品金额之和，只支持数值类型。
>   * **AVG**：均值，选定属性的属性值算数平均数，例如加入购物车的商品金额均值，只支持数值类型。
>   * **MAX**： 最大值，选定属性的属性值最大值，例如加入购物车的商品最大金额，只支持数值类型。
>   * **MIN** ：最小值，选定属性的属性值最小值，例如加入购物车的商品最小金额，只支持数值类型。
>   * **AVG\_PER**： 人均值，选定属性的属性值人均值，例如人均加入购物车的商品金额，只支持数值类型。‌

## **3. 返回结果示例**

```text
{
    //查询的指标，和输入一致
    "measure": "event.$startup.$browser_version.REMOVE_DUMPLICATE",
    //查看维度，和输入一致
    "byField": null,
    //区间值
    "series": [
        "[1,2)",
        "[2,5)"
    ],
    "rows": [
        {
            //对应byvalue的总用户数
            "sum": 3,
            //用户数，根据下标和区间值（series）对应
            "values": [
                2,
                1
            ],
            //维度结果（按照维度查看就是对应的维度就结果，按照日查看就是日期）如果等于all说明是按全部查看
            "byValue": "2020/09/02 00:00:00"
        },
        {
            "sum": 2,
            "values": [
                1,
                1
            ],
            "byValue": "2020/08/28 00:00:00"
        },
        {
            "sum": 2,
            "values": [
                1,
                1
            ],
            "byValue": "2020/08/31 00:00:00"
        },
        {
            "sum": 1,
            "values": [
                0,
                1
            ],
            "byValue": "2020/08/27 00:00:00"
        },
        {
            "sum": 1,
            "values": [
                0,
                1
            ],
            "byValue": "2020/09/01 00:00:00"
        }
    ],
    //查询时间
    "reportUpdateTime": "2020-09-03 11:49:26"
}
```

## **4. 接口调用示例**

```text
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X POST --data '{
    "measure": {
        "expression": "event.$startup.$browser_version",
        "aggregator": "REMOVE_DUMPLICATE"
    },
    "toDate": "2020-09-02",
    "fromDate": "2020-08-27",
    "unit": "DAY",
    "useCache": true,
    "samplingFactor": 1,
    "limit": 50,
    "crowds": [
        "$ALL"
    ]
}' http://127.0.0.1:4005/uba/api/distributions/analyze
```

