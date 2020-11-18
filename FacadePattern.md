---
title: 外观模式
categories:
  - 设计模式
tags:
  - 结构型模式
date: 2020-05-11 14:58:28
author: jungle

---
# 为什么要用外观模式? #
	当一个系统的功能越来越强，子系统会越来越多，客户对系统的访问也变得越来越复杂。
	外观模式是为了解决使用者类与被使用者类之间的依赖关系的，将各子系统关系放在一个Facade类中，降低了类类之间的耦合度。

# 什么是外观模式? #
	一种通过为多个复杂的子系统提供一个统一的接口，而使这些子系统更加容易被访问的模式。
	外部应用程序不用关心内部子系统的具体的细节，这样会大大降低应用程序的复杂度，提高了程序的可维护性。

# 何时使用外观模式? #
	
	- 对分层结构系统构建时，使用外观模式定义子系统中每层的入口点可以简化子系统之间的依赖关系。
	- 当一个复杂系统的子系统很多时，外观模式可以为系统设计一个简单的接口供外界访问。
	- 当客户端与多个子系统之间存在很大的联系时，引入外观模式可将它们分离，从而提高子系统的独立性和可移植性。

# 优缺点 #

## 优点 ##

	- 降低了子系统与客户端之间的耦合度，使得子系统的变化不会影响调用它的客户类。
	- 对客户屏蔽了子系统组件，减少了客户处理的对象数目，并使得子系统使用起来更加容易。
	- 降低了大型软件系统中的编译依赖性，简化了系统在不同平台之间的移植过程，因为编译一个子系统不会影响其他的子系统，也不会影响外观对象。

## 缺点 ##

	-  不能很好地限制客户使用子系统类。
	-  增加新的子系统可能需要修改外观类或客户端的源代码，违背了“开闭原则”。


# 示例代码 #

----------

		package com.FacadePattern;
		
		public class SubSystemOne {
		    public void MethodOne(){
		        System.out.println("子系统方法一");
		    }
		}

----------
		package com.FacadePattern;
		
		public class SubSystemTwo {
		    public void MethodTwo(){
		        System.out.println("子系统方法二");
		    }
		}

----------
		package com.FacadePattern;
		
		public class SubSystemThree {
		    public void MethodThree(){
		        System.out.println("子系统方法三");
		    }
		}

----------
		package com.FacadePattern;
		
		public class SubSystemFour {
		    public void MethodFour(){
		        System.out.println("子系统方法四");
		    }
		}

----------
		package com.FacadePattern;
		
		public class Facade {
		    SubSystemOne one;
		    SubSystemTwo two;
		    SubSystemThree three;
		    SubSystemFour four;
		
		    public Facade(){
		        one = new SubSystemOne();
		        two = new SubSystemTwo();
		        three = new SubSystemThree();
		        four = new SubSystemFour();
		    }
		
		    public void MethodA(){
		        System.out.println("\n 方法组 A() ---");
		        one.MethodOne();
		        two.MethodTwo();
		        four.MethodFour();
		    }
		
		    public void MethodB(){
		        System.out.println("\n 方法组 B() ---");
		        two.MethodTwo();
		        three.MethodThree();
		    }
		}

----------
		package com.FacadePattern;
		
		public class Main {
		
		    public static void main(String[] args) {
			// write your code here
		        Facade facade = new Facade();
		
		        facade.MethodA();
		        facade.MethodB();
		
		    }
		}

----------
输出:

 方法组 A() ---
子系统方法一
子系统方法二
子系统方法四

 方法组 B() ---
子系统方法二
子系统方法三

Process finished with exit code 0

----------

