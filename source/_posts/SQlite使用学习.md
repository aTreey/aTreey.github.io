---
title: SQL 使用
date: 2016-10-13 22:30:54
tags: SQL
---

## 常用SQL 语句

创建一张表

	create table if not exists t_product(productId integer, productName text, productPrice real);

删除一张表

	drop table if exists t_product;

插入一条记录

如果不写某个字段和对应值就会为NUll

	insert into t_product(productId, productName, productPrice) values (0001, 'iPhone7', 6100.8);


修改记录

	update t_product set productId = 20002 where productName = 'iPhone5s';

删除

	delete from t_product where productName = 'iphone5s';

**注意：条件语句不能单独存在，只能在修改／删除／查询之后**

## 简单查询

准备数据生成sql语句文件

	NSMutableString *mutableString = [NSMutableString string];
	int productIdValue = arc4random_uniform(1000);
	NSString *productNameValue = [NSString stringWithFormat:@"iPhone_%d",1 + arc4random_uniform(8)];
	CGFloat productPriceValue = 3000 + arc4random_uniform(1000);
	
	for (NSInteger i = 0; i < 1000; i++) {
	    [mutableString appendFormat:@"insert into t_product(productId, productName, productPrice) values (%d, %@, %f); \r\n", productIdValue, productNameValue, productPriceValue];
	}
	
	// 写入文件
	[mutableString writeToFile:@"/Users/Apeng/work/Demo/Sqlite/products.sql" atomically:YES encoding:NSUTF8StringEncoding error:NULL];


生成的sql语句文件

![](http://i1.piimg.com/567571/588e899a67c1cce6.png)

SQL 查询语句

	select *（表示字段）from t_product（表名）where(条件语句) productId > 100 (字段满足的条件) ;

	select * from t_product where productPrice < 5000; 

![](http://p1.bpimg.com/567571/54d2c6ade469b653.png)
or: 或

![](http://p1.bqimg.com/567571/c8cd40b4271ec880.png)

and: 且

![](http://i1.piimg.com/567571/a6b404e93f8bc0eb.png)

分页查询

limit 0,5 （ limit 索引，每页取得数据条数 ）

第一页 limit 0,5;

第二页 limit 5,5;

第三页 limit 10,5;

第四页 limit 15,5;

第N页 limit （N - 1）* 5

	select * from t_product where productName = 'iPhone_8' and productPrice < 3300 order by productPrice limit 5,5;

![](http://i1.piimg.com/567571/29e2799f64db4884.png)

排序查询

order by 默认是从小到大，一般是在某一个结果之后

默认升序

	// 默认升序
	select * from t_product where productName = 'iPhone_8' and productPrice < 3300 order by productPrice;

	// 降序
	select * from t_product where productName = 'iPhone_8' and productPrice < 3300 order by productPrice desc;


![](http://p1.bqimg.com/567571/c69e4c2790b900b6.png)

模糊查询

关键字搜索 ，% 是指通配符

SQL 顺序

	select * from 表名 条件语句（模糊匹配）排序语句 分页语句

	select * from t_product where productName like '%_4' order by productPrice limit 0,5;

![](http://i1.piimg.com/567571/4727877c1b85cb0e.png)


主键

数据库的约定俗称,自增长,建议创建表时增加主键，也可以之后再设计表

	CREATE TABLE IF NOT EXISTS t_class (id integer PRIMARY KEY, className text, classNO integer); 


外键约束

当两张表有关联时，需要使用外键约束，一张表中的字段名所对应的值只能来自于另一张表中

![](http://p1.bqimg.com/567571/6b8c31aca88003d0.png)

添加外键约束之后的变化

![](http://p1.bpimg.com/567571/4e0c9af7ec980748.png)


多表查询

有 `t_department`（部门表）和 `t_employee`(员工表)两张表

需求：在 `t_employee` 表中查出部门研发部的人员

1 嵌套查询：

先查根据部门名称查出部ID，

![](http://p1.bqimg.com/567571/29459ada951407a8.png)

然后再用部门ID查出员工表中的员工
![](http://i1.piimg.com/567571/0636fdca9e552d99.png)

以上两步的代码可合并

根据部门名称查出部门ID
	
	SELECT id from t_department WHERE departmentName = '研发部';

根据部门id查出这个部门的人员
	
	SELECT * from t_employee WHERE departmentID = 1;

合并后为
	
	select * from t_employee where departmentID = (SELECT id from t_department WHERE departmentName = '研发部'); 
	   
结果为：

![](http://i1.piimg.com/567571/fd184b9d631bb19f.png)


2. 链接查询

给表起别名，给字段起别名

	select employee.*, depart.departmentName deptN from t_department depart, t_employee employee;


/**多表查询之链接查询*/

	select t_employee.*, t_department.departmentName FROM t_department, t_employee;

![](http://i1.piimg.com/567571/d5bd63824decf7f6.png)

消除笛卡尔积

	select employee.*, depart.departmentName deptN from t_department depart, t_employee employee where employee.departmentID = depart.id and employee.departmentID = 1;

![](http://i1.piimg.com/567571/b326df9d3150885d.png)
