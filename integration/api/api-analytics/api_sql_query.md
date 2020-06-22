---
description: 方舟5.0版本中新增API
---

# 自定义查询

## 1.自定义SQL查询

适合查询结果为小数据量的场景\(千级数据量\)，例如分析结果统计，查询数据状态；如需导出更大规模数据，见【章节2.[自定义SQL导出](api_sql_query.md#2-zi-ding-yi-sql-dao-chu)】

### 1.1 接口地址

> 【POST】 /uba/api/sql/query

### 1.2 请求参数

#### 1.2.1 Body参数说明：

**body参数示例：**

```java
{
    "sql": "SELECT xwhat,xwhen,xwho FROM event_vd as event  where event.ds=20200214 and event.\"$startup_time\" is not null LIMIT 10",
    "format": "json"
}
```

**body参数说明：**

* sql: 查询的 SQL，例如 `SELECT xwhat,xwhen,xwho FROM event_vd LIMIT 10`。
* format: 文本输出可选项包括
  * csv：默认格式，sql查询结果输出为csv格式文件。默认文件编码为`UTF-8`，若导出文件要用excel打开，增加url参数`?csvUtfEncoding=0`
  * json：sql查询结果输出为一个完整JSON 的json文件

#### 1.2.2 特殊说明：

1. 可供查询的表有：事件表`event_vd`，属性表`profile_vd`。
2. 事件字段、事件属性字段、用户属性字段信息可以参见 API章节中的[ 元数据管理](../api-manage-project/api-meta.md)。
3. 事件或属性字段中如果包含`$`符号的则需要对该字段加上双引号，双引号也需要转义，例如`\"$startup_time\"`。
4. **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。
5. 表名可以不附带数据库名，API程序会根据认证参数，自动为表名加上数据库名，如果自带不符合认证校验的数据库名，接口将返回参数异常，提示包含非当前项目的数据库表
6. 不支持数据操纵语言DML、数据定义语言DDL以及数据控制语言DCL，只支持数据查询语言DQL（基本结构是由SELECT子句，FROM子句，WHERE 子句组成的查询块）
7. API程序默认开启 `limit` 检测，查询记录数不能超过系统配置的参数项`arkweb.open_api.sqlquery.limit`，该参数可以通过`ambari管理平台`进行变更设置，设置为`-1`表示不限制查询记录条数

### 1.3 返回结果示例

**format为 csv类型**

```java
xwhen,xwhat,xwho,
1581648715861,$pageview,JS4a8bf500c4f4aadfdf3a43586d9008fd4a8b,
1581648718542,$pageview,JS4a8bf500c4f4aadfdf3a43586d9008fd4a8b,
1581648726163,$pageview,JS4a8bf500c4f4aadfdf3a43586d9008fd4a8b,
```

**format为 ‌json类型**

```javascript
[
    {
        "xwhen": 1581648715861,
        "xwhat": "$pageview",
        "xwho": "JS4a8bf500c4f4aadfdf3a43586d9008fd4a8b"
    },
    {
        "xwhen": 1581648718542,
        "xwhat": "$pageview",
        "xwho": "JS4a8bf500c4f4aadfdf3a43586d9008fd4a8b"
    },
    //....
]
```

### **1.4 接口调用示例**

```haskell
curl -H "Content-Type:application/json" -H "token:4113c9cad1c301113783f433e254888c" -H "appKey:31abd9593e9983ec" -X POST --data '{
    "sql": "SELECT xwho,xwhat,xwhen,\"$browser\" FROM event_vd as event  where event.ds=20200214 and event.\"$startup_time\" is not null LIMIT 2",
    "format": "csv"
}' http://127.0.0.1:4005/uba/api/sql/query
```

## 2. 自定义SQL导出

适合sql查询结果为较大数据量的场景\(百万级数据量\)，例如导出事件数据，导出用户数据

### 2.1 接口地址

> 【POST】 /uba/api/sql/export

### 2.2 请求参数

#### 2.2.1 Body参数说明：

**body参数示例：**

```java
{
    "sql": "SELECT xwhat,xwhen,xwho FROM event_vd as event  where event.ds=20200214 and event.\"$startup_time\" is not null LIMIT 10",
    "format": "json"
}
```

**body参数说明：**

* sql: 查询的 SQL，例如 `SELECT xwhat,xwhen,xwho FROM event_vd LIMIT 10`。
* format: 文本输出可选项包括
  * csv：默认格式，sql查询结果输出为csv格式文件。默认文件编码为`UTF-8`，若导出文件要用excel打开，增加url参数`?csvUtfEncoding=0`
  * json：sql查询结果输出为**每行一个JSON** 的json文件

#### 2.2.2 特殊说明：

1. 可供查询的表有：事件表`event_vd`，属性表`profile_vd`。
2. 事件字段、事件属性字段、用户属性字段信息可以参见 API章节中的 [元数据管理](../api-manage-project/api-meta.md)。
3. 事件或属性字段中如果包含`$`符号的则需要对该字段加上双引号，双引号也需要转义，例如`\"$startup_time\"`。
4. **认证参数**：接口必传token和appKey两个参数，详情见 [项目接口认证](../#21-xiang-mu-jie-kou-ren-zheng)。
5. 表名可以不附带数据库名，API程序会根据认证参数，自动为表名加上数据库名，如果自带不符合认证校验的数据库名，接口将返回参数异常，提示包含非当前项目的数据库表
6. 不支持数据操纵语言DML、数据定义语言DDL以及数据控制语言DCL，只支持数据查询语言DQL（基本结构是由SELECT子句，FROM子句，WHERE 子句组成的查询块）
7. 该接口不对sql 进行limit 限制，但受限于查询时长，默认在10分钟超时时间内，经过测试该API能够导出200万左右条记录（仅供参考，不同机器配置和查询列数差异会导致可导出记录数存在一定的偏差）

### 2.3 返回结果示例

**format为 csv类型的输出举例：**

```java
xwhen,xwhat,xwho,
1581648715861,$pageview,JS4a8bf500c4f4aadfdf3a43586d9008fd4a8b,
1581648718542,$pageview,JS4a8bf500c4f4aadfdf3a43586d9008fd4a8b,
1581648726163,$pageview,JS4a8bf500c4f4aadfdf3a43586d9008fd4a8b,
```

**format为 json类型输出举例：**

```javascript
{"xwhen":1581648726045,"$browser":"Chrome","xwho":"JS4a8bf500c4f4aadfdf3a43586d9008fd4a8b","xwhat":"login"}
{"xwhen":1581650573221,"$browser":"Chrome","xwho":"a5qvKY6K1000047yuanyw@analysys.com.cn","xwhat":"login"}
{"xwhen":1581678914730,"$browser":"Firefox","xwho":"JS2db3b546aac1fd04f54d29a574a186042db3","xwhat":"login"}
{"xwhen":1581678914730,"$browser":"Firefox","xwho":"JS2db3b546aac1fd04f54d29a574a186042db3","xwhat":"login"}
{"xwhen":1581651522435,"$browser":"Chrome","xwho":"JS8ae9e9bf73b5b0dc3e42c8d31dd8078a8ae9","xwhat":"login"}
{"xwhen":1581669615438,"$browser":"Firefox","xwho":"JS4d372a84aa14b9279c0c35db28c3cb544d37","xwhat":"login"}
```

**输出结果备注：**

> 1. `/sql/export` 接口输出，因为可以支撑大数据量，为了方便客户端可以进行批次处理，json类型的输出并非是一个完整的json数组，而是一行一条json（无`[]`,按`\n\`分割）
> 2. `/sql/export`接口输出，因为可以支撑大数据量，接口response为 流式输出。如果是通过程序来调用，那么建议：
>    * 避免一次性加载response到内存中，改为流式接收 response body
>    * `Http Connection` 需要增加`SocketTimeout`时长，如果下载过程出现504 网关超时，则需要修改nginx 超时配置

### 2.4 接口调用示例

导出日期为20200214的所有事件数据，输出写入到output.json 文件中

```haskell
curl -o output.json -H "Content-Type:application/json" -H "token:3edbaf427ecdda80beef322ad3c333a4" -H "appKey:31abd9593e9983ec" -X POST --data '{
    "sql": "SELECT xwho,xwhat,xwhen,\"$browser\" FROM event_vd as event  where event.ds=20200214",
    "format": "json"
}' http://127.0.0.1:4005/uba/api/sql/export
```

### 2.5 Java HttpClient 接口调用示例

```java
   public void testSqlExport(){
        // 依赖 org.apache.httpcomponents 的 httpclient 包
				static String UTF = "UTF-8";        
				CloseableHttpClient client = HttpClients.createDefault();
        HttpPost httpPost = new HttpPost("http://127.0.0.1:4005/uba/api/sql/export");
        //Header 参数
        httpPost.setHeader("Content-Type", "application/json");
        httpPost.setHeader("appKey", YOUR_APP_KEY);
        httpPost.setHeader("token", YOUR_APP_TOKEN);
        // post body params
        Map<String,Object> params = new HashMap<>();
        params.put("format", "json");
        params.put("sql", "select *  from profile_vd where arkfq_5=1");
        ObjectMapper objectMapper = new ObjectMapper();
        StringEntity stringEntity = new StringEntity(objectMapper.writeValueAsString(params), UTF);
        stringEntity.setContentEncoding(UTF);
        httpPost.setEntity(stringEntity);

        //set timeout 
        RequestConfig reqConfig = RequestConfig.custom()
                .setSocketTimeout(10*60*1000).setConnectTimeout(60*1000).build();
        httpPost.setConfig(reqConfig);
        //假设存到 output.txt（也可以是其他处理方式）
        File dir = new File("/tmp");
        File file = new File(dir, "output.txt");
        try (CloseableHttpResponse response1 = client.execute(httpPost);
             FileOutputStream fos = new FileOutputStream(file)) {
            final HttpEntity entity = response1.getEntity();
            if (entity != null) {
                try (InputStream inputStream = entity.getContent()) {
                    // TODO: do something useful with the stream
                    try (BufferedReader br = new BufferedReader(new InputStreamReader(inputStream))) {
                        String line;
                        while ((line = br.readLine()) != null) {
                            // TODO: process the line.
                            // 以文件存储为例，按行读取，按行写入
                            fos.write((line+"\n").getBytes(UTF));\
                        }
                    }
                }
                EntityUtils.consume(entity);
            }

        }
    }
```

