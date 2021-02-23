# 事件数据导出

## 1. 介绍说明

导出方舟项目的事件数据到本地。

## 2. 运行环境

导出工具需要在JAVA环境中运行，单机版和集群版都可以使用。

{% hint style="info" %}
1、导出工具需要在方舟部署服务器/可访问方舟服务器上的机器上使用；

2、为了避免网络异常、数据传输速度等造成数据导出慢等问题，**建议**数据导出工作在**方舟**部署的任一**服务器上**使用。
{% endhint %}

## 3. 使用方法

事件数据导出工具通过父命令+子命令的形式直接启动。

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
      <td style="text-align:left">-T</td>
      <td style="text-align:left">4113c9cad1c301113783f3</td>
      <td style="text-align:left">&#x9879;&#x76EE;token&#xFF0C;&#x5BF9;&#x5E94;&#x7684;API AccessKey</td>
      <td
      style="text-align:left">&#x662F;</td>
    </tr>
    <tr>
      <td style="text-align:left">-s</td>
      <td style="text-align:left">20201020</td>
      <td style="text-align:left">&#x5BFC;&#x51FA;&#x6570;&#x636E;&#x7684;&#x5F00;&#x59CB;&#x65F6;&#x95F4;&#xFF0C;&#x683C;&#x5F0F;&#x4E3A;yyyyMMdd</td>
      <td
      style="text-align:left">&#x662F;</td>
    </tr>
    <tr>
      <td style="text-align:left">-e</td>
      <td style="text-align:left">20201023</td>
      <td style="text-align:left">&#x5BFC;&#x51FA;&#x6570;&#x636E;&#x7684;&#x7ED3;&#x675F;&#x65F6;&#x95F4;&#xFF0C;&#x683C;&#x5F0F;&#x4E3A;yyyyMMdd</td>
      <td
      style="text-align:left">&#x662F;</td>
    </tr>
    <tr>
      <td style="text-align:left">-d</td>
      <td style="text-align:left">/data/tmp/20201027/</td>
      <td style="text-align:left">&#x6307;&#x5B9A;&#x5BFC;&#x51FA;&#x7684;&#x76EE;&#x5F55;</td>
      <td style="text-align:left">&#x5426;</td>
    </tr>
    <tr>
      <td style="text-align:left">-m</td>
      <td style="text-align:left">true</td>
      <td style="text-align:left">
        <p>&#x51FA;&#x65F6;&#x7ED3;&#x679C;&#x4E3A;&#x591A;&#x4E2A;&#x6587;&#x4EF6;
          true&#x4E3A;&#x6BCF;&#x65E5;&#x4E00;&#x4E2A;&#x6587;&#x4EF6;</p>
        <p>false&#x4E3A;&#x6240;&#x6709;&#x65E5;&#x671F;&#x4E00;&#x4E2A;&#x6587;&#x4EF6;
          &#x9ED8;&#x8BA4;&#x4E3A;true</p>
      </td>
      <td style="text-align:left">&#x5426;</td>
    </tr>
    <tr>
      <td style="text-align:left">--help/-h</td>
      <td style="text-align:left">&#x65E0;</td>
      <td style="text-align:left">&#x663E;&#x793A;&#x4E8B;&#x4EF6;&#x6570;&#x636E;&#x5BFC;&#x51FA;&#x5E2E;&#x52A9;&#x6587;&#x6863;</td>
      <td
      style="text-align:left">&#x5426;</td>
    </tr>
  </tbody>
</table>

### 3.2.项目token位置获取

token获取路径：

```text
右下角管理->项目概览
```

![](../../../.gitbook/assets/project_token_get.jpg)

### 3.3.示例展示

导出项目test123321 20201020到20201023的事件数据到文件夹/data/tmp/20201203/下。

```text
arksh event-export -k test123321 -T 444ed5f2f167c1364494100fe8cf616e -s 20201020 -e 20201023 -d /data/tmp/20201203/
```

