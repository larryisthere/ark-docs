# Python SDK

Python SDK 主要用于服务端 Python 应用，如 Python Web 应用的后台服务。集成前请先登录[Github下载源码](https://github.com/analysys/argo-sdk-python)

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

* 数据接收地址：`http://host:port/up` 或 `https://host:port/up`

### 2.2 初始化接口

在程序启动时，调用AnalysysPythonSdk\(Collecter\) 初始化 Python SDK 实例。如下：

```python
from analysyspythonsdk import *

#数据接收地址
server_url = "https://host:port/up"
#数据分析创建的项目appkey 
appkey = "analysys"
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

至此 SDK 初始化完成，调用 SDK 提供的接口已可正常采集用户数据了。 说明：程序结束前需要调用 `flush()` 接口，该接口可以把本地缓存的尚未发送至数据接收服务器的数据发送到数据接收服务器。

## 3. 基础接口介绍

### 3.1 appkey设置

appkey 用于唯一区分当前应用所传数据的ID，接口如下:

```python
eguan.setAppId("analysys")
```

### 3.2 Debug 模式

Debug 主要用于开发者测试，接口如下：

```python
eguan.setDebugMode(2)
```

* debug：debug 模式,默认关闭状态。有以下三种值：

  * 0：表示关闭 Debug 模式
  * 1：表示打开 Debug 模式，但该模式下发送的数据仅用于查看数据是否正常回传，不计入平台数据统计
  * 2：表示打开 Debug 模式，该模式下发送的数据计入平台数据统计

  注意：发布版本时debug模式设置为0。

### 3.2 统计事件

事件跟踪，设置事件名称和事件详细信息。接口如下：

```python
def track(self,distinct_id,event_name,event_properties,data_platform,is_login=False):
```

* distinct\_id：用户 ID,长度大于 0 且小于 255字符
* event\_name：事件名称,以字母或 `$` 开头，可包含字母、数字、下划线和 `$`，字母不区分大小写，`$`开头为预置事件,不支持乱码和中文,最大长度 99字符
* event\_properties: 事件属性,最多包含 100条,且 key 以字母或 `$` 开头，可包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件属性,最大长度 125字符,不支持乱码和中文，value 类型约束\(String/Number/boolean/List\)，若为字符串,最大长度255字符
* data\_platform：平台类型,内容范围：JS、WeChat、Android、iOS
* is\_login:用户 ID 是否为登录 ID

示例：匿名用户浏览商品

```python
from analysyspythonsdk  import *
server_url = "https://sdk.analysys.cn:4089/up"
python_sdk_platform = "android"
appkey = "Eguanfangzhou"
collector = DefaultCollecter(server_url)
eguan = AnalysysPythonSdk(collector)
#注意：此方法为必须调用方法。设置数据分析项目appkey，用户请填写在数据端新建的项目名，例如：
eguan.setAppId(appkey)
eguan.setDebugMode(2)
#记录用户登录事件
distinct_id = "ABCDE"
properties = {
            #浏览商品用户的外网IP
            "ip" : "192.168.192.168",
            #浏览商品的时间
            "view_time" : time.strftime("%Y-%m-%d %H:%M:%S",time.localtime()),
            #浏览商品的ID
            "productId" : "Apple_666",
            #浏览商品的类别
            "productCatalog" : "electronic",
            #浏览商品的名字
            "productName" : "iPhone"
        }

#浏览商品跟踪
eguan.track(distinct_id,"view_product",properties,python_sdk_platform,False)
```

### 3.3 用户关联

用户 ID 关联接口。将 alias\_id 和 distinct\_id 关联，计算时会认为是一个用户的行为。该接口是在 distinct\_id 发生变化的时候调用，来告诉 SDK distinct\_id 变化前后的 ID 对应关系。该场景一般应用在用户注册/登录的过程中。比如：一个匿名用户浏览商品，系统为其分配的distinct\_id = "1234567890987654321"，随后该匿名用户进行注册，系统为其分配了新的注册 ID，alias\_id = "ABCDEF123456789"，此时就需要调用 alias 接口对两个 ID 进行关联。接口如下：

```python
def alias(self,alias_id,distinct_id,data_platform,alias_properties=None):
```

* alias\_id：用户注册 ID，长度大于 0，且小于 255字符
* distinct\_id：用户匿名ID，长度大于 0，且小于 255字符
* data\_platform：平台类型,内容范围：JS、WeChat、Android、iOS
* alias\_properties:关联属性，默认为空。

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

  以字母或`$`开头，可包含字母、数字、下划线和`$`，字母不区分大小写，`$`开头为预置事件属性,最大长度125字符,不支持乱码和中文

* **属性值**

  支持部分类型：`string`/`number`/`boolean`/`List`; 若为字符串,则最大长度255字符; 若为List,则最多包含100条,且key约束条件与属性名称一致,value最大长度255字符

设置单个或多个属性，如用户所在城市，用户昵称，用户头像信息等。如果之前存在，则覆盖，否则，新创建。接口如下：

```python
def profile_set(self,distinct_id,profile_properties,data_platform,is_login=False):
```

* distinct\_id: 用户ID,长度大于0且小于255字符
* profile\_properties: 用户属性
* data\_platform: 平台类型,内容范围：JS、WeChat、Android、iOS
* is\_login: 用户ID是否是登录 ID

示例：用户注册后设置用户的注册信息属性

```python
python_sdk_platform = "android"
distinct_id = "xiaomei"
properties = {
            "name": "梅西",
            "sex": "male",
            "age": 31,
            "phonenumber": "13912911291",
            "$email" : "XXX@qq.com"
        }
eguan.profile_set(distinct_id,properties,python_sdk_platform,True)
```

## 4. 更多接口

### 4.1 用户属性

#### 4.1.1 设置用户固有属性

只在首次设置时有效的属性。如：用户的注册时间。如果被设置的用户属性已存在，则这条记录会被忽略而不会覆盖已有数据，如果属性不存在则会自动创建。接口如下：

```python
def profile_set_once(self,distinct_id,profile_properties,data_platform,is_login=False):
```

* distinct\_id: 用户ID,长度大于0且小于255字符
* profile\_properties: 用户属性
* data\_platform: 平台类型,内容范围：JS、WeChat、Android、iOS
* is\_login: 用户ID是否是登录 ID

  示例：要统计用户注册时间

```python
python_sdk_platform = "android"
distinct_id = "DDDDD"
properties = {
           "sex" : "female",
           "registerTime" : "1534249333700"
        }
eguan.profile_set_once(distinct_id,properties,python_sdk_platform)
```

#### 4.1.2 设置用户属性相对变化值

设置用户属性的单个相对变化值\(相对增加,减少\)，只能对数值型属性进行操作，如果这个Profile之前不存在,则初始值为0。接口如下：

```python
def profile_increment(self,distinct_id,profile_properties,data_platform,is_login=False):
```

* distinct\_id: 用户ID,长度大于0且小于255字符
* profile\_properties: 用户属性
* data\_platform: 平台类型,内容范围：JS、WeChat、Android、iOS
* is\_login: 用户ID是否是登录 ID

示例：用户注册初始积分为0，在用户购买商品后，用户的积分增加20，则调用该接口，用户的积分变为0+20=20了：

```python
python_sdk_platform = "android"
distinct_id = "EEEEE"
properties = {
        "userPoint",20
    }
eguan.profile_increment(distinct_id,properties,python_sdk_platform)
```

#### 4.1.3 增加列表类型的属性

为列表类型的属性增加一个或多个元素，如：用户新增兴趣爱好，接口如下：

```python
def profile_append(self,distinct_id,profile_properties,data_platform,is_login=False):
```

* distinct\_id: 用户ID,长度大于0且小于255字符
* profile\_properties: 用户属性
* data\_platform: 平台类型,内容范围：JS、WeChat、Android、iOS
* is\_login: 用户ID是否是登录 ID

示例：用户初始填写的兴趣爱好为\["户外活动"，"足球赛事"，"游戏"\]，调用该接口追加\["学习"，"健身"\]，则用户的爱好变为\["户外活动"，"足球赛事"，"游戏"，"学习"，"健身"\]

```python
distinct_id = "FFFFF"
properties = {
            "interest" : ["户外活动","足球赛事","游戏"]
        }
eguan.profile_append(distinct_id,properties,python_sdk_platform,True)
```

#### 4.1.4 删除设置的属性

删除设置的属性值。接口如下：

```python
def profile_unset(self,distinct_id,profile_properties_keys,data_platform,is_login=False):
def profile_delete(self,distinct_id,data_platform,is_login=False):
```

* distinct\_id: 用户ID,长度大于0且小于255字符
* profile\_properties\_keys: 需删除的用户属性key，list类型
* data\_platform: 平台类型,内容范围：JS、WeChat、Android、iOS
* is\_login: 用户ID是否是登录 ID

示例：

```python
#  删除当前用户单个属性值
python_sdk_platform = "android"
distinct_id = "HHHHH"
eguan.profile_unset(distinct_id,["hobby"],python_sdk_platform,True)
# 删除当前用户所有属性值
eguan.profile_delete(distinct_id,python_sdk_platform,True)
```

### 4.2 通用属性

如果某个事件的属性，在所有事件中都会出现，则这个属性可以做为通用属性，通过 registerSuperProperties\(\) 将该属性设置为事件通用属性，则设置后每次发送事件的时候都会带有该属性。比如用户浏览/购买商品过程中的用户等级就可以作为通用属性。

> 通用属性是一个标准的 K-V 结构，K 和 V 均有相应的约束条件，如不符合则丢弃该次操作。

约束条件如下:

* **属性名称**

  以字母或 `$` 开头，可包含字母、数字、下划线和 `$`，字母不区分大小写，`$` 开头为预置事件属性,最大长度 125字符,不支持乱码和中文

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
from analysyspythonsdk  import *

# 场景一：实时发送方式，初始化python SDK，记录用户登录
server_url = "https://sdk.analysys.cn:4089/up"
python_sdk_platform = "android"
class Demo():
    def testDefaultCollecter(self):
        collector = DefaultCollecter(server_url)
        eguan = AnalysysPythonSdk(collector)
        eguan.setAppId("ssss")
        eguan.setDebugMode(2)
        distinct_id = "aaaaa"
        eguan.track(distinct_id,"u7777777",None,python_sdk_platform,is_login=True,)
        eguan.close()

        #场景二：某电商追踪用户浏览商品和下订单等事件
        superProperties = {
            "member" : "VIP",
            "age" : 20

        }
        eguan.registerSuperProperties(superProperties)
        eguan.getAllSuperProperties()
        eguan.getSingleSuperProperties("member")
        distinct_id = "ABCDE112345"
        properties = {
            "ip" : "169.169.169.169",
            "view_time" : time.strftime("%Y-%m-%d %H:%M:%S",time.localtime()),
            "productId" : "666",
            "productCatalog" : "electronic"
        }
        eguan.track(distinct_id,"view_product",properties,python_sdk_platform,False)
        eguan.unregisterSuperProperties("age")
        eguan.getAllSuperProperties()

        properties = {
            "productList" : ["iphone X","honor P8","computer"],
            "productPrice" : 23456,
            "isPay" : True
        }

        eguan.track(distinct_id,"payment",properties,python_sdk_platform,True)
        eguan.close()

        #场景三：用户识别

        alias_id = "12345"
        distinct_id = "xiaoming"
        eguan.alias(alias_id,distinct_id,python_sdk_platform)

        #场景四：设置用户属性
        distinct_id = "CCCCCCCCCCCCC"
        properties = {
            "name": "梅西",
            "sex": "male",
            "age": 31,
            "login_time": time.strftime("%Y-%m-%d %H:%M:%S",time.localtime()),
            "$email" : "XXX@qq.com"
        }
        eguan.profile_set(distinct_id,properties,python_sdk_platform,True)

        #场景五：设置用户只在首次设置时有效的属性
        distinct_id = "DDDDDDDDDDDDD"
        properties = {
            "sex" : "female",
            "activationTime" : "1534249333700"
        }
        eguan.profile_set_once(distinct_id,properties,python_sdk_platform)

        #场景六：设置用户数值型属性
        distinct_id = "EEEEEEEEEEEEE"
        eguan.profile_increment(distinct_id,{"gameAge":2},python_sdk_platform)

        #场景七：设置用户列表类型属性
        distinct_id = "FFFFFFFFFFFF"
        properties = {
            "hobby" : ["basketball","music","read"],
            "movies" : ["大话西游","一出好戏"]
        }
        eguan.profile_append(distinct_id,properties,python_sdk_platform,True)

        #场景八：删除用户属性
        distinct_id = "HHHHHHHHHHH"
        eguan.profile_unset(distinct_id,["hobb"],python_sdk_platform,True)

        #场景九：清除所有用户属性
        distinct_id = "GGGGGGGGGG"
        eguan.profile_delete(distinct_id,python_sdk_platform,True)

if __name__ == '__main__':
    EG = Demo()
    EG.testDefaultCollecter()
```

