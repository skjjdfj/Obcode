---
tags:
  - 技巧
  - openvpn
---

### socks-proxy

首先，由于gw的原因，openvpn需要配置socks代理，配置方法如下(kali是虚拟机)：
1. 查看自己本机的ip
2. 在openvpn文件中任意位置加上`socks-proxy IP PORT`端口是代理的端口，比如clash就是7890，IP是你自己主机的IP(一般自己主机装有代理软件)，使用ipconfig可以看到wlan的ip，如果是clash要打开bind模式
3. 然后`sudo openvpn FILE.ovpn`

## 检修

以下部分是如何检修openvpn，使用备忘录的方式


### 一般

如果找不到原因，重新下载一个opnvpn文件，重新配置后再次尝试

### 日志时间

一定要注意日志的时间，有时候日志时间不对会连不上
解决方法是修改系统时间(此时系统时间大概也是错的)

### IP
每次连接不上务必查看此时IP是否和配置的IP相同

### 网络

ping一个任意的ip和域名，如果ping不同，可能是没连上网
解决办法:
```bash
sudo systemctl restart networking
```
重启网络服务


