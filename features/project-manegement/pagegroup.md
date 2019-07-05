---
description: 该功能处于内测阶段，即将在 4.3.0 版本发布
---

# 页面组管理（alpha）

页面组是具有共同结构特征的页面集合，如商品详情页、新闻资讯页；可用于基础指标分析或热图分析。

## 1. 创建页面组

![](../../.gitbook/assets/yemianzu.gif)

可以设定一组或多组 URL 规则快速匹配一组具有相同结构的页面，支持 5 种URL规则：包含、开头为、结尾为、等于、正则表达式：

### 包含

任何包含输入字符串的URL。e.g. 输入URL 包含 abc，则将筛选出 abc.com、ark.abc.cn 等类似的URL；

### 开头为

任何开头为输入字符串的URL。e.g. 输入URL开头为http://www.example.com/category=1，则将筛选出http://www.example.com/category=1&id=1、http://www.example.com/category=1&id=2 等所有分类下的URL；

### 结尾为

任何结尾为输入字符串的URL。e.g. 输入URL结尾为 project-management，则将筛选出 http://www.example1.com/project-management 、 http://www.example2.com/project-management 等URL；

### 等于

和输入URL完全一样的网址，支持输入多个地址；

### 正则表达式

符合正则表达式的URL，通过通配符、定位符、分组等符号组成逻辑公式。e.g. \(?=.\*page.\*\)\(?!.\*otherpage\).\* 筛选出包含page 但是不包含 otherpage 的所有页面。

{% hint style="info" %}
规则中输入多个地址或特征时英文分号分隔
{% endhint %}

## 2. 页面组管理

页面组的修改、复制、删除

![](../../.gitbook/assets/image%20%2850%29.png)

## **附 正则表达式常用语法**

正则表达式使用单个字符串来描述、匹配一系列匹配某个句法规则的字符串，是对字符串操作的一种逻辑公式。

### 常用字符

正则表达式字符主要包含：通配符、定位符、分组等。

**通配符，**可以用来代替一个或多个字符，常用的有

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x7B26;&#x53F7;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
      <th style="text-align:left">&#x793A;&#x4F8B;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">*</td>
      <td style="text-align:left">*&#x524D;&#x7684;&#x5B57;&#x7B26;&#x91CD;&#x590D;&#x51FA;&#x73B0;n&#x6B21;&#xFF08;n&#x2265;0&#xFF09;</td>
      <td
      style="text-align:left">
        <p>&#x8868;&#x8FBE;&#x5F0F;&#x662F; argo*ark</p>
        <p>&#x5219;argoark&#x3001;argooooark&#x90FD;&#x53EF;&#x4EE5;&#x5339;&#x914D;</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">&#xFF1F;</td>
      <td style="text-align:left">&#xFF1F;&#x524D;&#x7684;&#x5B57;&#x7B26;&#x91CD;&#x590D;&#x51FA;&#x73B0;0&#x6B21;&#x6216;1&#x6B21;</td>
      <td
      style="text-align:left">
        <p>&#x8868;&#x8FBE;&#x5F0F;&#x662F; argo&#xFF1F;ark</p>
        <p>&#x5219;&#x53EA;&#x80FD;&#x5339;&#x914D;&#x51FA;argoark&#x3001;argark</p>
        </td>
    </tr>
  </tbody>
</table>**定位符，**基于指定位置的匹配，通常在开始或结束的位置

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x7B26;&#x53F7;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
      <th style="text-align:left">&#x793A;&#x4F8B;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">^</td>
      <td style="text-align:left">&#x4ECE;&#x5F00;&#x5934;&#x5339;&#x914D;&#x5B57;&#x7B26;&#x4E32;</td>
      <td
      style="text-align:left">
        <p>&#x8868;&#x8FBE;&#x5F0F;&#x662F; ^ark</p>
        <p>&#x5219; ark.analysys.cn&#x3001;arkargo &#x53EF;&#x4EE5;&#x5339;&#x914D;&#xFF0C;&#x4F46;
          docs.ark&#x3001;argoark &#x7B49;&#x4E0D;&#x5339;&#x914D;</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">$</td>
      <td style="text-align:left">&#x4EE5;&#x7ED3;&#x5C3E;&#x5339;&#x914D;&#x5B57;&#x7B26;&#x4E32;</td>
      <td
      style="text-align:left">
        <p>&#x8868;&#x8FBE;&#x5F0F;&#x662F; ark$</p>
        <p>&#x5219; doc.ark.cn&#x3001;argoark &#x53EF;&#x4EE5;&#x5339;&#x914D;&#xFF0C;&#x4F46;
          ark.analysys.cn&#x3001;arkargo &#x7B49;&#x4E0D;&#x5339;&#x914D;</p>
        </td>
    </tr>
  </tbody>
</table>更多可参考 [https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular\_Expressions](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions)



### **常用示例**

\*\*\*\*

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x5E38;&#x89C1;&#x7528;&#x6CD5;</th>
      <th style="text-align:left">&#x793A;&#x4F8B;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">&#x5934;&#x90E8;&#x57DF;&#x540D;&#x5B8C;&#x5168;&#x5339;&#x914D; &#xFF0C;&#x5C3E;&#x90E8;&#x5E26;/&#x6216;&#x5E26;?&#x52A0;&#x53C2;&#x6570;</td>
      <td
      style="text-align:left">
        <p>&#x8868;&#x8FBE;&#x5F0F;&#xFF1A;^https:\/\/www.analysys.cn($|\/$|\/\?.*)
          <br
          />
        </p>
        <p>&#x80FD;&#x5339;&#x914D;&#x4EE5;&#x4E0B;&#x9875;&#x9762;&#xFF1A;
          <br />https://www.analysys.cn
          <br />https://www.analysys.cn/
          <br />https://www.analysys.cn/?utmsource=xx
          <br />
        </p>
        <p>&#x4E0D;&#x80FD;&#x5339;&#x914D;&#x4EE5;&#x4E0B;&#x9875;&#x9762;&#xFF1A;
          <br
          />https://www.analysys.c/abc</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">&#x5934;&#x90E8;&#x5339;&#x914D; http &#x6216; https &#x534F;&#x8BAE;&#xFF0C;&#x5E76;&#x4E14;&#x5339;&#x914D;&#x591A;&#x4E2A;&#x5B50;&#x57DF;&#x540D;</td>
      <td
      style="text-align:left">
        <p>&#x8868;&#x8FBE;&#x5F0F;&#xFF1A;^(http|https):\/\/(ark|argo|qianfan).analysys.cn
          <br
          />
        </p>
        <p>&#x5339;&#x914D;&#x4EE5;&#x4E0B;&#x9875;&#x9762;&#xFF1A;
          <br />http://ark.analysys.cn
          <br />http://argo.analysys.cn
          <br />https://ark.analysys.cn
          <br />https://ark.analysys.cn/?utmsource=xx
          <br />https://qianfan.analysys.cn
          <br />
        </p>
        <p>&#x4E0D;&#x80FD;&#x5339;&#x914D;&#x4EE5;&#x4E0B;&#x9875;&#x9762;&#xFF1A;
          <br
          />https://www.analysys.cn</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">&#x6307;&#x5B9A;&#x5934;&#x90E8;&#x5339;&#x914D;&#x89C4;&#x5219;</td>
      <td
      style="text-align:left">
        <p>&#x8868;&#x8FBE;&#x5F0F;&#xFF1A;^http:\/\/analysys\.cn\/blog.*$
          <br />
        </p>
        <p>&#x5339;&#x914D;&#x4EE5;&#x4E0B;&#x9875;&#x9762;&#xFF1A;
          <br />http://analysys.cn/blog
          <br />http://analysys.cn/blog/
          <br />http://analysys.cn/blog/1194
          <br />http://analysys.cn/blog/1194#xxx</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">&#x5339;&#x914D; http &#x6216; https&#x5F00;&#x5934;&#x94FE;&#x63A5;&#xFF0C;&#x4E5F;&#x53EF;&#x5339;&#x914D;&#x4E0D;&#x5305;&#x542B;www&#x6216;&#x5C06;www&#x66FF;&#x6362;&#x6210;&#x5176;&#x5B83;&#x7531;&#x5B57;&#x6BCD;&#x6570;&#x5B57;&#x7EC4;&#x5408;&#x7684;&#x5B50;&#x57DF;&#x540D;&#x7684;&#x60C5;&#x51B5;</td>
      <td
      style="text-align:left">
        <p>&#x8868;&#x8FBE;&#x5F0F;&#xFF1A;^(http|https):\/\/[a-z0-9]*[.]*analysys.cn\/view\/sign\/signup$
          <br
          />
        </p>
        <p>&#x5339;&#x914D;&#x4EE5;&#x4E0B;&#x9875;&#x9762;&#xFF1A;
          <br />http://www.analysys.cn/view/sign/signup
          <br />https://www.analysys.cn/view/sign/signup
          <br />http://analysys.cn/view/sign/signup
          <br />http://ark.analysys.cn/view/sign/signup
          <br />https://analysys.cn/view/sign/signup</p>
        </td>
    </tr>
    <tr>
      <td style="text-align:left">&#x5339;&#x914D;&#x672B;&#x5C3E;&#x6570;&#x636E;&#x5FC5;&#x987B;&#x5728;&#xFF08;10603&#x5230;10782&#x4E4B;&#x95F4;&#xFF09;</td>
      <td
      style="text-align:left">
        <p>&#x8868;&#x8FBE;&#x5F0F;&#xFF1A;^http:\/\/analysys.cn\/blog\/10([6][1-9][0-9]|[6][0][3-9]|[7][0-7][0-9]|[7][8][0-2])$</p>
        <p>
          <br />&#x5339;&#x914D;&#x4EE5;&#x4E0B;&#x9875;&#x9762;&#xFF1A;
          <br />http://analysys.cn/blog/10603</p>
        <p>http://analysys.cn/blog/10666
          <br />http://analysys.cn/blog/10782</p>
        <p>
          <br />&#x4E0D;&#x80FD;&#x5339;&#x914D;&#x4EE5;&#x4E0B;&#x9875;&#x9762;&#xFF1A;
          <br
          />http://analysys.cn/blog/10884
          <br />http://analysys.cn/blog/10103
          <br />
        </p>
        </td>
    </tr>
  </tbody>
</table>