---
title: Java-Note19
categories:
  - Java
tags:
  - 异常
author:
  - Jungle
date: 2020-12-08 19:11:19

---
# 异常 #
**程序在执行过程中，出现的非正常的情况，最终导致JVM停止的非正常停止。**

## 异常体系 ##
- Throwable<java.lang.Throwable>
	- **Error<错误>**: 不能处理(只能修改源代码)，只能尽量避免。
	- **Exception<异常>**: 编译期异常，进行编译（写代码）java程序出现的问题
		- RuntimeExceptions: 运行期异常，java程序运行过程中出现的问题
		- 运行时异常被抛出可以不处理，即不捕获也不声明抛出，默认给JVM处理，终止程序。
	- 异常可以经过处理，程序可以继续执行。


## 怎么处理异常 ##
	
- **throw**: 可以使用throw关键字在指定的方法中抛出指定的异常
	- 使用格式：throw new xxxException("异常产生的原因");
	- 必须写在方法的内部
	- new的对象必须Exception或者Exception的子类对象
	- 必须处理这个异常对象：
		- 创建的是RuntimeException或者是RuntimeException的子类对象，可以不处理，默认交给JVM处理（打印异常对象，中断程序）
		- 创建的是编译异常（写代码时报错），要么throw，要么try...catch

- 异常处理的第一种方式：**在方法声明的时候使用throws关键字抛出异常**，最终交给JVM处理-->中断处理。
	- 注意：
		- 如果抛出了多个异常且异常对象有父子类关系，那么直接声明父类异常即可。
		- 如果调用了一个声明抛出异常的方法，就必须处理声明的异常：
			- 要么继续使用throws声明抛出，交给调用者处理，最终交给JVM
			- 要么try...catch自己处理


					package demo;/*
					  @author: LeBro
					  @DESCRIPTION: 异常处理
					  @creteTime: 2020/12/10  
					*/
					
					import java.io.FileNotFoundException;
					import java.io.IOException;
					
					public class demoThrows {
					    // throws继续抛出给JVM
					    public static void main(String[] args) throws IOException {
					        readFile("c:\\a.exe");
					    }
					
					    public static void readFile(String fileName) throws IOException {
					        // IOException 是 FileNotFoundException 的父类
					        if (!fileName.endsWith(".txt")){
					            throw new IOException("文件的后缀名不对");
					        }
					
					        if (!fileName.equals("c:\\a.txt")){
					            //编译异常：继续使用throws抛出异常-->给调用者
					            throw new FileNotFoundException("传递的文件路径错误");
					        }
					
					        System.out.println("文件没有问题，读取文件");
					    }
					}



- 异常处理的第二种方式：**使用try...catch自己处理异常**
	- 注意：
		- try中可能抛出多个异常对象，那么就可以使用多个catch来处理这些异常
			- 如果try中产生了异常，那么就会执行catch中的异常处理逻辑，完成后继续执行try...catch之后的代码
			- 如果没有产生异常，那么执行完try中的代码，就绪执行try...catch之后的代码


					package demo;/*
					  @author: LeBro
					  @DESCRIPTION: What to do...
					  @creteTime: 2020/12/10  
					*/
					
					import java.io.FileNotFoundException;
					import java.io.IOException;
					
					public class demoTrycatch {
					
					    public static void main(String[] args) {
					        try {
					            //可能产生异常的代码
					            readFile("c:\\a.exe");
					        } catch (IOException e) { //try中抛出什么异常就用什么来接受这个异常对象
					            e.printStackTrace();
					        }
					        System.out.println("后续代码");
					    }
					
					    public static void readFile(String fileName) throws IOException {
					        // IOException 是 FileNotFoundException 的父类
					        if (!fileName.endsWith(".txt")){
					            throw new IOException("文件的后缀名不对");
					        }
					
					        if (!fileName.equals("c:\\a.txt")){
					            //编译异常：继续使用throws抛出异常-->给调用者
					            throw new FileNotFoundException("传递的文件路径错误");
					        }
					
					        System.out.println("文件没有问题，读取文件");
					    }
					}

----------
# Throwable类中3个处理异常的方法  #
1. String getMessage()：返回此throwable的简短描述。
2. String toString()：返回throwable的详细消息字符串。
3. void printStackTrace()：JVM打印异常对象，默认此方法，打印的异常信息是最全面的。


----------
# finally代码块 #
**无论是否出现异常都会执行，通常与try...catch()...finally一起使用**
**一般用于资源释放，无论程序是否出现异常，最后都要资源释放(IO)**

- 如果finally中有return语句，永远返回finally中的结果，避免该情况。
- 尽量不要在finally中写return语句

----------
# 子父类异常 #
- 如果父类抛出了多个异常，子类重写父类方法时，抛出：
	- **和父类相同的异常**
	- 或者：**父类异常的子类**
	- 或者：**不抛出异常**
- 父类方法没有抛出异常，子类重写父类该方法时也不可抛出异常。此时子类产生该异常，只能捕获处理，不能声明抛出。
- **注意：父类异常时什么样，子类异常就什么样。**