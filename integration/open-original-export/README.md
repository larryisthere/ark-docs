---
description: 方舟不仅能够在产品的分析界面上下载查询的明细结果，还提供从平台中导出和获取数据的方案满足客户需求。
---

# 数据导出

**直接从Kafka中订阅数据**

* 适用于有实时计算实时获取数据的场景
* 可用于方舟私有部署的单机版和集群版
* 用于实时获取最细粒度的用户行为和属性数据

{% page-ref page="zhi-jie-cong-kafka-zhong-ding-yue-shu-ju.md" %}

**使用JDBC进行数据访问**

* 适用于有通过 JDBC 协议与其它系统打通的应用场景
* 相比直接从Kafka中订阅数据可满足数据中部分字段的查询和导出

{% page-ref page="shi-yong-jdbc-jin-hang-shu-ju-fang-wen.md" %}



