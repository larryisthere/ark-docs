---
description: >-
  方舟作为开放的架构，可以通过消费实时数据来满足更多使用场景。服务端接到一条 SDK 发来的数据后，会对数据做一些预处理并将数据写入到消息队列 Kafka
  中供下游各类计算模块使用。
---

# 直接从Kafka中消费数据

{% hint style="info" %}
消费实时数据功能，涉及较多的技术细节，适用于对相关功能有经验的用户参考。如果对文档内容有疑惑，请及时联系工作人员。
{% endhint %}

## 要求

1. 启动消费的机器需与部署方舟的机器在同一个内网，且必须可以解析方舟服务器的 host。
2. 请选用兼容的 Kafka 客户端版本，高版本服务端兼容低版本客户端，反之则可能存在兼容性问题。方舟 Kafka 服务端版本为0.8.7，具体情况可在服务器上查看。

## 数据源topic版本说明

方舟5.2版本数据接入模块做了架构调整，默认情况下使用老的架构，我们会根据用户的数据情况在升级时切换到新的架构，并且在升级到5.3之前一定会切换到新的架构下。所以会出现5.2版本有些用户使用的是老架构，有些使用的是新架构的情况。具体可以通过Ambari上下面这个参数来判断：如果is.new.process=true，说明是新架构，如果没有该参数或者is.new.process=false，说明是老架构。

![](../../.gitbook/assets/image%20%28665%29.png)

新架构和老架构原始数据保存在kafka中的topic不同。老架构profile数据保存在profile\__${appid}中，event数据保存在event\__${_appid_}中。而新架构下原始数据都保存在pre\_${_appid_}中，消费pre\_${_appid_}中的数据需要通过xwhat的值来判断是profile数据还是event数据，如果xwhat属于下面六个值中的一个，则说明是profile事件（用户数据），具体可参考[https://arkdocs.analysys.cn/integration/prepare/data-model](https://arkdocs.analysys.cn/integration/prepare/data-model) 中的Profile部分的说明。如果xwhat不属于下面六个值，那说明是Event事件数据。

profile事件名：

* $profile\_set
* $profile\_set\_once
* $profile\_unset
* $profile\_increment
* $profile\_append
* $profile\_delete

## 消费参数

| 参数名称 | 参数值 |
| :--- | :--- |
| topic | **老架构：**event\_$_{appid}/profile\_$_{appid}\(其中{appid}表示项目的appid\) |
|  | **新架构：**pre\_${appid} |
| partition | 不同的用户可能不一样，以实际情况为准 |
| zookeeper | ark1:2181,ark2:2181,ark3:2181 |
| broker | ark1:9092,ark2:9092, ark3:9092 |

## 消费数据

消费有shell、原生API等多种方式，可以选择一种适合使用场景的方式。

下面给出两种 Shell 方式启动消费的示例，使用 Shell 方式可以通过重定向标准输出将数据写入文件后处理或直接用管道作为其他进程的输入，可以对接各种编程语言实现的处理程序。

#### **使用 Kafka Console Consumer**

* 可以使用 Kafka 自带的 Kafka Console Consumer 通过命令行方式消费，例如从最新数据开始消费：

  `bin/kafka-console-consumer.sh --zookeeper ark1:2181 --topic topicname(根据需要输入想消费的topic名)`

* 可以将 stdout 输出到文件或作为其他数据处理进程的输入数据。

#### **使用 Simple Consumer Shell**

* 使用 Simple Consumer Shell 可以实现更灵活的消费，例如：

```bash
bin/kafka-run-class.sh kafka.tools.SimpleConsumerShell \
        --broker-list ark2:9092         \
        --offset 1234                         \
        --partition 2                          \
        --topic event_topic                    \
        --print-offsets
```

## 数据格式

消费的数据的格式与导入时的[数据格式](../prepare/data-type.md)基本一致。

{% hint style="info" %}
以上内容没有解答我的问题？[点击我进入方舟论坛去反馈](https://www.analysysdata.com/forum/index) 🚀
{% endhint %}

