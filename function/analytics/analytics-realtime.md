# 实时分析

实时分析，可以实时检测用户点击情况，使用路径，查看使用应用的人员信息，包括地域，设备属性，使用行为等。

## 实时分析模型能解决哪些问题？

1. 市场人员刚刚上线了一个活动，如何及时了解到用户的点击情况？
2. 产品刚刚更新上线，如何快速了解到产品的更新后，用户的下载/使用是否正常？
3. 收到一个用户在使用过程中遇到的 bug反馈，如何需要快速查找到该用户发现其使用路径进而解决bug？

## 功能说明

![ ](https://imguserradar.analysys.cn/fangzhou/img/2018/11/201811221718440115.png)

### 1. 条件定义标区

左侧为条件区域，搜索用户、展示事件、满足条件去查看用户的详细信息

#### 1.1 搜索用户

支持按照用户的属性模糊查询，默认为用户ID ，可下拉选择任一用户属性

![img](https://imguserradar.analysys.cn/fangzhou/img/2018/11/201811221724386659.png)

#### 1.2 展示事件

选择展示的事件， 可以选择触发一个或多个事件，查看触发事件的用户详情

![img](https://imguserradar.analysys.cn/fangzhou/img/2018/11/201811221727254847.png)

#### 1.3 满足条件

选择满足条件，可以按照事件属性设置条件，查看满足的条件的用户详情

![img](https://imguserradar.analysys.cn/fangzhou/img/2018/11/201811221730437556.png)

![img](https://imguserradar.analysys.cn/fangzhou/img/2018/11/201811221730492872.png)

#### 1.4 筛选用户群

用户群默认基于所有用户计算路径，也可以选择其他已保存人群进行计算。

![img](https://imguserradar.analysys.cn/fangzhou/img/2018/11/201811281445185608.png)

### 2. 数据展示区

数据展示区，默认展示最新的 500 条数据，每次刷新页面时重新加载最新数据，会记录新增加事件数据；也可以选择时间范围，展示字段。

#### 2.1 时间范围

![img](https://imguserradar.analysys.cn/fangzhou/img/2018/11/201811281450006618.png)

可以选择今日、昨日、近7日、近30日及 近90等常用范围，也可以在右侧自定义开始时间和结束时间。

#### 2.2 展示字段

![img](https://imguserradar.analysys.cn/fangzhou/img/2018/11/201811281455432685.png)

列表中展示用户行为记录，具体字段包括：

* 事件发生时间
* 用户ID，点击可进入单用户行为档案
* 事件ID
* 事件的通用属性：可通过列选择项展开，选择您想要关注的字段

![img](https://imguserradar.analysys.cn/fangzhou/img/2018/11/201811281452554754.png)

点击字段位置，可展开事件的全部属性。

![img](https://imguserradar.analysys.cn/fangzhou/img/2018/11/201811281458369246.png)

点击用户ID，可以查看单一用户的行为数据，详见[《单用户档案》](../segmentation/segmentation-user-sequence.md)。

### 3. 其他功能

其他保存、强制刷新等等所有分析模型通用功能，详见[《分析》](./)。

