# Step 5 发布应用

数据接入，验证完毕之后，保证 debugMode 模式设置为 0，即可发布应用。

{% hint style="info" %}
**关于 Debug 模式**

0 关闭 Debug 模式

1 打开 Debug 模式，但该模式下发送的数据仅用于调试，不计入平台数据统计 

2 打开 Debug 模式，该模式下发送的数据可计入平台数据统计
{% endhint %}

待应用发布后，用户使用产品时，即可触发事件，方舟后台回数即可进行数据分析。

{% page-ref page="getting-started.md" %}

另外，易观方舟支持各种用户细分，在接入第三方服务商后通过消息通知、电子邮件、短信等多种方式触达用户。

电子邮件和短信服务无需集成第三方SDK，只需要保证上报相应的邮箱地址和手机号码作为用户属性，在方舟当中进行绑定配置即可。

{% hint style="info" %}
若需要使用其中的消息通知功能，需要集成方舟推送部分的 SDK 和推送方的 SDK，同时在 [管理-服务集成配置](../features/project-manegement/integrations.md)中进行绑定配置，保证三方数据的连通
{% endhint %}

![ ](https://imguserradar.analysys.cn/fangzhou/img/2019/02/201902151613578859.png)

{% page-ref page="getting-started.md" %}

