---
title: MySQL学习笔记
categories:
  - MySQL
tags:
  - MySQL
  - DDL
  - DML
  - DQL
author:
  - Jungle
date: 2021-03-23
---



# 数据库的基本概念

	1. 数据库的英文单词： DataBase 简称 ： DB
	2. 什么数据库？
		* 用于存储和管理数据的仓库。
	
	3. 数据库的特点：
		1. 持久化存储数据的。其实数据库就是一个文件系统
		2. 方便存储和管理数据
		3. 使用了统一的方式操作数据库 -- SQL




# MySQL数据库软件

	1. 安装
		* 参见《MySQL基础.pdf》
		* 或者网上找教程
	2. 卸载
		1. 去mysql的安装目录找到my.ini文件
			* 复制 datadir="C:/ProgramData/MySQL/MySQL Server 5.5/Data/"
		2. 卸载MySQL
		3. 删除C:/ProgramData目录下的MySQL文件夹（要删除干净！）。
		
	3. 配置
		* MySQL服务启动
			1. 手动。
			2. cmd--> services.msc 打开服务的窗口
			3. 使用管理员打开cmd
				* net start mysql : 启动mysql的服务
				* net stop mysql:关闭mysql服务
		* MySQL登录
			1. mysql -uroot -p密码
				- 默认登录本地
				- 输入-p后直接回车，会以密文方式输入密码！
			2. mysql -hip -uroot -p连接目标的密码
				- 远程连接
				- -h表示主机
	            - ip表示要连接的主机的ip地址
			3. mysql --host=ip --user=root --password=连接目标的密码
				- 同方式二，不过更详细！
				- --host=ip	 				# 主机ip
				- --user=root				# 用户名
				- --password=连接目标的密码	# 密码
		* MySQL退出
			1. exit
			2. quit
	
		* MySQL目录结构
			1. MySQL安装目录：basedir="D:\MySQL"
				* 配置文件 my.ini
			2. MySQL数据目录：datadir="C:/ProgramData/MySQL/MySQL Server 5.5/Data/"
				* 几个概念
					* 数据库：对应 文件夹
					* 表：对应 文件
					* 数据：对应 数据

**结构图：**![mysql](C:\Users\LeBro\Pictures\mysql.png)





# SQL

## SQL定义及分类（DDL, DML, DQL, DCL）

	1.什么是SQL？
		Structured Query Language：结构化查询语言
		其实就是定义了操作所有关系型数据库的规则。每一种数据库操作的方式存在不一样的地方，称为“方言”。
		
	2.SQL通用语法
		1. SQL 语句可以单行或多行书写，以分号结尾。
		2. 可使用空格和缩进来增强语句的可读性。
		3. MySQL 数据库的 SQL 语句不区分大小写，关键字建议使用大写。
		4. 3 种注释
			* 单行注释: -- 注释内容 或 # 注释内容(mysql 特有) 
				* -- 要带空格 （常用）
				* # 不带空格
			* 多行注释: /* 注释 */
		
	3. SQL分类
		1) DDL(Data Definition Language)数据定义语言
			用来定义数据库对象：数据库，表，列等。关键字：create, drop,alter 等
		2) DML(Data Manipulation Language)数据操作语言
			用来对数据库中表的数据进行增删改。关键字：insert, delete, update 等
		3) DQL(Data Query Language)数据查询语言
			用来查询数据库中表的记录(数据)。关键字：select, where 等
		4) DCL(Data Control Language)数据控制语言(了解)
			用来定义数据库的访问权限和安全级别，及创建用户。关键字：GRANT， REVOKE 等

**分类图**

![mysql_SQL](C:\Users\LeBro\Pictures\mysql_SQL.png)



## DDL:操作数据库、表

### 操作数据库 CRUD

#### Retrieve 查询

> R(Retrieve)：查询
> 	* 查询所有数据库的名称:
> 		* show databases;
> 	* 查询某个数据库的字符集:查询某个数据库的创建语句
> 		* show create database 数据库名称;

```sql
-- 查询所有数据库的名称:
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |	-- 描述MySQL的信息，并不是真正的表---视图，不存在对应的物理文件
| mysql              |  -- 核心数据
| performance_schema |  -- 对性能提升做操作的数据库 （以上都不要轻举妄动！）
| test               |  -- 测试数据库，随便玩。
+--------------------+
4 rows in set (0.00 sec)

-- 查询某个数据库的字符集 及查询某个数据库的创建语句
mysql> show create database mysql;
+----------+----------------------------------------------------------------+
| Database | Create Database                                                |
+----------+----------------------------------------------------------------+
| mysql    | CREATE DATABASE `mysql` /*!40100 DEFAULT CHARACTER SET utf8 */ |
+----------+----------------------------------------------------------------+
1 row in set (0.00 sec)
```

#### Create 创建

> 	C(Create):创建
> 		* 创建数据库：
> 			* create database 数据库名称;
> 		* 创建数据库，判断不存在，再创建：
> 			* create database if not exists 数据库名称;
> 		* 创建数据库，并指定字符集
> 			* create database 数据库名称 character set 字符集名;

```sql
-- 创建自己的测试数据库
mysql> create database db1;
Query OK, 1 row affected (0.01 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| db1                |
| mysql              |
| performance_schema |
| test               |
+--------------------+
5 rows in set (0.01 sec)

mysql> show create database db1;
+----------+--------------------------------------------------------------+
| Database | Create Database                                              |
+----------+--------------------------------------------------------------+
| db1      | CREATE DATABASE `db1` /*!40100 DEFAULT CHARACTER SET utf8 */ |
+----------+--------------------------------------------------------------+
1 row in set (0.01 sec)

-- 创建数据库，判断不存在，再创建
mysql> create database if not exists db1;
Query OK, 1 row affected, 1 warning (0.00 sec)	
-- 之前已经存在db1，则不创建！
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| db1                |
| mysql              |
| performance_schema |
| test               |
+--------------------+
5 rows in set (0.00 sec)

-- 不存在db2，则创建！
mysql> create database if not exists db2;
Query OK, 1 row affected (0.00 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| db1                |
| db2                |
| mysql              |
| performance_schema |
| test               |
+--------------------+
6 rows in set (0.01 sec)

-- 创建数据库，并指定字符集：
mysql> create database db3 character set gbk;
Query OK, 1 row affected (0.00 sec)

mysql> show create database db3;
+----------+-------------------------------------------------------------+
| Database | Create Database                                             |
+----------+-------------------------------------------------------------+
| db3      | CREATE DATABASE `db3` /*!40100 DEFAULT CHARACTER SET gbk */ |
+----------+-------------------------------------------------------------+
1 row in set (0.00 sec)
```

#### Update 修改

> U(Update):修改
> 		* 修改数据库的字符集
> 			* alter database 数据库名称 character set 字符集名称;

```sql
mysql> alter database db3 character set utf8;
Query OK, 1 row affected (0.01 sec)

mysql> show create database db3;
+----------+--------------------------------------------------------------+
| Database | Create Database                                              |
+----------+--------------------------------------------------------------+
| db3      | CREATE DATABASE `db3` /*!40100 DEFAULT CHARACTER SET utf8 */ |
+----------+--------------------------------------------------------------+
1 row in set (0.00 sec)
```

#### Delete 删除

> D(Delete):删除
> 		* 删除数据库
> 			* drop database 数据库名称;
> 		* 判断数据库存在，存在再删除
> 			* drop database if exists 数据库名称;

```sql
-- 删除数据库
mysql> drop database db3;
Query OK, 0 rows affected (0.01 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| db1                |
| db2                |
| mysql              |
| performance_schema |
| test               |
+--------------------+
6 rows in set (0.00 sec)

-- 判断数据库存在，存在再删除
mysql> drop database if exists db2;
Query OK, 0 rows affected (0.00 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| db1                |
| mysql              |
| performance_schema |
| test               |
+--------------------+
5 rows in set (0.00 sec)
```



#### 使用数据库

> 使用数据库
> 		* 查询当前正在使用的数据库名称
> 			* select database();
> 	    * 使用数据库
> 			* use 数据库名称;

```sql
-- 查询当前正在使用的数据库名称
mysql> select database();
+------------+
| database() |
+------------+
| NULL       |
+------------+
1 row in set (0.00 sec)

-- 使用数据库
mysql> use db1;
Database changed

mysql> select database();
+------------+
| database() |
+------------+
| db1        |
+------------+
1 row in set (0.00 sec)
```





### 操作表 CRUD

#### Retrieve 查询表

> R(Retrieve)：查询
> 		* 查询某个数据库中所有的表名称
> 	  * show tables;
> * 查询表结构
> 	* desc 表名;

```sql
-- 查询前，先使用
mysql> use mysql
Database changed
-- 查询某个数据库中所有的表名称
mysql> show tables;
+---------------------------+
| Tables_in_mysql           |
+---------------------------+
| columns_priv              |
| db                        |
| event                     |
| func                      |
| general_log               |
| help_category             |
| help_keyword              |
| help_relation             |
| help_topic                |
| host                      |
| ndb_binlog_index          |
| plugin                    |
| proc                      |
| procs_priv                |
| proxies_priv              |
| servers                   |
| slow_log                  |
| tables_priv               |
| time_zone                 |
| time_zone_leap_second     |
| time_zone_name            |
| time_zone_transition      |
| time_zone_transition_type |
| user                      |
+---------------------------+
24 rows in set (0.00 sec)

-- 查询表结构
mysql> desc host;
+-----------------------+---------------+------+-----+---------+-------+
| Field                 | Type          | Null | Key | Default | Extra |
+-----------------------+---------------+------+-----+---------+-------+
| Host                  | char(60)      | NO   | PRI |         |       |
| Db                    | char(64)      | NO   | PRI |         |       |
| Select_priv           | enum('N','Y') | NO   |     | N       |       |
| Insert_priv           | enum('N','Y') | NO   |     | N       |       |
| Update_priv           | enum('N','Y') | NO   |     | N       |       |
| Delete_priv           | enum('N','Y') | NO   |     | N       |       |
| Create_priv           | enum('N','Y') | NO   |     | N       |       |
| Drop_priv             | enum('N','Y') | NO   |     | N       |       |
| Grant_priv            | enum('N','Y') | NO   |     | N       |       |
| References_priv       | enum('N','Y') | NO   |     | N       |       |
| Index_priv            | enum('N','Y') | NO   |     | N       |       |
| Alter_priv            | enum('N','Y') | NO   |     | N       |       |
| Create_tmp_table_priv | enum('N','Y') | NO   |     | N       |       |
| Lock_tables_priv      | enum('N','Y') | NO   |     | N       |       |
| Create_view_priv      | enum('N','Y') | NO   |     | N       |       |
| Show_view_priv        | enum('N','Y') | NO   |     | N       |       |
| Create_routine_priv   | enum('N','Y') | NO   |     | N       |       |
| Alter_routine_priv    | enum('N','Y') | NO   |     | N       |       |
| Execute_priv          | enum('N','Y') | NO   |     | N       |       |
| Trigger_priv          | enum('N','Y') | NO   |     | N       |       |
+-----------------------+---------------+------+-----+---------+-------+
20 rows in set (0.01 sec)
```



#### Create 创建表

> - 语法：
>   		create table 表名(
>     			列名1 数据类型1,
>     			列名2 数据类型2,
>     			....
>     			列名n 数据类型n
>     		);
>
>   * 注意：最后一列，不需要加逗号（,）
>  * 创建表
>       create table student(
>         id int,
>         name varchar(32),
>         age int ,
>         score double(4,1),
>         birthday date,
>         insert_time timestamp
>        );
> * 复制表：
> 	* create table 表名 like 被复制的表名;	

##### 常用数据类型

> 整数
>
> 取值范围如果加了unsigned，则最大值翻倍，如：tinyint unsigned的取值范围为(0~256)。

| 整数类型  | 数值范围                             |
| :-------: | ------------------------------------ |
|  tinyint  | 1个字节 范围(-128~127)               |
| smallint  | 2个字节 范围(-32768~32767)           |
| mediumint | 3个字节 范围(-8388608~8388607)       |
|    int    | 4个字节 范围(-2147483648~2147483647) |

> 小数
>
> 设一个字段定义为float(6,3)，如果插入一个数123.45678,实际数据库里存的是123.457，但总个数还以实际为准，即6位。
>
> 整数部分最大是3位，如果插入数12.123456，存储的是12.1234，如果插入12.12，存储的是12.1200.

|   小数类型   | 数值范围                                                     |
| :----------: | :----------------------------------------------------------- |
|  float(m,d)  | 单精度浮点型  8位精度(4字节)   m总个数，d小数位              |
| double(m,d)  | 双精度浮点型  16位精度(8字节)   m总个数，d小数位             |
| decimal(m,d) | 浮点型在数据库中存放的是近似值，而定点类型在数据库中存放的是精确值；参数m<65 是总个数，d<30且 d<m 是小数位。 |

> 日期
>
> 若定义一个字段为timestamp，这个字段里的时间数据会随其他字段修改的时候自动刷新，
>
> 所以这个数据类型的字段可以存放这条记录最后被修改的时间。

| 日期时间类型 | 含义                                                         |
| :----------: | ------------------------------------------------------------ |
|     date     | 日期 '2008-12-2'                                      即：yyyy-MM-dd |
|     time     | 时间 '12:25:36'                                         即：HH:mm:ss |
|   datetime   | 日期时间 '2008-12-2 22:06:44'              即：yyyy-MM-dd  HH:mm:ss |
|  timestamp   | （时间戳）自动存储记录修改时间         即：yyyy-MM-dd  HH:mm:ss |

> 字符串
>
> - char和varchar：
>
>   1. char(n) 若存入字符数小于n，则以空格补于其后，查询之时再将空格去掉。所以char类型存储的字符串末尾不能有空格，varchar不限于此。 
>
>   2. char(n) 固定长度，char(4)不管是存入几个字符，都将占用4个字节，varchar是存入的实际字符数+1个字节（n<=255）或2个字节(n>255)，所以varchar(4),存入3个字符将占用4个字节。 
>
>   3. char类型的字符串检索速度要比varchar类型的快。
>
> - varchar和text： 
>
>   1. varchar可指定n，text不能指定，内部存储varchar是存入的实际字符数+1个字节（n<=255）或2个字节(n>255)，text是实际字符数+2个字节。 
>
>   2. text类型不能有默认值。 
>
>   3. varchar可直接创建索引，text创建索引要指定前多少个字符。varchar查询速度快于text,在都创建索引的情况下，text的索引似乎不起作用。

|   字符串   | 含义                            |
| :--------: | ------------------------------- |
|  char(n)   | 固定长度，最多255个字符         |
| varchar(n) | 可变长度，最多65535个字符       |
|  tinytext  | 可变长度，最多255个字符         |
|    text    | 可变长度，最多65535个字符       |
| mediumtext | 可变长度，最多2的24次方-1个字符 |
|  longtext  | 可变长度，最多2的32次方-1个字符 |

##### 数据类型的属性

|       关键字       | 含义                     |
| :----------------: | ------------------------ |
|        NULL        | 数据列可包含NULL值       |
|      NOT NULL      | 数据列不允许包含NULL值   |
|      DEFAULT       | 默认值                   |
|    PRIMARY KEY     | 主键                     |
|   AUTO_INCREMENT   | 自动递增，适用于整数类型 |
|      UNSIGNED      | 无符号                   |
| CHARACTER SET name | 指定一个字符集           |

##### Create 创建表

**学生表结构图：**

<img src="C:\Users\LeBro\Pictures\studenttable.png" alt="studenttable" style="zoom: 50%;" />

```sql
-- 先建立一个新的测试数据库
mysql> create database db_Test;
Query OK, 1 row affected (0.00 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| db_test            |
| mysql              |
| performance_schema |
| test               |
+--------------------+
5 rows in set (0.00 sec)

-- 创建学生表
mysql> use db_test;
Database changed

mysql> create table student(
    ->                     id int,
    ->                     name varchar(32),
    ->                     age int ,
    ->                     score double(4,1),
    ->                     birthday date, -- 只需要年月日
    ->                     insert_time timestamp -- 最后一行不要有逗号
    ->                 );
Query OK, 0 rows affected (0.01 sec)

-- 查询刚刚建立的表
mysql> show tables;
+-------------------+
| Tables_in_db_test |
+-------------------+
| student           |
+-------------------+
1 row in set (0.00 sec)
-- 查询表结构
mysql> desc student;
+-------------+-------------+------+-----+-------------------+-----------------------------+
| Field       | Type        | Null | Key | Default           | Extra                       |
+-------------+-------------+------+-----+-------------------+-----------------------------+
| id          | int(11)     | YES  |     | NULL              |                             |
| name        | varchar(32) | YES  |     | NULL              |                             |
| age         | int(11)     | YES  |     | NULL              |                             |
| score       | double(4,1) | YES  |     | NULL              |                             |
| birthday    | date        | YES  |     | NULL              |                             |
| insert_time | timestamp   | NO   |     | CURRENT_TIMESTAMP | on update CURRENT_TIMESTAMP |
+-------------+-------------+------+-----+-------------------+-----------------------------+
6 rows in set (0.01 sec)

-- 复制表
mysql> create table stu like student;
Query OK, 0 rows affected (0.01 sec)

mysql> show tables;
+-------------------+
| Tables_in_db_test |
+-------------------+
| stu               |
| student           |
+-------------------+
2 rows in set (0.00 sec)
```



#### Delete 删除表

> D(Delete):删除
> 		* drop table 表名;
> 		* drop table  if exists 表名 ;

```sql
-- 删除前，先复制
mysql> create table stu like student;
Query OK, 0 rows affected (0.01 sec)

mysql> show tables;
+-------------------+
| Tables_in_db_test |
+-------------------+
| stu               |
| student           |
+-------------------+
2 rows in set (0.00 sec)

-- 1. 直接删除
mysql> drop table stu;
Query OK, 0 rows affected (0.01 sec)

mysql> show tables;
+-------------------+
| Tables_in_db_test |
+-------------------+
| student           |
+-------------------+
1 row in set (0.00 sec)

-- 2. 若存在再删除
mysql> drop table if exists stu;
Query OK, 0 rows affected, 1 warning (0.00 sec)
```

#### Update 修改表

##### 修改表名

> alter table 表名 rename to 新的表名;

```sql
mysql> alter table student rename to stu;
Query OK, 0 rows affected (0.03 sec)

mysql> show tables;
+-------------------+
| Tables_in_db_test |
+-------------------+
| stu               |
+-------------------+
1 row in set (0.00 sec)
```

##### 修改表的字符集

> alter table 表名 character set 字符集名称;

```sql
-- 1. 先显示
mysql> show create table stu;
+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table | Create Table                                                                                                                                                                                                                                                                                                         |
+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| stu   | CREATE TABLE `stu` (
  `id` int(11) DEFAULT NULL,
  `name` varchar(32) DEFAULT NULL,
  `age` int(11) DEFAULT NULL,
  `score` double(4,1) DEFAULT NULL,
  `birthday` date DEFAULT NULL,
  `insert_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
) ENGINE=InnoDB DEFAULT CHARSET=utf8 |
+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)

-- 2. 再修改
mysql> alter table stu character set gbk;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> show create table stu;
+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table | Create Table                                                                                                                                                                                                                                                                                                                           |
+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| stu   | CREATE TABLE `stu` (
  `id` int(11) DEFAULT NULL,
  `name` varchar(32) CHARACTER SET utf8 DEFAULT NULL,
  `age` int(11) DEFAULT NULL,
  `score` double(4,1) DEFAULT NULL,
  `birthday` date DEFAULT NULL,
  `insert_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
) ENGINE=InnoDB DEFAULT CHARSET=gbk |
+-------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

##### 添加一列

> alter table 表名 add 列名 数据类型;

```sql
mysql> alter table stu add gender varchar(10);
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc stu;
+-------------+-------------+------+-----+-------------------+-----------------------------+
| Field       | Type        | Null | Key | Default           | Extra                       |
+-------------+-------------+------+-----+-------------------+-----------------------------+
| id          | int(11)     | YES  |     | NULL              |                             |
| name        | varchar(32) | YES  |     | NULL              |                             |
| age         | int(11)     | YES  |     | NULL              |                             |
| score       | double(4,1) | YES  |     | NULL              |                             |
| birthday    | date        | YES  |     | NULL              |                             |
| insert_time | timestamp   | NO   |     | CURRENT_TIMESTAMP | on update CURRENT_TIMESTAMP |
| gender      | varchar(10) | YES  |     | NULL              |                             |
+-------------+-------------+------+-----+-------------------+-----------------------------+
7 rows in set (0.01 sec)
```



##### 修改列名称 类型

> alter table 表名 change 列名 新列名 新数据类型;
> alter table 表名 modify 列名 新数据类型;

```sql
-- 同时修改列名和类型
mysql> alter table stu change gender sex varchar(20);
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc stu;
+-------------+-------------+------+-----+-------------------+-----------------------------+
| Field       | Type        | Null | Key | Default           | Extra                       |
+-------------+-------------+------+-----+-------------------+-----------------------------+
| id          | int(11)     | YES  |     | NULL              |                             |
| name        | varchar(32) | YES  |     | NULL              |                             |
| age         | int(11)     | YES  |     | NULL              |                             |
| score       | double(4,1) | YES  |     | NULL              |                             |
| birthday    | date        | YES  |     | NULL              |                             |
| insert_time | timestamp   | NO   |     | CURRENT_TIMESTAMP | on update CURRENT_TIMESTAMP |
| sex         | varchar(20) | YES  |     | NULL              |                             |
+-------------+-------------+------+-----+-------------------+-----------------------------+
7 rows in set (0.01 sec)

-- 只改类型
mysql> alter table stu modify sex varchar(10);
Query OK, 0 rows affected (0.04 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc stu;
+-------------+-------------+------+-----+-------------------+-----------------------------+
| Field       | Type        | Null | Key | Default           | Extra                       |
+-------------+-------------+------+-----+-------------------+-----------------------------+
| id          | int(11)     | YES  |     | NULL              |                             |
| name        | varchar(32) | YES  |     | NULL              |                             |
| age         | int(11)     | YES  |     | NULL              |                             |
| score       | double(4,1) | YES  |     | NULL              |                             |
| birthday    | date        | YES  |     | NULL              |                             |
| insert_time | timestamp   | NO   |     | CURRENT_TIMESTAMP | on update CURRENT_TIMESTAMP |
| sex         | varchar(10) | YES  |     | NULL              |                             |
+-------------+-------------+------+-----+-------------------+-----------------------------+
7 rows in set (0.01 sec)
```



##### 删除列

> alter table 表名 drop 列名;

```sql
mysql> alter table stu drop sex;
Query OK, 0 rows affected (0.01 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> desc stu;
+-------------+-------------+------+-----+-------------------+-----------------------------+
| Field       | Type        | Null | Key | Default           | Extra                       |
+-------------+-------------+------+-----+-------------------+-----------------------------+
| id          | int(11)     | YES  |     | NULL              |                             |
| name        | varchar(32) | YES  |     | NULL              |                             |
| age         | int(11)     | YES  |     | NULL              |                             |
| score       | double(4,1) | YES  |     | NULL              |                             |
| birthday    | date        | YES  |     | NULL              |                             |
| insert_time | timestamp   | NO   |     | CURRENT_TIMESTAMP | on update CURRENT_TIMESTAMP |
+-------------+-------------+------+-----+-------------------+-----------------------------+
6 rows in set (0.01 sec)
```

### SQLYog 客户端图形化工具

> 注册码：
>
> ​	姓名（Name）：cr173
>
> ​	序列号（Code）：8d8120df-a5c3-4989-8f47-5afc79c56e7c
>
> ​	
>
> ​	姓名（Name）：cr173
>
> ​	序列号（Code）：59adfdfe-bcb0-4762-8267-d7fccf16beda
>
> ​	
>
> ​	姓名（Name）：cr173
>
> ​	序列号（Code）：ec38d297-0543-4679-b098-4baadf91f983



## DML：增删改表中数据

### 添加数据

> 	* 语法：
> 		* insert into 表名(列名1,列名2,...列名n) values(值1,值2,...值n);
> 	* 注意：
> 		1. 列名和值要一 一对应。
> 		2. 如果表名后，不定义列名，则默认给所有列添加值
> 			- insert into 表名 values(值1,值2,...值n);
> 		3. 除了数字类型，其他类型需要使用引号(单双都可以)引起来

```sql
-- 第一种方式（指定列添加值）
insert INto stu(id,name,age) value(1, 'Jungle', 26);
select * from stu;
-- SQLyog数据显示
id	name	age	score	birthday	insert_time
1	Jungle	26	\N	\N	2021-03-23 16:20:42

-- 第二种方式（默认给所有列添加值）
insert INto stu value(2, 'James', 32, 100, null, null);
select * from stu;
-- SQLyog数据显示
id	name	age	score	birthday	insert_time
1	Jungle	26	\N	\N	2021-03-23 16:20:42
2	James	32	100.0	\N	2021-03-23 16:25:25
-- 单双引号都可以
insert INto stu value(3, 'Lebro', 37, 100, "1984-12-30", null);
select * from stu;
-- SQLyog数据显示
id	name	age	score	birthday	insert_time
1	Jungle	26	\N	\N	2021-03-23 16:20:42
2	James	32	100.0	\N	2021-03-23 16:25:25
3	Lebro	37	100.0	1984-12-30	2021-03-23 16:30:55
```



### 删除数据

>  * 语法：
>      * delete from 表名 [where 条件]
>      * 注意：
>        1. 如果不加条件，则删除表中所有记录。
>        2. drop 对数据库和表整体完全删除；而delete对表中的记录删除，但结构还在。
>        3. 如果要删除所有记录
>          1. delete from 表名; 
>             - 不推荐使用。
>             - 有多少条记录就会执行多少次删除操作
>          2. TRUNCATE TABLE 表名; 
>             - 推荐使用，效率更高 
>             - 先删除表，然后再创建一张一模一样的空表。

```sql
-- 删除表中满足条件的记录
DELETE FROM stu WHERE id=1;
SELECT * FROM stu;
-- SQLyog数据显示
id	name	age	score	birthday	insert_time
2	James	32	100.0	\N	2021-03-23 16:25:25
3	Lebro	37	100.0	1984-12-30	2021-03-23 16:30:55

-- 删除所有记录
TRUNCATE TABLE stu;
SELECT * FROM stu;
-- SQLyog数据显示
id	name	age	score	birthday	insert_time

```



### 修改数据

> 	* 语法：
> 		* update 表名 set 列名1 = 值1, 列名2 = 值2,... [where 条件];
> 	* 注意：
> 		1. 如果不加任何条件，则会将表中所有记录全部修改。
> 		2. alter 是修改数据库和表的结构，update 时修改表的内容。

```sql
-- 先执行插入数据
INSERT INTO stu(id,NAME,age) VALUE(1, 'Jungle', 26);
INSERT INTO stu VALUE(2, 'James', 32, 100, NULL, NULL);
INSERT INTO stu VALUE(3, 'Lebro', 37, 100, '1984-12-30', NULL);

-- 再修改数据：

-- 1 修改单列数据
update stu set age=37 where id=2;
SELECT * FROM stu;
-- SQLyog数据显示
id	name	age	score	birthday	insert_time
1	Jungle	26	\N	\N	2021-03-23 16:45:29
2	James	37	100.0	\N	2021-03-23 16:47:05
3	Lebro	37	100.0	1984-12-30	2021-03-23 16:45:29

-- 2 修改多列数据
UPDATE stu SET score=100,birthday='1995-10-01' WHERE id=1;
SELECT * FROM stu;
-- SQLyog数据显示
id	name	age	score	birthday	insert_time
1	Jungle	26	100.0	1995-10-01	2021-03-23 16:50:22
2	James	37	100.0	\N	2021-03-23 16:47:05
3	Lebro	37	100.0	1984-12-30	2021-03-23 16:45:29
```





## DQL：查询表中的记录

> 语法：
>
> * select * from 表名;
>   - select
>     	字段列表
>   - from
>     	表名列表
>   - where
>     	条件列表
>   - group by
>     	分组字段
>   - having
>     	分组之后的条件
>   - order by
>     	排序
>   - limit
>     	分页限定
>



### 基础查询

> - 进行查询之前的数据准备：
>
>   CREATE TABLE student (
>     id int,
>     name varchar(20),
>     age int,
>     gender varchar(5),
>     address varchar(100),
>     math int,
>     english int
>   );
>
>   INSERT INTO student(id,NAME,age,gender,address,math,english)
>   VALUES(1,'马云',55,'男','杭州',66,78),
>   	(2,'马化腾',45,'女','深圳',98,87),
>   	(3,'马景涛',55,'男','香港',56,77),
>   	(4,'柳岩',20,'女','湖南',76,65),
>   	(5,'柳青',20,'男','湖南',86,NULL),
>   	(6,'刘德华',57,'男','香港',99,99),
>   	(7,'马德',22,'女','香港',99,99),
>   	(8,'德玛西亚',18,'男','南京',56,65);
>   	
>
> - select * from student;
>
>   id	name	age	gender	address	math	english
>   1	马云	55	男	杭州	66	78
>   2	马化腾	45	女	深圳	98	87
>   3	马景涛	55	男	香港	56	77
>   4	柳岩	20	女	湖南	76	65
>   5	柳青	20	男	湖南	86	\N
>   6	刘德华	57	男	香港	99	99
>   7	马德	22	女	香港	99	99
>   8	德玛西亚	18	男	南京	56	65

#### 多个字段的查询

> select 字段名1，字段名2... from 表名；
>
> - 注意：如果查询所有字段，则可以使用*来替代字段列表。

```sql
-- 查询姓名和年龄

-- 写法一：
SELECT NAME, age FROM student;

-- 写法二（推荐）：
SELECT 
	NAME, -- 姓名
	age   -- 年龄
FROM 
	student; -- 学生表
	
-- SQLyog数据显示
name	age
马云	55
马化腾	45
马景涛	55
柳岩	20
柳青	20
刘德华	57
马德	22
德玛西亚	18
```



#### 去除重复

> distinct
>
> - 注意：去重要保证记录之间完全一样！

```sql
-- 去除前：
SELECT address FROM student;

address
杭州
深圳
香港
湖南
湖南
香港
香港
南京

-- 去除后：
SELECT DISTINCT address FROM student;

address
杭州
深圳
香港
湖南
南京

-- 两条记录要完全一致才能去重！
SELECT NAME, address FROM student;
name	address
马云	杭州
马化腾	深圳
马景涛	香港
柳岩	湖南
柳青	湖南
刘德华	香港
马德	香港
德玛西亚	南京

SELECT DISTINCT NAME, address FROM student;
name	address
马云	杭州
马化腾	深圳
马景涛	香港
柳岩	湖南
柳青	湖南
刘德华	香港
马德	香港
德玛西亚	南京
```



#### 计算列

> - 一般可以使用四则运算计算一些列的值。（一般只会进行数值型的计算）
> - ifnull(表达式1,表达式2)：
>     * 表达式1：判断位于表达式1的字段是否为null；
>     * 表达式2：如果表达式1的字段为null，就用位于表达式2的字段值替换。	

```sql
-- 计算 math 和 english 分数之和：

-- 先显示带计算的数据：
select name, math, english from student;
name	math	english
马云	66	78
马化腾	98	87
马景涛	56	77
柳岩	76	65
柳青	86	\N
刘德华	99	99
马德	99	99
德玛西亚	56	65

-- 计算分数之和：
SELECT NAME, math, english, math + english FROM student;
-- 发现柳青这条数据出现了问题：如果有null参与的运算，计算结果都为null（null表示没参加某场考试）。
name	math	english	math + english
马云	66	78	144
马化腾	98	87	185
马景涛	56	77	133
柳岩	76	65	141
柳青	86	\N	\N
刘德华	99	99	198
马德	99	99	198
德玛西亚	56	65	121
-- 解决如下：
SELECT NAME, math, english, math + IFNULL(english, 0) FROM student;
name	math	english	math + ifnull(english, 0)
马云	66	78	144
马化腾	98	87	185
马景涛	56	77	133
柳岩	76	65	141
柳青	86	\N	86
刘德华	99	99	198
马德	99	99	198
德玛西亚	56	65	121
```





#### 起别名

> * as：as也可以省略

```sql
-- 方式一：
SELECT NAME, math, english, math + IFNULL(english, 0) AS '总分' FROM student;
name	math	english	总分
马云	66	78	144
马化腾	98	87	185
马景涛	56	77	133
柳岩	76	65	141
柳青	86	\N	86
刘德华	99	99	198
马德	99	99	198
德玛西亚	56	65	121

-- 方式二：省略as
SELECT NAME, math, english, math + IFNULL(english, 0) '总分' FROM student;
name	math	english	总分
马云	66	78	144
马化腾	98	87	185
马景涛	56	77	133
柳岩	76	65	141
柳青	86	\N	86
刘德华	99	99	198
马德	99	99	198
德玛西亚	56	65	121

-- 都起个名字：
SELECT NAME, math '数学', english '英语', math + IFNULL(english, 0) '总分' FROM student;
name	数学	英语	总分
马云	66	78	144
马化腾	98	87	185
马景涛	56	77	133
柳岩	76	65	141
柳青	86	\N	86
刘德华	99	99	198
马德	99	99	198
德玛西亚	56	65	121

```



### 条件查询

> - where子句后跟条件
> -  运算符
>   - < 、<= 、>= 、= 、<>
>   - BETWEEN...AND  
>   - IN( 集合) 
>   - LIKE：模糊查询
>     - 占位符：
>     - _:单个任意字符
>     - %：多个任意字符
>   - IS NULL  
>   - IS NOT NULL
>   - and  或 &&
>   - or  或 || 
>   - not  或 !

#### < 、<= 、>= 、>,  = 、<>  或 !=

```sql
-- 查询年龄大于20岁
SELECT * FROM student WHERE age > 20;
id	name	age	gender	address	math	english
1	马云	55	男	杭州	66	78
2	马化腾	45	女	深圳	98	87
3	马景涛	55	男	香港	56	77
6	刘德华	57	男	香港	99	99
7	马德	22	女	香港	99	99

-- 查询年龄大于等于20岁
SELECT * FROM student WHERE age >= 20;
id	name	age	gender	address	math	english
1	马云	55	男	杭州	66	78
2	马化腾	45	女	深圳	98	87
3	马景涛	55	男	香港	56	77
4	柳岩	20	女	湖南	76	65
5	柳青	20	男	湖南	86	\N
6	刘德华	57	男	香港	99	99
7	马德	22	女	香港	99	99

-- 查询年龄等于20岁
SELECT * FROM student WHERE age = 20; -- sql里面要注意等于就是：‘=’。
id	name	age	gender	address	math	english
4	柳岩	20	女	湖南	76	65
5	柳青	20	男	湖南	86	\N

-- 查询年龄不等于20岁
SELECT * FROM student WHERE age <> 20;
id	name	age	gender	address	math	english
1	马云	55	男	杭州	66	78
2	马化腾	45	女	深圳	98	87
3	马景涛	55	男	香港	56	77
6	刘德华	57	男	香港	99	99
7	马德	22	女	香港	99	99
8	德玛西亚	18	男	南京	56	65
SELECT * FROM student WHERE age != 20;
id	name	age	gender	address	math	english
1	马云	55	男	杭州	66	78
2	马化腾	45	女	深圳	98	87
3	马景涛	55	男	香港	56	77
6	刘德华	57	男	香港	99	99
7	马德	22	女	香港	99	99
8	德玛西亚	18	男	南京	56	65

-- 所有的记录：供上面对比。
SELECT * FROM student;
id	name	age	gender	address	math	english
1	马云	55	男	杭州	66	78
2	马化腾	45	女	深圳	98	87
3	马景涛	55	男	香港	56	77
4	柳岩	20	女	湖南	76	65
5	柳青	20	男	湖南	86	\N
6	刘德华	57	男	香港	99	99
7	马德	22	女	香港	99	99
8	德玛西亚	18	男	南京	56	65
```



#### BETWEEN...AND 

```sql
-- 查询年龄大于等于20 小于等于30

-- 方式一：（推荐写法！）
SELECT * FROM student WHERE age BETWEEN 20 AND 30;
id	name	age	gender	address	math	english
4	柳岩	20	女	湖南	76	65
5	柳青	20	男	湖南	86	\N
7	马德	22	女	香港	99	99

-- 方式二：
SELECT * FROM student WHERE age >= 20 AND age <= 30;
id	name	age	gender	address	math	english
4	柳岩	20	女	湖南	76	65
5	柳青	20	男	湖南	86	\N
7	马德	22	女	香港	99	99
```



#### IN( 集合)

```sql
-- 查询年龄22岁，18岁，25岁的信息

-- 方式一：（推荐写法！）
select * from student where age in(18, 22, 25);
id	name	age	gender	address	math	english
7	马德	22	女	香港	99	99
8	德玛西亚	18	男	南京	56	65

-- 方式二：
SELECT * FROM student WHERE age=22 OR age=18 OR age=25;
id	name	age	gender	address	math	english
7	马德	22	女	香港	99	99
8	德玛西亚	18	男	南京	56	65
```



#### IS NULL

```sql
-- 查询英语成绩为null，也就是查询缺考的。

-- 方式一：（null 值的判断不能用 = 或 != 来判断）
SELECT * FROM student WHERE english = NULL; 
	-- 查询出来没有结果！
	-- 说明 null 值的判断不能用 = 或 != 来判断。
id	name	age	gender	address	math	english

-- 方式二：
SELECT * FROM student WHERE english IS NULL;
	-- 可以通过 is null 来判断
id	name	age	gender	address	math	english
5	柳青	20	男	湖南	86	\N
```

#### IS NOT NULL

```sql
-- 查询英语成绩不为null，也就是参加考试了的。
SELECT * FROM student WHERE english IS NOT NULL;

id	name	age	gender	address	math	english
1	马云	55	男	杭州	66	78
2	马化腾	45	女	深圳	98	87
3	马景涛	55	男	香港	56	77
4	柳岩	20	女	湖南	76	65
6	刘德华	57	男	香港	99	99
7	马德	22	女	香港	99	99
8	德玛西亚	18	男	南京	56	65
```



#### LIKE：模糊查询

> - 占位符：
>   - _ :   单个任意字符
>   - %：多个任意字符

```sql
-- 查询姓马的有哪些：
	-- 姓 '马'，'马' 肯定在第一个位置。
SELECT * FROM student WHERE NAME LIKE '马%';
id	name	age	gender	address	math	english
1	马云	55	男	杭州	66	78
2	马化腾	45	女	深圳	98	87
3	马景涛	55	男	香港	56	77
7	马德	22	女	香港	99	99

-- 查询姓名中第二个字时化的人：
SELECT * FROM student WHERE NAME LIKE '_化%';
id	name	age	gender	address	math	english
2	马化腾	45	女	深圳	98	87

-- 查询姓名是三个字的人：
select * from student where name like '___';
id	name	age	gender	address	math	english
2	马化腾	45	女	深圳	98	87
3	马景涛	55	男	香港	56	77
6	刘德华	57	男	香港	99	99

-- 查询姓名中包含'马'字的人：（最常用）
SELECT * FROM student WHERE NAME LIKE '%马%';
id	name	age	gender	address	math	english
1	马云	55	男	杭州	66	78
2	马化腾	45	女	深圳	98	87
3	马景涛	55	男	香港	56	77
7	马德	22	女	香港	99	99
```




​		



​	