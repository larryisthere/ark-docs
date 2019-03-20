# 预置事件和属性

为了帮助用户更简单的使用方舟，我们预置了一些事件、事件属性和用户属性；部分默认自动采集，非默认采集的部分，用户有需要的话，可以直接使用预置字段作为事件 ID、属性 ID 进行上报。

## 预置事件

### 1. Event 类事件，用于行为计算的事件

| 事件ID | 事件名称 | 事件说明 | Android自动采集 | iOS自动采集 | JS自动采集 | 小程序自动采集 |
| :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $startup | 启动 | APP启动 / 打开网站 | Y | Y | Y | Y |
| $end | 关闭 | APP关闭 | Y | Y | - | - |
| $pageview | 浏览页面 | 浏览APP/网站页面 | Y | Y | Y | Y |
| $push\_receiver\_success | 消息推送成功 | 设备收到推送消息时触发 | N | N | - | - |
| $push\_click | 点击推送消息 | 设备点击了推送消息时触发 | N | N | - | - |
| $push\_process\_success | 成功处理push消息 | 成功处理push消息 | N | N | - | - |
| $webstay | 视区停留 | 停留在可视区域 | - | - | N | - |

其中 $webstay 是用于记录用户停留在可视化区域的事件，目的是分析可视化区域，不会作为普通的事件去分析，所以不会出现在事件分析下拉框中

### 2. Profile 系列事件，用于描述用户

| 事件ID | 事件说明 | Android自动采集 | iOS自动采集 | JS自动采集 | 小程序自动采集 |
| :--- | :--- | :---: | :---: | :---: | :---: |
| $alias | 关联身份 | N | N | N | N |
| $profile\_set | 设置用户信息，覆盖写 | N | N | N | N |
| $profile\_set\_once | 设置用户信息，有则不进行任何操作 | N | N | N | N |
| $profile\_increment | 增加或减少用户信息中的数字类型的属性 | N | N | N | N |
| $profile\_delete | 删除用户信息 | N | N | N | N |
| $profile\_append | 数组属性添加值 | N | N | N | N |
| $profile\_unset | 设置用户信息中的某个属性为空 | N | N | N | N |

除了这些预置事件之外，更多业务相关的事件，需要用户自定义埋点上报。

## 预置事件属性

事件属性描述事件发生的方式和内容，是分析过程中的维度，不同事件会有一些共同的属性，我们称之为通用属性，也会有一些自己的属性。

### 1. Event 事件通用属性

  
table th:first-of-type {  
    width: 90px;  
}  


  
table th:nth-of-type\(2\) {  
    width: 139px;  
}  


  
table th:nth-of-type\(3\) {  
    width: 80px;  
}  


  
table th:nth-of-type\(4\) {  
    width: 215px;  
}  


  
table th:nth-of-type\(5\) {  
    width: 60px;  
}  
  
table th:nth-of-type\(6\) {  
    width: 60px;  
}  
  
table th:nth-of-type\(7\) {  
    width: 60px;  
}  
  
table th:nth-of-type\(8\) {  
    width: 75px;  
}  


| 属性ID | 属性名称 | 属性值 数据类型 | 属性说明 | Android自动采集 | iOS自动采集 | JS自动采集 | 小程序自动采集 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $platform | 平台 | 字符串 | 应用平台，枚举取值：Android/iOS/JS/WeChat | Y | Y | Y | Y |
| $app\_version | 应用版本 | 字符串 | 应用版本，e.g. V1.0 | Y | Y | - | - |
| $device\_type | 设备类型 | 字符串 | 设备类型，e.g.  PC、移动设备 | N | N | N | N |
| $manufacturer | 设备制造商 | 字符串 | 制造厂商， e.g. 小米 | Y | Y | - | - |
| $brand | 设备品牌 | 字符串 | 设备品牌，e.g. 华为荣耀 | Y | Y | Y | Y |
| $model | 设备型号 | 字符串 | 设备型号，e.g. iPhone8、小米4 | Y | Y | Y | Y |
| $os | 操作系统 | 字符串 | 操作系统，e.g.Window、MacOS | Y | Y | Y | Y |
| $os\_version | 操作系统版本 | 字符串 | 操作系统版本，e.g.Windows 10 | Y | Y | Y | Y |
| $browser | 浏览器 | 字符串 | 浏览器名称，e.g. Chrome | - | - | Y | Y |
| $browser\_version | 浏览器版本 | 字符串 | 浏览器版本，e.g. Chrome 62.23.23 | - | - | Y | Y |
| $network | 网络类型 | 字符串 | 网络类型，e.g. WIFI、2G、3G、4G | Y | Y | - | Y |
| $carrier\_name | 运营商 | 字符串 | 接入运营商名称，e.g. 中国联通 | Y | Y | - | - |
| $screen\_width | 屏幕宽度 | 整数型 | 屏幕宽度/屏幕分辨率，e.g. 1920 | Y | Y | Y | Y |
| $screen\_height | 屏幕高度 | 整数型 | 屏幕高度/屏幕分辨率，e.g. 768 | Y | Y | Y | Y |
| $is\_login | 是否是注册用户 | 布尔值 | 是否是注册用户 | Y | Y | Y | Y |
| $ip | IP | 字符串 | IP | Y | Y | Y | Y |
| $country1 | 国家 | 字符串 | 事件发生时所在国家，e.g. 中国、美国 | Y | Y | Y | Y |
| $province2 | 省份 | 字符串 | 事件发生时所在省份，e.g.   北京、上海、福建 | Y | Y | Y | Y |
| $city3 | 城市 | 字符串 | 事件发生时所在城市，e.g. 北京、厦门 | Y | Y | Y | Y |
| $utm\_campaign\_id | 活动ID | 字符串 | 根据添加的内容自动生成，标识一次活动 | Y | Y | Y | Y |
| $utm\_campaign | 活动名称（广告名称） | 字符串 | 活动名称（广告名称） | Y | Y | Y | Y |
| $utm\_medium | 活动媒介（广告媒介） | 字符串 | 活动媒介（广告媒介） | Y | Y | Y | Y |
| $utm\_source | 活动来源（广告来源） | 字符串 | 活动来源（广告来源） | Y | Y | Y | Y |
| $utm\_content | 活动内容（广告内容） | 字符串 | 活动内容（广告内容） | Y | Y | Y | Y |
| $utm\_term | 活动关键字（广告关键字） | 字符串 | 广告关键字 | Y | Y | Y | Y |
| $is\_first\_day | 是否安装后首日访问 | 布尔值 | 是否安装后首日访问 | Y | Y | Y | Y |
| $channel | 下载渠道 | 字符串 | 下载渠道，事件中携带 | Y | Y | - | - |
| $lib | SDK类型 | 字符串 | SDK类型，例e.g.   python、iOS等 | Y | Y | Y | Y |
| $lib\_version | SDK版本 | 字符串 | SDK版本 e.g. ：11.2.5 | Y | Y | Y | Y |
| $time\_zone | 用户时区 | 字符串 | GMT+08:00 | Y | Y | Y | Y |
| $debug | Debug模式 | 整数型 | debug模式，profile系列中不携带   0：非debug 1：debug，不入库 2：debug，入库 | Y | Y | Y | Y |
| $language | 语言 | 字符串 | zh-cn | Y | Y | Y | Y |
| $session\_id | 会话标识 | 字符串 | 515950b8f1a6221c | Y | Y | Y | Y |

注1、注2、注3：自动通过解析 IP 获得

### 2. 部分 Event 事件自身属性

**$startup**

| 属性ID | 属性名称 | 属性值 数据类型 | 属性说明 | Android自动采集 | iOS自动采集 | JS自动采集 | 小程序自动采集 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $is\_first\_time | 是否安装后首次访问 | 布尔值 | 是否安装后首次访问 | Y | Y | Y | N |
| $is\_from\_background | 是否从后台唤醒 | 布尔值 | 是否从后台唤醒恢复 | Y | Y | N | N |

**$end**

| 属性ID | 属性名称 | 属性值 数据类型 | 属性说明 | Android自动采集 | iOS自动采集 | JS自动采集 | 小程序自动采集 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $duration | 使用时长 | 数值 | 从启动到关闭的使用时长 | Y | Y | - | - |

**$pageview**

| 属性ID | 属性名称 | 属性值 数据类型 | 属性说明 | Android自动采集 | iOS自动采集 | JS自动采集 | 小程序自动采集 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $url | 页面URL | 字符串 | 页面URL/页面完整路径 | Y | Y | Y | Y |
| $title | 页面标题 | 字符串 | 页面标题 | Y | Y | Y | Y |
| $referrer | 页面来源 | 字符串 | 页面来源 | - | - | Y | Y |
| $referrer\_domain | 页面来源域名 | 字符串 | 页面来源域名 | - | - | Y | - |
| $traffic\_source\_type | 流量来源类型 | 字符串 | 流量来源类型，数据处理 | - | - | N | N |
| $search\_engine | 搜索引擎 | 字符串 | 标识搜索引擎来源 | - | - | N | N |
| $search\_keyword | 搜索关键词 | 字符串 | 标识搜索关键词 | - | - | N | N |

**$push\_receiver\_success**

| 属性ID | 属性名称 | 属性值 数据类型 | 属性说明 | Android自动采集 | iOS自动采集 | JS自动采集 | 小程序自动采集 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $action\_type | 操作类型 | 整数型 | 点击消息通知后的操作类型 | N | N | - | - |
| $action | 操作 | 字符串 | 点击消息通知后的操作 | N | N | - | - |
| $utm\_campaign\_id | 活动ID | 字符串 | 根据添加的内容自动生成，标识一次活动 | N | N | - | - |
| $utm\_campaign | 活动名称（广告名称） | 字符串 | 活动名称（广告名称） | N | N | - | - |
| $utm\_medium | 活动媒介（广告媒介） | 字符串 | 活动媒介（广告媒介） | N | N | - | - |
| $utm\_source | 活动来源（广告来源） | 字符串 | 活动来源（广告来源） | N | N | - | - |
| $utm\_content | 活动内容（广告内容） | 字符串 | 活动内容（广告内容） | N | N | - | - |
| $utm\_term | 活动关键字（广告关键字） | 字符串 | 广告关键字 | N | N | - | - |
| $cpd | 自定义 | 字符串 | 自定义模块 | N | N | - | - |

**$push\_click**

| 属性ID | 属性名称 | 属性值 数据类型 | 属性说明 | Android自动采集 | iOS自动采集 | JS自动采集 | 小程序自动采集 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $action\_type | 操作类型 | 整数型 | 点击消息通知后的操作类型 | N | N | - | - |
| $action | 操作 | 字符串 | 点击消息通知后的操作 | N | N | - | - |
| $utm\_campaign\_id | 活动ID | 字符串 | 根据添加的内容自动生成，标识一次活动 | N | N | - | - |
| $utm\_campaign | 活动名称（广告名称） | 字符串 | 活动名称（广告名称） | N | N | - | - |
| $utm\_medium | 活动媒介（广告媒介） | 字符串 | 活动媒介（广告媒介） | N | N | - | - |
| $utm\_source | 活动来源（广告来源） | 字符串 | 活动来源（广告来源） | N | N | - | - |
| $utm\_content | 活动内容（广告内容） | 字符串 | 活动内容（广告内容） | N | N | - | - |
| $utm\_term | 活动关键字（广告关键字） | 字符串 | 广告关键字 | N | N | - | - |
| $cpd | 自定义 | 字符串 | 自定义模块 | N | N | - | - |

**$push\_process\_success**

| 属性ID | 属性名称 | 属性值 数据类型 | 属性说明 | Android自动采集 | iOS自动采集 | JS自动采集 | 小程序自动采集 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $action\_type | 操作类型 | 整数型 | 点击消息通知后的操作类型 | N | N | - | - |
| $action | 操作 | 字符串 | 点击消息通知后的操作 | N | N | - | - |
| $utm\_campaign\_id | 活动ID | 字符串 | 根据添加的内容自动生成，标识一次活动 | N | N | - | - |
| $utm\_campaign | 活动名称（广告名称） | 字符串 | 活动名称（广告名称） | N | N | - | - |
| $utm\_medium | 活动媒介（广告媒介） | 字符串 | 活动媒介（广告媒介） | N | N | - | - |
| $utm\_source | 活动来源（广告来源） | 字符串 | 活动来源（广告来源） | N | N | - | - |
| $utm\_content | 活动内容（广告内容） | 字符串 | 活动内容（广告内容） | N | N | - | - |
| $utm\_term | 活动关键字（广告关键字） | 字符串 | 广告关键字 | N | N | - | - |
| $cpd | 自定义 | 字符串 | 自定义模块 | N | N | - | - |

**$webstay**

| 属性ID | 属性名称 | 属性值 数据类型 | 属性说明 | Android自动采集 | iOS自动采集 | JS自动采集 | 小程序自动采集 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $url | 页面URL | 字符串 | 页面URL/页面完整路径 | - | - | N | - |
| $title | 页面标题 | 字符串 | 页面标题 | - | - | N | - |
| $referrer | 页面来源 | 字符串 | 页面来源 | - | - | N | - |
| $viewport\_width | 视区宽度 | 数值 | 视区宽度 | - | - | N | - |
| $viewport\_position | 视区距顶部的位置 | 数值 | 视区距顶部的位置 | - | - | N | - |
| $viewport\_height | 视区高度 | 数值 | 视区高度 | - | - | N | - |
| $event\_duration | 视区停留时间 | 数值 | 视区停留时间 | - | - | N | - |

### 3. Profile 系列事件属性

**$alias**

| 属性ID | 属性名称 | 属性值 数据类型 | 属性说明 | Android自动采集 | iOS自动采集 | JS自动采集 | 小程序自动采集 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $lib | 平台 | 字符串 | SDK类型，例如python、iOS等 | Y | Y | Y | Y |
| $is\_login | 是否登录 | BOOL | true/false | Y | Y | Y | Y |
| $lib\_version | SDK版本 | 字符串 | SDK版本 如：11.2.5 | Y | Y | Y | Y |
| $platform | 平台 | 字符串 | JS/iOS/Android/Wechat | Y | Y | Y | Y |
| $debug | debug模式 | 整数型 | debug模式 | Y | Y | Y | Y |
| $original\_id | 设备id | 字符串 | 上一个id | Y | Y | Y | Y |

**$profile\_set**

| 属性ID | 属性名称 | 属性值 数据类型 | 属性说明 | Android自动采集 | iOS自动采集 | JS自动采集 | 小程序自动采集 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| $lib | 平台 | 字符串 | SDK类型，例如python、iOS等 | Y | Y | Y | Y |
| $is\_login | 是否登录 | BOOL | true/false | Y | Y | Y | Y |
| $lib\_version | SDK版本 | 字符串 | SDK版本 如：11.2.5 | Y | Y | Y | Y |
| $platform | 平台 | 字符串 | JS/iOS/Android/Wechat | Y | Y | Y | Y |
| $debug | debug模式 | 整数型 | debug模式 | Y | Y | Y | Y |

## 预置用户属性

| 属性ID | 属性名称 | 属性值 数据类型 | 属性说明 | Android自动采集 | iOS自动采集 | JS自动采集 | 小程序自动采集 |
| :--- | :--- | :--- | :--- | :---: | :---: | :---: | :---: |
| distinct\_id | 唯一ID | 字符串 | - | - | - | - | - |
| xwho | 用户ID | 字符串 |  | Y | Y | Y | Y |
| xwhen | 用户属性更新时间 | 日期时间 |  | Y | Y | Y | Y |
| $idfv | IDFV | 字符串 | IDFV | - | Y | - | - |
| $idfa | IDFA | 字符串 | IDFA | - | Y | - | - |
| $mac | MAC | 字符串 | MAC | Y | - | - | - |
| $imei | IMEI | 字符串 | IMEI | Y | - | - | - |
| $getui | 个推ID | 字符串 | 个推推送ID | N | N | - | - |
| $jpush | 极光ID | 字符串 | 极光推送ID | N | N | - | - |
| $baidu | 百度ID | 字符串 | 百度推送ID | N | N | - | - |
| $xiaomi | 小米ID | 字符串 | 小米推送ID | N | N | - | - |
| $email | 邮箱 | 字符串 | 邮箱 | N | N | N | N |
| $platform | 最后一次使用平台 | 字符串 | 最后一次使用平台 | Y | Y | Y | Y |
| $lib | 最新版本的SDK类型 | 字符串 | SDK类型，e.g.  python、iOS等 | Y | Y | Y | Y |
| $lib\_version | 最新版本的SDK版本 | 字符串 | SDK版本，e.g. 11.2.5 | Y | Y | Y | Y |
| $country | 所在国家 | 字符串 | 用户所在国家 | N | N | N | N |
| $province | 所在省份 | 字符串 | 用户所在省份 | N | N | N | N |
| $city | 所在城市 | 字符串 | 用户所在城市 | N | N | N | N |
| $is\_login | 是否是登录帐号 | 布尔值 | 是否是登录账号 | Y | Y | Y | Y |
| $signup\_time | 注册时间 | 日期时间 | 注册时间 | N | N | N | N |
| $first\_visit\_time | 首次访问时间 | 日期时间 | 首次访问时间 | Y | Y | Y | Y |
| $time\_zone | 用户时区 | 字符串 | GMT+08:00 | Y | Y | Y | Y |
| $debug | Debug模式 | 整数型 | debug模式，profile系列中不携带   0：非debug 1：debug，不入库 2：debug，入库 | Y | Y | Y | Y |
| $language | 语言 | 字符串 | zh-cn | Y | Y | Y | Y |
| $session\_id | 会话标识 | 字符串 | 515950b8f1a6221c | Y | Y | Y | Y |

[![ ](https://imguserradar.analysys.cn/fangzhou/img/2019/01/201901151711159657.jpeg)](https://ark.analysys.cn/view/sign/signup.html?campaign_id=2111486795&utm_campaign=文档注册&utm_medium=自媒体&utm_source=文档&utm_content=&utm_term=)

