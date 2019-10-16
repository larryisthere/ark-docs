---
description: 方舟4.3.4版本中新增API
---

# 转化漏斗

## 1. 接口地址

> 【POST】 /uba/api/funnels/analyze

## 2. 请求参数示例

接口请求参数，更多参数说明参照 查询API 中的 [通用参数](./#2-tong-yong-can-shu) 说明。

```java
{
    // 【必填】转换步骤，这里总共三步，表示 启动->浏览页面->关闭三个事件的转化情况
    "steps":[
        {
            "expression":"event.$startup",
            "filter":null
        },
        {
            "expression":"event.$pageview",
            "filter":null
        },
        {
            "expression":"event.$end",
            "filter":null
        }
    ],
    //【必填】用户分群
    "crowds":[
        "$ALL"
    ],
    //【必填】 时间范围-开始时间
    "fromDate":"2019-06-17",
    //【必填】 时间范围-结束时间
    "toDate":"2019-06-18",
    //抽样 这里表示全量
    "samplingFactor":1,
    //按照第几个步骤的事件属性查看 -1表示任意步骤事件 不按维度查看可不传
    "byStepIndex":-1,
    //按XX维度查看，只支持单个维度，不传表示不按维度细分
    "byField":{
        "expression":"event.$Anything.$platform",
        "buckets":null
    },
    //转化周日 和convertTimeUnit一起使用 这里表示 转化周期七天
    "convertTime":7,
    "convertTimeUnit":"DAY",
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
        "relation":"and"
    },
    //使用缓存 这里表示 使用 缓存
    "useCache":true,
    "limit":50,
}
```

{% hint style="info" %}
**特殊说明：**

1、参数在示例中未标注必填的表示选填。

2、请求参数可以通过方舟产品生成，具体参照 [接口请求参数获取](./#3-jie-kou-qing-qiu-can-shu-kuai-jie-huo-qu)。

3、细分维度只支持单维度查看，不支持分群对比。

**认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。
{% endhint %}

### 2.1 特殊参数说明

> **convertTime：**转化周期，只能输入正整数，和 **convertTimeUnit** 一起使用。
>
> **convertTimeUnit**：转化周期单位，目前支持 DAY，HOUR，MINUTE 三种时间单位。

## 3. 返回结果示例

```java
{
    //返回的日期集合，集合中的第一个值 $ALL 表示 总体的转化情况
    "dateList":[
        "$ALL",
        "2019/06/17 00:00:00",
        "2019/06/18 00:00:00"
    ],
    //按平台查看
    "byField":"event.$Anything.$platform",
    //返回的各个平台集合，集合中的第一个值-1表示 总体的转化情况
    "byValues":[
        "-1",
        "JS"
    ],
    //步骤
    "stepEvents":[
        "event.$startup",
        "event.$pageview",
        "event.$end"
    ],
    //rows，各个维度具体的值，结果行数和byValues一致，根据下标取对应值
    "rows":[
        {
            //总体转化
            "byValue":"-1",
            //每个步骤的用户转化数，结果行数和stepEvents一致，根据下标取对应值
            "stepsDetail":[
                {
                    "expression":"event.$startup",
                    //步骤下对应每日的用户数，结果行数和日期范围dateList一致，根据下标取对应值
                    "convertedUser":[
                        50, // 6.17-6.18总体启用事件的用户数为50个
                        33, // 6.17号触发启用事件的用户数为33个
                        24  // 6.18号触发启用事件的用户数为24个
                    ]
                },
                {
                    "expression":"event.$pageview",
                    "convertedUser":[
                        50,
                        33,
                        24
                    ]
                },
                {
                    "expression":"event.$end",
                    "convertedUser":[
                        0,
                        0,
                        0
                    ]
                }
            ],
            //步骤之间的转化中位数，结果行数比步骤数（stepEvents）少1
            "medianConvertedTime":[
                //第一步到第二步
                {
                    "stepsExpress":"$startup~$pageview",
                    //结果行数和日期范围dateList一致，根据下标取对应值
                    "median":[
                        13, // 总体第一步到第二步转化时间中位数为13s
                        13, // 6.17号第一步到第二步转化时间中位数为13s 
                        18  // 6.18号第一步到第二步转化时间中位数为18s
                    ]
                },
                //第二步到第三步
                {
                    "stepsExpress":"$pageview~$end",
                    "median":null
                }
            ]
        },
        {
            "byValue":"JS",
            "stepsDetail":[
                {
                    "expression":"event.$startup",
                    "convertedUser":[
                        50,
                        33,
                        24
                    ]
                },
                {
                    "expression":"event.$pageview",
                    "convertedUser":[
                        50,
                        33,
                        24
                    ]
                },
                {
                    "expression":"event.$end",
                    "convertedUser":[
                        0,
                        0,
                        0
                    ]
                }
            ],
            "medianConvertedTime":[
                {
                    "stepsExpress":"$startup~$pageview",
                    "median":[
                        13,
                        13,
                        18
                    ]
                },
                {
                    "stepsExpress":"$pageview~$end",
                    "median":null
                }
            ]
        }
    ],
    "reportUpdateTime":"2019-07-30 09:59:06"
}
```

### 3.1 相关计算备注

漏斗结果只返回了基础的用户数，相关转化率和完成率可根据各步骤用户数来完成计算。

> **转换率**：和上一步行为的用户数对比
>
> **完成率**：和第一步行为的用户数对比
>
> **转化时间中位数**：第N步到第N+1步转化时间中位数，单位为秒

## 4. 接口调用示例

```java
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X POST --data '{
    "steps":[
        {
            "expression":"event.$startup"
        },
        {
            "expression":"event.$pageview"
        },
        {
            "expression":"event.$end"
        }
    ],
    "crowds":[
        "$ALL"
    ],
    "fromDate":"2019-06-17",
    "toDate":"2019-06-18",
    "byStepIndex":-1,
    "byField":{
        "expression":"event.$Anything.$platform"
    },
    "convertTime":7,
    "convertTimeUnit":"DAY",
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
    "samplingFactor":1,
    "limit":50
}' http://127.0.0.1:4005/uba/api/funnels/analyze
```

