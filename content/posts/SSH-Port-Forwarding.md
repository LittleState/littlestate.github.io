---
title: SSH Port Forwarding
date: 2021-11-23 22:31:45
tags:
---

## 动态转发
因为 SSH 服务器要去访问哪一个网站，完全是动态的，取决于原始通信，动态转发只需要把本地端口绑定到 SSH 服务器，所以叫动态转发。

```bash
$ ssh -D port tunnel-host -N
```

- D 表示动态转发，所有经过这个本地监听端口的流量，都通过 SSH 隧道。

- N 参数用于端口转发，表示建立的 SSH 只用于端口转发，不能执行远程命令，这样可以提供安全性。

## 本地转发（正向代理）
![cpf](simple-tunnel.png)

```bash
$ ssh -L port:host:hostport tunnel-host -N
```

上面就是将 tunnel-host 作为跳板机，在本地 port 端口与目标服务器（这里是 host）的 hostport　端口之间建立了一个 ssh 隧道。

```bash
$ ssh -L 8080:www:80 tunnel-host -N
```

- 访问 clienthost 8080 端口时，就相当于通过跳板服务器 tunnel-host 访问 www:80。

- www:80 可以是跳板机局域网中的一个设备。

## 远程转发（反向代理）

![rpf](rpf.jpg)

```bash
$ ssh -R port:host:hostport tunnel-host -N
```

访问 tunnel-host:port 会访问到本地的 host:hostport，可以通过这个功能将本地服务转发出去。

```bash
$ ssh -R 8080:192.168.1.100:80 root@x.x.x.x -p 0~65535 -N
```

如上，访问公网 x.x.x.x:8080 端口的话，可以访问到内网的 192.168.1.100:80。

## 实例

### 简易 VPN

```bash
$ ssh -L 233:192.168.10.10:22 root@192.168.10.11 -N
```

- 192.168.10.10 是 64-bit kali（A），192.168.10.11 是 32-bit kali（B）。
- 192.168.10.1 是宿主机（localhost） IP。

这里举例如果 A 禁止了 localhost 的 IP 地址，使其无法使用 ssh 访问 A，模拟网络中被防火墙隔离的情况。

但是这时 B 是一台没有被封禁的机器，就可以通过上述操作从 localhost 连接到 B 建立端口转发。之后如果访问 localhost 本机的 8080 端口，就会通过 B 来访问到 A 的 22 端口，这样就绕过了无法访问的情况。

### 两级跳板

![two](two.png)

端口转发可以有多级，比如新建两个 SSH 隧道，第一个隧道转发给第二个隧道，第二个隧道才能访问目标服务器。

首先，在本机新建第一级隧道。

```bash
$ ssh -L 7999:localhost:2999 tunnel1-host
```

上面命令在本地7999端口与tunnel1-host之间建立一条隧道，隧道的出口是tunnel1-host的localhost:2999，也就是tunnel1-host收到本机的请求以后，转发给自己的2999端口。

然后，在第一台跳板机（tunnel1-host）执行下面的命令，新建第二级隧道。

```bash
$ ssh -L 2999:target-host:7999 tunnel2-host -N
```

上面命令将第一台跳板机tunnel1-host的2999端口，通过第二台跳板机tunnel2-host，连接到目标服务器target-host的7999端口。

最终效果就是，访问本机的7999端口，就会转发到target-host的7999端口。

## config

```bash
# ~/.ssh/config
Host *
     Port 2222

Host remoteserver
     HostName remote.example.com
     User neo
     Port 2112
```

```bash
$ ssh remoteserver
# 等同于
$ ssh -p 2112 neo@remote.example.com
```

参考：

- [SSH 端口转发，阮一峰](https://wangdoc.com/ssh/port-forwarding.html)
- [An Illustrated Guide to SSH Tunnels, Scott Wiersdorf](https://solitum.net/posts/an-illustrated-guide-to-ssh-tunnels/)
- [SSH 远程端口转发，小马过河](https://lvii.github.io/system/2013-10-08-ssh-remote-port-forwarding/)

附录

```bash
/etc/hosts.allow
/etc/hosts.deny
```
