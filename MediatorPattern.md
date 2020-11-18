---
title: 中介者模式
categories:
  - 设计模式
tags:
  - 行为型模式
date: 2020-06-02 11:56:29
author: Jungle

---
# 引言 #
在现实生活中，常常会出现好多对象之间存在复杂的交互关系，这种交互关系常常是“网状结构”，它要求每个对象都必须知道它需要交互的对象。如果把这种“网状结构”改为“星形结构”的话，将大大降低它们之间的“耦合性”。这样的例子还有很多，例如，“房屋中介”、“人才交流中心”等。

# 定义 #
- 定义一个中介对象来封装一系列对象之间的交互，使原有对象之间的耦合松散，且可以独立地改变它们之间的交互。
- 中介者模式又叫调停模式，它是迪米特法则的典型应用。

# 优缺点 #

## 优点 ##
- 降低了对象之间的耦合性，使得对象易于独立地被复用。
- 将对象间的多对多关联转变为一对多的关联，提高系统的灵活性，使得系统易于维护和扩展。

## 缺点 ##
- 当同事类太多时，中介者的职责将很大，它会变得复杂而庞大，以至于系统难以维护。

# 使用场景 #
- 系统中对象之间存在比较复杂的引用关系，导致它们之间的依赖关系结构混乱而且难以复用该对象;
- 想通过一个中间类来封装多个类中的行为，而又不想生成太多的子类。

# 中介者模式的结构 #
- 抽象中介者（Mediator）角色：它是中介者的接口，提供了同事对象注册与转发同事对象信息的抽象方法。
- 具体中介者（ConcreteMediator）角色：实现中介者接口，协调各个同事角色之间的交互关系，因此它依赖于同事角色。
- 抽象同事类（Colleague）角色：定义同事类的接口，保存中介者对象，提供同事对象交互的抽象方法，实现所有相互影响的同事类的公共功能。
- 具体同事类（Concrete Colleague）角色：是抽象同事类的实现者，当需要与其他同事对象交互时，由中介者对象负责后续的交互。


# 实现代码 #
----------
		package com.MediatorPattern.example;
		
		//抽象中介者
		public abstract class Mediator {
		    //定义一个抽象的发送消息方法, 得到同事对象和发送消息
		    public abstract void Send(String message, Colleague colleague);
		}

----------
		package com.MediatorPattern.example;
		
		//抽象同事类
		public abstract class Colleague {
		    protected Mediator mediator;
		
		    //构造方法, 得到中介者对象
		    public Colleague(Mediator mediator){
		        this.mediator = mediator;
		    }
		}

----------

		package com.MediatorPattern.example;
		
		public class ConcreteColleague1 extends Colleague {
		    public ConcreteColleague1(Mediator mediator) {
		        super(mediator);
		    }
		
		    //发送信息时通常是中介者发送出去的
		    public void Send(String message){
		        //当前对象(colleague1)通过中介发送消息
		        mediator.Send(message, this);
		    }
		
		    public void Notify(String message){
		        System.out.println("同事1得到信息:" + message);
		    }
		}

----------
		package com.MediatorPattern.example;
		
		public class ConcreteColleague2 extends Colleague{
		    public ConcreteColleague2(Mediator mediator) {
		        super(mediator);
		    }
		
		    public void Send(String message){
		        mediator.Send(message, this);
		    }
		
		    public void Notify(String message){
		        System.out.println("同事2得到信息:" + message);
		    }
		}

----------
		package com.MediatorPattern.example;
		
		public class ConcreteMediator extends Mediator {
		    private ConcreteColleague1 colleague1;
		    private ConcreteColleague2 colleague2;
		
		    public void setColleague1(ConcreteColleague1 colleague1){
		        this.colleague1 = colleague1;
		    }
		    public void setColleague2(ConcreteColleague2 colleague2){
		        this.colleague2 = colleague2;
		    }
		
		    /*重写发送信息的方法, 根据对象做出选择判断, 通知对象*/
		    @Override
		    public void Send(String message, Colleague colleague) {
		        if(colleague == colleague1){
		            //colleague1 通过中介发送消息给 colleague2, colleague2 接受消息并调用 Notify().
		            colleague2.Notify(message);
		        }else {
		            colleague1.Notify(message);
		        }
		    }
		}

----------
		package com.MediatorPattern;
		
		import com.MediatorPattern.example.ConcreteColleague1;
		import com.MediatorPattern.example.ConcreteColleague2;
		import com.MediatorPattern.example.ConcreteMediator;
		
		public class Main {
		
		    public static void main(String[] args) {
			    // example test code
		        {
		            ConcreteMediator m = new ConcreteMediator();
		
		            ConcreteColleague1 c1 = new ConcreteColleague1(m);
		            ConcreteColleague2 c2 = new ConcreteColleague2(m);
		
		            m.setColleague1(c1);
		            m.setColleague2(c2);
		
		            c1.Send("吃过饭了没?");
		            c2.Send("没有, 你吃了没?");
		        }
		    }
		}

----------
**输出:**

- 同事2得到信息:吃过饭了没?
- 同事1得到信息:没有, 你吃了没?

----------



