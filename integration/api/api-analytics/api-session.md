---
description: 方舟5.1.0068版本中新增API
---

# Session分析

## 1. 接口地址

> 【POST】 /uba/api/sessions/analyze

## 2. 请求参数示例

接口请求参数，更多参数说明参照 查询API 中的 [通用参数](./#2-tong-yong-can-shu) 说明。

```java
{
    // 【必填】指标 可以输入多个
    "measures":[
        {
            //【必填】Session总体
            "expression":"event.$Anything$Session",
            //【必填】总次数
            "aggregator":"SESSION_TOTAL_COUNT"
        }
    ],
    // 针对于所有指标的过滤条件，非必填
    "filter":{
        "conditions":[
            {
                //【必填】属性表达式
                "expression":"user.xwho",
                //【必填】条件操作符号
                "function":"NOT_NULL",
                "params":[

                ]
            },
            {
                "expression":"session.$Anything.$platform",
                "function":"EQ",
                "params":[
                    "iOS",
                    "Android",
                    "JS"
                ]
            }
        ],
        "relation":"AND"
    },
    //【必填】用户分群 只支持查询单分群
    "crowds":[
        "$ALL"
    ],
    //【必填】 时间范围-开始时间
    "fromDate":"2020-07-03",
    //【必填】 时间范围-结束时间
    "toDate":"2020-07-09",
    //查询结果是否使用缓存 true为使用缓存 false为重新查询
    "useCache":true,
    //抽样 这里表示全量
    "samplingFactor":1,
    //查看时间粒度 这里表示按日查看
    "unit":"DAY",
    //按XX维度查看，可以输入多个
    "byFields":[
        {
            "expression":"session.$Anything.$platform"
        }
    ],
    //【必填】是否使用默认规则 true为使用默认规则 false为指定规则
    "defaultRule":false,
    //session规则 如果defaultRule=false，则需要指定session切割规则
    "sessionRule":{
        //【必填】参与session计算的事件
        "events":[
            "$pageview"
        ],
        //【必填】超时切割时间，单位为分钟
        "timeout":30,
        //【必填】零点是否自动切割
        "autoSplit":false,
        //【必填】UID跨屏切割
        "useUid":false,
        //指定切割事件，当出现X事件时就算一次新session，不必填
        "splitEvent": ""
    }
}
```

{% hint style="info" %}
**特殊说明：**

1、参数在示例中未标注必填的表示选填。

2、请求参数可以通过方舟产品生成，使用方法参照 [接口请求参数获取](./#3-jie-kou-qing-qiu-can-shu-kuai-jie-huo-qu)。

3、接口不支持日期对比和分群对比。

4、查询指标中如果包含时长的，如访问时长，停留时长等，返回结果的单位为毫秒。

**认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。
{% endhint %}

### 2.1 聚合表达式说明

> **aggregator：**聚合表达式，根据指标表达式是事件和属性而不同。
>
> * Session指标表达式： 
>
>   * **SESSION\_TOTAL\_COUNT** ：访问次数。
>   * **SESSION\_AVG\_COUNT**：人均访问次数。
>   * **SESSION\_DURATION\_SUM**：总访问时长。
>   * **SESSION\_PER\_DEEPNESS\_SUM**：单次访问深度。
>   * **SESSION\_BOUNCE\_COUNT**：跳出次数。
>   * **SESSION\_BOUNCE\_RATE**：跳出率。
>   * **SESSION\_DROPOUT\_COUNT**：退出次数。
>   * **SESSION\_DROPOUT\_RATE**：退出率。
>   * **SESSION\_AVG\_DURATION**：人均访问时长。
>   * **SESSION\_AVG\_DURATION**：人均访问时长。
>   * **SESSION\_PAGE\_DURATION**：页面停留总时长。
>   * **SESSION\_PAGE\_AVG\_DURATION**：平均页面停留时长。
>
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
        "event.$Anything.SESSION_TOTAL_COUNT",
        "event.$Anything.SESSION_DURATION_SUM"
    ],
    //查看维度，和输入一致
    "byFields": [
        "session.$Anything.$browser",
        "session.$Anything.$platform"
    ],
    //时间范围
    "series": [
        "2020-07-03",
        "2020-07-04",
        "2020-07-05",
        "2020-07-06",
        "2020-07-07",
        "2020-07-08",
        "2020-07-09"
    ],
    //返回结果
    "rows": [
        {
            //日期范围内合计，根据数组下标和 measures[]指标对应
            "sum": [
                11
            ],
            //每日的指标结果，根据数组下标和measures[]指标对应 
            "values": [
                //每日的具体指标数，根据下标和时间series[]下标对应
                [
                    0,
                    0,
                    0,
                    0,
                    4,
                    0,
                    7
                ]
            ],
            //具体维度分组结果
            "byValue": [
                "JS"
            ]
        },
        {
            "sum": [
                2
            ],
            "values": [
                [
                    0,
                    0,
                    0,
                    0,
                    2,
                    0,
                    0
                ]
            ],
            "byValue": [
                "iOS"
            ]
        }
    ],
    //查询时间
    "reportUpdateTime": "2020-07-10 11:03:26"
}
```

## 4. 接口调用示例

```java
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X POST --data '{
    "measures":[
        {
            "expression":"event.$Anything$Session",
            "aggregator":"SESSION_TOTAL_COUNT"
        }
    ],
    "filter":{
        "conditions":[
            {
                "expression":"session.$Anything.$platform",
                "function":"EQ",
                "params":[
                    "iOS",
                    "Android",
                    "JS"
                ]
            }
        ],
        "relation":"AND"
    },
    "crowds":[
        "$ALL"
    ],
    "fromDate":"2020-07-06",
    "toDate":"2020-07-12",
    "useCache":true,
    "samplingFactor":1,
    "unit":"DAY",
    "byFields":[
        {
            "expression":"session.$Anything.$platform"
        }
    ],
    "defaultRule":true
}' http://127.0.0.1:4005/ark/uba/api/sessions/analyze
```



