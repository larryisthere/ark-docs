---
description: WeCode SDK 中使用对其他模块介绍
---

# WeCode SDK插件

## 加密模块介绍

加密模块SDK，根据用户初始化SDK时参数encryptType的值对上报日志进行对应加密。  
使用加密模块SDK时，需确保证该插件在基础SDK文件前引入

```javascript


//1.ES6集成
//如为自行下载SDK。将以下代码添加至集成WeCode SDK代码位置即可。
//将AnalysysAgent_WeCode_Encrypt.es6.min.js文件存放地址替换到import后的引入文件地址
import '设置为ES6加密模块SDK实际存放地址'
...//其他SDK代码
//如为npm获取SDK。将以下代码添加至集成WeCode SDK代码位置即可
import 'ans-wecode-sdk/sdk/AnalysysAgent_WeCode_Encrypt.es6.min.js'

//2.AMD 规范集成（以 RequireJS 为例）
//获取AnalysysAgent_WeCode_Encrypt.amd.min.js，假设该文件放到与 require.js 同一目录中将以下代码添加至集成JS SDK代码位置上方即可
requirejs('./AnalysysAgent_WeCode_Encrypt.amd.min.js')
```

## 页面访问时长模块介绍

页面访问时长模块SDK，在页面关闭/手动调用页面统计事件`pageView`时，自动上报页面关闭事件`page_close`。自动采集页面停留时间`pageStayTime`（单位：毫秒）、页面地址`$url`和页面标题`$title`。

### 集成方式

```javascript
//1.ES6集成
//如为自行下载SDK。将以下代码添加至集成WeCode SDK代码位置即可。
//将AnalysysAgent_WeCode_PageViewStayTime.es6.min.js文件存放地址替换到import后的引入文件地址
import '设置为ES6页面访问时长模块SDK实际存放地址'
...//其他SDK代码
//如为npm获取SDK。将以下代码添加至集成WeCode SDK代码位置即可
import 'ans-wecode-sdk/sdk/AnalysysAgent_WeCode_PageViewStayTime.es6.min.js'

//2.AMD 规范集成（以 RequireJS 为例）
//获取AnalysysAgent_WeCode_PageViewStayTime.amd.min.js，
//假设该文件放到与 require.js 同一目录中将以下代码添加至集成JS SDK代码位置上方即可
requirejs('./AnalysysAgent_WeCode_PageViewStayTime.amd.min.js')
```

## 曝光点采集模块介绍

曝光点采集模块SDK，在WeCode SDK初始化时设置曝光点元素列表或在页面元素标签中标记该元素为曝光点元素，在该元素出现到可视界面时，采集当前可视界面的符合统计标准的元素曝光事件`exposure_points`。自动采集曝光点ID`exposure_id`、曝光点内容`exposure_content`、页面地址`$url`和页面标题`$title`。

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
//1.ES6集成
//如为自行下载SDK。将以下代码添加至集成WeCode SDK代码位置即可。
//将AnalysysAgent_WeCode_ExposurePoint.es6.min.js文件存放地址替换到import后的引入文件地址
import '设置为ES6页面访问时长模块SDK实际存放地址'
...//其他SDK代码
//如为npm获取SDK。将以下代码添加至集成WeCode SDK代码位置即可
import 'ans-wecode-sdk/sdk/AnalysysAgent_WeCode_ExposurePoint.es6.min.js'

//2.AMD 规范集成（以 RequireJS 为例）
//获取AAnalysysAgent_WeCode_ExposurePoint.amd.min.js，
//假设该文件放到与 require.js 同一目录中将以下代码添加至集成WeCode SDK代码位置上方即可
requirejs('./AnalysysAgent_WeCode_ExposurePoint.amd.min.js')
```

