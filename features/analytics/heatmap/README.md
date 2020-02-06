# 热图分析

热图分析模型能够用热谱图直观呈现用户在网站、H5页面、APP上的点击、滚动行为，帮助产品、运营人员了解用户的点击偏好，辅助做页面设计优化、内容调整等。

## 常见热图类型和使用场景

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x70ED;&#x56FE;&#x7C7B;&#x578B;</th>
      <th style="text-align:left">&#x793A;&#x4F8B;</th>
      <th style="text-align:left">&#x4F7F;&#x7528;&#x573A;&#x666F;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">&#x70B9;&#x51FB;&#x4F4D;&#x7F6E;&#x70ED;&#x56FE;</td>
      <td style="text-align:left">
        <img src="https://imguserradar.analysys.cn/fangzhou/img/2019/04/201904171402052004.png"
        alt="image" />
      </td>
      <td style="text-align:left">
        <p>&#x5C55;&#x793A;&#x7528;&#x6237;&#x5728;&#x7F51;&#x7AD9;&#x4E0A;&#x6240;&#x6709;&#x70B9;&#x51FB;&#x7684;&#x4F4D;&#x7F6E;&#xFF0C;&#x805A;&#x96C6;&#x7684;&#x70B9;&#x51FB;&#x8D8A;&#x591A;&#xFF0C;&#x989C;&#x8272;&#x8D8A;&#x4EAE;&#x3002;</p>
        <p>&#x901A;&#x5E38;&#x7528;&#x4E8E;&#x5206;&#x6790;&#x7740;&#x9646;&#x9875;</p>
        <p>1 &#x662F;&#x5426;&#x70B9;&#x51FB;&#x4E86;CTA&#x7684;&#x5185;&#x5BB9;&#xFF1F;</p>
        <p>2 &#x662F;&#x5426;&#x6709;&#x88AB;&#x5927;&#x91CF;&#x70B9;&#x51FB;&#x7684;&#x91CD;&#x8981;&#x6309;&#x94AE;&#x6216;&#x5143;&#x7D20;&#x88AB;&#x653E;&#x5230;&#x4E86;&#x5F88;&#x5C11;&#x6709;&#x7528;&#x6237;&#x5230;&#x8FBE;&#x7684;&#x5730;&#x65B9;&#xFF1F;</p>
        <p>3 &#x662F;&#x5426;&#x6709;&#x7528;&#x6237;&#x70B9;&#x51FB;&#x7684;&#x56FE;&#x7247;&#x6216;&#x6587;&#x5B57;&#x5176;&#x5B9E;&#x6CA1;&#x6709;&#x94FE;&#x63A5;&#xFF1F;e.g.&#x4EA7;&#x54C1;&#x7279;&#x5F81;&#xFF0C;icon+&#x6587;&#x5B57;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&#x70B9;&#x51FB;&#x5143;&#x7D20;&#x70ED;&#x56FE;</td>
      <td style="text-align:left">
        <img src="https://imguserradar.analysys.cn/fangzhou/img/2019/04/201904171402052731.png"
        alt="image" />
      </td>
      <td style="text-align:left">
        <p>&#x5C55;&#x793A;&#x53EF;&#x4EA4;&#x4E92;&#x5143;&#x7D20;&#x7684;&#x70B9;&#x51FB;&#x60C5;&#x51B5;&#x3002;</p>
        <p>&#x7528;&#x4E8E;&#x5206;&#x6790;&#xFF1A;</p>
        <p>1 &#x5177;&#x4F53;&#x662F;&#x54EA;&#x4E9B;&#x5143;&#x7D20;&#x5438;&#x5F15;&#x4E86;&#x591A;&#x5C11;&#x70B9;&#x51FB;&#xFF1F;&#x5360;&#x636E;&#x4E86;&#x6574;&#x9875;&#x70B9;&#x51FB;&#x591A;&#x5C11;&#x6BD4;&#x4F8B;&#xFF1F;</p>
        <p>2 &#x662F;&#x5426;&#x6709;&#x4E0D;&#x7B26;&#x5408;&#x6211;&#x4EEC;&#x9884;&#x671F;&#x7684;&#x5931;&#x8BEF;&#x8BF1;&#x5BFC;&#xFF1F;</p>
        <p>(&#x9700; JS SDK &gt; 4.3.1)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&#x6D4F;&#x89C8;&#x6DF1;&#x5EA6;&#x7EBF;</td>
      <td style="text-align:left">
        <img src="https://imguserradar.analysys.cn/fangzhou/img/2019/04/201904171402040221.png"
        alt="image" />
      </td>
      <td style="text-align:left">
        <p>&#x5C55;&#x793A;&#x7528;&#x6237;&#x62B5;&#x8FBE;&#x67D0;&#x4E2A;&#x533A;&#x57DF;&#x7684;&#x7559;&#x5B58;&#x6BD4;&#x4F8B;&#x3002;&#x767E;&#x5206;&#x6BD4;&#x8D8A;&#x4F4E;&#xFF0C;&#x8D8A;&#x5C11;&#x7528;&#x6237;&#x80FD;&#x591F;&#x770B;&#x5230;&#x8FD9;&#x4E00;&#x4F4D;&#x7F6E;&#x3002;</p>
        <p>&#x901A;&#x5E38;&#x7528;&#x4E8E;</p>
        <p>1 &#x5BFB;&#x627E;CTA&#x7684;&#x6700;&#x4F73;&#x4F4D;&#x7F6E;</p>
        <p>2 &#x5185;&#x5BB9;&#x8425;&#x9500;&#x8F6C;&#x6362;&#x76D1;&#x6D4B;e.g.&#x6570;&#x636E;&#x9AA4;&#x964D;&#x65F6;&#xFF0C;&#x8BF4;&#x660E;&#x54EA;&#x91CC;&#x51FA;&#x73B0;&#x4E86;&#x5185;&#x5BB9;&#x4E0A;&#x7684;&#x65AD;&#x88C2;&#xFF0C;&#x7528;&#x6237;&#x6CA1;&#x6709;&#x5174;&#x8DA3;&#x518D;&#x770B;&#x4E0B;&#x53BB;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">&#x6CE8;&#x610F;&#x529B;&#x70ED;&#x56FE;</td>
      <td style="text-align:left">
        <img src="https://imguserradar.analysys.cn/fangzhou/img/2019/04/201904171411256839.png"
        alt="image" />
      </td>
      <td style="text-align:left">
        <p>&#x5C55;&#x793A;&#x7528;&#x6237;&#x5728;&#x67D0;&#x4E2A;&#x533A;&#x57DF;&#x505C;&#x7559;&#x7684;&#x65F6;&#x957F;&#xFF0C;&#x505C;&#x7559;&#x7684;&#x65F6;&#x95F4;&#x8D8A;&#x957F;&#xFF0C;&#x8BE5;&#x533A;&#x57DF;&#x989C;&#x8272;&#x8D8A;&#x4EAE;</p>
        <p>&#x901A;&#x5E38;&#x7528;&#x4E8E;&#x5206;&#x6790;</p>
        <p>1 &#x4E86;&#x89E3;&#x5230;&#x7F51;&#x9875;&#x54EA;&#x4E9B;&#x5185;&#x5BB9;&#x5438;&#x5F15;&#x8BBF;&#x5BA2;&#xFF0C;&#x54EA;&#x4E9B;&#x5185;&#x5BB9;&#x8BA4;&#x4E3A;&#x91CD;&#x8981;&#x5374;&#x88AB;&#x7528;&#x6237;&#x5FFD;&#x7565;&#xFF1F;</p>
        <p>2 &#x662F;&#x5426;&#x6709;&#x88AB;&#x7528;&#x6237;&#x4ED4;&#x7EC6;&#x9605;&#x8BFB;&#x7684;&#x5185;&#x5BB9;&#x653E;&#x5230;&#x4E86;&#x8FC7;&#x4E8E;&#x9760;&#x4E0B;&#x7684;&#x4F4D;&#x7F6E;&#xFF1F;</p>
      </td>
    </tr>
  </tbody>
</table>不同类型的热图各有优缺点，比如，点击位置热图，劣势是上报的数据量会增加，但可以非常直观的定性分析用户的探索性需求，发现非交互元素上意料之外的大量点击；点击元素热图，过滤掉了部分不可点击的内容，对可点击元素可以集中定量分析，但就会不够直观。

在不同场景下选择适合的类型，目前方舟已经支持了 Web 端的点击位置热图、点击元素热图、浏览深度线，APP 端的点击位置热图和点击元素热图。

## Web/H5 热图

支持网站、H5 页面的点击位置热图、点击元素热图和浏览深度线， 支持两种查看方式：

* 嵌入式——在方舟产品中查看页面热图，可切换不同的平台
* 交互式——在原网站页面查看

{% page-ref page="web.md" %}

## APP 热图

目前支持原生应用的点击位置热图和点击元素热图。

{% page-ref page="app.md" %}



{% hint style="info" %}
以上内容没有解答我的问题？[点击我来反馈](https://support.qq.com/products/118522/) 🚀
{% endhint %}

