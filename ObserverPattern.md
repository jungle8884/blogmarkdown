---
title: 观察者模式
categories:
  - 设计模式
tags:
  - 行为型模式
date: 2020-06-02 10:45:56
author: Jungle

---
# 定义 #

**指多个对象间存在一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。**

- 这种模式有时又称作发布-订阅模式（Pub-Sub Pattern）。
- 观察者模式是一种对象行为型模式，其主要优点: 
	- 降低了目标与观察者之间的耦合关系，两者之间是抽象耦合关系;
	- 目标与观察者之间建立了一套触发机制。

# 缺点 #

- 目标与观察者之间的依赖关系并没有完全解除，而且有可能出现循环引用。
- 当观察者对象很多时，通知的发布会花费很多时间，影响程序的效率。

# 应用场景 #

- 对象间存在一对多关系，一个对象的改变需要同时改变其他对象; 而且它不知道具体有多少对象有待改变时, 应该考虑使用观察者模式。
- 当一个抽象模型有两个方面，其中一个方面依赖于另一方面时，可将这二者封装在独立的对象中以使它们可以各自独立地改变和复用。
- 实现观察者模式时要注意具体主题对象和具体观察者对象之间不能直接调用，否则将使两者之间紧密耦合起来，这违反了面向对象的设计原则。

# 观察者模式的结构： #
1. 抽象主题（Subject）角色：也叫抽象目标类、抽象发布者，它提供了一个用于保存观察者对象的聚集类和增加、删除观察者对象的方法，以及通知所有观察者的抽象方法。
2. 具体主题（Concrete Subject）角色：也叫具体目标类、具体发布者，它实现抽象目标中的通知方法，当具体主题的内部状态发生改变时，通知所有注册过的观察者对象。
3. 抽象观察者（Observer）角色：也叫抽象订阅者，它是一个抽象类或接口，它包含了一个更新自己的抽象方法，当接到具体主题的更改通知时被调用。
4. 具体观察者（Concrete Observer）角色：也叫具体订阅者，实现抽象观察者中定义的抽象方法，以便在得到目标的更改通知时更新自身的状态。

# 代码实现 #

## sample代码 ##
----------
		package com.ObserverPattern.sample;
		
		import java.util.ArrayList;
		import java.util.List;
		
		//抽象主题
		public abstract class Subject {
		    private List<Observer> observers = new ArrayList<Observer>();
		
		    //增加观察者
		    public void Attach(Observer observer){
		        observers.add(observer);
		    }
		    //移除观察者
		    public void Detach(Observer observer){
		        observers.remove(observer);
		    }
		    //通知
		    public void Notify(){
		        for (Observer o:observers
		             ) {
		            o.Update();
		        }
		    }
		}

----------
		package com.ObserverPattern.sample;
		
		//抽象观察者
		public abstract class Observer {
		    public abstract void Update();
		}

----------
		package com.ObserverPattern.sample;
		
		//具体主题
		public class ConcreteSubject extends Subject {
		    private String subjectState;
		
		    //具体观察者状态
		    public String getSubjectState(){
		        return subjectState;
		    }
		    public void setSubjectState(String subjectState) {
		        this.subjectState = subjectState;
		    }
		}

----------
		package com.ObserverPattern.sample;
		
		//具体观察者
		public class ConcreteObserver extends Observer {
		    private String name;
		    private String observerState;
		    private ConcreteSubject subject;
		
		    public ConcreteObserver(ConcreteSubject subject, String name){
		        this.subject = subject;
		        this.name = name;
		    }
		
		    @Override
		    public void Update() {
		        observerState = subject.getSubjectState();
		        System.out.println("观察者" + name + "的新状态是" + observerState);
		    }
		
		    public void setSubject(ConcreteSubject subject) {
		        this.subject = subject;
		    }
		    public ConcreteSubject getSubject() {
		        return subject;
		    }
		}

----------
		package com.ObserverPattern;
		
		import com.ObserverPattern.sample.ConcreteObserver;
		import com.ObserverPattern.sample.ConcreteSubject;
		
		public class Main {
		
		    public static void main(String[] args) {
			// write your code here
		        //sample测试代码
		        {
		            ConcreteSubject s = new ConcreteSubject();
		            s.Attach(new ConcreteObserver(s, "X"));
		            s.Attach(new ConcreteObserver(s, "Y"));
		            s.Attach(new ConcreteObserver(s, "Z"));
		
		            s.setSubjectState("ABC");
		            s.Notify();
		        }
		    }
		}

----------
**输出**

- 观察者X的新状态是ABC
- 观察者Y的新状态是ABC
- 观察者Z的新状态是ABC

----------


