# SDK更新日志

## Android SDK

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x66F4;&#x65B0;&#x65E5;&#x671F;</th>
      <th style="text-align:left">&#x7248;&#x672C;&#x53F7;</th>
      <th style="text-align:left">&#x66F4;&#x65B0;&#x5185;&#x5BB9;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">2018/08/02</td>
      <td style="text-align:left">V4.0.5</td>
      <td style="text-align:left">1) &#x589E;&#x52A0;&#x4E86; init(Context context, String key, String channel,
        String baseUrl,boolean autoProfile)&#x63A5;&#x53E3;;
        <br />2) push &#x5BF9;&#x5E94;&#x5B57;&#x6BB5;&#x7C7B;&#x578B;&#x4FEE;&#x6539;;
        <br
        />3) &#x4FEE;&#x6539;&#x7AEF;&#x53E3;&#x53F7;,&#x4E0A;&#x6570;,&#x6536;&#x6570;
        https &#x7531; 443 &#x4FEE;&#x6539;&#x4E3A; 4089;
        <br />4) &#x4FEE;&#x6539; webSocket &#x94FE;&#x63A5;,&#x4F7F;&#x7528; wss &#x7AEF;&#x53E3;&#x53F7;&#x4E3A;
        4091&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">2018/08/24</td>
      <td style="text-align:left">V4.0.6</td>
      <td style="text-align:left">1&#xFF09;log &#x8F93;&#x51FA;&#x65E5;&#x5FD7;&#x589E;&#x52A0;&#x53EF;&#x8BFB;&#x6027;&#xFF1B;
        <br
        />2&#xFF09;&#x652F;&#x6301; Hybrid &#x6A21;&#x5F0F;&#xFF1B;
        <br />3&#xFF09;&#x4F18;&#x5316;&#x53EF;&#x89C6;&#x5316;&#x4F18;&#x5316;&#x529F;&#x80FD;&#xFF1B;
        <br
        />4&#xFF09;&#x4F18;&#x5316; reset &#x65B9;&#x6CD5;&#x903B;&#x8F91;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">2018/09/18</td>
      <td style="text-align:left">V4.1.0</td>
      <td style="text-align:left">1&#xFF09;&#x5728;&#x521D;&#x59CB;&#x5316;&#x914D;&#x7F6E;&#x4E2D;&#x65B0;&#x589E;<code>encryptType</code>&#x53C2;&#x6570;&#xFF0C;&#x63D0;&#x4F9B;&#x5BF9;&#x4E0A;&#x4F20;&#x6570;&#x636E;&#x53EF;&#x9009;&#x662F;&#x5426;&#x8FDB;&#x884C;&#x52A0;&#x5BC6;&#x529F;&#x80FD;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">2018/11/19</td>
      <td style="text-align:left">V4.1.1</td>
      <td style="text-align:left">1&#xFF09;&#x5728;&#x5B8C;&#x5584;&#x65E5;&#x5FD7;&#x6253;&#x5370;;
        <br
        />2) &#x589E;&#x52A0;&#x65F6;&#x533A;&#x5B57;&#x6BB5;$time_zone &#x7C7B;&#x578B;
        String;
        <br />3)appkey &#x6539;&#x53D8; /debug &#x7531; 1 &#x53D8;&#x4E3A; 0 &#x6216;
        2/uploadURL &#x6539;&#x53D8;&#x65F6;,&#x91CD;&#x7F6E;&#x5B58;&#x50A8;&#x5185;&#x5BB9;&#xFF0C;&#x91CD;&#x65B0;&#x53D1;&#x9001;
        profileSetOnce;
        <br />4) &#x6570;&#x7EC4;&#x7C7B;&#x578B;&#x6570;&#x636E;&#x4EC5;&#x652F;&#x6301;&#x5143;&#x7D20;&#x4E3A;&#x5B57;&#x7B26;&#x4E32;&#x7C7B;&#x578B;&#x7684;&#x6570;&#x7EC4;;
        <br
        />4.1) &#x589E;&#x52A0;&#x4E0D;&#x5141;&#x8BB8;&#x8986;&#x76D6;&#x5B57;&#x6BB5;$first_visit_time;
        <br
        />5) &#x7CFB;&#x7EDF;&#x7248;&#x672C;&#x5B57;&#x6BB5; $os_version &#x4E4B;&#x524D;&#x589E;&#x52A0;
        &#x7CFB;&#x7EDF;&#x540D;&#x79F0;;
        <br />6) channel&#x8BBE;&#x7F6E;&#x903B;&#x8F91;&#x4FEE;&#x6539;&#x4E3A;&#xFF0C;&#x5F53;xml&#x8BBE;&#x7F6E;&#x4E0E;API&#x4F20;&#x503C;&#x4E24;&#x79CD;&#x65B9;&#x5F0F;&#x90FD;&#x8BBE;&#x7F6E;&#x65F6;&#x4F18;&#x5148;&#x4F7F;&#x7528;xml&#x8BBE;&#x7F6E;&#x503C;;
        <br
        />7)key&#x8BBE;&#x7F6E;&#x903B;&#x8F91;&#x4FEE;&#x6539;&#x4E3A;&#xFF0C;&#x5F53;xml&#x8BBE;&#x7F6E;&#x4E0E;API&#x4F20;&#x503C;&#x4E24;&#x79CD;&#x65B9;&#x5F0F;&#x8BBE;&#x7F6E;key&#x4E0D;&#x76F8;&#x540C;&#x65F6;&#xFF0C;&#x63D0;&#x793A;&#x8BBE;&#x7F6E;&#x5F02;&#x5E38;;
        <br
        />8) &#x529F;&#x80FD;&#x4F18;&#x5316;;
        <br />8.1) &#x7C7B;&#x578B;&#x5224;&#x65AD;&#x90E8;&#x5206;&#x505A;&#x4E86;&#x4F18;&#x5316;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">2018/12/17</td>
      <td style="text-align:left">V4.1.2</td>
      <td style="text-align:left">1)&#x4F18;&#x5316;<code>session_id</code>&#x76F8;&#x5173;
        <br />2)&#x5728;&#x9996;&#x6B21;&#x542F;&#x52A8;&#x4E2D;&#x589E;&#x52A0;<code>$first_visit_language</code>&#x5B57;&#x6BB5;
        <br
        />3)&#x589E;&#x52A0;<code>$language</code>&#x5B57;&#x6BB5;
        <br />4)&#x4F18;&#x5316;SDK&#x6027;&#x80FD;</td>
    </tr>
    <tr>
      <td style="text-align:left">2019/03/04</td>
      <td style="text-align:left">V4.2.1</td>
      <td style="text-align:left">1)&#x76F8;&#x5173;&#x529F;&#x80FD;&#x5C01;&#x88C5;&#x4E3A;&#x72EC;&#x7ACB;&#x6A21;&#x5757;
        <br
        />2)&#x4F18;&#x5316;SDK&#x6027;&#x80FD;</td>
    </tr>
    <tr>
      <td style="text-align:left">2019/03/13</td>
      <td style="text-align:left">v4.2.2</td>
      <td style="text-align:left">1)&#x4F18;&#x5316;&#x591A;&#x7EBF;&#x7A0B;&#x95EE;&#x9898;</td>
    </tr>
    <tr>
      <td style="text-align:left">2019/05/24</td>
      <td style="text-align:left">v4.3.1</td>
      <td style="text-align:left">
        <p>1)&#x63D0;&#x4F9B;&#x70ED;&#x56FE;&#x6A21;&#x5757;&#xFF08;&#x9002;&#x7528;&#x65B9;&#x821F;V4.3.0&#x7248;&#x672C;&#xFF09;</p>
        <p>2)&#x4F18;&#x5316;Log&#x63D0;&#x4F9B;&#x529F;&#x80FD;</p>
        <p>3)&#x4F18;&#x5316;&#x6027;&#x80FD;</p>
      </td>
    </tr>
  </tbody>
</table>## iOS SDK

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x66F4;&#x65B0;&#x65E5;&#x671F;</th>
      <th style="text-align:left">&#x7248;&#x672C;&#x53F7;</th>
      <th style="text-align:left">&#x66F4;&#x65B0;&#x5185;&#x5BB9;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">2018/08/02</td>
      <td style="text-align:left">V4.0.5</td>
      <td style="text-align:left">1) campaign_id &#x8C03;&#x6574;&#x4E3A;&#x5B57;&#x7B26;&#x4E32;&#x3002;ACTIONTYPE
        &#x9ED8;&#x8BA4;&#x4E3A;&#x6570;&#x503C;&#x578B;;
        <br />2) &#x589E;&#x52A0;&#x8C03;&#x8BD5;&#x6A21;&#x5F0F;&#x4E0B;&#x6253;&#x5370;&#x4E0A;&#x4F20;&#x6570;&#x636E;&#x8BE6;&#x60C5;&#x4FE1;&#x606F;;
        <br
        />3) &#x589E;&#x52A0;&#x9996;&#x6B21;&#x542F;&#x52A8;&#x8C03;&#x7528; profile_set_once
        &#x63A5;&#x53E3;&#x4E0A;&#x4F20;&#x542F;&#x52A8;&#x65F6;&#x95F4;;
        <br />4) &#x5220;&#x9664;&#x63A5;&#x53E3;&#xFF1A;startWithAppKey:channel:uploadURL:;
        <br
        />5) &#x589E;&#x52A0;&#x63A5;&#x53E3;&#xFF1A;startWithAppKey:channel:baseURL:autoProfile:;
        <br
        />6&#xFF09; &#x6570;&#x636E;&#x4E0A;&#x4F20;&#x53CA;&#x53EF;&#x89C6;&#x5316;&#x6570;&#x636E;&#x914D;&#x7F6E;&#x7AEF;&#x53E3;&#x4E3A;(&#x9ED8;&#x8BA4;https)&#xFF1A;4089;
        <br
        />7) &#x53EF;&#x89C6;&#x5316;&#x7AEF;&#x53E3;&#x4E3A;&#xFF08;&#x9ED8;&#x8BA4;wss&#xFF09;&#xFF1A;4091&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">2018/08/24</td>
      <td style="text-align:left">V4.0.6</td>
      <td style="text-align:left">1&#xFF09;log &#x8F93;&#x51FA;&#x65E5;&#x5FD7;&#x589E;&#x52A0;&#x53EF;&#x8BFB;&#x6027;&#xFF1B;
        <br
        />2&#xFF09;&#x652F;&#x6301; Hybrid &#x6A21;&#x5F0F;&#xFF1B;
        <br />3&#xFF09;&#x4F18;&#x5316;&#x53EF;&#x89C6;&#x5316;&#x4F18;&#x5316;&#x529F;&#x80FD;&#xFF1B;
        <br
        />4&#xFF09;&#x4F18;&#x5316; reset &#x65B9;&#x6CD5;&#x903B;&#x8F91;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">2018/09/18</td>
      <td style="text-align:left">V4.1.0</td>
      <td style="text-align:left">1&#xFF09;&#x5728;&#x521D;&#x59CB;&#x5316;&#x914D;&#x7F6E;&#x4E2D;&#x65B0;&#x589E;<code>encryptType</code>&#x53C2;&#x6570;&#xFF0C;&#x63D0;&#x4F9B;&#x5BF9;&#x4E0A;&#x4F20;&#x6570;&#x636E;&#x53EF;&#x9009;&#x662F;&#x5426;&#x8FDB;&#x884C;&#x52A0;&#x5BC6;&#x529F;&#x80FD;&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">2018/11/19</td>
      <td style="text-align:left">V4.1.1</td>
      <td style="text-align:left">1&#xFF09;&#x5728;&#x5B8C;&#x5584;&#x65E5;&#x5FD7;&#x6253;&#x5370;;
        <br
        />2) &#x589E;&#x52A0;&#x65F6;&#x533A;&#x5B57;&#x6BB5;$time_zone &#x7C7B;&#x578B;
        String;
        <br />3)appkey &#x6539;&#x53D8; /debug &#x7531; 1 &#x53D8;&#x4E3A; 0 &#x6216;
        2/uploadURL &#x6539;&#x53D8;&#x65F6;,&#x91CD;&#x7F6E;&#x5B58;&#x50A8;&#x5185;&#x5BB9;&#xFF0C;&#x91CD;&#x65B0;&#x53D1;&#x9001;
        profileSetOnce;
        <br />4) &#x6570;&#x7EC4;&#x7C7B;&#x578B;&#x6570;&#x636E;&#x4EC5;&#x652F;&#x6301;&#x5143;&#x7D20;&#x4E3A;&#x5B57;&#x7B26;&#x4E32;&#x7C7B;&#x578B;&#x7684;&#x6570;&#x7EC4;;&#x589E;&#x52A0;&#x4E0D;&#x5141;&#x8BB8;&#x8986;&#x76D6;&#x5B57;&#x6BB5;$first_visit_time;
        <br
        />5) &#x7CFB;&#x7EDF;&#x7248;&#x672C;&#x5B57;&#x6BB5; $os_version &#x4E4B;&#x524D;&#x589E;&#x52A0;
        &#x7CFB;&#x7EDF;&#x540D;&#x79F0;;
        <br />6) &#x529F;&#x80FD;&#x4F18;&#x5316;;
        <br />6.1) &#x4FEE;&#x590D;&#x5931;&#x8D25;&#x91CD;&#x4F20;&#x7B56;&#x7565;&#xFF0C;&#x95F4;&#x9694;&#x4E0D;&#x751F;&#x6548;bug&#xFF1B;
        <br
        />6.2) &#x4FEE;&#x590D;App&#x6740;&#x6389;&#x540E;&#x65E0;&#x6CD5;&#x4FDD;&#x5B58;end&#x4E8B;&#x4EF6;bug&#xFF1B;
        <br
        />6.3) &#x4FEE;&#x590D;App&#x9000;&#x5165;&#x540E;&#x53F0;&#x65E0;&#x6CD5;&#x4E0A;&#x4F20;bug&#x3002;</td>
    </tr>
    <tr>
      <td style="text-align:left">2018/12/17</td>
      <td style="text-align:left">V4.1.2</td>
      <td style="text-align:left">1)&#x4F18;&#x5316;<code>session_id</code>&#x76F8;&#x5173;
        <br />2)&#x5728;&#x9996;&#x6B21;&#x542F;&#x52A8;&#x4E2D;&#x589E;&#x52A0;<code>$first_visit_language</code>&#x5B57;&#x6BB5;
        <br
        />3)&#x589E;&#x52A0;<code>$language</code>&#x5B57;&#x6BB5;
        <br />4)&#x4F18;&#x5316;SDK&#x6027;&#x80FD;</td>
    </tr>
    <tr>
      <td style="text-align:left">2019/02/15</td>
      <td style="text-align:left">V4.2.0</td>
      <td style="text-align:left">1)&#x76F8;&#x5173;&#x529F;&#x80FD;&#x5C01;&#x88C5;&#x4E3A;&#x72EC;&#x7ACB;&#x6A21;&#x5757;
        <br
        />2)&#x4F18;&#x5316;SDK&#x6027;&#x80FD;</td>
    </tr>
    <tr>
      <td style="text-align:left">2019/05/24</td>
      <td style="text-align:left">v4.3.1</td>
      <td style="text-align:left">
        <p>1)&#x63D0;&#x4F9B;&#x70ED;&#x56FE;&#x6A21;&#x5757;&#xFF08;&#x9002;&#x7528;&#x65B9;&#x821F;V4.3.0&#x7248;&#x672C;&#xFF09;</p>
        <p>2)&#x4F18;&#x5316;Log&#x63D0;&#x4F9B;&#x529F;&#x80FD;</p>
        <p>3)&#x4F18;&#x5316;&#x6027;&#x80FD;</p>
      </td>
    </tr>
  </tbody>
</table>## JS SDK

| 更新日期 | 版本号 | 更新内容 |
| :--- | :--- | :--- |
| 2018/08/02 | V4.0.5 | 1\) 修改 `$webstay` 日志中,`$url` 参数的值超过长度 255 字符数时，将 `$url` 参数抛弃的 BUG。\(20180803\); 2\) 修改 auto 参数设为 false时 拦截 `$startup` 的上传的 BUG; 3\) 首次发送新用户的首次用户信息设置随 is\_frist\_time 参数为 true 时发送; 4\) UTM 调整 campaign\_id 调整为字符串; 5\) 增加 autoProfile 参数自动调用 profile\_set\_once 设置 `$first_visit_time`, `$first_visit_time` 采用字符串的时间类型。 格式为:  yyyy-MM-dd hh:mm:ss.SSS; 6\) debugModel 为1 与 2时的 log 输出规范化。 |
| 2018/08/24 | V4.0.6 | 1）log 输出日志增加可读性； 2）优化 reset 方法逻辑； 3）优化可 JS SDK 启动发布顺序； 4）修复其他 BUG。 |
| 2018/09/18 | V4.1.0 | 1）在初始化配置中新增`encryptType`参数，提供对上传数据可选是否进行加密功能； 2）增加配置参数`auto`控制启动事件与页面事件是否同时上报； 3\) 在Cookie中公开 JS 临时 ID； 4\) 优化启动事件上报逻辑。 |
| 2018/11/02 | V4.1.1 | 1）在初始化配置中新增 `pageProperty` 参数，提供自动采集时可增加页面自定义属性； 2）优化可视化模块加载方法； 3）增加历史记录跳转页面自动采集。 |
| 2018/11/22 | V4.1.2 | 1\)增加`$web_crawler`字段 2\)优化打印Log 3\)增加`$time_zone`字段 4）优化SDK性能 |
| 2018/12/17 | V4.1.3 | 1）优化`session_id`相关 2\)在首次启动中增加`$first_visit_language`字段 3\)增加`$language`字段 4\)优化SDK性能 |
| 2019/01/24 | V4.2.0 | 1\)相关功能封装为独立模块 2\)优化SDK性能 |

## 小程序 SDK

| 更新日期 | 版本号 | 更新内容 |
| :--- | :--- | :--- |
| 2018/08/02 | V4.0.5 | 1\) 修改 auto 参数设为 false 时拦截 `$startup` 的上传的BUG; 2\) 首次发送新用户的首次用户信息设置随 is\_frist\_time 参数为 true 时发送; 3\) UTM 调整 campaign\_id 调整为字符串且与 JS 保持一致; 4\) 增加 autoProfile 参数自动调用 profile\_set\_once 设置 `$first_visit_time`，`$first_visit_time` 采用字符串的时间类型。格式为:  yyyy-MM-dd hh:mm:ss.SSS; 5\) debugModel 为 1 与 2 时的 log 输出规范化。 |
| 2018/08/23 | V4.0.6 | 1）log 输出日志增加可读性； 2）优化 reset 方法逻辑。 |
| 2018/09/18 | V4.1.0 | 1）在初始化配置中新增`encryptType`参数，提供对上传数据可选是否进行加密功能； 2\) 增加配置参数`auto`控制启动事件与页面事件是否同时上报； 3\) 优化启动事件上报逻辑。 |
| 2018/12/13 | V4.1.2 | 1）优化`session_id`相关 2\)在首次启动中增加`$first_visit_language`字段 3\)增加`$language`字段 4）优化SDK性能 |
| 2019/02/19 | V4.2.0 | 1\)相关功能封装为独立模块 2\)优化SDK性能 |

## Java SDK

| 更新日期 | 版本号 | 更新内容 |
| :--- | :--- | :--- |
| 2018/09/18 | V4.0.7 | 1\) 代码结构优化； 2\) 完善异常处理； 3\) 上报地址格式校验； 4\) 优化属性值为NULL的校验。 |

## PHP SDK

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x66F4;&#x65B0;&#x65E5;&#x671F;</th>
      <th style="text-align:left">&#x7248;&#x672C;&#x53F7;</th>
      <th style="text-align:left">&#x66F4;&#x65B0;&#x5185;&#x5BB9;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">2018/12/21</td>
      <td style="text-align:left">V4.0.1</td>
      <td style="text-align:left">&#x5B8C;&#x6210;PHP SDK</td>
    </tr>
    <tr>
      <td style="text-align:left">2019/5/12</td>
      <td style="text-align:left">V4.0.9</td>
      <td style="text-align:left">
        <p>&#x4FEE;&#x590D;&#x5728;&#x90E8;&#x5206;&#x573A;&#x666F;&#x4E0B;&#x5F02;&#x5E38;</p>
        <p>&#x4F18;&#x5316;&#x76F8;&#x5173;&#x67B6;&#x6784;</p>
      </td>
    </tr>
  </tbody>
</table>

