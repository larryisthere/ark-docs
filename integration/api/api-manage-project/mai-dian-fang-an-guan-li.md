---
description: 方舟4.6.0112版本中新增。
---

# 埋点方案管理

## 1. 获取用户属性方案列表

获取系统预置、用户自定义、代码埋点的用户属性列表，包含计划内和计划外，未回数和已回数的所有用户属性。

### 1.1 接口地址

> 【GET】 /uba/api/schema/userProperties

### 1.2 请求参数示例

```text
?plan=1&dataStatus=1
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。

#### **1.2.1 入参说明**

| 参数名称 | 类型 | 必填 | 说明 | 枚举 |
| :--- | :--- | :--- | :--- | :--- |
| plan | Integer | N | 计划状态：1为计划内 0为计划外 | 0/1 |
| dataStatus | Integer | N | 是否回数：1为已回数 0为未回数 | 0/1 |

### 1.3 返回结果示例

```java
[
    {
        //用户属性ID，唯一
        "id":"xwho",
        //用户属性名称，用于页面展示
        "name":"用户ID",
        //是否可用 1为启用 0为禁用 （通过方舟系统可以控制用户属性的可用性）
        "enable":1,
        //是否可见 1为可见 0为不可见（用于方舟系统，系统预置但不用于分析的属性在页面上被隐藏了）
        "visible":1,
        //是否预置属性 1为预置属性 0为自定义属性
        "preset": 1,
        //数据类型，有string、boolean、number、datetime、array<string>五种
        "dataType": "string",
        //计划状态：1为计划内 0为计划外
        "plan": 1,
        //是否回数：1为已回数 0为未回数
        "dataStatus": 1,
        //是否有字典 1为有字典 0为未上传字典
        "dict": 0,
        //是否脱敏 1为全局脱敏 0为不脱敏、展示原始值
        "masking": 0
    }
]
```

### 1.4 接口调用示例

```javascript
curl -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" http://127.0.0.1:4005/ark/uba/api/schema/userProperties?plan=1&dataStatus=1
```

## 2. 获取事件方案列表

获取系统预置、用户自定义、代码埋点的事件列表，包含计划内和计划外，未回数和已回数的所有事件。

### 2.1 接口地址

> 【GET】 /uba/api/schema/event

### 2.2 请求参数示例‌

```text
?plan=1&dataStatus=1
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。‌

#### **2.2.1 入参说明**

| 参数名称 | 类型 | 必填 | 说明 | 枚举 |
| :--- | :--- | :--- | :--- | :--- |
| plan | Integer | N | 计划状态：1为计划内 0为计划外 | 0/1 |
| dataStatus | Integer | N | 是否回数：1为已回数 0为未回数 | 0/1 |

### 2.3 返回结果示例

```java
[
    {
        // 事件ID，唯一
        "id": "$startup",
        // 事件名称，用于页面展示
        "name": "启动",
        // 计划状态：1为计划内 0为计划外
        "plan": 1,
        // 是否回数：1为已回数 0为未回数
        "dataStatus": 1,
        // 是否预置事件 1为预置事件 0为自定义事件
        "preset": 1,
        // 是否可用 1为启用 0为禁用 （通过方舟系统可以控制事件的可用性）
        "enable": 1,
        // 备注
        "remark": "APP启动 / 打开网站",
        //回数平台
        "platform": "JS"
    },
    {
        "id": "login",
        "name": "登录",
        "plan": 1,
        "dataStatus": 1,
        "preset": 0,
        "enable": 1,
        "remark": "用户登录",
        "platform": "JS"
    }
]
```

### 2.4 接口调用示例

```java
curl -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" http://127.0.0.1:4005/ark/uba/api/schema/event?plan=1&dataStatus=1
```

## 3. 获取事件属性埋点方案

获取事件的属性列表，包含**事件自定义属性**和**通用属性**两种，自定义属性需要自己埋点上报，通用属性由方舟系统自动采集。

包含计划内和计划外，未回数和已回数的所有事件属性。

### 3.1 接口地址

> 【GET】 /uba/api/schema/eventProperties

### 3.2 请求参数示例

```text
//【必填】通过urlPath传参
eventId=?&plan=1&dataStatus=1
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#1-api-jie-shao)。

#### **3.2.1 入参说明**

| 参数名称 | 类型 | 必填 | 说明 | 枚举 |
| :--- | :--- | :--- | :--- | :--- |
| eventId | String | Y | 事件ID |  |
| plan | Integer | N | 计划状态：1为计划内 0为计划外 | 0/1 |
| dataStatus | Integer | N | 是否回数：1为已回数 0为未回数 | 0/1 |

### 3.3 返回结果示例

```java
[
    {
        //事件属性ID
        "id":"$is_first_time",
        //事件属性名称，用于页面展示
        "name":null,
        //备注
        "remark":null,
        //是否可用 1为启用 0为禁用 （通过方舟系统可以控制事件属性的可用性）
        "enable":1,
        //是否通用属性，1为通用属性 0为事件定义属性
        "global":0,
        //是否事件预置属性 1为预置属性 0为自定义属性
        "preset": 1,
        //数据类型，有string、boolean、number、datetime、array<string>五种
        "dataType": "boolean",
        //计划状态：1为计划内 0为计划外
        "plan": 1,
        //是否回数：1为已回数 0为未回数
        "dataStatus": 1,
        //是否有字典 1为有字典 0为未上传字典
        "dict": 0，
        //是否脱敏 1为全局脱敏 0为不脱敏、展示原始值
        "masking": 0
    }
]
```

> **global**： 0为事件自定义属性，1为通用属性，每个事件都默认有的属性称为通用属性。

### 3.4 接口调用示例

```javascript
curl -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" http://127.0.0.1:4005/uba/api/schema/eventProperties?eventId=%24startup&plan=1&dataStatus=1
```

## 4. 新增用户属性埋点方案

管理用户属性，在客户使用方舟时，对于已经明确的用户特性，可以提前在埋点方案中添加用户属性。

添加过程中，如果用户属性 在计划外，接口会自动将用户属性加入计划内并且修改属性名称和备注。

如果用户属性已经在埋点方案中且数据类型一致，则会自动跳过，不进行任何修改。

### 4.1 接口地址

> 【POST】 /uba/api/schema/userProperties

### 4.2 请求参数示例

```java
{
  //【必填】用户属性ID，规则：不能重复，由字母、数字和下划线组成，不能以数字开头，总长度不超过100个字符
  "id":"userProperty",
  //用户属性名称，规则：不能包含特殊字符，长度不超过50
  "name":"属性展示名称1",
  //【必填】用户属性数据类型
  "dataType":"string",
  //用户属性备注，规则：不能包含特殊字符，长度不超过100
  "remark":"备注"
}
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。
>
> **dataType：**属性数据类型，方舟目前支持五种数据类型，枚举值如下：
>
> * **string**：字符串；
> * **number**：数值，包含整数和小数点数据；
> * **boolean**：布尔，只包含 true/false；
> * **datetime**：日期，如yyyy-MM-dd HH:mm:ss.SSS 或yyyy-MM-dd HH:mm:ss 或yyyy-mm-dd；
> * **array&lt;string&gt;**：集合，字符串集合。

### 4.3 返回结果示例

```text
{"success":0}
```

### 4.4 接口调用示例

```javascript
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X POST --data '{
  "id":"userProperty",
  "name":"属性展示名称1",
  "dataType":"string",
  "remark":"备注"
}' http://127.0.0.1:4005/uba/api/schema/event
```

## 5. 新增事件和事件属性埋点方案

管理事件及事件属性，在客户使用方舟时，对于已经明确的行为定义，可以提前在埋点方案中添加事件和事件对应的属性。

添加过程中，如果事件或事件属性在计划外，接口会自动将事件加入到埋点方案并且修改事件和属性的展示名称和备注。

如果事件或者事件属性已经在埋点方案中，则会自动跳过，不进行任何修改。

### 5.1 接口地址

> 【POST】 /uba/api/schema/event

### 5.2 请求参数示例

```java
{
  //【必填】事件ID，规则：不能重复，由字母、数字和下划线组成，不能以数字开头，总长度不超过100个字符
  "id":"eventId",
  //事件名称，规则：不能包含特殊字符，长度不超过50
  "name":"事件展示名称",
  //事件备注，规则：不能包含特殊字符，长度不超过100
  "remark":"备注"
  //事件属性集合
  "properties":[
    {
      //【必填】事件属性ID，规则：不能重复，由字母、数字和下划线组成，不能以数字开头，总长度不超过100个字符
      "id":"eventProperty1",
      //事件属性名称，规则：不能包含特殊字符，长度不超过50
      "name":"事件属性展示名称1",
      //【必填】事件属性数据类型
      "dataType":"string",
      //事件属性备注，规则：不能包含特殊字符，长度不超过100
      "remark":"备注"
    },
    {
      "id":"eventProperty2",
      "name":"事件属性展示名称2",
      "dataType":"number",
      "remark":"备注"
    }
  ]
}
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。
>
> **dataType：**属性数据类型，方舟目前支持五种数据类型，枚举值如下：
>
> * **string**：字符串；
> * **number**：数值，包含整数和小数点数据；
> * **boolean**：布尔，只包含 true/false；
> * **datetime**：日期，如yyyy-MM-dd HH:mm:ss.SSS 或yyyy-MM-dd HH:mm:ss 或yyyy-mm-dd；
> * **array&lt;string&gt;**：集合，字符串集合。

### 5.3 返回结果示例

```text
{"success":0}
```

### 5.4 接口调用示例

```javascript
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X POST --data '{
  "id":"eventId",
  "name":"事件展示名称",
  "remark":"备注"
  "properties":[
    {
      "id":"eventProperty1",
      "name":"事件属性展示名称1",
      "dataType":"string",
      "remark":"备注"
    },
    {
      "id":"eventProperty2",
      "name":"事件属性展示名称2",
      "dataType":"number",
      "remark":"备注"
    }
  ]
}' http://127.0.0.1:4005/uba/api/schema/event
```

## 6. 修改用户属性埋点方案

根据属性ID修改用户属性的展示名称、备注，在属性未回数的情况下可以修改数据类型。

### 6.1 接口地址

> 【PUT】 /uba/api/schema/userProperties?id=wechatid

### 6.2 请求参数示例

```text
{
  //用户属性名称，规则：不能包含特殊字符，长度不超过50【不需要修改可不传】
  "name":"微信ID",
  //用户属性数据类型 【不需要修改可不传】
  "dataType":"string",
  //用户属性备注，规则：不能包含特殊字符，长度不超过100 【不需要修改可不传】
  "remark":"备注"
}
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#1-api-jie-shao)。
>
> **参数说明**：参数分为两部分，url上传用户属性ID，RequestBody中传需要修改的内容：
>
> * id：【必填】用户属性ID，【必填】通过urlPath传参；
> * RequestBody可以支持传入name，dataType，remark，需要修改哪部分内容就传哪个属性

### 6.3 返回结果示例

```text
{"success":0}
```

### 6.4 接口调用示例

```javascript
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X PUT --data '{
  "name":"微信ID"
}' http://127.0.0.1:4005/uba/api/schema/userProperties?id=wechatid
```

## 7. 修改事件埋点方案

根据事件ID修改埋点方案中事件的显示名称、备注。

### 7.1 接口地址

> 【PUT】 /uba/api/schema/event?id=submitPay

### 7.2 请求参数示例

```java
{
  //事件名称，规则：不能包含特殊字符，长度不超过50【不需要修改可不传】
  "name":"提交支付请求",
  //事件备注，规则：不能包含特殊字符，长度不超过100 【不需要修改可不传】
  "remark":"备注"
}
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#1-api-jie-shao)。
>
> **参数说明**：参数分为两部分，url上传用户属性ID，RequestBody中传需要修改的内容：
>
> * id：【必填】用户属性ID，【必填】通过urlPath传参；
> * RequestBody可以支持传入name，remark，需要修改哪部分内容就传哪个属性。

### 7.3 返回结果示例

```text
{"success":0}
```

### 7.4 接口调用示例

```javascript
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X PUT --data '{
  "name":"提交支付请求"
}' http://127.0.0.1:4005/uba/api/schema/event?id=submitPay
```

## 8. 修改事件属性埋点方案

根据事件ID和事件属性ID修改事件对应属性的展示名称、备注，在属性还未回数的情况下可以修改数据类型。

### 8.1 接口地址

> 【PUT】 /uba/api/schema/eventProperties?eventId=submitPay&propertyId=submitTime

### 8.2 请求参数示例

```java
{
  //事件属性名称，规则：不能包含特殊字符，长度不超过50【不需要修改可不传】
  "name":"提交时间",
  //事件属性数据类型 【不需要修改可不传】
  "dataType":"datetime",
  //事件属性备注，规则：不能包含特殊字符，长度不超过100 【不需要修改可不传】
  "remark":"备注"
}
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#1-api-jie-shao)。
>
> **参数说明**：参数分为两部分，url上传事件ID和用户属性ID，RequestBody中传需要修改的内容：
>
> * eventId：事件ID，【必填】通过urlPath传参；
> * propertyId：属性 ID，【必填】通过urlPath传参；
> * RequestBody可以支持传入name，dataType，remark，需要修改哪部分内容就传哪个属性

### 8.3 返回结果示例

```text
{"success":0}
```

### 8.4 接口调用示例

```javascript
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X PUT --data '{
  "name":"提交时间"
}' http://127.0.0.1:4005/uba/api/schema/eventProperties?eventId=submitPay&propertyId=submitTime
```

