# 小程序 SDK 使用说明
  
## 1. 导入SDK

> 最新版本：[AnalysysFangzhou_WX_SDK.min.js]
(https://ark.analysys.cn/sdk/mpa/AnalysysFangzhou_WX_SDK.zip)

> 将AnalysysFangzhou_WX_SDK.min.js文件放到小程序的utils目录下

![示例图片](http://imguserradar.analysys.cn/fangzhou/img/2018/03/201803141746269839.jpg)

> 在小程序的app.js文件中的第一行加入以下代码:

```
    var Ark_SDK = require("../utils/AnalysysFangzhou_WX_SDK.min.js");
    Ark_SDK.APPKEY = ‘APPKEY’; //该APPKEY 为方舟产品中所注册的APPKEY
```

![示例图片](http://imguserradar.analysys.cn/fangzhou/img/2018/03/201803141748047008.jpg)

> 登录微信公众平台，设置>开发设置>服务器域名>request合法域名，加入以下两个:
> 1)非DEBUG模式  https://fzxcx.analysys.cn 
> 2)DEBUG模式 https://fzxcxd.analysys.cn 

![示例图片](http://imguserradar.analysys.cn/fangzhou/img/2018/03/201803141749012633.jpg)

> 完成以上步骤后，在各个Page 内通过以下代码获取Ark_SDK全局函数:

```
    var app = getApp();
    var Ark_SDK = app. Ark_SDK;
```

## 2.开启DEBUG测试模式

> 1）DEBUG测试模式下可以快速查看应用日志的回传情况，缩短数据的上传时间，快速看到相关日志信息。

![示例图片](http://imguserradar.analysys.cn/fangzhou/img/2018/03/201803141750248482.jpg)

> 2）DEBUG测试模式下可以校验您对自定义事件埋码的准确性，方便您进行更正修改。

```
    Ark_SDK.DEBUG= true;
```

![示例图片](http://imguserradar.analysys.cn/fangzhou/img/2018/03/201803141751452950.jpg)

## 3.接入完成

关闭DEBUG模式。开启您的方舟产品之旅。

```
    Ark_SDK.DEBUG= false;
```

**注意事项：**

> 1）appKey为添加应用后获取到的AppKey，一个AppKey仅能用于一个APP，同一应用不同平台需分别添加并获取AppKey；

> 2）Ark_SDK.DEBUG默认设置为false。

## 3.重要API接口说明

**Ark_SDK.event({EI:””,EPD:{}})**

说明：用户自定义事件

参数：
* EI 事件 ID
* EPD 事件属性

> 1)EI 事件 I D可在方舟产品中的元事件管理进行预定义。

> 2)EPD 为 JSON 格式。

示例：
```
    Ark_SDK.event({
      EI:"ID01",
      EPD:{
        "event01":"事件01"
      }
      });  
```

**Ark_SDK.pageTag({})**

说明：设置页面属性

参数：
* Object 设置页面属性

示例：
```
    Ark_SDK.pageTag({"pageTitle":"首页"});
```

**Ark_SDK.setUInfo({"UID":"","UN":"","UPRO":"","EM":"","PN":"","SEX":"","BIY":"","QQ":"","WED":"","WBD":"","UPD":{},})**

说明：设置访问用户的个体信息。

参数：
* UID         用户ID
* UN          用户名称
* UPRO        用户来源
* EM          用户邮箱
* PN          用户电话
* SEX         用户性别
* BIY         用户生日
* QQ          用户QQ号
* WED         用户微信号
* WBD         用户微博号
* UPD         用户自定义属性字典

> 1)UPD 为JSON格式。

示例：
```
    Ark_SDK.setUInfo({
       "UID":"123456",
       "UN":"用户01",
       "UPRO":"直接注册",
       "EM":"123456@qq.com",
       "PN":"18600001234",
       "SEX":"1",
       "BIY":"1988-01-01",
       "QQ":"123456",
       "WED":"18600001234",
       "WBD":"123456@qq.com",
       "UPD":{"自定义属性01":"属性01"}
   })
```

## 4. 验证SDK集成是否正确

通过以下几步完成数据准确性和完整性的验证：

1）开启Debug模式，访问您的网站，对相关埋点的地方点击操作；

![示例图片](http://imguserradar.analysys.cn/fangzhou/img/2018/03/201803141752380144.jpg)

2）最多5分钟之内就可以在应用管理中查看日志；

![示例图片](http://imguserradar.analysys.cn/fangzhou/img/2018/03/201803141753172603.jpg)

3）系统会默认校验回传日志是否满足在预埋点事件和预上传用户属性中梳理的需求；将在元事件管理和用户属性管理页面显示校验结果；

元事件管理:

![示例图片](http://imguserradar.analysys.cn/fangzhou/img/2018/03/201803141753415414.jpg)

用户属性管理:

![示例图片](http://imguserradar.analysys.cn/fangzhou/img/2018/03/201803141754027559.jpg)

4）根据结果提示做相应调整，当状态都为 √ 的时候，表示集成正确，关闭Debug模式，即可发布网站。

![示例图片](http://imguserradar.analysys.cn/fangzhou/img/2018/03/201803141754197154.jpg)