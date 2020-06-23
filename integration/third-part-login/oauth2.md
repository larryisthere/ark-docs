# OAuth2.0登录

方舟从 5.0 版本开始集成了 OAuth2.0 授权登录认证，具体流程如下：

1. 用户打开方舟系统的登录页面，点击 “OAuth 登录” 按钮（仅配置后才有该按钮），浏览器会跳转到配置的登录页面（OAuth2.0 认证服务端提供的登录页面）
2. 用户在认证服务端上完成登录认证后（首次将请求用户授权方舟获取必要的用户资料），完成授权后将跳转回方舟系统。

{% hint style="info" %}
1、由于方舟系统是具有平台成员和项目成员两级权限设计，所以第一次通过OAuth2授权登录的用户，会同步用户信息到方舟平台权限体系。

2、通过OAuth2授权登录的用户，需要项目管理员在方舟中对该用户分配相应的项目权限，这样该用户就能在授权登录后，能够直接访问授权的项目。

3、授权码模式\(Authentication Code\)是OAuth2.0各种认证模式中功能最完整、流程最严密的授权模式

具体信息可以参考文档：[易观方舟-平台管理](../../features/enterprise-management/)
{% endhint %}

## 1. 配置步骤

方舟接入OAuth2服务认证，根据方舟部署情况（是全新部署还是升级到5.0\)存在两种情况，配置步骤上部分差异：

1. [全新部署的5.0 及以上版本](oauth2.md#11-quan-xin-bu-shu-5-0-yi-shang-ban-ben-de-pei-zhi-bu-zhou)
2. [从低版本升级到5.0 及以上](oauth2.md#12-sheng-ji-bu-shu-5-0-yi-shang-ban-ben-de-pei-zhi-bu-zhou)

### 1.1 全新部署5.0+版本的配置步骤

1）打开`Ambari`管理平台登录（http://{ARK1\_HOST}:8080/，具体地址请联系方舟项目经理）

![armbar&#x767B;&#x5F55;&#x754C;&#x9762;](../../.gitbook/assets/image%20%28363%29.png)

2）打开方舟应用服务 `Ark Web` 参数配置中心：

登录后，在`Ambari`主界面，左侧服务列表中，找到 `Ark Web`服务，点击进入，在右侧内容页，选择`Configs` 标签卡，最后点开 `Advanced ark-web-app-api-server` 参数配置项，操作如下图所示：

如下图所示：

![ark web &#x914D;&#x7F6E;&#x9875;&#x9762;](../../.gitbook/assets/image%20%28378%29.png)

3）填写OAuth2 参数配置项，保存修改好的参数

下滑页面，在`Advanced ark-web-app-api-server`的类别下找到`arkweb.oauth2` 和 `security.oauth2` 开头的参数，按照OAuth2服务端情况填写正确的OAuth2接入配置，确保配置无误后，点击右上角 `Save`按钮进行保存。操作步骤如下：

![](../../.gitbook/assets/image%20%28376%29.png)

_备注：图示中的配置项已有部分变更，所有配置项以第二节 《配置项说明表格》为准_

4）重启方舟`Ark Web` 服务，启动OAuth2第三方登录

确认OAuth2参数配置正确配置和保存后，重启`Ark Web` 服务，使OAuth2第三方登录配置生效。操作步骤如下：

![](../../.gitbook/assets/image%20%28374%29.png)

### 1.2 升级部署5.0+版本的配置步骤

若从5.0 以下版本升级上来的，在`Ambari`参数配置中心的`Advanced ark-web-app-api-server`类别中是找不到OAuth2相关配置项，这时候需要手动在`Custom ark-web-app-api-server` 中添加 配置项，即可生效，操作步骤如下：

1）打开自定义参数配置中心，添加自定义属性

在`Ambari`主界面，左侧服务列表中，找到 `Ark Web`服务，点击进入，在右侧内容页，选择`Configs` 标签卡，最后点开 `Custom ark-web-app-api-server` 参数配置项，点击 `Add Property`，操作如下图所示：

![](../../.gitbook/assets/image%20%28364%29.png)

2）添加OAuth2配置参数

在弹出的浮窗中，可以批量添加参数配置，一行对应一个参数设置，参数配置可以复制：

```text
# 请根据OAuth2服务器情况变更以下参数
arkweb.oauth2.enable=true
arkweb.oauth2.url.base_url=http://ark2:4005
arkweb.oauth2.user_info.name_key=preferred_username
security.oauth2.client.scope=all
security.oauth2.client.client_id=abcd1234
security.oauth2.client.client_secret=XYZ1234
security.oauth2.resource.user-info-uri=https://api.github.com/user
security.oauth2.client.access-token-uri=https://github.com/login/oauth/access_token
security.oauth2.client.user-authorization-uri=https://github.com/login/oauth/authorize
```

操作步骤如下图：

![](../../.gitbook/assets/image%20%28365%29.png)

3）保存配置项

再次检查参数是否正确配置，如果配置错误 ，在下面页面也是可以进行修改，确认无误，一定要记得点击右上角`Save`按钮进行保存。操作步骤如下：

![](../../.gitbook/assets/image%20%28373%29.png)

4）保存后，重启方舟`Ark Web` 服务

确认OAuth2参数配置正确配置和保存后，重启`Ark Web` 服务，使OAuth2第三方登录配置生效。操作步骤如下：

![](../../.gitbook/assets/image%20%28370%29.png)

## 2. 配置项说明

| **参数** | 是否必填 | 参数说明 |
| :--- | :--- | :--- |
| arkweb.oauth2.enable | Y | 是否开启OAuth2认证，开启则需要设置为true，默认为false |
| arkweb.oauth2.url.base\_url | N | 方舟产品主页的 根域名 |
| arkweb.oauth2.user\_info.name\_key | N | 在获取用户信息中，使用哪个属性作为方舟用户必需username。 默认为：preferred\_username |
| security.oauth2.client.scope | Y | 获取用户信息需要的权限列表 |
| security.oauth2.client.client\_id | Y | OAuth 2.0 服务端为方舟授权登录配置的客户端ID |
| security.oauth2.client.client\_secret | Y | OAuth 2.0 服务端为方舟授权登录配置的客户端密钥 |
| security.oauth2.resource.user-info-uri | N | OAuth 2.0 获取该用户信息的接口地址 |
| security.oauth2.client.access-token-uri | N | OAuth 2.0 获取 Access Token 的地址 |
| security.oauth2.client.user-authorization-uri | N | OAuth 2.0 的授权地址 |

