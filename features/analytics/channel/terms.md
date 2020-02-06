# 渠道相关名词解释

**渠道识别**

移动端和 Web 端略有差异

* Web 端/小程序：国内外都是通过URL的 Referer 和 LinkTag（e.g. UTM 参数） 识别；
* 移动端：国外通过 Google Play 和 AppStore 的参数传递来实现；国内应用商店百花齐放，情况比较复杂，通过渠道包、OneLink、DeepLink 等多种方式实现

**Referer**

HTTP Referer是header的一部分，当浏览器向web服务器发送请求的时候，一般会带上Referer，告诉服务器我是从哪个页面链接过来的，是一个页面的物理来源，部分情况下出于安全/隐私考虑会被隐藏。

通过对这个值的解析，可以知道来源 domain 甚至具体页面，还可以将 domain 进行可读性转义。

**Link Tag**

广告链接标记，通过类似 UTM 的参数来标记，在标记的目标页面 URL 中携带，可以人为配置，具体使用方法详见[《广告跟踪》](../../operation/utm.md)。

**会话（Session）**

Session 是一系列用户行为的集合。来源标识的也是一个 Session 的来源。

怎样一些列行为算是一个Session？详见 [《附6 Session 规则》](session.md)

**着陆页（LandingPage）**

用户进入目标网站的第一个页面。用户访问的一般过程：站内着陆页A → 后续受访页面B→ 后续受访页面……→ 站内出口页X

**跳出**

用户来到网站后，除了浏览LandingPage之外，没有发生其他任何操作就离开了网站，被视为跳出。



{% hint style="info" %}
以上内容没有解答我的问题？[点击我来反馈](https://support.qq.com/products/118522/) 🚀
{% endhint %}

