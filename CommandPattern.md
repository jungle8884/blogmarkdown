---
title: 命令模式
categories:
  - 设计模式
tags:
  - 行为型模式
date: 2020-05-13 21:36:42
author: Jungle

---
# Why? #

- 在软件系统中，“方法的请求者”与“方法的实现者”之间通常存在紧密的耦合关系--调用关系。这不利于软件功能的扩展与维护。例如，想对行为进行“撤销、重做、记录”等处理都很不方便，因此“如何将方法的请求者与方法的实现者解耦？”变得很重要，命令模式能很好地解决这个问题。


# Defination #
	
- 将一个请求封装为一个对象，使发出请求的责任和执行请求的责任分割开。请求对象和执行对象两者之间通过命令对象进行沟通，这样方便将命令对象进行储存、传递、调用、增加与管理。


# advantages and disadvantages #

## advantages ##

- 降低系统的耦合度。命令模式能将调用操作的对象与实现该操作的对象解耦。
- 增加或删除命令非常方便。采用命令模式增加与删除命令不会影响其他类，它满足“开闭原则”，对扩展比较灵活。
- 可以实现宏命令。命令模式可以与组合模式结合，将多个命令装配成一个组合命令，即宏命令。
- 方便实现 Undo 和 Redo 操作。命令模式可以与后面介绍的备忘录模式结合，实现命令的撤销与恢复。

## disadvantages ##
	
- 可能产生大量具体命令类。因为针对每一个具体操作都需要设计一个具体命令类，这将增加系统的复杂性。


# example code #

----------
		package com.CommandPattern;
		
		// 抽象命令
		public abstract class Command {
		    protected Barbecuer receiver;
		
		    public Command(Barbecuer receiver){
		        this.receiver = receiver;
		    }
		
		    // 执行命令
		    public abstract void ExcuteCommand();
		}

----------
		package com.CommandPattern;
		
		public class Barbecuer {
		    // 烤羊肉
		    public void BakeMutton(){
		        System.out.println("烤羊肉串!");
		    }
		
		    // 烤鸡翅
		    public void BakeChickenWing(){
		        System.out.println("烤鸡翅!");
		    }
		}

----------
		package com.CommandPattern;
		
		// 烤羊肉命令
		public class BakeMuttonCommand extends Command {
		    public BakeMuttonCommand(Barbecuer receiver) {
		        super(receiver);
		    }
		
		    @Override
		    public void ExcuteCommand() {
		        receiver.BakeMutton();
		    }
		}

----------
		package com.CommandPattern;
		
		// 烤鸡翅命令
		public class BakeChickenWingCommand extends Command {
		    public BakeChickenWingCommand(Barbecuer receiver) {
		        super(receiver);
		    }
		
		    @Override
		    public void ExcuteCommand() {
		        receiver.BakeChickenWing();
		    }
		}

----------
		package com.CommandPattern;
		
		import java.util.Date;
		import java.text.SimpleDateFormat;
		import java.util.ArrayList;
		import java.util.List;
		
		// 服务员
		public class Waiter {
		
		    private List<Command> orders = new ArrayList<>();
		
		    // 设置订单
		    public void SetOrder(Command command){
		        // 在客户提出请求时, 对没货的烧烤进行回绝.
		        if(command.toString().contains("BakeChickenWingCommand")){
		            System.out.println("服务员: 鸡翅没有了, 请点别的烧烤. ");
		        }
		        else {
		            // 记录客户所在的烧烤的日志, 以备算账收钱.
		            orders.add(command);
		
		            // 设置当前时间
		            Date now = new Date();
		            SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");//可以方便地修改日期格式
		            String DataTime = dateFormat.format(now);
		
		            System.out.println("增加订单: " + command.toString() + " 时间: " + DataTime);
		        }
		    }
		
		    // 取消订单
		    public void CancelOrder(Command command){
		        orders.remove(command);
		
		        // 设置当前时间
		        Date now = new Date();
		        SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy/MM/dd HH:mm:ss");//可以方便地修改日期格式
		        String DataTime = dateFormat.format(now);
		
		        System.out.println("取消订单: " + command.toString() + " 时间: " + DataTime);
		    }
		
		    // 通知执行
		    public void Notify(){
		        // 根据用户点好的烧烤订单通知厨房制作
		        for (Command cmd:orders) {
		            cmd.ExcuteCommand();
		        }
		    }
		}

----------
		package com.CommandPattern;
		
		public class Main {
		
		    public static void main(String[] args) {
			// write your code here
		        // 客户端实现
		        {
		            // 开店前的准备
		            Barbecuer boy = new Barbecuer();
		            Command bakeMuttoncommand_1 = new BakeMuttonCommand(boy);
		            Command bakeMuttoncommand_2 = new BakeMuttonCommand(boy);
		            Command bakeChichenWingCommand_1 = new BakeChickenWingCommand(boy);
		            Waiter girl = new Waiter();
		
		            // 开门营业 顾客点菜
		            girl.SetOrder(bakeMuttoncommand_1);
		            girl.SetOrder(bakeMuttoncommand_2);
		            girl.SetOrder(bakeChichenWingCommand_1);
		
		            // 点菜完毕, 通知厨房
		            girl.Notify();
		        }
		    }
		}

----------
**Out put:**

		增加订单: com.CommandPattern.BakeMuttonCommand@71e0338f 时间: 2020/05/16 00:49:46
		增加订单: com.CommandPattern.BakeMuttonCommand@7aa35e0f 时间: 2020/05/16 00:49:46
		服务员: 鸡翅没有了, 请点别的烧烤. 
		烤羊肉串!
		烤羊肉串!
		
		Process finished with exit code 0

----------


## Normal Code ##

----------
		package com.CommandPattern.Notion;
		
		// 用来声明执行操作的接口
		public abstract class Command {
		    protected Receiver receiver;
		
		    public Command(Receiver receiver){
		        this.receiver = receiver;
		    }
		
		    abstract public void Execute();
		}

----------
		package com.CommandPattern.Notion;
		
		// 将一个接收者对象绑定于一个动作, 调用接受者相应的操作, 以实现Execute.
		public class ConcreteCommand extends Command {
		    public ConcreteCommand(Receiver receiver) {
		        super(receiver);
		    }
		
		    @Override
		    public void Execute() {
		        receiver.Action();
		    }
		}

----------
		package com.CommandPattern.Notion;
		
		// 知道如何实施与执行一个请求相关的操作, 任何类都可能作为一个接收者.
		public class Receiver {
		    public void Action(){
		        System.out.println("执行请求! ");
		    }
		}

----------
		package com.CommandPattern.Notion;
		
		// 要求该命令执行这个请求
		public class Invoker {
		    private Command command;
		
		    public void SetCommand(Command command){
		        this.command = command;
		    }
		
		    public void ExecuteCommand(){
		        command.Execute();
		    }
		}

----------
		package com.CommandPattern;
		
		import com.CommandPattern.Notion.Command;
		import com.CommandPattern.Notion.ConcreteCommand;
		import com.CommandPattern.Notion.Invoker;
		import com.CommandPattern.Notion.Receiver;
		
		
		public class Main {
		
		    public static void main(String[] args) {
			// write your code here
		        // 客户端实现
		        {
		//            // 开店前的准备
		//            Barbecuer boy = new Barbecuer();
		//            Command bakeMuttoncommand_1 = new BakeMuttonCommand(boy);
		//            Command bakeMuttoncommand_2 = new BakeMuttonCommand(boy);
		//            Command bakeChichenWingCommand_1 = new BakeChickenWingCommand(boy);
		//            Waiter girl = new Waiter();
		//
		//            // 开门营业 顾客点菜
		//            girl.SetOrder(bakeMuttoncommand_1);
		//            girl.SetOrder(bakeMuttoncommand_2);
		//            girl.SetOrder(bakeChichenWingCommand_1);
		//
		//            // 点菜完毕, 通知厨房
		//            girl.Notify();
		        }
		        {
		            Receiver r = new Receiver();
		            Command c = new ConcreteCommand(r);
		            Invoker i = new Invoker();
		            i.SetCommand(c);
		            i.ExecuteCommand();
		        }
		    }
		}

----------
**Out put:**

		执行请求! 
		
		Process finished with exit code 0

----------








