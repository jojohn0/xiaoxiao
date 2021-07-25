---
title: MySQL笔记第三章-插入、删除、更新
date: 2021-04-07 16:48:09
tags: MySQL
categories: MySQL
# cover: https://z3.ax1x.com/2021/04/06/c1o8CF.png
description: 本章节主要介绍MySQL插入、删除、更新操作
---
# 插入、删除、更新

## 列属性

### datatype代表数据类型

* INT 整型
* VARCHAR(50) 代表 最多存储50个字符，如果只有5个字符，那就只存5个
* CHAR(50) 代表 存储50个字符，如果只有5个字符，其余的会由空格符填充
<!--more-->

### PK 属性

* 此属性勾选代表该属性被标记为主键

### NN 属性

* 勾选代表不能写空值，该属性决定该列是否可以写空值

### AI 属性

* 自动递增，通常用于主键

### Default 属性

* 代表默认该属性的默认值

## 插入单行

### 使用INSET关键字

```sql
INSERT INTO customers
VALUES (
	DEFAULT,
    'John',
    'Smith',
    '1990-01-01',
    NULL,
    'address',
    'city',
    'CA'
	)
```

* 在表后面可以选择提供想要插入值的列，可以使用任意顺序

```sql
INSERT INTO customers(
	first_name,
    last_name,
    birth_date,
    address,
    city,
    state
	)
VALUES (
    'John',
    'Smith',
    '1990-01-01',
    'address',
    'city',
    'CA'
	)
```

## 插入多行

* 在插入的每一列的值后面添加逗号

```sql
INSERT INTO shippers(name)
VALUES ('shipper1'),
  ('shipper2'),
  ('shipper3')
```

## 插入分层行

* 使用LAST_INSERT_ID()方法返回最近插入的id

```sql
INSERT INTO orders (customer_id,order_date,status)
VALUES (1,'2019-01-02',1);

INSERT INTO order_items
VALUES (LAST_INSERT_ID(),1,1,2.95),
        (LAST_INSERT_ID(),2,1,3.95)
```

## 创建表复制

* 使用CREATE TABLE AS语句，可以快速创建一张表的复制

```sql
CREATE TABLE orders_archived AS
SELECT * 
FROM orders
```

> 注意：使用此方法创建一张表 MySQL会忽略设计模式中的PK AI属性

* 使用选择语句作为插入语句中的一个子查询

```sql
INSERT INTO orders_archived
SELECT *
FROM orders
WHERE order_date < '2019-01-01'
```

## 更新单行

* 使用 UPDATE 关键字说明更新的表格，使用 SET 关键字说明要更新的列以及数据，使用WHERE关键字说明要更新的条件

```sql
UPDATE invoices
SET payment_total = DEFAULT,payment_date=NULL
WHERE invoice_id = 1
```



## 更新多行

* 与单行更新使用的方法相同



## 在UPDATE中使用子查询

```sql
UPDATE invoices
SET
	  payment_total = invoice_total * 0.5,
    payment_date = due_date
WHERE client_id IN (
		SELECT client_id
        FROM clients
        WHERE state IN ('CA','NY')
		)
```



## 删除行

* 使用DELETE FROM语句 删除表记录

```sql
DELETE FROM invoices
WHERE client_id = (
SELECT client_id
FROM clients
WHERE name = 'Myworks'
)
```

