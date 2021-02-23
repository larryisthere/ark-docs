# csv格式导入

## 1.介绍说明

* 多线程的方式，读取指定目录下json格式文件，导入程序分别将事件数据导入方舟事件数据Topic、用户数据导入方舟用户数据Topic；
* 已经读取完成的文件不会重复读取；
* 自动解析csv表头，作为字段属性名称；
* 可以对导入字段做字段名映射。

{% hint style="danger" %}
1、单个文件同时只能由一个线程读取。

2、文档中都是以集群版为例切换机器或连接服务，如果是**单机版**，将对应的ark2/ark3**改为ark1**即可。

3、导入文件需要以event\_和profile\_开头。

4、csv文件内第一行必须为表头。
{% endhint %}

{% hint style="info" %}
**建议**：为保证正式项目数据导入的准确性，在数据导入正式项目前，建议先创建一个测试项目，将测试数据导入测试项目中，测试数据导入完成并且数据校验无误后，即可删除测试项目，将正式数据导入到正式项目，进而保证了数据导入的准确性。
{% endhint %}

## 2.运行环境

导入工具需要在JAVA环境中运行，单机版和集群版都可以使用。

{% hint style="info" %}
1、导入工具需要在方舟部署服务器/可访问方舟服务器上的机器上使用；

2、为了避免网络异常、数据传输速度等造成数据导入慢等问题，**建议**数据导入工作在**方舟**部署的任一**服务器**上**使用**。
{% endhint %}

## 3.使用方法

csv文导入工具通过父命令+子命令的形式直接启动。

### 3.1.参数说明

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;&#x79F0;</th>
      <th style="text-align:left">&#x53C2;&#x6570;&#x793A;&#x4F8B;</th>
      <th style="text-align:left">&#x53C2;&#x6570;&#x8BF4;&#x660E;</th>
      <th style="text-align:left">&#x662F;&#x5426;&#x5FC5;&#x4F20;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">-k</td>
      <td style="text-align:left">31abd9593e9983ec</td>
      <td style="text-align:left">&#x9879;&#x76EE;appKey</td>
      <td style="text-align:left">&#x662F;</td>
    </tr>
    <tr>
      <td style="text-align:left">-d</td>
      <td style="text-align:left">/data/tmp</td>
      <td style="text-align:left">&#x9700;&#x8981;&#x5BFC;&#x5165;&#x7684;csv&#x6587;&#x4EF6;&#x7684;&#x5BFC;&#x5165;&#x76EE;&#x5F55;</td>
      <td
      style="text-align:left">&#x662F;</td>
    </tr>
    <tr>
      <td style="text-align:left">-c</td>
      <td style="text-align:left">/data/tmp/csv_import.properties</td>
      <td style="text-align:left">
        <p>&#x6307;&#x5B9A;&#x914D;&#x7F6E;&#x6587;&#x4EF6;&#x7EDD;&#x5BF9;&#x8DEF;&#x5F84;&#xFF0C;</p>
        <p>&#x914D;&#x7F6E;&#x6587;&#x4EF6;&#x540D;&#x4E3A;csv_import.properties</p>
      </td>
      <td style="text-align:left">&#x5426;</td>
    </tr>
    <tr>
      <td style="text-align:left">--help/-h</td>
      <td style="text-align:left">&#x65E0;</td>
      <td style="text-align:left">&#x663E;&#x793A;csv&#x6587;&#x4EF6;&#x5BFC;&#x5165;&#x5E2E;&#x52A9;&#x6587;&#x6863;</td>
      <td
      style="text-align:left">&#x5426;</td>
    </tr>
  </tbody>
</table>

{% hint style="info" %}
如果csv文件列已经包含了xwho,xwhen,xwhat，可以不用指定配置文件直接导入，如果数据中不含这3个字段，则需要添加配置文件。
{% endhint %}

### 3.2.配置文件参数说明

| 参数名称 | 参数示例 | 参数说明 | 是否必传 |
| :--- | :--- | :--- | :--- |
| event.attr.mapping | user\_id:xwho,update:xwhen | 将csv事件字段列名映射成方舟的字段名，字段映射中间用冒号分隔，多个字段映射中间用逗号分隔 | 否 |
| profile.attr.mapping | user\_id:xwho,update:xwhen | 将csv用户字段列名映射成方舟的字段名，字段映射中间用冒号分隔，多个字段映射中间用逗号分隔 | 否 |
| event.attr.to.number | height,length | 将csv事件文件的指定列转换成数值类型,多个字段用逗号分隔 | 否 |
| profile.attr.to.number | age,grade | 将csv用户文件的指定列转换成数值类型,多个字段用逗号分隔 | 否 |
| kafka.metadata.broker.list | ark1:9092,ark2:9092,ark3:9092 | kafka的主机名和端口号 | 否 |
| is.login.data | true | 导入数据是否属于登录数据 | 否 |

### 3.3.示例展示

将/data/tmp/20201027/csv\_import/文件夹下的事件数据导入项目test123321。

```text
arksh csv-import -k test123321 -d /data/tmp/20201027/csv_import
```

