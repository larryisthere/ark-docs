---
description: >-
  方舟不仅提供 Kafka订阅数据的方式，还提供了更加高效、稳定的 SQL 查询方式，即直接使用 JDBC、presto-cli、python 或 R
  进行数据查询。
---

# 使用程序访问数据库

{% hint style="info" %}
使用程序直接进行数据访问涉及较多的技术细节，适用于对相关功能有经验的用户参考。如果对文档内容有疑惑，请及时联系工作人员。
{% endhint %}

## JDBC

#### JDBC 信息

| 字段 | 信息 |
| :--- | :--- |
| jdbc url | jdbc:presto://ark2.analysys.xyz:8285/hive/default |
| driver | com.facebook.presto.jdbc.PrestoDriver |
| username | streaming |
| password | \(初始密码为空\) |

presto-jdbc.jar的下载地址：[https://jar-download.com/artifacts/com.facebook.presto/presto-jdbc/0.201](https://jar-download.com/artifacts/com.facebook.presto/presto-jdbc/0.201)

如果使用代码访问，可以直接通过maven添加presto-jdbc的依赖，Maven 的依赖定义如下：

```markup
<dependency>
    <groupId>com.facebook.presto</groupId>
    <artifactId>presto-jdbc</artifactId>
    <version>0.201</version>
</dependency>
```

## presto-shell

除了直接使用 JDBC 接口之外，也可以直接适用 presto-shell 工具进行查询。

直接登陆任意的方舟服务器，运行以下命令即可：

```bash
#presto必须使用isuhadoop用户来连接
su - isuhadoop
bin/presto-cli --server ark2:8285
```

### 查询用户信息数据

#### 查看用户信息表的表结构

```bash
desc chbase.db_appid.profile
```

#### 查询用户信息数据

```bash
select * from chbase.db_appid.profile limit 10
```

其中{appid}为要查询的appid

### 查询用户行为数据

#### 查看用户行为表的表结构

```bash
desc hive.db_appid.event_vd
```

#### 查询用户行为数据

```bash
select * from hive.db_appid.event_vd limit 10
```

其中{appid}为要查询的appid

### 数据导出

#### 导出不包含表头的CSV格式的文件

```bash
presto-cli --server ark2:8285 --execute 'select * from hive.db_{appid}.event_vd limit 10' --output-format CSV > dataFile.CSV
presto-cli --server ark2:8285 --execute 'select * from chbase.db_{appid}.profile limit 10' --output-format CSV > dataFile.CSV
```

{appid}为要查询项目的appid

dataFile.CSV为导出的文件路径

#### 导出包含表头的CSV格式的文件 

```bash
presto-cli --server ark2:8285 --execute 'select * from hive.db_{appid}.event_vd limit 10' --output-format CSV_HEADER > dataFile.CSV_HEADER
presto-cli --server ark2:8285 --execute 'select * from chbase.db_{appid}.profile limit 10' --output-format CSV_HEADER > dataFile. CSV_HEADER
```

{appid}为要查询项目的appid

dataFile.CSV为导出的文件路径

数据第一行为表头

#### 导出不包含表头的TSV格式的文件–没有表头

```bash
presto-cli --server ark2:8285 --execute 'select * from hive.db_{appid}.event_vd limit 10' --output-format TSV > dataFile.TSV
presto-cli --server ark2:8285 --execute 'select * from chbase.db_{appid}.profile limit 10' --output-format TSV > dataFile. TSV
```

{appid}为要查询项目的appid

dataFile.CSV为导出的文件路径

字段间以tab键分割

#### 导出包含表头的TSV格式的文件

```bash
presto-cli --server ark2:8285 --execute 'select * from hive.db_{appid}.event_vd limit 10' --output-format TSV_HEADER > dataFile.TSV_HEADER
presto-cli --server ark2:8285 --execute 'select * from chbase.db_{appid}.profile limit 10' --output-format TSV_HEADER > dataFile.TSV_HEADER
```

{appid}为要查询项目的appid

dataFile.CSV为导出的文件路径

数据第一行为表头，字段间以tab键分割

其他格式，请参考 presto-cli --help 命令中--output-format 选项的说明。

## Python

如果你更熟悉 python，也可以通过 [presto-python-client](https://pypi.org/project/presto-python-client/) 来对数据库进行访问。

### 连接方法

```python
import prestodb
conn=prestodb.dbapi.connect(
        host='ip address of ark2',
        port=8285,
        user='streaming',
        catalog='hive',
        schema='db_{appkey}',
)
cur = conn.cursor()
cur.execute('SELECT * FROM event_vd limit 10')
rows = cur.fetchall()
print rows

```

## R

可以尝试使用开源项目 [https://github.com/prestodb/RPresto](https://github.com/prestodb/RPresto) 来对数据库进行访问。

