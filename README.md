# 关于易观方舟

易观方舟是一款精细化运营分析产品。

将应用自身数据结合易观第三方数据，全景画像，通过多种模型深度分析用户行为，多种方式细分用户群体，洞察人群差异，辅助运营决策，多通道有效触达用户，进一步数据分析闭环验证效果。

帮助企业选择优质渠道、召回用户、用户价值提升等等，最终实现增收、节支、提效、避险。

## 核心价值：用户生命周期的管理

通过以下四个模块，达到分析-分群-触达-闭环验证的运营过程，实现用户全生命周期的管理

[看板](https://github.com/larryisthere/ark-docs/tree/03211ca894b85a2ac80a6540af9a600714d71d2c/docs/panel.md)——不同角色灵活自定义看板，实时跟踪关心的指标

[分析](https://github.com/larryisthere/ark-docs/tree/03211ca894b85a2ac80a6540af9a600714d71d2c/docs/analytics.md)——多模型多维度实时查询分析，任意节点下钻分析

[分群](https://github.com/larryisthere/ark-docs/tree/03211ca894b85a2ac80a6540af9a600714d71d2c/docs/segmentation.md)——多种方式细分人群，进一步分析或触达

[运营](https://github.com/larryisthere/ark-docs/tree/03211ca894b85a2ac80a6540af9a600714d71d2c/docs/operation.md)——连接第三方运营服务有效触达，数据闭环验证

![&#x6838;&#x5FC3;&#x4EF7;&#x503C;&#xFF1A;&#x7528;&#x6237;&#x751F;&#x547D;&#x5468;&#x671F;&#x7684;&#x7BA1;&#x7406;](https://imguserradar.analysys.cn/fangzhou/sysImg/201706141931230718.png)

## 产品特点

* 支持全端数据接入，跨屏打通数据
* 贯穿用户运营中各个环节的10+个分析模型，支持实时多维交叉查询，任意节点数据下钻
* 连接多个第三方运营服务商，支持针对不同的用户群体，个性化触达，闭环验证
* 易观第三方数据补充，全景用户画像
* 支持私有化部署，开放的 PaaS 架构，支持二次开发

## 接入流程

### Step 1 安装部署

在方舟技术团队支持下，快速安装部署。

> 注：公有云用户无需安装部署，跳过该步

### Step 2 登录方舟创建项目

部署完成后，登录平台管理员帐号，根据页面引导，创建项目

> **\[info\] 注意事项**
>
> 方舟支持将同一个应用多个平台的数据接入到一个项目中；建议您在接入正式数据前先创建一个测试环境项目用于测试。

### Step 3 接入数据和验证

![image](https://imguserradar.analysys.cn/fangzhou/img/2019/02/201902151449466087.png)

**邀请工程师根据以下三项进行数据接入**

* 项目 AppKey（创建项目后生成，用于唯一标识项目的数据）
* SDK [集成指南](https://github.com/larryisthere/ark-docs/tree/03211ca894b85a2ac80a6540af9a600714d71d2c/docs/sdk.md)
* 埋点方案（[如何设计埋点方案](https://github.com/larryisthere/ark-docs/tree/03211ca894b85a2ac80a6540af9a600714d71d2c/docs/integration-tracking-plan.md)）

**接入后 Debug 数据验证**

集成 SDK 之后，debugMode 模式设置为 0 ，点击相应埋点位置，验证数据上报的完整性、准确性和及时性

* 支持按事件或用户过滤，e.g. 当小鸣和小红同时进行埋点验证时，可以根据过滤出自己的数据，更集中的验证
* 支持点击事件查看事件属性，验证上报的属性值是否正确
* 支持开始实时刷新，秒级更新上报的事件

![image](https://imguserradar.analysys.cn/fangzhou/img/2019/02/201902151546546394.png%20)

**维护事件、属性显示名称**

数据验证完成后， 在[元事件](https://github.com/larryisthere/ark-docs/tree/03211ca894b85a2ac80a6540af9a600714d71d2c/docs/project-meta-events.md)、[用户属性](https://github.com/larryisthere/ark-docs/tree/03211ca894b85a2ac80a6540af9a600714d71d2c/docs/project-user-properties.md)、[事件属性](https://github.com/larryisthere/ark-docs/tree/03211ca894b85a2ac80a6540af9a600714d71d2c/docs/project-user-properties.md)中可以查看全部上报的埋点事件和属性，支持管理显示名称、属性单位、属性维度字典等方便分析时使用，详见 [管理](https://github.com/larryisthere/ark-docs/tree/03211ca894b85a2ac80a6540af9a600714d71d2c/docs/project-manegement.md) 中的功能使用说明。

![image](https://imguserradar.analysys.cn/fangzhou/img/2018/08/201808101148521850.png)

至此，用于分析的数据采集集成工作完成。

### Step 4 设置需要接入的运营服务

易观方舟支持各种用户细分，在接入第三方服务商后通过消息通知、电子邮件、短信等多种方式触达用户。

若需要使用其中的消息通知功能，需要集成方舟推送部分的 SDK 和推送方的 SDK，同时在 [管理-服务集成配置](https://github.com/larryisthere/ark-docs/tree/03211ca894b85a2ac80a6540af9a600714d71d2c/docs/project-integrations.md)中进行绑定配置，保证三方数据的连通

![image](https://imguserradar.analysys.cn/fangzhou/img/2019/02/201902151613578859.png)

> **\[info\]** 说明
>
> 电子邮件和短信服务无需集成第三方SDK，只需要保证上报相应的邮箱地址和手机号码作为用户属性，在方舟当中进行绑定配置即可。

### Step 5 发布应用

以上步骤都已经完成后，保证 debugMode 模式设置为 0，即可发布应用。

> **\[info\]** 关于 Debug 模式
>
> 0 表示关闭 Debug 模式；1 表示打开 Debug 模式，但该模式下发送的数据仅用于调试，不计入平台数据统计 ；2 表示打开 Debug 模式，该模式下发送的数据可计入平台数据统计。

## 开始使用

应用发布后，当用户产生行为时，即可实时收到数据，开始以下功能的使用：

[看板](https://github.com/larryisthere/ark-docs/tree/03211ca894b85a2ac80a6540af9a600714d71d2c/docs/panel.md)——不同角色灵活自定义看板，实时跟踪关心的指标

[分析](https://github.com/larryisthere/ark-docs/tree/03211ca894b85a2ac80a6540af9a600714d71d2c/docs/analytics.md)——10+ 个分析模型多维度实时查询分析

[分群](https://github.com/larryisthere/ark-docs/tree/03211ca894b85a2ac80a6540af9a600714d71d2c/docs/segmentation.md)——多种方式细分人群，进一步分析或触达

[运营](https://github.com/larryisthere/ark-docs/tree/03211ca894b85a2ac80a6540af9a600714d71d2c/docs/operation.md)——连接第三方运营服务有效触达

[![ ](https://imguserradar.analysys.cn/fangzhou/img/2019/01/201901151711159657.jpeg)](https://ark.analysys.cn/view/sign/signup.html?campaign_id=2111486795&utm_campaign=文档注册&utm_medium=自媒体&utm_source=文档&utm_content=&utm_term=)

