# LDAP登录

方舟从 5.0 版本开始集成了 LDAP 登录认证，登录流程如下：

1. 认证参数配置；
2. 用户在方舟系统中正常使用LDAP系统的用户名和密码进行登录；
3. 方舟系统会使用用户填写的用户名和密码去后台配置的 LDAP 系统中进行身份认证，认证成功之后用户就可以正常登录使用方舟系统。

{% hint style="info" %}
1、由于方舟系统是具有平台成员和项目成员两级权限设计，所以第一次通过LDAP认证登录的用户，会同步用户信息到方舟平台权限体系。

2、通过LDAP认证登录的用户，需要项目管理员在方舟中对该用户分配相应的项目权限，这样该用户就能在LDAP认证登录后，能够直接访问授权的项目。

具体信息可以参考文档：[易观方舟-平台管理](../../features/enterprise-management/)
{% endhint %}

## 1. 配置步骤

方舟接入LDAP服务认证，根据方舟部署情况（是全新部署还是升级到5.0\)存在两种情况，配置步骤上部分差异：

1. [全新部署的5.0 及以上版本](ldap.md#11-quan-xin-bu-shu-5-0-ban-ben-de-ldap-pei-zhi-bu-zhou)
2. [从低版本升级到5.0 及以上](ldap.md#12-sheng-ji-bu-shu-5-0-ban-ben-de-ldap-pei-zhi-bu-zhou)

### 1.1 全新部署5.0+ 版本的LDAP 配置步骤

1）打开`Ambari`管理平台登录（http://{ARK1\_HOST}:8080/，具体地址请联系方舟项目经理）

![ambari login](../../.gitbook/assets/image%20%28380%29.png)

2）打开方舟应用服务 `Ark Web` 参数配置中心：

登录后，在`Ambari`主界面，左侧服务列表中，找到 `Ark Web`服务，点击进入，在右侧内容页，选择`Configs` 标签卡，最后点开 `Advanced ark-web-app-api-server` 参数配置项，操作如下图所示：

如下图所示：

![](../../.gitbook/assets/image%20%28367%29.png)

3）填写LDAP 参数配置项，保存修改好的参数

下滑页面，在`Advanced ark-web-app-api-server`的类别下找到`arkweb.ldap` 开头的参数，按照LDAP服务情况填写正确的LDAP接入配置，确保配置无误后，点击右上角 `Save`按钮进行保存。操作步骤如下：

![](../../.gitbook/assets/image%20%28362%29.png)

4）重启方舟`Ark Web` 服务，启动LDAP第三方登录

确认LDAP参数配置正确配置和保存后，重启`Ark Web` 服务，使LDAP第三方登录配置生效。操作步骤如下：

![](../../.gitbook/assets/image%20%28381%29.png)

### 1.2 升级部署5.0+ 版本的LDAP 配置步骤

若从5.0 以下版本升级上来的，在`Ambari`参数配置中心的`Advanced ark-web-app-api-server`类别中是找不到LDAP相关配置项，这时候需要手动在`Custom ark-web-app-api-server` 中添加 配置项，即可生效，操作步骤如下：

1）打开自定义参数配置中心，添加自定义属性

在`Ambari`主界面，左侧服务列表中，找到 `Ark Web`服务，点击进入，在右侧内容页，选择`Configs` 标签卡，最后点开 `Custom ark-web-app-api-server` 参数配置项，点击 `Add Property`，操作如下图所示：

![](../../.gitbook/assets/image%20%28366%29.png)

2）添加LDAP配置参数

在弹出的浮窗中，可以批量添加参数配置，一行对应一个参数设置，参数配置可以复制：

```text
# 请根据LDAP服务器情况变更以下参数
arkweb.ldap.enabled = true
arkweb.ldap.urls= ldap://192.168.10.30:389/
arkweb.ldap.baseDn= dc=analysys,dc=com
arkweb.ldap.userDnPattern = cn=${username}
arkweb.ldap.admin = true
arkweb.ldap.adminUsername= cn=Manager,dc=analysys,dc=com
arkweb.ldap.adminPassword= 123456
arkweb.ldap.searchFilter=
arkweb.ldap.emailSuffix=@analysys.com.cn
```

操作步骤如下图：

![](../../.gitbook/assets/image%20%28371%29.png)

3）保存配置项

再次检查参数是否正确配置，如果配置错误 ，在下面页面也是可以进行修改，确认无误，一定要记得点击右上角`Save`按钮进行保存。操作步骤如下：

![](../../.gitbook/assets/image%20%28368%29.png)

4）保存后，重启方舟`Ark Web` 服务

确认LDAP参数配置正确配置和保存后，重启`Ark Web` 服务，使LDAP第三方登录配置生效。操作步骤如下：

![](../../.gitbook/assets/image%20%28375%29.png)

## 2. 配置项说明

| **参数** | 是否必填 | 参数说明 |
| :--- | :--- | :--- |
| arkweb.ldap.enabled | Y | 是否开启LDAP认证，开启则需要设置为true，默认为false |
| arkweb.ldap.urls | Y | LDAP 服务的地址，ldap://ip:port 格式 |
| arkweb.ldap.baseDn | Y | LDAP Base DN，进行LDAP 用户名检索的 Base Dn。例如：`ou=people,dc=example,dc=com` |
| arkweb.ldap.userDnPattern | Y | 登录的用户名格式，常用的例如：`uid=${username}` 或 `​cn=${username}@email.com` |
| arkweb.ldap.admin | Y | 当 LDAP 配置了禁止匿名访问时，需要设置为 true ，并配置管理员信息 |
| arkweb.ldap.adminUsername | N | 当 LDAP 配置了禁止匿名访问时，需要绑定的管理员账号 |
| arkweb.ldap.adminPassword | N | 当 LDAP 配置了禁止匿名访问时，需要绑定的管理员密码 |
| arkweb.ldap.searchFilter | N | 在 LDAP 中查找用户时是否按照指定的 filter 进行筛选 |
| arkweb.ldap.emailSuffix | Y | 登录后的用户名添加邮箱后缀。方舟中用户邮箱是必须要求的 |

