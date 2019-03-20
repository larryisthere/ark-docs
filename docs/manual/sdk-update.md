# SDK更新日志

## Android SDK

更新日期 | 版本号 | 更新内容
---|---|---
2018/08/02 | V4.0.5 |1) 增加了 init(Context context, String key, String channel, String baseUrl,boolean autoProfile)接口;<br>2) push 对应字段类型修改; <br>3) 修改端口号,上数,收数 https 由 443 修改为 4089;<br>4) 修改 webSocket 链接,使用 wss 端口号为 4091。
2018/08/24 | V4.0.6 |1）log 输出日志增加可读性；<br>2）支持 Hybrid 模式；<br>3）优化可视化优化功能；<br>4）优化 reset 方法逻辑。
2018/09/18 | V4.1.0 |1）在初始化配置中新增`encryptType`参数，提供对上传数据可选是否进行加密功能。
2018/11/19 | V4.1.1 |1）在完善日志打印; <br>2) 增加时区字段$time_zone 类型 String; <br>3)appkey 改变 /debug 由 1 变为 0 或 2/uploadURL 改变时,重置存储内容，重新发送 profileSetOnce; <br>4) 数组类型数据仅支持元素为字符串类型的数组;<br>4.1) 增加不允许覆盖字段$first_visit_time; <br>5) 系统版本字段 $os_version 之前增加 系统名称;<br>6) channel设置逻辑修改为，当xml设置与API传值两种方式都设置时优先使用xml设置值;<br>7)key设置逻辑修改为，当xml设置与API传值两种方式设置key不相同时，提示设置异常; <br>8) 功能优化;<br>8.1) 类型判断部分做了优化。
2018/12/17 | V4.1.2 |1)优化`session_id`相关<br>2)在首次启动中增加`$first_visit_language`字段<br>3)增加`$language`字段<br>4)优化SDK性能
2019/03/04 | V4.2.1 |1)相关功能封装为独立模块<br>2)优化SDK性能
2019/03/13 | v4.2.1 |1)优化多线程问题

## iOS SDK

更新日期 | 版本号 | 更新内容
---|---|---
2018/08/02 | V4.0.5 |1) campaign_id 调整为字符串。ACTIONTYPE 默认为数值型;<br>2) 增加调试模式下打印上传数据详情信息;<br>3) 增加首次启动调用 profile_set_once 接口上传启动时间;<br>4) 删除接口：startWithAppKey:channel:uploadURL:;<br>5) 增加接口：startWithAppKey:channel:baseURL:autoProfile:;<br>6） 数据上传及可视化数据配置端口为(默认https)：4089;<br>7) 可视化端口为（默认wss）：4091。
2018/08/24 | V4.0.6 |1）log 输出日志增加可读性；<br>2）支持 Hybrid 模式；<br>3）优化可视化优化功能；<br>4）优化 reset 方法逻辑。
2018/09/18 | V4.1.0 |1）在初始化配置中新增`encryptType`参数，提供对上传数据可选是否进行加密功能。
2018/11/19 | V4.1.1 |1）在完善日志打印; <br>2) 增加时区字段$time_zone 类型 String; <br>3)appkey 改变 /debug 由 1 变为 0 或 2/uploadURL 改变时,重置存储内容，重新发送 profileSetOnce; <br>4) 数组类型数据仅支持元素为字符串类型的数组;增加不允许覆盖字段$first_visit_time; <br>5) 系统版本字段 $os_version 之前增加 系统名称;<br>6) 功能优化;<br>6.1) 修复失败重传策略，间隔不生效bug；<br>6.2) 修复App杀掉后无法保存end事件bug；<br>6.3) 修复App退入后台无法上传bug。
2018/12/17 | V4.1.2 |1)优化`session_id`相关<br>2)在首次启动中增加`$first_visit_language`字段<br>3)增加`$language`字段<br>4)优化SDK性能
2019/02/15 | V4.2.0 |1)相关功能封装为独立模块<br>2)优化SDK性能
## JS SDK

更新日期 | 版本号 | 更新内容
---|---|---
2018/08/02 | V4.0.5 |1) 修改 `$webstay` 日志中,`$url` 参数的值超过长度 255 字符数时，将 `$url` 参数抛弃的 BUG。(20180803);<br>2) 修改 auto 参数设为 false时 拦截 `$startup` 的上传的 BUG;<br>3) 首次发送新用户的首次用户信息设置随 is_frist_time 参数为 true 时发送;<br>4) UTM 调整 campaign_id 调整为字符串;<br>5) 增加 autoProfile 参数自动调用 profile_set_once 设置 `$first_visit_time`, `$first_visit_time` 采用字符串的时间类型。 格式为:  yyyy-MM-dd hh:mm:ss.SSS;<br>6) debugModel 为1 与 2时的 log 输出规范化。
2018/08/24 | V4.0.6 |1）log 输出日志增加可读性；<br>2）优化 reset 方法逻辑；<br>3）优化可 JS SDK 启动发布顺序；<br>4）修复其他 BUG。
2018/09/18 | V4.1.0 |1）在初始化配置中新增`encryptType`参数，提供对上传数据可选是否进行加密功能；<br>2）增加配置参数`auto`控制启动事件与页面事件是否同时上报；<br>3) 在Cookie中公开 JS 临时 ID；<br>4) 优化启动事件上报逻辑。
2018/11/02 | V4.1.1 |1）在初始化配置中新增 `pageProperty` 参数，提供自动采集时可增加页面自定义属性；<br>2）优化可视化模块加载方法；<br>3）增加历史记录跳转页面自动采集。
2018/11/22| V4.1.2 |1)增加`$web_crawler`字段<br>2)优化打印Log<br>3)增加`$time_zone`字段<br>4）优化SDK性能
2018/12/17 | V4.1.3 |1）优化`session_id`相关<br>2)在首次启动中增加`$first_visit_language`字段<br>3)增加`$language`字段<br>4)优化SDK性能
2019/01/24 | V4.2.0 |1)相关功能封装为独立模块<br>2)优化SDK性能
## 小程序 SDK

更新日期 | 版本号 | 更新内容
---|---|---
2018/08/02 | V4.0.5 |1) 修改 auto 参数设为 false 时拦截 `$startup` 的上传的BUG;<br>2) 首次发送新用户的首次用户信息设置随 is_frist_time 参数为 true 时发送;<br>3) UTM 调整 campaign_id 调整为字符串且与 JS 保持一致;<br>4) 增加 autoProfile 参数自动调用 profile_set_once 设置 `$first_visit_time`，`$first_visit_time` 采用字符串的时间类型。格式为:  yyyy-MM-dd hh:mm:ss.SSS;<br>5) debugModel 为 1 与 2 时的 log 输出规范化。
2018/08/23 | V4.0.6 |1）log 输出日志增加可读性；<br>2）优化 reset 方法逻辑。
2018/09/18 | V4.1.0 |1）在初始化配置中新增`encryptType`参数，提供对上传数据可选是否进行加密功能；<br>2) 增加配置参数`auto`控制启动事件与页面事件是否同时上报；<br>3) 优化启动事件上报逻辑。
2018/12/13 | V4.1.2 |1）优化`session_id`相关<br>2)在首次启动中增加`$first_visit_language`字段<br>3)增加`$language`字段<br>4）优化SDK性能
2019/02/19 | V4.2.0 |1)相关功能封装为独立模块<br>2)优化SDK性能
## Java SDK

更新日期 | 版本号 | 更新内容
---|---|---
2018/09/18 | V4.0.7 |1) 代码结构优化；<br>2) 完善异常处理；<br>3) 上报地址格式校验；<br>4) 优化属性值为NULL的校验。

[![ ](https://imguserradar.analysys.cn/fangzhou/img/2019/01/201901151711159657.jpeg)](https://ark.analysys.cn/view/sign/signup.html?campaign_id=2111486795&utm_campaign=%E6%96%87%E6%A1%A3%E6%B3%A8%E5%86%8C&utm_medium=%E8%87%AA%E5%AA%92%E4%BD%93&utm_source=%E6%96%87%E6%A1%A3&utm_content=&utm_term=)