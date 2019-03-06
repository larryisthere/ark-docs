# Android SDK 使用说明

适用于Android应用，支持Hybrid APP。

## 1. 下载SDK

最新版本：[AnalysysFangzhou_Android_SDK_V3.7.9.3](https://ark.analysys.cn/sdk/AnalysysFangzhou_Android_SDK_V3.7.9.3.zip)

> 更新时间：2018/05/14
>
> 更新内容：
>
>   1) 增加debug日志
>
>   2) 检测EventInfo,PageInfo,EgUser中的自定义属性
>
>   3) 性能优化

## 2. 准备环境导入SDK

* 若是Gradle(Android studio/ intellij idea)环境：

  File -> Project Structure -> Module -> Dependencies -> Add... -> Library... -> Attach Classes. 选中AnalysysFangzhou_Android_SDK_V3.7.9.3.jar

* 若是Ant(Eclipse)环境：

  将SDK拷入项目libs目录下，选中AnalysysFangzhou_Android_SDK_V3.7.9.3.jar右键Build Path -> Add to Build Path

## 3. 配置Manifest

### 3.1. 申请权限

为了获取到相关数据来满足分析需求，请务必正确配置下列权限和组件

```xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.READ_PHONE_STATE"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
<uses-permission android:name="android.permission.GET_TASKS"/>
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
<uses-permission android:name="android.permission.BLUETOOTH"/>
<uses-permission android:name="android.permission.WRITE_SETTINGS"/>
```

**权限说明：**

  | 权限 | 用途 |
  | ------------- |:-------------:|
  |  android.permission.INTERNET  |  访问互联网的权限  |
  |  android.permission.READ_PHONE_STATE  |  访问电话相关信息  |
  |  android.permission.WRITE_EXTERNAL_STORAGE  |  获取外部存储卡写权限  |
  |  android.permission.ACCESS_WIFI_STATE  |  获取MAC地址的权限  |
  |  android.permission.ACCESS_COARSE_LOCATION  |  允许获取大概位置信息  |
  |  android.permission.ACCESS_FINE_LOCATION  |  允许获取精准定位信息  |
  |  android.permission.GET_TASKS  |  允许程序获取当前或最近运行的应用  |
  |  android.permission.RECEIVE_BOOT_COMPLETED  |  允许程序开机自动运行  |
  |  android.permission.ACCESS_NETWORK_STATE  |  访问网络连接情况  |
  |  android.permission.BLUETOOTH       |  允许应用程序读取蓝牙MAC  |
  |  android.permission.WRITE_SETTINGS      |  允许应用程序读取或写入系统设置  |

### 3.2. 组件配置，声明相关Service和Receiver

```xml
<service
  android:name="com.eguan.monitor.fangzhou.service.MonitorService"
  android:enabled="true"
  android:exported="true"
  android:process=":monitorService">
</service>

<service    
    android:name="com.eguan.monitor.fangzhou.service.EgAccessibilityService"    
    android:permission="android.permission.BIND_ACCESSIBILITY_SERVICE"    
    android:exported="true"    
    android:enabled="true"    
    android:process=":monitorService">    
    <intent-filter>    
        <action android:name="android.accessibilityservice.AccessibilityService"/>    
    </intent-filter>    
</service>    
    
<service
    android:name="com.eguan.monitor.fangzhou.service.MonitorJobService"
    android:permission="android.permission.BIND_JOB_SERVICE"
    android:process=":monitorService">
</service>

<receiver android:name="com.eguan.monitor.receiver.SystemStartReceiver">
  <intent-filter android:priority="9999">
    <action android:name="android.intent.action.BOOT_COMPLETED" />
    <action android:name="android.intent.action.USER_PRESENT" />
    <action android:name="android.intent.action.ACTION_POWER_CONNECTED" />
    <action android:name="android.intent.action.ACTION_POWER_DISCONNECTED" />
  </intent-filter>
</receiver>
```

### 3.3. meta-data配置
在manifest文件中添加位置信息获取的配置，以便获取地理位置信息进行分析。

```xml
<meta-data
  android:name="getLocationInfo"
  android:value="Yes"/>
```

在manifest文件中添加appkey的配置，防止appkey没有配置。    

```xml
<meta-data
  android:name="egAppKey"
  android:value="7752552892442721d"/>
```

在manifest文件中添加channel的配置，防止channel没有配置, channel不要配置成纯数字组成的字符串。

```xml
<meta-data
  android:name="egChannel"
  android:value="应用宝"/>
```

备注：    
meta-data配置中的name不能变

##  4. 初始化SDK
通过调用initEguan来初始化SDK，根据业务分析需求调取相应的API接口    

**initEguan(Context mContext,String key,String channel)**

参数：mContext,key,channel

* mContext 应用上下文对象Context；
* key 为添加应用后获取到的AppKey

> 一个AppKey仅能用于一个APP，同一应用不同平台需分别添加并获取AppKey


* channel　应用的下载渠道ID

标识不同渠道来源的数据，用于渠道分析

> 我们已经为您**预置了主流渠道ID**，建议直接引用便于数据统计，若未包含在下方列表中，请联系我们添加或自行定义，**仅支持输入字母和数字**

> 我们也提供了多渠道打包工具，使用方法见下方[9. 多渠道打包工具分](#tool)

**调用方法：**

```java
EguanMonitorAgent.getInstance().initEguan(mContext,key,channel);
```

预置渠道ID-渠道名称对应表：

  |  渠道ID  |  渠道名称  |
  | ------------- |:-------------:|
  |  GooglePlay  |  Google Play  |
  |  YingYongBao  |  应用宝  |
  |  WanDouJia  |  豌豆荚  |
  |  Qihoo360 Mobile Assistant  |  360手机助手  |
  |  Baidu Mobile Assistant  |  百度手机助手  |
  |  91 Assistant  |  91助手  |
  |  PP Assistant  |  PP助手  |
  |  Sogou Market  |  搜狗手机助手  |
  |  Huawei Application Market  |  华为应用市场  |
  |  Xiaomi AppStore  |  小米应用商店  |
  |  VIVO AppStore  |  VIVO应用商店  |
  |  OPPO AppStore  |  OPPO应用商店  |
  |  Meizu AppStore  |  魅族应用商店  | 
  |  Galaxy Apps  |  三星应用商店  |
  |  Coolpad AppStore  |  酷派应用商店  |
  |  ZTE AppStore  |  中兴应用商店  |
  |  TianYu AppCenter  |  天语应用中心  |
  |  Lenovo Store  |  联想乐商店  |
  |  Smartisan AppStore  |  锤子应用商店  |
  |  LeTV Store  |  乐视应用商店  |
  |  Suning AppStore  |  苏宁应用商店  |
  |  JingLi Store  |  金立软件商店  |
  |  189Store  |  天翼空间  |
  |  Wo Store  |  沃商店  |
  |  AnQuan Market  |  安全市场  |
  |  Android Market  |  安卓市场  |
  |  Anzhi Market  |  安智市场  |
  |  Droi Market  |  卓易市场  |
  |  MM Market  |  MM商场  |
  |  Dangbei Market  |  当贝市场  |
  |  MuZhiWan Market  |  拇指玩市场  |
  |  JiFeng Market  |  机锋市场  |
  |  MuMaYi Market  |  木蚂蚁市场  |
  |  N Market  |  N多市场  |
  |  Cool Market  |  酷市场  |
  |  Hisense AppStore  |  海信应用汇  |
  |  YingYongHui  |  应用汇  |
  |  JianYingYong  |  简应用  |
  |  MianShangDian  |  免商店  |
  |  YingYongKu  |  应用酷  |
  |  ShouJiLeYuan  |  手机乐园  |

备注：

需要在应用的自定义的Application类的onCreate或者在应用启动页面的
onCreate函数里面调用。<br/>
不要在子线程调用，initEguan里面已经加入了子线程处理，不会影响宿主应用的使用。

## 5. 重要API接口说明

### 5.1 采集应用打开关闭信息

**onResume(Context context)**

说明：onResume接口需要在所有的Activity页面中调用，或者使用一个父类的Activity，在父类的onResume函数中调用onResume接口，<br/>
      子类的Activity的onResume函数中调用super.onResume函数    
参数：

* Context 上下文

> 3.6.0.0之前的SDK版本（不含），没有Context参数
在每个 Activity 的 onResume 方法中调用此方法
建议 onResume 接口与 onPause 接口在项目中 Activity 的基类中调用，继承类就无需再次调用，请勿重复调用

调用方法：

```java
EguanMonitorAgent.getInstance().onResume(context);
```

**onPause (Context context)**

说明：onPause接口需要在所有的Activity页面中调用，或者使用一个父类的Activity，在父类的onPause函数中调用onPause接口，<br/>
      子类的Activity的onPause函数中调用super.onPause函数    
参数：

* Context   上下文

> 3.6.0.0之前的SDK版本（不含），没有Context参数
在每个 Activity 的 onPause 方法中调用此方法
在应用关闭前调用，记录应用关闭时间

调用方法：

```java
EguanMonitorAgent.getInstance().onPause (context);
```

**onKillProcess (Context context)**

说明：开发者调用Process.kill或者System.exit之类的方法杀死进程前调用方法
参数：

* Context 上下文

> 3.6.0.0之前的SDK版本（不含），没有Context参数

调用方法：

```java
EguanMonitorAgent.getInstance(). onKillProcess (context);
```

备注：

onResume和onPause需要在每个页面中成对使用

### 5.2 采集应用页面事件信息
具体需要采集哪些页面事件信息，详见应用管理-预埋点事件中的梳理

**onPageStart(Context mContext, String pageId)**

说明：页面打开接口
参数：

* mContext   上下文
* pageId  页面ID

> onPageStart接口在进入页面时onResume()调用，确保页面结束时间的统计准确性

调用方法：

```java
EguanMonitorAgent.getInstance().onPageStart(this, "PageRecordActivity");
```

**onPageEnd(Context mContext, String pageId, String url, Map<String, Object> pageProperty)**

说明：页面关闭接口
参数：

* mContext   上下文
* pageId   页面ID
* url  页面链接属性
* pageProperty  页面的自定义属性

示例：

```java
Map<String,Object> customParams = new HashMap<>();
customParams.put("level","1");
customParams.put("country","CN");
EguanMonitorAgent.getInstance().onPageEnd(this,"PageRecordActivity", "http://www.baidu.com", customParams);
```

> onPageEnd接口在页面即将结束时onPause()调用，确保页面信息统计的准确性
onPageStart和onPageEnd需要配对使用，并且两者的pageId应该是相同的

调用方法：

```java
EguanMonitorAgent.getInstance().onPageEnd(this, "PageRecordActivity", "http://www.baidu.com", customParams);
```

备注：

onPageStart和onPageEnd需要在每个页面中成对使用    

### 5.3 采集应用点击事件信息
具体需要采集哪些点击事件信息，详见应用管理-预埋点事件中的梳理

**eventInfo (Context mContext, String eventID, Map<String, Object> eventProperty)**

说明：自定义点击事件的接口
参数：

* mContext  上下文
* eventID  事件ID
* eventProperty  事件自定义属性

示例：

```java
Map<String, Object> customMap = new HashMap<String, Object>();
customMap.put("key1", "value1");
customMap.put("key2", "value2");
EguanMonitorAgent.getInstance().eventInfo(SecondActivity.this, "事件ID",customMap);
```

调用方法：

```java
EguanMonitorAgent.getInstance().eventInfo(SecondActivity.this, "事件ID", customMap);
```

### 5.4 采集用户登录退出信息

在用户登录之时，具体需要上传哪些用户属性，详见应用管理-预上传用户属性中的梳理

**onProfileSignOn(Context mContext,EGUser user)**

说明：用户登录统计
参数：

* EGUser 一个为统计用户信息方便建立的JaveBean对象

示例：

```java
// 新建一个用户
// 参数传值为用户的userID,为必添项
EGUser.Builder builder = new EGUser.Builder("00001");

// 设置用户的预定义属性信息
EGUser user = builder.setUserName("eguan")
  .setBirthday("2016-11-11")
  .setQQ("587234")
  .setEmail("587234@qq.com")
  .setPhoneNumber("18888888888")
  .setSex("man")
  .setUserProvider("QQ")
  .setWechatId("18888888888")
  .setWeiBoId("587234@qq.com");

// 用户信息自定义属性
HashMap<String,String> propertyDictionarys = new HashMap<String, String>();
propertyDictionarys.put("level","20");
propertyDictionarys.put("age","20");

// 设置用户自定义属性信息
builder.setUserPropertyDictionary(propertyDictionarys);

// 生成新建的用户对象
EGUser user = builder.build();

// 调用用户登录接口
EguanMonitorAgent.getInstance().onProfileSignOn(getApplicationContext(),user)
```

调用方法：

```java
EguanMonitorAgent.getInstance().onProfileSignOn(context,user);
```

**onProfileSignUpdate(Context mContext,EGUser user)**

说明：用户更新
参数：

* EGUser 一个为统计用户信息方便建立的JaveBean对象

示例：

```java
// 更新一个用户
// 参数传值为用户的userID,为必添项
EGUser.Builder builder = new EGUser.Builder("00001");

// 设置用户的预定义属性信息
EGUser user = builder.setUserName("hello world")
	.setEmail("12345@qq.com")
	.setPhoneNumber("16666666666")
	.setWechatId("16666666666")
	.setWeiBoId("12345@qq.com")
	.setUserPropertyDictionary(propertyDictionarys);

// 用户信息自定义属性
HashMap<String,String> propertyDictionarys = new HashMap<String, String>();
propertyDictionarys.put("level","30");
propertyDictionarys.put("age","30");

// 设置用户自定义属性信息
builder.setUserPropertyDictionary(propertyDictionarys);

// 生成更新的用户对象
EGUser user = builder.build();

// 调用用户登录接口
EguanMonitorAgent.getInstance().onProfileSignUpdate(getApplicationContext(),user)
```

调用方法：

```java
EguanMonitorAgent.getInstance().onProfileSignUpdate(context,user);
```

**onProfileSignOff(Context mContext)**

说明：用户登出统计
参数：

* mContext  上下文

调用方法：

```java
EguanMonitorAgent.getInstance().onProfileSignOff(context);
```

### 5.5 采集Hybrid应用页面信息

如您需要进行对Hydrid应用内相关H5页面进行数据统计，请使用WebView控件及其子控件，显示H5界面时，需要WebView在onLoadResoure中调用如下方法

**onLoadResource (WebView webView, String url)**

说明：Hybrid应用页面信息统计
参数：

* webView WebView显示对象
* url 需要显示的链接

示例：

```java
view.setWebViewClient(new WebViewClient() {
  @Override
  public void onLoadResource(WebView view, String url) {
    EguanMonitorAgent.getInstance().onLoadResource(view,url);
  }
});
```
调用方法：

```java
EguanMonitorAgent.getInstance().onLoadResource(view, url);
```

**H5页面需要统计的数据调用接口与操作如下：**

导入JS-SDK:

> 最新版本：[AnalysysFangzhou_JS_APP_SDK](http://ark.analysys.cn/sdk/AnalysysFangzhou_JS_APP_SDK.min.js) 


将JS-SDK下载到相应目录;

将以下代码修改目录后,复制到您所需分析页面中的 < head > 和 < /head > 标签之间即可。安装成功后，所有网址下的行为数据都将会被收集

```html
<script src="AnalysysFangzhou_JS_APP_SDK.min.js" async ></script>
```

**fz.pageTAG({Key:value,...})**

说明：设置页面属性


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



### 5.6 添加URL Scheme功能
在Manifests中的主activity里面添加<data android:host="com.eguan.cn" android:scheme="h5sdk" />用以h5界面通过访问带有该scheme及host信息的链接调起我们的app，
在主activity中的onCreate回调方法中调用此功能用于统计营销推广效果，可以统计到通过推广页面唤醒app的用户数量，同时需要调用配置H5-SDK相关方法

**onCreate(Activity activity, PushInfoManager.PushListener listener)**

说明：通过URL Scheme调起App
参数：

* activity  应用主页面
* listener  活动推送的后续自定义属性和行为的监听

示例：

```java
PushInfoManager.PushListener listener = new PushInfoManager.PushListener() {
    @Override
    public void execute(String action, String jsonParams) {
        //自定义属性和行为处理逻辑
    }
};
EguanMonitorAgent.getInstance().onCreate(MainActivity.this, listener);
```

调用方法：

```java
EguanMonitorAgent.getInstance().onCreate(MainActivity.this, listener);
```

### 5.7 消息推送
首先需要在对应的消息推送平台上开通推送并做相应配置，<br/>
然后在应用管理-推送配置中绑定相应平台的配置，比如，极光推送的配置，如下图<br/>
![GPS](http://file1.analysys.cn/fangzhou/3f51846b46dde479fe777912dcde6abc_1782x220.png)

**enablePush(Context mContext , String provider,String pushId)**

说明：打开provider对应平台的消息推送功能<br/>
参数：

* mContext  上下文
* provider  推送服务方，易观SDK支持的平台,可从PushProvider中查询
* pushId 对应服务方的关于该移端端的唯一ID

调用方法，以极光为例：

```java
EguanMonitorAgent.getInstance().enablePush (context, PushProvider.JPUSH, pushId);
```

**disablePush(Context mContext , String provider)**

说明：关闭provider对应平台的消息推送功能<br/>
参数：

* mContext，上下文
* provider, 推送服务方，易观SDK支持的平台，可从PushProvider中查询

调用方法，以极光为例：

```
EguanMonitorAgent.getInstance().disablePush (context,PushProvider.JPUSH);
```

预置推送ID-推送名称-调用方式对应表：

  |  推送ID  |  推送名称  |  调用方式  |
  | ------------- |:-------------:|
  |  GETUI  |  个推  |  PushProvider.GETUI  | 
  |  JPUSH  |  极光  |  PushProvider.JPUSH  |
  |  BAIDU  |  百度  |  PushProvider.BAIDU  |
  |  XINGE  |  信鸽  |  PushProvider.XINGE  |
  |  UMENG  |  友盟  |  PushProvider.UMENG  |
  |  XIAOMI  |  小米  |  PushProvider.XIAOMI  |
  |  HUAWEI  |  华为  |  PushProvider.HUAWEI  |
  |  YUNBA  |  云巴  |  PushProvider.YUNBA  |
  |  ALI  |  阿里  |  PushProvider.ALI  |
  |  IXINTUI  |  爱心推  | PushProvider.IXINTUI  |
  |  ARROWNOCK  |  箭扣  | PushProvider.ARROWNOCK | 

**addCampaign (Context mContext, String campaign, boolean isClick)**

说明：打开provider对应平台的消息推送功能<br/>
参数：

* mContext  上下文
* campaign  该对象为一个jsonObjecdt的String格式，其具体内容，为推送服务下发下来的JsonString，直接传入addCampaign接口即可
* isClick   活动通知是否点击打开

调用方法：

```java
/*

此字符串,为接收推送服务器下发来的Json格式,

其中主Key为”EGPUSH_CINFO”,主键为一个JsonArray对象,
对象内分为5组Key,Value,
其中Key值分别表示为:

  CAMPID:为活动推广播ID
  ACTIONTYPE:活动推送的后续行为类型，1-打开应用，2-跳转到指定页面，3-打开web链接，4-自定义行为
  MSGTYPE:消息的类型
  MSGID:消息ID
  CPD:用户自定义的Key,Value键值对,是对用户需求的扩展字段

*/

String = "{\n" +
         "    \"EGPUSH_CINFO\": {\n" +
         "        \"ACTIONTYPE\": \"1\",\n" +
         "        \"ACTION\": \"\",\n" + 
         "        \"OFFERID\": \"com.eguan.sdkdemo.SecondActivity\",\n" +
         "        \"MSGTYPE\": \"1\",\n" +
         "        \"MSGID\": \"123456\",\n" +
         "        \"CPD\": {\n" +
         "            \"key1\": \"value1\",\n" +
         "            \"key2\": \"value2\"\n" +
         "        }\n" +
         "    }\n" +
         "}";
EguanMonitorAgent.getInstance().addCampaign (context, campaignString, isClick);
```

**addCampaign (Context mContext, String campaign, boolean isClick, PushInfoManager.PushListener listener)**

说明：打开provider对应平台的消息推送功能<br/>
参数：

* mContext  上下文
* campaign  该对象为一个jsonObjecdt的String格式，其具体内容，为推送服务下发下来的JsonString，直接传入addCampaign接口即可
* isClick   活动通知是否点击打开
* listener  活动推送的后续自定义属性和行为的监听

调用方法：

```
EguanMonitorAgent.getInstance().addCampaign(context, campaign, isClick, listener);
```

以下详细说明如何集成第三方推送服务

#### 5.7.1 获取设备推送 ID

首先，开发者需要在 App 中集成第三方推送系统的 SDK，并在 App 初始化过程中获取设备推送 ID，并保存在方舟分析的用户信息中。以下简要说明集成第三方推送系统的集成方式。

##### 极光推送

请参考极光推送[《Android SDK 文档》](https://docs.jiguang.cn/jpush/client/Android/android_sdk/)，将极光推送的 SDK 集成到 App 中。开发者需要将 App 的设备推送 ID 保存在方舟分析的用户信息中。方舟分析的用户分群等分析功能会使用该设备推送 ID 进行用户关联。请参考参考 [《易观方舟 Android SDK 使用说明》](http://ark.analysys.cn/docs/manual/android.html) 将方舟分析 SDK 集成到 App 中。

极光推送使用唯一的 Registration Id 标示设备。开发者需要在 Manifest 中定义 Receiver 接受极光推送的广播，通过 cn.jpush.android.intent.REGISTRATION 消息获取 Registration Id。

```
  <receiver
    android:name="Your Receiver"
    android:enabled="true">
    <intent-filter>
      <action android:name="cn.jpush.android.intent.REGISTRATION" />
      <action android:name="cn.jpush.android.intent.MESSAGE_RECEIVED" />
      <action android:name="cn.jpush.android.intent.NOTIFICATION_RECEIVED" />
      <action android:name="cn.jpush.android.intent.NOTIFICATION_OPENED" />
      <category android:name="You package Name" />
    </intent-filter>
  </receiver>
```

广播 cn.jpush.android.intent.REGISTRATION 的 Intent 参数中JPushInterface.EXTRA_REGISTRATION_ID 为 Registration Id ，开发者在极光推送初始化完成后，在广播中获取 Registration Id 并将他保存在方舟分析的用户信息中：
 ```
 public void onReceive(Context context, Intent intent) {
    // 收到极光推送初始化成功的广播
    if (JPushInterface.ACTION_REGISTRATION_ID.equals(intent.getAction())) {
        // 获取极光推送的 RegistrationId
        final String registrationId = intent.getExtras().getString(JPushInterface.EXTRA_REGISTRATION_ID);
        // 将 Registration Id 存储在易观的用户信息中
        EguanMonitorAgent.getInstance().enablePush(context, PushProvider.JPUSH, registrationId);
    }
}
```
##### 百度云推送

请参考百度云推送[《Android SDK 文档》](http://push.baidu.com/doc/android/api)，将百度云推送的 SDK 集成到 App 中。开发者需要将 App 的设备推送 ID 保存在方舟分析的用户信息中。方舟分析的用户分群等分析功能会使用该设备推送 ID 进行用户关联。请参考参考 [《易观方舟 Android SDK 使用说明》](http://ark.analysys.cn/docs/manual/android.html) 将方舟分析 SDK 集成到 App 中。

百度云推送使用唯一的 Registration Id 标示设备。开发者需要在 Manifest 中定义 Receiver 接受百度云推送的广播，通过 com.baidu.android.pushservice.action.RECEIVE 消息获取 Registration Id。

```
  <receiver android:name="Your Receiver">   
      <intent-filter>   
          <!-- 接收push消息 -->   
          <action android:name="com.baidu.android.pushservice.action.MESSAGE" />   
          <!-- 接收bind,unbind,fetch,delete等反馈消息 -->   
          <action android:name="com.baidu.android.pushservice.action.RECEIVE" />   
          <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />   
      </intent-filter>   
  </receiver>   
```
广播 com.baidu.android.pushservice.action.RECEIVE 的 Intent 参数中包含 Channel Id ，开发者在百度推送初始化完成后，在广播中获取 Channel Id 并将他保存在方舟分析的用户信息中：
```
    @Override
    public void onBind(Context context, int errorCode, String appid,
                       String userId, String channelId, String requestId) {
        if (errorCode == 0) {
            // 绑定成功
            Log.d(TAG, "绑定成功");
            EguanMonitorAgent.getInstance().enablePush(context, PushProvider.BAIDU, channelId);
        }
    }
```
<!--
    ##### 小米推送
    
    请参考小米推送[《Android SDK 文档》](https://dev.mi.com/console/doc/detail?pId=41)，将小米推送送的 SDK 集成到 App 中。开发者需要将 App 的设备推送 ID 保存在方舟分析的用户信息中。方舟分析的用户分群等分析功能会使用该设备推送 ID 进行用户关联。请参考参考 [《易观方舟 Android SDK 使用说明》](http://ark.analysys.cn/docs/manual/android.html) 将方舟分析 SDK 集成到 App 中。
    
    小米推送使用唯一的 Registration Id 标示设备。开发者需要在 Manifest 中定义 Receiver 接受小米推送送的广播，通过广播消息获取 Registration Id。
    
    ```
    <receiver
        android:name="Your Receiver"
        android:exported="true">
        <intent-filter>
            <action android:name="com.xiaomi.mipush.RECEIVE_MESSAGE" />
        </intent-filter>
        <intent-filter>
            <action android:name="com.xiaomi.mipush.MESSAGE_ARRIVED" />
        </intent-filter>
        <intent-filter>
            <action android:name="com.xiaomi.mipush.ERROR" />
        </intent-filter>
    </receiver>
    ```
    
    
    广播消息的MiPushCommandMessage参数中可以获取到Registration Id ，开发者在小米推送初始化完成后，在广播中获取 Registration Id 并将他保存在方舟分析的用户信息中：
    ```
    @Override
        public void onReceiveRegisterResult(Context context, MiPushCommandMessage message) {
            String command = message.getCommand();
            List<String> arguments = message.getCommandArguments();
            String mRegId = ((arguments != null && arguments.size() > 0) ? arguments.get(0) : null);
    
            if (MiPushClient.COMMAND_REGISTER.equals(command)) {
                if (message.getResultCode() == ErrorCode.SUCCESS) {
                    EguanMonitorAgent.getInstance().enablePush(context, PushProvider.XIAOMI, mRegId);
                }
            }
        }
    ```
-->
#### 5.7.2 App 推送效果追踪

开发者可以使用方舟分析追踪推送效果。当用户收到推送消息时，通过方舟分析 SDK 上报消息推送成功事件，并通过漏斗分析等功能，将用户在推送前的行为和推送后的行为关联起来。
我们可以在 App 中监听第三方消息推送服务的广播，当收到消息时调用方舟分析的addCampaign接口记录 App 推送成功事件；当用户点击消息打开 App 或执行其他行为时，调用addCampaign接口记录 App 消息打开事件。
以下简要介绍如何在 App 中记录上述App消息推送成功，追踪极光推送的推送效果。

##### 极光推送

如极光推送文档所述，用户需要在 Receiver 中处理极光推送的广播，并在收到消息时使用方舟分析 SDK 追踪推送成功的事件。
```
 public void onReceive(Context context, Intent intent) {
    if (JPushInterface.ACTION_REGISTRATION_ID.equals(intent.getAction())) {
        // 极光推送注册成功通知，如前文所述，获得 Registration Id 并保存在 易观的用户信息中 
    } else if (JPushInterface.ACTION_MESSAGE_RECEIVED.equals(intent.getAction())) {
        // 用户收到了自定义消息，自定义消息不会展示在通知栏，只透传给 App
    } else if (JPushInterface.ACTION_NOTIFICATION_RECEIVED.equals(intent.getAction())) {
        // 用户收到了推送消息，使用易观追踪 "App 消息推送成功" 事件
        EguanMonitorAgent.getInstance().addCampaign(context, intent.getExtras().getString(JPushInterface.EXTRA_EXTRA), false, listener);
    } else if (JPushInterface.ACTION_NOTIFICATION_OPENED.equals(intent.getAction())) {
        // 用户点击打开消息，使用收到推送消息类似的方法，使用易观记录 "App 打开消息" 事件
	EguanMonitorAgent.getInstance().addCampaign(context, intent.getExtras().getString(JPushInterface.EXTRA_EXTRA), true, listener);
    }
}

// 推送的易观自定义事件和参数的回调
private PushInfoManager.PushListener listener = new PushInfoManager.PushListener() {
@Override
public void execute(String action, String jsonParams) {
        // 开发者自定义的逻辑处理
    }
}
```
##### 百度云推送

如百度云推送文档所述，用户需要在 Receiver 中处理百度云推送的广播，并在收到消息时使用方舟分析 SDK 追踪推送成功的事件。
```    
// 获取到活动推送通知消息   
@Override   
public void onNotificationArrived(Context context, String title,   
                                  String description, String customContentString) {   
    // 自定义内容获取方式，mykey和myvalue对应通知推送时自定义内容中设置的键和值   
    if (!TextUtils.isEmpty(customContentString)) {   
        JSONObject customJson = null;   
        try {   
            customJson = new JSONObject(customContentString);   
            String myvalue = null;   
            if (!customJson.isNull("EGPUSH_CINFO")) {  
                myvalue = customJson.getString("EGPUSH_CINFO");   
                EguanMonitorAgent.getInstance().addCampaign(context, myvalue, false, listener);   
            }   
        } catch (JSONException e) {   
            // TODO Auto-generated catch block   
            e.printStackTrace();   
        }   
    }   
}   

// 获取到活动推送通知点击消息   
@Override   
public void onNotificationClicked(Context context, String title,   
                                  String description, String customContentString) {   
    // 自定义内容获取方式，mykey和myvalue对应通知推送时自定义内容中设置的键和值   
    if (!TextUtils.isEmpty(customContentString)) {   
        JSONObject customJson = null;   
        try {   
            customJson = new JSONObject(customContentString);   
            String myvalue = null;   
            if (!customJson.isNull("EGPUSH_CINFO")) {   
                myvalue = customJson.getString("EGPUSH_CINFO");   
                EguanMonitorAgent.getInstance().addCampaign(context, myvalue, true, listener);   
            }   
        } catch (JSONException e) {   
            // TODO Auto-generated catch block   
            e.printStackTrace();   
        }   
    }   
}   

// 推送的易观自定义事件和参数的回调
private PushInfoManager.PushListener listener = new PushInfoManager.PushListener() {   
    @Override   
    public void execute(String action, String jsonParams) {   
        // 开发者自定义的逻辑处理   
    }   
}   
```
<!--
    ##### 小米推送
    
    如小米推送文档所述，用户需要在 Receiver 中处理小米推送的广播，并在收到消息时使用方舟分析 SDK 追踪推送成功的事件。
    ```
    // 获取到活动推送通知消息   
    @Override   
    public void onNotificationMessageArrived(Context context, MiPushMessage message) {   
        EguanMonitorAgent.getInstance().addCampaign(context, message.getExtra().get("EGPUSH_CINFO"), false, listener);   
    }   
    
    // 获取到活动推送通知点击消息    
    @Override     
    public void onNotificationMessageClicked(Context context, MiPushMessage message) {   
        EguanMonitorAgent.getInstance().addCampaign(context, message.getExtra().get("EGPUSH_CINFO"), true, listener);   
    }   
    
    // 推送的易观自定义事件和参数的回调
    private PushInfoManager.PushListener listener = new PushInfoManager.PushListener() {
    @Override
    public void execute(String action, String jsonParams) {
            // 开发者自定义的逻辑处理
        }
    }
    ```
-->
## 6. 代码混淆

如果您启用了混淆，请在你的proguard-rules.pro中加入如下代码：

```
-keep class com.eguan.** {
  public *;
}
-dontwarn com.eguan.**
```

## 7. 开启Debug模式
至此SDK已经集成完毕，为了可以快速在页面上看到应用回传的日志，来校验埋点的正确性和完整性，引入了Debug模式。

**setDebugMode(Context mContext, boolean flag)**

参数：

* flag
测试阶段设置为true，数据将发送到测试服务器；
正式发布是设置为false，数据将发送到正式服务器。

> flag默认设置为false
请确保 setDebugMode方法在initEguan初始化之后调用
APP正式对外发布时，切记确保flag为false

调用方法：

```
EguanMonitorAgent.getInstance().setDebugMode (context, true);
```

## 8. 验证SDK集成是否正确

通过以下几步完成数据准确性和完整性的验证：

1）开启Debug模式，在真机上运行测试应用，对相关埋点的地方点击操作；

2）可以在控制台查看初始化/发送/发送成功或失败的对应日志，日志使用的tag为"APP_SDKTAG"；

3）系统会默认校验回传日志是否满足在预埋点事件和预上传用户属性中梳理的需求；将在元事件管理和用户属性管理页面显示校验结果；

4）根据结果提示做相应调整，当状态都为 √ 的时候，表示集成正确，关闭Debug模式，即可发布应用（不同渠道包注意填写不同的渠道ID）。

## <div id="tool"></div> 9. 多渠道打包工具

### 9.1 下载工具

[EguanMultiChannelTool](https://ark.analysys.cn/sdk/EguanMultiChannelTool.zip)

### 9.2 使用前提条件

需要集成 3.7.8|20171128 以上版本 SDK

### 9.3 使用说明

1) 将已经签好名的1个或多个apk文件放进需要打包的同级目录下    

2) 创建多个与apk同名的的渠道信息txt文件,格式为每一行为一个渠道标识,以换行结束    

3) 运行eguan_multi_channel_tool.py(支持python2.7及python3.5以上版本)    

4) 在同级目录下会有个output文件夹,里面有多个子文件夹与多个apk一一对应,有最新生成的多渠道包    

### 9.4 注意事项

1) 请使用txt格式的文本文件配置渠道ID

2) 配置的渠道ID请使用初始化SDK中指定的渠道ID，便于做渠道数据统计

