---
tags:
  - 漏洞
  - 文件包含
  - 笔记
---

# 文件包含

  

先写一部分的HTBA的内容，以后遇到了会进行补充

# 文件泄露

## 本地文件包含-LFI(无过滤)


>PAGE指url的参数，具体情况具体分析

**路径遍历**
直接包含：

```

?PAGE=/etc/passwd

对应代码

include($_GET['language']);

```

因为使用的是绝地路径，所以可以直接包含
然而，在许多情况下，Web 开发人员可能会在language范围。例如，language参数可以用于文件名，并且可以添加在目录之后，如下：

```

?PAGE=../../../../etc/passwd

对应代码

include("./languages/" . $_GET['language']);

```

**文件名前缀**

在某些情况下，我们的输入可能会附加在不同的字符串之后。例如，它可以与前缀一起使用来获取完整的文件名，在这种情况下，如果我们尝试使用以下命令遍历目录 ../../../etc/passwd，最终的字符串将是lang_../../../etc/passwd，这是无效的：

```

?PAGE=/../../../../etc/passwd

对应代码

include("lang_" . $_GET['language']);

```

  

**扩展名**

在这种情况下，如果我们尝试阅读/etc/passwd，那么包含的文件将是 /etc/passwd.php，不存在：

```

对应代码

include($_GET['language'] . ".php");

```

没说怎么办

**二阶攻击**

未完待续...

## LFI基本绕过

  

**非递归路径遍历过滤器**

  

针对 LFI 的最基本的过滤器之一是搜索和替换过滤器，它只是删除 ( 的子字符串../）以避免路径遍历。例如：

```

$language = str_replace('../', '', $_GET['language']);

对策：

?PAGE=....//....//....//....//etc/passwd

..././

....\/

....////

```


**编码**

URL编码

`../`转换为`%2e%2e%2f`

其他的字符可以使用在线工具转换：

为此，我们必须对所有字符（包括点）进行 URL 编码。某些 URL 编码器可能不会对点进行编码，因为它们被视为 URL 方案的一部分

  

正如我们所看到的，我们也能够成功绕过过滤器并使用路径遍历来读取 /etc/passwd。此外，我们还可以使用Burp Decoder对编码后的字符串再次进行编码，得到double encoded字符串，它也可能绕过其他类型的过滤器。

  

**批准的路径**

```php

if(preg_match('/^\.\/languages\/.+$/', $_GET['language'])) {

    include($_GET['language']);

} else {

    echo 'Illegal path specified!';

}

```


### 附加扩展

  

**路径截断**

  

在 PHP 的早期版本中，定义的字符串的最大长度为 4096 个字符，这可能是由于 32 位系统的限制。如果传递更长的字符串，则只会是truncated，最大长度之后的任何字符都将被忽略。此外，PHP 还用于删除路径名中的尾部斜杠和单点，因此如果我们调用 (/etc/passwd/.）然后 /. 也会被截断，PHP 会调用 (/etc/passwd）。 PHP 和 Linux 系统通常也会忽略路径中的多个斜杠（例如////etc/passwd是一样的 /etc/passwd）。同样，当前目录快捷方式（.）在路径中间也将被忽略（例如 /etc/./passwd).

  

如果我们将这两个 PHP 限制结合在一起，我们可以创建非常长的字符串，并计算出正确的路径。每当我们达到 4096 个字符的限制时，附加的扩展名 (.php) 将被截断，并且我们将得到一条没有附加扩展名的路径。最后，还需要注意的是，我们还需要start the path with a non-existing directory为了使这项技术发挥作用。

  

示例：

```

?language=non_existing_directory/../../../etc/passwd/./././.[./ REPEATED ~2048 times]

```

当然，我们不需要手动输入 ./ 2048 次（总共 4096 个字符），但我们可以使用以下命令自动创建该字符串：

```bash

turtlera@htb[/htb]$ echo -n "non_existing_directory/../../../etc/passwd/" && for i in {1..2048}; do echo -n "./"; done

non_existing_directory/../../../etc/passwd/./././<SNIP>././././

```

  

**空字节**

  

5.5 之前的 PHP 版本容易受到 null byte injection，这意味着添加一个空字节（%00) 位于字符串末尾将终止该字符串，并且不考虑其后的任何内容。这是由于字符串在低级内存中的存储方式决定的，其中内存中的字符串必须使用空字节来指示字符串的结尾，如汇编语言、C 或 C++ 语言中所示。

  

要利用此漏洞，我们可以以空字节结束我们的有效负载（例如 /etc/passwd%00），这样最终的路径就传递到include()将是（/etc/passwd%00.php）。这样一来，即使 .php 附加到我们的字符串中，空字节之后的任何内容都会被截断，因此使用的路径实际上是/etc/passwd，导致我们绕过附加的扩展。

  
  

## LFI-PHP过滤器

### PHP过滤器

HTBA讲的不多，在ctf中还是听常用的，以后遇到的在这里补充

PHP 过滤器 是一种 PHP 包装器，我们可以在其中传递不同类型的输入，并通过我们指定的过滤器对其进行过滤。要使用 PHP 包装流，我们可以使用 php:// 字符串中的方案，我们可以使用以下命令访问 PHP 过滤器包装器 php://filter/.

这 filter 包装器有几个参数，但我们攻击所需的主要参数是 resource 和 read。这 resource 过滤器包装器需要参数，通过它我们可以指定要应用过滤器的流（例如本地文件），而 read 参数可以对输入资源应用不同的过滤器，因此我们可以使用它来指定要对资源应用哪个过滤器。

有四种不同类型的过滤器可供使用，它们是 字符串过滤器, 转换过滤器, 压缩过滤器， 和 加密过滤器。您可以在各自的链接上阅读有关每个过滤器的更多信息，但对 LFI 攻击有用的过滤器是 convert.base64-encode 过滤器，下 Conversion Filters.

### PHP 文件的模糊测试

第一步是使用类似的工具对不同的可用 PHP 页面进行模糊测试 ffuf 或者gobuster：

```

ffuf -w /opt/useful/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://<SERVER_IP>:<PORT>/FUZZ.php

```

提示：与正常的 Web 应用程序使用不同，我们不限于 HTTP 响应代码 200 的页面，因为我们具有本地文件包含访问权限，因此我们应该扫描所有代码，包括“301”、“302”和“403”页面，以及我们也应该能够阅读他们的源代码。

### 标准 PHP 包含

在前面的部分中，如果您尝试通过 LFI 包含任何 php 文件，您会注意到包含的 PHP 文件会被执行，并最终呈现为普通的 HTML 页面。例如，让我们尝试包含config.php页 （.php 由 Web 应用程序附加的扩展名）：

```

?language=config

```

正如我们所看到的，我们得到一个空结果来代替 LFI 字符串，因为config.php很可能仅设置 Web 应用程序配置，并不呈现任何 HTML 输出。

  

这在某些情况下可能很有用，例如访问我们无法访问的本地 PHP 页面（即 SSRF），但在大多数情况下，我们对通过 LFI 阅读 PHP 源代码更感兴趣，因为源代码往往会揭示重要的信息有关 Web 应用程序的信息。这就是base64php 过滤器变得很有用，因为我们可以使用它对 php 文件进行 Base64 编码，然后我们将获得编码后的源代码，而不是让它被执行和渲染。这对于我们处理带有附加 PHP 扩展的 LFI 的情况特别有用，因为我们可能仅限于包含 PHP 文件，如上一节所述。

笔记： 这同样适用于除 PHP 之外的 Web 应用程序语言，只要易受攻击的函数可以执行文件。否则，我们将直接获取源代码，并且不需要使用额外的过滤器/函数来读取源代码。请参阅第 1 节中的函数表，了解哪些函数具有哪些权限。

### 源代码披露

一旦我们有了想要读取的潜在 PHP 文件的列表，我们就可以开始使用 base64 PHP 过滤器。让我们尝试阅读一下源代码 config.php 使用 base64 过滤器，通过指定 convert.base64-encode 为 read 参数和 config 为 resource 参数，如下：

```

php://filter/read=convert.base64-encode/resource=config

```

笔记： 我们故意将资源文件留在字符串的末尾，因为 .php 扩展名会自动附加到我们输入字符串的末尾，这将使我们指定的资源成为 config.php.

这个方法读出来的是base64加密过的源代码，使用bash或是线上工具进行解码。

### 日志包含

第一次遇到日志包含是在thm的靶机Archangel中，参考[TryHackMe-Archangel - aldeid](https://www.aldeid.com/wiki/TryHackMe-Archangel)
**nginx**
日志位置：/var/log/nginx/
访问日志：access.log
错误日志：error.log

**apache**
日志位置：/var/log/apache2/
访问日志：access.log
错误日志：error.log



