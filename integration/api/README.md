# API

## 1. 调用方法

API 采用标准的 HTTP 方式，调用的 URL 为:

```text
http://{$WEB_URL}/uba/api/{$API_URL}
```

其中 **$WEB\_URL** 为访问网站的 URL，**$API\_URL** 表示具体的 API。

如果使用方舟公有云 服务，则对应 $WEB\_URL 如下:

```text
http://xxx.analysys.cn   
```

如果用户使用的是单机版私有化部署，则对应 $WEB\_URL 如下：

```text
http://{$host_name}:4005
```

如果用户使用的集群版私有化部署，则对应 $WEB\_URL 如下，$host\_name 为对应ark2 服务器：

```text
http://{$host_name}:4005
```

{% hint style="info" %}
如果私有化部署的过程中修改了Nginx的默认配置，或通过CDN等访问方舟，则请咨询相关人员获取配置信息。
{% endhint %}

### 1.1 默认参数

所有接口调用时都需要在**header**中带上token信息和appKey信息，非特殊说明的接口输入参数为JSON格式，示例如下：

```http
Request Headers
    Content-Type: application/json
    token: {$API_SECRET}
    appKey: {$APP_KEY}
```

$API\_SECRET 为项目API调用的验证标识，获取方式参考 [API Secret](./#3-api-secret)。

$APP\_KEY为项目标识，对应方舟系统中的AppKey。

### 1.2 调用示例

获取到 $WEB\_URL 地址后，就可以使用任意HTTP Client进行 API 调用，例如使用 curl工具访问事件分析，例子如下：

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

## 2. 通用参数

### 2.1 表达式

API中使用的指标、过滤条件、维度等都会使用表达式，所以几乎所有API都会使用表达式。表达式分为事件表达式和属性表达式两种，对应的参数名称都为expression。

#### 2.1.1 事件表达式

事件表达式格式为 **event.事件ID** 的方式，如 **注册事件** 的事件表达式为：

```java
"expression": event.signup
```

{% hint style="info" %}
任意事件由系统预置，事件ID为 $Anything，**任意事件** 表达式为：**event.$Anything**
{% endhint %}

#### 2.1.2 属性表达式

属性包括事件属性和用户属性

事件属性使用 **event.事件ID.属性ID** 的方式，例如表示 **注册事件的注册方式** 这个事件属性的表达式为:

```java
"expression": event.signup.signuptype
```

用户属性使用 **user.用户属性ID** 的方式，例如表示 **用户注册时间** 这个用户属性的表达式如下:

```java
"expression": user.$signup_time
```

### `2.2 过滤条件`

过滤条件主要用来筛选符合期望的某些事件结果或者用户结果，根据条件使用的场景不同，能选择到的过滤内容也不同：

* 如果过滤条件应用在某个事件下，可以使用通用事件属性、用户属性和这个事件独有的事件属性。
* 如果过滤条件是全局的，针对于所有事件生效，可以使用通用事件属性、用户属性和所有事件都有（根据属性ID判断）的事件属性。

过滤条件使用参数名称为flter，参数格式为JSON，如下：

```java
"filter":{
    //过滤条件列表，可以输入多个
    "conditions": [ 
       {
            //条件，如任意事件平台等于JS或iOS
            "expression": "event.$Anything.$platform",
            //条件操作符号
            "function": "equal",
            // 条件的参数，根据不同的操作符可以有一个或者多个，数组格式
            "params": ["JS","iOS"]
       },
       {
            // SDK版本等于4.0.4.001
            "express": "event.$Anything.$lib_version",
            "function": "equal",
            "params": ["4.0.4.001"]
       }
   ],
    //条件之间的关系，只能是 且（AND）/ 或（OR）
    "relation": "AND"
}                         
```

#### 2.2.1 条件之间的关系

各个条件之间的关系只支持AND/OR。

* **AND**：条件之间都是且的关系，查询的数据需要满足所有条件。
* **OR**：条件之间都是或的关系，查询的数据只要满足其中一个条件即可。

#### 2.2.2 条件操作符号

条件操作符主要用在过滤条件中，不同属性数值类型，支持不同操作运算符。

{% tabs %}
{% tab title="字符串" %}
* **EQ**

  等于，当params一个值时，表示等于这个值的才满足；当params有多个值时，表示满足其中一项即可，相当于in。

* **NOT\_EQ**

  不等于，当params一个值时，表示不等于这个值的都满足；当params有多个值时，表示不能是其中任意一项，相当于not in。

* **CONTAIN**

  包含，属性值包含指定字符串，相当于 like %params\[0\]%。

* **NOT\_CONTAIN**

  不包含，属性值不包含指定字符串，相当于not like %params\[0\]%。

* **NOT\_NULL**

  有值，属性值有值，相当于 is not null。

* **NULL**

  无值，属性值无值，相当于 is null。

* **NOT\_EMPTY**

  不为空字符串，属性值无值，相当于 != ''。

* **EMPTY**

  为空字符串，属性值有值，相当于 = ''。
{% endtab %}

{% tab title="数值" %}
* EQ

  等于，当params一个值时，表示等于这个值的才满足；当params有多个值时，表示满足其中一项即可，相当于in。

* NOT\_EQ

  不等于，当params一个值时，表示不等于这个值的都满足；当params有多个值时，表示不能是其中任意一项，相当于not in。

* GT

  大于。

* GTE

  大于等于。

* LT

  小于。

* LTE

  小于等于。

* RANGE

  区间，在某个数据区间内的数值，相当于 &gt;= params\[0\] and &lt;= params\[1\] 或者 between params\[0\] and params\[1\]。

* NOT\_NULL

  有值，属性值有值，相当于 is not null。

* NULL

  无值，属性值无值，相当于 is null。
{% endtab %}

{% tab title="布尔" %}
* TRUE

  为真。

* FALSE

  为假。

* NOT\_NULL

  有值，属性值有值，相当于 is not null。

* NULL

  无值，属性值无值，相当于 is null。
{% endtab %}

{% tab title="日期" %}
* ABSOLUTE\_TIME

  绝对时间，在某个具体的时间范围，相当于 &gt;= params\[0\] and &lt;= params\[1\] 或者 between params\[0\] and params\[1\]。

  ```java
    {
        "expression": "user.$signup_time",
        "function": "relative",
        "params": ["2019/05/01 00:00","2019/05/31 00:00"]
    }
  ```

* RELATIVE\_TIME\_OF\_CUURENT

  在相对当前的某段时间里。

  四天之内

  ```java
    {
        "expression": "user.$signup_time",
        "function": "relative",
        "params": ["4", "day", "within"]
    }
  ```

  四天之前

  ```java
    {
        "expression": "user.$signup_time",
        "function": "relative",
        "params": ["4", "day", "before"] 
    }
  ```

* RELATIVE\_TIME\_OF\_CUURENT

  在相对当前的时间区间。

  在过去3天到过去1天之内

  ```java
    {
        "expression": "user.$signup_time",
        "function": "relative_between",
        "params": ["3","1","day"]
    }
  ```

* RELATIVE\_TIME\_OF\_EVENT

  相对于事件发生时间。 如注册时间在事件发生时间之后（after）/之前（before）3天（day）/小时（hour）/分（minute）/秒（second）

  ```java
    {
        "expression": "user.$signup_time",
        "function": "relative_event_time",
        "params": ["after","3","day"]
    }
  ```

  注册时间在事件发生时间当天（day）/当周（week）/当月（month）

  ```java
    {
        "expression": "user.$signup_time",
        "function": "relative_event_time",
        "params": ["day"]
    }
  ```

* NOT\_NULL

  有值，属性值有值，相当于 is not null。

* NULL

  无值，属性值无值，相当于 is null。
{% endtab %}

{% tab title="集合" %}
* EQ

  等于，集合只取第一个值进行完整匹配。

* CONTAIN

  包含，集合里面包含某个值。

* NOT\_CONTAIN

  不包含，集合里面不包含某个值。

* NOT\_NULL

  有值，属性值有值，相当于 is not null。

* NULL

  无值，属性值无值，相当于 is null。

* EMPTY

  为空字符串，集合里面包含空字符串。

* NOT\_EMPTY

  不为空字符串，集合里面不包含空字符串。
{% endtab %}
{% endtabs %}

### 2.3 选择用户分群

查询某个分群的数据，对应分群code。如：

```java
"crowds":["$ALL"]
```

{% hint style="info" %}
1、默认为全部用户，**全部用户** 为 **$ALL**。自定义分群code 获取参见 [获取所有用户分群](api-cohort.md#1-huo-qu-suo-you-yong-hu-fen-qun-lie-biao)。

2、当前所有API**不支持多分群对比**。
{% endhint %}

### 2.4 查询时间范围

格式固定为 yyyy-MM-dd，查询都是 大于等于 开始时间，小于等于 结束时间

```java
//开始时间
"fromDate": "2019-06-18"
//结束时间
"toDate": "2019-06-24"
```

### 2.5 选择抽样

抽样因子，范围为1-64， 1表示全量数据，64 表示1/64 ，如抽样二分之一：

```java
"samplingFactor":2
```

### 2.6 细分维度查看

查询结果按照事件属性/用户属性分组进行查看，不传为不分组。事件分析支持多维度查看，其余图表分析只支持单维度。

```java
"byFields":[
        {
            //按照屏幕高度分组
            "expression":"event.$Anything.$screen_width",
            //是否使用分桶条件查看结果，只有属性为数值类型时才能使用
            //【数值类型时必填】这里表示按照 -∞ ~ 600，600 ~ 800，800 ~ +∞ 三个区间查看
            "buckets":[
                600,
                800
            ],
            // 【日期类型时必填】这里表示不汇总按照按日统计
            "byTimeType":"TIME_RANGE_DAY"
        }
    ]
```

{% hint style="info" %}
**特殊说明：**

1、分桶（**buckets**）只适用于**数值类型**属性，当前只支持**自定义区间**。

2、**日期类型属性**，只支持默认统计，默认为**不分组，**支持按照不同 [时间粒度枚举](./#29-shi-jian-mei-ju) 查看。

3、多维度使用 byFields:\[\]，单维度使用 byField:{}，内部参数结构和示例一致。
{% endhint %}

### 2.7 时间粒度

查询结果可以选择按日（DAY）/周（WEEK）/月（MONTH）/小时（HOUR）/分钟（MINUTE）查看，如按日查看：

```java
"unit":"DAY"
```

{% hint style="info" %}
在按小时/分钟时间粒度查看时，查询时间范围必须是单天
{% endhint %}

### 2.8 使用缓存

是否使用缓存，true/false

* **true**：使用缓存，取上一次结果，如果缓存失效或者结果不存在，会重新查询。
* **false**：不取缓存，直接查询。

```java
"useCache": true
```

### 2.9 时间枚举

在API中，很多地方（如留存周期、漏斗转化周期、事件分析时间粒度）都有涉及到分时间粒度查看，关于时间粒度的参数值统一如下：

* **SECOND：**秒
* **MINUTE**：分钟
* **HOUR**：小时
* **DAY**：天
* **WEEK**：周
* **MONTH**：月
* **QUARTER**：季度
* **YEAR**：年

## 3. API Secret 

API Secret 用于导出接口认证，API Secret和项目绑定，即每个API Secret只能访问当前项目，项目管理员进入方舟产品后，在 【管理】 - 【项目概览】模块中可以获取和重置API Secret，操作如下：

![](../../.gitbook/assets/image%20%2851%29.png)

## 4. 接口请求参数快捷获取

因为大部分查询API的输入参数结构都比较复杂，在前期使用或者参数较复杂时不好拼装参数。所以在方舟产品中，对应图表分析中提供了根据页面选择的条件生成API输入参数的入口，点击即可获取需要的参数。

步骤：在分析导航中选择对应分析模块进入，选择需要查询的内容，点击右侧区域 获取API接口参数的图标获取参数。以事件范围为例：

![](../../.gitbook/assets/image.png)

{% hint style="info" %}
API在4.3.4之后的版本（含4.3.4）中开放，API Secret和参数快捷获取入口也在方舟产品中同步添加。
{% endhint %}

