# 脚本工具

对于私有化部署的客户，有的情况下可能会需要对项目进行一些更高级的操作，我们提供了一些脚本。

它们位于：

```bash
/opt/soft/streaming/bin/
```

具体如下：

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x529F;&#x80FD;&#x63CF;&#x8FF0;</th>
      <th style="text-align:left">&#x811A;&#x672C;&#x540D;&#x79F0;</th>
      <th style="text-align:left">&#x53C2;&#x6570;&#x8BF4;&#x660E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>&#x521B;&#x5EFA;&#x9879;&#x76EE;</b>
      </td>
      <td style="text-align:left">create_project.sh</td>
      <td style="text-align:left">
        <p><b>enterpriseId</b> &#x4F01;&#x4E1A;id &#x662F;&#x4E00;&#x4E2A;&#x56FA;&#x5B9A;&#x503C;
          1</p>
        <p><b>appKey</b> &#x521B;&#x5EFA;&#x9879;&#x76EE;&#x7684;appKey</p>
        <p><b>project_cname</b> &#x521B;&#x5EFA;&#x9879;&#x76EE;&#x7684;&#x4E2D;&#x6587;&#x540D;&#x79F0;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>&#x5220;&#x9664;&#x9879;&#x76EE;</b>
      </td>
      <td style="text-align:left">drop_project.sh</td>
      <td style="text-align:left"><b>appKey</b> &#x8981;&#x5220;&#x9664;&#x7684;&#x9879;&#x76EE;&#x7684;appKey</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>&#x542F;&#x52A8;&#x4E00;&#x4E2A;&#x9879;&#x76EE;&#x7684;&#x5B9E;&#x65F6;&#x6D41;</b>
      </td>
      <td style="text-align:left">start_streaming.sh</td>
      <td style="text-align:left"><b>appKey</b> &#x8981;&#x542F;&#x52A8;&#x5B9E;&#x65F6;&#x6D41;&#x7684;&#x9879;&#x76EE;&#x7684;appKey</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>&#x505C;&#x6B62;&#x4E00;&#x4E2A;&#x9879;&#x76EE;&#x7684;&#x5B9E;&#x65F6;&#x6D41;</b>
      </td>
      <td style="text-align:left">stop_streaming.sh</td>
      <td style="text-align:left"><b>appKey</b> &#x8981;&#x505C;&#x6B62;&#x5B9E;&#x65F6;&#x6D41;&#x7684;&#x9879;&#x76EE;&#x7684;appKey</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>&#x505C;&#x6B62;&#x6240;&#x6709;&#x9879;&#x76EE;&#x7684;&#x5B9E;&#x65F6;&#x6D41;</b>
      </td>
      <td style="text-align:left">stop_all_streaming.sh</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>&#x91CD;&#x7F6E;&#x65B9;&#x821F; admin &#x7528;&#x6237;&#x7684;&#x5BC6;&#x7801;&#x4E3A;111111</b>
      </td>
      <td style="text-align:left">reset_manager_password.sh</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>&#x4FEE;&#x6539;&#x5DF2;&#x7ECF;&#x5B58;&#x5728;&#x7684;&#x6570;&#x636E;&#x7C7B;&#x578B;</b>
      </td>
      <td style="text-align:left">update_datatype_scheduler.sh</td>
      <td style="text-align:left">
        <p><b>appkey</b> &#x8981;&#x64CD;&#x4F5C;&#x7684;&#x9879;&#x76EE;&#x7684;
          appKey</p>
        <p><b>table</b> &#x8868;&#x540D; [event|profile]</p>
        <p><b>column</b> &#x5217;&#x540D;</p>
        <p><b>datatype</b> &#x76EE;&#x6807;&#x6570;&#x636E;&#x7C7B;&#x578B;</p>
        <p><b>startdate</b> &#x8D77;&#x59CB;&#x65E5;&#x671F;</p>
        <p><b>enddate</b> &#x622A;&#x6B62;&#x65E5;&#x671F;</p>
      </td>
    </tr>
  </tbody>
</table>

{% hint style="info" %}
以上内容没有解答我的问题？[点击我来反馈](https://support.qq.com/products/118522/) 🚀
{% endhint %}

