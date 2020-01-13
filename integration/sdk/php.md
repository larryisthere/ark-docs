---
description: PHP SDK使用说明
---

# PHP SDK

主要用于服务端 PHP 应用，如 PHP Web 应用的后台服务。集成前请先下载SDK

{% hint style="info" %}
[Releases包下载及更新说明](https://github.com/analysys/ans-php-sdk/releases)
{% endhint %}

## 1. 集成 SDK

将PHP SDK拷贝到本地工程内并在所需要位置引用 PHP SDK。

```php
    require_once 'AnalysysAgent_PHP_SDK.php';
```

## 2. SDK 初始化

### 2.1 获取配置信息

需获取的配置信息为：

* 数据接收地址：`http://host:port`

### 2.2 初始化接口

在程序所需初始化位置处，调用构造函数 new AnalysysAgent\(Consumer, APP\_KEY\) 初始化 PHP SDK 实例。如下：

```php
$app_key = '9421608fd544a65e';
$serverUrl = 'https://arksdk.analysys.cn:4089/';
$consumer = new SyncConsumer($serverUrl); //同步
//$consumer = new BatchConsumer($server); // 批量
//$consumer = new FileConsumer($file_path); // 落地文件
$analysys_agent = new AnalysysAgent($consumer, $appid);;
```

app\_key：网站获取的 Key

SyncConsumer：为实时事件收集器

事件收集器还提供批量收集器，用户可以指定为实时收集和批量收集

#### 2.2.1 实时收集器

用户每触发一次上传，该收集器则立即上传数据至接收服务器：

```php
new SyncConsumer($serverUrl);
```

* serverUrl：数据接收地址

#### 2.2.2 批量收集器

用户触发上传，该收集器将先缓存数据，直到数量达到用户设置的阈值或者用户设置的等待时间，才会触发真正的上传：

```php
new BatchConsumer($serverUrl);
new BatchConsumer($serverUrl, $batchNum);
new BatchConsumer($serverUrl, $batchNum, $batchSec);
```

* $serverUrl：数据接收地址
* $batchNum：批量发送数量，默认值：20条
* $batchSec：批量发送等待时间\(秒\)，默认值：10秒

#### 2.2.3 批量收集器

该收集器可以把用户触发的事件经过封装处理成标准的JSON写入本地文件中。支持多线程/多进程同时写入同一文件，默认为同步一条条写入文件，可以指定是否异步批量写入以及批量写入的条数：

```php
new FileConsumer($file_path);

new FileConsumer($file_path, $rule);

new FileConsumer($file_path, $rule, $async);

new FileConsumer($file_path, $rule, $async, $batch_num);
```

* $file\_path：数据保存的目录
* $async： 是否异步批量保存\(默认为false，如果设置为true则默认20条进行落地操作\)
* $rule: 文件存储规则。当传值为"DAY"时，文件按日写入数据（即每日生成一个文件），当传值为"HOUR"时，文件按小时写入数据（即每小时生成一个文件）。默认规则为按小时
* $batch\_num： 批量保存数量，默认值：20条

说明：程序结束前需要调用 flush\(\) 接口，该接口可以把本地缓存的尚未写入文件的数据写入到文件中。

至此 SDK初始化完成，调用 SDK 。 用户需要一直持有该初始化实例，用以之后数据传输，直至用户程序退出。

## 3. 基础接口介绍

### 3.1 Debug 模式

Debug 主要用于开发者测试，接口如下：

```php
$analysys_agent->setDebugMode($debug);
```

* $debug：debug 模式,类型：Number,默认为`0`。有以下三状态：
  * 0：表示关闭 Debug 模式
  * 1：表示打开 Debug 模式，但该模式下发送的数据仅用于调试，不计入平台数据统计 
  * 2：表示打开 Debug 模式，该模式下发送的数据可计入平台数据统计

    注意：发布版本时debug模式设置为\`0\`或删除设置Debug模式代码。

### 3.2 用户关联

服务端数据上报很多时候都要面对同来自客户端的用户数据进行关联的场景。如果不做数据关联，在服务端上报的数据就无法被方舟认为与客户端上报的数据是同一个人的，这时就需要在服务端上报事件前完成关联。

具体来说就是将 `$registerId` 和 `$distinctId`关联，计算时方舟就会认为是二者一个用户的行为。该接口是在 `$distinctId` 发生变化的时候调用（通常为用户登录），用来告诉 SDK `$distinctId` 变化前后的 ID 对应关系。该场景一般应用在用户注册/登录的过程中。比如：一个匿名用户浏览商品，系统为其分配的`$distinctId` = "1234567890987654321"，随后该匿名用户进行注册，系统为其分配了新的注册 ID，`$registerId` = "ABCDEF123456789"，此时就需要调用 alias 接口对两个 ID 进行关联。接口如下：

```php
$analysys_agent->alias($registerId, $distinctId, $platform, $xwhen);
```

* `$registerId`：用户注册 ID，长度大于 0，且小于 255字符
* `$distinctId`：用户匿名ID，长度大于 0，且小于 255字符，一般**从 Cookies 的 ARK\_ID 中获取**
* `$platform`：平台类型,内容范围：JS、WeChat、Android、iOS
* `$xwhen`: 用户自定义时间戳\(带毫秒的13位时间戳\)

示例：匿名用户浏览商品到注册会员

```php
$distinctId = $_COOKIE['ARK_ID'];
$registerId  = 'ABCDEF123456789';
$platform = 'JS';
function msectime()
{
    list($msec, $sec) = explode(' ', microtime());
    return (float) sprintf('%.0f', (floatval($msec) + floatval($sec)) * 1000);
}
$xwhen = msectime();

$analysys_agent->alias($registerId,$distinctId,$platform, $xwhen);
```

### 3.3 统计事件

事件跟踪，设置事件名称和事件详细信息。

{% hint style="warning" %}
请注意若用户已登录，请在执行 track 前进行用户关联，否则前后会被当做两人。
{% endhint %}

接口如下：

```php
$analysys_agent->track($distinctId, $isLogin, $eventName, $properties, $platform, $xwhen);
```

* `$distinctId`：用户 ID，长度大于 0 且小于 255字符。若用户已登录则为用户自己的ID，通常为手机号、电子邮件等；若用户未登录则可取客户端生成的 ARK\_ID，通常会存储在 Cookie 中。
* `$isLogin`：用户 ID 是否是登录 ID。若用户已登录则为 true，未登录则为 false。
* `$eventName`：事件ID,以字母或 `$` 开头，可包含字母、数字、下划线和 `$`，字母不区分大小写，`$`开头为预置事件,不支持乱码和中文,最大长度 99字符
* `$properties`: 事件属性,最多包含 100条,且 key 以字母或 `$` 开头，可包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件属性,最大长度 99字符,不支持乱码和中文,value 类型约束\(String/Number/boolean/list/数组\)，若为字符串,最大长度255字符
* `$platform`：平台类型,内容范围：JS、WeChat、Android、iOS
* `$xwhen`: 用户自定义时间戳\(带毫秒的13位时间戳\)

示例：用户浏览商品

```php
// 浏览商品
$distinctId = '1234567890987654321';
$isLogin = true;
$eventName  = 'eventName';
$platform = 'JS';
$bookList = array(
    'is AnalysysAgent PHP SDK'
);
$properties = array(
    '$ip'=>'112.112.112.112',
    'productType'=>'PHP书籍',
    'productName'=>'bookList',
    'producePrice'=>'60',
    'shop'=>'在线'
);
function msectime()
{
    list($msec, $sec) = explode(' ', microtime());
    return (float) sprintf('%.0f', (floatval($msec) + floatval($sec)) * 1000);
}
$xwhen = msectime();

$analysys_agent->track($distinctId, $isLogin, $eventName, $properties, $platform, $xwhen);
```

### 3.4 用户属性设置

SDK提供以下接口供用户设置用户的属性，比如用户的年龄/性别等信息。调用后会在用户表中插入一条用户ID为 $registerId 的记录，若 $registerId 的记录已存在，则覆盖。

{% hint style="warning" %}
请注意若用户已登录，请在执行 profileSet 前进行用户关联，否则前后会被当做两人。
{% endhint %}

示例：

```php
$analysys_agent->profileSet($registerId,$isLogin,$properties,$platform, $xwhen);
```

* `$registerId`: 用户ID,长度大于0且小于255字符
* `$isLogin`: 用户ID是否是登录 ID
* `$properties`: 用户属性
* `$platform`: 平台类型,内容范围：JS、WeChat、Android、iOS
* `$xwhen`: 用户自定义时间戳\(带毫秒的13位时间戳\)

其中，$properties 是一个标准的K-V结构，K和V均有相应的约束条件，如不符合则丢弃该次操作。 参数约束：

* **属性名称**

  以字母或`$`开头，可包含字母、数字、下划线和`$`，字母不区分大小写，`$`开头为预置事件属性,最大长度99字符,不支持乱码和中文

* **属性值**

  支持部分类型：string/number/boolean/集合/数组; 若为字符串,则最大长度255字符; 若为数组或集合,则最多包含100条,且key约束条件与属性名称一致,value最大长度255字符

用户注册后将用户ID从匿名更改为业务系统的ID，示例如下：

```php
$registerId = '1234567890987654321';
$isLogin = true;
$platform = 'JS';
$properties = array(
    '$city'=>'北京',
    '$province'=>'北京',
    'nickName'=>'昵称123',
    'userLevel'=>0,
    'userPoint'=>0,
    'interest'=>array(
        '户外活动',
        '足球赛事',
        '游戏'
    )
);
$analysys_agent->profileSet($registerId, $isLogin, $properties, $platform);
```

## 4. 更多接口

### 4.1 用户属性

#### 4.1.1 设置用户固有属性

只在首次设置时有效的属性。如：用户的注册时间。如果被设置的用户属性已存在，则这条记录会被忽略而不会覆盖已有数据，如果属性不存在则会自动创建。接口如下：

```php
$analysys_agent->profileSetOnce($registerId, $isLogin, $properties, $platform, $xwhen);
```

* `$registerId`: 用户ID,长度大于0且小于255字符
* `$isLogin`: 用户ID是否是登录 ID
* `$properties`: 事件属性
* `$platform`: 平台类型,内容范围：JS、WeChat、Android、iOS
* `$xwhen`: 用户自定义时间戳\(带毫秒的13位时间戳\)

示例：要统计用户注册时间

```php
$registerId = 'ABCDEF123456789';
$isLogin = true;
$platform = 'JS';
$properties = array(
    'registerTime'=>'20180101101010'
);

function msectime()
{
    list($msec, $sec) = explode(' ', microtime());
    return (float) sprintf('%.0f', (floatval($msec) + floatval($sec)) * 1000);
}
$xwhen = msectime();

$analysys_agent->profileSetOnce($registerId, $isLogin, $properties, $platform, $xwhen);
```

#### 4.1.2 设置用户属性相对变化值

设置用户属性的单个相对变化值\(相对增加,减少\)，只能对数值型属性进行操作，如果这个Profile之前不存在,则初始值为0。接口如下：

```php
$analysys_agent->profileIncrement($registerId, $isLogin, $properties, $platform, $xwhen);
```

* `$registerId`: 用户ID,长度大于0且小于255字符
* `$isLogin`: 用户ID是否是登录 ID
* `$properties`: 事件属性
* `$platform`: 平台类型,内容范围：JS、WeChat、Android、iOS
* `$xwhen`: 用户自定义时间戳\(带毫秒的13位时间戳\)

示例：用户注册初始积分为0，在用户购买商品后，用户的积分增加20，则调用该接口，用户的积分变为0+20=20了：

```php
$registerId = 'ABCDEF123456789';
$isLogin = true;
$platform = 'JS';
$properties = array(
    'userPoint'=>20
);

function msectime()
{
    list($msec, $sec) = explode(' ', microtime());
    return (float) sprintf('%.0f', (floatval($msec) + floatval($sec)) * 1000);
}
$xwhen = msectime();

$analysys_agent->profileIncrement($registerId, $isLogin, $properties, $platform, $xwhen);
```

#### 4.1.3 增加列表类型的属性

为列表类型的属性增加一个或多个元素，如：用户新增兴趣爱好，接口如下：

```php
$analysys_agent->profileAppend($registerId, $isLogin, $properties, $platform, $xwhen);
```

* `$registerId`: 用户ID,长度大于0且小于255字符
* `$isLogin`: 用户ID是否是登录 ID
* `$properties`: 事件属性
* `$platform`: 平台类型,内容范围：JS、WeChat、Android、iOS
* `$xwhen`: 用户自定义时间戳\(带毫秒的13位时间戳\)

示例：用户初始填写的兴趣爱好为\["户外活动"，"足球赛事"，"游戏"\]，调用该接口追加\["学习"，"健身"\]，则用户的爱好变为\["户外活动"，"足球赛事"，"游戏"，"学习"，"健身"\]

```php
$registerId = 'ABCDEF123456789';
$isLogin = true;
$platform = 'JS';
$properties = array(
    'interest'=>array(
        '户外活动',
        '足球赛事',
        '游戏'
    )
);

function msectime()
{
    list($msec, $sec) = explode(' ', microtime());
    return (float) sprintf('%.0f', (floatval($msec) + floatval($sec)) * 1000);
}
$xwhen = msectime();

$analysys_agent->profileAppend($registerId, $isLogin, $properties, $platform, $xwhen);
```

#### 4.1.4 删除设置的属性值

删除设置的属性值。接口如下：

```php
$analysys_agent->profileUnSet($registerId, $isLogin, $propertie, $platform, $xwhen);
$analysys_agent->profileDelete($registerId, $isLogin, $platform, $xwhen);
```

* `$registerId`: 用户ID,长度大于0且小于255字符
* `$isLogin`: 用户ID是否是登录 ID
* `$propertie`: 事件属性
* `$platform`: 平台类型,内容范围：JS、WeChat、Android、iOS
* `$xwhen`: 用户自定义时间戳\(带毫秒的13位时间戳\)

示例1： 要删除已经设置的用户昵称这一用户属性的值

```php
$registerId = 'ABCDEF123456789';
$isLogin = true;
$platform = 'JS';
$propertie = 'nickName';
function msectime()
{
    list($msec, $sec) = explode(' ', microtime());
    return (float) sprintf('%.0f', (floatval($msec) + floatval($sec)) * 1000);
}
$xwhen = msectime();
$analysys_agent->profileUnSet($registerId, $isLogin, $propertie, $platform, $xwhen);
```

示例2：要删除已经设置的所有用户属性

```php
$registerId = 'ABCDEF123456789';
$isLogin = true;
$platform = 'JS';

function msectime()
{
    list($msec, $sec) = explode(' ', microtime());
    return (float) sprintf('%.0f', (floatval($msec) + floatval($sec)) * 1000);
}
$xwhen = msectime();

$analysys_agent->profileDelete($registerId, $isLogin, $platform, $xwhen);
```

### 4.2 通用属性

如果某个事件的属性，在所有事件中都会出现，则这个属性可以做为通用属性，通过 registerSuperProperties\(\) 将该属性设置为事件通用属性，则设置后每次发送事件的时候都会带有该属性。比如用户浏览/购买商品过程中的用户等级就可以作为通用属性。

> 通用属性是一个标准的 K-V 结构，K 和 V 均有相应的约束条件，如不符合则丢弃该次操作。

约束条件如下:

* **属性名称**

  以字母或 `$` 开头，可包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件属性,最大长度99字符,不支持乱码和中文

* **属性值**
  * 支持部分类型：string/number/boolean/集合/数组;
  * 若为字符串,则最大长度 255字符;
  * 若为数组或集合,则最多包含 100条,且 key 约束条件与属性名称一致,value 最大长度 255字符

#### 4.2.1 注册通用属性

以用户浏览/购买商品的过程中发生的事件为例，用户的级别/积分就可以作为通用的属性，通过把用户级别/积分注册为通用属性，就可以避免在每次收集事件属性的时候都要手工设置该属性值。接口如下：

```php
$analysys_agent->registerSuperProperties($properties);
```

* `$properties`：设置多个属性

示例：

```php
// 设置多个通用属性
$properties = array(
    'userLevel'=>0,
    'userPoint'=>0
);
$analysys_agent->registerSuperProperties($properties);
```

#### 4.2.2 删除通用属性

如果要删除某个通用属性或者删除全部的通用属性，可以分别调用 unregisterSuperProperty 或 clearSuperProperties 接口。具体接口定义如下：

```php
$analysys_agent->unRegisterSuperProperty($key);
$analysys_agent->clearSuperProperties();
```

* key：属性名称

示例1：删除设置的用户积分属性

```php
// 删除单个通用属性
$analysys_agent->unRegisterSuperProperty('userPoint');
```

示例2：清除所有已经设置的通用属性

```php
// 清除所有通用属性
$analysys_agent->clearSuperProperties();
```

#### 4.2.3 获取通用属性

由属性名称查询获取单条通用属性，或者获取全部的通用属性。接口如下：

```php
$analysys_agent->getSuperProperty($key);
$analysys_agent->getSuperProperties();
```

* key：属性名称

示例1：查看已经设置的 userLevel 通用属性

```php
// 获取单个通用属性
$analysys_agent->getSuperProperty('userLevel');
```

示例2：查看所有已经设置的通用属性

```php
// 获取所有通用属性
$analysys_agent->getSuperProperties();
```

### 4.3  刷新缓存

立即发送所有收集的信息到服务器。

```php
$analysys_agent.flush();
```

## 5. SDK 使用样例

```php
<?php 
require_once 'AnalysysAgent_PHP_SDK.php';

$appid = '9421608fd544a65e';
$server = 'https://arksdk.analysys.cn:4089/';
$consumer = new BatchConsumer($server); //同步
//$consumer = new SyncConsumer($server); // 批量
$ans = new AnalysysAgent($consumer, $appid);
$ans->setDebugMode(2);


$distinctId = '1234567890987654321';
$isLogin = true;
$eventName  = 'eventName';
$platform = 'JS';
$bookList = array(
    'is AnalysysAgent PHP SDK'
);
function msectime()
{
    list($msec, $sec) = explode(' ', microtime());
    return (float) sprintf('%.0f', (floatval($msec) + floatval($sec)) * 1000);
}
$xwhen = msectime();
$track_properties = array(
    '$ip'=>'112.112.112.112',
    'productType'=>'PHP书籍',
    'productName'=>'bookList',
    'producePrice'=>'60',
    'shop'=>'在线'
);

$ans->track($distinctId, $isLogin, $eventName, $track_properties, $platform, $xwhen);


$registerId  = 'ABCDEF123456789';
$ans->alias($registerId, $distinctId, $platform, $xwhen);

$fileSet_properties = array(
    '$city'=>'北京',
    '$province'=>'北京',
    'nickName'=>'昵称123',
    'userLevel'=>0,
    'userPoint'=>0,
    'interest'=>array(
        '户外活动',
        '足球赛事',
        '游戏'
    )
);
$ans->profileSet($registerId,$isLogin, $fileSet_properties, $platform, $xwhen);

$fileSetOnce_properties = array(
    'registerTime'=>'20180101101010'
);
$ans->profileSetOnce($registerId, $isLogin, $fileSetOnce_properties, $platform, $xwhen);


$fileIncrement_properties = array(
    'userPoint'=>20
);
$ans->profileIncrement($registerId, $isLogin, $fileIncrement_properties, $platform, $xwhen);


$fileAppend_properties = array(
    'interest'=>array(
        '户外活动',
        '足球赛事',
        '游戏'
    )
);
$ans->profileAppend($registerId, $isLogin, $fileAppend_properties, $platform, $xwhen);


$ans->profileUnSet($registerId, $isLogin, "nickName", $platform, $xwhen);

$ans->profileDelete($registerId, $isLogin, $platform, $xwhen);

$registerSuperProperties_properties = array(
    'userLevel'=>0,
    'userPoint'=>0
);
$ans->registerSuperProperties($registerSuperProperties_properties);


$ans->unRegisterSuperProperty('userPoint');


$ans->clearSuperProperties();


$getSuperProperty = $ans->getSuperProperty('userLevel');
printf('getSuperProperty--->%s',$getSuperProperty);


$properties = $ans->getSuperProperties();
printf('getSuperProperties---->');
print_r($properties);
$ans->flush() //批量
 ?>
```

