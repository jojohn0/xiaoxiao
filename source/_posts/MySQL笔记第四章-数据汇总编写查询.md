---
title: MySQL笔记第四章-数据汇总编写查询
date: 2021-04-08 12:01:46
tags: MySQL
categories: MySQL
# cover: https://z3.ax1x.com/2021/04/06/c1o8CF.png
description: 本章节主要介绍MySQL的数据汇总的编写查询
---
# 数据汇总编写查询

## 聚合函数

* 聚合函数有很多：比如 MAX() MIN() AVG() SUM() COUNT()

* 聚合函数只运行非空值
<!--more-->

```sql
SELECT
	COUNT(payment_date) AS count_of_payments -- 只有15条数据，有两条为空值
FROM invoices
```

* 如果要运行所有值，在括号内加上*号

```sql
SELECT
	COUNT(*) AS total_records -- 17条数据，运行了所有值
FROM invoices
```

* 排除重复条目 使用DISTINCT关键字

```sql
SELECT
	-- COUNT(client_id) AS total_record 返回7条数据
	COUNT(DISTINCT client_id) AS total_record --返回3条数据
FROM invoices
WHERE invoice_date > '2019-07-01'
```

## GROUP BY子句

* 对一列或多列进行数据分组
* 一般来讲 每当你在选择语句中有聚合函数 而且在对数据进行分组 那么你可以直接根据SELECT子句里面的所有列进行分组

```sql
SELECT
	client_id,
	SUM(invoice_total) AS total_sales
FROM invoices
GROUP BY client_id
```

* 默认状态下，数据是按照GROUP BY子句中指定的列排序的

* 注意关键字顺序：WHERE关键字在GROUP BY的前面，GROUP BY关键字在ORDER BY的前面

* 练习

```sql
SELECT 
	date,
    ps.name AS payment_method,
    SUM(amount) AS total_payments
FROM payments p
JOIN payment_methods ps
	ON p.payment_method = payment_method_id
GROUP BY date,payment_method
ORDER BY date
```



## HAVING 子句

* 对一列或多列进行数据分组后，进行筛选
* 与WHERE子句不同的是 WHERE子句是在分组前进行筛选，而HAVING子句是在分组后进行筛选
* HAVING子句中用到的列，一定是SELECT子句中存在的

```sql
SELECT
	client_id,
	SUM(invoice_total) AS total_sales,
	COUNT(*) AS number_of_invoices
FROM invoices
GROUP BY client_id
HAVING total_sales > 500 AND number_of_invoices > 5
```

* 练习

```sql
SELECT 
	  c.customer_id,
    c.first_name,
    c.last_name,
	  c.state,
    SUM(oi.quantity * oi.unit_price) AS total_sales
FROM customers c
JOIN orders o
	USING (customer_id)
JOIN order_items oi
	USING (order_id)
WHERE state = 'VA'
GROUP BY 
	  c.customer_id,
    c.first_name,
    c.last_name
HAVING total_sales > 100
```

## ROLLUP运算符

* ROLLUP运算符只能应用于聚合值的列

```sql
SELECT
	ps.name AS payment_method,
    SUM(amount) AS total
FROM payments p
JOIN payment_methods ps
	ON p.payment_method = ps.payment_method_id
GROUP BY ps.name WITH ROLLUP --通过ROLLUP运算符 求取和值
```

