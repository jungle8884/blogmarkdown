---
title: 状态模式
categories:
  - 设计模式
tags:
  - 行为型模式
date: 2020-06-04 14:49:24
author: Jungle

---
# 引言 #
- 在软件开发过程中，应用程序中的有些对象可能会根据不同的情况做出不同的行为，我们把这种对象称为有状态的对象，而把影响对象行为的一个或多个动态变化的属性称为状态。
- 当有状态的对象与外部事件产生互动时，其内部状态会发生改变，从而使得其行为也随之发生改变。
	- 如人的情绪有高兴的时候和伤心的时候，不同的情绪有不同的行为，当然外界也会影响其情绪变化。
- 对这种有状态的对象编程，传统的解决方案是：
	- 将这些所有可能发生的情况全都考虑到，然后使用 if-else 语句来做状态判断，再进行不同情况的处理。
	- 但当对象的状态很多时，程序会变得很复杂。
	- 而且增加新的状态要添加新的 if-else 语句，这违背了“开闭原则”，不利于程序的扩展。
- 以上问题如果采用“状态模式”就能很好地得到解决。
- 状态模式的解决思想是：
	- 当控制一个对象状态转换的条件表达式过于复杂时，把相关“判断逻辑”提取出来，放到一系列的状态类当中，这样可以把原来复杂的逻辑判断简单化。

# 定义 #
- 对有状态的对象，把复杂的“判断逻辑”提取到不同的状态对象中，允许状态对象在其内部状态发生改变时改变其行为。
- 当一个对象的内在状态改变时允许改变其行为, 这个对象看起来像是改变了其类.

# 优点 #
- 状态模式将与特定状态相关的行为局部化到一个状态对象中，并且将不同状态的行为分割开来，满足“单一职责原则”。
- 减少对象间的相互依赖。将不同的状态引入独立的对象中会使得状态转换变得更加明确，且减少对象间的相互依赖。
- 有利于程序的扩展。通过定义新的子类很容易地增加新的状态和转换。

# 缺点 #
- 状态模式的使用必然会增加系统的类与对象的个数。
- 状态模式的结构与实现都较为复杂，如果使用不当会导致程序结构和代码的混乱。
- 状态模式把受环境改变的对象行为包装在不同的状态对象里，其意图是让一个对象在其内部状态改变的时候，其行为也随之改变。

# 使用场景 #
- 当一个对象的行为取决于它的状态, 并且它必须在运行时刻根据状态改变它的行为时, 就可以考虑使用状态模式了.

# 结构 #
- 环境（Context）角色：也称为上下文，它定义了客户感兴趣的接口，维护一个当前状态，并将与状态相关的操作委托给当前状态对象来处理。
- 抽象状态（State）角色：定义一个接口，用以封装环境对象中的特定状态所对应的行为。
- 具体状态（Concrete  State）角色：实现抽象状态所对应的行为。

# 实现代码 #

## sample代码 ##
----------
		package com.StatePattern.sample;
		
		public abstract class State {
		    public abstract void Handle(Context context);
		}

----------
		package com.StatePattern.sample;
		
		//Concrete类, 具体状态, 每一个子类实现一个与Context的一个状态相关的行为.
		public class ConcreteStateA extends State {
		    @Override
		    public void Handle(Context context) {
		        //设置ConcreteStateA的下一状态是ConcreteStateB
		        context.setState(new ConcreteStateB());
		    }
		}

----------
		package com.StatePattern.sample;
		
		//Concrete类, 具体状态, 每一个子类实现一个与Context的一个状态相关的行为.
		public class ConcreteStateB extends State {
		    @Override
		    public void Handle(Context context) {
		        //设置ConcreteStateB的下一状态是ConcreteStateA
		        context.setState(new ConcreteStateA());
		    }
		}

----------
		package com.StatePattern.sample;
		
		//Context类, 维护一个ConcreteState子类的实例, 这个实例定义当前的状态.
		public class Context {
		    private State state;
		    //定义Context的初始状态
		    public Context(State state){
		        this.state = state;
		    }
		
		    public State getState() {
		        return state;
		    }
		    public void setState(State state) {
		        this.state = state;
		        System.out.println("当前状态:" + state.getClass().getName());
		    }
		
		    //对请求做处理, 并设置下一状态
		    public void Request(){
		        state.Handle(this);
		    }
		}

----------
		package com.StatePattern;
		
		import com.StatePattern.sample.ConcreteStateA;
		import com.StatePattern.sample.Context;
		
		public class Main {
		
		    public static void main(String[] args) {
			    // sample code
		        {
		            //设置Context的初始状态为ConcreteStateA
		            Context c = new Context(new ConcreteStateA());
		
		            c.Request();
		            c.Request();
		            c.Request();
		            c.Request();
		        }
		    }
		}

----------
**输出**

- 当前状态:com.StatePattern.sample.ConcreteStateB
- 当前状态:com.StatePattern.sample.ConcreteStateA
- 当前状态:com.StatePattern.sample.ConcreteStateB
- 当前状态:com.StatePattern.sample.ConcreteStateA

----------


## example代码 ##
----------
		package com.StatePattern.example;
		
		//抽象状态
		public abstract class State {
		    public abstract void WriteProgram(Work w);
		}

----------
		package com.StatePattern.example;
		
		//工作
		public class Work {
		    private State current;
		    public Work(){
		        //工作初始化为上午工作状态, 即上午9点开始上班
		        current = new ForenoonState();
		    }
		
		    //"钟点"属性, 状态转换的依据.
		    private double hour;
		    public double getHour() {
		        return hour;
		    }
		    public void setHour(double hour) {
		        this.hour = hour;
		    }
		
		    //"任务完成"属性, 是否能下班的依据.
		    private boolean finish = false;
		    public void setFinish(boolean finish) {
		        this.finish = finish;
		    }
		    public boolean isFinish() {
		        return finish;
		    }
		
		    //设置状态
		    public void SetState(State s){
		        current = s;
		    }
		
		    //写程序
		    public void WriteProgram(){
		        current.WriteProgram(this);
		    }
		}

----------
		package com.StatePattern.example;
		
		//上午工作状态
		public class ForenoonState extends State {
		    @Override
		    public void WriteProgram(Work w) {
		        if(w.getHour() < 12){
		            System.out.println("当前时间: " + w.getHour() + "点" + "上午工作, 精神百倍");
		        }else {
		            w.SetState(new NoonState());
		            w.WriteProgram();
		        }
		    }
		}

----------
		package com.StatePattern.example;
		
		//中午工作状态
		public class NoonState extends State {
		    @Override
		    public void WriteProgram(Work w) {
		        if(w.getHour() < 13){
		            System.out.println("当前时间: " + w.getHour() + "点" + " 饿了, 午饭: 犯困, 午休.");
		        }else {
		            //超过13点, 则转入下午工作状态
		            w.SetState(new AfternoonState());
		            w.WriteProgram();
		        }
		    }
		}

----------
		package com.StatePattern.example;
		
		//下午和傍晚工作状态
		public class AfternoonState extends State {
		    @Override
		    public void WriteProgram(Work w) {
		        if(w.getHour() < 17){
		            System.out.println("当前时间: " + w.getHour() + "点" + "下午状态还不错, 继续努力");
		        }else{
		            //超过17点, 则转入傍晚工作状态.
		            w.SetState(new EveningState());
		            w.WriteProgram();
		        }
		    }
		}

----------
		package com.StatePattern.example;
		
		//晚间工作状态
		public class EveningState extends State {
		    @Override
		    public void WriteProgram(Work w) {
		        if(w.isFinish()){
		            //如果完成任务, 则转入下班状态
		            w.SetState(new RestState());
		            w.WriteProgram();
		        }else{
		            if(w.getHour() < 21){
		                System.out.println("当前时间: " + w.getHour() + "点 加班哦, 疲惫之极.");
		            }else {
		                //超过21点, 则转入睡眠工作状态
		                w.SetState(new SleepingState());
		                w.WriteProgram();
		            }
		        }
		    }
		}

----------
		package com.StatePattern.example;
		
		//下班休息状态
		public class RestState extends State {
		    @Override
		    public void WriteProgram(Work w) {
		        System.out.println("当前时间: " + w.getHour() + "点下班回家了.");
		    }
		}

----------
		package com.StatePattern.example;
		
		//睡眠状态
		public class SleepingState extends State {
		    @Override
		    public void WriteProgram(Work w) {
		        System.out.println("当前时间: " + w.getHour() + "点不行了, 睡着了.");
		    }
		}

----------
		package com.StatePattern;
		
		import com.StatePattern.example.Work;
		
		public class Main {
		
		    public static void main(String[] args) {
		        //example
		        {
		            //紧急项目
		            Work emergencyProjects = new Work();
		
		            emergencyProjects.setHour(9);
		            emergencyProjects.WriteProgram();
		
		            emergencyProjects.setHour(10);
		            emergencyProjects.WriteProgram();
		
		            emergencyProjects.setHour(12);
		            emergencyProjects.WriteProgram();
		
		            emergencyProjects.setHour(13);
		            emergencyProjects.WriteProgram();
		
		            emergencyProjects.setHour(14);
		            emergencyProjects.WriteProgram();
		
		            emergencyProjects.setHour(17);
		            //emergencyProjects.setFinish(true);
		            emergencyProjects.setFinish(false);
		            emergencyProjects.WriteProgram();
		
		            emergencyProjects.setHour(19);
		            emergencyProjects.WriteProgram();
		
		            emergencyProjects.setHour(22);
		            emergencyProjects.WriteProgram();
		        }
		    }
		}

----------
**输出:**

- 当前时间: 9.0点上午工作, 精神百倍
- 当前时间: 10.0点上午工作, 精神百倍
- 当前时间: 12.0点 饿了, 午饭: 犯困, 午休.
- 当前时间: 13.0点下午状态还不错, 继续努力
- 当前时间: 14.0点下午状态还不错, 继续努力
- 当前时间: 17.0点 加班哦, 疲惫之极.
- 当前时间: 19.0点 加班哦, 疲惫之极.
- 当前时间: 22.0点不行了, 睡着了.
----------










