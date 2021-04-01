---
title: Ado.net
categories:
  - 计算机技术
tags:
  - C#
  - Ado.net
date: 2020-10-16 20:20:42
author: Jungle

---
# DataSet和DateTable #
- DataSet : 内存中的数据缓存, 相当于把数据库放在了内存中.
- DataTable: 内存中数据的一个表.
	- DataColumn: 列, 可以设置属性名称, 类型.
	- DataRow: 行, 可以设置一条记录对应的属性值.
	- 方法使用:
	- DataTable dt = new DataTable("表名称");
		- dt.Rows.Add(DataRow对象) //RowState Added 新增一行
		- dt.AcceptChanges() //提交更改
		- dt.RejectChanges() //撤销更改
		- DataRow对象.Delte()	//状态值发生改变, 假删除
		- dt.Rows.Remove(DataRow对象) //真删除, 无法撤销
		- dt.Rows.RemoveAt(行索引) //同上
		- dt.Select(): 获取表的所有行
		- dt.Select("筛选条件")
		- dt.Select("筛选条件", "排序条件")

----------
# 反射 # 

## 创建对象 ##

		Type type = typeof(类名称);
		Activator.CreateInstance<类名称>(); //创建对象
		或者:
		Activator.CreateInstance(type); //同上, 都是调用的无参构造函数
		Activator.CreateInstance(type, 参数数组--->params object[] args); //调用有参构造函数

## 获取对象的所有公有属性 ##

		PropertyInfo[] properties = type.GetProperties();
	
## 获取对象的指定属性 ##

		PropertyInfo prop = type.GetProperty("属性名称");
		prop.GetValue(对象名); //获取属性值
		prop.SetValue(对象名, 属性值); //设置属性值
		

## 获取对象的所有公有字段 ##

		FieldInfo[] fields = type.GetFields();

## 获取所有私有字段 ##

		FieldInfo[] fields = type.GetFields(BindingFlags.Instance | BindingFlags.NonPublic);

## 获取指定名称的公共方法并调用 ##

		MethodInfo method = type.GetMethod("方法名称");
		method.Invoke(对象名, null); //没有参数的方法的调用
		
## 获取当前Type的所有公共构造函数 ##

		 ConstructorInfo[] cons = type.GetConstructors();

## 使用参数类型获取构造函数, 再而生成对象 ##

		Type[] typeParam = new Type[] { typeof(参数类型) }; 
		ConstructorInfo con = type.GetConstructor(typeParam);
		object[] oParam = new object[] { "参数值" };
		类名称 对象名 = (类名称)con.Invoke(oParam);

## 加载程序集  ##
		
		Assembly assembly = Assembly.Load("程序集的名称");
		Assembly assembly = Assembly.LoadFile("程序集的名称"); //要带上后缀 .dll

		Type type = assembly.GetType("程序集名称.类名称");
		object obj = Activator.CreateInstance(type);
		MethodInfo method = type.GetMethod("方法名称");
		method.Invoke(obj, new object[] { 参数值 });

----------
