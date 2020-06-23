# 数据接入管理

在可以应用数据之前需要进行数据接入，这个过程需要产品经理、数据分析师、工程师等多方协作。

![](../../../.gitbook/assets/image%20%28126%29.png)

通常需要经过以下三步：

### **Step1 设计埋点方案**

使用数据的需求方（产品经理/运营/数据分析师等）确定使用目的，梳理需要分析的指标，进而据此设计埋点方案（即采集哪些事件，上报哪些用户信息）

设计好的埋点方案功能可以批量上传在在线页面，集中管理，自动化验证。

{% page-ref page="schema.md" %}

{% hint style="info" %}
若只是简单的运营页面，也可以跳过埋点方案的设计，等到工程师完成Step2和3时，通过可视化埋点的功能进行圈选定义。

{% page-ref page="virtualizer.md" %}
{% endhint %}

### **Step2 集成SDK**

工程师根据埋点方案集成SDK，上报数据

{% page-ref page="sdks.md" %}

### **Step3  数据验证**

集成SDK后需要通过数据验证，确保数据接入的完整性、及时性和正确性，验证通过后即可发布应用

{% page-ref page="validation.md" %}



{% hint style="info" %}
以上内容没有解答我的问题？[点击我来反馈](https://support.qq.com/products/118522/) 🚀
{% endhint %}

