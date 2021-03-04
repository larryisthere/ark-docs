# JAVA工具包

## 1.介绍说明

        工具集将数据导入、数据导出等查询功能集成在一起，不需要安装多个程序完成不同的需求，提升了用户体验。

{% hint style="info" %}
1、提供统一的用户界面，用户能用规范统一的命令调用工具集中的工具

2、安装部署简单，通过rpm命令自动完成程序部署。
{% endhint %}

## 2.安装部署

### 2.1.安装

下载地址：

```text
https://www.analysysdata.com/tool/ark_tools-1.0.7-1.el7.noarch.rpm
```

linux服务器下载工具包命令： 

```text
wget https://www.analysysdata.com/tool/ark_tools-1.0.7-1.el7.noarch.rpm
```

通过外网下载或者上传工具集ark\_tools-1.0.7-1.el7.noarch.rpm到ark1、ark2、ark3任意一台服务器上。

在linux环境中将路径切换到工具集ark\_tools-1.0.7-1.el7.noarch.rpm所在目录，执行以下命令： 

```text
rpm -ivh ark_tools-1.0.7-1.el7.noarch.rpm
```

### 2.2.配置环境变量

将/usr/local/ark\_tools/bin/路径下的env.sh加入到环境变量中。

```text
source /usr/local/ark_tools/bin/env.sh
```

## 3.使用说明

### 3.1.父命令

所有子命令的执行，都由以下模式组成：

```text
arksh 子命令 子命令参数
```

### 3.2.参数说明

| 参数名 | 参数示例 | 参数说明 | 是否必传 |
| :--- | :--- | :--- | :--- |
| --help/-h | 无 | 列出arksh命令帮助文档 | 否 |
| --list/-l | 无 | 显示支持的所有子命令 | 否 |

### 3.3.示例展示

列出arksh命令帮助文档

```text
arksh --help
```

