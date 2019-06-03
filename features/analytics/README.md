# 分析

在实际的业务分析中，不同的场景需要用到不同的分析方法，分析模块集中了运营各个环节常用的一些分析模型。

## 分析模型通用操作

### 1. 新建图表

**三个入口：**

A. 分析导航下选择一个常用分析模型

![ ](https://imguserradar.analysys.cn/fangzhou/img/2018/08/201808092215060331.png)

也可以在分析的过程中快速切换分析模型

![ ](https://imguserradar.analysys.cn/fangzhou/img/2018/08/201808092218121388.jpg)

B. 在自己的看板里，右上角时刻可以选择或者创建图表，而在空白看板中，从页面的中央的按钮也可以快速添加

![ ](https://imguserradar.analysys.cn/fangzhou/img/2018/08/201808092223137517.jpg)

C. 还可以将已保存的图表留存为新的图表

### 2. 使用图表

![ ](https://imguserradar.analysys.cn/fangzhou/img/2018/08/201808092254029967.jpg)

1. 切换分析模型
2. 设定分析条件
3. 设置图表名称以及图表级别的操作：保存、另存为及下载
4. 选择用于对比的分群以及设定数据采样的级别
5. 时间选择，选择查询的时间段以及按时间聚合的粒度；右侧可以切换数据展示的形式
6. 数据展示区

### 3. 保存图表

条件筛选好后可以直接保存，保存后会存入分析图表库中，以便后续使用；

![ ](https://imguserradar.analysys.cn/fangzhou/img/2018/08/201808092147459839.png)

若是日常查看的指标，可以点击图标右上方的**保存**按钮，输入图表名称，选择想要保存到的看板；

也可以在已保存的图表基础上修改部分条件，然后另存为一个新的图表。

![ ](https://imguserradar.analysys.cn/fangzhou/img/2018/08/201808092259093950.gif)

### 4. 人群下钻

对于图表上任意数据节点，左击可以下钻查看选定人群概览，或者直接保存该人群，便于触达这个用户群或在其他模型中进一步分析。

![ ](https://imguserradar.analysys.cn/fangzhou/img/2018/08/201808092157021064.gif)

### 5. 导出数据

分析结果对应的数据保存为 Excel 格式，以便进行二次分析。

![ ](https://imguserradar.analysys.cn/fangzhou/img/2018/08/201808092158458590.png)

## 支持的分析模型

每个模型的具体的使用方法前往阅读相应章节

**1. 获取用户**

{% page-ref page="channel/" %}

{% page-ref page="heatmap/" %}

**2. 了解用户**

{% page-ref page="event.md" %}

{% page-ref page="session.md" %}

{% page-ref page="realtime.md" %}

**3. 用户转化**

{% page-ref page="funnel.md" %}

{% page-ref page="pathfinder.md" %}

**4. 用户留存**

{% page-ref page="retention.md" %}

更多模型持续开发中……

