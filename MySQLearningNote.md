---
title: MySQL学习笔记
categories:
  - MySQL
tags:
  - SQL
  - 约束
  - 数据库的设计
  - 数据库的备份和还原
  - 多表查询
  - 事务
date: 2021-03-23
author: Jungle

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

### 排序查询

```sql
* 语法：order by 子句
		* order by 排序字段1 排序方式1 ，  排序字段2 排序方式2...

	* 排序方式：
		* ASC：升序，默认的。
		* DESC：降序。

	* 注意：
		* 如果有多个排序条件，则当前边的条件值一样时，才会判断第二条件。
```

### 聚合函数

```sql
聚合函数：将一列数据作为一个整体，进行纵向的计算。
	1. count：计算个数
		1. 一般选择非空的列：主键 （推荐）
		2. count(*)
	2. max：计算最大值
	3. min：计算最小值
	4. sum：计算和
	5. avg：计算平均值
	
	* 注意：聚合函数的计算，排除null值。
		解决方案：
			1. 选择不包含非空的列进行计算
			2. IFNULL函数
```



### 分组查询

```sql
1. 语法：group by 分组字段；
2. 注意：
    1. 分组之后查询的字段：分组字段、聚合函数
    2. where 和 having 的区别？
        1. where 在分组之前进行限定，如果不满足条件，则不参与分组；
           having在分组之后进行限定，如果不满足结果，则不会被查询出来。
        2. where 后不可以跟聚合函数，having可以进行聚合函数的判断。

    -- 按照性别分组。分别查询男、女同学的平均分
    SELECT sex , AVG(math) FROM student GROUP BY sex;

    -- 按照性别分组。分别查询男、女同学的平均分,人数
    SELECT sex , AVG(math),COUNT(id) FROM student GROUP BY sex;

    --  按照性别分组。分别查询男、女同学的平均分,人数 要求：分数低于70分的人，不参与分组
    SELECT sex , AVG(math),COUNT(id) FROM student WHERE math > 70 GROUP BY sex;

    --  按照性别分组。分别查询男、女同学的平均分,人数 要求：分数低于70分的人，不参与分组,分组之后。人数要大于2个人
    SELECT sex , AVG(math),COUNT(id) FROM student WHERE math > 70 GROUP BY sex HAVING COUNT(id) > 2;

    SELECT sex , AVG(math),COUNT(id) nums FROM student WHERE math > 70 GROUP BY sex HAVING nums > 2;
```

### 分页查询

```sql
1. 语法：limit 开始的索引,每页查询的条数;
2. 公式：开始的索引 = （当前的页码 - 1） * 每页显示的条数

    -- 每页显示3条记录 
    SELECT * FROM student LIMIT 0,3; -- 第1页

    SELECT * FROM student LIMIT 3,3; -- 第2页

    SELECT * FROM student LIMIT 6,3; -- 第3页

3. limit 是一个MySQL"方言"，MySQL特有！
```

## DCL

```sql
* SQL分类：
	1. DDL：操作数据库和表
	2. DML：增删改表中数据
	3. DQL：查询表中数据
	4. DCL：管理用户，授权

* DBA：数据库管理员

* DCL：管理用户，授权
	1. 管理用户
		1. 添加用户：
			* 语法：CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
		2. 删除用户：
			* 语法：DROP USER '用户名'@'主机名';
		3. 修改用户密码：
			
			UPDATE USER SET PASSWORD = PASSWORD('新密码') WHERE USER = '用户名';
			UPDATE USER SET PASSWORD = PASSWORD('abc') WHERE USER = 'lisi';
			
			SET PASSWORD FOR '用户名'@'主机名' = PASSWORD('新密码');
			SET PASSWORD FOR 'root'@'localhost' = PASSWORD('123');

			* mysql中忘记了root用户的密码？
				1. cmd -- > net stop mysql 停止mysql服务
					* 需要管理员运行该cmd

				2. 使用无验证方式启动mysql服务： mysqld --skip-grant-tables
				3. 打开新的cmd窗口,直接输入mysql命令，敲回车。就可以登录成功
				4. use mysql;
				5. update user set password = password('你的新密码') where user = 'root';
				6. 关闭两个窗口
				7. 打开任务管理器，手动结束mysqld.exe 的进程
				8. 启动mysql服务
				9. 使用新密码登录。
		4. 查询用户：
			-- 1. 切换到mysql数据库
			USE myql;
			-- 2. 查询user表
			SELECT * FROM USER;
			
			* 通配符： % 表示可以在任意主机使用用户登录数据库

	2. 权限管理：
		1. 查询权限：
			-- 查询权限
			SHOW GRANTS FOR '用户名'@'主机名';
			SHOW GRANTS FOR 'lisi'@'%';

		2. 授予权限：
			-- 授予权限
			grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';
			-- 给张三用户授予所有权限，在任意数据库任意表上
			
			GRANT ALL ON *.* TO 'zhangsan'@'localhost';
		3. 撤销权限：
			-- 撤销权限：
			revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
			REVOKE UPDATE ON db3.`account` FROM 'lisi'@'%';
```



# 约束

> 概念： 对表中的数据进行限定，保证数据的正确性、有效性和完整性。

分类：
 	1. 主键约束：primary key
	2. 非空约束：not null
	3. 唯一约束：unique
	4. 外键约束：foreign key

## 非空约束

> not null，某一列的值不能为null

```sql
1. 创建表时添加约束
    CREATE TABLE stu(
        id INT,
        NAME VARCHAR(20) NOT NULL -- name为非空
    );
2. 创建表完后，添加非空约束
    ALTER TABLE stu MODIFY NAME VARCHAR(20) NOT NULL;

3. 删除name的非空约束
    ALTER TABLE stu MODIFY NAME VARCHAR(20);
```

## 唯一约束

> unique，某一列的值不能重复

```sql
1. 注意：
		* 唯一约束可以有NULL值，但是只能有一条记录为null
2. 在创建表时，添加唯一约束
    CREATE TABLE stu(
        id INT,
        phone_number VARCHAR(20) UNIQUE -- 手机号
    );
3. 删除唯一约束
    ALTER TABLE stu DROP INDEX phone_number;
4. 在表创建完后，添加唯一约束
    ALTER TABLE stu MODIFY phone_number VARCHAR(20) UNIQUE;
```

## 主键约束

> primary key ：非空且唯一

```sql
1. 注意：
    1. 含义：非空且唯一
    2. 一张表只能有一个字段为主键
    3. 主键就是表中记录的唯一标识

2. 在创建表时，添加主键约束
    create table stu(
        id int primary key,-- 给id添加主键约束
        name varchar(20)
    );

3. 删除主键
    -- 错误 alter table stu modify id int ; --不报错也不起作用！
    ALTER TABLE stu DROP PRIMARY KEY;

4. 创建完表后，添加主键
    ALTER TABLE stu MODIFY id INT PRIMARY KEY;

5. 自动增长：
    1.  概念：如果某一列是数值类型的，使用 auto_increment 可以来完成值得自动增长

    2. 在创建表时，添加主键约束，并且完成主键自增长
    create table stu(
        id int primary key auto_increment,-- 给id添加主键约束
        name varchar(20)
    );
    3. 删除自动增长
		ALTER TABLE stu MODIFY id INT;
		4. 添加自动增长
		ALTER TABLE stu MODIFY id INT AUTO_INCREMENT;
```

## 外键约束

> foreign key,让表于表产生关系，从而保证数据的正确性

```sql
1. 在创建表时，可以添加外键
    * 语法：
        create table 表名(
            ....
            外键列
            constraint 外键名称 foreign key (外键列名称) references 主表名称(主表列名称)
        );
        
    * 例子：
    	create table employee(
            id int primary key auto_increment,
            name varchar(20),
            age int,
            dep_id int, -- 外键列：外键对应主表的主键
            -- 创建外键约束
            constraint emp_depid_fk foreign key (dep_id) references
            department(id) on update cascade on delete cascade --同时实现级联操作
        );

2. 删除外键
    ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;

3. 创建表之后，添加外键
    ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称);
    
4. 级联操作
    1. 添加级联操作
        语法：ALTER TABLE 表名 ADD CONSTRAINT 外键名称 
             FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称) ON UPDATE CASCADE ON DELETE CASCADE;
    2. 分类：
        1. 级联更新：ON UPDATE CASCADE 
        2. 级联删除：ON DELETE CASCADE 
```

### 数据冗余

```sql
CREATE TABLE emp (
	id INT PRIMARY KEY AUTO_INCREMENT,
	NAME VARCHAR(30),
	age INT,
	dep_name VARCHAR(30),
	dep_location VARCHAR(30)
);

-- 添加数据
INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('张三', 20, '研发部', '广州');
INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('李四', 21, '研发部', '广州');
INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('王五', 20, '研发部', '广州');
INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('老王', 20, '销售部', '深圳');
INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('大王', 22, '销售部', '深圳');
INSERT INTO emp (NAME, age, dep_name, dep_location) VALUES ('小王', 18, '销售部', '深圳');

select * from emp;
id	NAME	age	dep_name	dep_location
1	张三	20	研发部	广州
2	李四	21	研发部	广州
3	王五	20	研发部	广州
4	老王	20	销售部	深圳
5	大王	22	销售部	深圳
6	小王	18	销售部	深圳


-- 出现了数据冗余，拆分表！
-- 解决方案：分成 2 张表

-- 创建部门表(id,dep_name,dep_location)
-- 一方，主表
create table department(
	id int primary key auto_increment,
	dep_name varchar(20),
	dep_location varchar(20)
);

-- 创建员工表(id,name,age,dep_id)
-- 多方，从表
create table employee(
	id int primary key auto_increment,
	name varchar(20),
	age int,
	dep_id int -- 外键对应主表的主键
)

-- 添加 2 个部门
insert into department values(null, '研发部','广州'),(null, '销售部', '深圳');
select * from department;
id	dep_name	dep_location
1	研发部	广州
2	销售部	深圳

-- 添加员工,dep_id 表示员工所在的部门
INSERT INTO employee (NAME, age, dep_id) VALUES ('张三', 20, 1);
INSERT INTO employee (NAME, age, dep_id) VALUES ('李四', 21, 1);
INSERT INTO employee (NAME, age, dep_id) VALUES ('王五', 20, 1);
INSERT INTO employee (NAME, age, dep_id) VALUES ('老王', 20, 2);
INSERT INTO employee (NAME, age, dep_id) VALUES ('大王', 22, 2);
INSERT INTO employee (NAME, age, dep_id) VALUES ('小王', 18, 2);
select * from employee;
id	name	age	dep_id
1	张三	20	1
2	李四	21	1
3	王五	20	1
4	老王	20	2
5	大王	22	2
6	小王	18	2
```

### 实现外键约束

```sql
-- 1) 删除副表/从表 employee
DROP TABLE employee;
-- 2) 创建从表 employee 并添加外键约束 emp_depid_fk
-- 多方，从表
CREATE TABLE employee(
	id INT PRIMARY KEY AUTO_INCREMENT,
	NAME VARCHAR(20),
	age INT,
	dep_id INT, -- 外键对应主表的主键
	-- 创建外键约束
	CONSTRAINT emp_depid_fk FOREIGN KEY (dep_id) REFERENCES department(id)
)
-- 3) 正常添加数据
INSERT INTO employee (NAME, age, dep_id) VALUES ('张三', 20, 1);
INSERT INTO employee (NAME, age, dep_id) VALUES ('李四', 21, 1);
INSERT INTO employee (NAME, age, dep_id) VALUES ('王五', 20, 1);
INSERT INTO employee (NAME, age, dep_id) VALUES ('老王', 20, 2);
INSERT INTO employee (NAME, age, dep_id) VALUES ('大王', 22, 2);
INSERT INTO employee (NAME, age, dep_id) VALUES ('小王', 18, 2);
SELECT * FROM employee;

-- 4) 部门错误的数据添加失败
-- 插入不存在的部门 （外键值可以为null，但是不能为不存在的值）
-- Cannot add or update a child row: a foreign key constraint fails
INSERT INTO employee (NAME, age, dep_id) VALUES ('老张', 18, 6);

-- 删除 employee 表的 emp_depid_fk 外键
alter table employee drop foreign key emp_depid_fk;

-- 在 employee 表情存在的情况下添加外键
alter table employee add constraint emp_depid_fk foreign key (dep_id) references department(id);
```

### 级联

> 在修改和删除主表的主键时，同时更新或删除副表的外键值，称为级联操作
>
> ALTER TABLE 表名 ADD CONSTRAINT 外键名称 
>
> ​	FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称) ON UPDATE CASCADE ON DELETE CASCADE;

|   级联操作语法    | 描述                                                         |
| :---------------: | ------------------------------------------------------------ |
| ON UPDATE CASCADE | 级联更新，只能是创建表的时候创建级联关系。更新主表中的主键，从表中的外键列也自动同步更新 |
| ON DELETE CASCADE | 级联删除                                                     |

```sql
-- 新的问题
    -- 要把部门表中的 id 值 2，改成 5，能不能直接更新呢？
    -- Cannot delete or update a parent row: a foreign key constraint fails
    update department set id=5 where id=2;
    
    -- 要删除部门 id 等于 1 的部门, 能不能直接删除呢？
    -- Cannot delete or update a parent row: a foreign key constraint fails
    delete from department where id=1;
    
-- 实现级联操作：

	-- 先删除 employee 表的 emp_depid_fk 外键
    ALTER TABLE employee DROP FOREIGN KEY emp_depid_fk;
    -- 设置级联更新
    ALTER TABLE employee ADD CONSTRAINT emp_depid_fk FOREIGN KEY (dep_id) REFERENCES department(id) ON UPDATE CASCADE;
    UPDATE department SET id=5 WHERE id=2;
    SELECT * FROM department;
    SELECT * FROM employee;
    -- 可以更新了！
    id	dep_name	dep_location
    1	研发部	广州
    5	销售部	深圳
    id	name	age	dep_id
    1	张三	20	1
    2	李四	21	1
    3	王五	20	1
    4	老王	20	5
    5	大王	22	5
    6	小王	18	5
    
    -- 级联删除
    -- 先删除 employee 表的 emp_depid_fk 外键
    ALTER TABLE employee DROP FOREIGN KEY emp_depid_fk;
    -- 设置级联删除
    ALTER TABLE employee ADD CONSTRAINT emp_depid_fk FOREIGN KEY (dep_id) REFERENCES department(id) ON DELETE CASCADE;
    DELETE FROM department WHERE id=1;
    SELECT * FROM department;
    SELECT * FROM employee;
    -- 可以删除了！
    id	dep_name	dep_location
	5	销售部	深圳
	id	name	age	dep_id
    4	老王	20	5
    5	大王	22	5
    6	小王	18	5
```

### 小结

> 级联操作要谨慎设置！

| 约束名 | 关键字      | 说明                                       |
| ------ | ----------- | ------------------------------------------ |
| 主键   | primary key | 唯一且非空                                 |
| 默认   | default     | 如果一列没有值，使用默认值                 |
| 非空   | not null    | 这一列必须有值                             |
| 唯一   | unique      | 这一列不能有重复值                         |
| 外键   | foreign key | 主表中主键列（id），在从表中外键列(dep_id) |

# 数据库的设计

## 多表之间的关系

### 分类

```sql
1. 一对一(了解)：
    * 如：人和身份证
    * 分析：一个人只有一个身份证，一个身份证只能对应一个人
2. 一对多(多对一)：
    * 如：部门和员工
    * 分析：一个部门有多个员工，一个员工只能对应一个部门
3. 多对多：
    * 如：学生和课程
    * 分析：一个学生可以选择很多门课程，一个课程也可以被很多学生选择    
```

### 实现关系

> 一对多结构图
>
> 如：部门和员工
> 分析：一个部门有多个员工，一个员工只能对应一个部门

![image-20210328153207034](C:\Users\LeBro\AppData\Roaming\Typora\typora-user-images\image-20210328153207034.png)

> 多对多结构图
>
> 如：学生和课程
> 实现方式：多对多关系实现需要借助第三张中间表。中间表至少包含两个字段，这两个字段作为第三张表的外键，分别指向两张表的主键

![image-20210328154906892](C:\Users\LeBro\AppData\Roaming\Typora\typora-user-images\image-20210328154906892.png)

> 一对一结构图
>
> 如：人和身份证
>
> 实现方式：一对一关系实现，可以在任意一方添加唯一外键指向另一方的主键。

![image-20210328155650922](C:\Users\LeBro\AppData\Roaming\Typora\typora-user-images\image-20210328155650922.png)

**注：一般会直接合并成一张表！**

### 案例

> 结构图分析

![image-20210328161247977](C:\Users\LeBro\AppData\Roaming\Typora\typora-user-images\image-20210328161247977.png)

```sql
-- 创建旅游线路分类表 tab_category
    -- cid 旅游线路分类主键，自动增长
    -- cname 旅游线路分类名称非空，唯一，字符串 100
    CREATE TABLE tab_category (
        cid INT PRIMARY KEY AUTO_INCREMENT,
        cname VARCHAR(100) NOT NULL UNIQUE
    );

    -- 创建旅游线路表 tab_route
    /*
    rid 旅游线路主键，自动增长
    rname 旅游线路名称非空，唯一，字符串 100
    price 价格
    rdate 上架时间，日期类型
    cid 外键，所属分类
    */
    CREATE TABLE tab_route(
        rid INT PRIMARY KEY AUTO_INCREMENT,
        rname VARCHAR(100) NOT NULL UNIQUE,
        price DOUBLE,
        rdate DATE,
        cid INT,
        FOREIGN KEY (cid) REFERENCES tab_category(cid) --省略外键写法
    );

    /*创建用户表 tab_user
    uid 用户主键，自增长
    username 用户名长度 100，唯一，非空
    password 密码长度 30，非空
    name 真实姓名长度 100
    birthday 生日
    sex 性别，定长字符串 1
    telephone 手机号，字符串 11
    email 邮箱，字符串长度 100
    */
    CREATE TABLE tab_user (
        uid INT PRIMARY KEY AUTO_INCREMENT,
        username VARCHAR(100) UNIQUE NOT NULL,
        PASSWORD VARCHAR(30) NOT NULL,
        NAME VARCHAR(100),
        birthday DATE,
        sex CHAR(1) DEFAULT '男',
        telephone VARCHAR(11),
        email VARCHAR(100)
    );

    /*
    创建收藏表 tab_favorite
    rid 旅游线路 id，外键
    date 收藏时间
    uid 用户 id，外键
    rid 和 uid 不能重复，设置复合主键，同一个用户不能收藏同一个线路两次
    */
    CREATE TABLE tab_favorite (
        rid INT, -- 线路id
        DATE DATETIME,
        uid INT, -- 用户id
        -- 创建复合主键
        PRIMARY KEY(rid,uid), -- 联合主键
        FOREIGN KEY (rid) REFERENCES tab_route(rid),
        FOREIGN KEY(uid) REFERENCES tab_user(uid)
    );
```

> 结构图查看

![image-20210328162130951](C:\Users\LeBro\AppData\Roaming\Typora\typora-user-images\image-20210328162130951.png)

## 数据库设计的范式

#### 概念

> 设计数据库时，需要遵循的一些规范。
>
> 要遵循后边的范式要求，必须先遵循前边的所有范式要求

```sql
设计关系数据库时，遵从不同的规范要求，设计出合理的关系型数据库，这些不同的规范要求被称为不同的范式，各种范式呈递次规范，越高的范式数据库冗余越小。
- 目前关系数据库有六种范式：
	- 第一范式（1NF）、第二范式（2NF）、第三范式（3NF）、【一般满足前三种即可】
	- 巴斯-科德范式（BCNF）、第四范式(4NF）和第五范式（5NF，又称完美范式）。
```

#### 分类

1. 第一范式（1NF）：每一列都是不可分割的原子数据项

2. 第二范式（2NF）：在1NF的基础上，非码属性必须完全依赖于码（在1NF基础上消除非主属性对主码的部分函数依赖）

   ```
   * 几个概念：
       1. 函数依赖：A-->B,如果通过A属性(属性组)的值，可以确定唯一B属性的值。则称B依赖于A
           例如：学号-->姓名。  （学号，课程名称） --> 分数
       2. 完全函数依赖：A-->B， 如果A是一个属性组，则B属性值得确定需要依赖于A属性组中所有的属性值。
           例如：（学号，课程名称） --> 分数
       3. 部分函数依赖：A-->B， 如果A是一个属性组，则B属性值得确定只需要依赖于A属性组中某一些值即可。
           例如：（学号，课程名称） -- > 姓名
       4. 传递函数依赖：A-->B, B -- >C . 如果通过A属性(属性组)的值，可以确定唯一B属性的值，在通过B属性（属性组）的值可以确定唯一C属性的值，则称 C 传递函数依赖于A
           例如：学号-->系名，系名-->系主任
       5. 码：如果在一张表中，一个属性或属性组，被其他所有属性所完全依赖，则称这个属性(属性组)为该表的码
           例如：该表中码为：（学号，课程名称）
           * 主属性：码属性组中的所有属性
           * 非主属性：除去码属性组的属性
   ```

3. 第三范式（3NF）：在2NF基础上，任何非主属性不依赖于其它非主属性（在2NF基础上消除传递依赖）



> 初始表：

![image-20210328163354932](C:\Users\LeBro\AppData\Roaming\Typora\typora-user-images\image-20210328163354932.png)

> 修改后满足第一范式，但仍存在的问题：
>
> 1. 存在非常严重的数据冗余（重复）：姓名-系名-系主任
> 2. 数据添加存在问题：添加新开设的系和系主任时，数据不合法
> 3. 数据删除也存在问题：张无忌同学毕业了，删除数据，会将系的数据一起删除

![image-20210328163505294](C:\Users\LeBro\AppData\Roaming\Typora\typora-user-images\image-20210328163505294.png)



> 修改后满足第二范式：（在1NF基础上消除非主属性对主码的部分函数依赖）
>
> ​	非码属性必须完全依赖于码，（学号，课程名称） --> 分数；学号 --> 姓名/系名/系主任
>
> 但仍未解决第2, 3问题

![image-20210328171157606](C:\Users\LeBro\AppData\Roaming\Typora\typora-user-images\image-20210328171157606.png)

> 修改后满足第三范式（在2NF基础上消除传递依赖）
>
> 全部解决！

![image-20210328172818747](C:\Users\LeBro\AppData\Roaming\Typora\typora-user-images\image-20210328172818747.png)

# 数据库的备份和还原

> 命令行

```sql
* 语法：
    * 备份： mysqldump -u用户名 -p密码 数据库名称 > 保存的路径
    * 还原：
        1. 登录数据库
        2. 创建数据库
        3. 使用数据库
        4. 执行文件：source 文件路径
 
 * 实战：
 		-- 备份数据库
		C:\Users\LeBro>mysqldump -uroot -proot db_test > D://DatabaseBackUP//bptest.sql
		
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
        -- 删除数据库
        mysql> drop database db_test;
        Query OK, 9 rows affected (0.05 sec)

        mysql> show databases;
        +--------------------+
        | Database           |
        +--------------------+
        | information_schema |
        | mysql              |
        | performance_schema |
        | test               |
        +--------------------+
        4 rows in set (0.00 sec)

		-- 还原数据库
		mysql> create database db_test;
        Query OK, 1 row affected (0.00 sec)

        mysql> use db_test;
        Database changed
        
        mysql> source D://DatabaseBackUP//bptest.sql;
        mysql> show tables;
        +-------------------+
        | Tables_in_db_test |
        +-------------------+
        | department        |
        | emp               |
        | employee          |
        | stu               |
        | student           |
        | tab_category      |
        | tab_favorite      |
        | tab_route         |
        | tab_user          |
        +-------------------+
        9 rows in set (0.00 sec)
```

> 图形化工具 ：提示操作即可。



# 多表查询

> 数据准备

```sql
-- 创建部门表
CREATE TABLE dept(
    id INT PRIMARY KEY AUTO_INCREMENT,
    NAME VARCHAR(20)
);
INSERT INTO dept (NAME) VALUES ('开发部'),('市场部'),('财务部');
-- 创建员工表
CREATE TABLE emp (
    id INT PRIMARY KEY AUTO_INCREMENT,
    NAME VARCHAR(10),
    gender CHAR(1), -- 性别
    salary DOUBLE, -- 工资
    join_date DATE, -- 入职日期
    dept_id INT,
    FOREIGN KEY (dept_id) REFERENCES dept(id) -- 外键，关联部门表(部门表的主键)
);
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('孙悟空','男',7200,'2013-02-24',1);
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('猪八戒','男',3600,'2010-12-02',2);
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('唐僧','男',9000,'2008-08-08',2);
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('白骨精','女',5000,'2015-10-07',3);
INSERT INTO emp(NAME,gender,salary,join_date,dept_id) VALUES('蜘蛛精','女',4500,'2011-03-14',1);
```

## 笛卡尔积

> 有两个集合A,B，取这两个集合的所有组成情况。
> 要完成多表查询，需要消除无用的数据

```sql
SELECT * FROM emp;

SELECT * FROM dept;

SELECT * FROM emp, dept;

-- emp 5条数据
id	NAME	gender	salary	join_date	dept_id
1	孙悟空	男	7200	2013-02-24	1
2	猪八戒	男	3600	2010-12-02	2
3	唐僧	男	9000	2008-08-08	2
4	白骨精	女	5000	2015-10-07	3
5	蜘蛛精	女	4500	2011-03-14	1

-- dept 3条数据
id	NAME
1	开发部
2	市场部
3	财务部

-- 笛卡尔积 3*5=15条数据
id	NAME	gender	salary	join_date	dept_id	id	NAME
1	孙悟空	男	7200	2013-02-24	1	1	开发部
1	孙悟空	男	7200	2013-02-24	1	2	市场部
1	孙悟空	男	7200	2013-02-24	1	3	财务部
2	猪八戒	男	3600	2010-12-02	2	1	开发部
2	猪八戒	男	3600	2010-12-02	2	2	市场部
2	猪八戒	男	3600	2010-12-02	2	3	财务部
3	唐僧	男	9000	2008-08-08	2	1	开发部
3	唐僧	男	9000	2008-08-08	2	2	市场部
3	唐僧	男	9000	2008-08-08	2	3	财务部
4	白骨精	女	5000	2015-10-07	3	1	开发部
4	白骨精	女	5000	2015-10-07	3	2	市场部
4	白骨精	女	5000	2015-10-07	3	3	财务部
5	蜘蛛精	女	4500	2011-03-14	1	1	开发部
5	蜘蛛精	女	4500	2011-03-14	1	2	市场部
5	蜘蛛精	女	4500	2011-03-14	1	3	财务部
```



## 内连接查询

> 1. 隐式内连接：使用where条件消除无用数据

```sql
-- 查询所有员工信息和对应的部门信息
SELECT * FROM emp,dept WHERE emp.`dept_id` = dept.`id`;
id	NAME	gender	salary	join_date	dept_id	id	NAME
1	孙悟空	男	7200	2013-02-24	1	1	开发部
2	猪八戒	男	3600	2010-12-02	2	2	市场部
3	唐僧	男	9000	2008-08-08	2	2	市场部
4	白骨精	女	5000	2015-10-07	3	3	财务部
5	蜘蛛精	女	4500	2011-03-14	1	1	开发部

-- 查询员工表的名称，性别。部门表的名称
SELECT emp.name,emp.gender,dept.name FROM emp,dept WHERE emp.`dept_id` = dept.`id`;
-- 规范写法：
SELECT 
	t1.`name`, -- 员工表的姓名
    t1.`gender`, -- 员工表的性别
    t2.`name` -- 部门表的名称
FROM
	emp t1, -- 员工表
	dept t2 -- 部门表
WHERE 
	t1.`dept_id`=t2.`id`;
	
name	gender	name
孙悟空	男	开发部
猪八戒	男	市场部
唐僧	男	市场部
白骨精	女	财务部
蜘蛛精	女	开发部
```

> 2. 显式内连接：select 字段列表 from 表名1 [inner] join 表名2 on 条件
>
>    - 与隐式内连接结果一致
>
>    - inner 可省略
>    - 查询交集部分

```sql
SELECT 
	t1.`NAME`,
	t1.`gender`,
	t2.`NAME`
FROM 
	emp t1
INNER JOIN 
	dept t2
ON 
	t1.`dept_id`=t2.`id`	
	
NAME	gender	NAME
孙悟空	男	开发部
猪八戒	男	市场部
唐僧	男	市场部
白骨精	女	财务部
蜘蛛精	女	开发部
```

> 小结：
>
> - 从哪些表中查询数据
> - 条件是什么
> - 查询哪些字段

## 外链接查询

> 1. 左外连接：select 字段列表 from 表1 left [outer] join 表2 on 条件；
>
> 查询的是左表所有数据以及其交集部分。

```sql
-- 查询所有员工信息，如果员工有部门，则查询部门名称，没有部门，则不显示部门名称
	-- emp
    id	NAME	gender	salary	join_date	dept_id
    1	孙悟空	男	7200	2013-02-24	1
    2	猪八戒	男	3600	2010-12-02	2
    3	唐僧	男	9000	2008-08-08	2
    4	白骨精	女	5000	2015-10-07	3
    5	蜘蛛精	女	4500	2011-03-14	1
    6	小白龙	女	4500	2021-03-29	\N
    -- dept
    id	NAME
    1	开发部
    2	市场部
    3	财务部
    
    -- 隐式查询（求交集）
    SELECT t1.*, t2.`NAME`
    FROM 
        emp t1, dept t2
    WHERE 
        t1.`dept_id`=t2.`id`
        
    -- 同 inner join（求交集）
    select t1.*, t2.`NAME`
    From
        emp t1
    inner join
        dept t2
    on t1.`dept_id`=t2.`id`   
    
    -- 小白龙 dept_id 为 null，不显示！
    id	NAME	gender	salary	join_date	dept_id	NAME
    1	孙悟空	男	7200	2013-02-24	1	开发部
    2	猪八戒	男	3600	2010-12-02	2	市场部
    3	唐僧	男	9000	2008-08-08	2	市场部
    4	白骨精	女	5000	2015-10-07	3	财务部
    5	蜘蛛精	女	4500	2011-03-14	1	开发部
    
-- 查询所有员工信息，如果员工有部门，则查询部门名称，没有部门，则不显示部门名称
select t1.*, t2.`NAME`
From
	emp t1
left join
	dept t2
on t1.`dept_id`=t2.`id`
-- 左外连接实现目的：（以左表为基准求交集）
id	NAME	gender	salary	join_date	dept_id	NAME
1	孙悟空	男	7200	2013-02-24	1	开发部
2	猪八戒	男	3600	2010-12-02	2	市场部
3	唐僧	男	9000	2008-08-08	2	市场部
4	白骨精	女	5000	2015-10-07	3	财务部
5	蜘蛛精	女	4500	2011-03-14	1	开发部
6	小白龙	女	4500	2021-03-29	\N	\N
```

> 2. 右外连接：select 字段列表 from 表1 right [outer] join 表2 on 条件；
>
> 查询的是右表所有数据以及其交集部分。

```sql
-- 查询所有员工信息，如果员工有部门，则查询部门名称，没有部门，则不显示部门名称
select t1.*, t2.`NAME`
From
	emp t1
right join
	dept t2
on t1.`dept_id`=t2.`id`
-- 右连接实现目的：（以右表为基准求交集）
id	NAME	gender	salary	join_date	dept_id	NAME
1	孙悟空	男	7200	2013-02-24	1	开发部
5	蜘蛛精	女	4500	2011-03-14	1	开发部
2	猪八戒	男	3600	2010-12-02	2	市场部
3	唐僧	男	9000	2008-08-08	2	市场部
4	白骨精	女	5000	2015-10-07	3	财务部
```

## 子查询

> 查询中嵌套查询，称嵌套查询为子查询

```sql
-- 查询工资最高的员工信息
    -- 1 查询最高的工资是多少 9000
    SELECT MAX(salary) FROM emp;

    -- 2 查询员工信息，并且工资等于9000的
    SELECT * FROM emp WHERE emp.`salary` = 9000;

    -- 一条sql就完成这个操作，子查询
    SELECT * FROM emp WHERE emp.`salary` = (SELECT MAX(salary) FROM emp);
```

> 子查询不同情况

### 子查询的结果是单行单列的

> * 子查询可以作为条件，使用运算符去判断。
> *  运算符： > >= < <= =

```sql
-- 查询员工工资小于平均工资的人
SELECT 
	*
FROM 
	emp
WHERE
	emp.`salary` < (SELECT AVG(emp.`salary`) FROM emp)
	
id	NAME	gender	salary	join_date	dept_id
2	猪八戒	男	3600	2010-12-02	2
4	白骨精	女	5000	2015-10-07	3
5	蜘蛛精	女	4500	2011-03-14	1
6	小白龙	女	4500	2021-03-29	\N
```



### 子查询的结果是多行单列的

> - 子查询可以作为条件，使用运算符in来判断

```sql
-- 查询'财务部'和'市场部'所有的员工信息
SELECT id FROM dept WHERE NAME = '财务部' OR NAME = '市场部';
SELECT * FROM emp WHERE dept_id = 3 OR dept_id = 2;
-- 子查询
SELECT 
	*
FROM 
	emp
WHERE
	emp.`dept_id`
IN
	(SELECT dept.`id` FROM dept WHERE dept.`NAME` IN ('财务部', '市场部'))
-- 查询结果
id	NAME	gender	salary	join_date	dept_id
2	猪八戒	男	3600	2010-12-02	2
3	唐僧	男	9000	2008-08-08	2
4	白骨精	女	5000	2015-10-07	3
```



### 子查询的结果是多行多列的

> - 子查询可以作为一张虚拟表参与查询

```sql
-- 查询员工入职日期是2011-11-11日之后的员工信息和部门信息
-- 子查询
SELECT *
FROM
	dept t1,
	(SELECT * FROM emp WHERE emp.`join_date` > '2011-11-11') t2
WHERE 
	t1.id = t2.dept_id
-- 查询结果
id	NAME	id	NAME	gender	salary	join_date	dept_id
1	开发部	1	孙悟空	男	7200	2013-02-24	1
3	财务部	4	白骨精	女	5000	2015-10-07	3

-- 普通内连接
    -- 隐式内连接查询
    select 
        *
    from
        dept t1,
        emp t2
    where 
        t1.`id` = t2.dept_id
    and	t2.`join_date` > '2011-11-11';
    -- 显示内连接查询 
    SELECT 
	*
    FROM 
        dept t1
    INNER JOIN
        emp t2
    ON 
        t1.`id` = t2.dept_id
    WHERE
        t2.join_date > '2011-11-11';
-- 查询结果
id	NAME	id	NAME	gender	salary	join_date	dept_id
1	开发部	1	孙悟空	男	7200	2013-02-24	1
3	财务部	4	白骨精	女	5000	2015-10-07	3
```

## 多表查询练习：

### 数据准备

```sql
-- 部门表
CREATE TABLE dept (
  id INT PRIMARY KEY, -- 部门id
  dname VARCHAR(50), -- 部门名称
  loc VARCHAR(50) -- 部门所在地
);

-- 添加4个部门
INSERT INTO dept(id,dname,loc) VALUES 
(10,'教研部','北京'),
(20,'学工部','上海'),
(30,'销售部','广州'),
(40,'财务部','深圳');

-- 职务表，职务名称，职务描述
CREATE TABLE job (
  id INT PRIMARY KEY,
  jname VARCHAR(20),
  description VARCHAR(50)
);

-- 添加4个职务
INSERT INTO job (id, jname, description) VALUES
(1, '董事长', '管理整个公司，接单'),
(2, '经理', '管理部门员工'),
(3, '销售员', '向客人推销产品'),
(4, '文员', '使用办公软件');

-- 员工表
CREATE TABLE emp (
  id INT PRIMARY KEY, -- 员工id
  ename VARCHAR(50), -- 员工姓名
  job_id INT, -- 职务id
  mgr INT , -- 上级领导
  joindate DATE, -- 入职日期
  salary DECIMAL(7,2), -- 工资
  bonus DECIMAL(7,2), -- 奖金
  dept_id INT, -- 所在部门编号
  CONSTRAINT emp_jobid_ref_job_id_fk FOREIGN KEY (job_id) REFERENCES job (id),
  CONSTRAINT emp_deptid_ref_dept_id_fk FOREIGN KEY (dept_id) REFERENCES dept (id)
);

-- 添加员工
INSERT INTO emp(id,ename,job_id,mgr,joindate,salary,bonus,dept_id) VALUES 
(1001,'孙悟空',4,1004,'2000-12-17','8000.00',NULL,20),
(1002,'卢俊义',3,1006,'2001-02-20','16000.00','3000.00',30),
(1003,'林冲',3,1006,'2001-02-22','12500.00','5000.00',30),
(1004,'唐僧',2,1009,'2001-04-02','29750.00',NULL,20),
(1005,'李逵',4,1006,'2001-09-28','12500.00','14000.00',30),
(1006,'宋江',2,1009,'2001-05-01','28500.00',NULL,30),
(1007,'刘备',2,1009,'2001-09-01','24500.00',NULL,10),
(1008,'猪八戒',4,1004,'2007-04-19','30000.00',NULL,20),
(1009,'罗贯中',1,NULL,'2001-11-17','50000.00',NULL,10),
(1010,'吴用',3,1006,'2001-09-08','15000.00','0.00',30),
(1011,'沙僧',4,1004,'2007-05-23','11000.00',NULL,20),
(1012,'李逵',4,1006,'2001-12-03','9500.00',NULL,30),
(1013,'小白龙',4,1004,'2001-12-03','30000.00',NULL,20),
(1014,'关羽',4,1007,'2002-01-23','13000.00',NULL,10);

-- 工资等级表
CREATE TABLE salarygrade (
  grade INT PRIMARY KEY,   -- 级别
  losalary INT,  -- 最低工资
  hisalary INT -- 最高工资
);

-- 添加5个工资等级
INSERT INTO salarygrade(grade,losalary,hisalary) VALUES 
(1,7000,12000),
(2,12010,14000),
(3,14010,20000),
(4,20010,30000),
(5,30010,99990);
```

### 查询所有员工信息。

> 查询员工编号，员工姓名，工资，职务名称，职务描述

```sql
/*
    分析：
        1.员工编号，员工姓名，工资，需要查询emp表  职务名称，职务描述 需要查询job表
        2.查询条件 emp.job_id = job.id
*/

-- 方法一：
SELECT 
    t1.`id`, -- 员工编号
    t1.`ename`, -- 员工姓名
    t1.`salary`,-- 工资
    t2.`jname`, -- 职务名称
    t2.`description` -- 职务描述
FROM 
    emp t1, job t2
WHERE 
    t1.`job_id` = t2.`id`;
    
-- 方法二：
SELECT 
	t1.`id`, 	-- 员工编号
	t1.`ename`,  	-- 员工姓名
	t1.`salary`, 	-- 工资
	t2.`jname`, 	-- 职务名称
	t2.`description` -- 职务描述
FROM 
	emp t1 		-- 员工表
INNER JOIN 
	job t2		-- 职务表
ON 
	t1.`job_id` = t2.`id`;
	
-- 查询结果
id	ename	salary	jname	description
1009	罗贯中	50000.00	董事长	管理整个公司，接单
1004	唐僧	29750.00	经理	管理部门员工
1006	宋江	28500.00	经理	管理部门员工
1007	刘备	24500.00	经理	管理部门员工
1002	卢俊义	16000.00	销售员	向客人推销产品
1003	林冲	12500.00	销售员	向客人推销产品
1010	吴用	15000.00	销售员	向客人推销产品
1001	孙悟空	8000.00	文员	使用办公软件
1005	李逵	12500.00	文员	使用办公软件
1008	猪八戒	30000.00	文员	使用办公软件
1011	沙僧	11000.00	文员	使用办公软件
1012	李逵	9500.00	文员	使用办公软件
1013	小白龙	30000.00	文员	使用办公软件
1014	关羽	13000.00	文员	使用办公软件
```

### 查询员工编号，员工姓名，工资，职务名称，职务描述，部门名称，部门位置

```sql
/*
    分析：
        1. 员工编号，员工姓名，工资 emp  职务名称，职务描述 job  部门名称，部门位置 dept
        2. 条件： emp.job_id = job.id and emp.dept_id = dept.id
*/

SELECT 
    t1.`id`, -- 员工编号
    t1.`ename`, -- 员工姓名
    t1.`salary`,-- 工资
    t2.`jname`, -- 职务名称
    t2.`description`, -- 职务描述
    t3.`dname`, -- 部门名称
    t3.`loc` -- 部门位置
FROM 
    emp t1, job t2, dept t3
WHERE 
    t1.`job_id` = t2.`id` AND t1.`dept_id`=t3.`id`;
    
-- 查询结果
id	ename	salary	jname	description	dname	loc
1008	猪八戒	30000.00	文员	使用办公软件	学工部	上海
1004	唐僧	29750.00	经理	管理部门员工	学工部	上海
1001	孙悟空	8000.00	文员	使用办公软件	学工部	上海
1013	小白龙	30000.00	文员	使用办公软件	学工部	上海
1011	沙僧	11000.00	文员	使用办公软件	学工部	上海
1014	关羽	13000.00	文员	使用办公软件	教研部	北京
1007	刘备	24500.00	经理	管理部门员工	教研部	北京
1009	罗贯中	50000.00	董事长	管理整个公司，接单	教研部	北京
1003	林冲	12500.00	销售员	向客人推销产品	销售部	广州
1010	吴用	15000.00	销售员	向客人推销产品	销售部	广州
1005	李逵	12500.00	文员	使用办公软件	销售部	广州
1012	李逵	9500.00	文员	使用办公软件	销售部	广州
1006	宋江	28500.00	经理	管理部门员工	销售部	广州
1002	卢俊义	16000.00	销售员	向客人推销产品	销售部	广州
```

### 查询员工姓名，工资，工资等级 

```sql
/*
    分析：
        1.员工姓名，工资 emp  工资等级 salarygrade
        2.条件：emp.salary BETWEEN salarygrade.losalary and salarygrade.hisalary
*/

SELECT 
	t1.`ename`,
	t1.`salary`,
	t2.`grade`
FROM
	emp t1, salarygrade t2
WHERE 
	t1.`salary` BETWEEN t2.`losalary` AND t2.`hisalary`;
-- 查询的结果
ename	salary	grade
孙悟空	8000.00	1
卢俊义	16000.00	3
林冲	12500.00	2
唐僧	29750.00	4
李逵	12500.00	2
宋江	28500.00	4
刘备	24500.00	4
猪八戒	30000.00	4
罗贯中	50000.00	5
吴用	15000.00	3
沙僧	11000.00	1
李逵	9500.00	1
小白龙	30000.00	4
关羽	13000.00	2
```

### 查询员工姓名，工资，职务名称，职务描述，部门名称，部门位置，工资等级

```sql
/*
    分析：
        1. 员工姓名，工资 emp ， 职务名称，职务描述 job 部门名称，部门位置，dept  工资等级 salarygrade
        2. 条件： 	  emp.job_id = job.id 
        		and 
        			emp.dept_id = dept.id 
        		and 
        			emp.salary BETWEEN salarygrade.losalary and salarygrade.hisalary
*/

SELECT 
	t1.`ename`, -- 员工姓名
	t1.`salary`, -- 工资
	t2.`jname`, -- 职务名称
	t2.`description`, -- 职务描述 
	t3.`dname`, -- 部门名称
	t3.`loc`, -- 部门位置
	t4.`grade` -- 工资等级
FROM
	emp t1, job t2, dept t3, salarygrade t4
WHERE 
	t1.`dept_id` = t3.`id`
	AND	t1.`job_id` = t2.`id`
	AND	t1.`salary` BETWEEN t4.`losalary` AND t4.`hisalary`;
	
-- 查询结果
ename	salary	jname	description	dname	loc	grade
孙悟空	8000.00	文员	使用办公软件	学工部	上海	1
沙僧	11000.00	文员	使用办公软件	学工部	上海	1
李逵	9500.00	文员	使用办公软件	销售部	广州	1
林冲	12500.00	销售员	向客人推销产品	销售部	广州	2
李逵	12500.00	文员	使用办公软件	销售部	广州	2
关羽	13000.00	文员	使用办公软件	教研部	北京	2
卢俊义	16000.00	销售员	向客人推销产品	销售部	广州	3
吴用	15000.00	销售员	向客人推销产品	销售部	广州	3
唐僧	29750.00	经理	管理部门员工	学工部	上海	4
宋江	28500.00	经理	管理部门员工	销售部	广州	4
刘备	24500.00	经理	管理部门员工	教研部	北京	4
猪八戒	30000.00	文员	使用办公软件	学工部	上海	4
小白龙	30000.00	文员	使用办公软件	学工部	上海	4
罗贯中	50000.00	董事长	管理整个公司，接单	教研部	北京	5
```

### 查询出部门编号、部门名称、部门位置、部门人数

```sql
/*
    分析：
        1.部门编号、部门名称、部门位置 dept 表。 部门人数 emp表
        2.使用分组查询：按照emp.dept_id完成分组，查询count(id)
        3.使用子查询将第2步的查询结果和dept表进行关联查询

*/

SELECT
	t1.id,
	t1.`dname`,
	t1.`loc`,
	t2.total
FROM 
	dept t1,	
	(SELECT 
		dept_id,
		COUNT(id) total
	FROM
		emp
	GROUP BY
		emp.`dept_id`) t2
WHERE 
	t1.`id` = t2.dept_id;
	
-- 查询结果
id	dname	loc	total
10	教研部	北京	3
20	学工部	上海	5
30	销售部	广州	6
```

### 查询所有员工的姓名及其直接上级的姓名

> 没有领导的员工也需要查询

```sql
/*
    分析：
        1.姓名 emp， 直接上级的姓名 emp
            * emp表的id 和 mgr 是自关联
        2.条件 emp.id = emp.mgr
        3.查询左表的所有数据，和 交集数据
            * 使用左外连接查询

*/
SELECT 
	t1.`ename` '员工',
	t2.`ename` '直接上级'
FROM
	emp t1
LEFT JOIN
        emp t2
ON 
	t1.`mgr` = t2.`id`;

-- 查询结果
员工	直接上级
孙悟空	唐僧
卢俊义	宋江
林冲	宋江
唐僧	罗贯中
李逵	宋江
宋江	罗贯中
刘备	罗贯中
猪八戒	唐僧
罗贯中	
吴用	宋江
沙僧	唐僧
李逵	宋江
小白龙	唐僧
关羽	刘备
```

# 事务

## 事务的基本介绍

> 概念：如果一个包含多个步骤的业务操作，被事务管理，那么这些操作要么同时成功，要么同时失败。

1. 操作：

   1. 开启事务： start transaction;
   2. 回滚：rollback;
   3. 提交：commit;

2.  例子：

   ```sql
   CREATE TABLE account (
       id INT PRIMARY KEY AUTO_INCREMENT,
       NAME VARCHAR(10),
       balance DOUBLE
   );
   -- 添加数据
   INSERT INTO account (NAME, balance) VALUES ('zhangsan', 1000), ('lisi', 1000);
   
   SELECT * FROM account;
   --还原数据
   UPDATE account SET balance = 1000; 
   
   -- 张三给李四转账 500 元:
       -- 0. 开启事务
       START TRANSACTION;
       -- 1. 张三账户 -500		
       UPDATE account SET balance = balance - 500 WHERE NAME = 'zhangsan';
       -- 2. 李四账户 +500
       -- 出错了...
       UPDATE account SET balance = balance + 500 WHERE NAME = 'lisi';
   
       -- 发现执行没有问题，提交事务
       	-- 在提交之前是临时数据，查询数据只能看到临时更改的数据状态！
       	-- 关闭mysql执行窗体之后，发现数据并未保存，原因是：自动回滚！
       COMMIT; -- 提交后数据保存！
   
       -- 发现出问题了，回滚事务
       ROLLBACK;
   ```
   
3. MySQL数据库中事务默认自动提交

   - 事务提交的两种方式：
     - 自动提交：
       - 一条DML(增删改)语句会自动提交一次事务。
       - MySQL 数据库默认是自动提交事务
     - 手动提交：
        - 需要先开启事务，再提交
        - Oracle 数据库默认是手动提交事务
   - 修改事务的默认提交方式：
     - 查看事务的默认提交方式：SELECT @@autocommit; 
       - 1 代表自动提交  
       - 0 代表手动提交
     - 修改默认提交方式： set @@autocommit = 0;

## 事务的四大特征（重点掌握！）

> 重点掌握！

1. 原子性：是不可分割的最小操作单位，要么同时成功，要么同时失败。
2. 持久性：当事务提交或回滚后，数据库会持久化的保存数据。
3. 隔离性：多个事务之间，相互独立。
4. 一致性：事务操作前后，数据总量不变。



## 事务的隔离级别（了解！）

> 概念：多个事务之间是隔离的，相互独立的。
>
> 但是如果多个事务操作同一批数据，则会引发一些问题，设置不同的隔离级别就可以解决这些问题。

- 存在问题：
  1. 脏读：一个事务，读取到另一个事务中没有提交的数据
  2. 不可重复读(虚读)：在同一个事务中，两次读取到的数据不一样。
  3. 幻读：一个事务操作(DML)数据表中所有记录，另一个事务添加了一条数据，则第一个事务查询不到

- 隔离级别：

  1. read uncommitted：读未提交
      - 产生的问题：脏读、不可重复读、幻读

  2. read committed：读已提交 （Oracle）

    * 产生的问题：不可重复读、幻读
  3. repeatable read：可重复读 （MySQL默认）

    * 产生的问题：幻读

  4. serializable：串行化

    * 可以解决所有的问题

    ```sql
    * 注意：隔离级别从小到大安全性越来越高，但是效率越来越低
    * 数据库查询隔离级别：
    	* select @@tx_isolation;
    * 数据库设置隔离级别：
    	* set global transaction isolation level  级别字符串;
    ```

- 演示：

  ```sql
  set global transaction isolation level read uncommitted;
  -- set global transaction isolation level read committed;
  -- set global transaction isolation level repeatable read;
  start transaction;
  -- 转账操作
  update account set balance = balance - 500 where id = 1;
update account set balance = balance + 500 where id = 2;
  ```
  
  

