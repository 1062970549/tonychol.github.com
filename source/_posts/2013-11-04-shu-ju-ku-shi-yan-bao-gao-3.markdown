---
layout: post
title: "数据库实验报告3"
date: 2013-11-04 20:33
comments: true
categories: 
---

####本次实验报告的内容是按照要求，建立图书管理系统的数据库。

1. 建立现有图书管理系统数据库(bookmanager)，给出创建这些数据库对象的SQL命令。
	
	* 建立数据库（bookmanager）,并且建立表格 readers，borrowinf，books 和 readertype，最后插入相关数据：
	
			CREATE DATABASE bookmanager;
			USE bookmanager;
			CREATE TABLE readers(
    		编号 VARCHAR(8) NOT NULL,
    		姓名 VARCHAR(8) NOT NULL,
    		读者类型 int NOT NULL,
    		已借数量 int NOT NULL
    		);　
    		CREATE TABLE borrowinf(
    		读者编号 VARCHAR(8) NOT NULL,
    		图书编号 VARCHAR(15) NOT NULL,
    		借期 DATE NOT NULL,
    		还期 DATE //借期可以是空值！
    		);
    		CREATE TABLE books(
    		编号 VARCHAR(15) NOT NULL,
    		书名 VARCHAR(42) NOT NULL,
    		作者 VARCHAR(8) NOT NULL,
    		出版社 VARCHAR(28) NOT NULL,
    		定价 FLOAT(4,2) NOT NULL
    		);
    		 CREATE TABLE readertype
    		(类型编号 int,
    		类型名称 VARCHAR(8),
    		限借阅数量 int,
    		借阅期限 int
    		);
			
			
		![](http://d.pr/i/6NGB.png) 
		
		![](http://d.pr/i/pHzj.png)
	
	* 导入数据
		
		* 可以通过 txt 文档快速导入数据！
	
		
			格式如下：
		
				LOAD DATA LOCAL INFILE "路径名" INTO TABLE 表格名称(列名1,列名2,列名3,列名4，...);
				
			建立的 txt 文档如图所示：
			
			![](http://d.pr/i/FgpM.png)
			
			![](http://d.pr/i/DYot.png)
		
			例如，在本表格：
			
				LOAD DATA LOCAL INFILE "/Users/TonyChol/Desktop/reader.txt" INTO TABLE readers(编号,姓名,读者类型,已借数量);
				
				LOAD DATA LOCAL INFILE "/Users/TonyChol/Desktop/borrowinf.txt" INTO TABLE borrowinf(读者编号,图书编号,借期,还期);
				
				LOAD DATA LOCAL INFILE "/Users/TonyChol/Desktop/books.txt" INTO TABLE books(编号,书名,作者,出版社,定价);//这里存在一个问题，就是 FLOAT 的小数数位中，整数数位的参数能都省略？
				
				LOAD DATA LOCAL INFILE "/Users/TonyChol/Desktop/readertype.txt" INTO TABLE readertype(类型编号,类型名称,限借阅数量,借阅期限);
			
		
		![](http://d.pr/i/hEtY.png)
		
		![](http://d.pr/i/NwUq.png)
		
		![](http://d.pr/i/RZrv.png)
		
		![](http://d.pr/i/LuDc.png)
		
		

2. 写出完成下列操作的SQL命令。

	* 分别建立表readers、books上的主键索引。
		* 首先，主键算是一种索引，更通俗的讲，主键中的记录必须是「NOT NULL」的，而且是「UNIQUE」的。
		* 声明主键的方法有两种：
			* 在创建表的时候就给表加上主键：
			
					CREATE TABLE 表名 (
					字段描述等等等... , 
					PRIMARY KEY(某列名1,某列名2,...));
					
			* 也可以更新表结构时为表加上主键：
			
					ALTER TABLE 表名 ADD PRIMARY KEY (某列名1,某列名2,...);
					
		* 具体操作：˙
		
			* 在 Terminal 中输入：
			
					ALTER TABLE readers ADD PRIMARY KEY (编号);
					
				结果出现了意想不到的错误：
				
				提示：
						
						 Duplicate entry '20060600' for key 'PRIMARY'
						 
				![](http://d.pr/i/vCsu.png)
				
				后来 Google 之后发现，是原来的「readers」表格中，「编号」列里的数据重复，原因是在创建表的时候定义列的字符长度果断，导致插入的记录（也就是「编号」）重复。 必须修改列中的数据才能插入主键。
				
			* 在修改了列中的记录之后，再次输入插入主键的 SQL 语句，提示成功！
			
				![](http://d.pr/i/I5ks.png)
				
			* 在 books 表中添加主键，主键是「编号」：
			
				![](http://d.pr/i/ehuS.png)
	
2. 给出借阅超期信息单。
	
			
	
3. 查询编号为“2005060328”的读者的借阅信息。
	
	* 具体操作：
	
			SELECT 读者编号,姓名,图书编号,借期,还期,书名,作者,出版社,定价   
			FROM borrowinf,books,readers 
			WHERE borrowinf.读者编号 = readers.编号 AND  
			books.编号 = borrowinf.图书编号 AND  
			borrowinf.读者编号 = 2005060328;
	
	![](http://d.pr/i/3huH.png)

4. 查询姓名为“王立群”的读者的借阅信息。

	* 具体操作：
	
			SELECT 读者编号,姓名,图书编号,借期,还期,书名,作者,出版社,定价   
			FROM borrowinf,books,readers 
			WHERE borrowinf.读者编号 = readers.编号 
			AND  books.编号 = borrowinf.图书编号 
			AND  readers.姓名 = '王立群';
		
	![](http://d.pr/i/kxV4.png)
	

5. 查询类型为“研究生”的读者信息。

	* 一开始输入：
	
			SELECT   readers.编号, 姓名, 已借数量,  图书编号,  书名   
			 FROM readers, borrowinf, books 
			 WHERE readers.读者类型 = 2 AND  
			 readers.编号 = borrowinf. 读者编号 AND  
			 books.编号 = borrowinf. 图书编号;
			 
	* 结果报错：
	
			ERROR 1052 (23000): Column '编号' in field list is ambiguous
			
	* 是因为第一行的「编号」是模凌两可的，无法辨别，必须加上前缀！
	
			SELECT readers.编号, 姓名, 已借数量, 图书编号, 书名   
			FROM readers, borrowinf, books 
			WHERE readers.读者类型 = 2 AND  
			readers.编号 = borrowinf. 读者编号 AND  
			books.编号 = borrowinf. 图书编号;
		 
	![](http://d.pr/i/QLtD.png)

6. 查询书名中包含文字“程序设计”的图书信息。

	
7. 查询图书馆的藏书量。
	
8. 查询图书馆的图书总值。

9. 查询出版社的馆藏图书数量。

10. 查询“2007-1-1”至“2007-12-31”各类读者的借阅数量。
    
11. 查询“2007-1-1”至“2007-12-31”作者为“梁晓峰”的图书的借阅情况。

12. 查询借阅了作者为“张大海”的图书的读者编号和图书编号。

13. 查询所有教师的借阅图书情况，包括读者编号、姓名和已借数量。

14. 查询所有书名为“C语言程序设计”的读者编号和借阅日期。

15. 查询借阅日期与至少一位读者借阅日期相同的所有读者编号和姓名。

16. 查询所有研究生借阅图书的情况，包括姓名、已借数量、所借书名、借期和还期。

17. 查询“蓝天”出版社出版的图书借阅情况，包括读者编号、借期、还期。用连接查询和子查询两种方法实现。
	
18. 查询没有借阅“青山”出版社图书的读者编号。

	
	
		
		
		


