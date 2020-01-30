---
title: SQL基础篇
date: 2020-01-02 10:16:02
tags:
  - SQL
categories: CS数据库
mathjax: true
---
本文对SQL内容进行了一些梳理
**来源声明**：
来源来自日本MICK所著的SQL教程，有能力的请去社区支持正版，文末附有电子书供学习使用

# 绪论-搭建SQL的学习环境
## PostgreSQL的安装和连接设置
从 [PostgreSQL官网](https://www.enterprisedb.com/downloads/postgresql) 下载合适自己设备的版本	
如果仅供本地使用
在 C:\Program Files\PostgreSQL\9.5\data 路径中将
```
listen_addresses = '*'
```
修改为
```
listen_addresses = 'localhost'
```
登录SQL
打开cmd
输入
```
C:\PostgreSQL\9.5\bin\psql.exe –U postgres
```
窗口显示出“postgres=#”，意味着连接成功了。 下面就可以执行 SQL 语句了。

**创建学习使用的数据库**
1. 执行创建数据库的SQL语句
```
CREATE DATABASE shop;
```
2. 结束psql 
数据库创建成功之后，结束 psql。为了结束 psql， 需要输入“\q”，然后按下回车键。这样就切断了与 postgreSQL 的连接，返回到命令提示符窗口

**连接学习用的数据库（登录）**
登录上一步输入的shop
```
C:\PostgreSQL\9.5\bin\psql.exe –U postgres –d shop
```
d是指定某一个数据库的意思 

# 数据库和SQL
<!--more-->
## 数据库是什么
	● 数据库是将大量数据保存起来，通过计算机加工而成的可以进行高效访问 的数据集合。	● 用来管理数据库的计算机系统称为数据库管理系统（DBMS）。	● 通过使用DBMS，多个用户便可安全、简单地操作大量数据。	● 数据库有很多种类，本书将介绍如何使用专门的SQL语言来操作关系数 据库。	● 关系数据库通过关系数据库管理系统（RDBMS）进行管理。

DBMS种类：
层次数据库（Hierarchical Database，HDB） 最古老的数据库之一，它把数据通过层次结构（树形结构）的方式表现出来。
关系数据库（Relational Database，RDB），和 Excel 工作表一样，它也采用由行和列组成的二维表来 管理数据，所以简单易懂，还使用专门的 SQL（Structured Query Language，结构化查询语言）对数据进行操作。
面向对象数据库（Object Oriented Database，OODB） 编程语言当中有一种被称为面向对象语言的语言。把数据以及对数据的操作集合起来以对象为单位进行管理，因此得名。面向对象数据库就是用来保存这些对象的数据库。
XML数据库（XML Database，XMLDB） 最近几年，XML作为在网络上进行交互的数据的形式逐渐普及起来。 XML 数据库可以对 XML 形式的大量数据进行高速处理。
键值存储系统（Key-Value Store，KVS） 这是一种单纯用来保存查询所使用的主键（Key）和值（Value）的组 合的数据库。可以把它想象成关联数组或者散列 （hash）。近年来，随着键值存储系统被应用到Google 等需要对大量数据 进行超高速查询的 Web 服务当中，它正逐渐为人们所关注。
## 数据库的结构
● RDBMS通常使用客户端/服务器这样的系统结构。	● 通过从客户端向服务器端发送SQL语句来实现数据库的读写操作。	● 关系数据库采用被称为数据库表的二维表来管理数据。	● 数据库表由表示数据项目的列（字段）和表示一条数据的行（记录）所组 成，以记录为单位进行数据读写。	● 本书将行和列交汇的方格称为单元格，每个单元格只能输入一个数据。

## SQL概要
● SQL是为操作数据库而开发的语言。	● 虽然SQL也有标准，但实际上根据RDBMS的不同SQL也不尽相同。	● SQL通过一条语句来描述想要进行的操作，发送给RDBMS。	● 原则上SQL语句都会使用分号结尾。	● SQL根据操作目的可以分为DDL、DML和DCL。
SQL 用关键字、表名、列名等组合而成的一条语句（SQL 语句）来 描述操作的内容。关键字是指那些含义或使用方法已事先定义好的英语单
词，存在包含“对表进行查询”或者“参考这个表”等各种意义的关键字。 根据对 RDBMS 赋予的指令种类的不同，SQL 语句可以分为以下三类。
●DDL
	DDL（Data	Definition	Language，数据定义语言）		用来创建或者删除存储 数据用的数据库以及数据库中的表等对象。DDL 包含以下几种指令。
CREATE：
 创建数据库和表等对象 DROP： 删除数据库和表等对象 ALTER： 修改数据库和表等对象的结构
●DML
	DML（Data	Manipulation	Language，数据操纵语言）		用来查询或者变更 表中的记录。DML 包含以下几种指令。
SELECT：查询表中的数据 INSERT：向表中插入新数据 UPDATE：更新表中的数据 DELETE：删除表中的数据
●DCL
	DCL（Data	Control	Language，数据控制语言）		用来确认或者取消对数据 库中的数据进行的变更。除此之外，还可以对 RDBMS的用户是否有权限 操作数据库中的对象（数据库表等）进行设定。DCL 包含以下几种指令。
COMMIT： 确认对数据库中的数据进行的变更 ROLLBACK：
 取消对数据库中的数据进行的变更 GRANT： 赋予用户操作权限 REVOKE： 取消用户的操作权限
实际使用的 SQL 语句当中有 90% 属于 DML

**SQL的基本书写规则**
■SQL语句要以分号（;）结尾 
■SQL语句不区分大小写 
为了理解方便●	关键字大写 ●	表名的首字母大写 ●	其余（列名等）小写
插入到表中的数据是区分大小写的。例如，在操作过程中，数据 Computer、COMPUTER 或computer，三者是不一样的。
■常数的书写方式是固定的 
字符串和日期常数需要使用单引号（'）括起来。 数字常数无需加注单引号（直接书写数字即可）。
■单词需要用半角空格或者换行来分隔 
单词之间需要使用半角空格或者换行符进行分隔。
**表的创建**
● 表通过CREATE TABLE语句创建而成。	● 表和列的命名要使用有意义的文字。	● 指定列的数据类型（整数型、字符型和日期型等）。	● 可以在表中设置约束（主键约束和NOT NULL约束等）。
**命名规则**
数据库名称、表名和列名等可以使用以下三种字符。	●	半角英文字母　　●	半角数字　　●	下划线（_）
此外，名称必须以半角英文字母开头。
名称不能重复。
删除表/列：DROP + 表/列的名字
表定义的更新：ALTER TABLE 语句
比如添加列/删除列
ALTER TABLE 表名 ADD/DROP 列名 VARCHAR（）；
变更名字：
ALTER TABLE 表名 RENAME TO 另外一个表名

插入数据示意：
```
SQL Server PostgreSQL
-- DML ：插入数据
BEGIN TRANSACTION;—————————①
INSERT INTO Product VALUES ('0001', 'T恤衫', '衣服',
1000, 500, '2009-09-20');
INSERT INTO Product VALUES ('0002', '打孔器', '办公用品',
500, 320, '2009-09-11');
INSERT INTO Product VALUES ('0003', '运动T恤', '衣服',
4000, 2800, NULL);
INSERT INTO Product VALUES ('0004', '菜刀', '厨房用具',
3000, 2800, '2009-09-20');
INSERT INTO Product VALUES ('0005', '高压锅', '厨房用具',
6800, 5000, '2009-01-15');
INSERT INTO Product VALUES ('0006', '叉子', '厨房用具',
500, NULL, '2009-09-20');
INSERT INTO Product VALUES ('0007', '擦菜板', '厨房用具',
880, 790, '2008-04-28');
INSERT INTO Product VALUES ('0008', '圆珠笔', '办公用品',
100, NULL,'2009-11-11');
COMMIT;
```

# 查询基础
## SELECT 语句基础
从表中选取数据select
通过select语句查询并选取必要数据--query
获取表格中的一列：
```
SELECT 列的名字
From table;
```
查询所有列 
```
SELECT *
From tablename;
```
关键词AS 设定别名
```
SELECT xxx_id AS id,
SELECT origin_name AS name；
```
这样列里面的名称就会 变化
注意 如果要设成中文就要加双引号 

常数的查询
```
SELECT '商品' AS string,
38            AS number,
'2009-02-24'  AS date,
product id,product name
FROM Product;
```
从结果中删除重复行
用DISTINCT 语句
```
SELECT DISTINCT product_type
from product;
```
对单列使用DISTINCT语句
```
SELECT DISTINC purchase_price
FROM Product;
```
对多列使用DISTINCT
```
SELECT DISTINC purchase_price，registe_date
FROM Product;
```

根据where语句选择记录,WHERE 用来比较得到的结果和选择的是否相等
语法为：
```
SELECT <列名>, ……
 FROM <表名>
 WHERE <条件表达式>;
```

```
SELECT product_name,product_type
FROM Product
WHERE Product_type = '衣服';
```
注释的书写方式
一行注释
```
--本SELECT语句会从结果中删除重复行
SELECT DISTINCT product_id,purchase_price
FROM Product;
```
多行注释
```
/*这是
注释*/
```
## 算法运算符和比较运算符
乘法运算
```
SELECT product_name,
sale_price,
sale_price*2 AS "sale_price_x2"
From Product;
```
NULL值怎么操作都是NULL

比较运算符
不等于：
如果要选择单价为五百的商品名称和商品种类，代码为：
```
SELECT product_name,product_type
From Product
WHERE sale_price = 500;
```
如果选择不为五百的，代码为：
```
SELECT product_name,product_type
From Product
WHERE sale_price <> 500;
```

大于等于：
```
SELECT product_name,product_type
From Product
WHERE sale_price >= 500;
```

小于（选择某某日期之前）：
```
SELECT product_name,product_type,regis_date
FROM Product
WHERE regist_data < '2009-09-27';
```
获得销售单价大于进货单价500元以上的记录：
```
SELECT product_name,sale_price,purchase_price
FROM Product
WHERE sale_price - purchase_price >=500;
```

对字符串使用不等号的注意事项：
在一组字符串中选出大于2的数据：
```
SELECT char
FROM Chars
WHERE chr>'2'
```
比较的时候会按字符串的逻辑进行比较 而是按照字典顺寻(比如顺序为1 10 11 2 222 3)

不能对NULL使用比较运算符
实际使用中如果一定要选取NULL的话,需要在条件语句后加上 IS NULL
希望选取的不是NULL的时候，在条件语句后加上IS NOT NULL
## 逻辑运算符
NOT运算符
```
SELECT product_name,sale_price,purchase_price
FROM Product
WHERE NOT sale_price - purchase_price >=500;
```
和＜是相等的

AND运算符 需要两侧查询都成立才成立
OR有一个条件成立就行
AND:
```
--商品种类为“厨房用具”+销售单价大于等于3000日元
SELECT product_name, purchase_price
FROM Prodcut
WHERE product_type = '厨房用具’
AND sale_price >= 3000;
```
OR:
```
--商品种类为“厨房用具”或销售单价大于等于3000日元
SELECT product_name, purchase_price
FROM Prodcut
WHERE product_type = '厨房用具’
AND sale_price >= 3000;
```
括号强化：
```
--使用括号，让运算符or先于and执行
SELECt product_name, product type, regist_date
FROM Product
WHERE product_type = '办公用品'
AND (regist date = '2009-09-11
or regist_date = '2009-09-20');
```
## 练习题
1. 编写一条 SQL 语句，从 Product（商品）表中选取出“登记日期（regist_
date）在 2009 年 4 月 28 日之后”的商品。查询结果要包含 product_
name 和 regist_date 两列。
A:
```
SELECT product_name, regist_date
  FROM Product
 WHERE regist_date > '2009-04-28';
```
2. 请说出对 Product 表执行如下 3 条 SELECT 语句时的返回结果
```
① SELECT *
 FROM Product
 WHERE purchase_price = NULL;
② SELECT *
 FROM Product
 WHERE purchase_price <> NULL;
③ SELECT *
 FROM Product
 WHERE product_name > NULL;
```
A: 略
3. SELECT 语句能够从 Product 表中取出“销
售单价（sale_price）比进货单价（purchase_price）高出 500
日元以上”的商品。请写出两条可以得到相同结果的 SELECT 语句。执行
结果如下所示。
执行结果
```
 product_name | sale_price | purchase_price
---------------+-------------+----------------
T恤衫         | 1000        | 500
运动T恤       | 4000        | 2800
高压锅        | 6800        | 5000
```
A:
```
-- SELECT语句①
SELECT product_name, sale_price, purchase_price
  FROM Product
 WHERE sale_price >= purchase_price + 500;


-- SELECT语句②
SELECT product_name, sale_price, purchase_price
  FROM Product
 WHERE sale_price - 500 >= purchase_price;
```
4. 请写出一条 SELECT 语句，从 Product 表中选取出满足“销售单价打九 折之后利润高于 100 日元的办公用品和厨房用具”条件的记录。查询结果 要包括 product_name 列、product_type 列以及销售单价打九折之 后的利润（别名设定为 profit）。
A:
提示：销售单价打九折，可以通过 sale_price 列的值乘以 0.9 获得，利润可 以通过该值减去 purchase_price 列的值获得。
```
SELECT product_name, product_type,
       sale_price * 0.9 - purchase_price AS profit
  FROM Product
 WHERE sale_price * 0.9 - purchase_price > 100
   AND (   product_type = '办公用品'
        OR product_type = '厨房用具');
```






# 聚合与排序
## 对表进行聚合查询
COUNT 数行数
```
SELECT COUNT(*)
FROM Product;
```
例子创建三行NULL的表格查询整个的行数和某一列的行数，结果分别为3和0
```
shop=# --创建新表格
shop=# CREATE TABLE NULLTBL
shop-# (col_1  INTEGER NULL);
CREATE TABLE
shop=# --插入三行NULL
shop=# BEGIN TRANSACTION;
BEGIN
shop=# INSERT INTO NULLTBL VALUES(NULL);
INSERT 0 1
shop=# INSERT INTO NULLTBL VALUES(NULL);
INSERT 0 1
shop=# INSERT INTO NULLTBL VALUES(NULL);
INSERT 0 1
shop=# COMMIT;
COMMIT
shop=#
shop=#
shop=# SELECT COUNT(*),COUNT(col_1)
shop-# FROM NULLTBL;
```
求和：
```
SELECT SUM（sale_price), SUM(purchase_price)
FROM Product;
```
取平均:
```
SELECT AVG(sale_price)
FROM Product;
```
取多列平均
```
SELECT AVG(sale_price), AVG(purchase_price)
FROM Product;
```
计算最值
```
SELECT MAX(sale_price), MIN(purchase_price)
FROM Product;
```
有时候需要先使用DISTINCT去重后再计算有多少种类
```
SELECT COUNT (DISTINCT product_type)
FROM Product;
```
和下面这个结果是不一样的
```
SELECT DISTINCT (COUNT product_type)
FROM Product;
```

## 对表进行分组
GROUP BY 子句可以进行分组
```
--按照商品种类统计数据
SELECT product_type, COUNT(*)
FROM Product
GROUP BY product_type;
```


注意一下：在书写的时候顺序是SELECT，FROM，WHERE，GROUP BY
执行的时候顺寻是FROM，WHERE，GROUP BY， SELECT


## 为聚合结果指定条件
书写顺序SELECT，FROM，WHERE，GROUP BY,HAVING
HAVING 子句
跟在group子句后面加条件
HAVING子句的构成要素
## 对查询结果进行排序
用ORDER BY字句
书写顺序SELECT，FROM，WHERE，GROUP BY,HAVING，ORDER BY

指定升序和降序用ASC和DESC 

可以指定多个排序键

排序键包含NULL时会在开头或者末尾进行汇总

SQL在DBMS内部执行顺序：FROM-WHERE-GROUP BY-HAVING-SELECT-ORDER BY

SELECT子句中未包含的子句也可以用ORDER BY里面用

注意不要使用列编号

# 数据更新（先搁置）
## 数据的插入（INSERT）
## 数据的删除（DELET)
## 数据的更新 UPDATE
## 事务

# 复杂查询
## 视图
视图和表：
区别是是否保存了实际的数据
视图保存的是SELECT语句 表中保存的是实际的数据
可以节省容量 也可以将频繁使用的SELECT保存成视图
视图数据会随着原表变化自动更新
表中需要UPDATE才能更新
创建视图的方法(CREATE VIEW)：
```
CREATE VIEW ProductSum (product_type, cny_product)
AS
SELECT product_type, COUNT(*)
FROM Product
GROUP BY product_type;
```
第一行是视图里的列名
第二行AS不能省 和 重命名不一样
下面语句是主体

在FROM子句中使用视图来代替表
```
SELECT product_type, cnt_product
FROM ProductSum;
```

在SQL里面，还可以以视图为基础创建多重视图
可以在刚刚那个视图ProductSum的基础上，再创建一个视图ProductSumJim
```
CREATE VIEW ProductSumJim(product_type, cnt_product)
AS
SELECT product_type, cnt_product
FROM ProductSum
WHERE product_type = '办公用品'
CREATE VIEW
SELECT product_type, cnt_product
FROM ProductSumJim;
```


视图的限制1--定义视图时不能使用ORDER BY子句

视图的限制2--对视图进行更新
删除视图 DROP VIEW + 视图的名字（如果不存在关联视图）
如果存在关联视图
就用
DROP VIEW 名字 CASCADE
可以递归删除

## 子查询
子查询和视图
 可以嵌套
```
SELECT product_id,product_name,sale_price
FROM Product
WHERE sale_price > (SELECT AVG(sale_price) FROM Prodcut)
```
子查询名称
为子查询设定名称需要用AS
有时可以省略

标量子查询：
返回单一值的子查询

标量子查询的书写位置：
```
SELECT product_id,
product_name,
sale_price,
(SELECT AVG(SALE_price)FROM Prodcut) AS avg_price --这里就是标量子查询 
FROM Product；
```
使用标量子查询绝对不能返回多行结果

关联子查询(可以对集合进行切分，结合条件一定写在嵌套内)
```
SELECT product_type, product_name, sale_price
FROM Product AS P1
WHERE sale_price > (SELECT AVG(sale_price)FROM Product AS P2 
WHERE P1.product_type =P2.product_type
GROUP BY Product_type);
```

# 函数、谓词、CASE表达式
## 各式各样的函数
函数的种类：算术 字符串 日期 转换 聚合 
 一些算术： ABS：绝对值 ；
 MOD：求余
 ```
 SELET n,p,
 MOD(n,p) AS mod_col
 FROM SampleMath;
 ```
 ROUND:四舍五入 m是对象数值，n是保留几位小数
 ```
 SELECT m,n
 ,
 ROUND(m,n)
 ```
 字符串：
 ```
 SELECT str1,str2,str3,
 str1||str2||str3 AS str_concat
 FROM SampleStr
 WHERE str1= "山田"
 ```
 以上是个拼接的例子

 UPPER 大写
 REPLACE 转换
 SUBSTRING 字符串的截取
 ```
 SELECT str1,
 SUBSTRING(str1 FROM 3 for 2) AS sub_str
 FROM SampleStr;
 ```
 从第3个位置截取2个字符（截取第三和第四位字符）


 日期函数：
 CURRENT_DATE
 ```
 SELECT CURRENT_DATE, CURRENT_TIME;
 --获取当前日期，时间
 ```
这个函数无法在SQL——SERVER中执行
CURRENT_TIMESTAMP可以在SQLserver中使用
EXTRACT可以看具体的

转换函数：
CAST
```
SELECT CAST('0001'AS INTEGER) AS int_col
```
变成1
```
SELECT CAST('2009-12-14' AS DATE) AS date_col
```
本来是字符串类型，变成日期类型
COALESCE（将NULL转化为其他值）
```
SELECT COALESCE(NULL,1) AS col_1
           COALSCE(NULL, "test", str) AS col_2
```
还可以用其变成其他值


## 谓词（predicate）
谓词的返回值是真值
LIKE 
BETWEEN
IS NULL
IS NOT NULL
IN
EXSIT
## CASE 表达式
```
CASE WHEN<求值表达式>THEN<表达式>
     WHEN<求值表达式>THEN<表达式>
     WHEN<求值表达式>THEN<表达式>

 END / ELSE<表达式>
```
case 表达式可以进行行列转换

# 集合运算 UNION INTERCEPT EXCEPT
## 表的加减法
UNION 语句 
每一个UNION查询必须有相同的字段个数 列的类型必须一致



**相关资源下载**：
 MICK-SQL基础教程[点击下载](download/MICK-SQL基础教程.pdf)
 MICK-SQL进阶教程[点击下载](download/MICK-SQL进阶教程.pdf)