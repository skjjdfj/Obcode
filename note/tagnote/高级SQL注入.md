---
tags:
  - 笔记
  - sql注入
---
## 二阶sql注入

二阶 SQL 注入（也称为存储 SQL 注入）利用了以下漏洞：用户提供的输入被保存并随后在应用程序的不同部分使用，可能在一些初始处理之后。这种类型的攻击更加隐蔽，因为恶意 SQL 代码不需要立即导致 SQL 语法错误或其他明显问题，这使得它更难使用标准输入验证技术进行检测。当数据被检索并在 SQL 命令中使用时，第二次使用数据时，会发生注入，因此名称为 “**Second Order**”。

