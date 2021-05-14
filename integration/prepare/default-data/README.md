# 预置事件和属性

为了帮助开发者更快速的集成SDK，了解采集哪些用户行为和属性，我们预置了一些事件、事件属性和用户属性。

{% hint style="info" %}
部分默认自动采集，Android、iOS、JS 和 小程序端略有差异；

非默认采集的字段，如有需要，可以直接使用预置字段作为事件 ID、属性 ID 进行上报，以便数据集成时，能够自动生成预置看板。

以下表格中涉及标识为 Y/N/- 的含义

* **Y 表示相应平台默认自动采集**
* **N 表示相应平台不默认采集**
* **S 表示由服务器自动根据相应规则自动处理**
* **-  表示相应平台不支持采集该事件或属性**
{% endhint %}

{% hint style="danger" %}
除了预置的事件和属性ID会以$开头，其他自定义的事件ID和属性ID，须注意命名方式：仅支持字母、数字和下划线，不能以数字或下划线开头，上限125个半角字符。
{% endhint %}

## 预置事件

### 1. **Event：**基础预置事件

{% hint style="info" %}
基础预置事件会默认加入到方舟埋点方案

启动、关闭、浏览页面事件会在集成了基础 SDK 后会自动采集；

**$app\_click**、**$web\_click** 用于记录点击网页/APP页面，用于分析点击位置热图、点击元素热图，集成 SDK 时设置参数_autoHeatmap_ 设置为 _true ；_

**$websta**y 用于记录用户停留在可视区域，分析浏览深度线，集成 SDK 时设置参数 _autoWebstay_ 为 _true；_

**$user\_click** 用于采集用户点击元素的事件，集成 SDK 时开启全埋点功能，即设置参数 _autoTrack 为 true_ 

对于用不到的事件可以选择不用，比如集成的平台中没有小程序时，可以不用 $share；没有 APP 时，可以不用 $app\_crash
{% endhint %}

| 事件ID | 事件显示名称 | 事件说明 | Android | iOS | JS | 小程序 |
| :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $startup | 启动 | APP启动 / 打开网站 | Y | Y | Y | Y |
| $end | 关闭 | APP关闭 | Y | Y | - | - |
| $pageview | 浏览页面 | 浏览APP/网站页面 | Y | Y | Y | Y |
| $user\_click | 点击元素 | 全埋点自动采集元素点击行为 | N | N | N | N |
| $app\_click | App点击 | App热图点击事件（用于热图分析） | N | N | - | - |
| $web\_click | Web点击 | Web热图点击事件（用于热图分析） | - | - | N | - |
| $webstay | 视区停留 | 停留在可视区域（用于分析网页浏览深度） | - | - | N | - |
| $share | 小程序分享 | 点击小程序分享按钮 | - | - | - | N |
| $app\_crash | APP崩溃 | APP崩溃信息 | N | N | - | - |

### 2. Event：APP 推广监测预置事件

{% hint style="info" %}
在首次创建 APP 推广监测成功后，系统会自动将 APP 推广监测相关事件添加在埋点方案中

无需在 SDK 中进行特殊设置，用户点击来推广链接激活后会自动上报数据
{% endhint %}

| 事件ID | 事件显示名称 | 事件说明 | Android | iOS | JS | 小程序 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| $campaign\_track | APP推广监测 | APP扫描监测扫描二维码时上报 | S | S | - | - |
| $first\_installation | 首次安装激活 | APP扫描监测扫描二维码后首次打开APP会时上报激活事件 | N | N | - | - |

### 3. Event：消息通知预置事件

{% hint style="info" %}
在服务集成配置中配置消息通知推送通道后，系统会自动将这 3 个事件添加到埋点方案中

集成 SDK 时需要注意消息通知模块的设置，详见 [SDK 集成文档](https://docs.analysys.cn/integration/sdk/android/xiao-xi-tui-song-mo-kuai)
{% endhint %}

| 事件ID | 事件显示名称 | 事件说明 | Android | iOS | JS | 小程序 |
| :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $push\_receiver\_success | 消息推送成功 | 设备收到推送消息时触发 | Y | Y | - | - |
| $push\_click | 点击推送消息 | 设备点击了推送消息时触发 | Y | Y | - | - |
| $push\_process\_success | 成功处理push消息 | 成功处理push消息 | Y | Y | - | - |

#### 绑定用户实名信息

| 事件ID | 事件显示名称 | 事件说明 | Android | iOS | JS | 小程序 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| $alias | 绑定用户实名信息 | 用户实名认证 | S | S | S | S |

###  4. Profile 系列事件

{% hint style="info" %}
$alias 事件之外的Profile 系列的事件用于上报用户属性，所以同样不会作为单独的事件去分析，即不会出现在分析模型中事件的选项中，也不会计入任意事件的计算。
{% endhint %}

| 事件ID | 事件说明 | Android | iOS | JS | 小程序 |
| :--- | :--- | :---: | :---: | :---: | :---: |
| $alias | 用户实名认证 | N | N | N | N |
| $profile\_set | 设置用户信息，覆盖写 | N | N | N | N |
| $profile\_set\_once | 设置用户信息，有则不进行任何操作 | Y | Y | Y | Y |
| $profile\_increment | 增加或减少用户信息中的数字类型的属性 | N | N | N | N |
| $profile\_delete | 删除用户信息 | N | N | N | N |
| $profile\_append | 数组属性添加值 | N | N | N | N |
| $profile\_unset | 设置用户信息中的某个属性为空 | N | N | N | N |

{% hint style="warning" %}
除了上述预置事件之外，更多业务相关的事件，需要自定义埋点上报。
{% endhint %}

{% page-ref page="../../../features/project-manegement/data-integration/schema.md" %}

## 预置 Event 事件通用属性

事件属性描述事件发生的方式和内容，是分析过程中的维度，也可以用于条件过滤。

* 事件通用属性：所有事件共同拥有的属性，e.g. 平台、应用版本、操作系统等；
* 事件自有属性：某个事件独有的事件属性，e.g. 浏览页面事件 $pageview 会有 $url、$title 等属性。

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x5C5E;&#x6027;ID</th>
      <th style="text-align:left">&#x5C5E;&#x6027;&#x663E;&#x793A;&#x540D;&#x79F0;</th>
      <th style="text-align:left">&#x6570;&#x636E;&#x7C7B;&#x578B;</th>
      <th style="text-align:left">&#x5C5E;&#x6027;&#x8BF4;&#x660E;</th>
      <th style="text-align:center">Android</th>
      <th style="text-align:center">iOS</th>
      <th style="text-align:center">JS</th>
      <th style="text-align:center">&#x5C0F;&#x7A0B;&#x5E8F;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">$platform</td>
      <td style="text-align:left">&#x5E73;&#x53F0;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x5E94;&#x7528;&#x5E73;&#x53F0;&#xFF0C;&#x679A;&#x4E3E;&#x53D6;&#x503C;&#xFF1A;JS/iOS/Android/Wechat</td>
      <td
      style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$app_version</td>
      <td style="text-align:left">&#x5E94;&#x7528;&#x7248;&#x672C;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x5E94;&#x7528;&#x7248;&#x672C;&#xFF0C;e.g. V1.0</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">-</td>
      <td style="text-align:center">-</td>
    </tr>
    <tr>
      <td style="text-align:left">$device_type</td>
      <td style="text-align:left">&#x8BBE;&#x5907;&#x7C7B;&#x578B;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x8BBE;&#x5907;&#x7C7B;&#x578B;&#xFF0C;e.g. PC&#x3001;&#x79FB;&#x52A8;&#x8BBE;&#x5907;</td>
      <td
      style="text-align:center">S</td>
        <td style="text-align:center">S</td>
        <td style="text-align:center">S</td>
        <td style="text-align:center">S</td>
    </tr>
    <tr>
      <td style="text-align:left">$manufacturer</td>
      <td style="text-align:left">&#x8BBE;&#x5907;&#x5236;&#x9020;&#x5546;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x5236;&#x9020;&#x5382;&#x5546;&#xFF0C; e.g. &#x5C0F;&#x7C73;</td>
      <td
      style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
        <td style="text-align:center">-</td>
        <td style="text-align:center">-</td>
    </tr>
    <tr>
      <td style="text-align:left">$brand</td>
      <td style="text-align:left">&#x8BBE;&#x5907;&#x54C1;&#x724C;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x8BBE;&#x5907;&#x54C1;&#x724C;&#xFF0C;e.g. &#x534E;&#x4E3A;&#x8363;&#x8000;</td>
      <td
      style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
        <td style="text-align:center">S</td>
        <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$model</td>
      <td style="text-align:left">&#x8BBE;&#x5907;&#x578B;&#x53F7;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x8BBE;&#x5907;&#x578B;&#x53F7;&#xFF0C;e.g. iPhone8&#x3001;&#x5C0F;&#x7C73;4</td>
      <td
      style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
        <td style="text-align:center">S</td>
        <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$os</td>
      <td style="text-align:left">&#x64CD;&#x4F5C;&#x7CFB;&#x7EDF;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x64CD;&#x4F5C;&#x7CFB;&#x7EDF;&#xFF0C;e.g.Window&#x3001;MacOS</td>
      <td
      style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
        <td style="text-align:center">S</td>
        <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$os_version</td>
      <td style="text-align:left">&#x64CD;&#x4F5C;&#x7CFB;&#x7EDF;&#x7248;&#x672C;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x64CD;&#x4F5C;&#x7CFB;&#x7EDF;&#x7248;&#x672C;&#xFF0C;e.g.Windows 10</td>
      <td
      style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
        <td style="text-align:center">S</td>
        <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$browser</td>
      <td style="text-align:left">&#x6D4F;&#x89C8;&#x5668;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x6D4F;&#x89C8;&#x5668;&#x540D;&#x79F0;&#xFF0C;e.g. Chrome</td>
      <td style="text-align:center">-</td>
      <td style="text-align:center">-</td>
      <td style="text-align:center">S</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$browser_version</td>
      <td style="text-align:left">&#x6D4F;&#x89C8;&#x5668;&#x7248;&#x672C;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x6D4F;&#x89C8;&#x5668;&#x7248;&#x672C;&#xFF0C;e.g. Chrome 62.23.23</td>
      <td
      style="text-align:center">-</td>
        <td style="text-align:center">-</td>
        <td style="text-align:center">S</td>
        <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$network</td>
      <td style="text-align:left">&#x7F51;&#x7EDC;&#x7C7B;&#x578B;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x7F51;&#x7EDC;&#x7C7B;&#x578B;&#xFF0C;e.g. WIFI&#x3001;2G&#x3001;3G&#x3001;4G</td>
      <td
      style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
        <td style="text-align:center">-</td>
        <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$carrier_name</td>
      <td style="text-align:left">&#x8FD0;&#x8425;&#x5546;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x63A5;&#x5165;&#x8FD0;&#x8425;&#x5546;&#x540D;&#x79F0;&#xFF0C;e.g. &#x4E2D;&#x56FD;&#x8054;&#x901A;</td>
      <td
      style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
        <td style="text-align:center">-</td>
        <td style="text-align:center">-</td>
    </tr>
    <tr>
      <td style="text-align:left">$screen_width</td>
      <td style="text-align:left">&#x5C4F;&#x5E55;&#x5BBD;&#x5EA6;</td>
      <td style="text-align:left">&#x6570;&#x503C;</td>
      <td style="text-align:left">&#x5C4F;&#x5E55;&#x5BBD;&#x5EA6;/&#x5C4F;&#x5E55;&#x5206;&#x8FA8;&#x7387;&#xFF0C;e.g.
        1920</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$screen_height</td>
      <td style="text-align:left">&#x5C4F;&#x5E55;&#x9AD8;&#x5EA6;</td>
      <td style="text-align:left">&#x6570;&#x503C;</td>
      <td style="text-align:left">&#x5C4F;&#x5E55;&#x9AD8;&#x5EA6;/&#x5C4F;&#x5E55;&#x5206;&#x8FA8;&#x7387;&#xFF0C;e.g.
        768</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$is_login</td>
      <td style="text-align:left">&#x662F;&#x5426;&#x662F;&#x6CE8;&#x518C;&#x7528;&#x6237;</td>
      <td style="text-align:left">&#x5E03;&#x5C14;</td>
      <td style="text-align:left">&#x662F;&#x5426;&#x662F;&#x6CE8;&#x518C;&#x7528;&#x6237;</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$ip</td>
      <td style="text-align:left">IP</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">IP&#x5730;&#x5740;</td>
      <td style="text-align:center">S</td>
      <td style="text-align:center">S</td>
      <td style="text-align:center">S</td>
      <td style="text-align:center">S</td>
    </tr>
    <tr>
      <td style="text-align:left">$country</td>
      <td style="text-align:left">&#x56FD;&#x5BB6;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">
        <p>&#x4E8B;&#x4EF6;&#x53D1;&#x751F;&#x65F6;&#x6240;&#x5728;&#x56FD;&#x5BB6;&#xFF0C;</p>
        <p>e.g. &#x4E2D;&#x56FD;&#x3001;&#x7F8E;&#x56FD;</p>
      </td>
      <td style="text-align:center">S</td>
      <td style="text-align:center">S</td>
      <td style="text-align:center">S</td>
      <td style="text-align:center">S</td>
    </tr>
    <tr>
      <td style="text-align:left">$province</td>
      <td style="text-align:left">&#x7701;&#x4EFD;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">
        <p>&#x4E8B;&#x4EF6;&#x53D1;&#x751F;&#x65F6;&#x6240;&#x5728;&#x7701;&#x4EFD;&#xFF0C;</p>
        <p>e.g. &#x5317;&#x4EAC;&#x3001;&#x4E0A;&#x6D77;&#x3001;&#x798F;&#x5EFA;</p>
      </td>
      <td style="text-align:center">S</td>
      <td style="text-align:center">S</td>
      <td style="text-align:center">S</td>
      <td style="text-align:center">S</td>
    </tr>
    <tr>
      <td style="text-align:left">$city</td>
      <td style="text-align:left">&#x57CE;&#x5E02;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">
        <p>&#x4E8B;&#x4EF6;&#x53D1;&#x751F;&#x65F6;&#x6240;&#x5728;&#x57CE;&#x5E02;&#xFF0C;</p>
        <p>e.g. &#x5317;&#x4EAC;&#x3001;&#x53A6;&#x95E8;</p>
      </td>
      <td style="text-align:center">S</td>
      <td style="text-align:center">S</td>
      <td style="text-align:center">S</td>
      <td style="text-align:center">S</td>
    </tr>
    <tr>
      <td style="text-align:left">$utm_campaign_id</td>
      <td style="text-align:left">&#x6D3B;&#x52A8;ID</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">
        <p>&#x6839;&#x636E;&#x6DFB;&#x52A0;&#x7684;&#x5185;&#x5BB9;&#x81EA;&#x52A8;&#x751F;&#x6210;&#xFF0C;</p>
        <p>&#x6807;&#x8BC6;&#x4E00;&#x6B21;&#x6D3B;&#x52A8;</p>
      </td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$utm_campaign</td>
      <td style="text-align:left">&#x6D3B;&#x52A8;/&#x5E7F;&#x544A;&#x540D;&#x79F0;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x7279;&#x5B9A;&#x7684;&#x63A8;&#x5E7F;&#x6D3B;&#x52A8;&#xFF0C;e.g. &#x53CC;11&#x63A8;&#x5E7F;</td>
      <td
      style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$utm_medium</td>
      <td style="text-align:left">&#x6D3B;&#x52A8;/&#x5E7F;&#x544A;&#x5A92;&#x4ECB;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x63A8;&#x5E7F;&#x7C7B;&#x578B;&#xFF0C;e.g. SEM&#xFF0C;cpc</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$utm_source</td>
      <td style="text-align:left">&#x6D3B;&#x52A8;/&#x5E7F;&#x544A;&#x6765;&#x6E90;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x63A8;&#x5E7F;&#x6765;&#x6E90;&#xFF0C;e.g. &#x4ECA;&#x65E5;&#x5934;&#x6761;</td>
      <td
      style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$utm_content</td>
      <td style="text-align:left">&#x6D3B;&#x52A8;/&#x5E7F;&#x544A;&#x5185;&#x5BB9;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x5E7F;&#x544A;&#x5185;&#x5BB9;&#xFF0C;e.g. &#x4F18;&#x60E0;&#x4FE1;&#x606F;</td>
      <td
      style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$utm_term</td>
      <td style="text-align:left">&#x6D3B;&#x52A8;/&#x5E7F;&#x544A;&#x5173;&#x952E;&#x5B57;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x5E7F;&#x544A;&#x5173;&#x952E;&#x5B57;&#xFF0C;e.g. &#x7528;&#x6237;&#x753B;&#x50CF;</td>
      <td
      style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$is_first_day</td>
      <td style="text-align:left">&#x662F;&#x5426;&#x5B89;&#x88C5;&#x540E;&#x9996;&#x65E5;&#x8BBF;&#x95EE;</td>
      <td
      style="text-align:left">&#x5E03;&#x5C14;</td>
        <td style="text-align:left">&#x662F;&#x5426;&#x5B89;&#x88C5;&#x540E;&#x9996;&#x65E5;&#x8BBF;&#x95EE;</td>
        <td
        style="text-align:center">Y</td>
          <td style="text-align:center">Y</td>
          <td style="text-align:center">Y</td>
          <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$channel</td>
      <td style="text-align:left">&#x4E0B;&#x8F7D;&#x6E20;&#x9053;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x4E0B;&#x8F7D;&#x6E20;&#x9053;&#xFF0C;SDK &#x521D;&#x59CB;&#x5316;&#x65F6;&#x4F20;&#x5165;&#x3002;&#x4EC5;
        Android &#x901A;&#x8FC7;&#x6E20;&#x9053;&#x5305;&#x5206;&#x53D1;&#x65F6;&#x624D;&#x6709;&#x610F;&#x4E49;&#xFF0C;iOS
        &#x4F1A;&#x7EDF;&#x4E00;&#x4E3A;&#x201C;App Store&#x201D;</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">-</td>
      <td style="text-align:center">-</td>
    </tr>
    <tr>
      <td style="text-align:left">$lib</td>
      <td style="text-align:left">SDK&#x7C7B;&#x578B;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">SDK&#x7C7B;&#x578B;&#xFF0C;e.g. python&#x3001;iOS&#x7B49;</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$lib_version</td>
      <td style="text-align:left">SDK&#x7248;&#x672C;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">SDK&#x7248;&#x672C;&#xFF0C; e.g. &#xFF1A;11.2.5</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$session_id</td>
      <td style="text-align:left">SessionID</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x4F1A;&#x8BDD;ID&#xFF0C;e.g. 515950b8f1a6221c</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$language</td>
      <td style="text-align:left">&#x8BED;&#x8A00;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x7CFB;&#x7EDF;&#x8BED;&#x8A00;&#xFF0C;e.g. zh-cn</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$time_zone</td>
      <td style="text-align:left">&#x7528;&#x6237;&#x65F6;&#x533A;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x7528;&#x6237;&#x65F6;&#x533A;&#xFF0C;e.g.GMT+08:00</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">$user_agent</td>
      <td style="text-align:left">UA</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">UA</td>
      <td style="text-align:center">-</td>
      <td style="text-align:center">-</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">-</td>
    </tr>
    <tr>
      <td style="text-align:left">$web_crawler</td>
      <td style="text-align:left">&#x662F;&#x5426;&#x662F;&#x722C;&#x866B;</td>
      <td style="text-align:left">&#x5E03;&#x5C14;</td>
      <td style="text-align:left">&#x722C;&#x866B;&#x8BC6;&#x522B;</td>
      <td style="text-align:center">-</td>
      <td style="text-align:center">-</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">-</td>
    </tr>
    <tr>
      <td style="text-align:left">$is_time_calibrated</td>
      <td style="text-align:left">&#x662F;&#x5426;&#x4E0E;&#x670D;&#x52A1;&#x7AEF;&#x65F6;&#x95F4;&#x6821;&#x51C6;</td>
      <td
      style="text-align:left">&#x5E03;&#x5C14;</td>
        <td style="text-align:left">&#x662F;&#x5426;&#x4E0E;&#x670D;&#x52A1;&#x7AEF;&#x65F6;&#x95F4;&#x6821;&#x51C6;</td>
        <td
        style="text-align:center">N</td>
          <td style="text-align:center">N</td>
          <td style="text-align:center">-</td>
          <td style="text-align:center">N</td>
    </tr>
    <tr>
      <td style="text-align:left">$device_id</td>
      <td style="text-align:left">&#x8BBE;&#x5907;ID</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x8BBE;&#x5907;ID&#xFF0C;idfa/oaid &gt; idfv/androidid &gt; uuid</td>
      <td
      style="text-align:center">N</td>
        <td style="text-align:center">N</td>
        <td style="text-align:center">-</td>
        <td style="text-align:center">-</td>
    </tr>
    <tr>
      <td style="text-align:left">xwho</td>
      <td style="text-align:left">&#x7528;&#x6237;ID</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x7528;&#x6237;ID</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">xwhen</td>
      <td style="text-align:left">&#x89E6;&#x53D1;&#x65F6;&#x95F4;</td>
      <td style="text-align:left">&#x65E5;&#x671F;</td>
      <td style="text-align:left">&#x4E8B;&#x4EF6;&#x53D1;&#x751F;&#x7684;&#x65F6;&#x95F4;</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
      <td style="text-align:center">Y</td>
    </tr>
    <tr>
      <td style="text-align:left">distinct_id</td>
      <td style="text-align:left">&#x552F;&#x4E00;ID</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x7CFB;&#x7EDF;&#x5728; xwho &#x7684;&#x57FA;&#x7840;&#x4E0A;&#x6839;&#x636E;&#x4E00;&#x4E9B;&#x89C4;&#x5219;&#x751F;&#x6210;&#x7684;&#x552F;&#x4E00;
        IDS</td>
      <td style="text-align:center">S</td>
      <td style="text-align:center">S</td>
      <td style="text-align:center">S</td>
      <td style="text-align:center">S</td>
    </tr>
    <tr>
      <td style="text-align:left">$importflag</td>
      <td style="text-align:left">&#x662F;&#x5426;&#x5DE5;&#x5177;&#x5BFC;&#x5165;</td>
      <td style="text-align:left">&#x6570;&#x503C;</td>
      <td style="text-align:left">&#x6807;&#x8BC6;&#x662F;&#x5426;&#x662F;&#x5DE5;&#x5177;&#x5BFC;&#x5165;&#xFF0C;1&#x4E3A;&#x5DE5;&#x5177;&#x5BFC;&#x5165;</td>
      <td
      style="text-align:center">N</td>
        <td style="text-align:center">N</td>
        <td style="text-align:center">N</td>
        <td style="text-align:center">N</td>
    </tr>
    <tr>
      <td style="text-align:left">$debug</td>
      <td style="text-align:left">Debug&#x6A21;&#x5F0F;</td>
      <td style="text-align:left">&#x6570;&#x503C;</td>
      <td style="text-align:left">&#x6807;&#x8BC6;&#x6570;&#x636E;&#x5904;&#x7406;&#x65B9;&#x5F0F;&#xFF0C;0&#xFF1A;&#x975E;debug
        1&#xFF1A;debug&#xFF0C;&#x4E0D;&#x5165;&#x5E93; 2&#xFF1A;debug&#xFF0C;&#x5165;&#x5E93;</td>
      <td
      style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
        <td style="text-align:center">Y</td>
    </tr>
  </tbody>
</table>

{% hint style="info" %}
**非自动采集的属性，会根据相应字段自动解析**

**$ip ：**方舟的收数服务会自动记录上报的数据来源 IP，根据 IP 解析为国家、省份、城市三个字段

**$county：**通过 IP 解析

**$province：**通过 IP 解析

**$city ：**通过 IP 解析

**$device\_type：**通过 UA 解析
{% endhint %}

{% hint style="info" %}
**部分自动采集的属性不会作为独立的属性用于分析**

**$debug：**用于标识是否入库

* 0：表示关闭 Debug 模式
* 1：表示打开 Debug 模式，但该模式下发送的数据仅用于调试，不计入平台数据统计 
* 2：表示打开 Debug 模式，该模式下发送的数据可计入平台数据统计

**$session\_id：**标识一次会话

**$user\_agent：**UA，用于解析设备类型、浏览器、浏览器版本、操作系统、操作系统版本  
  
**$device\_id：系统唯一标识，默认不采集\(4.4.5版本新增\)**

**Andorid 采集规则：**advertising id &gt; android id &gt; uuid，按照先后顺序获取

**iOS 采集规则：**idfa&gt;idfv&gt;uuid，按照先后顺序获取
{% endhint %}

## 预置 Event 事件自身属性

部分预置事件在通用属性之外，还有自身独有的属性

### 基础预置事件

### **$startup**

| 属性ID | 属性显示名称 | 数据类型 | 属性说明 | Android | iOS | JS | 小程序 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $is\_first\_time | 是否安装后首次访问 | 布尔 | 是否安装后首次访问 | Y | Y | Y | Y |
| $is\_from\_background | 是否从后台唤醒 | 布尔 | 是否从后台唤醒恢复 | Y | Y | - | - |
| $start\_source | 启动来源 | 字符串 | 标识APP的启动来源，e.g. 通过点击图标启动、点击通知、URL唤醒、3D touch等 | N | N | - | - |
| $scene | 场景值 | 字符串 | 标识小程序的场景值，e.g 顶部搜索框的搜索结果页 | - | - | - | Y |
| $scene\_type | 场景值类型 | 字符串 | 标识小程序场景值类型 | - | - | - | S |

### **$end**

| 属性ID | 属性显示名称 | 数据类型 | 属性说明 | Android | iOS | JS | 小程序 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $duration | 使用时长 | 数值 | 从启动到关闭的使用时长 单位：毫秒 | Y | Y | - | - |

### **$pageview**

| 属性ID | 属性显示名称 | 数据类型 | 属性说明 | Android | iOS | JS | 小程序 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $url | 页面URL\(含参\) | 字符串 | 页面完整路径 | Y | Y | Y | Y |
| $url\_domain | 页面URL  | 字符串 | 去参的页面URL | - | - | N | - |
| $title | 页面标题 | 字符串 | 页面标题 | Y | Y | Y | - |
| $referrer | 页面来源 | 字符串 | 页面来源 | Y | Y | Y | Y |
| $referrer\_domain | 页面来源域名 | 字符串 | 页面来源域名 | - | - | Y | - |
| $traffic\_source\_type | 流量来源类型 | 字符串 | 流量来源类型，数据处理 | - | - | S | - |
| $search\_engine | 搜索引擎 | 字符串 | 标识搜索引擎来源，e.g. 百度 | - | - | S | - |
| $search\_keyword | 搜索关键词 | 字符串 | 标识搜索词来源，e.g. 易观方舟 | - | - | S | - |
| $social\_media | 社交媒体 | 字符串 | 标识社交媒体来源，e.g. 微博 | - | - | S | - |
| $social\_share\_from | 社交媒体分享来源 | 字符串 | 标识微信来源，e.g. 微信朋友圈、微信群 | - | - | S | - |
| $scene | 场景值 | 字符串 | 标识小程序的场景值，e.g 顶部搜索框的搜索结果页 | - | - | - | Y |
| $scene\_type | 场景值类型 | 字符串 | 标识小程序场景值类型 | - | - | - | S |
| $startup\_time | 启动时间 | 日期 | yyyy-MM-dd hh:mm:ss.SSS | - | - | Y | Y |
| $share\_id | 分享者ID | 字符串 | 当点击分享的小程序后上报来源 | - | - | - | N |
| $share\_level | 转发层级 | 数值 | 当点击分享的小程序后上报来源 | - | - | - | N |
| $share\_path | 转发地址 | 字符串 | 当点击分享的小程序后上报来源 | -- | - | - | N |

{% hint style="info" %}
* **$referrer 字段在 App 中手动调用 pageview 接口，默认不采集**
* **$url\_domain, $traffic\_source\_type, $search\_engine 等非自动采集的属性，系统会根据$url 和 $referrer 自动解析**
{% endhint %}

### **$user\_click**

| 属性ID | 属性显示名称 | 数据类型 | 属性说明 | Android | iOS | JS | 小程序 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $title | 页面标题 | 字符串 | 页面标题 | Y | Y | Y | - |
| $parent\_url  | 父页面URL   | 字符串 | 页面URL，为空则为顶级页 | Y | - | - | - |
| $url    | 页面URL | 字符串 | 页面URL | Y | Y | Y | N |
| $url\_path | 页面地址（不含参） | 字符串 | 页面地址（不含参） | - | - | Y | - |
| $element\_path    | 元素路径 | 字符串 | APP 为元素唯一标识；JS 为元素路径 | Y | Y | Y | N |
| $element\_class\_name    | 元素样式的类 | 字符串 | 仅 JS 有效 | - | - | Y | N |
| $element\_target\_url    | 元素链接地址 | 字符串 | 仅 JS 有效 | - | - | Y | N |
| $element\_id  | 元素ID |  字符串 | 元素ID | Y | Y | Y | N |
| $element\_name  | 元素名称  | 字符串 | 仅 JS 有效 | - | - | Y | N |
| $element\_type | 元素类型 | 字符串 | 元素类型 | Y | Y | Y | N |
| $element\_position  |  列表控件位置 （可选） | 字符串 | 列表控件位置 （可选） | Y | Y | - | - |
| $element\_content  | 元素内容 | 字符串 | 元素的内容（优先级：内容&gt;描述&gt;空） | Y | Y | Y | N |

### **$webstay**

| 属性ID | 属性显示名称 | 数据类型 | 属性说明 | Android | iOS | JS | 小程序 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $url | 页面URL | 字符串 | 页面URL/页面完整路径 | - | - | N | - |
| $title | 页面标题 | 字符串 | 页面标题 | - | - | N | - |
| $referrer | 页面来源 | 字符串 | 页面来源 | - | - | N | - |
| $referrer\_domain | 页面来源域名 | 字符串 | 页面来源域名 | - | - | N | - |
| $viewport\_width | 视区宽度 | 数值 | 视区宽度 | - | - | N | - |
| $viewport\_position | 视区距顶部的位置 | 数值 | 视区距顶部的位置 | - | - | N | - |
| $viewport\_height | 视区高度 | 数值 | 视区高度 | - | - | N | - |
| $event\_duration | 视区停留时间 | 数值 | 视区停留时间 | - | - | N | - |

### **$app\_click**

| 属性ID | 属性显示名称 | 数据类型 | 属性说明 | Android | iOS | JS | 小程序 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $page\_width | 页面宽度 | 数值 | 热图页面宽度 | Y | Y | Y | - |
| $page\_height | 页面高度 | 数值 | 热图页面高度 | Y | Y | Y | - |
| $screen\_dpi | 屏幕DPI | 数值 | 屏幕DPI | Y | - | - | - |
| $screen\_scale | 屏幕缩放比 | 数值 | 屏幕缩放比 | Y | - | - | - |
| $click\_x | 点击X坐标 | 数值 | 热图点击X坐标 | Y | Y | Y | - |
| $click\_y | 点击Y坐标 | 数值 | 热图点击Y坐标 | Y | Y | Y | - |
| $url | 页面URL | 字符串 | 点击热图时页面URL | Y | Y | Y | - |
| $element\_x | 元素X坐标 | 数值 | 点击元素X坐标 | Y | Y | Y | - |
| $element\_y | 元素Y坐标 | 数值 | 点击元素Y坐标 | Y | Y | Y | - |
| $element\_path | 元素路径 | 字符串 | 热图元素路径 | Y | Y | Y | - |
| $element\_name | 元素名称 | 字符串 | 热图元素名称 | - | Y | - | - |
| $element\_type | 元素类型 | 字符串 | 热图元素类型 | Y | Y | Y | - |
| $element\_content | 元素内容 | 字符串 | 热图元素的内容 | Y | Y | Y | - |
| $element\_clickable | 是否可以点击元素 | 数值 | 是否可以点击元素 | Y | Y | Y | - |

### **$web\_click**

| 属性ID | 属性显示名称 | 数据类型 | 属性说明 | Android | iOS | JS | 小程序 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $page\_width | 页面宽度 | 数值 | 热图页面宽度 | Y | Y | Y | - |
| $page\_height | 页面高度 | 数值 | 热图页面高度 | Y | Y | Y | - |
| $screen\_dpi | 屏幕DPI | 数值 | 屏幕DPI | Y | - | - | - |
| $screen\_scale | 屏幕缩放比 | 数值 | 屏幕缩放比 | Y | - | - | - |
| $click\_x | 点击X坐标 | 数值 | 热图点击X坐标 | Y | Y | Y | - |
| $click\_y | 点击Y坐标 | 数值 | 热图点击Y坐标 | Y | Y | Y | - |
| $url | 页面URL | 字符串 | 点击热图时页面URL | Y | Y | Y | - |
| $element\_x | 元素X坐标 | 数值 | 点击元素X坐标 | Y | Y | Y | - |
| $element\_y | 元素Y坐标 | 数值 | 点击元素Y坐标 | Y | Y | Y | - |
| $element\_path | 元素路径 | 字符串 | 热图元素路径 | Y | Y | Y | - |
| $element\_name | 元素名称 | 字符串 | 热图元素名称 | - | Y | - | - |
| $element\_type | 元素类型 | 字符串 | 热图元素类型 | Y | Y | Y | - |
| $element\_content | 元素内容 | 字符串 | 热图元素的内容 | Y | Y | Y | - |
| $element\_clickable | 是否可以点击元素 | 数值 | 是否可以点击元素 | Y | Y | Y | - |
| $element\_target\_url    | 元素链接地址 | 字符串 | 仅 JS 有效 | - | - | Y | - |

### **$share**

| 属性ID | 属性显示名称 | 数据类型 | 属性说明 | Android | iOS | JS | 小程序 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $share\_id | 分享者ID | 字符串 | 当点击分享的小程序后上报来源 | - | - | - | N |
| $share\_level | 转发层级 | 数值 | 当点击分享的小程序后上报来源 | - | - | - | N |
| $share\_path | 转发地址 | 字符串 | 当点击分享的小程序后上报来源 | - | - | - | N |

### $app\_crash

| 属性ID | 属性显示名称 | 数据类型 | 属性说明 | Android | iOS | JS | 小程序 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $crash\_data | 崩溃原因 | 字符串 | 崩溃的堆栈信息 | Y | Y | - | - |

### **$alias**

| 属性ID | 属性显示名称 | 数据类型 | 属性说明 | Android | iOS | JS | 小程序 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $original\_id | 匿名ID | 字符串 | 实名绑定前的匿名ID | Y | Y | Y | Y |

### APP 推广监测预置事件

### $campaign\_track

| 属性ID | 属性名称 | 属性值数据类型 | 属性说明 | Android 自动采集 | iOS 自动采集 | JS 自动采集 | 小程序 自动采集 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| $campaign\_channelid | 推广渠道ID | 数值 | APP推广监测扫描二维码时上报 | Y | Y | - | - |
| $campaign\_shortlink | 推广跳转短链CODE | 字符串 | APP推广监测扫描二维码时上报 | Y | Y | - | - |
| $prop\_0 | 推广渠道信息1 | 字符串 | APP推广监测扫描二维码时上报 | Y | Y | - | - |
| $prop\_1 | 推广渠道信息2 | 字符串 | APP推广监测扫描二维码时上报 | Y | Y | - | - |
| $prop\_2 | 推广渠道信息3 | 字符串 | APP推广监测扫描二维码时上报 | Y | Y | - | - |
| $prop\_3 | 推广渠道信息4 | 字符串 | APP推广监测扫描二维码时上报 | Y | Y | - | - |
| $prop\_4 | 推广渠道信息5 | 字符串 | APP推广监测扫描二维码时上报 | Y | Y | - | - |

### $first\_installation

| 属性ID | 属性名称 | 属性值数据类型 | 属性说明 | Android 自动采集 | iOS 自动采集 | JS 自动采集 | 小程序 自动采集 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| $fingerprint | 设备指纹 | 数值 | 设备指纹 | S | S | - | - |
| $track\_xwhen | 激活时间 | 数值 | 指纹匹配上的时间 | S | S | - | - |

#### 

### 消息通知预置事件

### **$push\_receiver\_success**

| 属性ID | 属性显示名称 | 数据类型 | 属性说明 | Android | iOS | JS | 小程序 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $action\_type | 操作类型 | 数值 | 点击消息通知后的操作类型 | N | N | - | - |
| $action | 操作 | 字符串 | 点击消息通知后的操作 | N | N | - | - |

### **$push\_process\_success**

| 属性ID | 属性显示名称 | 数据类型 | 属性说明 | Android | iOS | JS | 小程序 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $action\_type | 操作类型 | 数值 | 点击消息通知后的操作类型 | N | N | - | - |
| $action | 操作 | 字符串 | 点击消息通知后的操作 | N | N | - | - |

### **$push\_click**

| 属性ID | 属性显示名称 | 数据类型 | 属性说明 | Android | iOS | JS | 小程序 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $action\_type | 操作类型 | 数值 | 点击消息通知后的操作类型 | N | N | - | - |
| $action | 操作 | 字符串 | 点击消息通知后的操作 | N | N | - | - |

## 预置用户属性

### **基础预置用户属性**

{% hint style="info" %}
默认加入到方舟埋点方案-用户方案中
{% endhint %}

| 属性ID | 属性名称 | 属性值数据类型 | 属性说明 | Android 自动采集 | iOS 自动采集 | JS 自动采集 | 小程序 自动采集 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| distinct\_id | 唯一ID | 数值 | 方舟系统生成的用户唯一ID | S | S | S | S |
| xwho | 用户ID | 字符串 | 行为主体 | Y | Y | Y | Y |
| xwhen | 用户属性更新时间 | 日期时间 | 用户属性更新时间 | Y | Y | Y | Y |
| $original\_id | 匿名ID | 字符串 | 实名绑定前的匿名ID | S | S | S | S |
| $original\_id\_list | 匿名ID列表 | 字符串 | 实名绑定过的匿名ID列表 | S | S | S | S |
| $idfv | IDFV | 字符串 | IDFV | - | N | - | - |
| $idfa | IDFA | 字符串 | IDFA | - | N | - | - |
| $mac | MAC | 字符串 | MAC | Y | - | - | - |
| $imei | IMEI | 字符串 | IMEI | N | - | - | - |
| $wechatopenid | 微信OpenID | 字符串 | 微信OpenID | - | - | - | N |
| $phone | 手机号 | 字符串 | 手机号 | N | N | N | N |
| $email | 邮箱 | 字符串 | 邮箱 | N | N | N | N |
| $platform | 最后一次使用平台 | 字符串 | 最后一次使用平台 | Y | Y | Y | Y |
| $lib | 最新版本的SDK类型 | 字符串 | SDK类型，e.g.  python、iOS等 | Y | Y | Y | Y |
| $lib\_version | 最新版本的SDK版本 | 字符串 | SDK版本，e.g. 11.2.5 | Y | Y | Y | Y |
| $country | 所在国家 | 字符串 | 用户所在国家 | N | N | N | N |
| $province | 所在省份 | 字符串 | 用户所在省份 | N | N | N | N |
| $city | 所在城市 | 字符串 | 用户所在城市 | N | N | N | N |
| $is\_login | 是否是注册帐号 | 布尔值 | 是否是注册帐号 | Y | Y | Y | Y |
| $signup\_time | 注册时间 | 日期时间 | 注册时间 | N | N | N | N |
| $first\_visit\_time | 首次访问时间 | 日期时间 | 首次访问时间 | Y | Y | Y | Y |
| $first\_visit\_language | 语言 | 字符串 | 设备语言 | Y | Y | Y | Y |
| $time\_zone | 首次访问时区 | 字符串 | GMT+08:00 | Y | Y | Y | Y |

### **推送相关用户属性**

{% hint style="info" %}
集成服务配置/EA配置后注册
{% endhint %}

| 属性ID | 属性名称 | 属性值数据类型 | 属性说明 | Android 自动采集 | iOS 自动采集 | JS 自动采集 | 小程序 自动采集 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| $getui | 个推ID | 字符串 | 个推推送ID | N | N | - | - |
| $jpush | 极光ID | 字符串 | 极光推送ID | N | N | - | - |
| $baidu | 百度ID | 字符串 | 百度推送ID | N | N | - | - |
| $xiaomi | 小米ID | 字符串 | 小米推送ID | N | N | - | - |
| $huawei | 华为ID | 字符串 | 华为推送ID | N | - | - | - |
| $aliyun | 阿里云ID | 字符串 | 阿里云推送ID | N | N | N | N |
| $xinge | 信鸽ID | 字符串 | 信鸽推送ID | N | N | N | N |
| $apns | ANPS | 字符串 | ANPS | - | N | - | - |
| $meizu | 魅族ID | 字符串 | 魅族推送ID | N | - | - | - |
| $oppo | OPPOID | 字符串 | OPPO推送ID | N | - | - | - |
| $vivo | VIVOID | 字符串 | VIVO推送ID | N | - | - | - |

### 渠道监测相关用户属性

{% hint style="info" %}
首次创建APP推广监测成功时触发注册
{% endhint %}

| 属性ID | 属性名称 | 属性值数据类型 | 属性说明 | Android 自动采集 | iOS 自动采集 | JS 自动采集 | 小程序 自动采集 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| $prop\_0 | 推广渠道信息1 | 字符串 | APP推广监测扫描二维码时上报 | N | N | - | - |
| $prop\_1 | 推广渠道信息2 | 字符串 | APP推广监测扫描二维码时上报 | N | N | - | - |
| $prop\_2 | 推广渠道信息3 | 字符串 | APP推广监测扫描二维码时上报 | N | N | - | - |
| $prop\_3 | 推广渠道信息4 | 字符串 | APP推广监测扫描二维码时上报 | N | N | - | - |
| $prop\_4 | 推广渠道信息5 | 字符串 | APP推广监测扫描二维码时上报 | N | N | - | - |
| $campaign\_shortlink | 推广跳转短链CODE | 字符串 | APP推广监测扫描二维码时上报 | N | N | - | - |
| $campaign\_channelid | 推广渠道ID | 数值 | APP推广监测扫描二维码时上报 | N | N | - | - |

{% hint style="info" %}
以上内容没有解答我的问题？[点击我进入方舟论坛去反馈](https://www.analysysdata.com/forum/index) 🚀
{% endhint %}

