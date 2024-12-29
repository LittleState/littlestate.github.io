---
title: 通过公网 IP 使用 RDP 与 WOL
date: 2021-11-28 17:57:57
tags:
---

## frp

在还没有公网 IP 时，通过 frp 将 3389 端口映射出来，通过 RDP 访问到主机后进行以下的操作。

## DDNS

1. 首先在红米 AC2100 中支持的 DDNS 服务商中选择了 no-ip.com。
2. 注册并将配置好的 Hostname 填写到路由器的 DDNS 中，用户名就是注册 no-ip.com 时的。

## 远程开机

上面配置完后应该就可以通过域名来远程控制主机了，但是还没法远程开机。

1. 开启主板 BIOS 中的 wake on lan。
2. 勾选电脑网卡中的以下选项  
![wol](wol.png)
3. 开启以下选项  
![wol](wol2.png)
4. 关闭电源中的快速启动（重要！）  
![wol](wol3.png)

## 给路由器添加静态 arp

编辑 /etc/rc.local在exit的上一行加入arp -s 192.168.31.XXX xx:xx:xx:xx:xx:xx就可以了。

参考：

- [AC2100官方固件开启SSH方法](https://www.right.com.cn/forum/thread-4032490-1-1.html)
- [小米路由器成功外网WOL唤醒](https://blog.haitianhome.com/miwifi-wol-waiwang.html)
- [小米、红米 AC2100 一键开启 SSH，可自定义安装各种插件](https://zhuanlan.zhihu.com/p/260531160)
