# 数据批量导入工具说明

使用前一定请先阅读[数据格式](./integration-data-type.md)和[数据模型](./integration-data-model.md)的文档

## 1. 概述

批量导入工具用于将历史数据或外部数据以文件的形式导入易观方舟，以供分析使用。

使用批量导入工具导入的数据需符合[数据格式](./integration-data-type.md)和[数据模型](./integration-data-model.md)中的模型和数据类型的要求，否则可能会造成导入的数据部分(甚至是全部)全部丢失。

## 2. 运行环境

机器：方舟集群的最后一台机器

用户：streaming

## 3. 时间窗口过滤

**流处理时间过滤说明**

* 易观方舟的数据处理中默认处理数据中 xwhen 在时间窗口：系统时间 -10T 到系统时间 +3H 内的数据。
* 如果导入的数据在默认时间窗口外，且希望易观方舟对其进行分析的需求，请务必关闭流处理时间窗口过滤。

**关闭和开启流处理时间窗口过滤**

* 流处理时间窗口过滤由项目 Ark Streaming 的配置参数：FILTER 控制。
* 默认 FILTER=true，表示开启流处理时间窗口过滤；FILTER=false 时，表示关闭流处理时间窗口过滤。
* 若修改流处理时间窗口过滤的参数，则在 Ambari 上修改该参数，并重启 Ark Streaing 服务即可。

## 4. 使用方式

1. 将每一行都是一个符合[数据格式](./integration-data-type.md)和[数据模型](./integration-data-model.md)的数据文件(文件名称不做要求)放置到集群最后一台机器的某个目录下，如/home/streaming。
2. 使用用户streaming 切换到目录导入工具目录下： cd /opt/soft/streaming/bin。
3. 导入profile数据： sh ./write_demo_profile_data.sh。
4. 导入event数据： sh ./write_demo_event_data.sh。

**参数说明：**

三个参数依次是：appid、文件路径、kafkabroker list（一般是：ark1.analysys.xyz:9092,ark2.analysys.xyz:9092,ark3.analysys.xyz:9092）
其中参数传入的appid，会覆盖数据中的appid。

## 5. 注意事项

* 数据一经导入不易清除，请谨慎操作。
* 文件路径支持目录，目录下的文件会按照文件名称的字典顺序，依次被导入。
* 若 event 中的用户涉及到登录用户，请务必先导入涉及到的登录 id 的 profile 信息。
* 导入的 profile 信息，若是登录 id 的 profile 信息，请务必保证该登录 id 已进行过身份关联操作或在导入前先导入身份绑定操作。
* 若导入数据前，有关闭流处理时间窗口过滤的操作，请务必在导入的数据被流处理处理完成后，修改参为：FILTER=true
* 若导入数据的格式不符合方舟环境，将导致导入的用户无法在前端展示

[![ ](https://imguserradar.analysys.cn/fangzhou/img/2019/01/201901151711159657.jpeg)](https://ark.analysys.cn/view/sign/signup.html?campaign_id=2111486795&utm_campaign=%E6%96%87%E6%A1%A3%E6%B3%A8%E5%86%8C&utm_medium=%E8%87%AA%E5%AA%92%E4%BD%93&utm_source=%E6%96%87%E6%A1%A3&utm_content=&utm_term=)