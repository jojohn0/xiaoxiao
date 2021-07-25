---
title: MySQL笔记第一章-语句
date: 2021-04-06 20:04:58
tags: MySQL
categories: MySQL
# cover: https://z3.ax1x.com/2021/04/06/c1o8CF.png
description: 本章节主要介绍MySQL基础语句
---
# 语句

## 选择语句

### 注释

```sql
-- WHERE customer_id = 1
```
<!--more-->

### 语句顺序不能修改

* select，from，where，order by顺序不能修改(sql关键字不区分大小写)
* 建议将每条子句放在新行里

```sql
SELECT * -- *号返回所有的列
FROM customers
-- WHERE customer_id = 1
ORDER BY first_name
```

### select 语句

* select 语句选择列，可以选择表达式(运算优先级基于数学)

```sql
SELECT 
	last_name,points,points * 10 + 10
```

* 使用AS为表达式结果集一个描述性名称

```sql
SELECT 
	points * 10 + 10 AS discount_factor
```

* 为描述性名称中加入空格(给名字添加引号)
  
```sql
SELECT 
	points * 10 + 10 AS 'discount factor'
```

* 使用 DISTINCT 关键字删除重复项目

```sql
SELECT DISTINCT state
```

## WHERE子句

* WHERE子句用于筛选数据

### 运算符

```sql
> >= < <= != <>
```

### sql中的日期

```sql
SELECT 
	*
FROM customers
WHERE birth_date > '1990-01-01'
```

## AND,OR 和 NOT 操作符

* 这三种操作符用作筛选数据时结合多条搜索条件

### AND操作符

* 返回多个条件符合的结果

```sql
SELECT 
	*
FROM customers
WHERE birth_date > '1990-01-01' AND points > 1000
```

### OR操作符

* 只要有一个条件正确，则返回结果

```sql
SELECT 
	*
FROM customers
WHERE birth_date > '1990-01-01' OR points > 1000
```

> AND优先级高于OR

### NOT操作符

```sql
SELECT 
	*
FROM customers
WHERE NOT (birth_date > '1990-01-01' OR points > 1000)

-- 相当于 WHERE birth_date <= '1990-01-01' AND points <= 1000 
```

## IN操作符

* 使用IN操作符简化OR操作

```sql
SELECT *
FROM customers
WHERE state = 'VA' OR state = 'MA'
-- 相当于 WHERE state IN ('VA','MA')
```

## BETWEEN操作符

* 当一个属性同一个范围值进行比较，可以使用BETWEEN操作符

```sql
SELECT *
FROM customers
-- WHERE points >= 1000 AND points  <= 3000
WHERE points BETWEEN 1000 AND 3000
```

## LIKE操作符

* 检索遵循特定字符串模式的行

### %表示任意字符数(可以用在开头结尾中间都可以) _下划线表示单个字符

```sql
SELECT *
FROM customers
WHERE last_name LIKE '%y' --查询以y结尾的名字
WHERE last_name LIKE '%y%' --查询名字中有y的集合
WHERE last_name LIKE '____y' -- 查询名字中五个字符，并且以y结尾的集合
```

## REGEXP操作符

* ^表示字符串的开头
* $表示字符串的结尾
* |表示搜索多个结果
* []表示包含多个结果，例如[ge]i 表示匹配 gi ei [a-h]e 表示e前面可以有a到h的结果

## NULL操作符

* 使用NULL操作符查询是否存在null值的集合

```sql
SELECT *
FROM customers
WHERE phone IS NULL
```

## ORDER BY 语句

* 用来设置排序

### 使用关键字DESC降序

```sql
SELECT *
FROM customers
ORDER BY first_name DESC
```

## LIMIT语句

* 查询前n条数据

```sql
SELECT *
FROM customers
LIMIT n
```

* 设置偏移量来查询

```sql
SELECT *
FROM customers
LIMIT 6,3 
--- 表示查询7-9数据
```
