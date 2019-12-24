# 4.3.4及以上版本适用

导入工具主要是将历史数据或者外部数据转化为标准格式后导入方舟系统。

使用导入工具的文件内容格式需要符合 [数据格式](../../prepare/data-type.md) ，更多详情请阅读 [数据模型](../../prepare/data-model.md)。

## 1. 介绍说明

* 多线程的方式，读取导入工具data目录中的.json格式文件，导入程序分别将事件数据导入方舟事件数据Topic、用户数据导入方舟用户数据Topic；
* 实现断点续转功能，保证数据不丢失，不重复；
* 支持动态添加源数据文件到源数据目录，实时获取新添加的文件，导入文件数据。

{% hint style="danger" %}
1、单个文件同时只能由一个线程读取。

2、文档中都是以集群版为例切换机器或连接服务，如果是**单机版**，将对应的ark2/ark3**改为ark1**即可。

3、在 [3.7 数据验证](equal4.3.4orafter.md#37-shu-ju-yan-zheng) 的时候用到了 **$importFlag** 属性，如果需要用此方式验证，在每条事件记录和用户记录中，都需要添加 $importFlag 属性。
{% endhint %}

{% hint style="info" %}
**建议**：为保证正式项目数据导入的准确性，在数据导入正式项目前，建议先创建一个测试项目，将测试数据导入测试项目中，测试数据导入完成并且数据校验无误后，即可删除测试项目，将正式数据导入到正式项目，进而保证了数据导入的准确性。
{% endhint %}

## 2. 运行环境

导入工具需要在JAVA环境中运行，单机版和集群版都可以使用。

{% hint style="info" %}
1、导入工具需要在方舟部署服务器/可访问方舟服务器上的机器上使用；

2、为了避免网络异常、数据传输速度等造成数据导入慢等问题，**建议**数据导入工作在**方舟**部署的任一**服务器**上**使用**。
{% endhint %}

## 3. 使用方法

### 3.1  下载

如果你是单机版，请 [下载单机版导入工具](https://imguserradar.analysys.cn/files/ark/tool/ImportJsonToKafka-StandAlone.tar.gz)，如果你是集群版，请 [下载集群版导入工具](https://imguserradar.analysys.cn/files/ark/tool/ImportJsonToKafka-Cluster.tar.gz)。

### 3.2 解压

导入工具是一个压缩包，在下载成功后，需要在对应服务器上进行解压：

> **解压参考命令**：tar -zxf xxx.tar.gz
>
> **解压单机版**：tar -zxf ImportJsonToKafka-StandAlone.tar.gz
>
> **解压集群**版：tar -zxf ImportJsonToKafka-Cluster.tar.gz

目录显示如下：

```text
├── bin
│   ├── forceShutdown.sh
│   ├── shutdown.sh
│   └── startup.sh
├── conf
│   └── dataMigration.properties
├── data
│   ├── profile_20190101.json
│   └── event_20190101.json
├── jars
│   └── JsonToKafka-jar-with-dependencies.jar
└── logs
    ├── start.log
    ├── error.log
    ├── shutdown.log
    ├── log.out
    ├── event/eventImportSpeed.log
    ├── event/eventErrorData.log
    ├── profile/profileImportSpeed.log
    └── profile/profileErrorData.log
```

> **conf：**项目信息、服务器信息、文件目录等相关配置文件。
>
> **data**：可用来存放需要导入的文件，默认为空。
>
> **logs**：在启动、运行、终止等操作时才会有文件产生，刚下载时目录为空不影响使用。

### 3.3 配置文件

配置文件在conf/dataMigration.properties中，正常情况使用默认配置即可。

> **特殊参数说明**
>
> **base.source.dataDir**：导入工具根目录下data目录的绝对路径，如果启动时未指定，配置文件中需要替换成导入工具根目录下data目录的绝对路径。
>
> **base.appKey**：项目appKey，如果启动时未指定y，配置文件中需要替换成对应项目的appKey。

### 3.4 开始导入

以下所有操作都是在数据导入工具根目录执行。如果部署的方舟版本低于4.3.4，导入历史数据时需要关闭数据流的时间验证，具体操作流程见 [数据流验证](./#4-shu-ju-liu-yan-zheng-kai-qi-guan-bi)。

#### 3.4.1 数据导入工具根目录

进入工具文件夹根目录，如工具包是解压在 /data1目录下：cd  /data1/ImportJsonToKafka

#### 3.4.2 准备导入数据

将要导入的源数据放到 **data** 目录下，数据文件必须为json格式。

{% hint style="danger" %}
**事件数据文件命名**：必须以 **event\_** 开头，如 event\_20190101.json；

**用户数据文件命名**：必须以 **profile\_** 开头，如 profile\_20190101.json。
{% endhint %}

#### 3.4.3 启动导数

执行启动脚本：sh bin/startup.sh 0 appKey /data1/ImportJsonToKafka/data

> 新文件启动时需要带一下三个参数：
>
> **参数一，0**：必传参数，0代表开始新的导入任务；
>
> **参数二，appKey**：项目AppKey \(可在方舟页面查看\)，如果不动态传入，则需要修改配置文件中的appKey；
>
> **参数三，/data1/ImportJsonToKafka/data**：数据导入工具根目录下data目录的绝对路径。

### 3.5 停止导入

导入分为正常停止和强制停止两种，为了数据完整性和正确性，推荐使用正常停止。

#### 3.5.1 正常停止（推荐）

* 执行停止脚本：sh bin/shutdown.sh
* 查看停止日志，确认程序停止是否成功：cat logs/shutdown.log，如日志内容如下，则说明数据导入工具停止成功。 

![](../../../.gitbook/assets/image%20%28153%29.png)

#### 3.5.2 强制停止（不推荐,若数据未导入完成,会导致数据丢失或重复）

* 执行停止脚本：sh bin/forceShutdown.sh

### 3.6 继续导入

如果导入过程中被停止，指定数据未全部导入完成，则可以在原来的基础上继续导入。

执行启动脚本：sh bin/startup.sh 1

{% hint style="info" %}
1、如果想在原来的基础上导入，则启动只用传一个固定参数1，其他参数不用再传；

2、确保数据导入工具已经停止。
{% endhint %}

### 3.7 数据验证

数据导入完成后，可以验证导入的数据量是否正确。

#### 3.7.1 核对导入kafka的数据量是否正确

具体操作可参考  4.1 核对kafka中的数据量和kafka消费记录。

#### 3.7.2 核对Hive中事件数据量和Hbase中的用户数据量是否正确

1）登录ark3机器

2）su streaming （切换到streaming帳号）

3）核对事件数据

/usr/lib/presto/bin/presto-cli --server ark2:8285 --catalog hive --schema db\_~~**test123**~~ 

select count\(1\) from event\_vd where "$importFlag" = '1';

exit 退出

4）核对用户数据

/usr/lib/presto/bin/presto-cli --server ark2:8285 --catalog chbase --schema db\_~~**test123**~~ 

select count\(1\) from profile where "$importFlag" = '1'; 

exit 退出

{% hint style="info" %}
1、删除线部分 ~~**test123**~~ ****需要修改为要导入数据项目的appKey；

2、如果数据验证没有问题说明导入任务已经完成；

3、如果是单机版，需要将ark2改为ark1；

4、如果导入的数据中没有添加 $importFlag 属性，就不能用此字段作为查询条件。
{% endhint %}

{% hint style="danger" %}
如果在导入数据之前进行了 数据流验证关闭 操作，需要将相关服务开启，具体操作见 4.7 停止所有项目的数据流
{% endhint %}

## 4. 数据流验证

### 4.1 核对kafka中的数据量和kafka消费记录

1）登录ark1机器

2）sh /usr/hdp/current/kafka-broker/bin/kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list ark1:9092 --topic event\_~~**test123**~~ --time -1 && sh /usr/hdp/current/kafka-broker/bin/kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list ark1:9092 --topic profile\_~~**test123**~~ --time -1 && mysql -uroot -p~~**uname@password**~~ -e "select \* from ~~**db\_test123.**~~td\_kafka\_offset;"

示例结果如下图：

![](../../../.gitbook/assets/image%20%28133%29.png)

{% hint style="info" %}
1、删除线部分 ~~**test123**~~ ****需要修改为要导入数据项目的appKey；

2、删除线部分 ~~**uname@password**~~ 需要修改为mysql对应的用户名和密码，联系平台管理员获取。
{% endhint %}

3）如果kakfa中的数据量和Mysql存储的消费记录一致，则说明当前kafka中的数据已经消费完成。

{% hint style="info" %}
手动记录当前项目Kafka中，事件Topic和用户Topic各自的总数据量，方便后续进行数据导入后的校验。
{% endhint %}

## 5. 常见问题

### 5.1 项目显示未回数

在数据导入成功后，项目还是显示未回数，此时需要手动执行脚本，刷新项目的回数状态。操作如下：

1）登录ark3 机器

2）su streaming （切换到streaming帳号）

3）/opt/soft/streaming/bin/redis\_zadd\_hoteword.sh ~~**test123**~~

{% hint style="info" %}
删除线部分 ~~**test123**~~ ****需要修改为要导入数据项目的appKey；
{% endhint %}

### 5.2 元事件/分析中未找到事件/用户属性

在事件和用户属性导入成功后，在元事件管理中和分析模块中未找到刚刚导入成功的事件和用户属性，可能是因为埋点方案规则的影响，事件和用户属性都在计划外，需要将事件/属性添加到计划中才能在分析模块中使用。

![&#x8BA1;&#x5212;&#x5916;&#x4E8B;&#x4EF6;&#x6DFB;&#x52A0;&#x5230;&#x57CB;&#x70B9;&#x65B9;&#x6848;&#x4E2D;](../../../.gitbook/assets/image%20%28145%29.png)

![&#x8BA1;&#x5212;&#x5916;&#x7528;&#x6237;&#x5C5E;&#x6027;&#x6DFB;&#x52A0;&#x5230;&#x57CB;&#x70B9;&#x65B9;&#x6848;&#x4E2D;](../../../.gitbook/assets/image%20%28172%29.png)

### 5.4 su streaming提示输入密码

如果出现以下情况，并且不知道密码，可以退出后使用 sudo su streaming 进行用户切换

![](../../../.gitbook/assets/image%20%28120%29.png)

### 5.5 继续导入另外一个项目怎么操作？

在一个项目导入完成后，需要将数据重新导入到另外一个项目中时，需要停掉服务，重新启动，具体操作如下：

1） 进入工具文件夹根目录，如工具包是解压在 /data1目录下：cd  /data1/ImportJsonToKafka

2）停止数据输入工具，进行bin目录下的shutdown.sh，参考命令：sh bin/shutdown.sh

3）建议删除 logs目录下的日志文件，方便查看最新数据导入情况，参考命令：rm -rf logs/\*

4）准备导入文件，开始导入操作。流程参考和启动方法参考 [开始导入](./#34-kai-shi-dao-ru)

### 5.6 导入数据量不一致，怎么查看异常数据？

1）进入工具文件夹根目录，如工具包是解压在 /data1目录下：cd  /data1/ImportJsonToKafka

2）进入 logs目录，查看 error.log 文件是否有错误日志信息；

3）查看异常的源数据信息：

* 若导入的是事件数据，请到 event 文件夹中，查看eventErrorData.log 中记录的详细异常数据信息；
* 若导入的是用户数据，请到 profile文件夹中，查看profileErrorData.log 中记录的详细异常数据信息。

## 6. 附加说明

### 6.1 日志文件

导入工具在启动、运行、停止中都会打印日志，日志文件都在logs目录下，关于不同日志文件内容说明如下：

**start.log：**数据导入工具的启动日志；

**error.log：**数据导入工具运行的详细错误日志；

**shutdown.log：**数据导入工具正常停止时打印的停止信息；

**log.out：**数据导入工具正常运行时打印的日志；

**event/eventImportSpeed.log：**事件数据实时导入进度和速度 ；

**event/eventErrorData.log：**事件数据导入过程中解析错误的数据 ；

**profile/profileImportSpeed.log：**用户数据实时导入进度和速度；

**profile/profileErrorData.log：**用户数据导入过程中解析错误的数据。

