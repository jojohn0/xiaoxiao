---
title: MySQL笔记第二章-连接
date: 2021-04-06 20:09:36
tags: MySQL
categories: MySQL
# cover: https://z3.ax1x.com/2021/04/06/c1o8CF.png
description: 本章节主要介绍MySQL基础语句
---
# 连接

## 内连接

### JOIN关键字

* 使用JOIN关键字可以将一张表通过和另一张表共有的某个属性和另一张表相连接
<!--more-->

```sql
SELECT *
FROM order_items
INNER JOIN products ON order_items.product_id = products.product_id
```

* 可以在表后为表起别名

```sql
SELECT *
FROM order_items o
INNER JOIN products p
	ON o.product_id = p.product_id
```

* 如果选择列时有共有属性，需要在属性前加上哪张表的前缀

```sql
SELECT o.product_id
FROM order_items o
INNER JOIN products p
	ON o.product_id = p.product_id
```

## 跨数据库连接

* 通过在表前加前缀实现跨数据库连接

```sql
SELECT *
FROM order_items oi
INNER JOIN sql_inventory.products p
	ON oi.product_id = p.product_id
```

## 自连接

* 与内连接唯一不同的是需要使用不同的别名，并且每一列也需要加上前缀

```sql
USE sql_hr;
SELECT 
	e.employee_id,
    e.first_name,
    m.first_name AS manager
FROM employees e
INNER JOIN employees m
	ON  e.reports_to = m.employee_id
```

## 多表连接

```sql
USE sql_store;

SELECT *
FROM orders o
INNER JOIN customers c
	ON o.customer_id = c.customer_id
INNER JOIN order_statuses os
	ON os.order_status_id = o.status
```

## 复合连接条件(多个条件连接表格)

* 使用AND关键字实现复合连接

```sql
USE sql_store;

SELECT *
FROM order_items oi
JOIN order_item_notes oin
	ON oi.order_id = oin.order_id
    AND oi.product_id = oin.product_id
```

## 隐式连接语法(不推荐使用)

* 普通连接

```sql
SELECT *
FROM orders o
JOIN customers c
  ON o.customer_id =c.customer_id
```

* 使用WHERE关键字实现隐式连接

```sql
SELECT *
FROM orders o ,customers c
WHERE o.customer_id = c.customer_id
```

## 外连接

* 外连接分为左连接和右连接

### 左连接

* 所有左表的结果都会返回，不论条件是否正确

```sql
SELECT 
    c.customer_id,
    c.first_name,
    o.order_id
FROM customers c
LEFT JOIN orders o
	ON o.customer_id =c.customer_id
ORDER BY c.customer_id
```

### 右连接

* 所有右表结果都会返回，不论条件是否正确

```sql
SELECT 
    c.customer_id,
    c.first_name,
    o.order_id
FROM customers c
RIGHT JOIN orders o
	ON o.customer_id =c.customer_id
ORDER BY c.customer_id
```


## 多表外连接

```sql
USE sql_store;

SELECT 
	o.order_date,
    o.order_id,
    c.first_name,
    s.name AS shipper,
    ots.name AS status
FROM orders o
JOIN customers c
	ON o.customer_id = c.customer_id
LEFT JOIN order_statuses ots
	ON o.status = ots.order_status_id
LEFT JOIN shippers s
	ON o.shipper_id = s.shipper_id
```

## 自外连接

```sql
USE sql_hr;

SELECT 
	e.employee_id,
    e.first_name,
    m.first_name AS manager
FROM employees e
LEFT JOIN employees m
	ON e.reports_to = m.employee_id
```

## USING子句

* USING子句可以简化操作

```sql
USE sql_store;

SELECT 
	o.order_id,
    c.first_name
FROM orders o
JOIN customers c
	USING (customer_id)
  -- 相当于 ON o.customer_id = c.customer_id
```

* USING关键字只能在不同表中列的名字完全相同的情况下使用

## 自然连接(不推荐)

* 依照数据库引擎连接，无法控制它,不确定性
  
```sql
SELECT 
	o.order_id,
    customers.first_name
FROM orders o
NATURAL JOIN customers
```

## 交叉连接

* 交叉连接第一个表的每条记录和第二个表的每条记录

* 显式语法

```sql
SELECT 
	c.first_name AS customer,
    p.name AS product
FROM customers c
CROSS JOIN products p
```

* 隐式语法

```sql
SELECT 
	c.first_name AS customer,
    p.name AS product
FROM customers c,products p
```

## 联合(Unions)

* 使用UNION运算符合并多段查询的数据
* 查询返回列的数量需要一样，否则会报错
* 第一段查询写了啥都会被用来决定列名

```sql
--一段查询
SELECT 
	order_id,
    order_date,
    'Active' AS status
FROM orders
WHERE order_date >= '2019-01-01'
UNION
--另一段查询
SELECT 
	order_id,
    order_date,
    'Archived' AS status
FROM orders
WHERE order_date < '2019-01-01'
```