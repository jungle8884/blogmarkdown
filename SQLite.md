---
title: SQLite
categories:
  - 计算机技术
tags:
  - 数据库
date: 2020-10-06 11:06:41
author: jungle

---
# 开发工具 #
DB Browser for SQLite

# 新建数据库 #
图形化界面操作

# 创建表 #
图形化界面操作后会生成sql语句.

## 三级联动 ##

- 表一: 目标类型

		CREATE TABLE "TargetType" (
			"id"	INTEGER NOT NULL,
			"type"	TEXT NOT NULL,
			PRIMARY KEY("id" AUTOINCREMENT)
		);

- 表二: 设施目标

		CREATE TABLE "FacilitiesTarget" (
			"id"	INTEGER NOT NULL,
			"type_id"	INTEGER NOT NULL DEFAULT 1,
			"type_name"	TEXT DEFAULT '设施目标',
			"field_name"	TEXT,
			PRIMARY KEY("id" AUTOINCREMENT)
		);

