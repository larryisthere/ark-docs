# 工具导出

使用工具导出的文件内容格式和方舟定义的标准 [数据格式](../prepare/data-type.md) 一致，可直接用于 标准数据导入 工具导入方舟。

## 1. 介绍说明

导出方舟项目数据到本地，包括Hbase中的用户数据和Hive中的事件数据。

## 2. 运行环境

导入工具需要在JAVA环境中运行，单机版和集群版都可以使用。

{% hint style="info" %}
1、导入工具需要在方舟部署服务器/可访问方舟服务器上的机器上使用；

2、为了避免网络异常、数据传输速度等造成数据导入慢等问题，**建议**数据导出工作在**方舟**部署的任一**服务器上**使用。
{% endhint %}

## 3. 使用方法

### 3.1  下载

如果你是单机版，请 [下载单机版导出工具](https://imguserradar.analysys.cn/files/ark/tool/ExportData4Json-StandAlone.tar.gz)，如果你是集群版，请 [下载集群版导出工具](https://imguserradar.analysys.cn/files/ark/tool/ExportData4Json-Cluster.tar.gz)。

### 3.2 解压

导出工具是一个压缩包，在下载成功后，需要在对应服务器上进行解压：

> **解压参考命令**：tar -zxf xxx.tar.gz
>
> **解压单机版**：tar -zxf ExportData4Json-StandAlone.tar.gz
>
> **解压集群**版：tar -zxf ExportData4Json-Cluster.tar.gz

目录显示如下：

```text
├── bin
│   ├── shutdown.sh
│   └── startup.sh
├── conf
│   └── data-exporter.properties
├── jars
│   └── data-exporter-jar-with-dependencies.jar
└── logs
```

> **conf**：项目信息、服务器信息、文件导出目录等相关配置文件。
>
> **logs**：在启动、运行、终止等操作时才会有文件产生，刚下载时目录为空。

### 3.3 配置文件

在导出前需要修改配置文件conf/data-exporter.properties中的内容，未在文档中特殊说明的配置默认不需要修改。

> **1）常用参数说明**
>
> **appkey**=test001;test001：项目appKey，对应**table参数**中的表，用分号隔开，如需要同时导出事件和用户属性，appKey需要填写两次。
>
> **sink.json.path=**/data1/test1/event;/data1/test1/profile：导出数据的存放目录，目录为绝对路径，包含事件数据和用户数据两部分，两个路径之间用分号隔开；
>
> **source.day=**19700101-20200101：导出数据的时间范围，日期格式为yyyyMMdd；
>
> **jobId=**job\_0001：数据导出工具JobId，单次导出为一个Job，每次导出换一个名字。
>
> **2）不常用参数说明**
>
> **debug=**true：是否打印 DEBUG级别的日志；
>
> **table=**event\_vd;profile：对应的表名，事件表名为event\_vd，用户属性表名为profile；
>
> **limit=**25000：单次从数据库查询的数据量，可根据服务器性能和导出速度进行调整；
>
> **sink.json.prefi**x=event;profile：导出的文件命名前缀，事件为event，用户属性为profile。

{% hint style="info" %}
导出的数据文件在 **sink.json.path** 对应的路径下，如果路径不存在，工具包会自动创建。
{% endhint %}

### 3.4 开始导出

#### 3.4.1 数据导出工具根目录

进入工具文件夹根目录，如工具包是解压在 /data1目录下：cd  /data1/ExportData4Json。

#### 3.4.2 启动导出

执行启动脚本：sh bin/startup.sh。

### 3.5 停止导出

执行停止脚本：sh bin/shutdown.sh。

## 4. 常见问题

### 4.1 导出数据丢失

如果导出未成功或者数据丢失，可以进入通过查看 **logs/error.log** 文件查看失败的原因。



