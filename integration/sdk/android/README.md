# Android SDK

## Android SDK 使用说明

Android SDK 用于 Android 原生 App，集成前请先下载 SDK

{% hint style="info" %}
[Releases包及更新说明](https://github.com/analysys/ans-android-sdk/releases)
{% endhint %}

| Jar包 | 功能描述 | 是否必选 | 服务端版本 |
| :---: | :---: | :---: | :--- |
| analysys\_core\_xxx.jar\(aar\) | 基础模块 | 必选 | 全部 |
| analysys\_visual\_xxx.jar\(aar\) | 可视化热图模块 | 可选 | 热图模块适用方舟V4.3.0及以上版本 |
| analysys\_push\_xxx.jar\(aar\) | 推送模块 | 可选 | 全部 |
| analysys\_encrypt\_xxx.jar\(aar\) | 加密模块 | 可选 | CBC模式适用方舟V4.2.7及以上版本 |
| analysys\_allgro-xxx.aar | 全埋点 | 可选 | 适用方舟V4.5.3及以上版本 |
| analysys\_arkanalysys-xxx.aar | 全功能包含基础模块、可视化热图模块、推送模块、加密模块、全埋点模块 | 可选 | 适用方舟V4.5.3及以上版本 |

注意：请您根据自身业务需求来引用相关的SDK。

### 快速集成

如果您是第一次使用易观方舟产品，可以通过阅读本文快速了解此产品。

#### 1. 选择集成方式

目前我们提供了Eclipse SDK 集成、AndroidStudio SDK 集成的方式。

#### 2. 配置 Manifest

在AndroidManifest.xml文件中配置权限、AppKey和Channel。

#### 3. 设置初始化接口

通过初始化接口配置您的AppKey，配置Channel。

#### 4. 设置上传地址

通过`setUploadURL`接口设置您上传数据的地址。

#### 5. 设置采集页面或事件

通过手动埋点，设置需要采集的页面或事件。

#### 6. 打开Debug模式查看日志

通过设置Debug模式，开/关 log 查看日志。

通过以上6步您即可验证SDK是否已经集成成功。更多接口说明请您查看API文档。

## 集成配置

### 选择集成工具

{% tabs %}
{% tab title="AndroidStudio SDK 集成" %}
**注意：aar集成方式需要android sdk V4.4.0以上版本才支持，V4.4.0以下版本集成可参考** [**方舟gradle集成方式**](https://docs.analysys.cn/ark/integration/sdk/android/fang-zhou-gradle-ji-cheng-fang-shi) **；本地aar配置和远程aar配置只需要选择一种即可。**

**本地aar配置**

  
         ****`compile files('libs/analysys_xxxx.aar')`

          如本地aar配置还有有问题：参考 [方舟gradle集成方式](https://docs.analysys.cn/ark/integration/sdk/android/fang-zhou-gradle-ji-cheng-fang-shi)

**远程aar配置**

  
     ****`dependencies  
{  
    //添加 analysys-arkanalysys SDK 依赖  
    compile('cn.com.analysys:analysys-arkanalysys:4.4.4')  
}`
{% endtab %}

{% tab title="Eclipse SDK 集成" %}
1.将需要的 jar 包拷贝到本地工程 libs 子目录下；在Eclipse中右键工程根目录，选择 `property —> Java Build Path —> Libraries` ，然后点击 Add External JARs... 选择指向 jar 的路径，点击 OK，即导入成功。（ADT17 及以上不需要手动导入）
{% endtab %}
{% endtabs %}

### 配置 Manifest

AndroidStudio SDK使用aar集成用户无需配置Manifest；AppKey、Channel 在Mainfest可选配置；

远程jar包和Eclipse SDK集成用户需要按照下面文档进行配置：

AndroidManifest.xml文件需要配置内容包括权限、provider、AppKey、Channel 和 .crt 格式证书名称（用于https网络通信）。

1. provider在SDK 4.4.0以上版本为必选配置，否则SDK无法正常工作；SDK 4.4.0以下版本无需配置；
2. AppKey和Channel也可以通过init\(\)初始化接口配置，两种方式配置任意一种即可。
3. 如果key使用以上两种方式配置，则两值必须相同，否则设置失败。
4. 如果channel使用两种方式，则优先取xml文件内配置的值。
5. https 网络通信默认信任所有证书，如需使用 .crt 格式证书，请在 xml 配置证书名称，并将 .crt 格式证书添加到项目 assets 目录下。 示例如下：

```markup
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />

<application >

    ......
    <!-- provider中添加的内容为4.4.0以上版本新增，请注意添加,否则sdk不能正常使用，必选
  -->
    <provider
    android:name="com.analysys.database.AnsContentProvider"
    android:authorities="[应用包名].AnsContentProvider"
    android:enabled="true"
    android:exported="false" />
    <!--  设置appKey  可选 -->
    <meta-data
    android:name="ANALYSYS_APPKEY"
    android:value="4g6dp3d9fh5r0s3jr87j3ej94k04" />
    <!--  设置渠道  可选-->
    <meta-data
    android:name="ANALYSYS_CHANNEL"
    android:value="WanDouJia" />
    <!--  服务端加密证书（在assets上预制）  可选-->
    <meta-data
    android:name="ANALYSYS_KEYSTONE"
    android:value="analysys.crt"/>

</application>

```

权限说明:

* INTERNET（必须）允许程序连接网络，与服务器进行数据传输
* ACCESS\_NETWORK\_STATE（必须）检测网络类型，用于区分用户网络状态 2G/3G/4G/WIFI
* READ\_PHONE\_STATE（必须）获取用户设备 IMEI，使用 IMEI 和 MAC 地址做设备唯一标识
* ACCESS\_WIFI\_STATE（必须）获取用户设备 MAC 地址，用做设备唯一标识

### 混淆设置

如果您的应用使用了代码混淆，请添加如下配置，以避免错误混淆导致SDK不可用。

```text
-dontwarn com.analysys.**
-keep class com.analysys** {*;}

-keepclassmembers class * {
   public <init> (org.json.JSONObject);
}

-keepclassmembers enum * {
    public static **[] values();
    public static ** valueOf(java.lang.String);
}

-keep public class [您的应用包名].R$*{
public static final int *;
}
```

## 基础模块

以下接口生效依赖于基础SDK模块，需集成基础SDK相关analysys\_core\_xxx文件，请正确集成。

### 初始化接口

建议在应用 Application 中调用 SDK 初始化接口 init\(\)， 配置 AppKey、Channel，注意：初始化接口为必须调用接口。 接口如下:

```java
public static void init(Context context, AnalysysConfig config);
```

* context ：应用上下文对象
* config ：为自定义实体 bean，用于设置初始化属性值，目前config支持的属性有：
  1. AppKey：在网站获取的 AppKey
  2. channel：应用下发的渠道
  3. autoProfile ：设置是否追踪新用户的首次属性，false：不追踪新用户的首次属性，true：追踪新用户的首次属性\(默认\)
  4. encryptType ：设置数据上传时的加密方式，目前只支持 AES 加密，AES 加密分为EncryptEnum.AES（128位密钥，ECB 加密模式）和 EncryptEnum.AES\_CBC（128位密钥，CBC 加密模式）。
  5. setAutoInstallation ：设置是否进行渠道跟踪，`true`为跟踪，`false`为不跟踪（默认`false`）。
  6. setAllowTimeCheck ：是否允许时间校准，默认值：`false`
  7. setMaxDiffTimeInterval ：最大允许时间误差，单位：秒，默认值：`30`秒
  8. setAutoHeatMap ：是否开启热图，默认值：`false`
  9. setAutoTrackPageView ：pageView自动上报开关，默认值：`true`
  10. setAutoTrackFragmentPageView：是否开启全埋点fragment页面自动上报， 默认值：`false`
  11. setAutoTrackClick：是否开启全埋点点击事件， 默认值：`false`
  12. setEnableException：是否允许崩溃追踪，默认值：`false`

{% hint style="info" %}
EncryptEnum.AES\_CBC模式适用方舟V4.2.7及以上版本
{% endhint %}

示例：

```java
public class AnalysysApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        AnalysysConfig config = new AnalysysConfig();
        // 设置key，77a52s552c892bn442v721为样例数据，请根据实际情况替换相应内容
        config.setAppKey("77a52s552c892bn442v721");
        // 设置渠道
        config.setChannel("豌豆荚");
        // 设置追踪新用户的首次属性
        config.setAutoProfile(true);
        // 设置进行渠道跟踪
        config.setAutoInstallation(true);
        // 设置上传数据使用AES CBC模式加密，需添加加密模块
        config.setEncryptType(EncryptEnum.AES_CBC);
        // 设置服务器时间校验
        config.setAllowTimeCheck(true);
        // 时间最大允许偏差为5分钟
        config.setMaxDiffTimeInterval(5 * 60);
        // 设置采集热图信息
        config.setAutoHeatMap(true);
        // 设置pageView自动上报总开关
        config.setAutoTrackPageView(true);
        // 设置fragment的pageView自动上报开关
        config.setAutoTrackFragmentPageView(true);
        // 设置控件点击自动上报总开关
        config.setAutoTrackClick(true);
        // 调用初始接口
        AnalysysAgent.init(this, config);

    }
}

```

### 设置上传地址

自定义上传地址，接口设置后，所有事件信息将上传到该地址。 接口如下：

```java
public static void setUploadURL(Context context, String url);
```

* context：应用上下文对象
* url：数据上传地址，格式为 `scheme://host + :port`\(不包含`/`后的内容\)。**scheme** 必须以 `http://` 或 `https://` 开头，**host** 只支持域名和 IP，取值长度 1 - 255 字符，**port** 端口号必须携带

示例：

```java
public class AnalysysApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        AnalysysConfig config = new AnalysysConfig();
        // 设置key，77a52s552c892bn442v721为样例数据，请根据实际情况替换相应内容
        config.setAppKey("77a52s552c892bn442v721");
        // 设置渠道
        config.setChannel("豌豆荚");
        // 设置追踪新用户的首次属性
        config.setAutoProfile(true);
        // 设置使用AES加密
        config.setEncryptType(EncryptEnum.AES);
        // 调用初始接口
        AnalysysAgent.init(this, config);
        //设置自定义上传地址为 scheme://host + :port
        AnalysysAgent.setUploadURL(mContext,"/*设置为实际地址*/");
    }
}
```

### Debug 接口

Debug 接口主要用于开发者测试。可以开/关日志，通过logcat查看tag为`analysys`的Log日志。 接口如下：

```java
public static void setDebugMode(Context context, int debugMode);
```

* context：应用上下文对象
* debugMode：debug 模式，默认关闭状态。发布版本时 debugMode 模式设置为`0`
  * `0`：表示关闭 Debug 模式
  * `1`：表示打开 Debug 模式，但该模式下发送的数据仅用于调试，不计入平台数据统计
  * `2`：表示打开 Debug 模式，该模式下发送的数据可计入平台数据统计

     注意：若设置其他值则不生效,使用默认值。

示例：

```java
public class AnalysysApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();

        //  设置 打开 debug 模式
        AnalysysAgent.setDebugMode(this, 2);
        AnalysysConfig config = new AnalysysConfig();
        // 设置key，77a52s552c892bn442v721为样例数据，请根据实际情况替换相应内容
        config.setAppKey("77a52s552c892bn442v721");
        // 设置渠道
        config.setChannel("豌豆荚");
        // 设置追踪新用户的首次属性
        config.setAutoProfile(true);
        // 设置使用AES加密
        config.setEncryptType(EncryptEnum.AES);
        // 调用初始接口
        AnalysysAgent.init(this, config);
        //设置自定义上传地址为 scheme://host + :port
        AnalysysAgent.setUploadURL(mContext,"/*设置为实际地址*/");
    }
}
```

### App启动来源监测

此接口需在Activity基类或应用启动首页的onCreate生命周期进行设置。 接口如下：

```java
public static void launchSource(int source);
```

* context：应用上下文对象
* source：应用启动来源，默认值为 `1` ，
  * 1：默认值：通过图标\(icon\)启动应用
  * 2 ：传值为 2 ，通过点击推送消息\(msg\)启动
  * 3 ：传值为 3 ，通过DeepLink\(url\)启动
  * 5 ：传值为 5 ，通过其他方式启动

示例：

```java
public class BasicActivity extends AppCompatActivity {
  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);

    Intent intent = getIntent();
    if (intent != null) {
      Uri uri = intent.getData();
      if (uri != null) {
        // 判断如果是deepLink启动，设置启动来源为 3
        AnalysysCMB.launchSource(3);
      }
    }
  }
}
```

### 统计页面接口

页面跟踪，SDK 默认设置跟踪所有页面，支持自定义页面信息。 接口如下：

```java
public static void pageView(Context context,String pageName);
public static void pageView(Context context,String pageName, Map<String, Object> properties);
```

* context：应用上下文对象
* pageName：页面标识，为字符串，取值长度 1 - 255 字符
* properties：页面信息，properties 最多包含 100条，且 key 以字母或`$`开头，包含字母、数字、下划线和`$`，字母不区分大小写，`$`开头为预置事件/属性，取值长度 1 - 99 字符，不支持乱码和中文，value 支持类型：String/Number/boolean/字符串集合/字符串数组，若为数组或集合，则最多包含 100条，字符串单条取值长度 1 - 255 字符

示例：

```java
// 正在开展某个活动，需要统计活动页面；
AnalysysAgent.pageView(mContext,"活动页");

......

// 访问手机活动页面，活动页面内容为优惠出售iPhone手机，手机价格为5000元
Map<String, Object> info = new HashMap<>();
info.put("commodityName", "iPhone");
info.put("commodityPrice", 5000);
AnalysysAgent.pageView(mContext, "商品页", info);
```

#### 忽略部分页面自动采集

开发者可以设置某些页面不被自动采集，设置后自动采集时将会忽略这些页面。 接口如下:

```java
/**
 * PageView自动上报-设置页面级黑名单
 * @param pages 页面名称列表
 */
public static void setPageViewBlackListByPages(List<String> pages);
```

示例:

```java
List<String> pages = new ArrayList<String>();
pages.add("com.analysys.demo.activity.MainActivity");
// 忽略MainActivity页面自动采集
AnalysysAgent.setPageViewBlackListByPages(pages);
```

注意：设置忽略页面时，需要添加完整的页面路径（包名 + 类名名）

#### 自动采集添加自定义信息

若用户开启页面自动采集功能，可将自定义页面信息添加至`$pageview`事件中。SDK对外提供一个接口`ANSAutoPageTracker`供继承至`Activity`的类使用，若类遵循该协议，则须实现`registerPageProperties`方法，并将自定义参数返回，SDK会将此部分信息添加至`$pageview`事件的自定义参数中，且自定义参数优先级高于自动采集参数（即：相同key情况下，用户key会覆盖自动采集key）。

{% hint style="info" %}
前提：页面自动采集功能未手动关闭  
自定义参数只能获取页面生命周期onStart之前的参数，之后参数无法获取到。
{% endhint %}

```java
public interface ANSAutoPageTracker {
    /**
     * 获取对应页面的自定义参数
     */
    Map<String, Object> registerPageProperties();

    /**
     * 手动设置页面的url
     */
    String registerPageUrl();
}
```

示例:

```java
public class MainActivity extends AppCompatActivity implements ANSAutoPageTracker {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
    @Override
    public Map<String, Object> registerPageProperties() {
        //  $title为自动采集使用key，用户可覆盖
        Map<String, Object> properties = new HashMap<String, Object>();
        properties.put("$title", "详情页");
        return properties;
    }
    @Override
    public String registerPageUrl() {
        // 页面$url字段，将覆盖SDK默认字段
        return "HomePage";
    }
}
```

### 统计事件接口

用户行为追踪，可以设置自定义属性。 接口如下：

```java
public static void track(Context context, String eventName);
public static void track(Context context, String eventName, Map<String, Object> eventInfo);
```

* context：应用上下文对象
* eventName：事件ID，以字母或 `$` 开头，包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件/属性，最大长度 99字符，不支持乱码和中文
* eventInfo：自定义属性，用于对事件描述。eventInfo 最多包含 100条，且 key 以字母或 `$` 开头，包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件/属性，取值长度 1 - 99 字符，不支持乱码和中文，value 支持类型：String/Number/boolean/字符串集合/字符串数组，若为数组或集合，则最多包含 100条，字符串单条取值长度 1 - 255 字符

示例：

```java
// 添加事件
AnalysysAgent.track(mContext, "back");

......

// 用户购买手机
Map<String, Object> info = new HashMap<>();
info.put("type", "Phone");
info.put("name","Apple iPhone8");
info.put("money", 4000);
info.put("count",1);
AnalysysAgent.track(mContext, "buy", info);
```

### 匿名ID与用户关联

用户 id 关联接口。将需要绑定的用户ID 和匿名ID进行关联，计算时会认为是一个用户的行为。 接口如下：

```java
public static void alias(Context context, String aliasId);
```

* context：应用上下文对象
* aliasId：需要关联的用户ID。 取值长度 1 - 255 字符

示例：

```java
// 登陆账号时调用，只设置当前登陆账号即可和之前行为打通
AnalysysAgent.alias(mContext,"sanbo");
```

### 匿名ID设置

接口如下：

```java
public static void identify(Context context, String distinctId);
```

* context：应用上下文对象
* distinctId：唯一身份标识，取值长度 1 - 255 字符

示例:

```java
// 设置匿名ID为`fangke009901`,注意此方法需要在初始化之前调用
AnalysysAgent.identify(mContext,"fangke009901");
```

### 匿名ID获取

获取用户通过identify接口设置或自动生成的id。优先级如下:用户设置的id &gt; 代码自动生成的id

接口如下：

```java
public static String getDistinctId(Context context);
```

示例:

```java
// 获取匿名id
AnalysysAgent.getDistinctId(mContext);
```

### 用户属性设置

> 用户属性是一个标准的 K-V 结构，K 和 V 均有相应的约束条件，如不符合则丢弃该次操作。

约束条件如下：

> #### 属性名称 <a id="1.1"></a>

```text
以字母或`$`开头，包含字母、数字、下划线和`$`，字母不区分大小写，`$`开头为预置事件/属性，取值长度 1 - 99 字符，不支持乱码和中文
```

> #### 属性值 <a id="2.1"></a>

```text
支持部分类型：String/Number/boolean/字符串集合/字符串数组；若为字符串，则取值长度 1 - 255 字符；若为数组或集合，则最多包含 100条，且 key 约束条件与属性名称一致，value 取值长度 1 - 255 字符
```

#### 设置用户属性

给用户设置单个或多个属性，如果之前不存在，则新建，否则覆盖。 接口如下：

```java
//设置单个用户属性
public static void profileSet(Context context, String propertyName, Object propertyValue);
//设置多个用户属性
public static void profileSet(Context context, Map<String, Object> property);
```

* context：应用上下文对象
* propertyName：属性名称，约束见[属性名称](./#1.1)
* propertyValue：属性值，约束见[属性值](./#2.1)
* property：属性列表，约束见[属性名称](./#1.1)，[属性值](./#2.1)

示例：

```java
//设置用户的邮箱地址为yonghu@163.com
AnalysysAgent.profileSet(mContext,"Email","yonghu@163.com");

......

// 设置用户的邮箱和微信
Map<String, Object> info = new HashMap<>();
info.put("Email", "yonghu@163.com");
info.put("WeChatID", "weixinhao");
AnalysysAgent.profileSet(mContext, info);
```

#### 设置用户固有属性

设置用户的固有属性，只在首次设置时有效的属性。 如：应用的激活时间、首次登录时间等。如果被设置的用户属性已存在，则这条记录会被忽略而不会覆盖已有数据，如果属性不存在则会自动创建。 接口如下：

```java
public static void profileSetOnce(Context context, String propertyName, Object propertyValue);
public static void profileSetOnce(Context context, Map<String, Object> property);
```

* context：应用上下文对象
* propertyName：属性名称，约束见[属性名称](./#1.1)
* propertyValue：属性值，约束见[属性值](./#2.1)
* property：属性列表，约束见[属性名称](./#1.1)，[属性值](./#2.1)

示例：

```java
// 设置用户激活时间
AnalysysAgent.profileSetOnce(mContext,"activationTime",1521280551929);

// 设置用户性别和出生时间
Map<String, Object> setOnceProfile = new HashMap<>();
setOnceProfile.put("birth", 548798705000);
setOnceProfile.put("sex", "male");
AnalysysAgent.profileSetOnce(mContext, setOnceProfile);
```

#### 设置用户属性相对变化值

设置用户属性的相对变化值\(相对增加，减少\)，只能对数值型属性进行操作，如果这个 Profile 之前不存在，则初始值为 0。 接口如下：

```java
public static void profileIncrement(Context context, String propertyName, Number propertyValue);
public static void profileIncrement(Context context, Map<String, Number> property);
```

* context：应用上下文对象
* propertyName：属性名称，约束见[属性名称](./#1.1)
* propertyValue：属性值，约束见[属性值](./#2.1)
* property：属性列表，约束见[属性名称](./#1.1)，[属性值](./#2.1)

示例：

```java
//用户增加一岁
AnalysysAgent.profileIncrement(mContext,"age",1);

......

//用户玩某个游戏时间增加一年，游戏等级增加2
Map<String, Number> incrementProfile = new HashMap<>();
incrementProfile.put("gameAge", 1);
incrementProfile.put("gameRating", 2);
AnalysysAgent.profileIncrement(mContext, incrementProfile);
```

#### 增加列表类型的属性

用户列表属性增加元素。 接口如下：

```java
//增加列表类型的属性
public static void profileAppend(Context context, String propertyName, List<Object> propertyValue);
```

* context：应用上下文对象
* propertyName：属性名称，约束见[属性名称](./#1.1)
* propertyValue：属性值，约束见[属性值](./#2.1)

示例：

```java
//增加多个用户爱好
List<Object> list = new ArrayList<>();
list.add("PlayBasketball");
list.add("music");
AnalysysAgent.profileAppend(mContext, "hobby", list);
```

#### 删除设置的属性值

删除已设置的用户属性值。 接口如下：

```java
public static void profileUnset(Context context, String propertyName);
public static void profileDelete(Context context);
```

* context：应用上下文对象
* propertyName：属性名称，约束见[属性名称](./#1.1)

示例：

```java
//  删除当前用户单个属性值
AnalysysAgent.profileUnset(mContext, "age");

//  删除当前用户所有属性值
AnalysysAgent.profileDelete(mContext);
```

### 通用属性

> 通用属性是每次上传事件信息都会带有的属性，通用属性是一个标准的 K-V 结构，K 和 V 均有相应的约束条件，如不符合则丢弃该次操作。

约束条件如下:

> #### 属性名称 <a id="1"></a>

```text
以字母或 `$` 开头，包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件/属性，取值长度 1 - 99 字符，不支持乱码和中文
```

> #### 属性值 <a id="2"></a>

```text
支持部分类型：String/Number/boolean/字符串集合/字符串数组；若为字符串，则取值长度 1 - 255 字符；若为数组或集合，则最多包含 100条，且 key 约束条件与属性名称一致，value取值长度 1 - 255 字符
```

#### 注册通用属性

某一个体，在固定范围内，持续拥有的属性，每次数据上传都会携带。 接口如下:

```java
public static void registerSuperProperty(Context context, String superPropertyName, Object superPropertyValue);
public static void registerSuperProperties(Context context, Map<String, Object> superProperty);
```

* context：应用上下文对象
* superPropertyName：属性名称，约束见[属性名称](./#1)
* superPropertyValue：属性值，约束见[属性值](./#2)
* superProperty：属性列表，约束见[属性名称](./#1)，[属性值](./#2)

示例：

```java
// 在某视频平台，购买一年会员，该年内只需设置一次即可
AnalysysAgent.registerSuperProperty(mContext, "member","VIP");

......

// 小明在20岁的时候，购买了一年腾讯会员
Map<String, Object> property = new HashMap<>();
property.put("platform", "TX");
property.put("age", "20");
property.put("member", "VIP");
property.put("user", "xiaoming");
AnalysysAgent.registerSuperProperties(mContext, property);
```

#### 删除通用属性

根据属性名称，删除已设置过的通用属性。 接口如下：

```java
//删除单个通用属性
public static void unRegisterSuperProperty(Context context, String superPropertyName);
//清除所有通用属性
public static void clearSuperProperties(Context context);
```

* context：应用上下文对象
* superPropertyName：属性名称，约束见[属性名称](./#1)

示例：

```java
// 删除已经设置的用户年龄属性
AnalysysAgent.unRegisterSuperProperty(mContext, "age");

......

// 清除所有已经设置的通用属性
AnalysysAgent.clearSuperProperties(mContext);
```

#### 获取通用属性

查询获取通用属性。 接口如下：

```java
// 获取单个通用属性
public static Object getSuperProperty(Context context, String superPropertyName);
// 获取所有的通用属性
public static Map<String, Object> getSuperProperties(Context context);
```

* context：应用上下文对象
* superPropertyName：属性名称，约束见[属性名称](./#1)

示例：

```java
// 查看已经设置的"member"的通用属性
AnalysysAgent.getSuperProperty(mContext, "member");

......

// 查看所有已经设置的通用属性
AnalysysAgent.getSuperProperties(mContext);
```

### SDK 发送策略

#### 设置上传间隔时间

上传间隔时间设置，在 debug 模式关闭后生效。当前数据默认为实时上传，建议设置上传时间隔为`15s`，并需要与setMaxEventSize接口配套使用 ，当设置后，数据达到设定条数或时间触发上传。 接口如下：

```java
public static void setIntervalTime(Context context, long flushInterval;);
```

* context：应用上下文对象
* flushInterval：间隔时间（参数单位/秒），flushInterval 值大于 1

示例：

```java
// 如设置上传间隔时间为20秒
AnalysysAgent.setIntervalTime(mContext,20);
```

#### 设置事件最大上传条数

上传条数设置，在 debug 模式关闭后生效；当前数据默认为实时上传，建议设置上传的条数为 10条。 并需要与setIntervalTime接口配套使用接口，当设置后，数据达到设定条数或时间触发上传。接口如下：

```java
public static void setMaxEventSize(Context context, long size);
```

* context：应用上下文对象
* size：上传条数，size 值大于 1

示例：

```java
// 如设置上传条数为20条
AnalysysAgent.setMaxEventSize(mContext,20);
```

#### 本地缓存上限值

SDK 本地默认缓存数据的上限值为 10000条，最小值为100条，当达到此阈值值将会删除最早 10条数据。可以通过 `setMaxCacheSize` 方法来设定缓存数据的上限值（参数单位/条）。 接口如下：

```java
public static void setMaxCacheSize(Context context, long size)
```

* size：本地最多数据缓存条数

示例:

```java
//设置本地数据缓存上限值为2000条
AnalysysAgent.setMaxCacheSize(mContext,2000);
```

#### 手动上传

调用该接口立刻上传数据。 接口如下：

```java
public static void flush(Context context);
```

* context：应用上下文对象

示例：

```java
@Override
protected void onPause() {
    super.onPause();
    // 切换页面时想立即上传所有数据
    AnalysysAgent.flush(mContext);
}
```

### 获取预置属性

获取SDK默认采集的一些预置属性信息，接口如下：

```java
public static Map<String, Object> getPresetProperties(Context context) ;
```

示例：

```java
// 获取预置属性
Map<String, Object> properties = AnalysysAgent.getPresetProperties(mContext);
```

### 清除本地设置

清除本地现有的设置（包括 id 和通用属性）重新开始统计。 接口如下：

```java
public static void reset(Context context);
```

* context：应用上下文对象

示例：清除本地现有的设置，包括id和通用属性

```java
// 删除设置的id和通用属性
AnalysysAgent.reset(mContext);
```

## 可视化热图SDK接口

以下接口生效依赖于可视化热图模块，需集成可视化热图SDK相关analysys\_visual\_xxx.jar文件，请正确集成。

### 设置 webSocket 地址

设置服务器地址，用于可视化调试阶段连接 Websocket 服务器。 接口如下：

```java
public static void setVisitorDebugURL(Context context, String url);
```

* context：应用上下文对象
* url：websocket服务器地址，格式为 `scheme://host + :port`\(不包含 `/` 后的内容\)。**scheme** 必须以 `ws://` 或 `wss://` 开头，**host** 只支持域名和 IP，取值长度 1 - 255 字符，**port** 端口号必须携带

示例：

```java
//设置websocket服务器地址为 scheme://host + :port
AnalysysAgent.setVisitorDebugURL(mContext,"/*设置为实际地址*/");
```

### 设置请求埋点配置地址

设置服务器地址，用于可视化请求埋点配置信息。 接口如下：

```java
public static void setVisitorConfigURL(Context context, String url);
```

* context：应用上下文对象
* url：请求埋点配置的服务器地址，格式为 `scheme://host + :port`\(不包含 `/` 后的内容\)。**scheme** 必须以 `http://` 或 `https://` 开头，**host** 只支持域名和 IP，取值长度 1 - 255 字符，**port** 端口号必须携带

示例：

```java
// 设置请求埋点配置的服务器地址为 scheme://host + :port
AnalysysAgent.setVisitorConfigURL(mContext,"/*设置为实际地址*/");
```

## 热图采集

热图采集生效依赖于基础SDK模块，展示依赖于可视化热图模块，需集依赖于`analysys_core_xxx.jar(aar)、analysys_visual_xxx.jar(aar)`文件，请正确集成。

### 设置热图采集

控制是否采集用户点击热图信息。参考初始化代码如下：

```java
public class AnalysysApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        AnalysysConfig config = new AnalysysConfig();
        // 其他初始化带代码
        // 设置开启采集热图信息
            config.setAutoHeatMap(true);
        …………
        // 调用初始接口
        AnalysysAgent.init(this, config);

    }
}
```

### 设置热图页面黑名单

开发者可以设置某些页面不被热图事件自动采集，自动采集时将会忽略这些页面。

```java
public static void setHeatMapBlackListByPages(List<String> pages);
```

* pages：忽略上报页面全路径名称集合

示例:

```java
// 添加需要忽略页面的全路径名
List<String> pages = new ArrayList<>();
pages.add(MainActivity.class.getName());
pages.add(UserSettingActivity.class.getName());
// 设置热图页面级黑名单
AnalysysAgent.setHeatMapBlackListByPages(pages);
```

## 加密模块介绍 <a id="toc_44"></a>

加密模块生效依赖于加密SDK模块，需集成加密SDK相关analysys\_encrypt\_xxx.jar文件，请正确集成。

在初始化SDK时需设置加密方式，目前只支持 AES 加密，AES加密分为EncryptEnum.AES（128位密钥，ECB加密模式）和 EncryptEnum.AES\_CBC（128位密钥，CBC加密模式），如不设置此参数，数据上传不加密。

示例：

```java
// 设置 AES 加密 ECB 模式
config.setEncryptType(EncryptEnum.AES);
```

## 消息推送 SDK 接口

以下接口生效依赖于推送模块，需集成推送SDK相关analysys\_push\_xxx.jar文件，请正确集成。

### 简介

推送消息是一种常用的运营方法。在合适的时间把合适的内容通过合适的方式推送给合适的人群，不仅能促进用户活跃，也能极大提升用户对产品的体验。 易观推送现在支持极光、个推、百度、小米四个三方平台。支持通过易观开发者平台设置以下三种类型的推送消息内容: 注意：该接口依赖于推送模块

* 下发通知，点击通知，打开宿主 App
* 下发通知，点击通知，打开宿主 App 的特定页面
* 下发通知，点击通知，触发打开特定的 URL 页面

首先，开发者需要在 App 中集成第三方推送系统的 SDK，并在 App 初始化过程中获取设备推送 ID，并保存在方舟分析的用户信息中。以下简要说明集成第三方推送系统的集成方式\(在 4.4 中可参考各平台相关说明及示例代码\)

### 设置设备推送 ID

该接口用于设置推送平台（provider）提供的设备推送 ID。 接口如下：

```java
setPushID(Context mContext, String provider,String pushId)
```

* mContext：上下文
* provider：推送服务方，易观SDK支持的平台,可从 PushProvider 中查询
* pushId：对应服务方的关于该移端端的唯一 ID

调用方法，以极光为例：

```java
AnalysysAgent.setPushID(context, PushProvider.JPUSH, 极光推送对应的设备ID);
```

对应表：

| 推送ID | 推送平台 | 调用方式 |
| :--- | :--- | :--- |
| GETUI | 个推 | PushProvider.GETUI |
| JPUSH | 极光 | PushProvider.JPUSH |
| BAIDU | 百度 | PushProvider.BAIDU |
| XIAOMI | 小米 | PushProvider.XIAOMI |

### 统计活动推广信息

#### 追踪活动推广信息 API

追踪记录 provider 对应平台的消息推送的信息。 接口如下：

```java
trackCampaign (Context mContext, String campaign, boolean isClick)
```

* mContext：上下文
* campaign：该对象为一个 jsonObject 的 String 格式，其具体内容，为推送服务下发下来的 JsonString，直接传入 trackCampaign 接口即可
* isClick：活动通知是否点击打开

调用方法：

```java
AnalysysAgent.trackCampaign (context, campaignString, isClick);
```

#### 追加 PushListener 参数的追踪活动推广信息 API

```java
trackCampaign (Context mContext, String campaign, boolean isClick, PushListener listener)
```

* mContext：上下文
* campaign：该对象为一个 jsonObject 的 String 格式，其具体内容，为推送服务下发下来的 JsonString，直接传入 trackCampaign 接口即可
* isClick：活动通知是否点击打开
* listener：活动推送的后续自定义属性和行为的监听

调用方法：

```java
PushListener listener = new PushListener() {
        @Override
        public void execute(String action, String jsonParams) {
            Log.e("EgPushTAG","通知被点击");
        }
};
AnalysysAgent.trackCampaign(context, campaign, isClick, listener);
```

### 三方推送平台 SDK 集成及示例代码

首先，开发者需要在 App 中集成第三方推送系统的 SDK，并在 App 初始化过程中获取设备推送 ID，并保存在方舟分析的用户信息中。目前易观SDK支持极光、百度、小米、个推四家第三方推送统计的支持。以下简要说明集成第三方推送系统的集成方式。

#### 极光推送

请参考极光推送[《Android SDK 文档》](https://docs.jiguang.cn/jpush/client/Android/android_sdk/)，将极光推送的 SDK 集成到 App 中。开发者需要将 App 的设备推送 ID 保存在方舟分析的用户信息中。方舟分析的用户分群等分析功能会使用该设备推送 ID 进行用户关联。

极光推送使用唯一的 Registration Id 标示设备。开发者需要在 Manifest 中定义 Receiver 接受极光推送的广播，通过 cn.jpush.android.intent.REGISTRATION 消息获取 Registration Id。

```java
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

Receiver 在 onReceiver 回调中接收的 Intent 中JPushInterface.EXTRA\_REGISTRATION\_ID 的 Action 中获取为 Registration Id ，开发者在极光推送初始化完成后，在广播中获取JPushInterface.EXTRA\_REGISTRATION\_ID 的值为 Registration Id 并将他保存在方舟分析的用户信息中；JPushInterface.ACTION\_NOTIFICATION\_RECEIVED 的 Action 表示通知被接收；JPushInterface.ACTION\_NOTIFICATION\_OPENED 的Action 表示通知接收成功后，被用户点击。

```java
public void onReceive(Context context, Intent intent) {
    mContext = context;
    Bundle bundle = intent.getExtras();
    if (JPushInterface.ACTION_REGISTRATION_ID.equals(intent.getAction())) {
        String regId = bundle.getString(JPushInterface.EXTRA_REGISTRATION_ID);
        Log.d(TAG, "接收Registration Id: " + regId);
        //易观打开推送接口
        AnalysysAgent.setPushID(context, PushProvider.JPUSH, regId);
    } else if (JPushInterface.ACTION_NOTIFICATION_RECEIVED.equals(intent.getAction())) {
        int notifactionId = bundle.getInt(JPushInterface.EXTRA_NOTIFICATION_ID);
        Log.d(TAG, "接收到推送下来的通知的ID: " + notifactionId);
        //接收到Push的通知
        AnalysysAgent.trackCampaign(context,bundle.getString(JPushInterface.EXTRA_EXTRA),false);
    } else if (JPushInterface.ACTION_NOTIFICATION_OPENED.equals(intent.getAction())) {
        Log.d(TAG, "用户点击打开了通知");
        //易观添加活动推广接口，点击了Push推下来的通知
        AnalysysAgent.trackCampaign(context,bundle.getString(JPushInterface.EXTRA_EXTRA),true);
    }
}
```

当统计消息接收成功或统计点击通知后需要执行自定义行为是，可实现 PushListener 接口，将实现对象传入 trackCampaign，例如要在点击通知后，输出日志，如下调用：

```java
PushListener listener = new PushListener() {
        @Override
        public void execute(String action, String jsonParams) {
            Log.e("EgPushTAG","通知被点击");
        }
    };
AnalysysAgent.trackCampaign(context,bundle.getString(JPushInterface.EXTRA_EXTRA),true,listener);
```

#### 个推推送

请参考个推推送[《Android SDK 文档》](http://docs.getui.com/getui/start/andorid/)，将个推推送的 SDK 集成到 App 中。开发者需要将 App 的设备推送 ID 保存在方舟分析的用户信息中。方舟分析的用户分群等分析功能会使用该设备推送 ID 进行用户关联。

个推推送使用唯一的 clientid 标示设备。开发者需要在 Manifest 中定义 继承GTIntentService 的自定义 Service 接受个推推送的消息。

```java
<service android:name="Your service" />
```

在继承自 GTIntentService 的 Service 中复写回调方法,在 onReceiveClientId回调中调用 setPushID，将他保存在方舟分析的用户信息中;在 onReceiveMessageData 回调接口中，调用 trackCampaign 接口设置用户点击通知后事件追踪。

```java
@Override
public void onReceiveClientId(Context context, String clientid) {
    Log.e(TAG, "onReceiveClientId -> " + "clientid = " + clientid);
    AnalysysAgent.setPushID(context, PushProvider.GETUI,clientid);
}

@Override
public void onReceiveMessageData(Context context, GTTransmitMessage gtTransmitMessage) {
    Log.e(TAG,"onReceiveMessageData -> " + "msg:" + gtTransmitMessage.toString());
    byte[] payload = gtTransmitMessage.getPayload();
    String data = new String(payload);
    AnalysysAgent.trackCampaign(context,data,true);
}
```

当统计点击通知后需要执行自定义行为时，可实现 PushListener 接口，将实现对象传入 trackCampaign，例如要在点击通知后，输出日志，如下调用：

```java
PushListener listener = new PushListener() {
        @Override
        public void execute(String action, String jsonParams) {
            Log.e("EgPushTAG","通知被点击");
        }
    };
AnalysysAgent.trackCampaign(context,data,true,listener);
```

#### 百度云推送

请参考百度云推送[《Android SDK 文档》](http://push.baidu.com/doc/android/api)，将百度云推送的 SDK 集成到 App 中。开发者需要将 App 的设备推送 ID 保存在方舟分析的用户信息中。方舟分析的用户分群等分析功能会使用该设备推送 ID 进行用户关联。

百度云推送使用唯一的 Registration Id 标示设备。开发者需要在 Manifest 中定义 Receiver 接受百度云推送的广播，通过 com.baidu.android.pushservice.action.RECEIVE 消息获取 Registration Id。

```java
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

在百度的 Receiver 在自定义了获取百度推送 ID 的回调接口 onBind，获取Channel Id，在该回调中调用 setPushID，将他保存在方舟分析的用户信息中;在 onNotificationArrived 回调接口中，调用 trackCampaign 接口设置接收通知成功;在 onNotificationClicked 回调接口中，调用 trackCampaign 接口设置用户点击通知后事件追踪。

```java
@Override
public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
    Log.d(TAG, responseString);
    if (errorCode == 0) {
        // 绑定成功
        Log.d(TAG, "绑定成功");
        AnalysysAgent.setPushID(context, PushProvider.BAIDU,channelId);
    }
}
@Override
public void onNotificationArrived(Context context, String title,
                                    String description, String customContentString) {
    Log.d(TAG, notifyString);
    // 你可以參考 onNotificationClicked中的提示从自定义内容获取具体值
    AnalysysAgent.trackCampaign(context,customContentString,false);
}

@Override
public void onNotificationClicked(Context context, String title,
                                    String description, String customContentString) {
    Log.d(TAG, notifyString);
    AnalysysAgent.trackCampaign(context,customContentString,true);
}
```

当统计点击通知后需要执行自定义行为时，可实现 PushListener 接口，将实现对象传入trackCampaign，例如要在点击通知后，输出日志，如下调用：

```java
PushListener listener = new PushListener() {
        @Override
        public void execute(String action, String jsonParams) {
            Log.e("EgPushTAG","通知被点击");
        }
    };
AnalysysAgent.trackCampaign(context,customContentString,true,listener);
```

#### 小米推送

请参考小米推送[《Android SDK 文档》](https://dev.mi.com/console/doc/detail?pId=41)，将小米推送送的 SDK 集成到 App 中。开发者需要将 App 的设备推送 ID 保存在方舟分析的用户信息中。方舟分析的用户分群等分析功能会使用该设备推送 ID 进行用户关联。

小米推送使用唯一的 Registration Id 标示设备。开发者需要在 Manifest 中定义 Receiver 接受小米推送送的广播，通过广播消息获取 Registration Id。

```java
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

广播消息的 MiPushCommandMessage 参数中可以获取到 Registration Id ，开发者在小米推送初始化完成后，在广播中获取 Registration Id 并将他保存在方舟分析的用户信息中; 在回调方法 onNotificationMessageArrived 中记录记录 App 推送成功事件；在 onNotificationMessageClicked 回调中记录用户点击消息打开 App 或执行其他行为的事件

```java
@Override
public void onReceiveRegisterResult(Context context, MiPushCommandMessage message) {
    String command = message.getCommand();
    List<String> arguments = message.getCommandArguments();
    String cmdArg = ((arguments != null && arguments.size() > 0) ? arguments.get(0) : null);
    if (MiPushClient.COMMAND_REGISTER.equals(command)) {
        if (message.getResultCode() == ErrorCode.SUCCESS) {
            AnalysysAgent.setPushID(context, PushProvider.XIAOMI,cmdArg);
        }
    }
}
@Override
public void onNotificationMessageArrived(Context context, MiPushMessage message) {
    mMessage = message.getContent();
    AnalysysAgent.trackCampaign(context,mMessage,false);
}
@Override
public void onNotificationMessageClicked(Context context, MiPushMessage message) {
    mMessage = message.getContent();
    AnalysysAgent.trackCampaign(context,mMessage,true);
}
```

当统计点击通知后需要执行自定义行为时，可实现 PushListener接口，将实现对象传入 trackCampaign，例如要在点击通知后，输出日志，如下调用：

```java
PushListener listener = new PushListener() {
        @Override
        public void execute(String action, String jsonParams) {
            Log.e("EgPushTAG","通知被点击");
        }
    };
AnalysysAgent.trackCampaign(context,mMessage,true,listener);
```

