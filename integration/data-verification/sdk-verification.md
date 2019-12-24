---
description: 客户端开发工程师埋点后很可能希望快速了解埋点是否正确被触发。
---

# 客户端埋点验证

由于易观方舟支持实时分析，我们首推在产品内通过建立专门用于测试的项目来进行验证。这样可以对数据从上报、入库到使用进行全环节验证。又不会对生产环境的上报的数据造成污染。

但很多时候负责埋点的工程师并没有方舟的使用权限，或希望借助方舟而用另一种更熟悉的方法进行埋点验证。

针对这种场景，我们也提供了相应的解决方案。

借助这种方案可以做到：

* 客户端有操作时，验证是否会正确触发上报；
* 查看上报事件的属性（名称、属性名称及类型）是否符合预期；
* 了解到客户端操作的行为序列；

## 网站埋点（JS）

### 调试模式开启时

```javascript
debugMode: 1 或 2
```

当调试模式开启时，SDK 会向浏览器的控制台中输出日志。日志中会包含一些告警、错误，也会包含上报事件的内容。

以 Chrome 为例，步骤如下：

* 启动 Chrome，并访问已经埋好点的网站
* 按 F12 或 Ctl/Cmd + Alt/Opt + I 打开 “开发者工具”
* 点击 “Console” 页签进入控制台
* 正常浏览页面，接可以看到控制台有大量的日志

![ ](https://imguserradar.analysys.cn/fangzhou/img/2019/02/201902271209482455.png)

接下来，为了方便查看事件报文的内容，我们可以在过滤器中设定关键字“analysys”筛选出报文。

```text
SDK初始化相关日志
Send message to server: **实际上报地址**
上报数据相关日志
```

如日志发送成功，控制台会输出：

```text
Send message success
```

### 调试模式未开启时

```javascript
debugMode: 0
```

生产环境通常会关闭调试模式，在调试模式未开启时SDK不会向浏览器的控制台发送任何日志，这对调试造成了一些不利。但通过浏览器自带的开发者工具也查看到上报的事件内容。下面以 Chrome 为例，介绍相应的测试方法。

#### Chrome

![ ](https://imguserradar.analysys.cn/fangzhou/img/2019/02/201902262137150450.png)

步骤如下：

* 启动 Chrome，并访问已经埋好点的网站
* 按 F12 或 Ctl/Cmd + Alt/Opt + I 打开 “开发者工具”
* 如上图，点击“Network”页签
* 正常浏览页面，就能在浏览器中看到上报的埋点日志

![ ](https://imguserradar.analysys.cn/fangzhou/img/2019/02/201902262143438120.png)

* 如上图，在左上方红框位置的过滤器中输入“up?”
* 然后点击每条记录，就能在右侧红框“Request Payload”中看到上报报文的内容了

## APP埋点（iOS/Android）

移动端 SDK 也会输出日志，如果你是开发者，你可以按照下面的说明开启调试模式，通过 SDK 的日志调试。同时我们也提供一种面向非开发者的，通过抓包工具来查看上报日志的方式。

### 如果你是开发者

步骤如下：

* 先在代码中设置调试状态开启

**Andorid**

```java
AnalysysAgent.setDebugMode(this, 2);
```

```text
0：关闭 Debug 模式
1：打开 Debug 模式，但该模式下发送的数据仅用于调试，不计入平台数据统计
2：打开 Debug 模式，该模式下发送的数据可计入平台数据统计
```

**iOS**

```text
AnalysysAgent setDebugMode:AnalysysDebugButTrack
```

```text
AnalysysDebugOff：关闭 Debug 模式
AnalysysDebugOnly：打开 Debug 模式，但该模式下发送的数据仅用于调试，不计入平台数据统计
AnalysysDebugButTrack：打开 Debug 模式，该模式下发送的数据可计入平台数据统计
```

* 使用 Eclipse、AndroidStudio 或 Xcode 工具等，请在 Console 中搜索 tag 为“Analysys”

若初始化成功后，控制台会输出：

```text
SDK初始化相关日志
Send message to server: **实际上报地址**
上报数据相关日志
```

如日志发送成功，控制台会输出：

```text
Send message success
```

### 如果你不是开发者

非开发者往往 App 已经安装在手机上，若想调试需要将 App 的流量发送到流量分析工具中进行调试。市场上比较知名的工具如下：

```text
mitmproxy
https://mitmproxy.org/#mitmweb

Charles
https://www.charlesproxy.com/download/ 

Fiddler
https://www.telerik.com/fiddler
```

步骤如下：

* 从上述流量监控工具中选择适合您的，安装并按提示将您 app 的流量转发到工具里
* 在工具中的过滤器中输入“up?”
* 正常使用 app，就能在工具中看到上报的埋点日志
* 点击每条记录，就能查看上报报文的内容了

如果上述方案仍然不能满足您的需要，可联系我们，我们非常愿意了解您的需要，并尽力提供适合您的解决方案。

