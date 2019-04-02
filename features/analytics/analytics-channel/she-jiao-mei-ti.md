# 社交媒体

### 

| 域名 | 社交媒体 |
| :--- | :--- |
| t.cn | 微博 |
| weibo.com | 微博 |
| weibo.cn | 微博 |
| desktop.weibo.com | 微博 |
| qzone.qq.com | QQ空间 |
| im.qq.com | QQ |
| qun.qq.com | QQ群 |
| wx.qq.com | 微信 |
| wechat.com | 微信 |
| weixin.qq.com | 微信 |
| mp.weixin.qq.com | 微信 |
| mp.weixinbridge.com | 微信 |
| linkedin.com | 领英 |
| maimai.cn | 脉脉 |
| zhihu.com | 知乎 |
| douban.com | 豆瓣 |
| lofter.com | Lofter |
| facebook.com | Facebook |
| twitter.com | Twitter |
| myspace.com | MySpace |
| instagram.com | Instagram |
| t.qq.com | 腾讯微博 |
| t.sohu.com | 搜狐微博 |
| renren.com | 人人网 |
| share.renren.com | 人人网 |
| page.renren.com | 人人网 |
| life.renren.com | 人人网 |
| xiaozu.renren.com | 人人网 |
| kaixin001.com | 开心网 |
| love.163.com | 花田 |
| immomo.com | 陌陌 |
| tantanapp.com | 探探 |
| zhenai.com | 珍爱网 |
| jiayuan.com | 世纪佳缘 |
| baihe.com | 百合网 |

**鉴于微信在目前的流量获取中占据了非常重要的位置，所以我们根据微信环境中打开的数据参数进行了进一步识别**

| 分享来源 | 场景 | 说明 | 参数 |
| :--- | :--- | :--- | :--- |
| 微信-公众号 | 公众号中查看图文消息-点击“阅读原文” | refferer中识别是微信公众号环境，mp.weixin.qq.com |  |
| 微信-朋友圈 | 朋友圈里查看分享的页面类链接 | refferer中识别是微信环境，再根据url中参加上参数识别 | from=timeline |
| 微信-群聊 | 查看分享给群组的页面链接 | refferer中识别是微信环境，再根据url中参加上参数识别 | from=groupmessage |
| 微信-单聊 | 查看分享给单个好友的页面链接 | refferer中识别是微信环境，再根据url中参加上参数识别 | from=singlemessage |
| 微信-其他 | 公众号中直接的文字/图片链接、朋友圈/群聊/单聊消息中的URL链接、扫描在微信中的二维码 | 如果没有自定义加参数的话，只能识别到是微信中打开，但无法识别更具体的来源 |  |

### 

