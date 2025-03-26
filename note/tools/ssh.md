---
tags:
  - 技巧
  - ssh
---

有时ssh连接会出问题，所以这里来总结一些解决办法

使用`-v`进入详细模式，查看哪里出问题了。


```
-o ProxyCommand='pv -qL 1K | nc %h %p | pv -qL 1K'
```

来源：https://unix.stackexchange.com/questions/722954/ssh-stuck-at-expecting-ssh2-msg-kex-ecdh-reply

```
-o KexAlgorithms=ecdh-sha2-nistp521
```
来源：https://unix.stackexchange.com/questions/722954/ssh-stuck-at-expecting-ssh2-msg-kex-ecdh-reply

