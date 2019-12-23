---
description: 此功能基于SDK建议使4.4.0及以后版本，适用方舟V4.5.3及以上版本试用此功能
---

# 全埋点介绍

### 设置全埋点采集

控制是否采集用户点击可触控元素。接口如下：

```objectivec
+ (void)setAutoTrackClick:(BOOL)isAuto;
```

* isAuto：开关值，默认为NO关闭，设置YES为开

示例：

```objectivec
//  设置采集全埋点
[AnalysysAgent setAutoTrackClick:YES];
```

Swift示例:

```swift
AnalysysAgent.setAutoTrackClick(true);
```

### 设置全埋点页面黑名单

开发者可以设置某些页面不被全埋点自动采集，设置后`setAutoTrackClick`自动采集时将会忽略这些页面上的事件。接口如下:

```objectivec
+ (void)setAutoClickBlackListByPages:(NSSet<NSString *> *)controllerNames;
```

* controllers：需要忽略的控制器名称，字符串集合。

示例：

```objectivec
[AnalysysAgent setAutoClickBlackListByPages:[NSSet setWithObject:@"CFHomePageController"]];
```

Swift代码示例:

必须使用 `包名.类名`

```swift
AnalysysAgent.setAutoClickBlackListByPages(["SwiftOnlineShopDemo.CFHomePageController"]);
```

### 设置全埋点控件黑名单

开发者可以设置某些控件不被全埋点自动采集，设置后`setAutoTrackClick`自动采集时将会忽略这些页面。接口如下:

```objectivec
(void)setAutoClickBlackListByViewTypes:(NSSet<NSString *> *)viewNames;;
```

* viewNames：需要忽略的控件名称，字符串集合。

示例：

```objectivec
[AnalysysAgent setAutoClickBlackListByViewTypes:[NSSet setWithObject:@"ANSButton"]];
```

Swift代码示例:

必须使用 `包名.类名`

```swift
AnalysysAgent.setAutoClickBlackListByViewTypes(["SwiftOnlineShopDemo.CFButton"]);
```

