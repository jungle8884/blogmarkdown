---
title: 访问者模式
categories:
  - 设计模式
tags:
  - 行为型模式
date: 2020-06-07 15:44:02
author: Jungle

---
# 引言 #
- 在现实生活中，有些集合对象中存在多种不同的元素，且每种元素也存在多种不同的访问者和处理方式。
- 这些被处理的数据元素相对稳定而访问方式多种多样的数据结构，如果用“访问者模式”来处理比较方便。
- 访问者模式能把处理方法从数据结构中分离出来，并可以根据需要增加新的处理方法，且不用修改原来的程序代码与数据结构，这提高了程序的扩展性和灵活性。

# 定义 #
- 表示一个作用于某对象结构中的各元素的操作. 
- 它使你可以在不改变各元素的类的前提下定义作用于这些元素的新操作.

# 优点 #
- 扩展性好。能够在不修改对象结构中的元素的情况下，为对象结构中的元素添加新的功能。
- 复用性好。可以通过访问者来定义整个对象结构通用的功能，从而提高系统的复用程度。
- 灵活性好。访问者模式将数据结构与作用于结构上的操作解耦，使得操作集合可相对自由地演化而不影响系统的数据结构。
- 符合单一职责原则。访问者模式把相关的行为封装在一起，构成一个访问者，使每一个访问者的功能都比较单一。

# 缺点 #
- 增加新的元素类很困难。在访问者模式中，每增加一个新的元素类，都要在每一个具体访问者类中增加相应的具体操作，这违背了“开闭原则”。
- 破坏封装。访问者模式中具体元素对访问者公布细节，这破坏了对象的封装性。
- 违反了依赖倒置原则。访问者模式依赖了具体类，而没有依赖抽象类。

# 应用场景 #
- 通常在以下情况可以考虑使用访问者（Visitor）模式。 
	- 对象结构相对稳定，但其操作算法经常变化的程序。
	- 对象结构中的对象需要提供多种不同且不相关的操作，而且要避免让这些操作的变化影响对象的结构。
	- 对象结构包含很多类型的对象，希望对这些对象实施一些依赖于其具体类型的操作。
	- 访问者（Visitor）模式实现的关键是如何将作用于元素的操作分离出来封装成独立的类。

# 结构 #
- 抽象访问者（Visitor）角色：定义一个访问具体元素的接口，为每个具体元素类对应一个访问操作 visit() ，该操作中的参数类型标识了被访问的具体元素。
- 具体访问者（ConcreteVisitor）角色：实现抽象访问者角色中声明的各个访问操作，确定访问者访问一个元素时该做什么。
- 抽象元素（Element）角色：声明一个包含接受操作 accept() 的接口，被接受的访问者对象作为 accept() 方法的参数。
- 具体元素（ConcreteElement）角色：实现抽象元素角色提供的 accept() 操作，其方法体通常都是 visitor.visit(this) ，另外具体元素中可能还包含本身业务逻辑的相关操作。
- 对象结构（Object Structure）角色：是一个包含元素角色的容器，提供让访问者对象遍历容器中的所有元素的方法，通常由 List、Set、Map 等聚合类实现。


# 代码实现 #
----------
		package com.VisitorPattern.sample;
		
		//为该对象结构中ConcreteElement的每一个类声明一个Visit操作
		public abstract class Visitor {
		    public abstract void VisitConcreteElementA(ConcreteElementA concreteElementA);
		    public abstract void VisitConcreteElementB(ConcreteElementB concreteElementB);
		}

----------
		package com.VisitorPattern.sample;
		
		//定义一个Accept操作, 它以一个访问者为参数.
		public abstract class Element {
		    public abstract void Accept(Visitor visitor);
		}

----------
		package com.VisitorPattern.sample;
		
		//具体访问者, 实现每个由Visitor声明的操作. 每个操作实现算法的一部分, 而该算法片段乃是对应于结构中对象的类.
		public class ConcreteVisitor1 extends Visitor {
		    @Override
		    public void VisitConcreteElementA(ConcreteElementA concreteElementA) {
		        System.out.println(concreteElementA.getClass().getName() + "被" + this.getClass().getName() + "访问");
		    }
		
		    @Override
		    public void VisitConcreteElementB(ConcreteElementB concreteElementB) {
		        System.out.println(concreteElementB.getClass().getName() + "被" + this.getClass().getName() + "访问");
		    }
		}

----------
		package com.VisitorPattern.sample;
		
		public class ConcreteVisitor2 extends Visitor {
		    @Override
		    public void VisitConcreteElementA(ConcreteElementA concreteElementA) {
		        System.out.println(concreteElementA.getClass().getName() + "被" + this.getClass().getName() + "访问");
		    }
		
		    @Override
		    public void VisitConcreteElementB(ConcreteElementB concreteElementB) {
		        System.out.println(concreteElementB.getClass().getName() + "被" + this.getClass().getName() + "访问");
		    }
		}

----------
		package com.VisitorPattern.sample;
		
		//具体元素, 实现Accept操作
		public class ConcreteElementA extends Element {
		    @Override
		    public void Accept(Visitor visitor) {
		        //充分利用双分派技术, 实现处理与数据结构的分离.
		        visitor.VisitConcreteElementA(this);
		    }
		
		    //其他的相关方法
		    public void Operation(){}
		}

----------
		package com.VisitorPattern.sample;
		
		public class ConcreteElementB extends Element {
		    @Override
		    public void Accept(Visitor visitor) {
		        //充分利用双分派技术, 实现处理与数据结构的分离.
		        visitor.VisitConcreteElementB(this);
		    }
		
		    //其他的相关方法
		    public void Operation(){}
		}

----------
		package com.VisitorPattern.sample;
		
		import java.util.ArrayList;
		import java.util.List;
		
		public class ObjectStructure {
		    private List<Element> elements = new ArrayList<Element>();
		
		    public void Attach(Element element){
		        elements.add(element);
		    }
		
		    public void Detach(Element element){
		        elements.remove(element);
		    }
		
		    public void Accept(Visitor visitor){
		        for (Element e:elements
		             ) {
		            e.Accept(visitor);
		        }
		    }
		}

----------
		package com.VisitorPattern;
		
		import com.VisitorPattern.sample.*;
		
		public class Main {
		
		    public static void main(String[] args) {
			    // sample code here
		        {
		            ObjectStructure o = new ObjectStructure();
		            o.Attach(new ConcreteElementA());
		            o.Attach(new ConcreteElementB());
		
		            ConcreteVisitor1 v1 = new ConcreteVisitor1();
		            ConcreteVisitor2 v2 = new ConcreteVisitor2();
		
		            o.Accept(v1);
		            o.Accept(v2);
		        }
		    }
		}

----------
**输出**

- com.VisitorPattern.sample.ConcreteElementA被com.VisitorPattern.sample.ConcreteVisitor1访问
- com.VisitorPattern.sample.ConcreteElementB被com.VisitorPattern.sample.ConcreteVisitor1访问
- com.VisitorPattern.sample.ConcreteElementA被com.VisitorPattern.sample.ConcreteVisitor2访问
- com.VisitorPattern.sample.ConcreteElementB被com.VisitorPattern.sample.ConcreteVisitor2访问

----------






