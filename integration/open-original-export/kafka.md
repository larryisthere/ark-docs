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

## 消费参数

| 参数名称 | 参数值 |
| :--- | :--- |
| topic | event_{appid}/profile_{appid}\(其中{appid}表示项目的appid\) |
| partition | partitionid\(从0开始，至少3个partition） |
| zookeeper | ark1:2181,ark2:2181,ark3:2181 |
| broker | ark1:9092,ark2:9092, ark3:9092 |

## 消费数据

消费有shell、原生API等多种方式，可以选择一种适合使用场景的方式。

下面给出两种 Shell 方式启动消费的示例，使用 Shell 方式可以通过重定向标准输出将数据写入文件后处理或直接用管道作为其他进程的输入，可以对接各种编程语言实现的处理程序。

#### **使用 Kafka Console Consumer**

* 可以使用 Kafka 自带的 Kafka Console Consumer 通过命令行方式消费，例如从最新数据开始消费：

  `bin/kafka-console-consumer.sh --zookeeper ark1:2181 --topic event_topic`

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

