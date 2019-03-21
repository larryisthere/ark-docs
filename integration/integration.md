# 关于数据接入

## 接入方式

易观方舟支持全端数据接入：

* **客户端**

  [Android SDK](sdk/sdk-android.md)

  [iOS SDK](sdk/sdk-ios.md)

  [JS SDK](sdk/sdk-js.md)

  [小程序 SDK](sdk/sdk-wx.md)

  [Hybrid Android SDK](sdk/sdk-hybrid-android.md)

  [Hybrid iOS SDK](sdk/sdk-hybrid-ios.md)

* **服务端**

  [Java SDK](sdk/sdk-java.md)

* **工具导入**

  [历史数据导入](tool-import.md)

## 不同接入方式的差异

|  | 适用场景 | 埋点方式 |
| :--- | :--- | :--- |
| 客户端埋点 | 更适合采集用户前端行为数据，简单易用 | 代码埋点/可视化埋点 |
| 服务端埋点 | 更适合采集业务相关数据，因为和业务调用同步，会更进准，同时安全性更高 | 代码埋点 |
| 工具导入 | 生成符合格式的数据直接导入，通常使用于已经存在历史数据，并希望导入方舟来分析 | 工具导入 |

## 注意事项

1. 无论通过哪种方式接入，建议先阅读 [数据格式](integration-prepare/integration-data-type.md)和 [预置事件和属性](integration-prepare/integration-default-data.md) 以便更好的集成；
2. 集成阶段开启 Debug 模式，方便调试数据，但在发布的时候关闭，详细方法参考各 SDK 文档；
3. 按照预先设计好的埋点方案来进行埋点，直接影响数据质量；
4. 埋点的时候只需埋事件 ID 或者属性 ID 即可，无需中文名，这样在产品中可以根据分析需要还以灵活命名，随时更改中文名称；
5. 选择合适的用户标识，根据是否需要分析匿名用户、注册用户等来决定，建议阅读 [如何准确识别用户](https://github.com/larryisthere/ark-docs/tree/03211ca894b85a2ac80a6540af9a600714d71d2c/integration-user-identify.md)。

   阅读 [接入前准备](integration-prepare/) 了解更多，如有任何问题，您可以随时联系您的数据顾问。

[![ ](https://imguserradar.analysys.cn/fangzhou/img/2019/01/201901151711159657.jpeg)](https://ark.analysys.cn/view/sign/signup.html?campaign_id=2111486795&utm_campaign=文档注册&utm_medium=自媒体&utm_source=文档&utm_content=&utm_term=)

