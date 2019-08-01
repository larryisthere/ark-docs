# 来源识别规则

易观方舟会根据一次访问中着陆页的来源来判断当次访问的来源，且本次访问（Session）中所有的事件都会继承这次访问的来源。

数据上报后会根据日志当中 url 中的UTM 参数 、 $referrer （$referrer\_domain）、$channel、消息相关事件中的utm参数等按照一定规则来处理，给每条日志添加UTM参数来源

同时根据如下规则和优先级会对渠道来源进行分组

| 渠道来源类型 | 预定义的识别规则 |
| :--- | :--- |
| 消息通知 | $utm\_campaign\_id 不为空  $utm\_medium = push\_notification |
| 电子邮件 | $utm\_campaign\_id 不为空  $utm\_medium = email |
| 系统预定义的XXX | $utm\_campaign\_id 不为空  $utm\_medium = xxx |
| 指定广告跟踪 | $utm\_source 不为空  $utm\_campaign 不为空  $utm\_medium ≠ push\_notification,email,xxx |
| 搜索引擎 | $utm\_campaign 为空  $utm\_medium = search |
| 社交媒体 | $utm\_campaign 为空  $utm\_medium = social |
| 外部链接 | $utm\_campaign 为空  $utm\_medium = referral |
| 直接访问 | $utm\_campaign 为空  $utm\_medium = direct |

详细识别规则如下： 

![](../../../.gitbook/assets/image%20%2827%29.png)

{% hint style="info" %}
示例说明

1. 小鸣打开了google，通过 Q 这个搜索词，找到了您的网站，那我们会根据点击网站时上报日志中的reffererdomain识别出来是谷歌，谷歌的域名被识别为搜索引擎，那么本次访问的所有事件都会加上 utm\_medium = search 的来源，归为搜索引擎的来源类型，并进一步解析搜索词 

2. 小鱼通过知乎中的链接进入到您的网站，我们会根据访问记录中的reffererdomain识别出它是知乎，知乎属于社交媒体，那么本次访问会标记为来源知乎，渠道来源类型属于社交媒体 

3. 小芳收到了一封您的网站通过方舟发送的邮件，她点击了邮件中的立即查看按钮进入到网站中，那我们会根据点击网站时上报的url识别出来其中utm参数，判断为电子邮件 

4. 小平在公众号上扫描了您创建的带UTM参数的二维码，进入网站时，我们会根据url中的参数解析为相应的来源，归为指定广告跟踪 ……
{% endhint %}

**另外，易观方舟同时兼容了百度统计的参数解析，即如果您自主投放时使用了hm参数，也是可以成功解析**

对应关系如下：

| 百度统计hm参数 | 参数名称 | 说明 | 对应参数名 |
| :--- | :--- | :--- | :--- |
| hmsr | 媒介平台 | 广告所投放的平台，如：新浪、搜狐等 | $utm\_source |
| hmpl | 计划名称 | 广告所属的推广计划，如：元旦促销 | $utm\_medium |
| hmcu | 单元名称 | 广告所属的推广单元，如：七夕促销 | $utm\_campaign |
| hmkw | 关键词 | 该条广告对应的关键词 | $utm\_term |
| hmci | 创意 | 广告内容的简要描述信息 | $utm\_content |

