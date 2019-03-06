# 分析

在实际的业务分析中，不同的场景需要用到不同的分析方法，分析模块集中了运营中各个环节常用的一些分析模型。

## 分析模型通用操作

### 1、新建图表

**三个入口：**

A.  分析导航下选择一个常用分析模型，或者打开更多从中选择一个模型

B.  导航栏快捷方式，选择创建分析图表

C.  从已保存图表点击进入，修改条件，另存为

![新建图表](https://imguserradar.analysys.cn/files/ark/doc/24.gif)

**进入图表详情页**

![图表操作](https://imguserradar.analysys.cn/files/ark/doc/25.png)

#### A. 指标定义区：选择指标、分析维度、人群
该区域用来定义想要分析的数据
- A1 选择指标，指标约束条件
- A2 选择查询数据维度
- A3 选择指标对比人群
- A4 选择时间范围
- A5 选择时间维度

#### B. 图形展示区
该区域可视化展示选定条件下的数据表现

#### C. 数据列表区
该区域展示计算结果的具体数据

### 2. 保存图表
条件筛选好后可以直接保存，保存后将提交系统计算；

![保存](https://imguserradar.analysys.cn/files/ark/doc/26.png)

也可以点击 **开始计算**立即查看分析结果，

若是日常查看的指标，可以点击图标右上方的**保存**按钮，输入图表名称，选择想要保存到的看板；

也可以在已保存的图表基础上修改部分条件，另存为一个新的图表。

![保存](https://imguserradar.analysys.cn/files/ark/doc/27.gif)

### 3. 数据下钻
对于图表上任意数据节点，左击可以下钻查看选定人群概览，或者直接保存该人群，便于触达这个用户群或在其他模型中进一步分析。

![下钻](https://imguserradar.analysys.cn/files/ark/doc/28.gif)

### 4. 导出数据
分析结果中的列表数据可以到处到Excel进行二次分析

![导出数据](https://imguserradar.analysys.cn/files/ark/doc/29.png)


## 支持的分析模型
每个模型的具体的使用方法前往阅读相应章节

**用户获取**
  - [渠道分析](./analystics_chart_channel.md)

**了解用户**
  - [事件分析](./analystics_chart_event.md)
  - [基础用户画像](./analystics_chart_composition.md)
  - [领域偏好](./analystics_chart_categorypreference.md)
  - [场景偏好](./analystics_chart_usagepreference.md)
  - [APP偏好](.//analystics_chart_apppreference.md)
  
**用户转化**
  - [转化漏斗](./analystics_chart_funnel.md)
  - [智能路径](./analystics_chart_pathfinder.md)
  
**用户留存**
  - [留存分析](./analystics_chart_retention.md)
  
更多模型持续开发中……