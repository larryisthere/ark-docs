---
description: 方舟5.1.0068版本中新增API
---

# 渠道分析

## 1. 接口地址

> 【POST】 /uba/api/channels/analyze

## 2. 请求参数示例

接口请求参数，更多参数说明参照 查询API 中的 [通用参数](./#2-tong-yong-can-shu) 说明。

```java
{
    //【必填】 时间范围-开始时间
    "fromDate":"2020-08-18",
    //【必填】 时间范围-结束时间
    "toDate":"2020-08-24",
    // 【必填】基础指标 可以输入多个
    "measures":[
        {
            "aggregator":"USER_COUNT"
        },
        {
            "aggregator":"SESSION_TOTAL_COUNT"
        }
    ],
    // 【必填】针对于所有指标的过滤条件 系统生成的默认条件 根据平台和维度查看自动生成 一般不需要修改 $platform为必填参数
    "defaultFilter":{
        "conditions":[
            {
                "expression":"event.$Anything.$platform",
                "function":"EQ",
                "params":[
                    "JS"
                ]
            }
        ],
        "relation":"AND"
    },
    // 针对于所有指标的过滤条件，非必填，自己选择
    "filter":{
        "conditions":[
            {
                "expression":"session.$Anything.$browser",
                "function":"EQ",
                "params":[
                    "Chrome"
                ]
            }
        ],
        "relation":"AND"
    },
    //查询结果是否使用缓存 true为使用缓存 false为重新查询
    "useCache":true,
    //抽样 这里表示全量
    "samplingFactor":1,
    //查看时间粒度 这里表示按日查看
    "unit":"DAY",
    //返回条数
    "limit":50,
    //【必填】用户分群 只支持查询单分群
    "crowds":[
        "$ALL"
    ],
    //渠道转化目标
    "convertMeasures":[
        {
            //【必填】
            "expression":"event.login",
            //【聚合表达式】
            "aggregator":"TRIGGER_USER_COUNT"
        }
    ],
    //按XX维度查看，可以输入多个
    "byFields":[
        {
            "expression":"session.$Anything.$traffic_source_type"
        }
    ]
}
```

{% hint style="info" %}
**特殊说明：**

1、参数在示例中未标注必填的表示选填。

2、请求参数可以通过方舟产品生成，使用方法参照 [接口请求参数获取](./#3-jie-kou-qing-qiu-can-shu-kuai-jie-huo-qu)。

3、接口不支持日期对比和分群对比。

4、查询指标中如果包含时长的，如访问时长，停留时长等，返回结果的单位为**毫秒**。

**认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。
{% endhint %}

### 2.1 基础指标聚合表达式说明

> * **aggregator** 指标表达式：
>   * **USER\_COUNT** ：访问用户数。
>   * **SESSION\_TOTAL\_COUNT**：访问次数。
>   * **PAGE\_VIEW**：浏览量（PV）。
>   * **SESSION\_AVG\_COUNT**：人均访问次数。
>   * **PAGE\_VIEW\_AVG**：人均页面浏览量。
>   * **SESSION\_AVG\_DURATION**：人均访问时长。
>   * **SESSION\_AVG\_PAGE\_NUMS**：单次访问页面浏览量。
>   * **SESSION\_PER\_DURATION**：单次访问时长。
>   * **SESSION\_AVG\_EVENT\_NUMS**：单次访问事件数。
>   * **SESSION\_BOUNCE\_RATE**：跳出率。
>   * **DAY1\_RETENTION\_USER\_COUNT**：次日留存率。
>   * **DAY7\_RETENTION\_USER\_COUNT**：7日留存率。
>   * **DAY14\_RETENTION\_USER\_COUNT**：14日留存率。
>   * **DAY30\_RETENTION\_USER\_COUNT**：30日留存率。

### 2.2 转化目标聚合表达式说明

> **aggregator：**聚合表达式，根据指标表达式是事件和属性而不同。
>
> * 指标表达式：
>   * **TRIGGER\_USER\_COUNT** ：转化用户数。
>   * **TRIGGER\_USER\_PERCENT**：转化用户数占比。
>   * **SESSION\_TOTAL\_COUNT**：转化次数。
>   * **SESSION\_TOTAL\_PERCENT**：转化次数占比。
> * 属性表达式支持的聚合操作符如下：
>   * **REMOVE\_DUMPLICATE** ：表示去重数，支持所有数据类型。
>   * **SUM**：总和，选定属性的属性值求和，例如加入购物车的商品金额之和，只支持数值类型。
>   * **AVG**：均值，选定属性的属性值算数平均数，例如加入购物车的商品金额均值，只支持数值类型。
>   * **MAX**： 最大值，选定属性的属性值最大值，例如加入购物车的商品最大金额，只支持数值类型。
>   * **MIN** ：最小值，选定属性的属性值最小值，例如加入购物车的商品最小金额，只支持数值类型。
>   * **AVG\_PER**： 人均值，选定属性的属性值人均值，例如人均加入购物车的商品金额，只支持数值类型。

## 3. 返回结果示例

```java
{
    //查询的指标，和输入一致
    "measures": [
        "event.$Anything.USER_COUNT",
        "event.$Anything.SESSION_TOTAL_COUNT",
        "event.login.TRIGGER_USER_COUNT"
    ],
    //查看维度，和输入一致
    "byFields": [
        "session.$Anything.$traffic_source_type"
    ],
    //时间范围
    "series": [
        "2020/08/18 00:00:00",
        "2020/08/19 00:00:00",
        "2020/08/20 00:00:00",
        "2020/08/21 00:00:00",
        "2020/08/22 00:00:00",
        "2020/08/23 00:00:00",
        "2020/08/24 00:00:00"
    ],
    //返回结果,数组的大小就是按维度查看的结果
    "rows": [
        {
            //日期范围内合计，根据数组下标和 measures[]指标对应
            "sum": [
                176,
                434,
                0
            ],
            //每日的指标结果，根据数组下标和measures[]指标对应 
            "values": [
                //每日的具体指标数，根据下标和时间series[]下标对应
                [
                    9,
                    18,
                    23,
                    9,
                    21,
                    11,
                    85
                ],
                [
                    27,
                    54,
                    69,
                    27,
                    63,
                    33,
                    161
                ],
                [
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0
                ]
            ],
            //具体维度分组结果
            "byValue": [
                "search"
            ]
        },
        {
            "sum": [
                1,
                8,
                0
            ],
            "values": [
                [
                    0,
                    0,
                    1,
                    1,
                    0,
                    0,
                    1
                ],
                [
                    0,
                    0,
                    4,
                    3,
                    0,
                    0,
                    1
                ],
                [
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0
                ]
            ],
            "byValue": [
                "direct"
            ]
        },
        {
            "sum": [
                1,
                1,
                0
            ],
            "values": [
                [
                    0,
                    0,
                    0,
                    1,
                    0,
                    0,
                    0
                ],
                [
                    0,
                    0,
                    0,
                    1,
                    0,
                    0,
                    0
                ],
                [
                    0,
                    0,
                    0,
                    0,
                    0,
                    0,
                    0
                ]
            ],
            "byValue": [
                "referral"
            ]
        }
    ],
    //查询时间
    "reportUpdateTime": "2020-08-25 11:03:26"
}
```

## 4. 接口调用示例

```java
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X POST --data '{
    "fromDate":"2020-08-18",
    "unit":"DAY",
    "measures":[
        {
            "aggregator":"USER_COUNT"
        },
        {
            "aggregator":"SESSION_TOTAL_COUNT"
        }
    ],
    "useCache":true,
    "samplingFactor":1,
    "toDate":"2020-08-24",
    "defaultFilter":{
        "conditions":[
            {
                "expression":"session.$Anything.$platform",
                "function":"EQ",
                "params":[
                    "JS"
                ]
            }
        ],
        "relation":"AND"
    },
    "limit":50,
    "crowds":[
        "$ALL"
    ],
    "convertMeasures":[
        {
            "expression":"event.login",
            "aggregator":"TRIGGER_USER_COUNT"
        }
    ],
    "byFields":[
        {
            "expression":"session.$Anything.$traffic_source_type"
        }
    ]
}' http://127.0.0.1:4005/ark/uba/api/channels/analyze
```



