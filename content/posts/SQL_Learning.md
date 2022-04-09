---
title: "SQL必知必会"
date: 2022-03-25T19:45:02+08:00
draft: false
tags: ["code","STL"]
series: ["SQL必知必会"]
categories: ["Coding"]
---

<!--more-->

## Lesson 5 高级数据过滤
```sql

--operator AND IN OR

SELECT prod_id,prod_price,prod_name
FROM Products
WHERE vend_id ='DLL01' AND prod_price <= 4;

SELECT prod_id,prod_price,prod_name,vend_id
FROM Products
WHERE vend_id ='DLL01' OR vend_id ='BRS01';
--AND 比OR优先级高 
SELECT prod_price,prod_name
FROM Products
WHERE (vend_id ='DLL01' OR vend_id = 'BRS01')
		AND prod_price >= 10;


--IN 与or功能相同
--比OR快
SELECT prod_id,prod_price,prod_name,vend_id
FROM Products
WHERE vend_id IN ('DLL01','BRS01');

--NOT 否定条件,与其他条件一起使用

SELECT prod_name,vend_id
FROM Products
WHERE NOT vend_id IN ('DLL01','BRS01');

--5.5.1
SELECT vend_name,vend_state,vend_country
FROM Vendors
WHERE vend_country='USA' AND vend_state='CA';

--5.5.2
SELECT order_num,prod_id,quantity
FROM OrderItems
WHERE prod_id IN ('BR01','BR02','BR03') AND quantity>=100;

--5.5.3
SELECT prod_name,prod_price
FROM Products
WHERE prod_price>3 AND prod_price<6
ORDER BY prod_price
--
```
## Lesson 6 用通配符进行过滤

```sql
--通配符只能用于文本字段

SELECT prod_id,prod_name
FROM Products
WHERE prod_name LIKE 'Fish%';--%表示任意字符出现任意次数，Fish开头的

SELECT prod_id,prod_name
FROM Products
WHERE prod_name LIKE 'F%y';-- WHERE email LIKE 'b%@forta.com'


SELECT prod_id,prod_name
FROM Products
WHERE prod_name LIKE '_ inch teddy bear';
-- _ 下划线只匹配单个字符
--输出 8 inch teddy bear, 而不会返回 12 inch teddy bear


SELECT cust_contact
FROM Customers
WHERE cust_contact LIKE '[JM]%'--[]匹配方括号中任意一个字符，%匹配第一个字符后的任意数目字符。返回以J或M开头的
ORDER BY cust_contact;


SELECT cust_contact
FROM Customers
WHERE cust_contact LIKE '[^JM]%'-- ^脱字号 否定
ORDER BY cust_contact;

SELECT cust_contact
FROM Customers
WHERE NOT cust_contact LIKE '[JM]%'-- NOT 否定
ORDER BY cust_contact;

--6.4.1
SELECT prod_desc,prod_name
FROM Products
WHERE prod_desc LIKE '%toy%';

--6.4.2
SELECT prod_desc,prod_name
FROM Products
WHERE NOT prod_desc LIKE '%toy%';


--6.4.3
SELECT prod_desc,prod_name
FROM Products
WHERE (prod_desc LIKE '%toy%' )
AND (prod_desc LIKE '%carrots%');

--6.4.4
SELECT prod_desc,prod_name
FROM Products
WHERE prod_desc LIKE '%toy%carrots%' ;

```

## Lesson 7 创建计算字段
```sql

--拼接 +
SELECT vend_name + '('+ vend_country + ')'
FROM Vendors
ORDER BY vend_name;

--去掉值右边的全部空格 ：RTRIM()函数
--去掉左边的空格LTRIM(),TRIM()去掉左右的空格
SELECT RTRIM(vend_name)+'('+RTRIM(vend_country)+')'
FROM Vendors
ORDER BY vend_name;

--别名 as
SELECT RTRIM(vend_name)+'('+RTRIM(vend_country)+')'
AS vend_title
FROM Vendors
ORDER BY vend_name;

--执行算数计算
SELECT prod_id,quantity,item_price,
quantity*item_price AS expand_price--计算字段
FROM OrderItems
WHERE order_num=20008;

--7.5.1
SELECT  vend_id,vend_name AS vname,
vend_address AS vaddress,
vend_city AS vcity
FROM Vendors
ORDER BY vend_name;

--7.5.2
SELECT prod_id,prod_price,
prod_price*0.9 AS sale_price--计算字段
FROM Products;
```
```sql
--8 使用函数处理数据

--upper()upper,Upper()  转换为大写
--LOWER()			转换为小写
--LEFT()			返回最左边的字符
--RIGHT()			返回最右边的字符
--LTRIN()			去掉左边空格
--RTRIN()			去掉左边空格
--SUBSTR(),SUBSYRING()提取组成部分
--SOUNDEX()   语音类似的字符串
SELECT vend_name,UPPER(vend_name) AS vend_name_upcase
FROM Vendors
ORDER BY vend_name;

--DATEPART()
--返回日期的某一部分
SELECT *
FROM Orders
WHERE DATEPART(yy,order_date) = 2020;

--DATETIME BETWEEN AND
SELECT *
FROM Orders
WHERE order_date BETWEEN '2020-01-01'AND '2021-01-12' ;

--数值处理函数
--ABS()
--COS()
--EXP()
--PI()
--SIN()
--SQRT()
--TAN()

--8.4.1
--SUBSTRING(str,i,len)i表示第i个字符开始（计数从1开始不是从0开始）
--len表示截取长度
SELECT cust_id,cust_name,cust_contact,cust_city,
(SUBSTRING(cust_contact,1,2)+SUBSTRING(cust_city,1,3)) AS user_login
FROM Customers;

--8.4.2
--选择某个时间段的两种方法
SELECT order_num,order_date
FROM Orders
WHERE order_date BETWEEN '2020-01-01'AND '2020-01-31';

SELECT order_num,order_date
FROM Orders
WHERE DATEPART( YY,order_date)=2020 AND DATEPART(MM,order_date)=1;
```

## Lesson9  汇总数据
```sql
--Lesson 9
--汇总数据

--AVG()
SELECT AVG(prod_price) AS avg_price--只能用于单个列,自动忽略NULL的行
FROM Products;

--COUNT(*)  计算表中行数，不忽略NULL
--COUNT(column)  计算某一列中有值的行数，忽略NULL

--MAX() 忽略NULL
--MIN() 忽略NULL
--SUM()


-- 对以上五个函数，可以指定：
-- ALL(默认)对所有行计算,DISTINCT只包含不同的值

--DISTINCT 不能用于COUNT(*),且必须使用列名，不能用于计算或表达式
SELECT AVG(DISTINCT prod_price) AS avg_price
FROM Products
WHERE vend_id = 'DLL01';

--9.5.1
SELECT SUM(quantity)AS sold_prod
FROM OrderItems;

--9.5.2
SELECT SUM(quantity)AS sold_prod
FROM OrderItems
WHERE prod_id ='BR01';

--9.5.3
SELECT MAX(prod_price)AS max_price
FROM Products
WHERE prod_price<10;

```
## Lesson10 分组数据
```sql
--Lesson10 分组数据
--过滤分组HAVING
--过滤行WHERE
--the same usage 

SELECT cust_id, COUNT(*) AS orders 
FROM Orders 
GROUP BY cust_id
HAVING COUNT(*) >= 2;

--WHERE 在数据分组前进行过滤
--HAVING 在数据分组后进行过滤

SELECT vend_id, COUNT(*) AS num_prods
FROM Products
WHERE prod_price >= 4
GROUP BY vend_id
HAVING COUNT(*) >=2--（计数大于等于2的分组）具有两个以上产品的供应商
ORDER BY vend_id;

--记住语句顺序↑

--10.7.1
SELECT order_num,COUNT(*) AS order_lines
FROM OrderItems
GROUP BY order_num
ORDER BY order_lines;

--10.7.2
SELECT MIN(prod_price) AS chengben
FROM Products
GROUP BY vend_id
ORDER BY chengben;

--10.7.3
--至少含100项的所有订单的订单号
SELECT order_num
FROM OrderItems
GROUP BY order_num
HAVING SUM(quantity)>=100
ORDER BY order_num;

--10.7.4
SELECT order_num,SUM(item_price*quantity) AS total
FROM OrderItems
GROUP BY order_num
HAVING SUM(item_price*quantity)>=1000
ORDER BY order_num;

--10.7.5
SELECT order_num, COUNT(*) AS items 
FROM OrderItems 
GROUP BY order_num --不能是items
HAVING COUNT(*) >= 3
ORDER BY items, order_num;



```
## Lesson 11 子查询
```sql
--Lesson11 子查询
SELECT cust_id
FROM Orders
WHERE order_num IN (SELECT order_num
					FROM OrderItems
					WHERE prod_id = 'RGAN01');
--对每个顾客执行COUNT(*)
SELECT cust_name, cust_state, 
		(SELECT COUNT(*) 
		 FROM Orders 
		 WHERE Orders.cust_id = Customers.cust_id) AS orders
FROM Customers
ORDER BY cust_name;

--11.5.1
SELECT cust_id
FROM Orders
WHERE order_num in (SELECT order_num
					FROM OrderItems
					WHERE item_price>=10 );

--11.5.2
SELECT cust_id,order_date
FROM Orders
WHERE order_num IN (SELECT order_num
					FROM OrderItems
					WHERE prod_id='BR01')
--11.5.3
SELECT cust_email
FROM Customers
WHERE cust_id IN (SELECT cust_id
				  FROM Orders
				  WHERE order_num IN (SELECT order_num
									  FROM OrderItems
									  WHERE prod_id='BR01'))
						
--11.5.4
--我们需要一个顾客 ID列表，其中包含他们已订购的总金额。
--编写SQL 语句，返回顾客 ID（Orders 表中的 cust_id）
--并使用子查询返回 total_ordered 以便返回每个顾客的订单总数。
--将结果按金额从大到小排序。提示：你之前已经使用 SUM()计算订单总数。
SELECT cust_id,
			(SELECT SUM(item_price*quantity)
			FROM OrderItems
			WHERE Orders.order_num=OrderItems.order_num)
			As total_ordered
FROM Orders
ORDER BY total_ordered DESC;

--11.5.5
SELECT prod_name,
		(SELECT SUM(quantity)
		 FROM OrderItems
		 WHERE Products.prod_id=OrderItems.prod_id)
		 AS quant_sold
FROM Products;
				  


```

## Lesson 12 联结表

```SQL
-- 联结表(join)

--12.2创建联结表
SELECT vend_name, prod_name, prod_price 
FROM Vendors, Products
WHERE Vendors.vend_id = Products.vend_id;
--如果没有WHERE，则第一个表中的每一行都与第二个表
--每一行匹配（笛卡尔积）

--内联结（同样写法）
SELECT vend_name, prod_name, prod_price 
FROM Vendors
INNER JOIN Products ON Vendors.vend_id = Products.vend_id;

--联结多个表
--联结越多表性能下降越厉害
SELECT prod_name, vend_name, prod_price, quantity 
FROM OrderItems, Products, Vendors
WHERE Products.vend_id=Vendors.vend_id
 AND OrderItems.prod_id=Products.prod_id
 AND order_num=20007;

 --12.4.1
SELECT cust_name,order_num
FROM Customers,Orders
WHERE Customers.cust_id=Orders.cust_id
ORDER BY cust_name,order_num;

SELECT cust_name,order_num
FROM Customers INNER JOIN Orders 
ON Customers.cust_id=Orders.cust_id
ORDER BY cust_name,order_num;
--12.4.2
--(1)子查询实现
SELECT cust_name,order_num,
				(SELECT SUM(item_price*quantity)
				 FROM OrderItems
				 WHERE OrderItems.order_num=Orders.order_num)AS OrderTotal--完全限定列名
FROM Customers
INNER JOIN Orders ON Customers.cust_id=Orders.cust_id
ORDER BY cust_name,order_num;


--(2)联结？
SELECT cust_name,Orders.order_num,SUM(item_price*quantity)AS OrderTotal--?
FROM Customers,Orders,OrderItems
WHERE Customers.cust_id=Orders.cust_id
	AND Orders.order_num =OrderItems.order_num
GROUP BY cust_name,Orders.order_num
ORDER BY cust_name,order_num;

--12.4.3
SELECT order_date
FROM Orders,OrderItems
WHERE OrderItems.order_num=Orders.order_num AND prod_id='BR01'

SELECT order_date
FROM Orders
INNER JOIN OrderItems ON OrderItems.order_num=Orders.order_num
WHERE prod_id='BR01'

--12.4.4
--多个联结
SELECT order_date,cust_email
FROM Orders
INNER JOIN OrderItems ON OrderItems.order_num=Orders.order_num 
INNER JOIN Customers ON Orders.cust_id=Customers.cust_id
WHERE prod_id='BR01'

--12.4.5
/*5. 再让事情变得更加有趣些，我们将混合使用联结、聚合函数和分组。 准备好了吗？
回到第 10课，当时的挑战是要求查找值等于或大于 1000 的所有订单号。
这些结果很有用，但更有用的是订单数量至少达到 这个数的顾客名称。
因此，编写 SQL 语句，使用联结从 Customers 表返回顾客名称（cust_name），
并从 OrderItems 表返回所有订单的 总价。 
提示：要联结这些表，还需要包括 Orders 表（因为 Customers 表 与 OrderItems 表不直接相关，
Customers 表与 Orders 表相关，而 Orders 表与 OrderItems 表相关）。
不要忘记 GROUP BY 和 HAVING， 并按顾客名称对结果进行排序。
你可以使用简单的等联结或 ANSI的INNER JOIN 语法。
或者，如果你很勇敢，请尝试使用两种方式编写。
*/
--是的，我很勇敢！
/*
--（1）简单的等联结
SELECT cust_name,SUM(quantity*order_item*item_price) AS TotalPrice,
				SUM(quantity*order_item) AS OrderQuan--订单数量
FROM Customers,Orders,OrderItems
WHERE Customers.cust_id=Orders.cust_id 
	AND OrderItems.order_num=Orders.order_num 
Group BY Customers.cust_name
HAVING SUM(quantity*order_item)>1000;

--（2）等联结
SELECT cust_name,sum(quantity*order_item*item_price) AS TotalPrice,
				SUM(quantity*order_item) AS OrderQuan--订单数量
FROM Orders
INNER JOIN Customers ON Customers.cust_id=Orders.cust_id 
INNER JOIN OrderItems ON OrderItems.order_num=Orders.order_num 
Group BY Customers.cust_name
HAVING SUM(quantity*order_item)>1000;

*/
--题解写的是返回订单总价>=1000
SELECT cust_name,SUM(quantity*item_price) AS TotalPrice,
				SUM(quantity*item_price) AS OrderQuan--订单数量
FROM Customers,Orders,OrderItems
WHERE Customers.cust_id=Orders.cust_id 
	AND OrderItems.order_num=Orders.order_num 
Group BY Customers.cust_name
HAVING SUM(quantity*item_price)>=1000;

```