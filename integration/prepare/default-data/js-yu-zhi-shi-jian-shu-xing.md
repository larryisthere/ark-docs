# JS 预制事件/属性

### **基础预置事件**

| 事件ID | 属性ID | 数据类型 | 事件显示名称 | 事件说明 |
| :--- | :--- | :--- | :--- | :--- |
| $startup |  |  | 启动 | 打开网站 |
|  | $is\_first\_time | 布尔 | 是否安装后首次访问 | 是否安装后首次访问 |
|  | $预制属性 |  |  | 其他预制通用属性 |
| $pageview |  |  | 浏览页面 | 浏览APP/网站页面 |
|  | $url | 字符串 | 页面URL\(含参\) | 页面完整路径 |
|  | $url\_domain | 字符串 | 页面URL（去参） | 去参的页面URL |
|  | $title | 字符串 | 页面标题 | 页面标题 |
|  | $referrer | 字符串 | 页面来源 | 页面来源（$referrer 字段在 App 中手动调用 pageview 接口，默认不采集） |
|  | $referrer\_domain | 字符串 | 页面来源域名 | 页面来源域名 |
|  | $search\_engine | 字符串 | 搜索引擎 | 标识搜索引擎来源，e.g. 百度（通过UTM解析） |
|  | $search\_keyword | 字符串 | 搜索关键词 | 标识搜索词来源，e.g. 易观方舟（通过UTM解析） |
|  | $social\_media | 字符串 | 社交媒体 | 标识社交媒体来源，e.g. 微博（通过UTM解析） |
|  | $social\_share\_from | 字符串 | 社交媒体分享来源 | 标识微信来源，e.g. 微信朋友圈、微信群（通过UTM解析） |
|  | $预制属性 |  |  | 其他预制通用属性 |
| $user\_click |  |  | 点击元素 | 全埋点自动采集元素点击行为 |
|  | $title | 字符串 | 页面标题 | 页面标题 |
|  | $url | 字符串 | 页面URL | 页面URL |
|  | $url\_path | 字符串 | 页面地址（不含参） | 页面地址（不含参） |
|  | $element\_path | 字符串 | 元素路径 | APP 为元素唯一标识；JS 为元素路径 |
|  | $element\_class\_name | 字符串 | 元素样式的类 | 元素样式的类 |
|  | $element\_target\_url    | 字符串 | 元素链接地址 | 元素链接地址 |
|  | $element\_id  | 字符串 | 元素ID | 元素ID |
|  | $element\_name  | 字符串 | 元素名称 | 元素名称  |
|  | $element\_type | 字符串 | 元素类型 | 元素类型 |
|  | $element\_position | 字符串 | 列表控件位置  | 列表控件位置 （可选） |
|  | $element\_content  | 字符串 | 元素内容 | 元素的内容优先级：内容&gt;描述&gt;空 |
|  | $预制属性 |  |  | 其他预制通用属性 |
| $webstay |  |  | 视区停留 | 停留在可视区域（用于分析网页浏览深度 |
|  | $url | 字符串 | 页面URL | 页面URL/页面完整路径 |
|  | $title | 字符串 | 页面标题 | 页面标题 |
|  | $referrer | 字符串 | 页面来源 | 页面来源 |
|  | $referrer\_domain | 字符串 | 页面来源域名 | 页面来源域名 |
|  | $viewport\_width | 字符串 | 视区宽度 | 视区宽度 |
|  | $viewport\_position | 数值 | 视区距顶部的位置 | 视区距顶部的位置 |
|  | $viewport\_height | 数值 | 视区高度 | 视区高度 |
|  | $event\_duration | 数值 | 视区停留时间 | 视区停留时间 |
| $web\_click |  |  | App点击 | Web热图点击事件（用于热图分析） |
|  | $page\_width | 数值 | 页面宽度 | 热图页面宽度 |
|  | $page\_height | 数值 | 页面高度 | 热图页面高度 |
|  | $click\_x | 数值 | 点击X坐标 | 热图点击X坐标 |
|  | $click\_y | 数值 | 点击Y坐标 | 热图点击Y坐标 |
|  | $url | 字符串 | 页面URL | 点击热图时页面URL |
|  | $element\_x | 数值 | 元素X坐标 | 点击元素X坐标 |
|  | $element\_y | 数值 | 元素Y坐标 | 点击元素Y坐标 |
|  | $element\_path | 字符串 | 元素路径 | 热图元素路径 |
|  | $element\_name | 字符串 | 元素名称 | 热图元素名称 |
|  | $element\_type | 字符串 | 元素类型 | 热图元素类型 |
|  | $element\_content | 字符串 | 元素内容 | 热图元素的内容 |
|  | $element\_clickable | 数值 | 是否可以点击元素 | 是否可以点击元素 |
|  | $预制属性 |  |  | 其他预制通用属性 |
| $alias |  |  |  |  |
|  | $original\_id | 字符串 | 匿名ID | 实名绑定前的匿名ID |

### 预置事件通用属性

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x5C5E;&#x6027;ID</th>
      <th style="text-align:left">&#x5C5E;&#x6027;&#x663E;&#x793A;&#x540D;&#x79F0;</th>
      <th style="text-align:left">&#x6570;&#x636E;&#x7C7B;&#x578B;</th>
      <th style="text-align:left">&#x5C5E;&#x6027;&#x8BF4;&#x660E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">$platform</td>
      <td style="text-align:left">&#x5E73;&#x53F0;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x5E94;&#x7528;&#x5E73;&#x53F0;&#xFF0C;&#x679A;&#x4E3E;&#x53D6;&#x503C;&#xFF1A;JS/iOS/Android/Wechat</td>
    </tr>
    <tr>
      <td style="text-align:left">$app_version</td>
      <td style="text-align:left">&#x5E94;&#x7528;&#x7248;&#x672C;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x5E94;&#x7528;&#x7248;&#x672C;&#xFF0C;e.g. V1.0</td>
    </tr>
    <tr>
      <td style="text-align:left">$manufacturer</td>
      <td style="text-align:left">&#x8BBE;&#x5907;&#x5236;&#x9020;&#x5546;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x5236;&#x9020;&#x5382;&#x5546;&#xFF0C; e.g. &#x5C0F;&#x7C73;&#xFF08;&#x901A;&#x8FC7;UA&#x89E3;&#x6790;&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left">$brand</td>
      <td style="text-align:left">&#x8BBE;&#x5907;&#x54C1;&#x724C;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x8BBE;&#x5907;&#x54C1;&#x724C;&#xFF0C;e.g. &#x534E;&#x4E3A;&#x8363;&#x8000;&#xFF08;&#x901A;&#x8FC7;UA&#x89E3;&#x6790;&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left">$model</td>
      <td style="text-align:left">&#x8BBE;&#x5907;&#x578B;&#x53F7;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x8BBE;&#x5907;&#x578B;&#x53F7;&#xFF0C;e.g. iPhone8&#x3001;&#x5C0F;&#x7C73;4&#xFF08;&#x901A;&#x8FC7;UA&#x89E3;&#x6790;&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left">$os</td>
      <td style="text-align:left">&#x64CD;&#x4F5C;&#x7CFB;&#x7EDF;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x64CD;&#x4F5C;&#x7CFB;&#x7EDF;&#xFF0C;e.g.Window&#x3001;MacOS&#xFF08;&#x901A;&#x8FC7;UA&#x89E3;&#x6790;&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left">$os_version</td>
      <td style="text-align:left">&#x64CD;&#x4F5C;&#x7CFB;&#x7EDF;&#x7248;&#x672C;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x64CD;&#x4F5C;&#x7CFB;&#x7EDF;&#x7248;&#x672C;&#xFF0C;e.g.Windows 10&#xFF08;&#x901A;&#x8FC7;UA&#x89E3;&#x6790;&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left">$browser</td>
      <td style="text-align:left">&#x6D4F;&#x89C8;&#x5668;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x6D4F;&#x89C8;&#x5668;&#x540D;&#x79F0;&#xFF0C;e.g. Chrome&#xFF08;&#x901A;&#x8FC7;UA&#x89E3;&#x6790;&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left">$browser_version</td>
      <td style="text-align:left">&#x6D4F;&#x89C8;&#x5668;&#x7248;&#x672C;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x6D4F;&#x89C8;&#x5668;&#x7248;&#x672C;&#xFF0C;e.g. Chrome 62.23.23&#xFF08;&#x901A;&#x8FC7;UA&#x89E3;&#x6790;&#xFF09;</td>
    </tr>
    <tr>
      <td style="text-align:left">$screen_width</td>
      <td style="text-align:left">&#x5C4F;&#x5E55;&#x5BBD;&#x5EA6;</td>
      <td style="text-align:left">&#x6570;&#x503C;</td>
      <td style="text-align:left">&#x5C4F;&#x5E55;&#x5BBD;&#x5EA6;/&#x5C4F;&#x5E55;&#x5206;&#x8FA8;&#x7387;&#xFF0C;e.g.
        1920</td>
    </tr>
    <tr>
      <td style="text-align:left">$screen_height</td>
      <td style="text-align:left">&#x5C4F;&#x5E55;&#x9AD8;&#x5EA6;</td>
      <td style="text-align:left">&#x6570;&#x503C;</td>
      <td style="text-align:left">&#x5C4F;&#x5E55;&#x9AD8;&#x5EA6;/&#x5C4F;&#x5E55;&#x5206;&#x8FA8;&#x7387;&#xFF0C;e.g.
        768</td>
    </tr>
    <tr>
      <td style="text-align:left">$is_login</td>
      <td style="text-align:left">&#x662F;&#x5426;&#x662F;&#x6CE8;&#x518C;&#x7528;&#x6237;</td>
      <td style="text-align:left">&#x5E03;&#x5C14;</td>
      <td style="text-align:left">&#x662F;&#x5426;&#x662F;&#x6CE8;&#x518C;&#x7528;&#x6237;</td>
    </tr>
    <tr>
      <td style="text-align:left">$ip</td>
      <td style="text-align:left">IP</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">
        <p></p>
        <p>&#x6536;&#x6570;&#x670D;&#x52A1;&#x4F1A;&#x81EA;&#x52A8;&#x8BB0;&#x5F55;&#x4E0A;&#x62A5;&#x7684;&#x6570;&#x636E;&#x6765;&#x6E90;
          IP&#xFF0C;&#x6839;&#x636E; IP &#x89E3;&#x6790;&#x4E3A;&#x56FD;&#x5BB6;&#x3001;&#x7701;&#x4EFD;&#x3001;&#x57CE;&#x5E02;&#x4E09;&#x4E2A;&#x5B57;&#x6BB5;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">$country</td>
      <td style="text-align:left">&#x56FD;&#x5BB6;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x9ED8;&#x8BA4;&#x6839;&#x636E;&#x7528;&#x6237;IP&#x89E3;&#x6790;&#x83B7;&#x53D6;,&#x6216;&#x7528;&#x6237;&#x624B;&#x52A8;&#x4E0A;&#x4F20;&#x8986;&#x76D6;</td>
    </tr>
    <tr>
      <td style="text-align:left">$province</td>
      <td style="text-align:left">&#x7701;&#x4EFD;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x9ED8;&#x8BA4;&#x6839;&#x636E;&#x7528;&#x6237;IP&#x89E3;&#x6790;&#x83B7;&#x53D6;,&#x6216;&#x7528;&#x6237;&#x624B;&#x52A8;&#x4E0A;&#x4F20;&#x8986;&#x76D6;</td>
    </tr>
    <tr>
      <td style="text-align:left">$city</td>
      <td style="text-align:left">&#x57CE;&#x5E02;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x9ED8;&#x8BA4;&#x6839;&#x636E;&#x7528;&#x6237;IP&#x89E3;&#x6790;&#x83B7;&#x53D6;,&#x6216;&#x7528;&#x6237;&#x624B;&#x52A8;&#x4E0A;&#x4F20;&#x8986;&#x76D6;</td>
    </tr>
    <tr>
      <td style="text-align:left">$utm_campaign_id</td>
      <td style="text-align:left">&#x6D3B;&#x52A8;ID</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">
        <p>&#x6839;&#x636E;&#x6DFB;&#x52A0;&#x7684;&#x5185;&#x5BB9;&#x81EA;&#x52A8;&#x751F;&#x6210;&#xFF0C;</p>
        <p>&#x6807;&#x8BC6;&#x4E00;&#x6B21;&#x6D3B;&#x52A8;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">$utm_campaign</td>
      <td style="text-align:left">&#x6D3B;&#x52A8;/&#x5E7F;&#x544A;&#x540D;&#x79F0;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x7279;&#x5B9A;&#x7684;&#x63A8;&#x5E7F;&#x6D3B;&#x52A8;&#xFF0C;e.g. &#x53CC;11&#x63A8;&#x5E7F;</td>
    </tr>
    <tr>
      <td style="text-align:left">$utm_medium</td>
      <td style="text-align:left">&#x6D3B;&#x52A8;/&#x5E7F;&#x544A;&#x5A92;&#x4ECB;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x63A8;&#x5E7F;&#x7C7B;&#x578B;&#xFF0C;e.g. SEM&#xFF0C;cpc</td>
    </tr>
    <tr>
      <td style="text-align:left">$utm_source</td>
      <td style="text-align:left">&#x6D3B;&#x52A8;/&#x5E7F;&#x544A;&#x6765;&#x6E90;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x63A8;&#x5E7F;&#x6765;&#x6E90;&#xFF0C;e.g. &#x4ECA;&#x65E5;&#x5934;&#x6761;</td>
    </tr>
    <tr>
      <td style="text-align:left">$utm_content</td>
      <td style="text-align:left">&#x6D3B;&#x52A8;/&#x5E7F;&#x544A;&#x5185;&#x5BB9;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x5E7F;&#x544A;&#x5185;&#x5BB9;&#xFF0C;e.g. &#x4F18;&#x60E0;&#x4FE1;&#x606F;</td>
    </tr>
    <tr>
      <td style="text-align:left">$utm_term</td>
      <td style="text-align:left">&#x6D3B;&#x52A8;/&#x5E7F;&#x544A;&#x5173;&#x952E;&#x5B57;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x5E7F;&#x544A;&#x5173;&#x952E;&#x5B57;&#xFF0C;e.g. &#x7528;&#x6237;&#x753B;&#x50CF;</td>
    </tr>
    <tr>
      <td style="text-align:left">$is_first_day</td>
      <td style="text-align:left">&#x662F;&#x5426;&#x5B89;&#x88C5;&#x540E;&#x9996;&#x65E5;&#x8BBF;&#x95EE;</td>
      <td
      style="text-align:left">&#x5E03;&#x5C14;</td>
        <td style="text-align:left">&#x662F;&#x5426;&#x5B89;&#x88C5;&#x540E;&#x9996;&#x65E5;&#x8BBF;&#x95EE;</td>
    </tr>
    <tr>
      <td style="text-align:left">$lib</td>
      <td style="text-align:left">SDK&#x7C7B;&#x578B;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">SDK&#x7C7B;&#x578B;&#xFF0C;e.g. python&#x3001;iOS&#x7B49;</td>
    </tr>
    <tr>
      <td style="text-align:left">$lib_version</td>
      <td style="text-align:left">SDK&#x7248;&#x672C;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">SDK&#x7248;&#x672C;&#xFF0C; e.g. &#xFF1A;11.2.5</td>
    </tr>
    <tr>
      <td style="text-align:left">$session_id</td>
      <td style="text-align:left">SessionID</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x4F1A;&#x8BDD;ID&#xFF0C;e.g. 515950b8f1a6221c</td>
    </tr>
    <tr>
      <td style="text-align:left">$language</td>
      <td style="text-align:left">&#x8BED;&#x8A00;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x7CFB;&#x7EDF;&#x8BED;&#x8A00;&#xFF0C;e.g. zh-cn</td>
    </tr>
    <tr>
      <td style="text-align:left">$time_zone</td>
      <td style="text-align:left">&#x7528;&#x6237;&#x65F6;&#x533A;</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">&#x7528;&#x6237;&#x65F6;&#x533A;&#xFF0C;e.g.GMT+08:00</td>
    </tr>
    <tr>
      <td style="text-align:left">$user_agent</td>
      <td style="text-align:left">UA</td>
      <td style="text-align:left">&#x5B57;&#x7B26;&#x4E32;</td>
      <td style="text-align:left">UA</td>
    </tr>
    <tr>
      <td style="text-align:left">$web_crawler</td>
      <td style="text-align:left">&#x662F;&#x5426;&#x662F;&#x722C;&#x866B;</td>
      <td style="text-align:left">&#x5E03;&#x5C14;</td>
      <td style="text-align:left">&#x722C;&#x866B;&#x8BC6;&#x522B;</td>
    </tr>
    <tr>
      <td style="text-align:left">$is_time_calibrated</td>
      <td style="text-align:left">&#x662F;&#x5426;&#x4E0E;&#x670D;&#x52A1;&#x7AEF;&#x65F6;&#x95F4;&#x6821;&#x51C6;</td>
      <td
      style="text-align:left">&#x5E03;&#x5C14;</td>
        <td style="text-align:left">&#x662F;&#x5426;&#x4E0E;&#x670D;&#x52A1;&#x7AEF;&#x65F6;&#x95F4;&#x6821;&#x51C6;</td>
    </tr>
    <tr>
      <td style="text-align:left">$debug</td>
      <td style="text-align:left">Debug&#x6A21;&#x5F0F;</td>
      <td style="text-align:left">&#x6570;&#x503C;</td>
      <td style="text-align:left">&#x6807;&#x8BC6;&#x6570;&#x636E;&#x5904;&#x7406;&#x65B9;&#x5F0F;&#xFF0C;0&#xFF1A;&#x975E;debug
        1&#xFF1A;debug&#xFF0C;&#x4E0D;&#x5165;&#x5E93; 2&#xFF1A;debug&#xFF0C;&#x5165;&#x5E93;</td>
    </tr>
  </tbody>
</table>

### **预置用户通用属性**

| 属性ID | 属性名称 | 属性值数据类型 | 属性说明 |
| :--- | :--- | :--- | :--- |
| $original\_id | 匿名ID | 字符串 | 实名绑定前的匿名ID |
| $phone | 手机号 | 字符串 | 非自动采集，需要用户上传 |
| $email | 邮箱 | 字符串 | 非自动采集，需要用户上传 |
| $platform | 最后一次使用平台 | 字符串 | 最后一次使用平台 |
| $lib | 最新版本的SDK类型 | 字符串 | SDK类型，e.g.  python、iOS等 |
| $lib\_version | 最新版本的SDK版本 | 字符串 | SDK版本，e.g. 11.2.5 |
| $country | 所在国家 | 字符串 | 默认根据用户IP解析获取,或用户手动上传覆盖 |
| $province | 所在省份 | 字符串 | 默认根据用户IP解析获取,或用户手动上传覆盖 |
| $city | 所在城市 | 字符串 | 默认根据用户IP解析获取,或用户手动上传覆盖 |
| $is\_login | 是否是注册帐号 | 布尔值 | 是否是调用过alias接口 |
| $signup\_time | 注册时间 | 日期时间 | 注册时间 |
| $first\_visit\_time | 首次访问时间 | 日期时间 | 首次访问时间 |
| $first\_visit\_language | 语言 | 字符串 | 设备语言 |
| $time\_zone | 首次访问时区 | 字符串 | GMT+08:00 |

