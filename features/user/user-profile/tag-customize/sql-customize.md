# 如何自定义SQL创建标签

## 数据源

![](../../../../.gitbook/assets/image%20%28288%29.png)

在系统通过SQL创建标签，可以使用的数据源主要有事件行为表、用户属性表以及标签表。其中标签表根据创建的标签数量不同，存在多张表的可能，表名按照smarttag\_开头，后跟数字。有关事件行为和用户属性的详细介绍，请参考《[数据模型]()》、《[数据格式](../../../../integration/prepare/data-type.md)》。

| 表名称 | 说明 | 关联字段 |
| :--- | :--- | :--- |
| event\_vd | 存储用户事件行为数据 | xwho,distinct\_id |
| profile\_vd | 存储用户属性数据 | xwho,distinct\_id |
| smarttag\_\* | 存储用户标签数据 | xwho,distinct\_id |

## 查询结果格式  

在使用自定义SQL进行标签加工时，需注意遵从如下的格式返回查询数据。

{% hint style="info" %}
查询仅支持返回三列数据，请按照**xwho,distinct\_id和所需返回的标签值**顺序返回查询结果。

xwho为用户id，在用户未登录的情况下，xwho 会是一个方舟匿名生成的 ID，在已登录的情况下的，xwho 应及时被客户自己业务系统的ID所改写。

distinct\_id为方舟的唯一ID，与xwho一一对应，在计算时会使用这个ID。
{% endhint %}

{% tabs %}
{% tab title="字符" %}
```sql
/*加工字符型标签*/
SELECT xwho,distinct_id,USER_SEX AS VALUE 
FROM profile_vd
WHERE ds=20200501 
```

**USER\_SEX**即为查询返回的字符型标签
{% endtab %}

{% tab title="数值" %}
```sql
/*
加工数值型标签
统计3月份用户发生某个事件的次数作为标签
*/
SELECT xwho,distinct_id,COUNT(*) AS VALUE 
FROM event_vd
WHERE xwhat='事件名称'
AND ds between 20200301 and 20200331
GROUP BY
xwho,distinct_id
```
{% endtab %}

{% tab title="集合" %}
```sql
/*
加工集合类型标签
用户购买次数最多的3个商品
*/
SELECT xwho,distinct_id, array_agg(productdel) AS value
FROM 
(SELECT xwho,distinct_id, product,RANK() 
OVER (PARTITION BY distinct_id ORDER BY ct DESC) AS rank 
FROM 
(SELECT xwho,distinct_id,product,COUNT(*) AS ct 
FROM event_vd 
WHERE 
xwhat='buy' /*事件名称*/
GROUP BY xwho,distinct_id,product /*事件属性-商品名称*/ 
)a 
)b 
WHERE 
rank <=3 /*指定取次数最多的多少个商品作为标签*/
GROUP BY xwho,distinct_id
```
{% endtab %}

{% tab title="时间" %}
```sql
/*
加工时间类型标签
用户最后购买时间
*/
SELECT xwho,distinct_id,max(xwhen)/*xwhen为方舟事件数据模型的预置字段，保存事件发生时间*/
AS VALUE
FROM event_vd
WHERE xwhat='buy'
AND ds BETWEEN 20200301 AND 20200330
GROUP BY 
xwho,distinct_id
```
{% endtab %}
{% endtabs %}

## 动态时间范围

在之前的介绍中我们介绍到可以配置标签的更新周期，每天、每周、每月定期执行标签加工。在实际的加工过程中，会遇到需要每次加工计算前多少日、上月、当月等等动态时间范围的标签，这里将主要介绍如何在SQL中定义动态时间。

```sql
/*
使用动态时间范围创建标签
最近30天用户启动次数
*/
SELECT xwho,distinct_id,count(*) as value 
FROM event_vd 
WHERE xwhat='$startup' 
AND from_unixtime(xwhen/1000) /*事件发生时间，xwhen为时间戳*/
BETWEEN now()-interval '30' day /*统计事件的时间范围，当前时间到30天前*/
AND now()/*当前时间*/
GROUP BY xwho,distinct_id
```

标签的默认加工时间在每日或指定周期的0点开始，因此now\(\)代表了执行当日的时间，但由于是在0时开始执行，所以基本不会包含当日的数据，可以认为实际统计的日期在执行的上一日到前30日的时间范围。通过指定interval后的数值，可以计算如：

近1日：  BETWEEN now\(\)- interval '1' day and now\(\)

近7日：  BETWEEN now\(\)- interval '7' day and now\(\)

近30日：BETWEEN now\(\)- interval '30' day and now\(\)

 易观方舟使用的查询引擎是presto ，关于presto的Date和Time类型的函数可以参考presto的官方文档：[https://prestodb.github.io/docs/0.201/functions/datetime.html](https://prestodb.github.io/docs/0.201/functions/datetime.html)

## SQL标签示例

### 1 将用户属性表属性作为标签

加工标签时，有时需要将上传至用户属性表的属性纳入标签体系，这时候通过SQL可以直接查询属性表，指定需要保存为标签的属性进行加工。

```sql
SELECT xwho,distinct_id,user_level AS VALUE
FROM profile_vd /*用户属性表*/

/*user_level为属性表中的用户等级字段*/
```

{% hint style="info" %}
将用户属性作为标签保存的好处在于，标签表会保存标签的历史数据，如以上的user\_level，在用户表中我们只能保存用户当前的user\_level状态，如果我们想要去查询用户一个月前的用户等级通过属性表是无法回溯的，而在标签中可以回溯，甚至我们对比历史和当前的状态，选择用户级别发生明显变化的用户群出来。
{% endhint %}

### 2 首次访问距当前天数

```sql
SELECT xwho,distinct_id,
DATE_DIFF('day',from_unixtime("$first_visit_time"/1000),now())
AS VALUE
FFRFROM
FROM profile_vd
/*
$first_visit_time为用户属性表首次访问时间
DATE_DIFF函数计算起始时间的间隔天数
*/
```

### 3 最后购买距当前天数

```sql
SELECT xwho,distinct_id,
DATE_DIFF('day',from_unixtime(lastbuy/1000),now())
AS VALUE
FROM
(
SELECT xwho,distinct_id,MAX(xwhen) as lastbuy
FROM event_vd
WHERE xwhat='buy'
GROUP BY xwho,distinct_id
) a

/*
xwhat指定购买行为，取购买行为的最大日期作为最后购买时间
通过DATE_DIFF函数计算最后购买时间到当前的间隔天数作为标签
*/
```

### 4 RFM用户价值标签

在常用的用户分层标签中，RFM价值模型应用最为广泛，该模型通过三个基本的指标（最后购买时间距当前天数、购买次数、购买总金额）对用户进行分段。

```sql
SELECT xwho,distinct_id,
CASE 
/*4. 按照每个用户rfm三项的得分给用户做分层*/
WHEN r=1 and f=1 and m=1 THEN '重要价值客户'
WHEN r=0 and f=1 and m=1 THEN '重要保持客户'
WHEN r=0 and f=0 and m=1 THEN '重要挽留客户'
WHEN r=1 and f=0 and m=1 THEN '重要发展客户'
WHEN r=1 and f=1 and m=0 THEN '一般价值客户'
WHEN r=1 and f=0 and m=0 THEN '一般发展客户'
WHEN r=0 and f=1 and m=0 THEN '一般保持客户'
WHEN r=0 and f=0 and m=0 THEN '一般挽留客户'
END AS VALUE
FROM
(
SELECT xwho,distinct_id,
/*3. 每个用户的得分比上平均分，大于平均则为1，否则为0*/
CASE WHEN r/avg_r>1 THEN 1 ELSE 0 END AS r,
CASE WHEN f/avg_f>1 THEN 1 ELSE 0 END AS f,
CASE WHEN m/avg_m>1 THEN 1 ELSE 0 END AS m 
FROM
(
 Ss select a.xwho as xwho,a.distinct_id as distinct_id,
a.r as r,a.f as f,a.m as m,
/*2. 求分段后每个指标的平均分数*/
avg(a.r) over() as avg_r,
avg(a.f) over() as avg_f,
avg(a.m) over() as avg_m
FROM
(
SELECT xwho,distinct_id,tag_152,tag_148,tag_149,
Ceiling(percent_rank() over (order by tag_152 desc)/0.2) as r,
/*1. 此处0.2为按照20%的比例将指标切分为5段，并赋值1-5*/
Ceiling(percent_rank() over (order by tag_148)/0.2) as f,

ceiling(percent_rank() over (order by tag_149)/0.2) as m

FROM smarttag_1 
where ds=20200604
)a
)b
)c
/*
计算此标签可预先将rfm三项分别保存为标签再进行计算，
此处tag_152,tag_148,tag_149分别对应了最后购买距今天数、购买次数和购买总金额
*/
```

### 5 首末次行为

将用户首次或最后一次发生某个行为的事件属性作为标签，如首次购买金额、最后访问平台等

```sql
/*最后访问平台*/
SELECT a.xwho,a.distinct_id,a.platform AS VALUE
FROM event_vd 
a
INNER JOIN
(
SELECT xwho,distinct_id,max(xwhen) as t
FROM event_vd
WHERE xwhat='$startup'
GROUP BY xwho,distinct_id
) b
ON a.xwho=b.xwho and a.distinct_id=b.distinct_id and a.xwhen=b.t

/*可通过调整max为min计算首次访问的平台名称作为标签*/
```

### 6 最近30天访问天数

```sql
SELECT xwho, distinct_id, 
COUNT(DISTINCT format_datetime(from_unixtime(xwhen/1000),'yyyyMMdd')) AS VALUE 
FROM event_vd 
WHERE 
xwhat='$startup' and
from_unixtime(xwhen/1000) between now()-interval '30' day and now() 
GROUP BY xwho,distinct_id

/*
可替换xwhat指定需要统计的事件名称
可替换指定需要统计的时间周期
可调整时间格式，统计访问的月数、周数、小时数等
*/
```

