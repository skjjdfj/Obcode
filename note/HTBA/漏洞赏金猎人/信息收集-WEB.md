# 指纹

### 指纹识别技术

有几种技术可用于 Web 服务器和技术指纹识别：

- `横幅抓取`：横幅抓取涉及分析 Web 服务器和其他服务呈现的横幅。这些横幅通常会显示服务器软件、版本号和其他详细信息。
- `分析 HTTP 标头`：与每个网页请求和响应一起传输的 HTTP 标头包含大量信息。`Server` 标头通常披露 Web 服务器软件，而 `X-Powered-By` 标头可能会显示其他技术，例如脚本语言或框架。
- `Probing for Specific Responses` ：向目标发送特制请求可以引发揭示特定技术或版本的独特响应。例如，某些错误消息或行为是特定 Web 服务器或软件组件的特征。
- `分析页面内容`：网页的内容，包括其结构、脚本和其他元素，通常可以提供有关底层技术的线索。例如，可能有一个版权标头，指示正在使用的特定软件。

### 指纹抓取工具

- Wappalyzer
- BuiltWith
- WhatWeb
- Nmap
- Netcraft
- wafw00f


# 爬行

Web 爬虫会跟踪从一个页面到另一个页面的链接，收集信息。这些爬虫本质上是使用预定义算法来发现和索引网页的机器人，使其可以通过搜索引擎访问或用于数据分析和 Web 侦察等其他目的。

### 原理

网络爬虫的基本操作简单而强大。它以种子 URL 开头，这是要抓取的初始网页。爬网程序获取此页面，解析其内容并提取其所有链接。然后，它将这些链接添加到队列中并对其进行爬网，迭代地重复该过程。根据其范围和配置，爬虫可以探索整个网站甚至大部分 Web。

抓取策略有两种主要类型：广度优先和深度优先

广度优先：全面了解网站的结构和内容
深度优先：查找特定内容或深入了解网站结构

### 提取有价值的信息

爬网程序可以提取各种数据，每个数据在侦查过程中都有特定的用途：

- `链接（内部和外部）：`这些是 Web 的基本构建块，连接网站内的页面（`内部链接`）和到其他网站（`外部链接`）。爬虫会仔细收集这些链接，让您能够绘制网站的结构、发现隐藏的页面并识别与外部资源的关系。
- `评论`：博客、论坛或其他交互式页面上的评论部分可能是信息的金矿。用户经常在评论中无意中透露敏感细节、内部流程或漏洞暗示。
- `元数据`：元数据是指`有关数据的数据`。在网页的上下文中，它包括页面标题、描述、关键字、作者姓名和日期等信息。此元数据可以提供有关页面内容、用途以及与侦查目标相关性的宝贵上下文。
- `敏感文件`：可以将 Web 爬虫配置为主动搜索可能无意中暴露在网站上的敏感文件。这包括`备份文件`（例如 `.bak`、`.old`）、`配置文件`（例如 `web.config`、`settings.php`）、`日志文件`（例如 `error_log`、`access_log`）以及包含密码、`API 密钥`或其他机密信息的其他文件。仔细检查提取的文件，尤其是备份和配置文件，可以揭示大量敏感信息，例如`数据库凭证`、`加密密钥`，甚至源代码片段。


## robots.txt

对于 Web 侦察，robots.txt 是有价值的情报来源。在遵守本文件中概述的指令的同时，安全专业人员可以收集有关目标网站的结构和潜在漏洞的重要见解：

- `发现隐藏的目录`：robots.txt 中不允许的路径通常指向网站所有者有意希望远离搜索引擎爬虫的目录或文件。这些隐藏区域可能包含敏感信息、备份文件、管理面板或其他可能引起攻击者兴趣的资源。
- `映射网站结构`： 通过分析允许和不允许的路径，安全专业人员可以创建网站结构的基本地图。这可能会显示未从主导航链接的部分，这可能会导致未被发现的页面或功能。
- `检测爬虫陷阱`：一些网站故意在robots.txt中包含“蜜罐”目录，以引诱恶意机器人。识别此类陷阱可以深入了解目标的安全意识和防御措施。

常见指令：
- Disallow
- Allow
- Crawl-delay
- Sitemap

## Well-Known URIs


## 爬虫工具

**流行的爬虫工具：**
- Burp Suite Spider
- OWASP ZAP （Zed Attack Proxy）
- Scrapy （Python Framework）
- Apache Nutch (Scalable Crawler)


# 搜索引擎

| Operator                | Operator Description | Example                                             | Example Description                                                                     |     |
| :---------------------- | :------------------- | :-------------------------------------------------- | :-------------------------------------------------------------------------------------- | --- |
| `site:`                 | 将结果限制为特定网站或域。        | `site:example.com`                                  | Find all publicly accessible pages on example.com.                                      |     |
| `inurl:`                | 查找 URL 中包含特定术语的页面。   | `inurl:login`                                       | Search for login pages on any website.                                                  |     |
| `filetype:`             | 搜索特定类型的文件。           | `filetype:pdf`                                      | Find downloadable PDF documents.                                                        |     |
| `intitle:`              | 查找标题中包含特定术语的页面。      | `intitle:"confidential report"`                     | Look for documents titled "confidential report" or similar variations.                  |     |
| `intext:` or `inbody:`  | 在页面的正文文本中搜索术语。       | `intext:"password reset"`                           | Identify webpages containing the term “password reset”.                                 |     |
| `cache:`                | 显示网页的缓存版本（如果可用）。     | `cache:example.com`                                 | View the cached version of example.com to see its previous content.                     |     |
| `link:`                 | 查找链接到特定网页的页面。        | `link:example.com`                                  | Identify websites linking to example.com.                                               |     |
| `related:`              | 查找与特定网页相关的 Web 站点。   | `related:example.com`                               | Discover websites similar to example.com.                                               |     |
| `info:`                 | 提供有关网页的信息摘要。         | `info:example.com`                                  | Get basic details about example.com, such as its title and description.                 |     |
| `define:`               | 提供单词或短语的定义。<br>      | `define:phishing`                                   | Get a definition of "phishing" from various sources.                                    |     |
| `numrange:`             | 搜索特定范围内的数字。          | `site:example.com numrange:1000-2000`               | Find pages on example.com containing numbers between 1000 and 2000.                     |     |
| `allintext:`            | 查找正文文本中包含所有指定单词的页面。  | `allintext:admin password reset`                    | Search for pages containing both "admin" and "password reset" in the body text.         |     |
| `allinurl:`             | 查找 URL 中包含所有指定单词的页面。 | `allinurl:admin panel`                              | Look for pages with "admin" and "panel" in the URL.                                     |     |
| `allintitle:`           | 查找标题中包含所有指定单词的页面。    | `allintitle:confidential report 2023`               | Search for pages with "confidential," "report," and "2023" in the title.                |     |
| `AND`                   | 通过要求所有项都存在来缩小结果范围。   | `site:example.com AND (inurl:admin OR inurl:login)` | Find admin or login pages specifically on example.com.                                  |     |
| `OR`                    | 通过包含包含任何术语的页面来扩大结果。  | `"linux" OR "ubuntu" OR "debian"`                   | Search for webpages mentioning Linux, Ubuntu, or Debian.                                |     |
| `NOT`                   | 排除包含指定术语的结果。         | `site:bank.com NOT inurl:login`                     | Find pages on bank.com excluding login pages.                                           |     |
| `*` (wildcard)          | 表示任何字符或单词。           | `site:socialnetwork.com filetype:pdf user* manual`  | Search for user manuals (user guide, user handbook) in PDF format on socialnetwork.com. |     |
| `..` (range search)     | 查找指定数值范围内的结果。        | `site:ecommerce.com "price" 100..500`               | Look for products priced between 100 and 500 on an e-commerce website.                  |     |
| `" "` (quotation marks) | 搜索精确短语。              | `"information security policy"`                     | Find documents mentioning the exact phrase "information security policy".               |     |
| `-` (minus sign)        | 从搜索结果中排除术语。          | `site:news.com -inurl:sports`                       | Search for news articles on news.com excluding sports-related content.                  |     |
|                         |                      |                                                     |                                                                                         |     |

###  谷歌 Dorking

Google Dorking，也称为 Google Hacking，是一种利用搜索运算符的力量使用 Google 搜索发现网站上的敏感信息、安全漏洞或隐藏内容的技术。

以下是一些常见的 Google Dorks 示例，更多示例请参考 [Google Hacking Database](https://www.exploit-db.com/google-hacking-database)：

-  查找登录页面：
    - `site:example.com inurl：login`
    - `site:example.com (inurl:login OR inurl:admin)`
- 识别暴露的文件：
    - `site:example.com filetype：pdf`
    - `site:example.com (filetype:xls OR filetype:docx)`
- 发现配置文件：
    - `site:example.com inurl:config.php`
    - `site:example.com (ext:conf OR ext:cnf)` （搜索配置文件常用的扩展名）
- 查找数据库备份：
    - `site:example.com inurl：backup`
    - `site:example.com filetype：sql`

# Web 档案

其实就是一个网站：
https://web.archive.org/

这个网站有类似于时光机的功能，但是被墙了，而且很厉害。

# 自动化侦察

### 侦测框架

这些框架旨在为 Web 侦查提供一整套工具：

- [FinalRecon](https://github.com/thewhiteh4t/FinalRecon)：一种基于 Python 的侦察工具，为不同的任务提供一系列模块，例如 SSL 证书检查、Whois 信息收集、标头分析和爬网。其模块化结构可根据特定需求轻松定制。
- [侦察：](https://github.com/lanmaster53/recon-ng)一个用 Python 编写的强大框架，为不同的侦察任务提供各种模块的模块化结构。它可以执行 DNS 枚举、子域发现、端口扫描、网络爬虫，甚至利用已知漏洞。
- [theHarvester](https://github.com/laramies/theHarvester)：专为从搜索引擎、PGP 密钥服务器和 SHODAN 数据库等不同公共来源收集电子邮件地址、子域、主机、员工姓名、开放端口和横幅而设计。它是一个用 Python 编写的命令行工具。
- [SpiderFoot](https://github.com/smicallef/spiderfoot)：一种开源智能自动化工具，可与各种数据源集成以收集有关目标的信息，包括 IP 地址、域名、电子邮件地址和社交媒体资料。它可以执行 DNS 查找、网络爬取、端口扫描等。
- [OSINT 框架](https://osintframework.com/)：用于开源情报收集的各种工具和资源的集合。它涵盖了广泛的信息来源，包括社交媒体、搜索引擎、公共记录等。

