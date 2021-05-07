---
description: 方舟5.4.1版本中新增API。
---

# 广告跟踪

## 1. 创建广告跟踪

创建一个广告跟踪。

### 1.1 接口地址

> 【POST】 /ark/uba/api/operations/utm

### 1.2 请求参数示例

```javascript
// 【非必填】通过urlPath传参, username是创建人的用户名
loginUser=?
// body 中传参
{
    // 【必填】目标网址
    "pageUrl": "http://www.baidu.com",
    // 【必填】广告名称
    "utmCampaign": "xx平台广告",
    // 【必填】广告媒介
    "utmMedium": "品牌搜索",
    // 【必填】广告来源
    "utmSource": "微博",
    // 广告内容
    "utmContent": "点击领取优惠券",
    // 广告关键字
    "utmTerm": "优惠券",
    // 备注
    "remarks": ""
}
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](/Users/yuan/Documents/gitbook/api/api.md#2.1%20项目接口认证)。

### 1.3 返回结果示例

```javascript
{
    "success":0, 
    // 广告跟踪唯一标识
     "campaignId": 124343232
}
```

### 1.4 接口调用示例

```javascript
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X POST --data '{
    "utmType": "H5",
    "pageUrl": "http://www.baidu.com",
    "utmCampaign": "xx平台广告",
    "utmMedium": "品牌搜索",
    "utmSource": "微博",
    "utmContent": "点击领取优惠券",
    "utmTerm": "优惠券",
    "remarks": ""
}' http://127.0.0.1:4005/ark/uba/api/operations/utm?username=admin
```

## 2. 获取广告跟踪

获取广告跟踪信息, 包括长短链

### 2.1 接口地址

> 【GET】 /ark/uba/api/operations/utm/{campaignId}

### 2.2 请求参数示例

```scala
//【必填】广告跟踪唯一标识 url路径参数
/ark/uba/api/operations/utm/124343232
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](/Users/yuan/Documents/gitbook/api/api.md#2.1%20项目接口认证)。

### 2.3 返回结果示例

```javascript
{
    "id": 4,
    "campaignId": "1591933676",
    "utmType": "eee",
    "callbackUrl": null,
    "pageUrl": "www.as.com",
    "utmCampaign": "",
    "utmMedium": "xcczx",
    "utmSource": "ew",
    "utmContent": "fdf",
    "utmTerm": "fvdfv",
    "checkUrl": null,
    "checkShort": null,
    "checkQrcode": null,
    "url": "www.as.com?campaign_id=1591933676&utm_campaign=&utm_medium=xcczx&utm_source=ew&utm_content=fdf&utm_term=fvdfv",
    "shortUrl": "utmJNOubA",
    "qrcode": null,
    "remarks": "s",
    "appKey": "apitestauto",
    "userId": -99,
    "status": 1,
    "createTime": 1618458179000,
    "updateTime": 1618486625000,
    "canOperated": 0,
    "createName": null,
    "loginName": null,
    "email": null,
    "phone": null,
    "uniqueSign": "1591933676",
    "batchNum": null,
    "project": null,
    "originUrl": null
}
```

### 2.4 接口调用示例

```javascript
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X GET  http://127.0.0.1:4005/ark/uba/api/operations/utm/124343232
```

## 3 查看转化效果‌

查询该广告跟踪的转化效果

### 3.1 接口地址

> 【POST】 /ark/uba/api/operations/utm/analysys/{campaignId}

### 3.2 请求参数示例

```javascript
{
    // 【必填】查询起始日期
    "fromDate": "2021-04-03",
    // 【必填】查询结束日期
    "toDate": "2021-04-12",
    "useCache": true,
    // 转化目标
    "expression":"event.login"
       
}
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](/Users/yuan/Documents/gitbook/api/api.md#2.1%20项目接口认证)。

### 3.3 返回结果示例

```javascript
{
  "success":0
}
```

### 3.4 接口调用示例

```javascript
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X POST --data '{
    "convertMeasures":[
        {
            "expression":"event.login",
            "aggregator":"TRIGGER_USER_COUNT"
        }
    ],
    "fromDate": "2021-04-03",
    "toDate": "2021-04-12",
    "useCache": true
}' http://127.0.0.1:4005/ark/uba/api/operations/utm/analysys/124343232
```

## 4. 删除广告跟踪‌

逻辑删除广告跟踪数据

### 4.1 接口地址

> 【DELETE】 /ark/uba/api/operations/utm/drop/{campaignId}

### 4.2 请求参数示例

```scala
//【必填】广告跟踪唯一标识 url路径参数
/ark/uba/api/operations/utm/drop/124343232
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](/Users/yuan/Documents/gitbook/api/api.md#2.1%20项目接口认证)。

### 4.3 返回结果示例

```javascript
{
  "success":0
}
```

### 4.4 接口调用示例

```javascript
curl -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X DELETE http://127.0.0.1:4005/ark/uba/api/operations/utm/drop/124343232
```

