---
title: 代理模式
categories:
  - 设计模式
tags:
  - 结构型模式
date: 2020-05-13 21:35:20
author: Jungle

---
# Why? #

	在有些情况下，一个客户不能或者不想直接访问另一个对象，这时需要找一个中介帮忙完成某项任务，这个中介就是代理对象。

# Defination #

	由于某些原因需要给某对象提供一个代理以控制对该对象的访问。这时，访问对象不适合或者不能直接引用目标对象，代理对象作为访问对象和目标对象之间的中介。

# How? #
	
	1. 远程代理: 这种方式通常是为了隐藏目标对象存在于不同地址空间的事实，方便客户端访问。
		- 例如，WebService在.NET中的应用.
	2. 虚拟代理: 这种方式通常用于要创建的目标对象开销很大时。
		- 例如，下载一幅很大的图像需要很长时间，因某种计算比较复杂而短时间无法完成，
		- 这时可以先用小比例的虚拟代理替换真实的对象，消除用户对服务器慢的感觉。
	3. 安全代理: 这种方式通常用于控制不同种类客户对真实对象的访问权限。
	4. 智能指针: 主要用于调用目标对象时，代理附加一些额外的处理功能。
		- 例如，增加计算真实对象的引用次数的功能，这样当该对象没有被引用时，就可以自动释放它。
	5. 延迟加载，指为了提高系统的性能，延迟对目标的加载。
		- 例如，Hibernate 中就存在属性的延迟加载和关联表的延时加载。

# Code #

----------
		package com.ProxyPattern;
		
		// 定义共用接口
		public interface Subject {
		    public abstract void Request();
		}

----------
		package com.ProxyPattern;
		
		public class RealSubject implements Subject {
		    @Override
		    public void Request() {
		        System.out.println("真实的请求");
		    }
		}

----------
		package com.ProxyPattern;
		
		public class Proxy implements Subject {
		    RealSubject realSubject;
		    @Override
		    public void Request() {
		        if(realSubject == null){
		            realSubject = new RealSubject();
		        }
		        realSubject.Request();
		    }
		}

----------
		package com.ProxyPattern;
		
		public class Main {
		
		    public static void main(String[] args) {
			// write your code here
		        Proxy proxy = new Proxy();
		        proxy.Request();
		    }
		}

----------
		真实的请求
		
		Process finished with exit code 0

----------
**---参考代码来自于大话设计模式**




