# 页面时长统计功能

## 目前支持页面时长统计的SDK有那些SDK

目前有：Android SDK、iOS SDK 、JS SDK

## 页面时长功能介绍

页面访问时长统计，当开启自动采集页面功能或手动调用页面采集功能，当页面关闭时自动上报页面关闭事件`page_close`。包含以下属性，页面停留时间`pageStayTime`（单位：毫秒）、页面地址`$url`和页面标题`$title`。

## 页面时长功能触发时机

Android：在页面切换的时候会触发如：Activity、Fragments切换

iOS：在页面切换的时候会触发如：UIViewController

JS：URL变化

## 页面时长为什么不是预制事件

此功能需要用户按需选择，故不作为预制事件，请手动加入元事件管理中

