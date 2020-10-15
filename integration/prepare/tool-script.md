# 部署环境检测工具

## 概述

检测工具主要用于检测服务器是否满足部署方舟分析的要求。 其检测项目有：

1. 系统检查\(发布版本，语言编码\)等
2. cpu 检查（核数）
3. 内存大小检查
4. 磁盘挂载使用检查
5. 磁盘个数检查
6. 网络检查是否能上外网
7. 目前监听的端口检查
8. 进程检查
9. 开机自启服务检查
10. 登录检查
11. 计划任务检查
12. 用户检查
13. 密码检查
14. Sudoers 检查
15. jdk 检查
16. 防火墙检查
17. ssh 检查
18. syslog 检查
19. SNMP 检查
20. 软件检查

## 执行检测

sh check.sh 

[点击下载脚本](http://install.ark.analysys.cn:8080/ark/check.sh)

## 查看检测结果

./log/InstallCheck-xxxxx-2020xxxx.txt

{% hint style="info" %}
以上内容没有解答我的问题？[点击我进入方舟论坛去反馈](https://www.analysysdata.com/forum/index) 🚀
{% endhint %}

