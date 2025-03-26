---
tags:
  - 技巧
  - 提权
  - Linux
---
### 系统信息

```bash
uname -a
```

### 进程和服务信息
```bash
ps aux
ps -ef 
top 
cat /etc/services
```

**以root运行的进程**
```bash
ps aux | grep root 
ps -ef | grep root
```

### 计划任务
```bash
crontab -l
ls -alh /var/spool/cron
ls -al /etc/ | grep cron
ls -al /etc/cron*
cat /etc/cron*
cat /etc/at.allow
cat /etc/at.deny
cat /etc/cron.allow
cat /etc/cron.deny
cat /etc/crontab
cat /etc/anacrontab
cat /var/spool/cron/crontabs/root 

```
### 文件信息

**一般文件**
```bash
ls -la
```

**对于可执行文件**
```bash
strings FILE
```

