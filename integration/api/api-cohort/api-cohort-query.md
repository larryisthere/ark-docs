# 分群查询

## 1. 分群的用户明细查询

获取某个分群下的用户明细。

> 更新记录：4.3.4版本新增，4.6版本中增加分页功能
>
> 适合单批次获取小量级数据量（根据内存限定，一般为1万），如果需要获取或者导出更多，在5.0版本中，可选用 [分群的用户用户明细导出](api-cohort-query.md#2-fen-qun-de-yong-hu-ming-xi-dao-chu) 。

### 1.1 接口地址

> 【POST】  /uba/api/cohort/users

### 1.2 请求参数示例

{% tabs %}
{% tab title="4.3.4版本" %}
获取单个分群的用户属性数据

```java
{
    // 【必填】分群code
    "cohortCode":"arkfq_3",
    //需要获取的用户条数
    "limit":2,
    //【选填】指定需要查询的用户属性列，传用户属性ID
    "properties":["xwho", "distinct_id", "$imei", "$first_visit_language", "$signup_time"]
}
```

> **limit**：获取用户分群下的用户条数，默认为1000。
>
> **properties**：指定需要的用户属性列，传入用户属性ID，可以通过方舟系统或者 [元数据管理](../api-manage-project/api-meta.md)-[用户属性](../api-manage-project/api-meta.md#1-huo-qu-yong-hu-shu-xing) 接口获取用户属性列表。不指定默认查询方舟系统【元数据管理 - 用户属性】中 可见 的所有用户属性。

> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。
{% endtab %}

{% tab title="4.6版本" %}
 增加page和pageSize两个参数，用来支持分页查询。

```java
{
    // 【必填】分群code
    "cohortCode":"arkfq_3",
    //需要获取的用户条数
    "limit":2,
    //【选填】指定需要查询的用户属性列，传用户属性ID
    "properties":["xwho", "distinct_id", "$imei", "$first_visit_language", "$signup_time"],
  	//【选填】当前页，从1开始，不需要分页不用传值
  	"page":1
    //【选填】每页大小，和page配合使用，值不能大于limit
  	"pageSize":1000，
}
```

> **limit**：获取用户分群下的用户条数，无分页参数时默认为1000。有分页时默认为分群用户数，如果指定limit值会影响总页数和最终返回的结果集。
>
> **properties**：指定需要的用户属性列，传入用户属性ID，可以通过方舟系统或者 [元数据管理](../api-manage-project/api-meta.md)-[用户属性](../api-manage-project/api-meta.md#1-huo-qu-yong-hu-shu-xing) 接口获取用户属性列表。不指定默认查询方舟系统【元数据管理 - 用户属性】中 可见 的所有用户属性。
>
> **page**：当前页数。page为空表示不分页，page有值时取指定页结果，页数从1开始。
>
> **pageSize**：每页条数。page有值时，pageSize不能为空，且值不能大于limit。
>
> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。
{% endtab %}

{% tab title="5.2版本" %}
5.2可支持获取用户的标签属性

```haskell
{
    // 【必填】分群code
    "cohortCode":"arkfq_3",
    //需要获取的用户条数
    "limit":2,
    //【选填】指定需要查询的用户属性列，传用户属性ID
    "properties":["xwho", "distinct_id", "$imei", "$first_visit_language", "$signup_time"],
  	//【4.6中新增】【选填】当前页，从1开始，不需要分页则不用传值
  	"page":1
    //【4.6中新增】【选填】每页大小，和page配合使用，值不能大于limit
  	"pageSize"：1000，
    //【选填】5.2版本新增，要获取的标签code，如果不指定则不获取任何标签内容
  	"tags":[
      	//标签参数包含code和version两个部分，version可以不指定，不指定时默认获取最新标签
        {"code":"tag_3"},
        {"code":"tag_4"},
        {"code":"tag_6"}
    ]
}
```
{% endtab %}
{% endtabs %}

### 1.3 返回结果示例

```java
{
    //分群下总用户条数
    "count":46,
    //【4.6中新增】当前页
    "page": 1,
    //结果返回的用户条数
    "size":2,
    //用户详情
    "users":[
        {
            "$imei":null,
            "$first_visit_language":"zh-cn",
            "distinct_id":4255875062385414000,
            "$signup_time":null,
            "xwho":"JSd650856937040a530ff54fdaab4e56d7d650"
        },
        {
            "$imei":null,
            "$first_visit_language":"zh-cn",
            "distinct_id":-4635244755532264000,
            "$signup_time":null,
            "xwho":"JS1c6d11e3e67bf0dc02030d3fd393310b1c6d",
            //tag.开头的为标签数据，指定标签code才会查询
            "tag.tag_3":"高消费"
        }
    ]
}
```

### 1.4 接口调用示例

```java
 curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X POST --data '{
    "cohortCode":"arkfq_3",
    "limit":2,
    "properties":["xwho", "distinct_id", "$imei", "$first_visit_language", "$signup_time"]
}' http://127.0.0.1:4005/uba/api/cohort/users
```

## 2. 分群的用户明细导出

5.0版本中新增

适用于需要一次性导出或者获取更多\(百万级以下\)的分群用户明细数据的场景，返回接口使用的是流式输出。

### 2.1 接口地址

> 【POST】 /uba/api/cohort/users/export

### 2.2 请求参数

```java
{
    // 【必填】分群code
    "cohortCode":"arkfq_3",
    //需要获取的用户条数，不传指获取全部
    "limit":10000,
    //【选填】指定需要查询的用户属性列，传用户属性ID
    "properties":["xwho", "distinct_id", "$imei", "$first_visit_language", "$signup_time"],
}
```

> **limit**：获取用户分群下的用户条数，可传可不传，不传为全部
>
> **properties**：指定需要的用户属性列，传入用户属性ID，可以通过方舟系统或者 [元数据管理-用户属性](../api-manage-project/api-meta.md#1-huo-qu-yong-hu-shu-xing) 接口获取用户属性列表。不指定默认查询方舟系统【元数据管理 - 用户属性】中 可见 的所有用户属性。
>
> **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。

### 2.3 返回结果示例

```javascript
{"$city":"BJ","$email":"(已脱敏)","$platform":"JS","xwho":"JSa0438f992d07a31d9f079ca479cd4796a043"}
{"$city":"CS","$email":"(已脱敏)","$platform":"JS","xwho":"JS01cb9f5096f452a10b03e90b0694dee401cb"}
{"$city":"SH","$email":"(已脱敏)","$platform":"JS","xwho":"JS9e931e93f9674491b77eba3103e638cf9e93"}
{"$city":"SZ","$email":"(已脱敏)","$platform":"JS","xwho":"JS7b4dc11ab426603295ab11811d69797a7b4d"}
{"$city":null,"$email":"(已脱敏)","$platform":"JS","xwho":"JS72889204a97a39e06a220c1aa3b4fdbd7288"}
{"$city":"GZ","$email":"(已脱敏)","$platform":"JS","xwho":"JS9a99fc0dcebf31b7a75f349cac6cd2c09a99"}
```

> 1. `行为序列导出` 接口输出，因为可以支撑大数据量，为了方便客户端可以进行批次处理，json类型的输出并非是一个完整的json数组，而是一行一条json（无`[]`,按`\n`分割）
> 2. `行为序列导出`接口输出，因为可以支撑大数据量，接口response为 流式输出。如果是通过程序来调用，那么建议：
>    * 建议一：避免一次性加载response到内存中，改为流式接收 response body
>    * 建议二：`Http Connection` 需要增加`SocketTimeout`时长，同时修改Nginx 超时配置

### 2.4 接口调用示例

导出人群为`arkfq_98`的所有用户，输出写入到output.json 文件中

```haskell
 curl -o output.json -H "Content-Type:application/json" -H "token:3edbaf427ecdda80beef322ad3c333a4" -H "appKey:31abd9593e9983ec" -X POST --data '{
    "cohortCode":"arkfq_98",
    "properties":["xwho","$email","$platform","$city"]
}' https://127.0.0.1:4005/uba/api/cohort/users/export
```

关于流式导出类型API的java 客户端调用示例可以参考 [【API-自定义查询-Java HttpClient 接口调用示例](../api-analytics/api_sql_query.md#25-java-httpclient-jie-kou-tiao-yong-shi-li)】

