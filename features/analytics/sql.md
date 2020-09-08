---
description: 本文档中描述的内容，主要针对熟悉SQL的数据分析师或技术人员，如果对文档内容有疑惑，请咨询我们。
---

# 自定义查询

如果使用方舟现有的分析功能无法满足您的分析需求，或者您想通过SQL来进行更加灵活的数据分析并导出结果，您可以使用我们提供的自定义查询工具。该工具还可以针对查询结果生成图表完成可视化分析。

{% hint style="info" %}
该工具从方舟4.2.2版本开始提供，如果您的版本还没有该工具，您可以升级方舟到4.2.2版本来进行体验。
{% endhint %}

## 1. 工具入口

进入方舟后在当前项目下 选择【分析 - 自定义查询】即可打开方舟自定义查询工具。

自定义查询工具界面如下图：

![](../../.gitbook/assets/image%20%2872%29.png)

{% hint style="info" %}
易观方舟自定义查询工具只能查询您当前正在使用的项目。如果您在方舟中切换了项目，可以按上述步骤重新点击“自定义查询”打开新的自定义查询工具窗口。
{% endhint %}

## 2. 数据表

在自定义查询工具中只能看到您当前在方舟中访问的项目的表，目前有**event\_vd（事件表）**和**profile\_vd（用户表）**，未来还会支持导入客户自定义的其它辅助数据表。

### 2.1 查看表结构

然后进入PrestoSQL，即可看到当前项目的库名、表名和字段信息。查看步骤如下：

![](../../.gitbook/assets/image%20%2852%29.png)

### 2.2 字段说明

#### **2.2.1 event\_vd （事件表）**

事件表包含了所有事件的详细信息（不包括虚拟事件），该表中存储了所有用户的行为数据，表中的每一行都是一个行为记录。该表中的字段主要分为两类：事件本身的特殊字段和普通的属性字段。

事件特殊字段如下：

| 字段 | 说明 | 示例 |
| :--- | :--- | :--- |
| distinct\_id | 易观方舟为该用户分配的内部 ID，与 profile\_vd 表的 distinct\_id 字段相关联 | 599898980 |
| xwho | 用户行为发生的原始ID，可以为注册用户ID或临时ID | lurenjian |
| xwhen | 用户行为发生的时间，精确到毫秒 | 1552220964474 |
| xwhat | 用户行为名称 | $pageview |
| ds | 用户行为发生的日期，精确到天 | 20190210 |

{% hint style="info" %}
distinct\_id并不是真正的用户ID，而是易观方舟基于用户真实ID生成的用于唯一标识一个用户的ID，详细说明请参见《[如何准确识别用户](../../integration/prepare/user-identify.md)》
{% endhint %}

#### **2.2.2 profile\_vd（用户表）**

用户表的每一行代表一个 用户，类似于事件表，用户表的字段也分为特殊字段和用户的其它 属性两大类，其中特殊字段的说明如下：

| 字段 | 说明 | 示例 |
| :--- | :--- | :--- |
| distinct\_id | 易观方舟为该用户分配的内部 ID，与 profile\_vd 表的 distinct\_id 字段相关联 | 599898980 |
| xwho | 用户行为发生的原始ID，可以为注册用户ID或临时ID | lurenjian |

## 3. 功能使用

### 3.1 基本功能使用

直接在SQL输入框中写SQL即可，注意一次只能执行一条SQL，而且**SQL的未尾不能加分号。**

下图示例查询每天的事件量:

![](../../.gitbook/assets/image%20%28218%29.png)

默认情况下以表格的方式展示结果，为了优化性能，页面最多展示1000条数据，如果您需要查更多的数据，可以将结果下载下来，最多只能下载100w数据。如果您想下载更多数据，请使用数据导出的方式通过api来下载。

#### **3.1.1 图表展示**

![](../../.gitbook/assets/image%20%2834%29.png)

如上图，选择以图表的方式展示数据，支持多种图表。

#### **3.1.2 下载数据**

自定义查询工具支持将查询结果下载，当查询结果数据量大于1000时，页面为了性能考虑只展示1000条，但下载结果时最多可下载100w数据。

![](../../.gitbook/assets/image%20%2887%29.png)

结果下载支持菜单栏中的CSV和Excel指下载的文件格式，如果结果数据量大，建议您尽量下载CSV格式，CSV格式在相同数据量的情况下文件更小，下载更快。

Clipboard指将下载结果复制到粘贴板，您可以在其它文件编辑器里“粘贴”操作，即可将结果粘贴到其它编辑器中。

### **3.2** 常用函数

#### 日期函数

ds字段是事件发生的日期，也是事件表的分区键。ds可以用来快速过滤数据，任何情况下都建议尽量使用ds来过滤数据，而不是使用xwhen

以下是一些ds字段的使用例子

* 查询7月份每天的事件总量以及用户数

```text
SELECT
	ds,
	count(1) AS pv,
	count(DISTINCT distinct_id) AS uv
FROM
	event_vd
where
  ds between 20200701 and 20200731 
GROUP BY
	1
ORDER BY
	1
```

* 查询某1天的数据

```text
select count(1) from event_vd where ds=20200101
```

* 查询连续一段时间的数据

```text
select count(1) from event_vd where ds between 20200701 and 20200707
```

xwhen字段是事件发生的时间，记录的是Unix 时间戳，精度到毫秒。其他日期类型的字段也都是以UNIXTIME的形式存储。例如：1577870326123。 这类字段在使用中，往往可以先用from\_unixtime\(\)函数转换为timestamp类型

通过xwhen查询不同时间粒度的数据

* 按不同时间粒度统计，返回日期类型。按周/月/季时，返回的是该周期第一天.

```text
select
  date_trunc('week',from_unixitme(xwhen/1000)) as cycle,
  count(1) as pv
from 
  event_vd
where 
  ds between 20200101 and 20200630
group by 
  1
order by 
  1
```

* unit：时间单位unit字符串可取的值有：year，quarter，month，week，day，hour，minute，second，millisecond。

通过date\_format 处理日期字段，查询不同粒度周期范围的数据

* 查询每天8点的数据

```text
select 
  * 
from 
  event_vd 
where 
  ds between 20200101 and 20200630 
  and date_format(from_unixtime(xwhen/1000),‘%H’) = ‘08’
```

* 常用标记符

| 标记符 | 说明 |
| :--- | :--- |
| %Y | 年份，4位数字 |
| %y | 年份，2位数字 |
| %m | 月份\(01-12\) |
| %c | 月份\(1-12\) |
| %d | 日\(00-31\) |
| %e | 日\(0-31\) |
| %H | 小时\(00-23\) |
| %k | 小时\(0-23\) |
| %i | 分钟\(00-59\) |
| %s | 秒\(00-50\) |
| %T | 时分秒\(等同于%H:%i:%s\) |
| %w | 星期几\(0-6,周日为一周第一天\) |

也支持通过extract函数直接从日期类型属性中提取数据

* 按年，月查看历史以来订单次数

```text
select 
  extract(year from from_unixtime(xwhen/1000)) as year,
  extract(month from from_unixtime(xwhen/1000)) as month,
  count(1)
from 
  event_vd
where
  xwhat='%order%'
group by 
  1,2
order by 
  1,2
```

* unit：时间单位unit字符串可取的值有：year，quarter，month，week，day，day\_of\_month，day\_of\_week，hour 等等。

### **3.3 常见场景**

#### **通过用户ID查看单个用户的行为序列**

用户上报的行为有可能包含不同的登陆状态数据，因此建议通过profile表的查询distinct\_id后再通过distinct\_id查询行为序列

```text
SELECT
  EVENT .*
FROM
  event_vd event,
  profile_vd PROFILE
WHERE
  EVENT .distinct_id = PROFILE .distinct_id
AND EVENT .ds = 20200701
AND PROFILE .xwho = '001'
ORDER BY
  EVENT .xwhen
```

#### 查询7月份活跃天数大于5天的用户

查询活跃天数，相当于查询用户活跃日期的去重数。活跃时段数，活跃周数等借助日期函数的使用都能通过该方法计算。

```text
SELECT
	distinct_id
FROM
	event_vd
WHERE
	ds BETWEEN 20200701
AND 20200731
GROUP BY
	distinct_id
HAVING
	count(DISTINCT ds) > 5
```

#### 查询7月用户下单次数分布

通过单个用户的1个或者多个行为统计指标，对用户分布进行分析。都统一先把相应指标计算出来，然后通过case...when 语法进行分布。结合不同维度组合分析，可以查看不同维度下的用户分布情况。

```text
SELECT
	CASE
WHEN order_cnts < 5 THEN
	'[0-5)'
WHEN order_cnts < 10 THEN
	'[5-10)'
WHEN order_cnts < 20 THEN
	'[10-20)'
WHEN order_cnts < 50 THEN
	'[20-50)'
ELSE
	'[50-∞)'
END,
 count(1)
FROM
	(
		SELECT
			distinct_id,
			count(1) AS order_cnts
		FROM
			event_vd
		WHERE
			ds BETWEEN 20200701
		AND 20200731
		AND xwhat = 'order'
		GROUP BY
			1
	)
GROUP BY
	1
ORDER BY
	1
```

#### 查询做过A事件但是未做过B事件的用户

通过left join计算2个事件活跃用户的差集

```text
SELECT
	a.distinct_id
FROM
	(
		SELECT DISTINCT
			distinct_id
		FROM
			event_vd
		WHERE
			ds BETWEEN 20200701
		AND 20200731
		AND xwhat = 'A'
	) a
LEFT JOIN (
	SELECT DISTINCT
		distinct_id
	FROM
		event_vd
	WHERE
		ds BETWEEN 20200701
	AND 20200731
	AND xwhat = 'B'
) b ON a.distinct_id = b.distinct_id
WHERE
	b.distinct_is IS NULL
```

通过计算不同事件的活跃次数，再进行过滤

```text
SELECT
	distinct_id
FROM
	(
		SELECT
			distinct_id,
			COUNT_if (xwhat = 'A') AS A_CNTS,
			COUNT_if (xwhat = 'B') AS B_CNTS
		FROM
			event_vd
		WHERE
			ds BETWEEN 20200701
		AND 20200731
		AND xwhat IN ('A', 'B')
		GROUP BY
			1
	)
WHERE
	a_cnts >= 1
AND b_cnts = 0
```

#### 计算用户使用时长

计算会话内相邻2个事件的间隔时长作为每个事件的使用时长，以此统计累积总时长。会话的结束事件，不统计时长。

```text
SELECT
  distinct_id,
  sum(next_xwhen - xwhen)
FROM
  (
    SELECT
      distinct_id,
      xwhen,
      lead (xwhen) over (
        PARTITION BY distinct_id,
        "$session_id"
      ORDER BY
        xwhen
      ) AS next_xwhen
    FROM
      event_vd
    WHERE
      ds BETWEEN 20200701
    AND 20200731
    AND "$session_id" IS NOT NULL
  )
GROUP BY
  1
```

一些场景下，没有采集会话ID，通过相邻2个事件的间隔时长作为每个事件的使用时长，需要设定一个阈值，比如30分钟，间隔时间超过这个时间则不计算。

```text
SELECT
	distinct_id,
	sum(
		CASE
		WHEN next_xwhen - xwhen > 1800000 THEN
			0
		ELSE
			next_xwhen - xwhen
		END
	)
FROM
	(
		SELECT
			distinct_id,
			xwhen,
			lead (xwhen) over (
				PARTITION BY distinct_id
				ORDER BY
					xwhen
			) AS next_xwhen
		FROM
			event_vd
		WHERE
			ds BETWEEN 20200701
		AND 20200731
		AND "$session_id" IS NOT NULL
	)
GROUP BY
	1
```

计算特定2个事件之间的时长。通常可以用来统计某个事件的响应时间，比如 点击按钮A，间隔多久事件B相应。先统计时长，再筛选出当前事件为A，且下一事件为B的记录。同样需要根据业务设定一个阈值,比如1分钟。

```text
SELECT
	distinct_id,
	xwhen,
	next_xwhen - xwhen
FROM
	(
		SELECT
			distinct_id,
			xwhen,
			xwhat,
			lead (xwhen) over (
				PARTITION BY distinct_id
				ORDER BY
					xwhen
			) AS next_xwhen,
			lead (xwhat) over (
				PARTITION BY distinct_id
				ORDER BY
					xwhen
			) AS next_xwhat
		FROM
			event_vd
		WHERE
			ds BETWEEN 20200701
		AND 20200731
		AND xwhat IN ('A', 'B')
		AND "$session_id" IS NOT NULL
	)
WHERE
	xwhat = 'A'
AND next_xwhat = 'B'
AND next_xwhen - xwhen <= 60000
```



#### 通过地理位置计算距离

计算2个地理位置的距离，比如，计算\(112.879325,28.244123\) 与 \(112.887949,28.234258\)的距离，单位为米。

```text
SELECT
	ST_Distance (
		to_spherical_geography (
			ST_point (112.879325, 28.244123)
		),
		to_spherical_geography (
			ST_point (112.887949, 28.234258)
		)
	);
```

### 

### 3.4 使用优化

ds字段是日期字段，您可以使用ds字段过滤数据。为了提高查询性能，请您尽量在SQL中添加ds字段过滤，只查询需要的数据。

易观方舟使用的查询引擎是presto，关于presto的Date和Time类型的函数可以参考presto的官方文档：[https://prestodb.github.io/docs/0.201/functions/datetime.html](https://prestodb.github.io/docs/0.201/functions/datetime.html)



{% hint style="info" %}
以上内容没有解答我的问题？[点击我来反馈](https://support.qq.com/products/118522/) 🚀
{% endhint %}

