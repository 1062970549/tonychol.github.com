---
layout: post
title: "数据库实验报告2"
date: 2013-11-04 20:18
comments: true
categories: 
---


###上机实验2步骤

* 创建数据库
	* 登陆MySQL：在终端输入命令：
			
			mysql -p -u root
	* 输入命令：
	
			CREATE DATABASE mysqlTest;
			
	* 终端提示数据库创建成功：
	
			Query OK, 1 row affected (0.02 sec)

	<img src="http://d.pr/i/MTEO.png" width="640" />
	* 在终端输入以下命令以查看创建的数据库：
	
			SHOW DATABASES;
			
	* 可以看到出现了新创建的数据库。
	
	<img src="http://d.pr/i/vX4b.png" width="640" />
	
	* 删除数据库：
	
			DROP DATABASE mysqlTest;
			
		这样就删除了mysqlTest这个数据库。
		
	<img src="http://d.pr/i/f9pc.png" width="640">
	
	
* 创建表，更改表和删除表

	* 创建表
		1. 新建一个数据库：
				
				CREATE DATABASE schedule;
				
				
		2. 在这个数据库建表之前要使用此数据库：
		
				USE schedule;
				
				
		3.  输入命令：
		
				CREATE TABLE classTable
				(
					class_name VARCHAR(15) NOT NULL,
					class_date DATE NOT NULL
				);
				
			* 这里的classTable表示我要在「schedule」这个数据库里建立「classTable」的表格，括号里面的内容是表格的呈现的属性名称，对应的数据类型以及相应的数据属性，也就是「NULL」和「NOT NULL」。
			
		4. 创建表格之后可以查看这个表格的结构，输入命令：
		
				DESCRIBE classTable;
				
			可以在终端中得到一个表格（如图）：
			
	<img src="http://d.pr/i/fk8C.png" width="640">
	
	* 更改表
	
		1. 重命名表格
		
				ALTER TABLE classTable RENAME timetable;
				
			![](http://d.pr/i/hpNj.png)
			
		2. 向表中添加一列：
		
			
				ALTER TABLE timetable ADD id int;
				
			![](http://d.pr/i/anyQ.png)
		3. 删除表中的一列：
		
				ALTER TABLE timetable DROP id;
				
			![](http://d.pr/i/yZVP.png)
		
		4. 修改一个列的数据类型：
		
				ALTER TABLE timetable MODIFY class_name VARCHAR(50);
			
			![](http://d.pr/i/qhaw.png)
			
		5. 重命名一个列：
		
				ALTER TABLE timetable CHANGE COLUMN class_name class varchar(50);
				
			![](http://d.pr/i/wtMH.png)
			
		
		**最终修改过后的表格如图：**
		
		![](http://d.pr/i/ePeV.png)
			
			
	* 删除表
	
			DROP TABLE testtable;
			//此步为删除一个叫做「testtable」的表格。
		![](http://d.pr/i/w8PF.png)
			
		删除列已经在「更改表」中提及。
		
	* 对表格进行查询
	
		1. 基本查询：
		
				SELECT * from timetable;
				
			这个时候的截图是这个样子（提示Empty set）
	![](http://d.pr/i/vRon.png)
		
		这个时候应该给表格增加内容给：
		
		![](http://d.pr/i/CRgM.png)
		
		此时表timetable增加了一行，也就是一个记录。
		
		这个时候再次执行命令：
		
			SELECT * from timetable;
			
		就看到了新的记录。
		![](http://d.pr/i/Rj4R.png)
		
		
	2. WHERE 子句：
	
		* 先给表格添加额外的列：
		
		_________

			//添加「学分」属性
			ALTER TABLE timetable ADD credit int;
			//修改「class_date」成「class_day」，显示星期几
			ALTER TABLE timetable CHANGE COLUMN class_date class_day VARCHAR(10);
			desc timetable;
			
		![](http://d.pr/i/1vRC.png)
		
		* 再给表格加上几条记录：
		
		______
		
			INSERT INTO timetable VALUES ('changweifenfangcheng', 'Mon', 3);
			INSERT INTO timetable VALUES ('MySQL', 'Wed', 3);
			INSERT INTO timetable VALUES ('data analysis', 'Fri', 3);
			INSERT INTO timetable VALUES ('career planning','Wen', 2);
			SELECT * from timetable;
			
		![](http://d.pr/i/nj8y.png)				
		
		* 开始查询：
		
		___
		
			//查询2学分的课程
			SELECT * FROM timetable WHERE credit = 2;
		![](http://d.pr/i/7jUC.png)
		
			
		* 在表格中进行插入：
		
				//插入课程：马克思
				INSERT INTO timeable VALUES ('Makesi', 'Mon', 3);
				
		![](http://d.pr/i/9ijG.png)
		
		* 删除某行：
		
				//删除课程：马克思
				DELETE FROM timetable WHERE class = 'Makesi';
				
		![](http://d.pr/i/JVrU.png)
		
		* 更改数据：
		
				//修改课程「MySQL」为「Database」
				UPDATE timetable SET class = 'Database' WHERE class = 'MySQL';
				
		![](http://d.pr/i/WHrP.png)
		
		* 这里遇到了一些问题：**当中最大的问题就是无法插入带有中文字符的记录**:
		
			一旦输入中文，就会有如下提示：
			
				Incorrect string value: '\xE5\x90\x8C\xE5\xAD\xA6' 
				
			![](http://d.pr/i/wsfm.png)
			
		
		
			* 如何解决呢？？
			
			只须在创建数据库的时候加上参数：
			
					default charset utf8 collate utf8_unicode_ci;
					
			例如：
			
					create database cc default charset utf8 collate utf8_unicode_ci;
					
			这样子在插入中文记录的时候就不会再报错了。
					
			
			
				
		
* 视图
		
	首先视图是什么？
			
	>视图是一个虚拟的表，可以理解成现有表格的引用。
	>作用在于筛选有用的数据
	>每当查询视图时，MySQL通过使用 SQL 语句来重建数据。
		
	* 创建视图的格式：
			
			CREATE VIEW view_name AS
			SELECT column_name(s)
			FROM table_name
			WHERE ......
				
	* 具体操作：
			
			CREATE VIEW threeCredit AS
			SELECT class 
    		FROM timetable
    		WHERE credit = 3;
    		
    ![](http://d.pr/i/NbK.png)
    	
    * 再删除表中的「抽象代数」，试试看更新「threeCredit」视图：
    		
    		SELECT * from timetable;
    			
    ![](http://d.pr/i/GHKm.png)
    		
    		DELETE FROM timetable WHERE class = '抽象代数';
    				
    ![](http://d.pr/i/pOIN.png)
    		
    		SELECT * FROM threeCredit;
    		//查询视图；
    				
    ![](http://d.pr/i/EI3o.png)
    		
    **这说明只要原表格的数据更改了变化了，那么视图里的数据也会更着改变，即视图可以看做是原表格的引用！**
    		
    		
   * 视图的更新：
    	
    * 更新视图的 SQL 语句格式：
    			
    		ALTER VIEW view_name AS
			SELECT column_name(s)
			FROM table_name
			WHERE ......
					
	* 具体操作（给原来的视图加上 class_day 一列）
			
			ALTER VIEW threeCredit AS
			SELECT class,class_day
			FROM timetable
			WHERE credit = 3;
						
	![](http://d.pr/i/HknD.png)
			
				
* 索引的建立
		
	* 「索引」的用途：
				
		* 在表中创建索引，以便更加快速高效地查询数据。
				
	* 索引建立的 SQL 语句格式：
			
			CREATE INDEX index_name
			ON table_name (column_name)
					
	* **如果希望以降序(升序)索引某个列中的值，可以在列名称之后添加保留字 DESC（ASC）：**
			
	* **假如希望索引不止一个列，也可以在括号中列出这些列的名称，用逗号隔开：**
			
	* 具体操作:
			
		* 创建一个简单的索引，名为 "classIndex"，在 timetable 表的 class 列：
				
				CREATE INDEX classIndex
				ON timetable(class);
						
		![](http://d.pr/i/z5J3.png)
			
		* 创建多列的索引，加上排序：
		
				CREATE INDEX classIndexDESC
				ON timetable(class,credit DESC);		
		
		![](http://d.pr/i/kjQk.png)
		
		
		* 删除索引：
		
				ALTER TABLE timetable DROP INDEX classIndexDESC;
				
		![](http://d.pr/i/rUdz.png)
		
		
		
		
			
			
		
			
	
					
    	
    		
    		
    				
    		

		
		
			
				
		
	
		
			

