# 1.  集成 SDK

工程师根据以下两项进行数据接入：

* 项目 AppKey（创建项目后生成，用于唯一标识项目的数据）
* 集成 [JS SDK ](../../integration/sdk/js/js.md)

![](../../.gitbook/assets/image%20%2829%29.png)

## **接入后 Debug 数据验证**

集成 SDK 之后，[验证数据](../../integration/data-verification/)接入的准备性。验证完毕之后，保证 debugMode 模式设置为 0，发布应用。

{% hint style="info" %}
**关于 Debug 模式**

0 关闭 Debug 模式

1 打开 Debug 模式，但该模式下发送的数据仅用于调试，不计入平台数据统计 

2 打开 Debug 模式，该模式下发送的数据可计入平台数据统计
{% endhint %}

{% page-ref page="virtualizer.md" %}



{% hint style="info" %}
以上内容没有解答我的问题？[点击我来反馈](https://support.qq.com/products/118522/) 🚀
{% endhint %}

