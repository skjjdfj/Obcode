---
tags:
  - 工具
  - 指纹识别
  - 信息收集
---

# 安装

```shell-session
Turtlera@htb[/htb]$ sudo apt update && sudo apt install -y perl
Turtlera@htb[/htb]$ git clone https://github.com/sullo/nikto
Turtlera@htb[/htb]$ cd nikto/program
Turtlera@htb[/htb]$ chmod +x ./nikto.pl
```


# 指纹识别

```bash
nikto -h inlanefreight.com -Tuning b
```

`-h` 标志指定目标主机。`-Tuning b` 标志告诉 `Nikto` 只运行软件识别模块。

