---
title: 责任链模式
categories:
  - 设计模式
tags:
  - 行为型模式
date: 2020-05-13 21:36:19
author: Jungle

---
# Why? #
1. 经常存在这样的情况：可能有多个不同的对象完成同一类工作，
2. 例如文档打印输出，可以选择不同的打印机名称。
3. 然而文档编辑器（或者是系统）事先并不知道有多少种打印机可以使用，不知道机器上安装了哪些打印设备。
4. 这些设备有些是系统安装时自带的，有些是后续安装的。
5. 如何让系统能够访问这些事先不知道是哪一个类型的打印机呢？
6. 操作系统规定了打印机驱动程序标准接口，所有打印机都要提供支持这种接口的驱动程序。
7. 当打印机安装时，相应的驱动程序就被安装到系统中，被链入到打印驱动链表。
8. 当文档编辑器（或者是系统）要打印输出文档时，
9. 首先要选择打印机（指定打印机名称），通过该打印机名称（实际上是打印机ID）将打印命令请求发给打印驱动链表。
10. 打印驱动链表中的打印驱动程序根据请求指定的打印机ID确定是否与自身的名字ID所匹配，匹配则执行请求，不匹配则将请求传递给链表中的下一驱动程序。


# Defination #
- 使多个对象都有机会处理请求, 从而避免请求的发送者和接收者之间的耦合关系.
- 将多个对象连成一条链, 并沿着这条链传递该请求, 直到有一个对象处理它为止.


# Structure #
- 抽象处理者（Handler）角色：定义一个处理请求的接口，包含抽象处理方法和一个后继连接。
- 具体处理者（Concrete Handler）角色：实现抽象处理者的处理方法，判断能否处理本次请求，如果可以- 处理请求则处理，否则将该请求转给它的后继者。
- 客户类（Client）角色：创建处理链，并向链头的具体处理者对象提交请求，它不关心处理细节和请求的传递过程。
	

# advantages and disadvantages  #
## advantages ##
- 降低了对象之间的耦合度。该模式使得一个对象无须知道到底是哪一个对象处理其请求以及链的结构，发送者和接收者也无须拥有对方的明确信息。
- 增强了系统的可扩展性。可以根据需要增加新的请求处理类，满足开闭原则。
- 增强了给对象指派职责的灵活性。当工作流程发生变化，可以动态地改变链内的成员或者调动它们的次序，也可动态地新增或者删除责任。
- 责任链简化了对象之间的连接。每个对象只需保持一个指向其后继者的引用，不需保持其他所有处理者的引用。
- 责任分担。每个类只需要处理自己该处理的工作，不该处理的传递给下一个对象完成，明确各类的责任范围，符合类的单一职责原则。
## disadvantages ##
- 不能保证每个请求一定被处理。由于一个请求没有明确的接收者，所以不能保证它一定会被处理，该请求可能一直传到链的末端都得不到处理。
- 对比较长的责任链，请求的处理可能涉及多个处理对象，系统性能将受到一定影响。
- 责任链建立的合理性要靠客户端来保证，增加了客户端的复杂性，可能会由于责任链的错误设置而导致系统出错，如可能会造成循环调用。


# How #
- 有多个对象可以处理一个请求，哪个对象处理该请求由运行时刻自动确定。
- 可动态指定一组对象处理请求，或添加新的处理者。
- 在不明确指定请求处理者的情况下，向多个处理者中的一个提交请求。


# Example Code #
----------
		package com.ChainofResponsiblityPattern;
		
		public abstract class Handler {
		    protected Handler successor;
		
		    //设置继任者
		    public void SetSuccessor(Handler successor){
		        this.successor = successor;
		    }
		
		    //处理请求的抽象方法
		    public abstract void HandleRequest(int request);
		}

----------
		package com.ChainofResponsiblityPattern;
		
		public class ConcreteHandler_1 extends Handler {
		    @Override
		    public void HandleRequest(int request) {
		        if(request >= 0 && request < 10){
		            System.out.println( this.getClass().getName() + "处理请求" + request);
		        }else if(successor != null){
		            successor.HandleRequest(request);
		        }
		    }
		}

----------
		package com.ChainofResponsiblityPattern;
		
		public class ConcreteHandler_2 extends Handler {
		    @Override
		    public void HandleRequest(int request) {
		        if(request >= 10 && request < 20){
		            System.out.println( this.getClass().getName() + "处理请求" + request);
		        }else if(successor != null){
		            successor.HandleRequest(request);
		        }
		    }
		}

----------
		package com.ChainofResponsiblityPattern;
		
		public class ConcreteHandler_3 extends Handler {
		    @Override
		    public void HandleRequest(int request) {
		        if(request >= 20 && request < 30){
		            System.out.println( this.getClass().getName() + "处理请求" + request);
		        }else if(successor != null){
		            successor.HandleRequest(request);
		        }
		    }
		}

----------
		package com.ChainofResponsiblityPattern;
		
		public class Main {
		
		    public static void main(String[] args) {
			// write your code here
		        Handler h1 = new ConcreteHandler_1();
		        Handler h2 = new ConcreteHandler_2();
		        Handler h3 = new ConcreteHandler_3();
		
		        h1.SetSuccessor(h2);
		        h2.SetSuccessor(h3);
		
		        int[] requests = {2, 5, 14, 22, 18, 3, 27, 20};
		
		        for (int request:requests) {
		            h1.HandleRequest(request);
		        }
		    }
		}

----------
**Run output:**

		com.ChainofResponsiblityPattern.ConcreteHandler_1处理请求2
		com.ChainofResponsiblityPattern.ConcreteHandler_1处理请求5
		com.ChainofResponsiblityPattern.ConcreteHandler_2处理请求14
		com.ChainofResponsiblityPattern.ConcreteHandler_3处理请求22
		com.ChainofResponsiblityPattern.ConcreteHandler_2处理请求18
		com.ChainofResponsiblityPattern.ConcreteHandler_1处理请求3
		com.ChainofResponsiblityPattern.ConcreteHandler_3处理请求27
		com.ChainofResponsiblityPattern.ConcreteHandler_3处理请求20
		
		Process finished with exit code 0

----------
# example Code #

----------

		package com.ChainofResponsiblityPattern.example;
		
		public abstract class Manager {
		    protected String name;
		
		    // 管理者的上一级
		    protected Manager superior;
		
		    public Manager(String name){
		        this.name = name;
		    }
		
		    // 设置管理者的上一级
		    public void SetSuperior(Manager superior){
		        this.superior = superior;
		    }
		
		    abstract public void RequestApplications(Request request);
		}

----------
		package com.ChainofResponsiblityPattern.example;
		
		// 经理
		public class CommonManager extends Manager {
		    public CommonManager(String name) {
		        super(name);
		    }
		
		    @Override
		    public void RequestApplications(Request request) {
		        if(request.RequestType == "请假" && request.Number <= 2){
		            System.out.println(name + ":" + request.RequestContent + " 数量" + request.Number + " 被批准");
		        }else {
		            // 其余的申请转到上一级
		            if(superior != null)
		                superior.RequestApplications(request);
		        }
		    }
		}

----------
		package com.ChainofResponsiblityPattern.example;
		
		// 总监
		public class Majordomo extends Manager {
		    public Majordomo(String name) {
		        super(name);
		    }
		
		    @Override
		    public void RequestApplications(Request request) {
		        if(request.RequestType == "请假" && request.Number <= 5){
		            System.out.println(name + ": " + request.RequestContent + " 数量 " + request.Number + " 被批准");
		        }else {
		            // 其余的申请转到上一级
		            if(superior != null)
		                superior.RequestApplications(request);
		        }
		    }
		}

----------
		package com.ChainofResponsiblityPattern.example;
		
		// 总经理
		public class GeneralManager extends Manager {
		    public GeneralManager(String name) {
		        super(name);
		    }
		
		    @Override
		    public void RequestApplications(Request request) {
		        if(request.RequestType == "请假"){
		            System.out.println(name + ":" + request.RequestContent + " 数量 " + request.Number + " 被批准");
		        }else if(request.RequestType == "加薪" && request.Number <= 500){
		            System.out.println(name + ":" + request.RequestContent + " 数量 " + request.Number + " 被批准");
		        }
		        else if(request.RequestType == "加薪" && request.Number > 500){
		            System.out.println(name + ":" + request.RequestContent + " 数量 " + request.Number + " 再说吧");
		        }
		    }
		}

----------
		package com.ChainofResponsiblityPattern;
		
		import com.ChainofResponsiblityPattern.example.CommonManager;
		import com.ChainofResponsiblityPattern.example.GeneralManager;
		import com.ChainofResponsiblityPattern.example.Majordomo;
		import com.ChainofResponsiblityPattern.example.Request;
		
		public class Main {
		
		    public static void main(String[] args) {
			// write your code here
		        {
		//            Handler h1 = new ConcreteHandler_1();
		//            Handler h2 = new ConcreteHandler_2();
		//            Handler h3 = new ConcreteHandler_3();
		//
		//            h1.SetSuccessor(h2);
		//            h2.SetSuccessor(h3);
		//
		//            int[] requests = {2, 5, 14, 22, 18, 3, 27, 20};
		//
		//            for (int request:requests) {
		//                h1.HandleRequest(request);
		//            }
		        }
		        // 加薪案例:
		        {
		            CommonManager jinli = new CommonManager("金利");
		            Majordomo zongjian = new Majordomo("宗剑");
		            GeneralManager zhongjingli = new GeneralManager("钟精励");
		
		            jinli.SetSuperior(zongjian);
		            zongjian.SetSuperior(zhongjingli);
		
		            Request request1 = new Request();
		            request1.RequestType = "请假";
		            request1.RequestContent = "小菜请假";
		            request1.Number = 1;
		            jinli.RequestApplications(request1);
		
		            Request request2 = new Request();
		            request2.RequestType = "请假";
		            request2.RequestContent = "小菜请假";
		            request2.Number = 4;
		            jinli.RequestApplications(request2);
		
		            Request request3 = new Request();
		            request3.RequestType = "加薪";
		            request3.RequestContent = "小菜请求加薪";
		            request3.Number = 500;
		            jinli.RequestApplications(request3);
		
		            Request request4 = new Request();
		            request4.RequestType = "加薪";
		            request4.RequestContent = "小菜请求加薪";
		            request4.Number = 1000;
		            jinli.RequestApplications(request4);
		
		        }
		
		    }
		}

----------
**Run output:**

		金利:小菜请假 数量1 被批准
		宗剑: 小菜请假 数量 4 被批准
		钟精励:小菜请求加薪 数量 500 被批准
		钟精励:小菜请求加薪 数量 1000 再说吧
		
		Process finished with exit code 0

----------
**代码参考来自大话设计模式**




	