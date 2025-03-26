---
tags:
  - 漏洞
  - 走私
---

# HTTP

http请求走私大致就是这个样子

![[Snipaste_2024-10-16_21-31-39.png]]
### CL.TE 

**CL.TE** 代表 **Content-Length/Transfer-Encoding**。**CL.TE** 这个名字来自两个涉及的头文件：**Content-Length** 和 **Transfer-Encoding**。在 CL.TE 技术中，攻击者利用不同服务器（通常是前端和后端服务器）确定这些标头优先级的方式之间的差异。例如：
- 代理使用 Content-Length 标头来确定请求的结束。
- 后端服务器使用 Transfer-Encoding 标头。


### TE.CL

**TE.CL** 代表 **Transfer-Encoding/Content-Length**。此技术与 CL.TE 方法相反。在 TE.CL 方法，标头解释的差异被翻转，因为前端服务器使用 Transfer-Encoding 标头来确定请求的结束，而后端服务器使用 Content-Length 标头。

The TE.当代理优先考虑 `Transfer-Encoding` 标头，而后端服务器优先考虑 `Content-Length` 标头时，就会出现 CL 技术。

### TE.TE

**Transfer Encoding Obfuscation，**也称为 **TE.TE** 代表 **Transfer-Encoding/Transfer-Encoding**。与 CL.TE 或 TE.CL 方法，则当前端和后端服务器都使用 Transfer-Encoding 标头时，就会出现 TE.TE 技术。在 TE.TE 技术中，攻击者利用服务器对  HTTP 标头中存在的 Transfer-Encoding 的不一致处理。

TE.TE 漏洞并不总是需要多个 Transfer-Encoding 标头。相反，它通常涉及一个格式错误的 Transfer-Encoding 标头，前端和后端服务器对该标头的解释不同。在某些情况下，前端服务器可能会忽略或去除 Header 的格式错误部分并正常处理请求，而后端服务器可能会因为 Header 格式错误而对请求进行不同的解释，从而导致请求走私。


# HTTP2


# WebSockets



# 异步


