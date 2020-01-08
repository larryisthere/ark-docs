# Python SDK

Python SDK 主要用于服务端 Python 应用，如 Python Web 应用的后台服务。集成前请先登录下载SDK

{% hint style="info" %}
[Releases包下载及更新说明](https://github.com/analysys/ans-python-sdk/releases)
{% endhint %}

## 1. 集成准备

### 1.1 安装Python SDK模块

导入方式:下载源码,进入模块目录，通过以下命令安装模块：

```text
python setup.py install
```

### 1.2 导入 SDK

```text
from analysyspythonsdk import *
```

## 2. SDK 初始化

### 2.1 获取配置信息

需获取的配置信息为：

* 数据接收地址：`http://host:port` 或 `https://host:port`

### 2.2 初始化接口

在程序启动时，调用AnalysysPythonSdk\(Collecter\) 初始化 Python SDK 实例。如下：

```python
from analysyspythonsdk import *

#数据接收地址
server_url = "https://host:port"
#数据分析创建的项目appkey 
appkey = "fangzhouargo"
#使用实时收集器
collector = DefaultCollecter(server_url)
#初始化python SDK实例
eguan = AnalysysPythonSdk(collector)
#设置数据分析项目所用appkey
eguan.setAppId(appkey)
```

appkey：数据分析创建的项目appkey DefaultCollecter：为实时事件收集器 事件收集器还提供批量收集器，用户可以指定为实时收集和批量收集。

#### 2.2.1 实时收集器

用户每触发一次上传，该收集器则立即上传数据至接收服务器：

```python
collector = DefaultCollecter(server_url)
```

* server\_url：数据接收地址

#### 2.2.2 批量收集器

用户触发上传，该收集器将先缓存数据，直到数量达到用户设置的阈值或者用户设置的等待时间，才会触发真正的上传：

```python
collector = BatchCollecter(server_url,max_interval_time=15,queue_cache_max_size=10,collecter_max_size=100,queue_max_size=1000,request_timeout=None)
```

* server\_url：数据接收地址
* max\_interval\_time:前后发送的间隔时间
* queue\_cache\_max\_size：队列缓存最大值，超过即发送
* collecter\_max\_size：收集器单次请求最大发送值
* queue\_max\_size：整个队列缓存最大值
* request\_timeout：请求超时时间

#### 2.2.3 存储本地文件收集器

该收集器将用户调用接口的数据写入到本地文件，支持单条写入或批量写入：

```python
collector = LogCollecter(log_path,is_batch=True,batch_num=10,general_rule="hour")
```

* log\_path: 数据落文件目录
* is\_batch: 是否批量写入，默认为False
* batch\_num: 批量写入时，每次写入条数。默认为20条
* general\_rule: 文件存储规则。当传值为"day"时，文件按日写入数据（即每日生成一个文件），当传值为"hour"时，文件按小时写入数据（即每小时生成一个文件）。默认规则为按小时

至此 SDK 初始化完成，调用 SDK 提供的接口已可正常采集用户数据了。

说明：程序结束前需要调用 `flush()` 接口，该接口可以把本地缓存的尚未发送至数据接收服务器的数据发送到数据接收服务器。

## 3. 基础接口介绍

### 3.1 appkey设置

appkey 用于唯一区分当前应用所传数据的ID，接口如下:

```python
eguan.setAppId("fangzhouargo")
```

### 3.2 Debug 模式

Debug 主要用于开发者测试，接口如下：

```python
eguan.setDebugMode(1)
```

* debug：debug 模式,默认关闭状态。有以下三种值：

  * 0：表示关闭 Debug 模式
  * 1：表示打开 Debug 模式，但该模式下发送的数据仅用于查看数据是否正常回传，不计入平台数据统计
  * 2：表示打开 Debug 模式，该模式下发送的数据计入平台数据统计

  注意：发布版本时debug模式设置为0。

### 3.2 统计事件

事件跟踪，设置事件名称和事件详细信息。接口如下：

```python
def track(self,distinct_id,event_name,event_properties,data_platform="python",is_login=False,xwhen=_current_time()):
```

* distinct\_id：用户 ID,长度大于 0 且小于 255字符
* event\_name：事件ID,以字母或 `$` 开头，可包含字母、数字、下划线和 `$`，字母不区分大小写，`$`开头为预置事件,不支持乱码和中文,最大长度 99字符
* event\_properties: 事件属性,最多包含 100条,且 key 以字母或 `$` 开头，可包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件属性,最大长度99字符,不支持乱码和中文，value 类型约束\(String/Number/boolean/List\)，若为字符串,最大长度255字符
* data\_platform：平台类型,内容范围：JS、WeChat、Android、iOS
* is\_login:用户 ID 是否为登录 ID
* xwhen:用户自定义数据产生时间\(单位:毫秒,即13位时间戳\) 注:xwhen自定义用于用户上传历史数据

示例：匿名用户浏览商品

```python
from analysyspythonsdk  import *
server_url = "https://sdk.analysys.cn:4089/up"
python_sdk_platform = "android"
appkey = "fangzhouargo"
collector = DefaultCollecter(server_url)
eguan = AnalysysPythonSdk(collector)
#注意：此方法为必须调用方法。设置数据分析项目appkey，用户请填写在数据端新建的项目名，例如：
eguan.setAppId(appkey)
eguan.setDebugMode(1)
#记录用户登录事件
distinct_id = "ABCD123456"
commodity_properties = {
            "commodity_name" : "跑步机",
            "commodity_brand" : "zhonghua",
            "commodity_price" : 5000,
            "commodity_quality" : True,
            "commodity_function" : ["run","记录心率"]
        }
eguan.track(distinct_id,"SearchCommodity",commodity_properties,python_sdk_platform,is_login=False)

```

### 3.3 用户关联

用户 ID 关联接口。将 用户ID 和 匿名ID 关联，计算时会认为是一个用户的行为。该接口是在 匿名ID 发生变化的时候调用，来告诉 SDK 匿名ID 变化前后的 ID 对应关系。该场景一般应用在用户注册/登录的过程中。比如：一个匿名用户浏览商品，系统为其分配的匿名ID是"1234567890987654321"，随后该匿名用户进行注册，系统为其分配了新的注册 ID，用户ID "ABCDEF123456789"，此时就需要调用 alias 接口对两个 ID 进行关联。接口如下：

```python
def alias(self,alias_id,distinct_id,data_platform="python",alias_properties=None,is_login=True,xwhen=_current_time()):
```

* alias\_id：用户注册 ID，长度大于 0，且小于 255字符
* distinct\_id：用户匿名ID，长度大于 0，且小于 255字符
* data\_platform：平台类型,内容范围：JS、WeChat、Android、iOS
* alias\_properties:关联属性，默认为空。
* xwhen:用户自定义数据产生时间\(单位:毫秒,即13位时间戳\) 注:xwhen自定义用于用户上传历史数据

示例：匿名用户浏览商品到注册会员

```python
python_sdk_platform = "android"
#匿名ID
distinct_id = "12345"
#登录ID
alias_id = "xiaoming"
#调用alias方法，将用户登录前和登录后的ID进行关联
eguan.alias(alias_id,distinct_id,python_sdk_platform)
```

### 3.4 用户属性设置

SDK提供以下接口供用户设置用户的属性，比如用户的年龄/性别等信息。

> 用户属性是一个标准的K-V结构，K和V均有相应的约束条件，如不符合则丢弃该次操作。

参数约束:

* **属性名称**

  以字母或`$`开头，可包含字母、数字、下划线和`$`，字母不区分大小写，`$`开头为预置事件属性,最大长度99字符,不支持乱码和中文

* **属性值**

  支持部分类型：`string`/`number`/`boolean`/`List`; 若为字符串,则最大长度255字符; 若为List,则最多包含100条,且key约束条件与属性名称一致,value最大长度255字符

设置单个或多个属性，如用户所在城市，用户昵称，用户头像信息等。如果之前存在，则覆盖，否则，新创建。接口如下：

```python
def profile_set(self,distinct_id,profile_properties,data_platform="python",is_login=False,xwhen=_current_time()):
```

* distinct\_id: 用户ID,长度大于0且小于255字符
* profile\_properties: 用户属性
* data\_platform: 平台类型,内容范围：JS、WeChat、Android、iOS
* is\_login: 用户ID是否是登录 ID
* xwhen:用户自定义数据产生时间\(单位:毫秒,即13位时间戳\) 注:xwhen自定义用于用户上传历史数据

示例：用户注册后设置用户的注册信息属性

```python
python_sdk_platform = "android"
alias_id = "zhangsan"
profile_properties = {
            "sex" : "男",
            "phonenumber" : "13966667777",
            "email" : "xxx@163.com",
            "hobby" : ["basketball","听音乐","mountain-climbing"],
            "is_female" : False
        }

eguan.profile_set(alias_id,profile_properties,python_sdk_platform,is_login=True)
```

## 4. 更多接口

### 4.1 用户属性

#### 4.1.1 设置用户固有属性

只在首次设置时有效的属性。如：用户的注册时间。如果被设置的用户属性已存在，则这条记录会被忽略而不会覆盖已有数据，如果属性不存在则会自动创建。接口如下：

```python
def profile_set_once(self,distinct_id,profile_properties,data_platform="python",is_login=False,xwhen=_current_time()):
```

* distinct\_id: 用户ID,长度大于0且小于255字符
* profile\_properties: 用户属性
* data\_platform: 平台类型,内容范围：JS、WeChat、Android、iOS
* is\_login: 用户ID是否是登录 ID
* xwhen:用户自定义数据产生时间\(单位:毫秒,即13位时间戳\) 注:xwhen自定义用于用户上传历史数据 示例：要统计用户注册时间

  示例：要统计用户注册时间

```python
python_sdk_platform = "android"
alias_id = "zhangsan"
properties = {
           "vipLevel" : 0,
           "registerTime" : "1534249333700"
        }
eguan.profile_set_once(alias_id,properties,python_sdk_platform,is_login=True)
```

#### 4.1.2 设置用户属性相对变化值

设置用户属性的单个相对变化值\(相对增加,减少\)，只能对数值型属性进行操作，如果这个Profile之前不存在,则初始值为0。接口如下：

```python
def profile_increment(self,distinct_id,profile_properties,data_platform="python",is_login=False,xwhen=_current_time()):
```

* distinct\_id: 用户ID,长度大于0且小于255字符
* profile\_properties: 用户属性
* data\_platform: 平台类型,内容范围：JS、WeChat、Android、iOS
* is\_login: 用户ID是否是登录 ID
* xwhen:用户自定义数据产生时间\(单位:毫秒,即13位时间戳\) 注:xwhen自定义用于用户上传历史数据

示例：用户注册初始积分为0，在用户购买商品后，用户的积分增加20，则调用该接口，用户的积分变为0+20=20了：

```python
python_sdk_platform = "android"
alias_id = "zhangsan"
properties = {
        "userPoint":20
    }    
eguan.profile_increment(alias_id,properties,python_sdk_platform,is_login=True)
```

#### 4.1.3 增加列表类型的属性

为列表类型的属性增加一个或多个元素，如：用户新增兴趣爱好，接口如下：

```python
def profile_append(self,distinct_id,profile_properties,data_platform="python",is_login=False,xwhen=_current_time()):
```

* distinct\_id: 用户ID,长度大于0且小于255字符
* profile\_properties: 用户属性
* data\_platform: 平台类型,内容范围：JS、WeChat、Android、iOS
* is\_login: 用户ID是否是登录 ID
* xwhen:用户自定义数据产生时间\(单位:毫秒,即13位时间戳\) 注:xwhen自定义用于用户上传历史数据

示例：用户初始填写的兴趣爱好为\["户外活动"，"足球赛事"，"游戏"\]，调用该接口追加\["学习"，"健身"\]，则用户的爱好变为\["户外活动"，"足球赛事"，"游戏"，"学习"，"健身"\]

```python
alias_id = "zhangsan"
properties = {
            "interest" : ["户外活动","足球赛事","游戏"]
        }
eguan.profile_append(alias_id,properties,python_sdk_platform,is_login=True)
```

#### 4.1.4 删除设置的属性

删除设置的属性值。接口如下：

```python
def profile_unset(self,distinct_id,profile_properties_keys,data_platform="python",is_login=False,xwhen=_current_time()):
def profile_delete(self,distinct_id,data_platform="python",is_login=False,xwhen=_current_time()):
```

* distinct\_id: 用户ID,长度大于0且小于255字符
* profile\_properties\_keys: 需删除的用户属性key，list类型
* data\_platform: 平台类型,内容范围：JS、WeChat、Android、iOS
* is\_login: 用户ID是否是登录 ID
* xwhen:用户自定义数据产生时间\(单位:毫秒,即13位时间戳\) 注:xwhen自定义用于用户上传历史数据

示例：

```python
# 要删除已经设置的用户爱好这一用户属性
python_sdk_platform = "android"
alias_id = "zhangsan"
eguan.profile_unset(alias_id,["hobby"],python_sdk_platform,is_login=True)
# 要清除已经设置的所有用户属性
eguan.profile_delete(alias_id,python_sdk_platform,is_login=True)
```

### 4.2 通用属性

如果某个事件的属性，在所有事件中都会出现，则这个属性可以做为通用属性，通过 registerSuperProperties\(\) 将该属性设置为事件通用属性，则设置后每次发送事件的时候都会带有该属性。比如用户浏览/购买商品过程中的用户等级就可以作为通用属性。

> 通用属性是一个标准的 K-V 结构，K 和 V 均有相应的约束条件，如不符合则丢弃该次操作。

约束条件如下:

* **属性名称**

  以字母或 `$` 开头，可包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件属性,最大长度99字符,不支持乱码和中文

* **属性值**
  * 支持部分类型：string/number/boolean/List;
  * 若为字符串,则最大长度 255字符;
  * 若为List,则最多包含 100条,且 key 约束条件与属性名称一致,value 最大长度 255字符

#### 4.2.1 注册通用属性

以用户浏览/购买商品的过程中发生的事件为例，用户的级别/积分就可以作为通用的属性，通过把用户级别/积分注册为通用属性，就可以避免在每次收集事件属性的时候都要手工设置该属性值。接口如下：

```python
def registerSuperProperties(self,super_properties):
```

* super\_properties：设置多个属性

示例：

```python
#设置事件通用属性
superProperties = {
            "member" : "VIP",
            "userPoint" : 10
        }
eguan.registerSuperProperties(superProperties)
```

#### 4.2.2 删除通用属性

如果要删除某个通用属性或者删除全部的通用属性，可以分别调用 unregisterSuperProperty 或 clearSuperProperties 接口。具体接口定义如下：

```python
def unregisterSuperProperties(self,super_properties_key):
def clearSuperProperties(self):
```

* super\_properties\_key：属性名称

示例：

```python
# 删除设置的用户积分属性
eguan.unregisterSuperProperties("member")
# 清除所有已经设置的通用属性
eguan.clearSuperProperties()
```

#### 4.2.3 获取通用属性

由属性名称查询获取单条通用属性，或者获取全部的通用属性。接口如下：

```python
def getSingleSuperProperties(self,super_properties_key):
def getAllSuperProperties(self):
```

* super\_properties\_key：属性名称

示例：

```python
# 查看已经设置的 member 通用属性
eguan.getSingleSuperProperties("member")
# 查看所有已经设置的通用属性
eguan.getAllSuperProperties()
```

## 5. SDK 使用样例

```python
# _*_coding:utf-8_*_
from __future__ import unicode_literals

from argoagent  import *
'''
server_url为数据上传的地址

python_sdk_platform为数据来源平台。如:服务端收到从Android平台传来的数据后，若使用python SDK采集数据,此时platform字段填写Android,便于数据归类统计分析。
建议传入字段如：iOS/Android/JS/WeChat.（不区分大小写）。限传字符串类型。demo中假定该后台数据对应移动端Android数据。

python SDK使用场景示例如下：
'''
server_url = "https://sdk.analysys.cn:4089"
python_sdk_platform = "js"
appkey = "fangzhouargo"
class Demo():
    def pythonsdkdemo(self):

        # 默认方式发送数据，数据逐条发送
        collector = DefaultCollecter(server_url)
        # eguan = AnalysysPythonSdk(collector)
        # 批量发送数据，数据达到一定条数或达到相应时间进行批量发送
        # collector = BatchCollecter(server_url, 10, 2, 100, 1000, request_timeout=None)
        eguan = AnalysysPythonSdk(collector)

        # 注意：此方法为必须调用方法。设置项目名称，用户请填写在数据端新建的项目appkey，例如：
        eguan.setAppId(appkey)

        #设备debug模式，允许设置0,1,2.默认为0,不打印日志，数据进行统计分析。设置为1时打印日志，上传的数据不进行统计分析。设置为2时打印日志，上传的数据进行统计分析。例如：
        eguan.setDebugMode(1)

        #设置匿名ID，此ID为用户未登陆状态下使用的数据上传用户ID。此ID可以由用户自己定义，如果移动端接入Android SDK，此ID也可以从Android SDK端获取传到后台。
        distinct_id = "ABCD123456"
        '''
        假定用户场景： 用户先将未登陆状态下的数据传到方舟后台进行统计。当发生登陆行为后，以登录的用户ID向方舟后台发送数据。
        说明：
        1、未登录状态下发送的数据，is_login参数必须设置为False
        2、在集成SDK时，在登录位置需调用alias接口。在数据逻辑上形成登录状态，之后在方舟上可以看到该登录ID用户与之前匿名ID用户的数据为同一用户。
        3、登录后发送数据，is_login参数必须设置为True
        4、登录后发送数据，形参distinct ID应使用已登录的ID，而非匿名ID。

        '''
        #记录普通用户浏览商品详情，商品的属性随事件上传（用户未登录）
        commodity_properties = {
            "commodity_name" : "跑步机",
            "commodity_brand" : "zhonghua",
            "commodity_price" : 5000,
            "commodity_quality" : True,
            "commodity_function" : ["run","记录心率"]
        }
        eguan.track(distinct_id,"SearchCommodity",commodity_properties,python_sdk_platform,is_login=False)

        # 记录普通用户点击注册按钮，注册事件未携带属性（用户未登录）
        eguan.track(distinct_id,"register",None,python_sdk_platform,is_login=False)

        #用户进行登录。假定alias_id为用户登录后的ID
        alias_id = "zhangsan"
        eguan.alias(alias_id,distinct_id,python_sdk_platform)

        #用户登录后，需要将该用户的用户属性上传
        profile_properties = {
            "sex" : "男",
            "phonenumber" : "13966667777",
            "email" : "xxx@163.com",
            "hobby" : ["basketball","听音乐","mountain-climbing"],
            "is_female" : False
        }
        eguan.profile_set(alias_id,profile_properties,python_sdk_platform,is_login=True)

        # 用户登录，且上传用户属性后，继续浏览APP，再之后用户进行购买、支付等操作。用户在购买、支付等行为追踪时，想带上该商品的属性。
        # 注册通用属性（当通用属性设置后，之后的事件均携带该通用属性）
        super_properties = {
            "commodity_name" : "减肥药",
            "commodity_brand" : "luomiou",
            "commodity_price" : 1000,
            "commodity_quality" : True,
        }

        eguan.registerSuperProperties(super_properties)

        #获取已存在的通用属性
        # eguan.getAllSuperProperties()
        #获取单个通用属性
        # eguan.getSingleSuperProperties("commodity_name")

        #记录购买button被点击
        purchase_properties = {
            "Referer" : "gouwuche"
        }

        eguan.track(alias_id,"purchase",purchase_properties,python_sdk_platform,is_login=True)

        #记录支付行为
        payment_properties = {
            "pay_usage" : "Wechat"
        }
        eguan.track(alias_id,"payment",payment_properties,python_sdk_platform,is_login=True)
        #批量发送时，将队列中缓存的数据立即上传
        eguan.close()

if __name__ == '__main__':
    EG = Demo()
    EG.pythonsdkdemo()

```

