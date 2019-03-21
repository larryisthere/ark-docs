---
description: 方舟不仅能够在产品的分析界面上下载查询的明细结果，还提供从平台中导出和获取数据的方案满足客户需求。
---

# 数据导出

**直接从Kafka中订阅数据**

* 适用于有实时计算实时获取数据的场景
* 可用于方舟私有部署的单机版和集群版
* 用于实时获取最细粒度的用户行为和属性数据

**使用JDBC进行数据访问**

* 适用于有通过 JDBC 协议与其它系统打通的应用场景
* 相比直接从Kafka中订阅数据可满足数据中部分字段的查询和导出

## 1. 订阅实时数据

订阅实时数据功能，涉及较多的技术细节，适用于对相关功能有经验的用户参考。如果对文档内容有疑惑，请及时联系工作人员。

方舟作为开放的架构，可以通过订阅实时数据来满足更多使用场景。服务端接到一条 SDK 发来的数据后，会对数据做一些预处理并将数据写入到消息队列 Kafka 中供下游各类计算模块使用。

### 1.1 要求

* 启动订阅的机器需与部署方舟的机器在同一个内网，且必须可以解析方舟服务器的 host。
* 请选用兼容的 Kafka 客户端版本，高版本服务端兼容低版本客户端，反之则可能存在兼容性问题。方舟 Kafka 服务端版本为0.8.7，具体情况可在服务器上查看。

### 1.2 订阅参数

| 参数名称 | 参数值 |
| :--- | :--- |
| topic | event_{appid}/profile_{appid}\(其中{appid}表示项目的appid\) |
| partition | partitionid\(从0开始，至少3个partition） |
| zookeeper | ark1:2181,ark2:2181,ark3:2181 |
| broker | ark1:9092,ark2:9092, ark3:9092 |

### 1.3 订阅数据

订阅有shell、原生API等多种方式，可以选择一种适合使用场景的方式。

下面给出两种 Shell 方式启动订阅的示例，使用 Shell 方式可以通过重定向标准输出将数据写入文件后处理或直接用管道作为其他进程的输入，可以对接各种编程语言实现的处理程序。

**1. 使用 Kafka Console Consumer**

* 可以使用 Kafka 自带的 Kafka Console Consumer 通过命令行方式订阅，例如从最新数据开始订阅：

  `bin/kafka-console-consumer.sh --zookeeper ark1:2181 --topic event_topic`

* 可以将 stdout 输出到文件或作为其他数据处理进程的输入数据。

**2. 使用 Simple Consumer Shell**

* 使用 Simple Consumer Shell 可以实现更灵活的订阅，例如：

```bash
bin/kafka-run-class.sh kafka.tools.SimpleConsumerShell \
        --broker-list ark2:9092         \
        --offset 1234                         \
        --partition 2                          \
        --topic event_topic                    \
        --print-offsets
```

### 1.4 数据格式

订阅的数据的格式与导入时的[数据格式](../integration/integration-prepare/integration-data-type.md)基本一致。

## 2. 使用JDBC进行数据访问

使用JDBC进行数据访问涉及较多的技术细节，适用于对相关功能有经验的用户参考。如果对文档内容有疑惑，请及时联系工作人员。

方舟不仅提供 Kafka订阅数据的方式，还提供了更加高效、稳定的 SQL 查询方式，即直接使用 JDBC 或者 presto-cli 进行数据查询。关于具体如何使用 JDBC 连接 Presto 可以直接参考官方文档。

### 2.1 JDBC

* jdbc 信息

| 字段 | 信息 |
| :--- | :--- |
| jdbc url | jdbc:presto://ark2.analysys.xyz:8285/hive/default |
| driver | com.facebook.presto.jdbc.PrestoDriver |
| username | streaming |
| password | \(密码为空\) |

* 如果使用代码访问，我们建议使用 0.170 版本的 Presto JDBC Driver 来进行访问，Maven 的依赖定义如下：

```bash
<dependency>
    <groupId>com.facebook.presto</groupId>
    <artifactId>presto-jdbc</artifactId>
    <version>0.170</version>
</dependency>
```

### 2.2 使用 presto-shell 进行查询

除了直接使用 JDBC 接口之外，也可以直接适用 presto-shell 工具进行查询。

* 直接登陆任意的方舟服务器，运行以下命令即可：

```bash
bin/presto-cli --server ark2:8285
```

### 2.3 presto-shell常规使用

* 查询用户信息数据

-- 查看用户信息表的表结构

```bash
desc chbase.db_appid.profile
```

-- 查询用户信息数据

```bash
select * from chbase.db_appid.profile limit 10
```

其中{appid}为要查询的appid

* 查询用户行为数据

-- 查看用户行为表的表结构

```bash
desc hive.db_appid.event_vd
```

-- 查询用户行为数据

```bash
select * from hive.db_appid.event_vd limit 10
```

其中{appid}为要查询的appid

### 2.4 数据导出

* 导出不包含表头的CSV格式的文件

```bash
presto-cli --server ark2:8285 --execute 'select * from hive.db_{appid}.event_vd limit 10' --output-format CSV > dataFile.CSV
presto-cli --server ark2:8285 --execute 'select * from chbase.db_{appid}.profile limit 10' --output-format CSV > dataFile.CSV
```

{appid}为要查询项目的appid

dataFile.CSV为导出的文件路径

* 导出包含表头的CSV格式的文件 

```bash
presto-cli --server ark2:8285 --execute 'select * from hive.db_{appid}.event_vd limit 10' --output-format CSV_HEADER > dataFile.CSV_HEADER
presto-cli --server ark2:8285 --execute 'select * from chbase.db_{appid}.profile limit 10' --output-format CSV_HEADER > dataFile. CSV_HEADER
```

{appid}为要查询项目的appid

dataFile.CSV为导出的文件路径

数据第一行为表头

* 导出不包含表头的TSV格式的文件–没有表头

```bash
presto-cli --server ark2:8285 --execute 'select * from hive.db_{appid}.event_vd limit 10' --output-format TSV > dataFile.TSV
presto-cli --server ark2:8285 --execute 'select * from chbase.db_{appid}.profile limit 10' --output-format TSV > dataFile. TSV
```

{appid}为要查询项目的appid

dataFile.CSV为导出的文件路径

字段间以tab键分割

* 导出包含表头的TSV格式的文件

```bash
presto-cli --server ark2:8285 --execute 'select * from hive.db_{appid}.event_vd limit 10' --output-format TSV_HEADER > dataFile.TSV_HEADER
presto-cli --server ark2:8285 --execute 'select * from chbase.db_{appid}.profile limit 10' --output-format TSV_HEADER > dataFile.TSV_HEADER
```

{appid}为要查询项目的appid

dataFile.CSV为导出的文件路径

数据第一行为表头，字段间以tab键分割

* 其他格式，请参考 presto-cli --help 命令中--output-format 选项的说明。

[![ ](https://imguserradar.analysys.cn/fangzhou/img/2019/01/201901151711159657.jpeg)](https://ark.analysys.cn/view/sign/signup.html?campaign_id=2111486795&utm_campaign=文档注册&utm_medium=自媒体&utm_source=文档&utm_content=&utm_term=)

