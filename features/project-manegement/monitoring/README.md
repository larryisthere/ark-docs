---
description: 对于常规指标的波动，我们不必每天登录系统追踪其波动，通过监控告警，方舟会在指标波动达到一定范围时通过平台内的消息通知、电子邮件等方式第一时间进行提醒。
---

# 监控告警

## 智能监控

方舟会在每日0时自动计算所有事件过去12周的历史波动区间并与过去一天的实际值进行比较，如果波动超出一定的范围则会进行提醒。

![&#x667A;&#x80FD;&#x544A;&#x8B66; - &#x67E5;&#x770B;&#x544A;&#x8B66;&#x8BE6;&#x60C5;](../../../.gitbook/assets/image%20%28269%29.png)

详见：

{% page-ref page="intelligent-monitoring.md" %}

## 自定义监控

如果希望更灵活配置监控条件可以使用自定义监控。

与智能监控对比，你可以设定事件具体的属性、可以选择告警时间以及将指标的绝对值作为告警条件。

![](../../../.gitbook/assets/image%20%2898%29.png)

详见：

{% page-ref page="monitoring.md" %}

## 智能监控 vs 自定义监控

<table>
  <thead>
    <tr>
      <th style="text-align:left"></th>
      <th style="text-align:left">&#x667A;&#x80FD;&#x76D1;&#x63A7;</th>
      <th style="text-align:left">&#x81EA;&#x5B9A;&#x4E49;&#x76D1;&#x63A7;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>&#x76D1;&#x63A7;&#x65B9;&#x5F0F;</b>
      </td>
      <td style="text-align:left">&#x81EA;&#x52A8;&#x76D1;&#x63A7;</td>
      <td style="text-align:left">&#x624B;&#x52A8;&#x8BBE;&#x7F6E;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>&#x76D1;&#x63A7;&#x8303;&#x56F4;</b>
      </td>
      <td style="text-align:left">&#x6240;&#x6709;&#x542F;&#x7528;&#x4E8B;&#x4EF6;&#x7684;&#x6B21;&#x6570;&#x548C;&#x7528;&#x6237;&#x6570;</td>
      <td
      style="text-align:left">&#x6307;&#x5B9A;&#x4E8B;&#x4EF6;&#x66F4;&#x591A;&#x7684;&#x8BA1;&#x7B97;&#x65B9;&#x5F0F;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>&#x6307;&#x6807;&#x8BBE;&#x5B9A;</b>
      </td>
      <td style="text-align:left">&#x4E0D;&#x542B;&#x7EF4;&#x5EA6;</td>
      <td style="text-align:left">&#x53EF;&#x8BBE;&#x5B9A;&#x7EF4;&#x5EA6;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>&#x8BA1;&#x7B97;&#x65F6;&#x95F4;</b>
      </td>
      <td style="text-align:left">&#x6BCF;&#x65E5;0&#x70B9;&#x63D0;&#x9192;</td>
      <td style="text-align:left">&#x81EA;&#x5B9A;&#x65F6;&#x95F4;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>&#x5BF9;&#x6BD4;&#x533A;&#x95F4;</b>
      </td>
      <td style="text-align:left">&#x4E0E;&#x8FC7;&#x5F80;12&#x5468;&#x5E73;&#x5747;&#x503C;&#x7684;&#x9884;&#x6D4B;&#x533A;&#x95F4;&#x5BF9;&#x6BD4;</td>
      <td
      style="text-align:left">&#x4E0E;&#x6307;&#x5B9A;&#x503C;&#x5BF9;&#x6BD4;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>&#x9608;&#x503C;&#x8BBE;&#x5B9A;</b>
      </td>
      <td style="text-align:left">
        <p>&#x4E25;&#x683C;&#xFF1A;+/=10%</p>
        <p>&#x4E2D;&#x7B49;&#xFF1A;+/-30%</p>
        <p>&#x5BBD;&#x677E;&#xFF1A;+/-50%</p>
      </td>
      <td style="text-align:left">&#x81EA;&#x884C;&#x8BBE;&#x5B9A;&#x767E;&#x5206;&#x6BD4;&#xFF0C;&#x7EDD;&#x5BF9;&#x503C;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>&#x544A;&#x8B66;&#x65B9;&#x5F0F;</b>
      </td>
      <td style="text-align:left">&#x7AD9;&#x5185;&#x6D88;&#x606F;&#x3001;&#x90AE;&#x4EF6;</td>
      <td style="text-align:left">&#x7AD9;&#x5185;&#x6D88;&#x606F;&#x3001;&#x90AE;&#x4EF6;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>&#x544A;&#x8B66;&#x8303;&#x56F4;</b>
      </td>
      <td style="text-align:left">
        <p>&#x7BA1;&#x7406;&#x5458;&#x63A7;&#x5236;&#x5168;&#x5C40;&#x5F00;&#x5173;</p>
        <p>&#x4E2A;&#x4EBA;&#x63A7;&#x5236;&#x81EA;&#x5DF1;&#x7684;&#x5F00;&#x5173;</p>
      </td>
      <td style="text-align:left">&#x6307;&#x5B9A;&#x90AE;&#x4EF6;&#x63A5;&#x6536;&#x4EBA;</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>&#x544A;&#x8B66;&#x8BE6;&#x60C5;</b>
      </td>
      <td style="text-align:left">&#x4E8B;&#x4EF6;&#x5206;&#x6790;</td>
      <td style="text-align:left">&#x4E8B;&#x4EF6;&#x5206;&#x6790;</td>
    </tr>
  </tbody>
</table>

{% hint style="info" %}
以上内容没有解答我的问题？[点击我来反馈](https://support.qq.com/products/118522/) 🚀
{% endhint %}

