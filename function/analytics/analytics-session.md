# Session 分析

除了分析每一个事件之外，方舟还支持Session的分析。Session，即会话，是指在指定的时间段内在您的网站/H5/小程序/APP上发生的一系列用户行为。例如，一次会话可以包含多个页面浏览、交互事件和电子商务交易。

## 功能说明

入口：进入事件分析，切换分析粒度到 Session

![ ](https://imguserradar.analysys.cn/fangzhou/img/2018/08/201808141203191074.png)

Session口径：方舟会预置几个通常的口径，当然也支持自定义Session口径，详见[《Session 管理》](../project-manegement/project-session.md)。

选择好口径之后，就可以选择指标来进行分析，支持细分维度和过滤条件。

Session 的主要指标说明：

* Session 总次数：在选定时间范围内，Session次数；
* Session 总用户数：在选定时间范围内，有Session用户数；由于一个用户可以有多个Session，所以Session总用户数≤Session总次数；
* Session 人均次数：在选定时间范围内，Session次数/Session用户数；通常越高说明用户粘性越好；
* Session 时长：假如某 个Session里，先后发生了事件a &gt; b &gt; c &gt; d，则Session 的时长为 d 减去 a，事件 d 之后的时长未知；
* Session 深度：Session内触发事件的次数，一个Session里发生了多少个事件；
* Session 退出率：某个事件的退出率指该事件作为 Session 的结束事件的次数除以该事件发生次数，退出率指 Session 数除以 Session 中所有事件发生次数。比如有三个 Session，第一个 Session 事件序列为A,B；第二个 Session 事件序列为C；第三个 Session 事件序列为D,E,F；则 Session 中B事件的退出率为33.33%, 任意事件的退出率为 50%；
* Session 跳出率：只有一个事件的Session数除以总 Session数。比如有三个 Session，第一个 Session 事件序列为 A,B；第二个 Session 事件序列为 C；第三个 Session 事件序列为D,E,F；则 Session 总体的跳出率为33.33%。

