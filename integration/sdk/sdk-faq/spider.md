# 爬虫数据如何识别？

易观方舟JS SDK 会根据数据中的 UA 信息来识别的是上报的事件是否是爬虫产生的，方便在分析时根据事件中的爬虫属性来过滤掉这部分非真实用户产生的数据。

## 识别爬虫的正则表达式

```text
`/(bot|crawler|spider|scrapy|dnspod|ia_archiver|jiankongbao|slurp|transcoder|networkbench|oneapm|PhantomJS|BingPreview)/i`
```

##  常见的爬虫来源

1. **百度：**Baiduspider  Mozilla/5.0 \(compatible; Baiduspider/2.0;+[http://www.baidu.com/search/spider.html](http://www.baidu.com/search/spider.html)\)
2. **百度图片：**Baiduspider-image+\(+[http://www.baidu.com/search/spider.htm](http://www.baidu.com/search/spider.htm)\)
3. **百度PC：** Mozilla/5.0 \(compatible; Baiduspider-render/2.0; +[http://www.baidu.com/search/spider.html](http://www.baidu.com/search/spider.html)
4. **百度移动端：**Mozilla/5.0 \(iPhone; CPU iPhone OS 9\_1 like Mac OS X\) AppleWebKit/601.1.46 \(KHTML, like Gecko\) Version/9.0 Mobile/13B143 Safari/601.1 \(compatible; Baiduspider-render/2.0; +[http://www.baidu.com/search/spider.html](http://www.baidu.com/search/spider.html)\)
5. **谷歌：**Googlebot   Mozilla/5.0 \(compatible; Googlebot/2.1; +[http://www.google.com/bot.html](http://www.google.com/bot.html)\)  
6. **Google图片：**AdsBot-Google-Mobile \(+[http://www.google.com/mobile/adsbot.html](http://www.google.com/mobile/adsbot.html)\) Mozilla \(iPhone; U; CPU iPhone OS 3 0 like Mac OS X\) AppleWebKit \(KHTML, like Gecko\) Mobile Safari
7. **360蜘蛛：**360Spider 360搜索  Mozilla/5.0 \(compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0\);
8. **360网站安全：**360spider \([http://webscan.360.cn](http://webscan.360.cn/)\)
9. Bing爬虫: bingbot  Mozilla/5.0 \(compatible; bingbot/2.0; +[http://www.bing.com/bingbot.htm](http://www.bing.com/bingbot.htm)\)
10. **腾讯搜搜蜘蛛：**Sosospider  Sosospider+\(+[http://help.soso.com/webspider.htm](http://help.soso.com/webspider.htm)\)
11. **搜搜图片：** Sosoimagespider+\(+[http://help.soso.com/soso-image-spider.htm](http://help.soso.com/soso-image-spider.htm)\)
12. **雅虎蜘蛛：**Yahoo!   雅虎英文 Mozilla/5.0 \(compatible; Yahoo! Slurp; [http://help.yahoo.com/help/us/ysearch/slurp](http://help.yahoo.com/help/us/ysearch/slurp)\)
13. **雅虎中国：**Mozilla/5.0 \(compatible; Yahoo! Slurp China; [http://misc.yahoo.com.cn/help.html](http://misc.yahoo.com.cn/help.html)\)
14. **有道蜘蛛：**YoudaoBot Mozilla/5.0 \(compatible; YoudaoBot/1.0; [http://www.youdao.com/help/webmaster/spider/](http://www.youdao.com/help/webmaster/spider/); \)
15. **搜狗蜘蛛：**Sogou News Spider   Sogou web spider/4.0\(+[http://www.sogou.com/docs/help/webmasters.htm\#07](http://www.sogou.com/docs/help/webmasters.htm#07)\)
16. **搜狗图片：**Sogou Pic Spider/3.0\(+[http://www.sogou.com/docs/help/webmasters.htm\#07](http://www.sogou.com/docs/help/webmasters.htm#07)\)
17. **瑞典 Speedy Spider:** Speedy Spider  Mozilla/5.0 \(Windows; U; Windows NT 5.1; en-US\) Speedy Spider \([http://www.entireweb.com/about/search\_tech/speedy\_spider/](http://www.entireweb.com/about/search_tech/speedy_spider/)\)
18. **俄罗斯 yandex :** YandexBot Mozilla/5.0 \(compatible; YandexBot/3.0; +[http://yandex.com/bots](http://yandex.com/bots)\)
19. **MSN蜘蛛：**msnbot/msnbot-media msnbot/1.1 \(+[http://search.msn.com/msnbot.htm](http://search.msn.com/msnbot.htm)\)
20. **必应蜘蛛：**bingbot/compatible Mozilla/5.0 \(compatible; bingbot/2.0; +[http://www.bing.com/bingbot.htm](http://www.bing.com/bingbot.htm)\)
21. **听云爬虫：**networkbench Mozilla/5.0 \(Windows NT 10.0; Trident/7.0; rv: 11.0;NetworkBench/[8.0.1.309](http://con.analysys.cn/8.0.1.309)-5774440-2481662\) like Gecko
22. **Alexa蜘蛛：**ia\_archiver ia\_archiver/8.9 \(Windows NT 3.1; en-US;\)
23. **宜sou蜘蛛：**EasouSpider Mozilla/5.0 \(compatible; EasouSpider; +[http://www.easou.com/search/spider.html](http://www.easou.com/search/spider.html)\)
24. **华为赛门铁克蜘蛛：**HuaweiSymantecSpider  HuaweiSymantecSpider/1.0+DSE-[support@huaweisymantec.com+\(compatible](mailto:support@huaweisymantec.com+%28compatible); MSIE 7.0; Windows NT 5.1; Trident/4.0; .NET CLR 2.0.50727; .NET CLR 3.0.4506.2152; .NET CLR ; [http://www.huaweisymantec.com/cn/IRL/spider](http://www.huaweisymantec.com/cn/IRL/spider)\)
25. **七牛镜像蜘蛛：**qiniu qiniu-imgstg-spider-1.0
26. **DNSPod监控：**DNSPod  DNSPod-Monitor/2.0
27. **俄罗斯 LinkpadBot：**LinkpadBot   Mozilla/5.0 \(compatible; LinkpadBot/1.06; +[http://www.linkpad.ru](http://www.linkpad.ru/)\)
28. **英国 MJ12bot：**MJ12bot   Mozilla/5.0 \(compatible; MJ12bot/v1.4.0; [http://www.majestic12.co.uk/bot.php?+](http://www.majestic12.co.uk/bot.php?+)\)
29. **即刻蜘蛛：**JikeSpider
30. **一淘网蜘蛛：**EtaoSpider Mozilla/5.0 \(compatible; EtaoSpider/1.0; EtaoSpider\)
31. **人工智能爬虫：**crawler Mozilla/5.0 \(compatible; 008/0.83; [http://www.80legs.com/webcrawler.html](http://www.80legs.com/webcrawler.html)\) Gecko/2008032620
32. **Scrapy爬虫：** scrapy
33. **监控宝：**jiankongbao Mozilla/4.0 \(compatible; MSIE 6.0; Windows NT 5.1; SV1; JianKongBao Monitor 1.1\)
34. **OneAPM爬虫：**OneAPM FFAgent Mozilla/5.0 \(Windows NT 6.1; WOW64; rv:39.0: OneAPM FFAgent\)Gecko/20100101 Firefox/39.0
35. **PhantomJS：**PhantomJS  Mozilla/5.0 \(Unknown; Linux x86\_64\)AppleWebKit/538. 1 \(KHTML,like Gecko\)PhantomJS/2.1.1 Safari/538.1
36. **BingPreview：**Mozilla / 5.0 + \(Windows + NT + 6.1; + WOW64\) + AppleWebKit / 534++\(KHTML, +like + Gecko\) + BingPreview / 1.0 b



