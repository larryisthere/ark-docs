# 分群管理

## 1. 创建分群 <a id="4-huo-qu-suo-you-yong-hu-fen-qun-lie-biao"></a>

通过分群规则创建分群。在 4.5.0 版本中新增。‌

### 1.1 接口地址 <a id="41-jie-kou-di-zhi"></a>

> 【POST】 /uba/api/cohort

### 1.2 请求参数示例 <a id="42-qing-qiu-can-shu-shi-li"></a>

```java
{
    //分群名称，不允许重复
    "name": "apitest1",
    //动态分群
    "dynamic": 0,
    //分群创建规则
    "content": {
        "ruleGroup":[],
        "relations":[]
    }
}
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](https://app.gitbook.com/@analysys/s/ark/~/drafts/-LoxLQTQwGY5HTZzArUH/primary/integration/api#21-xiang-mu-jie-kou-ren-zheng)。
>
> **操作用户**：通过API创建的分群默认为API所有，不属于任何用户，如果想让某个用户在页面上对此分群进行操作，需要在URL上带loginUser参数，详情见 [操作用户](../#51-cao-zuo-yong-hu)。

#### 1.2.1 入参说明 <a id="4-2-1-ru-can-shuo-ming"></a>

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;&#x79F0;</th>
      <th style="text-align:left">&#x7C7B;&#x578B;</th>
      <th style="text-align:left">&#x5FC5;&#x586B;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
      <th style="text-align:left">&#x679A;&#x4E3E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">name</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">Y</td>
      <td style="text-align:left">&#x5206;&#x7FA4;&#x540D;&#x79F0;&#xFF0C;&#x4E0D;&#x80FD;&#x91CD;&#x590D;</td>
      <td
      style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">dynamic</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">Y</td>
      <td style="text-align:left">
        <p>1&#xFF1A;&#x4E3A;&#x52A8;&#x6001;&#x5206;&#x7FA4; &#x6BCF;&#x65E5;&#x51CC;&#x6668;&#x8BA1;&#x7B97;&#x4E00;&#x6B21;</p>
        <p>0&#xFF1A;&#x9759;&#x6001;&#x5206;&#x7FA4; &#x5728;&#x521B;&#x5EFA;&#x5206;&#x7FA4;&#x7684;&#x65F6;&#x5019;&#x8BA1;&#x7B97;&#x4E00;&#x6B21;</p>
      </td>
      <td style="text-align:left">
        <p>1</p>
        <p>0</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">content</td>
      <td style="text-align:left">Json</td>
      <td style="text-align:left">Y</td>
      <td style="text-align:left">&#x5206;&#x7FA4;&#x521B;&#x5EFA;&#x89C4;&#x5219;</td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>#### 1.2.2 创建规则说明

分群规则是多层嵌套的结构，以下针对于分群规则详情说明。

**一、规则组装**

content里面存放的是分群规则和规则之间的关系，可自由组装。

```java
"content":{
    //规则，可以是多个
    "ruleGroup":[
        {
            "rules":[
                {
                    ...
                }
            ],
            "relation":"AND"
        }
    ],
    //规则之间的逻辑关系，当前支持且和非。 且：AND，非：AND_NOT
    "relations":[
        "AND"
    ]
}
```

{% hint style="info" %}
**规则之间的关系定位为集合类型，是为了支持规则之间的多种关系，使用如下：**

**1、r**elations **为空**，规则之间之间逻辑关系**默认**为 **且**。

2、relations 1个值，规则之间都使用相同逻辑关系。

3、relations 大于1个值，在规则个数大于2时使用，表示第N个和第N+1个规则之间的逻辑关系。即 relations.size = ruleGroup.size -1。
{% endhint %}

**二、规则定义**

ruleGroup 存放的规则内容的集合以及间的逻辑关系

```java
{
    "ruleGroup":{
        "rules":[
            ...
        ],
        //规则内容之间的逻辑关系，当前支持 AND/OR 两种
        "relation":"AND"
    }
}
```

**三、规则内容**

规则内容当前支持用户属性规则和事件规则两种。

**用户属性规则**

```java
{
    //【必填】user表示为用户属性规则
    "type":"user", 
    //【必填】用户属性表达式
    "expression":"user.$email",  
    //【必填】操作符，和属性数据类型相关
    "function":"NOT_EQ",  
    // 规则条件值，根据不同的操作符可以有一个或多个，数组
    "params":["XX"] 
}
```

以上表示的意思是：获取 **用户邮箱不等于XX** 的用户。

> **参数说明：**
>
> * **expression：**用来指定具体事件/事件属性，更多介绍参照 [用户属性表达式](../api-analytics/#2-1-2-shu-xing-biao-da-shi)。
> * **function：**聚合操作符操作符，根据不同数据类型支持不同操作符，具体参考 [条件操作符号](../api-analytics/#2-2-2-tiao-jian-cao-zuo-fu-hao)。
> * **filter：**事件的过滤条件，具体参考 [过滤条件](../api-analytics/#22-guo-lv-tiao-jian)。

**事件规则**

```java
{
    //【必填】event表示为事件规则
    "type":"event",
    //【必填】事件表达式/事件属性表达式
    "expression":"event.$startup",
    //【必填】事件发生的时间范围，和 eventRelativeTimeParam二选一.如果两个都传了会以eventAbsoluteTimeParams为准
    "eventAbsoluteTimeParams":["2019-09-12","2019-09-18"],
    //【必填】聚合操作符 如果是事件指定为触发次数：TOTAL_COUNT，如果是属性指定为去重数：REMOVE_DUMPLICATE
    "aggregator":"TOTAL_COUNT",
    //【必填】触发次数/去重数使用的操作符
    "function":"GTE",
    //触发次数/去重数满足的条件值
    "params":[
        "1"
    ],
    //针对于这个事件的具体过滤条件
    "filter":{
        ...
    }
}
```

以上表示的意思是：获取 **2019-09-12到2019-09-18内启动次数大于1** 的用户。

> **参数说明：**
>
> * **expression：**用来指定具体事件/事件属性，更多介绍参照 [表达式](../api-analytics/#21-biao-da-shi)。
> * **eventAbsoluteTimeParams：**事件发生的时间范围，绝对时间，**数组**（length=2），内容格式为yyyy-MM-dd。
> * **eventRelativeTimeParam：**事件发生的时间范围，相对时间，在创建动态分群时使用。需要按照指定格式传入，如近七日：6,0,day，过去七日：7,1,day，今日：0 day。
> * **aggregator：**聚合操作符，分群中只支持两种：
>   * **事件的触发次数：TOTAL\_COUNT** 
>   * **事件属性的去重数：REMOVE\_DUMPLICATE**
> * **function：**聚合操作符操作符，和数值类型的运算符一致（但**不支持** NOT\_NULL 和 NULL），具体参考 [条件操作符号](../api-analytics/#2-2-2-tiao-jian-cao-zuo-fu-hao)。
> * **filter：**事件的过滤条件，具体参考 [过滤条件](../api-analytics/#22-guo-lv-tiao-jian)。

### 1.3 返回结果示例 <a id="43-fan-hui-jie-guo-shi-li"></a>

```java
‌{
    "id": 2,
    "code": "arkfq_2",
    "name": "test",
    "dynamic": 1,
    "createTime": "2019-09-12 11:53:24",
    "content": {...}
}
```

### 1.4 接口调用示例 <a id="44-jie-kou-tiao-yong-shi-li"></a>

```java
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X POST --data '{
	"name":"apiTest02",
	"dynamic":0,
	"content":{
	    "ruleGroup":[
	        {
	            "rules":[
	                {
	                    "type":"user",
	                    "expression":"user.xwho",
	                    "function":"NOT_NULL",
	                    "params":[
	
	                    ]
	                },
	                {
	                    "type":"user",
	                    "expression":"user.$email",
	                    "function":"NOT_EQ",
	                    "params":[
	                        "xx1",
	                        "xx2"
	                    ]
	                },
	                {
	                    "type":"user",
	                    "expression":"user.$signup_time",
	                    "function":"RELATIVE_TIME_OF_CUURENT",
	                    "params":[
	                        "30",
	                        "day",
	                        "within"
	                    ]
	                }
	            ],
	            "relation":"AND"
	        },
	        {
	            "rules":[
	                {
	                    "type":"event",
	                    "expression":"event.$startup",
	                    "eventAbsoluteTimeParams":[
	                        "2019-09-09",
	                        "2019-09-15"
	                    ],
	                    "aggregator":"TOTAL_COUNT",
	                    "function":"GTE",
	                    "params":[
	                        0
	                    ]
	                }
	            ],
	            "relation":"AND"
	        }
	    ],
	    "relations":[
	        "AND"
	    ]
	}
}' http://127.0.0.1:4005/uba/api/cohort
```

## 2. 获取单个用户分群信息

获取单个用户分群信息。在 4.5.0 版本中新增。

### 2.1 接口地址

> 【GET】 /uba/api/cohort/{id}

### 2.2 请求参数示例

```java
?needMore=false
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。

#### 2.2.1 入参说明

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;&#x79F0;</th>
      <th style="text-align:left">&#x7C7B;&#x578B;</th>
      <th style="text-align:left">&#x5FC5;&#x586B;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
      <th style="text-align:left">&#x679A;&#x4E3E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">id</td>
      <td style="text-align:left">Long</td>
      <td style="text-align:left">Y</td>
      <td style="text-align:left">&#x5206;&#x7FA4;ID</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">needMore</td>
      <td style="text-align:left">Boolean</td>
      <td style="text-align:left">N</td>
      <td style="text-align:left">
        <p>&#x662F;&#x5426;&#x9700;&#x8981;&#x5206;&#x7FA4;&#x7684;&#x8BE6;&#x7EC6;&#x4FE1;&#x606F;&#x3002;</p>
        <p>&#x5907;&#x6CE8;&#xFF1A;URL&#x4F20;&#x53C2;&#xFF0C;&#x9ED8;&#x8BA4;&#x4E3A;false</p>
      </td>
      <td style="text-align:left">
        <p>true</p>
        <p>false</p>
      </td>
    </tr>
  </tbody>
</table>### 2.3 返回结果示例

```java
{
    //分群id 在操作某个分群时使用
    "id": 2,
    //分群code 在查询用户详情时需要用到分群时都通过code来筛选
    "code": "arkfq_2",
    //分群名称，用户页面定义
    "name": "test",
    //动态分群 1为动态分群 每日凌晨计算一次
    "dynamic": 1,
    //当前分群的用户数
    "userNumber": 992,
    //分群状态 success：成功，failed：失败，calculating：计算中
    "status": "success",
    //分群最后一次计算时间 
    "calculatedTime": "2019-09-12 11:53:29"
}
```

如果URL上传参 needMore=true，则返回参数中会多一下内容：

```java
{
    //分群来源 自定义分群是通过分群模块选择规则创建
    "source": "自定义分群",
    //创建时间
    "createdTime": "2019-09-12 11:53:24",
    //创建人
    "createdUser": "管理员",
    //最后更新时间
    "updatedTime": "2019-09-12 11:53:29",
    //分群规则内容
    "content": {...}
}
```

### 2.4 接口调用示例

```java
curl -H "token:1b554f363d56238bf33a201620f2e9a9" -H "appKey:31abd9593e9983ec" http://127.0.0.1:4005/uba/api/cohort/2?needMore=false
```

## 3. 获取所有用户分群列表

获取某个项目下所有的用户分群列表。在 4.3.5 版本中新增。

### 3.1 接口地址

> 【GET】 /uba/api/cohort

### 3.2 请求参数示例

```java
//4.5.0 版本中才支持输入参数过滤
?needMore=true&status=success
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。

#### 3.2.1 入参说明

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;&#x79F0;</th>
      <th style="text-align:left">&#x7C7B;&#x578B;</th>
      <th style="text-align:left">&#x5FC5;&#x586B;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
      <th style="text-align:left">&#x679A;&#x4E3E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">needMore</td>
      <td style="text-align:left">Boolean</td>
      <td style="text-align:left">N</td>
      <td style="text-align:left">
        <p>&#x662F;&#x5426;&#x9700;&#x8981;&#x5206;&#x7FA4;&#x7684;&#x8BE6;&#x7EC6;&#x4FE1;&#x606F;&#x3002;</p>
        <p>&#x5907;&#x6CE8;&#xFF1A;URL&#x4F20;&#x53C2;&#xFF0C;&#x9ED8;&#x8BA4;&#x4E3A;false</p>
      </td>
      <td style="text-align:left">
        <p>true</p>
        <p>false</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">status</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">N</td>
      <td style="text-align:left">
        <p>&#x5206;&#x7FA4;&#x8BA1;&#x7B97;&#x72B6;&#x6001;&#x3002;&#x6210;&#x529F;&#x3001;&#x5931;&#x8D25;&#x3001;&#x8BA1;&#x7B97;&#x4E2D;&#x4E09;&#x79CD;&#x72B6;&#x6001;&#x3002;</p>
        <p>&#x5907;&#x6CE8;&#xFF1A;URL&#x4F20;&#x53C2;&#xFF0C;&#x4E0D;&#x4F20;&#x4E3A;&#x83B7;&#x53D6;&#x6240;&#x6709;&#x72B6;&#x6001;&#x5206;&#x7FA4;&#x3002;</p>
      </td>
      <td style="text-align:left">
        <p>success</p>
        <p>failed</p>
        <p>calculating</p>
      </td>
    </tr>
  </tbody>
</table>### 3.3 返回结果示例

```java
[
    {
        //【4.5.0中新增】分群id 在操作某个分群时使用
        "id": 2,
        //分群code 在查询用户详情时需要用到分群时都通过code来筛选
        "code": "arkfq_2",
        //分群名称，用户页面定义
        "name": "test",
        //动态分群 1为动态分群 每日凌晨计算一次
        "dynamic": 1,
        //当前分群的用户数
        "userNumber": 992,
        //【4.5.0中新增】分群状态 success：成功，failed：失败，calculating：计算中
        "status": "success",
        //【4.5.0中新增】分群最后一次计算时间 
        "calculatedTime": "2019-09-12 11:53:29",
        //----------以下输出字段在needMore=true时才会返回------------//
        //【4.5.0中新增】分群来源 自定义分群是通过分群模块选择规则创建
        "source": "自定义分群",
        //【4.5.0中新增】创建时间
        "createdTime": "2019-09-12 11:53:24",
        //【4.5.0中新增】创建人
        "createdUser": "管理员",
        //【4.5.0中新增】最后更新时间
        "updatedTime": "2019-09-12 11:53:29",
        //【4.5.0中新增】分群规则内容
        "content": {}
    },
    {
        "id": 5,
        "code": "arkfq_5",
        "name": "test",
        //0为静态分群 在创建分群的时候计算一次
        "dynamic": 0,
        "userNumber": 22,
        "status": "success",
        "calculatedTime": "2019-09-15 13:53:29",
        //分群来源，分析下钻分群通过个分析模块下钻创建，下钻的分群不返回规则内容
        "source": "分析下钻分群",
        "createdTime": "2019-09-15 13:53:24",
        "createdUser": "管理员",
        "updatedTime": "2019-09-15 13:53:29",
        "content": ""
    }
]
```

### 3.4 接口调用示例

```java
curl -H "token:1b554f363d56238bf33a201620f2e9a9" -H "appKey:31abd9593e9983ec" http://127.0.0.1:4005/uba/api/cohort?needMore=true&status=success
```

## 4. 重新计算单个分群

静态分群只在创建的时候计算一次，如果创建后有数据更新，可以调用接口重新计算一次分群。在 4.5.0 版本中新增。

### 4.1 接口地址

> 【POST】 /uba/api/cohort/{id}/recalculate

### 4.2 请求参数示例

无

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。

#### 4.2.1 入参说明

| 参数名称 | 类型 | 必填 | 说明 | 枚举 |
| :--- | :--- | :--- | :--- | :--- |
| id | Long | Y | 分群ID |  |

### 4.3 返回结果示例

```java
{
    //人群ID
    "id": 2,
    //分群状态
    "status": "calculating"
}
```

### 4.4 接口调用示例

```java
curl -H "token:1b554f363d56238bf33a201620f2e9a9" -H "appKey:31abd9593e9983ec" -X POST http://127.0.0.1:4005/uba/api/cohort/2/recalculate
```

## 5. 删除单个分群

当前用户分群规则是只能删除自己创建的分群，如果API不带loginUser参数只能删除通过API创建并且未带loginUser的分群。如果带loginUser参数，就能删除对应用户创建的分群。在 4.5.0 版本中新增。

### 5.1 接口地址

> 【DELETE】 /uba/api/cohort/{id}

### 5.2 请求参数示例

无

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。
>
> **操作用户**：通过API默认只能删除属于API的分群，如果需要删除某个用户的分群，需要在URL上带loginUser参数，详情见 [操作用户](../#51-cao-zuo-yong-hu)。

#### 5.2.1 入参说明

| 参数名称 | 类型 | 必填 | 说明 | 枚举 |
| :--- | :--- | :--- | :--- | :--- |
| id | Long | Y | 分群ID |  |

### 5.3 返回结果示例

```java
{
    //返回success说明接口调用成功
    "success":0
}
```

### 5.4 接口调用示例

```java
curl -H "token:1b554f363d56238bf33a201620f2e9a9" -H "appKey:31abd9593e9983ec" -X DELETE http://127.0.0.1:4005/uba/api/cohort/2
```

