---
title: frp 配置并设为启动项
date: 2021-11-26 16:09:21
tags:
---

## 一般配置

```ini
; frps.ini
[common]
bind_port = 7000
token = <token>
vhost_http_port = 8080

dashboard_port = 7900
dashboard_user = <user>
dashboard_pwd = <pwd>
```

- bind_port 是 frp 服务端、客户端连接时使用的端口号。
- token 是用来鉴权的。
- vhost_http_port 是下面配置的 [web] 服务穿透出来的端口，可以通过访问公网 vps 的这个端口访问到 [web]。
- dashboard 是在 vps 上开启了一个 web 管理页面。

```ini
; frpc.ini
[common]
server_addr = <ip_addr>
server_port = 7000
token = <token>

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 6000

; 让外网可以访问到内网的某个 Web 服务
[web]
type = http
local_ip = 127.0.0.1
local_port = 80
custom_domains = <domain>

; Remote Desktop Protocol
[rdp]
type = tcp
local_ip = 127.0.0.1
local_port = 3389
remote_port = 3389
```

- token 必须和服务端相同。
- [ssh] 的 remote_port 是在公网 vps 上监听了 6000 端口，通过这个端口就可以访问到 [ssh] 设定好的 local_port。
- [web] 的 custom_domains 可以是本机 host 文件静态解析的域名。

## stcp

上面 tcp 仅需要 vps 配置好 fprs 客户端使用 frpc 连接就好。而 stcp 多了一步验证，vps 中的 frps.ini 配置还是和上面相同

访问端---vps---服务端

```ini
; B: 被访问的机器 frpc.ini
[secret_ssh]
type = stcp
role = server
sk = <secret_key>
local_ip = 127.0.0.1
local_port = 22
```

- sk 值的类型是 string。
- local_ip 与 local_port 是打算穿透出去的内网服务。

```ini
; A: 访问者的 frpc.ini
[secret_ssh_visitor]
type = stcp
role = visitor
server_name = secret_ssh
sk = <secret_key>
bind_addr = 127.0.0.1
bind_port = 6000
```

- server_name 就是上面被访问机器的服务名称，这里就是 [secret_ssh] 中的 secret_ssh。
- 服务正确运行后，访问本机 (B) 的 127.0.0.1:6000 的话就会转发到远端机器 (A) 的 127.0.0.1:22

## 设为启动项

1. 将 frp 软件目录 systemd 文件夹下对应的配置文件放入`/etc/systemd/system/`
2. 并修改配置文件中的路径
3. systemctl enable 添加自启  
systemctl disable 取消自启
