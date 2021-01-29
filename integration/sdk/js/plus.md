---
description: JS SDK 中使用对其他模块介绍
---

# JS SDK插件

## 加密模块介绍

加密模块，根据用户初始化SDK时参数`encryptType`的值对上报日志进行对应加密。  
使用加密模块SDK时，需确保证该插件在基础SDK文件前引入

```javascript
//1.同步集成
//将以下JS代码添加到接入JS SDK代码的上方。
//将AnalysysAgent_Encrypt.min.js文件访问地址替换到script标签中的src位置
<script type="text/javascript" src="/*设置为非ES6加密模块SDK实际存放地址*/"></script>
...
//集成JS SDK

//2.ES6集成
//如为自行下载SDK。将以下代码添加至集成JS SDK代码位置即可。
//将AnalysysAgent_Encrypt.es6.min.js文件存放地址替换到import后的引入文件地址
import '设置为ES6加密模块SDK实际存放地址'
...//其他SDK代码
//如为npm获取SDK。将以下代码添加至集成JS SDK代码位置即可
import 'ans-javascript-sdk/sdk/AnalysysAgent_Encrypt.es6.min.js'

//3.CommonJS 规范集成
//将以下代码添加至集成JS SDK代码位置上方即可
require('ans-javascript-sdk/sdk/AnalysysAgent_Encrypt.es6.min.js')

//4.AMD 规范集成（以 RequireJS 为例）
//获取AnalysysAgent_Encrypt.amd.min.js，假设该文件放到与 require.js 同一目录中将以下代码添加至集成JS SDK代码位置上方即可
requirejs('./AnalysysAgent_Encrypt.amd.min.js')
```

## GBK转码模块介绍

GBK转码模块，针对符合UTM的参数值如为GBK/GB2312编码格式，进行UTF-8编码转换。  
使用加密模块SDK时，需确保证该插件在基础SDK文件前引入

```javascript
//1.同步集成或异步集成
//将以下JS代码添加到接入JS SDK代码的上方。将AnalysysAgent_GBK.min.js文件访问地址替换到script标签中的src位置
<script type="text/javascript" src="/*设置为非ES6GBK转码模块SDK实际存放地址*/"></script>
...
//集成JS SDK



//2.ES6集成
//如为自行下载SDK。将以下代码添加至集成JS SDK代码位置即可。将AnalysysAgent_GBK.es6.min.js文件存放地址替换到import后的引入文件地址
import '设置为ES6GBK转码模块SDK实际存放地址'
//如为npm获取SDK。将以下代码添加至集成JS SDK代码位置即可
import 'ans-javascript-sdk/sdk/AnalysysAgent_GBK.es6.min.js'

//3.CommonJS 规范集成
//将以下代码添加至集成JS SDK代码位置上方即可
var AnalysysAgent = require('ans-javascript-sdk/sdk/AnalysysAgent_GBK.es6.min.js')

//4.AMD 规范集成（以 RequireJS 为例）
//获取AnalysysAgent_GBK.amd.min.js，假设该文件放到与 require.js 同一目录中将以下代码添加至集成JS SDK代码位置上方即可
requirejs('./AnalysysAgent_GBK.amd.min.js')
```

## 可视化埋点介绍

### 接入可视化模块

将 `AnalysysAgent_JS_SDK_VISUAL.min.js` 放到与 JS SDK`AnalysysAgent_JS_SDK.min.js` 文件存放的同一文件目录中。或通过：`SDKFileDirectory`接口指定SDK目录。

### 设置请求埋点配置地址

```javascript
//该地址为获取可视化埋点列表接口。
//标准化方舟中，该地址与上报日志地址相同。
{
    visitorConfigURL:"/*设置为实际地址*/"
}
```

### 集成方式

该方式集成需要手动配置可视化模块`AnalysysAgent_JS_SDK_VISUAL.min.js`文件访问路径并将可视化模块js文件放到可通过url访问的目录中。

```javascript
//1.异步或同步集成
//在AnalysysAgent.initApi中，配置加载可视化模块文件目录访问地址
{
    SDKFileDirectory:"/*设置为实际地址*/"//可通过url访问该目录中的该可视化模块js文件。
}
//以Vue.js为例，将AnalysysAgent_JS_SDK_VISUAL.min.js放到/public/js/sdk或/static/js/sdk目录中。
//配置加载可视化模块文件目录访问地址
{
    SDKFileDirectory:"./js/sdk/"//Vue.js中public或static目录中内容，
    //构建时会copy至dist目录中。所以该设置时没有public或static这一层目录。
}
//RequireJS为例，将AnalysysAgent_JS_SDK_VISUAL.min.js放到require.js同一目录中。
//配置加载可视化模块文件目录访问地址
{
    SDKFileDirectory:"./js/lib/"//RequireJS中require.js的访问目录为./js/lib/。
}

//2.npm获取SDK集成
//需将ans-javascript-sdk/sdk/AnalysysAgent_JS_SDK_VISUAL.min.js复制到可通过url访问的目录
//也可通过url访问该文件来验证可视化模块是否可以使用
{
//可通过url访问该目录中的该可视化模块js文件。
//例如：所放文件目录为"./js/sdk/",可视化模块js文件访问地址为：http://localhost:8080/js/sdk/AnalysysAgent_JS_SDK_VISUAL.min.js
    SDKFileDirectory:"/*设置为实际地址*/"
}
```

### 允许 iframe 加载

使用可视化埋点功能需要使用 `iframe` 来加载您的网站。如果您的网站禁止了 `iframe` 加载，就无法使用可视化埋点功能，需要在服务器配置中设置 `X-Frame-Options` 允许 `iframe` 加载。

```javascript
//配置nginx中 X-Frame-Options 响应头
//您的网站为https协议，https://example.com/为您访问的方舟的域名
add_header X-Frame-Options: Allow-From https://example.com
//您的网站为http协议，http://example.com为您访问的方舟的域名
add_header X-Frame-Options: Allow-From http://example.com
```

{% hint style="info" %}
注意：

X-Frame-Options更多使用方式请参考：[X-Frame-Options 响应头](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/X-Frame-Options)

可视化埋点时候需要方舟与埋点页面协议头相同。否则会被浏览器拒绝，无法进行可视化埋点。
{% endhint %}

### window对象

以下 window 对象中的属性被复写后将导致可视化埋点功能无法正常使用。

```javascript
window.top
window.parent
window.name
window.location
```

或有防止iframe嵌套的js代码。可视化埋点功能无法正常使用。 例如：

```javascript
if(self !== window.top){
    window.reload()
}

```

## 热图模块介绍

将 `AnalysysAgent_JS_SDK_HEATMAP.min.js` 放到与 JS SDK`AnalysysAgent_JS_SDK.min.js` 文件存放的同一文件目录中，展示热图的时会自动调用。或通过：`SDKFileDirectory`接口指定SDK目录。

```javascript
//1.异步或同步集成
//在AnalysysAgent.initApi中，配置加载热图模块文件目录访问地址
{
    SDKFileDirectory:"/*设置为实际地址*/"//可通过url访问该目录中的该热图模块js文件。
}
//以Vue.js为例，将AnalysysAgent_JS_SDK_HEATMAP.min.js放到public/js/sdk目录中。
//配置加载热图模块文件目录访问地址
{
    SDKFileDirectory:"./js/sdk/"//Vue.js中public目录中内容，构建时会copy至dist目录中。所以该设置时没有public这一层目录。
}
//RequireJS为例，将AnalysysAgent_JS_SDK_HEATMAP.min.js放到require.js同一目录中。
//配置加载热图模块文件目录访问地址
{
    SDKFileDirectory:"./js/lib/"//RequireJS中require.js的访问目录为./js/lib/。
}

//2.npm获取SDK集成
//需将ans-javascript-sdk/sdk/AnalysysAgent_JS_SDK_HEATMAP.min.js复制到可通过url访问的目录
//也可通过url访问该文件来验证热图模块是否可以使用
{
//可通过url访问该目录中的该热图模块js文件。
//例如：所放文件目录为"./js/sdk/",热图模块js文件访问地址为：http://localhost:8080/js/sdk/AnalysysAgent_JS_SDK_HEATMAP.min.js
    SDKFileDirectory:"/*设置为实际地址*/"
}
```

## 页面访问时长模块介绍

页面访问时长模块，当开启自动采集功能或手动调用页面统计功能，页面关闭时自动上报页面关闭事件`page_close`。包含以下属性，页面停留时间`pageStayTime`（单位：毫秒）、页面地址`$url`和页面标题`$title`。

### 集成方式

```javascript
//1.同步集成
//将以下JS代码添加到接入JS SDK代码的上方。
//将AnalysysAgent_PageViewStayTime.min.js文件访问地址替换到script标签中的src位置
<script type="text/javascript" src="/*设置为非ES6页面访问时长SDK实际存放地址*/"></script>
...
//集成JS SDK

//2.ES6集成
//如为自行下载SDK。将以下代码添加至集成JS SDK代码位置即可。
//将AnalysysAgent_PageViewStayTime.es6.min.js文件存放地址替换到import后的引入文件地址
import '设置为ES6页面访问时长模块SDK实际存放地址'
...//其他SDK代码
//如为npm获取SDK。将以下代码添加至集成JS SDK代码位置即可
import 'ans-javascript-sdk/sdk/AnalysysAgent_PageViewStayTime.es6.min.js'

//3.CommonJS 规范集成
//将以下代码添加至集成JS SDK代码位置上方即可
require('ans-javascript-sdk/sdk/AnalysysAgent_PageViewStayTime.amd.min.js')

//4.AMD 规范集成（以 RequireJS 为例）
//获取AnalysysAgent_PageViewStayTime.amd.min.js，
//假设该文件放到与 require.js 同一目录中将以下代码添加至集成JS SDK代码位置上方即可
requirejs('./AnalysysAgent_PageViewStayTime.amd.min.js')
```

## 曝光点采集模块介绍

曝光点采集模块，在JS SDK初始化时设置曝光点元素列表或在页面元素标签中标记该元素为曝光点元素，在该元素出现到可视界面时，采集当前可视界面的符合统计标准的元素曝光事件`exposure_points`。自动采集曝光点ID`exposure_id`、曝光点内容`exposure_content`、页面地址`$url`和页面标题`$title`。

设置采集曝光点元素点击行为，可通过初始化设置或曝光点元素标签中标记该元素为采集曝光点点击行为。在曝光点元素被点击时，采集当前元素的曝光点点击事件`exposure_click` 。采集曝光点ID`exposure_id`、曝光点内容`exposure_content`、页面地址`$url`和页面标题`$title`。

### 初始化设置曝光点

在SDK初始化设置中增加以下设置。

```javascript
{
    exposure:{
        valid_time:300,//默认300毫秒。曝光点元素有效展示时间
        element_list:[
            document.getElementById('id_1'),
            document.getElementById('id_2')
        ],//设置曝光点元素列表。
        //或者
        //element_list:function(){
        //    return document.getElementsByTagName('a')
        //}，
        exposure_click:false,//或true,默认:false。设置是否采集曝光点元素点击行为。
        property:{
            text:'123'
        },//设置曝光点元素自定义属性。自定义属性的属性值仅支持String类型
        // 或者
        //property:function(ele){
        //    return {
        //        text:'123'
        //    }
        //},
        multiple:false//或true，或3000。默认:false。设置曝光元素同页面下重复采集该元素有效曝光
    }
}
```

* `exposure` 曝光点采集设置。类型：`JSON` 
  * `valid_time` 曝光点元素有效展示事件。默认:`300`。单位:毫秒。类型:`Number` 。
  * `element_list` 设置曝光点元素列表。类型:`Array` / `Function (返回值为Array/HTMLCollection)`
  * exposure\_click 设置是否采集曝光点点击行为。默认:`false` 。类型:`Boolean` 。
  * property 设置曝光点元素自定义属性。类型:`JSON`  / `Function (返回值为JSON)`
  * multiple 设置曝光元素同页面下重复采集该元素有效曝光。默认:`false` 。类型:`Boolean` / `Number` 

**元素标签设置曝光点**

在页面元素上增加`data-ark-exposure` 标签，如增加其标签内容，则该内容为该曝光元素的自定义属性。该标签值必须为可转为JSON的字符串。

```javascript
<!--设置曝光点元素-->
<div class="banner01"  data-ark-exposure="{&quot;exposure_id&quot;:&quot;abc&quot;}"></div>

```

### 集成方式

```javascript
//1.同步集成
//将以下JS代码添加到接入JS SDK代码的上方。
//将AnalysysAgent_ExposurePoint.min.js文件访问地址替换到script标签中的src位置
<script type="text/javascript" src="/*设置为非ES6页面访问时长SDK实际存放地址*/"></script>
...
//集成JS SDK

//2.ES6集成
//如为自行下载SDK。将以下代码添加至集成JS SDK代码位置即可。
//将AnalysysAgent_ExposurePoint.es6.min.js文件存放地址替换到import后的引入文件地址
import '设置为ES6页面访问时长模块SDK实际存放地址'
...//其他SDK代码
//如为npm获取SDK。将以下代码添加至集成JS SDK代码位置即可
import 'ans-javascript-sdk/sdk/AnalysysAgent_ExposurePoint.es6.min.js'

//3.CommonJS 规范集成
//将以下代码添加至集成JS SDK代码位置上方即可
require('ans-javascript-sdk/sdk/AnalysysAgent_ExposurePoint.amd.min.js')

//4.AMD 规范集成（以 RequireJS 为例）
//获取AAnalysysAgent_ExposurePoint.amd.min.js，
//假设该文件放到与 require.js 同一目录中将以下代码添加至集成JS SDK代码位置上方即可
requirejs('./AnalysysAgent_ExposurePoint.amd.min.js')
```

## mPaas模块介绍

mPaas通信模块，为iOS端使用mPaas框架H5容器且使用Hybrid模式时，通过自定义 JSAPI方式进行JS SDK与iOS SDK之间通信。H5页面中需集成JS SDK与该插件。Android端H5容器内H5页面无需集成该插件。

### 集成方式

```javascript
//1.同步集成
//将以下JS代码添加到接入JS SDK代码的上方。
//将AnalysysAgent_MPAAS.min.js文件访问地址替换到script标签中的src位置
<script type="text/javascript" src="/*设置为非ES6mPaas通信模块SDK实际存放地址*/"></script>
...
//集成JS SDK

//2.ES6集成
//如为自行下载SDK。将以下代码添加至集成JS SDK代码位置即可。
//将AnalysysAgent_MPAAS.es6.min.js文件存放地址替换到import后的引入文件地址
import '设置为ES6mPaas通信模块SDK实际存放地址'
...//其他SDK代码
//如为npm获取SDK。将以下代码添加至集成JS SDK代码位置即可
import 'ans-javascript-sdk/sdk/AnalysysAgent_MPAAS.es6.min.js'

//3.CommonJS 规范集成
//将以下代码添加至集成JS SDK代码位置上方即可
require('ans-javascript-sdk/sdk/AnalysysAgent_MPAAS.amd.min.js')

//4.AMD 规范集成（以 RequireJS 为例）
//获取AnalysysAgent_MPAAS.amd.min.js，
//假设该文件放到与 require.js 同一目录中将以下代码添加至集成JS SDK代码位置上方即可
requirejs('./AnalysysAgent_MPAAS.amd.min.js')
```

