# JS SDK 使用说明

> 最新版本：3.0.2.2|20171023   
> 更新时间：2017/10/23  
> 更新内容：  
>
  1) 更改结束日志上传机制;   
  2) 修改上传内容为空的BUG; 
  
## 1. 导入SDK

将以下JS代码复制到您所需分析页面中的 `<head>` 和 `</head>` 标签之间,并将AppKey修改为添加应用后获取到的Appkey。
修改为您安装成功后，所有网址下的行为数据都将会被收集。
```
<script>
    window.fz = window.fz || [];
    (function() {
        window.fz.methods = 'DEBUG init open pageTAG setUInfo setPID delUInfo event openAPP'.split(' ');
        window.fz.factory = function(b) {
            return function() {
                var a = Array.prototype.slice.call(arguments);
                a.unshift(b);
                window.fz.push(a);
                return window.fz;
            }
        };
        for (var i = 0; i < fz.methods.length; i++) {
            var key = window.fz.methods[i];
            fz[key] = window.fz.factory(key);
        }
        var date = new Date();
        var time = new String(date.getFullYear()) + new String(date.getMonth() + 1) + new String(date.getDate());

        var d = document,
            c = d.createElement('script'),
            n = d.getElementsByTagName('script')[0];
            c.type = 'text/javascript';
        c.async = true;
        c.defer = true;
        c.id = 'devSDK';
        c.src = (location.protocol == 'http:' ?'http://':'https://')+'ark.analysys.cn/sdk/AnalysysFangzhou_JS_SDK.min.js?v=' + time
        n.parentNode.insertBefore(c, n);
        fz.init(AppKey) //AppKey为添加应用后获取到的Appkey;
    })();
</script>
```
## 2.开启DEBUG测试模式
> 1）DEBUG测试模式下可以快速查看应用日志的回传情况，缩短数据的上传时间，快速看到相关日志信息。

> 2）DEBUG测试模式下可以校验您对自定义事件埋码的准确性，方便您进行更正修改。


```
    fz.DEBUG= true;
```

## 3.接入完成
关闭DEBUG模式。开启您的方舟产品之旅。

```
    fz.DEBUG= false;
```

**注意事项：**

> 1）appKey为添加应用后获取到的AppKey，一个AppKey仅能用于一个APP，同一应用不同平台需分别添加并获取AppKey；

> 2）fz.DEBUG默认设置为false。   


## 3.重要API接口说明

### 3.1 采集页面事件信息 

**fz.init(AppKey)**


说明：初始化SDK,并发送页面开始日志


参数：
* AppKey

> AppKey为添加应用后获取到的Appkey;


示例：
```
    fz.init(AppKey)
```

**fz.pageTAG({Key:value,...})**

说明：设置页面属性
> 请在fz.init方法前调取该接口，即可生效。


参数：
* Object

> 为JSON格式，加入相关KEY与VALUE值，如只有KEY值无具体VALUE值，不会上传该字段。


示例：

```
    fz.pageTAG({"页面属性01":"属性01"});  
```

**fz.open()**

说明：用以Url地址不变的情况下，检测模块切换状况。

> 如有改变页面Title行为，请在该方法之前使用JS改变页面Title，后使用该方法。


示例：
```
    fz.open();
```

**fz.event({EI:””,EPD:{}})**

说明：用户自定义事件

参数：
* EI 事件ID
* EPD 事件属性

> 1)EI 事件ID可在方舟产品中的元事件管理进行预定义。

> 2)EPD 为JSON格式。

示例：
```
    fz.event({
      EI:"ID01",
      EPD:{
        "event01":"事件01"
      }
      });  
```

**fz.setUInfo({"UID":"","UN":"","UPRO":"","EM":"","PN":"","SEX":"","BIY":"","QQ":"","WED":"","WBD":"","UPD":{},})**

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

> 2)请在fz.init方法前调取该接口，即可生效。

示例：
```
    fz.setUInfo({
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
   }); 
```


**fz.delUInfo()**
说明：删除通过setUInfo接口所设置的访问用户的个体信息。

示例：
```
    fz.delUInfo();
```

**fz.openAPP(scheme)**
说明：仅用于使用JS SDK唤醒集成了方舟的iOS/Android SDK的应用。

参数：
* scheme 为安装了方舟的iOS/Android SDK的应用安装相关文档所配置的scheme

示例：
```
    fz.openAPP("h5sdk");
```

## 4. 验证SDK集成是否正确

通过以下几步完成数据准确性和完整性的验证：

1）开启Debug模式，访问您的网站，对相关埋点的地方点击操作；

2）最多5分钟之内就可以在应用管理中查看日志；

3）系统会默认校验回传日志是否满足在预埋点事件和预上传用户属性中梳理的需求；将在元事件管理和用户属性管理页面显示校验结果；

4）根据结果提示做相应调整，当状态都为 √ 的时候，表示集成正确，关闭Debug模式，即可发布网站。
