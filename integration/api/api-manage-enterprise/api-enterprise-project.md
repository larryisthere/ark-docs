# 项目管理

## 1. 获取项目列表

### 1.1 接口地址

> 【GET】 /uba/manage/enterprise/projects

### 1.2 请求参数示例

无

> **认证参数**：接口必传 xtoken 参数，详情见 [平台接口认证](../#22-ping-tai-jie-kou-ren-zheng)。
>
> **操作用户**：接口如果要记录操作人，URL上带loginUser参数，详情见 [操作用户](../#51-cao-zuo-yong-hu)。

### 1.3 返回结果示例

```java
[
    {
        "appKey": "streamingut608",
        "normalToken":"xxxx",
        "cname": "streaming单元测试项目",
        "version": "4.2.7",
        //以下字段在【4.5.1版本】中新增
        "logo": "/data/static/files/logo/aaa.png",
        "createTime": 1578901233445,
        "sdkBack": "iOS,Android,H5",
        "status": 1,
        "partitionNum": 3,
        "stream": 0
    }
]
```

#### 1.3.1 出参说明：

| 参数名称 | 类型 | 说明 |
| :--- | :--- | :--- |
| appKey | String | 项目AppKey |
| normalToken | String | 项目内接口授权码 |
| cname | String | 项目名称，用于展示 |
| version | String | 项目当前版本信息 |
| logo | String | 项目图标访问路径，默认不显示，只有上传logo后显示 |
| createTime | Long | 十三位时间戳 |
| sdkBack | String | 已回数SDK接入平台\(如 Java,JS,Android \)，未回数时不显示 |
| status | int | 项目状态；1表示正常，2表示已删除，3表示异常不可用，只返回状态值1的项目 |
| partitionNum | int | 数据存储分区数 |
| stream | int | 项目数据数据流状态；0表示未启动，1表示启动中，2表示已启动，3表示异常中断，5表示关闭中 |

### 1.4 接口调用示例

```java
curl -H "Content-Type:application/json" -H "xtoken:9CF0444E9DFD9E3D9CAE49B79F939B61" -X GET http://127.0.0.1:4005/uba/manage/enterprise/projects
```

## 2. 获取单个项目详细

### 2.1 接口地址

> 【GET】 /uba/manage/enterprise/projects/{appKey}

### 2.2 请求参数示例

```java
无
```

> **认证参数**：接口必传 xtoken 参数，详情见 [平台接口认证](../#22-ping-tai-jie-kou-ren-zheng)。
>
> **操作用户**：接口如果要记录操作人，URL上带loginUser参数，详情见 [操作用户](../#51-cao-zuo-yong-hu)。

#### 2.2.1 入参说明：

| 参数名称 | 类型 | 必填 | 说明 |
| :--- | :--- | :--- | :--- |
| appKey | String | Y | 项目App标识，备注：URL传参 |

### 2.3 返回结果示例

```java
{
    "appKey":"streamingut608",
    "cname":"streaming单元测试项目",
    "version":"4.2.7",
    "logo":"/data/static/files/logo/aaa.png",
    "createTime":1578901233445,
    "sdkBack":"iOS,Android,H5",
    "status":1,
    "partitionNum":3,
    "stream":0
}
```

#### 2.3.1 出参说明：

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;&#x79F0;</th>
      <th style="text-align:left">&#x7C7B;&#x578B;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">appKey</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&#x9879;&#x76EE;AppKey</td>
    </tr>
    <tr>
      <td style="text-align:left">cname</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&#x9879;&#x76EE;&#x540D;&#x79F0;&#xFF0C;&#x7528;&#x4E8E;&#x5C55;&#x793A;</td>
    </tr>
    <tr>
      <td style="text-align:left">version</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&#x9879;&#x76EE;&#x5F53;&#x524D;&#x7248;&#x672C;&#x4FE1;&#x606F;</td>
    </tr>
    <tr>
      <td style="text-align:left">logo</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&#x9879;&#x76EE;&#x56FE;&#x6807;&#x8BBF;&#x95EE;&#x8DEF;&#x5F84;&#xFF0C;&#x9ED8;&#x8BA4;&#x4E0D;&#x663E;&#x793A;&#xFF0C;&#x53EA;&#x6709;&#x4E0A;&#x4F20;logo&#x540E;&#x663E;&#x793A;</td>
    </tr>
    <tr>
      <td style="text-align:left">createTime</td>
      <td style="text-align:left">Long</td>
      <td style="text-align:left">&#x5341;&#x4E09;&#x4F4D;&#x65F6;&#x95F4;&#x6233;</td>
    </tr>
    <tr>
      <td style="text-align:left">sdkBack</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&#x5DF2;&#x56DE;&#x6570;SDK&#x63A5;&#x5165;&#x5E73;&#x53F0;(&#x5982; Java,JS,Android
        )&#xFF0C;&#x672A;&#x56DE;&#x6570;&#x65F6;&#x4E0D;&#x663E;&#x793A;</td>
    </tr>
    <tr>
      <td style="text-align:left">status</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">
        <p>&#x9879;&#x76EE;&#x72B6;&#x6001;&#xFF1B;</p>
        <p>1&#x8868;&#x793A;&#x6B63;&#x5E38;&#xFF0C;2&#x8868;&#x793A;&#x5DF2;&#x5220;&#x9664;&#xFF0C;3&#x8868;&#x793A;&#x5F02;&#x5E38;&#x4E0D;&#x53EF;&#x7528;&#xFF0C;&#x53EA;&#x8FD4;&#x56DE;&#x72B6;&#x6001;&#x503C;1&#x7684;&#x9879;&#x76EE;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">partitionNum</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">&#x6570;&#x636E;&#x5B58;&#x50A8;&#x5206;&#x533A;&#x6570;</td>
    </tr>
    <tr>
      <td style="text-align:left">stream</td>
      <td style="text-align:left">int</td>
      <td style="text-align:left">&#x9879;&#x76EE;&#x6570;&#x636E;&#x6570;&#x636E;&#x6D41;&#x72B6;&#x6001;&#xFF1B;0&#x8868;&#x793A;&#x672A;&#x542F;&#x52A8;&#xFF0C;1&#x8868;&#x793A;&#x542F;&#x52A8;&#x4E2D;&#xFF0C;2&#x8868;&#x793A;&#x5DF2;&#x542F;&#x52A8;&#xFF0C;3&#x8868;&#x793A;&#x5F02;&#x5E38;&#x4E2D;&#x65AD;&#xFF0C;5&#x8868;&#x793A;&#x5173;&#x95ED;&#x4E2D;</td>
    </tr>
  </tbody>
</table>### 2.4 接口调用示例

```java
curl -H "Content-Type:application/json" -H "xtoken:9CF0444E9DFD9E3D9CAE49B79F939B61" -X GET http://127.0.0.1:4005/uba/manage/enterprise/projects/streamingut608
```

## 3. 创建新项目

### 3.1 接口地址

> 【POST】 /uba/manage/enterprise/projects

### 3.2 请求参数示例

```java
{
      "appKey":" streamingut608",
      "name":"测试项目"
}
```

> **认证参数**：接口必传 xtoken 参数，详情见 [平台接口认证](../#22-ping-tai-jie-kou-ren-zheng)。
>
> **操作用户**：接口如果要记录操作人，URL上带loginUser参数，详情见 [操作用户](../#51-cao-zuo-yong-hu)。

#### 3.2.1 入参说明：

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;&#x79F0;</th>
      <th style="text-align:left">&#x7C7B;&#x578B;</th>
      <th style="text-align:left">&#x5FC5;&#x586B;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">appKey</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">Y</td>
      <td style="text-align:left">
        <p>&#x65B0;&#x9879;&#x76EE;&#x7684;AppKey&#x6807;&#x8BC6;&#xFF0C;&#x53EA;&#x63A5;&#x53D7;&#x82F1;&#x6587;&#x6570;&#x5B57;&#x7EC4;&#x5408;&#xFF0C;&#x4E0D;&#x4F20;&#x7CFB;&#x7EDF;&#x81EA;&#x52A8;&#x751F;&#x6210;&#x3002;</p>
        <p>&#x5907;&#x6CE8;&#xFF1A;Body&#x53C2;&#x6570;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">name</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">Y</td>
      <td style="text-align:left">&#x9879;&#x76EE;&#x540D;&#x79F0;&#xFF0C;&#x53EA;&#x63A5;&#x53D7;&#x82F1;&#x6587;&#x6570;&#x5B57;&#x7EC4;&#x5408;&#x3002;</td>
    </tr>
  </tbody>
</table>### 3.3 返回结果示例

```java
{
    "appKey": "streamingut608",
    "success": 0
}
```

#### 3.3.1 出参说明：

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;&#x79F0;</th>
      <th style="text-align:left">&#x7C7B;&#x578B;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">appKey</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">&#x9879;&#x76EE;AppKey</td>
    </tr>
    <tr>
      <td style="text-align:left">success</td>
      <td style="text-align:left">Integer</td>
      <td style="text-align:left">
        <p>0&#x8868;&#x793A;&#x521B;&#x5EFA;&#x5B8C;&#x6210;&#xFF0C;&#x65E0;&#x5F02;&#x5E38;&#x3002;</p>
        <p>&#x5907;&#x6CE8;&#xFF1A;&#x56E0;&#x4E3A;&#x9879;&#x76EE;&#x521B;&#x5EFA;&#x9700;&#x8981;&#x8C03;&#x914D;&#x975E;&#x5E38;&#x591A;&#x7CFB;&#x7EDF;&#x8D44;&#x6E90;&#xFF0C;&#x6240;&#x4EE5;&#x8BE5;&#x63A5;&#x53E3;&#x4E3A;&#x5F02;&#x6B65;&#x64CD;&#x4F5C;&#xFF0C;&#x9879;&#x76EE;&#x771F;&#x6B63;&#x53EF;&#x7528;&#x72B6;&#x6001;&#x9700;&#x8981;&#x8C03;&#x7528;&#x3010;&#x9879;&#x76EE;&#x8BE6;&#x60C5;&#x3011;&#x63A5;&#x53E3;&#xFF0C;&#x5176;&#x4E2D;finish
          &#x5B57;&#x6BB5;&#x8868;&#x793A;&#x9879;&#x76EE;&#x521B;&#x5EFA;&#x60C5;&#x51B5;</p>
      </td>
    </tr>
  </tbody>
</table>### 3.4 接口调用示例

```java
curl -H "Content-Type:application/json" -H "xtoken:9CF0444E9DFD9E3D9CAE49B79F939B61" -X POST --data '{" appKey ":" streamingut608","name ":"测试项目"}'  http://127.0.0.1:4005/uba/manage/enterprise/projects
```

## 4 开启/关闭数据流

### 4.1 接口地址

> 【POST】 /uba/manage/enterprise/projects/{appKey}/stream

### 4.2 请求参数示例

```java
{
    //数据流状态 true为开启数据流 false为关闭数据流 关闭数据流功能在4.5版本中新增
    "streamSwitch": true
}
```

> **认证参数**：接口必传 xtoken 参数，详情见 平台接口认证。
>
> **操作用户**：接口如果要记录操作人，URL上带loginUser参数，详情见 操作用户。

#### 4.2.1 入参说明：

<table>
  <thead>
    <tr>
      <th style="text-align:left">&#x53C2;&#x6570;&#x540D;&#x79F0;</th>
      <th style="text-align:left">&#x7C7B;&#x578B;</th>
      <th style="text-align:left">&#x5FC5;&#x586B;</th>
      <th style="text-align:left">&#x8BF4;&#x660E;</th>
      <th style="text-align:left">&#x679A;&#x4E3E;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">appKey</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">Y</td>
      <td style="text-align:left">&#x65B0;&#x9879;&#x76EE;&#x7684;AppKey&#x6807;&#x8BC6;&#xFF0C;&#x53EA;&#x63A5;&#x53D7;&#x82F1;&#x6587;&#x6570;&#x5B57;&#x7EC4;&#x5408;&#xFF0C;&#x4E0D;&#x4F20;&#x7CFB;&#x7EDF;&#x81EA;&#x52A8;&#x751F;&#x6210;&#x3002;&#x5907;&#x6CE8;&#xFF1A;URL&#x53C2;&#x6570;</td>
      <td
      style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">streamSwitch</td>
      <td style="text-align:left">Boolean</td>
      <td style="text-align:left">Y</td>
      <td style="text-align:left">
        <p>&#x6570;&#x636E;&#x6D41;&#x72B6;&#x6001; true&#x4E3A;&#x5F00;&#x542F;&#x6570;&#x636E;&#x6D41;&#xFF0C;false&#x4E3A;&#x5173;&#x95ED;&#x6570;&#x636E;&#x6D41;&#x3002;</p>
        <p>&#x5907;&#x6CE8;&#xFF1A;&#x5173;&#x95ED;&#x6570;&#x636E;&#x6D41;&#x5728;4.5&#x7248;&#x672C;&#x4E2D;&#x624D;&#x4F1A;&#x652F;&#x6301;&#x3002;</p>
      </td>
      <td style="text-align:left">true/false</td>
    </tr>
  </tbody>
</table>### 4.3 返回结果示例

```java
{
  //0表示操作成功，数据流开启/关闭中
  "success": 0
}
```

#### 4.3.1 出参说明：

| 参数名称 | 类型 | 说明 |
| :--- | :--- | :--- |
| success | Integer | 0表示操作成功，数据流开启/关闭中。备注：因为开启数据流/关闭数据流是异步操作，接口调用只是发起了相关请求，最终的操作结果需要通过streamStatus查看最终启停状态。 |

### 4.4 接口调用示例

```java
curl -H "Content-Type:application/json" -H "xtoken:E1E967DCE07B10839F87195B78E1F5F5" -X POST --data '{
    "streamSwitch": true
}' http://127.0.0.1:4005/uba/manage/enterprise/projects/streamingut608/stream?loginUser=admin@analysys.com.cn
```

