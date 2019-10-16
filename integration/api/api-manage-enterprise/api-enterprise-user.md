# 成员管理

## 1. 注册企业用户

方舟企业管理后台支持多个项目成员共享，在添加项目成员前需要先注册用户。

### 1.1 接口地址

> 【POST】 /uba/manage/enterprise/accounts

### 1.2 请求参数示例

```java
{
    //【必填】邮箱地址
    "email": "api@analysys.com.cn",
    //用户名
    "userName": "api",
    //真实姓名
    "name": "接口用户",
    //【必填】密码 
    "password": "123456",
    // 【必填】手机号码
    "phone": "1380000000",
    //用户所属部门
    "department": "技术部"
}
```

> **角色**：通过接口添加的用户角色默认为平台成员。
>
> **认证参数**：接口必传 xtoken 参数，详情见 [平台接口认证](../#22-ping-tai-jie-kou-ren-zheng)。
>
> **操作用户**：接口如果要记录操作人，URL上带loginUser参数，详情见 [操作用户](../#51-cao-zuo-yong-hu)。

#### 1.2.1 入参说明：

| 参数名称 | 类型 | 必填 | 说明 |
| :--- | :--- | :--- | :--- |
| email | String | Y | 邮箱 不能重复 可用于登录 |
| userName | String | N | 用户名：不能重复 可用于登录 |
| name | String | N | 真实姓名：常用于展示 |
| password | String | Y | 登录密码 |
| phone | String | N | 手机号码：不能重复 可用于登录 |
| department | String | N | 用户所属部门 |

### 1.3 返回结果示例

```java
{
    //用户新增成功会返回方舟产品中对应的用户ID
    "userId":1,
    //邮箱地址
    "email":"api@analysys.com.cn"
}
```

### 1.4 接口调用示例

```java
curl -H "Content-Type:application/json" -H "xtoken:9CF0444E9DFD9E3D9CAE49B79F939B61" -X POST --data '{
    "email": "api@analysys.com.cn",
    "userName": "api",
    "name": "接口用户",
    "password": "123456",
    "phone": "1380000000",
    "department": "技术部"
}' http://127.0.0.1:4005/uba/manage/enterprise/accounts?loginUser=admin@analysys.com.cn
```

## 2. 禁用/启用企业用户

禁用企业用户不会将用户记录删除，只会修改为不可用，禁用只会的帳号将不能登录方舟系统，如果需要继续使用，需要调用接口恢复用户状态。

### 2.1 接口地址

> 【POST】 /uba/manage/ enterprise/accounts/activation

### 2.2 请求参数示例

```java
{
    //【必填】状态：false为禁用 true为启用
    "activation": false
}
```

> **认证参数**：接口必传 xtoken 参数，详情见 [平台接口认证](../#22-ping-tai-jie-kou-ren-zheng)。
>
> **操作用户**：接口如果要记录操作人，URL上带loginUser参数，详情见 [操作用户](../#51-cao-zuo-yong-hu)。

#### 2.2.1 入参说明：

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
      <td style="text-align:left">email</td>
      <td style="text-align:left">String</td>
      <td style="text-align:left">Y</td>
      <td style="text-align:left">&#x8981;&#x7981;&#x7528;/&#x542F;&#x7528;&#x7684;&#x7528;&#x6237;&#x90AE;&#x7BB1;&#xFF0C;&#x5907;&#x6CE8;&#xFF1A;URL&#x4F20;&#x53C2;</td>
      <td
      style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">activation</td>
      <td style="text-align:left">Boolean</td>
      <td style="text-align:left">Y</td>
      <td style="text-align:left">false&#x4E3A;&#x7981;&#x7528;&#xFF0C;true&#x4E3A;&#x542F;&#x7528;&#x3002;&#x5907;&#x6CE8;&#xFF1A;Body&#x53C2;&#x6570;</td>
      <td
      style="text-align:left">
        <p>true</p>
        <p>false</p>
        </td>
    </tr>
  </tbody>
</table>### 2.3 返回结果示例

```java
{
    //0 表示返回成功
    "success":0
}
```

### 2.4 接口调用示例

```java
curl -H "Content-Type:application/json" -H "xtoken:9CF0444E9DFD9E3D9CAE49B79F939B61" -X POST --data '{
    "activation": false
}' 'http://127.0.0.1:4005/uba/manage/enterprise/accounts/activation?loginUser=admin@analysys.com.cn&email=api@analysys.com.cn'
```

