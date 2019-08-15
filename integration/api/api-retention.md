# 留存分析

## 1. 接口地址

> 【POST】 /uba/api/retentions/analyze

## 2. 请求参数示例

接口请求参数，更多参数说明参照 查询API 中的 [通用参数](./#2-tong-yong-can-shu) 说明。

### 2.1 按属性细分

```java
{
    //【必填】初始行为
    "firstEvent": {
		"expression": "event.$startup"
    },
    //【必填】回访行为
	"secondEvent": {
		"expression": "event.$pageview"
    },
    //按XX维度查看，只支持单个维度，不传表示不按维度细分，这里表示按照平台查看
	"byField": {
		"expression": "event.$startup.$platform"
    },
    //按初始/回访行为的属性查看，1表示初始行为 2表示回访行为 -1表示用户属性或无维度
    "byFieldIndex": 1,
    //【必填】用户分群 可以输入多个，这里表示所有用户
    "crowds": ["$ALL"],
    //【必填】 时间范围-开始时间
    "fromDate": "2019-07-17",
    //【必填】 时间范围-结束时间
    "toDate": "2019-07-19",
    //【必填】留存范围，和unit匹配使用，这里表示查询7日留存
	"duration": 7,
	//【必填】
    "unit": "DAY",
    //抽样 这里表示全量
    "samplingFactor": 1,
    //最多返回多少维度记录
	"limit": 50,
    //使用缓存 这里表示 使用 缓存
    "useCache": true
}
```

### 2.2 不细分同时显示其他指标

```java
{
    "firstEvent": {
		"expression": "event.$startup"
	},
	"secondEvent": {
		"expression": "event.$pageview"
    },
    //额外查询的指标
    "measures": {
		"expression": "event.$Anything",
		"aggregator": "TRIGGER_USER_COUNT"
	},
    "crowds": ["$ALL"],
    "fromDate": "2019-07-17",
	"toDate": "2019-07-19",
    "byField": null,
    "byFieldIndex": -1
	"duration": 7,
	"unit": "DAY",
    "samplingFactor": 1,
    "useCache": true,
	"limit": 50
}
```

{% hint style="info" %}
**特殊说明：**

1、参数在示例中未标注必填的表示选填。

2、请求参数可以通过方舟产品生成，具体参照 [接口请求参数获取](./#4-jie-kou-qing-qiu-can-shu-kuai-jie-huo-qu)。

3、细分维度只支持单维度查看，并且按维度查看时不能同时查看其他指标。

4、measures 指标查询，aggregator参考事件分析 [聚合表达式说明](api-event.md#21-ju-he-biao-da-shi-shuo-ming)。

**默认参数**：每个接口都必传token和appKey两个参数，详情见 [默认参数](./#11-mo-ren-can-shu)。
{% endhint %}

### 2.3 特殊参数说明

> **duration：**需要查询的留存范围，如支持次日、七日、14日、30日，和 unit 联合使用。
>
> **unit**：查询范围周期单位，目前支持DAY，WEEK，MONTH三种时间单位。

## 3. 返回结果示例

### 3.1 按属性细分

```java
{
    //细分维度 平台
    "byField": "event.$startup.$platform",
    //各个平台的结果
    "rows": [
        {
            //平台值
            "byValue": "JS",
            //目标用户数（当日）
            "totalPeople": 127,
            //留存人数 按照索引 依次为当日、第一日、第二日...第七日
            "retentionPeoples": [
                127,
                10,
                7,
                2,
                4,
                6,
                5,
                2
            ],
            "measures": null
        },
        {
            "byValue": "Android",
            "totalPeople": 12,
            "retentionPeoples": [
                12,
                0,
                1,
                1,
                0,
                0,
                0,
                0
            ],
            "measures": null
        }
    ],
    "reportUpdateTime": "2019-07-24 11:44:31"
}
```

### 3.2 不细分同时显示其他指标

```java
{
    //不细分
    "byField": null,
    //返回总体和每日留存情况，第一条结果为总体，后续为每日详情
    "rows": [
        {
            //总体（这里的总体是各日期汇总去重之后的结果，和页面上展示的总体结果不一致）
            "byValue": "$ALL",
            //目标用户数
            "totalPeople": 139,
            //留存人数 按照索引 依次为当日、第一日、第二日...第七日
            "retentionPeoples": [
                139,
                10,
                8,
                7,
                7,
                7,
                6,
                2
            ],
            //留存用户中同时触发指标指标事件和retentionPeoples对应
            "measures": [
                139,
                //第一日留存10人中 其中进行了任意事件触发用户数的有10人
                10,
                8,
                7,
                7,
                7,
                6,
                2
            ]
        },
        //对应每日留存
        {
            "byValue": "2019/07/17 00:00:00",
            "totalPeople": 29,
            "retentionPeoples": [
                29,
                7,
                6,
                1,
                1,
                4,
                4,
                2
            ],
            "measures": [
                29,
                7,
                6,
                1,
                1,
                4,
                4,
                2
            ]
        },
        {
            "byValue": "2019/07/18 00:00:00",
            "totalPeople": 100,
            "retentionPeoples": [
                100,
                6,
                1,
                1,
                4,
                4,
                3,
                0
            ],
            "measures": [
                100,
                6,
                1,
                1,
                4,
                4,
                3,
                0
            ]
        },
        {
            "byValue": "2019/07/19 00:00:00",
            "totalPeople": 22,
            "retentionPeoples": [
                22,
                2,
                3,
                7,
                6,
                4,
                0,
                0
            ],
            "measures": [
                22,
                2,
                3,
                7,
                6,
                4,
                0,
                0
            ]
        }
    ],
    "reportUpdateTime": "2019-07-24 14:20:28"
}
```

## 4. 接口调用示例

```java
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X POST --data '{
    "firstEvent": {
        "expression": "event.$startup"
    },
    "secondEvent": {
        "expression": "event.$pageview"
    },
    "measures": {
        "expression": "event.$Anything",
        "aggregator": "TRIGGER_USER_COUNT"
    },
    "crowds": ["$ALL"],
    "fromDate": "2019-07-17",
    "toDate": "2019-07-19",
    "byField": null,
    "byFieldIndex": -1,
    "duration": 7,
    "unit": "DAY",
    "samplingFactor": 1,
    "useCache": true,
    "limit": 50
}' http://127.0.0.1:4005/uba/api/retentions/analyze
```

