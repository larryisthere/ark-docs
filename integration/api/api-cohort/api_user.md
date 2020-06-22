# 用户档案

## 1 单用户属性明细

根据用户ID获取用户属性数据。

### 1.1 接口地址

> 【POST】 /uba/api/cohort/users/{xwho}

{% hint style="info" %}
xwho 表示用户ID，取用户属性ID为xwho的值，传参方式：path variable，非request body
{% endhint %}

### 1.2 请求参数示例

```java
{
    //【选填】这里表示不支持，默认获取方舟系统所有可用的用户属性
    "properties":[]
}
```

> **properties**：指定需要的用户属性列，传入用户属性ID，可以通过方舟系统或者 [元数据管理](../api-manage-project/api-meta.md)-[用户属性](../api-manage-project/api-meta.md#1-huo-qu-yong-hu-shu-xing) 接口获取用户属性列表。不指定默认查询方舟系统【元数据管理 - 用户属性】中 可见 的所有用户属性。
>
> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。

### 1.3 返回结果示例

```java
{
        "$imei": null,
        "chuangke_topic": null,
        "$first_visit_language": "zh-cn",
        "$city": null,
        "$email": null,
        "$platform": "JS",
        "chuangke_region": null,
        "chuangke_createtime": null,
        "$country": null,
        "$first_visit_time": 1557403591272,
        "phone": null,
        "$province": null,
        "$signup_time": null,
        "$mac": null,
        "company_code": null,
        "xwho": "JSd650856937040a530ff54fdaab4e56d7d650",
        "email": null
    }
```

### 1.4 接口调用示例

```java
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X POST --data '{
    "properties":[]
}' http://127.0.0.1:4005/uba/api/cohort/users/JSd650856937040a530ff54fdaab4e56d7d650
```

## **2. 单用户行为序列**

根据用户ID查看单用户某段时间内的行为轨迹，按发生时间倒序，包含发生事件和对应事件属性。

此接口适用于查询小数据量的场景，一般建议不超过100条行为记录。如果需要导出或者获取更多行为，可通过 [单用户行为序列接口](api_user.md#3-dan-yong-hu-hang-wei-xu-lie-dao-chu) 获取。

### **2.1 接口地址**

> 【POST】 /uba/api/cohort/users/{xwho}/sequence

xwho 表示用户ID，取用户属性ID为xwho的值，传参方式：path variable

### **2.2 请求参数示例**

```java
{
    //【必填】 时间范围-开始时间
    "fromDate":"2019-06-18",
    //【必填】 时间范围-结束时间
    "toDate":"2019-06-20",
    //【选填】指定事件范围，对应事件ID，不传默认查询指定时间范围内的所有发生事件
    "events":[],
    //查看多少条行为记录 默认为100
    "limit":100,
    //使用缓存 这里表示是
    "useCache":true,
}
```

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。

### **2.3 返回结果示例**

```java
{
    //事件行为列表
    "rows": [
        {
            //事件ID
            "eventId": "$pageview",
            //事件发生时间
            "eventTime": "2020/02/12 21:08:38",
            //事件下的属性
            "properties": {
                "$browser_version": "Chrome 80.0.3987.100",
                "$utm_medium": null,
                "$network": null,
                "$os": "Mac OS X",
                "$platform": "JS",
                "$carrier_name": null,
                "$ip": "111.197.233.250",
                "project_name": "用户数据权限(勿删)",
                "$screen_height": 1050,
                "$referrer": "https://arkpaastest.analysys.cn:4005/project-management/member-management",
                "$user_agent": null,
                "$language": "zh",
                "$is_login": 1,
                "$province": "北京",
                "$url": "https://arkpaastest.analysys.cn:4005/project-management/role-management",
                "company": "企业管理",
                "company_code": null,
                "tag": null,
                "$device_type": null,
                "$traffic_source_type": "direct",
                "demo_type": null,
                "$os_version": "Mac OS X 10.15.3",
                "$search_engine": null,
                "$web_crawler": 0,
                "$is_first_day": 0,
                "$city": "北京",
                "$model": null,
                "$screen_width": 1680,
                "$channel": null,
                "$brand": null,
                "$utm_source": "direct",
                "$browser": "Chrome",
                "$url_domain": "https://arkpaastest.analysys.cn:4005/project-management/role-management",
                "$app_version": null,
                "$lib": "JS",
                "$country": "中国",
                "$utm_campaign": null,
                "$title": "易观方舟",
                "$utm_content": null,
                "$lib_version": "4.2.0.1",
                "$referrer_domain": "arkpaastest.analysys.cn",
                "$search_keyword": null,
                "$manufacturer": null,
                "$session_id": "14a851b749ab64ca"
            }
        }
    ],
    //查询时间
    "reportUpdateTime": "2020-02-14 15:33:54"
}
```

### **2.4 接口调用示例**

```javascript
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X POST --data '{
  "fromDate":"2020-02-06",
    "toDate":"2020-02-12",
    "events":[],
    "limit":100,
    "useCache":true
}' http://127.0.0.1:4005/uba/api/cohort/users/JSd650856937040a530ff54fdaab4e56d7d650/sequence
```

## 3 单用户行为序列导出

5.0版本中新增

适用于需要导出或者获取更多\(百万级以下\)的用户行为轨迹场景，返回接口使用的是流式输出。

### 3.1 接口地址

> 【POST】 /uba/api/cohort/users/{xwho}/sequence/export

xwho 表示用户ID，取用户属性ID为xwho的值，传参方式：path variable

### 3.2 请求参数

```java
{
    //【必填】 时间范围-开始时间
    "fromDate":"2019-06-18",
    //【必填】 时间范围-结束时间
    "toDate":"2019-06-20",
    //【选填】指定事件范围，对应事件ID，不传默认查询指定时间范围内的所有发生事件
    "events":[],
    //使用缓存 这里表示是
    "useCache":true,
}
```

> 认证参数：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。

### 3.3 返回结果示例

```javascript
{"$app_version":"(已脱敏)",......,"$os_version":"Mac OS X 10.15.2","xwhat":"$pageview","$lib":"JS"}
{"$app_version":"(已脱敏)",......,"$os_version":"Mac OS X 10.15.2","xwhat":"$search","$lib":"JS"}
{"$app_version":"(已脱敏)",......,"$os_version":"Mac OS X 10.15.2","xwhat":"$click","$lib":"JS"}
```

> 1. 因为可以支撑大数据量，为了方便客户端可以进行批次处理，json类型的输出并非是一个完整的json数组，而是一行一条json（无`[]`,按`\n`分割）
> 2. 因为可以支撑大数据量，接口response为 流式输出。如果是通过程序来调用，那么建议：
>    * 建议一：避免一次性加载response到内存中，改为流式接收 response body
>    * 建议二：`Http Connection` 需要增加`SocketTimeout`时长，同时修改nginx 超时配置
> 3. 关于流式导出类型API的java 客户端调用示例可以参考 [【API-自定义sql查询】](../analytics/api-sql-query.md)的第2.5章节

### 3.4 接口调用示例

导出`xwho=JS7daf4d2bfa06b1723346b884ce986f8a7daf`的3月6日到16日之间的所有行为数据，输出写入到output.json 文件中

```haskell
curl -o output.json -H "Content-Type:application/json" -H "token:3edbaf427ecdda80beef322ad3c333a4" -H "appKey:31abd9593e9983ec" -X POST --data '{
  "fromDate":"2020-03-06",
    "toDate":"2020-03-16",
    "events":[],
    "useCache":true
}' https://127.0.0.1:4005/uba/api/cohort/users/JS7daf4d2bfa06b1723346b884ce986f8a7daf/sequence/export
```

关于流式导出类型API的java 客户端调用示例可以参考 [【API-自定义查询-Java HttpClient 接口调用示例](../api-analytics/api_sql_query.md#25-java-httpclient-jie-kou-tiao-yong-shi-li)】

