---
description: 方舟4.3.4版本中新增API
---

# 事件分析

## 1. 接口地址

> 【POST】 /uba/api/events/analyze

## 2. 请求参数示例

接口请求参数，更多参数说明参照 查询API 中的 [通用参数](./#2-tong-yong-can-shu) 说明。

```java
{
    // 【必填】指标 可以输入多个
    "measures":[
        //这里的指标是 平台等于JS的任意事件的触发用户数
        {
            //【必填】事件/事件属性表达式
            "expression":"event.$Anything",
            // 【必填】聚合操作符
            "aggregator":"TRIGGER_USER_COUNT",
            // 针对于当前指标选择事件的过滤条件，非必填
            "filter":{
                "conditions":[
                    {
                        "expression":"event.$Anything.$platform",
                        "function":"EQ",
                        "params":[
                            "JS"
                        ]
                    }
                ],
                "relation":"and"
            }
        }
    ],
    // 针对于所有指标的过滤条件，非必填
    "filter":{
        "conditions":[
            {
                //【必填】属性表达式
                "expression":"event.$Anything.$platform",
                //【必填】条件操作符号
                "function":"EQ",
                "params":[
                    "JS",
                    "iOS",
                    "Android"
                ]
            }
        ],
        "relation":"AND"
    },
    //【必填】用户分群 可以输入多个
    "crowds":[
        "$ALL"
    ],
    //【必填】 时间范围-开始时间
    "fromDate":"2019-06-18",
    //【必填】 时间范围-结束时间
    "toDate":"2019-06-20",
    //抽样 这里表示全量
    "samplingFactor":1,
    //使用缓存 这里表示是
    "useCache":true,
    //查看时间粒度 这里表示按日查看
    "unit":"DAY",
    //按XX维度查看，可以输入多个
    "byFields":[
        {
            "expression":"event.$Anything.$screen_width",
            //这里表示结果按照 无限小~600，600~800，800~无限大三个区间查看
            "buckets":[
                600,
                800
            ]
        }
    ]
}
```

{% hint style="info" %}
**特殊说明：**

1、参数在示例中未标注必填的表示选填。

2、请求参数可以通过方舟产品生成，使用方法参照 [接口请求参数获取](./#4-jie-kou-qing-qiu-can-shu-kuai-jie-huo-qu)。

3、接口不支持日期对比。

**默认参数**：每个接口都必传token和appKey两个参数，详情见 [默认参数](./#11-mo-ren-can-shu)。
{% endhint %}

### 2.1 聚合表达式说明

> **aggregator：**聚合表达式，根据指标表达式是事件和属性而不同。
>
> * 事件表达式支持的聚合操作符如下： 
>
>   * **TRIGGER\_USER\_COUNT** ：触发用户数，触发选定事件的去重用户数，例如加入购物车的触发用户数。 
>   * **TOTAL\_COUNT**： 触发次数，触发选定事件的总次数，例如加入购物车的触发次数。 
>   * **AVG\_COUNT**：人均触发次数，平均每个用户触发选定事件的次数，例如加入购物车的人均触发次数。 
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
    "measures":[
        "event.$Anything.unique"
    ],
    //查看维度，和输入一致
    "byFields":[
        "event.$Anything.$screen_width"
    ],
    //时间范围
    "series":[
        "2019-06-18",
        "2019-06-19",
        "2019-06-20"
    ],
    //返回结果
    "rows":[
        {
           //总和
            "sum":[
                50
            ],
            //用户数，根据数组下标和指标（measures）/维度（byFields） 和 时间（series）对应
            "values":[
                [
                    22,
                    18,
                    20
                ]
            ],
            //维度结果
            "byValue":[
                "800~+∞"
            ]
        },
        {
            "sum":[
                10
            ],
            "values":[
                [
                    9,
                    2,
                    0
                ]
            ],
            "byValue":[
                "-∞~600"
            ]
        },
        {
            "sum":[
                5
            ],
            "values":[
                [
                    4,
                    1,
                    0
                ]
            ],
            "byValue":[
                "未知"
            ]
        }
    ],
    //返回的结果行数
    "numRows":3,
    //查询时间
    "reportUpdateTime":"2019-07-01 15:55:43"
}
```

## 4. 接口调用示例

```java
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X POST --data '{
    "measures":[
        {
            "filter":{
                "conditions":[
                    {
                        "expression":"event.$Anything.$platform",
                        "function":"EQ",
                        "params":[
                            "JS"
                        ]
                    }
                ],
                "relation":"and"
            },
            "expression":"event.$Anything",
            "aggregator":"TRIGGER_USER_COUNT"
        }
    ],
    "filter":{
        "conditions":[
            {
                "expression":"event.$Anything.$platform",
                "function":"EQ",
                "params":[
                    "JS",
                    "iOS",
                    "Android"
                ]
            }
        ],
        "relation":"AND"
    },
    "byFields":[
        {
            "expression":"event.$Anything.$screen_width",
            "buckets":[
                600,
                800
            ]
        }
    ],
    "crowds":[
        "$ALL"
    ],
    "fromDate":"2019-06-18",
    "toDate":"2019-06-20",
    "samplingFactor":1,
    "useCache":true,
    "unit":"DAY"
}' http://127.0.0.1:4005/uba/api/events/analyze
```

