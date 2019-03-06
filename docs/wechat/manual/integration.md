# 接入流程

![接入流程](https://imguserradar.analysys.cn/files/ark/doc/2.png)

## Step 1：注册登录

从易观方舟小程序官网进入[注册页面](https://ark.analysys.cn/mpa/view/sign/signup.html)，使用公司邮箱注册，也可在易观方舟官网上直接注册。如已有帐号，请直接登录。

## Step 2：添加应用

根据页面提示点击**添加应用**，填写**应用名称**、**应用领域**，确认添加并获取 AppKey，此 AppKey 用于集成 SDK 时标识应用。

> 不同的应用需分别添加获取 AppKey。

> 建议在正式接入前，先建一个测试应用。

![添加应用](https://imguserradar.analysys.cn/fangzhou/img/2018/10/201810241418302506.gif)

## Step 3：明确分析需求，梳理埋点事件和上传的用户属性

在开始接入数据之前，通过以下几个问题帮助梳理

1. 分析您的应用当前所处的阶段，设置合理的目标，拉新？促活？提高转化？

2. 分析实现目标需要采集哪些行为数据？

3. 需要从哪些维度去分析用户？

我们提供了**事件模板**和**用户属性模板**帮助您梳理需要采集的数据，以便做好埋点管理

![预埋点事件模板](https://imguserradar.analysys.cn/files/ark/doc/4.png)

![预上传用户属性模板](https://imguserradar.analysys.cn/files/ark/doc/5.png)

下载地址：

[易观方舟预埋点事件梳理模板_20170609.xlsx](http://imguserradar.analysys.cn/files/ark/template/%E6%98%93%E8%A7%82%E6%96%B9%E8%88%9F%E9%A2%84%E5%9F%8B%E7%82%B9%E4%BA%8B%E4%BB%B6%E6%A2%B3%E7%90%86%E6%A8%A1%E6%9D%BF_20170609.xlsx)

[易观方舟预上传用户属性模板_20170609.xlsx](http://imguserradar.analysys.cn/files/ark/template/%E6%98%93%E8%A7%82%E6%96%B9%E8%88%9F%E9%A2%84%E4%B8%8A%E4%BC%A0%E7%94%A8%E6%88%B7%E5%B1%9E%E6%80%A7%E6%A8%A1%E6%9D%BF_20170609.xlsx)

## Step 4：邀请工程师集成

在**成员管理**中，邀请负责集成 SDK 的工程师，并授予管理员权限，对方收到邮件加入方舟后，即可看到授权的应用。

![邀请工程师](https://imguserradar.analysys.cn/fangzhou/img/2018/10/201810241419512860.gif)

## Step 5：工程师集成 SDK

工程师登录方舟后，进入相应的应用，查看 AppKey，按照[小程序 SDK 使用说明](./sdk-wx.md)集成即可。

## Step 6：验证埋点的正确性和完整性

1）集成 SDK 后，**开启 Debug** 模式；

2）在真机上运行测试应用，对相关埋点的地方点击操作；

3）最多5分钟之内就可以在应用管理中查看 **回传日志**；

4）系统会默认校验回传日志是否满足在预梳理的需求，并在 **元事件管理** 和
**用户属性管理** 页面显示校验结果；

5）根据结果提示做相应调整。

![元事件管理](https://imguserradar.analysys.cn/fangzhou/img/2018/10/201810241421288388.png)

## Step 7：发布应用

当匹配状态都为 √ 的时候，表示集成正确，**关闭 Debug** 模式，即可发布应用。

![回传状态](https://imguserradar.analysys.cn/fangzhou/img/2018/10/201810241422332275.png)

[**开始集成>>**](./sdk-wx.md)
