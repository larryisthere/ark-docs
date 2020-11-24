---
description: 此功能基于SDK建议使4.4.0及以后版本，适用方舟V4.6.0及以上版本试用此功能
---

# 全埋点模块

## 全埋点集成

{% tabs %}
{% tab title="AndroidStudio SDK 集成" %}
第一步：在项目根目录project目录的`build.gradle`文件中添加全埋点依赖插件

```text
buildscript {
    ...
    dependencies {
        classpath 'com.android.tools.build:gradle:2.2.3'
        //添加易观全埋点插件依赖，当前是使用的全埋点版本号为1.0.2
        classpath 'cn.com.analysys:analysys-allgro-plugin:1.1.2' 
    }
    ...
}
```

第二步：在主项目的build.gradle文件中添加易观插件apply plugin: 'com.analysys.android.plugin'和易观全埋点sdk依赖

```text
apply plugin: 'com.android.application'
//易观全埋点插件 'com.analysys.android.plugin'
apply plugin: 'com.analysys.android.plugin'

dependencies {
   //易观SDK依赖，当前是使用的版本号为4.4.9
   implementation 'cn.com.analysys:analysys-arkanalysys:4.5.2'
}
```
{% endtab %}
{% endtabs %}

### 设置全埋点采集

控制是否采集用户点击可触控元素。参考初始化代码如下：

示例：

```java
public class AnalysysApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        AnalysysConfig config = new AnalysysConfig();
        // 其他初始化带代码
        // 设置开启控件点击自动上报
            config.setAutoTrackClick(true);
        // 设置开启fragment的pageView自动上报
            config.setAutoTrackFragmentPageView(true);
        // 调用初始接口
        AnalysysAgent.init(this, config);
    }
}
```

### 设置全埋点页面黑名单

开发者可以设置某些页面不被全埋点自动采集，自动采集时将会忽略这些页面上的事件。接口如下:

```java
/**
 * 点击自动上报-设置页面级黑名单
 * @param pages 页面名称列表
 */
public void setAutoClickBlackListByPages(List<String> pages);
```

* pages：忽略上报页面全路径名称集合

示例:

```java
List<String> pages = new ArrayList<>();
pages.add("com.analysys.demo.activity.MainActivity");
// 忽略MainActivity页面元素点击自动采集
AnalysysAgent.setAutoClickBlackListByPages(pages);
```

### 设置全埋点控件黑名单

开发者可以设置某些控件触发后不被全埋点自动采集，自动采集时将会忽略这些控件事件的采集。接口如下:

```java
/**
 * 点击自动上报-设置元素类型级黑名单
 * @param element 单个控件对象
 */
public void setAutoClickBlackListByViewTypes(List<Class> viewTypes);
```

示例:

```java
List<Class> viewTypes = new ArrayList<>();
viewTypes.add(RatingBar.class);
// 忽略控件类点击自动采集
AnalysysAgent.setAutoClickBlackListByViewTypes(viewTypes);
```

#### 

### 设置全埋点view黑名单

开发者可以设置某个view元素不被全埋点自动采集，自动采集时将会忽略这些view。接口如下:

```java
/**
 * 点击自动上报-设置某个view类型级黑名单
 * @param element 单个控件对象
 */
public void setAutoClickBlackListByView(View element);
```

示例:

```java
private View mView;
...
省略mView初始化逻辑
...
// 忽略当前控件对象自动采集
AnalysysAgent.setAutoClickBlackListByView(mView);
```

