---
title: 享元模式
categories:
  - 设计模式
tags:
  - 结构型模式
date: 2020-05-12 17:06:14
author: jungle

---
# Why? #
	面向对象技术可以很好地解决一些灵活性或可扩展性问题，但在很多情况下需要在系统中增加类和对象的个数。
	当对象数量太多时，将导致运行代价过高，带来性能下降等问题。
	享元模式通过共享技术实现相同或相似对象的重用提高系统资源的利用率。

# 定义 #
	运用共享技术有效地支持大量细粒度的对象的复用。
	系统只使用少量的对象，而这些对象都很相似，状态变化很小，可以实现对象的多次复用。
	由于享元模式要求能够共享的对象必须是细粒度对象，因此它又称为轻量级模式。

# 什么时候使用? #
	
	1. 系统中有大量对象。 
	2. 这些对象消耗大量内存。 
	3. 这些对象的状态大部分可以外部化。
	4. 这些对象可以按照内蕴状态分为很多组，当把外蕴对象从对象中剔除出来时，每一组对象都可以用一个对象来代替。
	5. 系统不依赖于这些对象身份，这些对象是不可分辨的。 
	6. 注意事项： 
		- 1、注意划分外部状态和内部状态，否则可能会引起线程安全问题。
		- 2、这些类必须有一个工厂对象加以控制。 
		
# 具体示例代码 #

----------
		package com.FlyweightPattern;
	
		public interface Flyweight {
		    public abstract void Operation(int extrinsicstate);
		}

----------
		package com.FlyweightPattern;
		
		public class ConcreteFlyweight implements Flyweight {
		    @Override
		    public void Operation(int extrinsicstate) {
		        System.out.println("具体Flyweight:" + extrinsicstate);
		    }
		}


----------
		package com.FlyweightPattern;
		
		import java.util.Hashtable;
		
		public class FlyweightFactory {
		    private Hashtable flyweights = new Hashtable();
		
		    public FlyweightFactory(){
		        flyweights.put("X", new ConcreteFlyweight());
		        flyweights.put("Y", new ConcreteFlyweight());
		        flyweights.put("Z", new ConcreteFlyweight());
		    }
		
		    public Flyweight GetFlyweight(String key){
		        return ((Flyweight)flyweights.get(key));
		    }
		
		}

----------
		package com.FlyweightPattern;
		
		public class UnsharedConcreteFlyweight implements Flyweight {
		    @Override
		    public void Operation(int extrinsicstate) {
		        System.out.println("不共享的具体Flyweight:" + extrinsicstate);
		    }
		}


----------
		package com.FlyweightPattern;
		
		public class Main {
		
		    public static void main(String[] args) {
			// write your code here
		        int extrinsicstate = 22;
		
		        FlyweightFactory f = new FlyweightFactory();
		
		        Flyweight fx = f.GetFlyweight("X");
		        fx.Operation(--extrinsicstate);
		
		        Flyweight fy = f.GetFlyweight("Y");
		        fx.Operation(--extrinsicstate);
		
		        Flyweight fz = f.GetFlyweight("Z");
		        fx.Operation(--extrinsicstate);
		
		        UnsharedConcreteFlyweight uf = new UnsharedConcreteFlyweight();
		        uf.Operation(--extrinsicstate);
		    }
		}

----------
## 输出 ##
		具体Flyweight:21
		具体Flyweight:20
		具体Flyweight:19
		不共享的具体Flyweight:18
		
		Process finished with exit code 0

----------
# 容易理解的例子 #
- 在享元对象内部并且不会随着环境改变而改变的共享部分, 可以称为是享元对象的内部状态; [WebSite]
- 而随着环境改变而改变的、不可以共享的状态就是外部状态了. [User]


----------
		package com.FlyweightPattern.example;
		
		public class User {
		    private String name;
		    public User(String name){
		        this.name = name;
		    }
		
		    public String getName(){
		        return name;
		    }
		}

----------
		package com.FlyweightPattern.example;
		
		public interface WebSite {
		    public abstract void Use(User user);
		}

----------
		package com.FlyweightPattern.example;
		
		public class ConcreteWebSite implements WebSite {
		    private String name = "";
		    public ConcreteWebSite(String name){
		        this.name = name;
		    }
		    @Override
		    public void Use(User user) {
		        System.out.println("网站分类: " + name + " 用户: " + user.getName());
		    }
		}

----------
		package com.FlyweightPattern.example;
		
		import java.util.Hashtable;
		
		public class WebSiteFactory {
		    private Hashtable flyweights = new Hashtable();
		
		    // 获得网站分类
		    public WebSite GetWebSiteCategory(String key){
		        if (!flyweights.containsKey(key))
		            flyweights.put(key, new ConcreteWebSite(key));
		        return ((WebSite)flyweights.get(key));
		    }
		
		    // 获得网站分类数
		    public int GetWebSiteCount() {
		        return flyweights.size();
		    }
		}

----------
## 输出 ##
		网站分类: 产品展示 用户: 小蔡
		网站分类: 产品展示 用户: 大鸟
		网站分类: 产品展示 用户: 娇娇
		网站分类: 产品展示 用户: 老顽童
		网站分类: 产品展示 用户: 桃谷六仙
		网站分类: 产品展示 用户: 南海鄂神
		得到网站分类总数为2
		
		Process finished with exit code 0

----------




