# 数据接入和验证

![ ](https://imguserradar.analysys.cn/fangzhou/img/2019/02/201902151449466087.png)

**邀请工程师根据以下三项进行数据接入**

* 项目 AppKey（创建项目后生成，用于唯一标识项目的数据）
* SDK [集成指南](../integration/sdk/)
* 埋点方案（[如何设计埋点方案](../integration/prepare/tracking-plan.md)）

**接入后 Debug 数据验证**

集成 SDK 之后，debugMode 模式设置为 0 ，点击相应埋点位置，验证数据上报的完整性、准确性和及时性

* 支持按事件或用户过滤，e.g. 当小鸣和小红同时进行埋点验证时，可以根据过滤出自己的数据，更集中的验证
* 支持点击事件查看事件属性，验证上报的属性值是否正确
* 支持开始实时刷新，秒级更新上报的事件

![ ](https://imguserradar.analysys.cn/fangzhou/img/2019/02/201902151546546394.png%20)

**维护事件、属性显示名称**

数据验证完成后， 在[元事件](../features/project-manegement/meta-events.md)、[用户属性](../features/project-manegement/user-properties.md)、[事件属性](../features/project-manegement/event-properties.md)中可以查看全部上报的埋点事件和属性，支持管理显示名称、属性单位、属性维度字典等方便分析时使用，详见 [管理 ](../features/project-manegement/)中的功能使用说明。

![ ](https://imguserradar.analysys.cn/fangzhou/img/2018/08/201808101148521850.png)

至此，用于分析的数据采集集成工作完成。

