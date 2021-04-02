---
title: Java-Note21
categories:
  - 后端
  - Java
tags:
  - 进程
  - 线程
author:
  - Jungle
date: 2021-01-09 11:38:15

---
# 进程 #
进程是正在运行的程序。

- 是系统进行资源分配和调用的独立单位
- 每一个进程都有它自己的内存空间和系统资源

# 线程 #
是进程中的单个顺序控制流，是一条执行路径

- 单线程：一个进程如果只有一条执行路径，则称为单线程程序
- 多线程：一个进程如果有多条执行路径，则称为多线程程序


# 多线程的实现方式 #

## 方式1：继承Thread类 ##
- 定义一个类MyThread继承Thread类
- 在MyThread类中重写run()方法
- 创建MyThread类的对象
- 启动线程

- **实现代码：**

		public class MyThread extends Thread {
		    @Override
		    public void run() {
		        for(int i=0; i<100; i++) {
		            System.out.println(i);
		        }
		    }
		}
				
		//run()：封装线程执行的代码，直接调用，相当于普通方法的调用
		//start()：启动线程；然后由JVM调用此线程的run()方法
	
		public class MyThreadDemo {
		    public static void main(String[] args) {
		        MyThread my1 = new MyThread();
		        MyThread my2 = new MyThread();
		
		//        my1.run();
		//        my2.run();
		
		        //void start​() 导致此线程开始执行; Java虚拟机调用此线程的run方法
		        my1.start();
		        my2.start();
		    }
		}

## 方式2：实现Runnable接口 ##
- 定义一个类MyRunnable实现Runnable接口
- 在MyRunnable类中重写run()方法
- 创建MyRunnable类的对象
- 创建Thread类的对象，把MyRunnable对象作为构造方法的参数
- 启动线程

- **实现代码**

		public class MyRunnable implements Runnable {
		
		    @Override
		    public void run() {
		        for(int i=0; i<100; i++) {
		            System.out.println(Thread.currentThread().getName()+":"+i);
		        }
		    }
		}
	
		public class MyRunnableDemo {
		    public static void main(String[] args) {
		        //创建MyRunnable类的对象
		        MyRunnable my = new MyRunnable();
		
		        //创建Thread类的对象，把MyRunnable对象作为构造方法的参数
		        //Thread​(Runnable target)
		//        Thread t1 = new Thread(my);
		//        Thread t2 = new Thread(my);
		        //Thread​(Runnable target, String name)
		        Thread t1 = new Thread(my,"高铁");
		        Thread t2 = new Thread(my,"飞机");
		
		        //启动线程
		        t1.start();
		        t2.start();
		    }
		}

## 相比继承Thread类，实现Runnable接口的好处 ##
- 避免了Java单继承的局限性
- 适合多个相同程序的代码去处理同一个资源的情况，把线程和程序的代码、数据有效分离，较好的体现了面向对象的设计思想

----------

## Thread类中获取和设置线程名称的方法 ##
- void setName(String name)：将此线程的名称更改为等于参数 name
- String getName()：返回此线程的名称
- static Thread currentThread​()： 返回对当前正在执行的线程对象的引用

## 线程调度 ##
**两种调度模型**

- 分时调度模型：所有线程轮流使用 CPU 的使用权，平均分配每个线程占用 CPU 的时间片
- 抢占式调度模型：优先让优先级高的线程使用 CPU，如果线程的优先级相同，那么会随机选择一个，优先级高的线程获取的 CPU 时间片相对多一些
- **Java使用的是抢占式调度模型**

## Thread类中设置和获取线程优先级的方法 ##
- public final void setPriority(int newPriority)：更改此线程的优先级
- public final int getPriority()：返回此线程的优先级
	- 线程默认优先级是5；线程优先级的范围是：1-10
	- 线程优先级高仅仅表示线程获取的CPU时间片的几率高，但是要在次数比较多，或者多次运行的时候才能看到你想要的效果

## 线程控制 ##

- static void sleep​(long millis)：使当前正在执行的线程停留（暂停执行）指定的毫秒数

		public class ThreadSleep extends Thread {
		    @Override
		    public void run() {
		        for (int i = 0; i < 100; i++) {
		            System.out.println(getName() + ":" + i);
		            try {
		                Thread.sleep(1000);
		            } catch (InterruptedException e) {
		                e.printStackTrace();
		            }
		        }
		    }
		}
	
		public class ThreadSleepDemo {
		    public static void main(String[] args) {
		        ThreadSleep ts1 = new ThreadSleep();
		        ThreadSleep ts2 = new ThreadSleep();
		        ThreadSleep ts3 = new ThreadSleep();
		
		        ts1.setName("曹操");
		        ts2.setName("刘备");
		        ts3.setName("孙权");
		
		        ts1.start();
		        ts2.start();
		        ts3.start();
		    }
		}

- void setDaemon​(boolean on)：将此线程标记为守护线程，当运行的线程都是守护线程时，Java虚拟机将退出

		public class ThreadDaemon extends Thread {
		    @Override
		    public void run() {
		        for (int i = 0; i < 100; i++) {
		            System.out.println(getName() + ":" + i);
		        }
		    }
		}
		
		public class ThreadDaemonDemo {
		    public static void main(String[] args) {
		        ThreadDaemon td1 = new ThreadDaemon();
		        ThreadDaemon td2 = new ThreadDaemon();
		
		        td1.setName("关羽");
		        td2.setName("张飞");
		
		        //设置主线程为刘备（当刘备消失后，关羽和张飞很快就消失了）
		        Thread.currentThread().setName("刘备");
		
		        //设置守护线程
		        td1.setDaemon(true);
		        td2.setDaemon(true);
		
		        td1.start();
		        td2.start();
		
		        for(int i=0; i<10; i++) {
		            System.out.println(Thread.currentThread().getName()+":"+i);
		        }
		    }
		}

- void join​()：等待这个线程死亡

		public class ThreadJoin extends Thread {
		    @Override
		    public void run() {
		        for (int i = 0; i < 100; i++) {
		            System.out.println(getName() + ":" + i);
		        }
		    }
		}
	
		public class ThreadJoinDemo {
		    public static void main(String[] args) {
		        ThreadJoin tj1 = new ThreadJoin();
		        ThreadJoin tj2 = new ThreadJoin();
		        ThreadJoin tj3 = new ThreadJoin();
		
		        tj1.setName("康熙");
		        tj2.setName("四阿哥");
		        tj3.setName("八阿哥");
		
		        tj1.start();
		        try {
		            tj1.join(); //康熙挂了，四阿哥和八阿哥才抢夺皇位
		        } catch (InterruptedException e) {
		            e.printStackTrace();
		        }
		        tj2.start();
		        tj3.start();
		    }
		}

----------
# 线程同步 #
	刚才讲解了电影院卖票程序，好像没有什么问题。但是在实际生活中，售票时出票也是需要时间的，
	所以，在出售一张票的时候，需要一点时间的延迟，接下来我们去修改卖票程序中卖票的动作：
	每次出票时间100毫秒，用sleep()方法实现

**卖票出现了问题**

- 相同的票出现了多次
- 出现了负数的票


**问题原因：**

- 线程执行的随机性导致的


**为什么出现问题?(这也是我们判断多线程程序是否会有数据安全问题的标准)**

- 是否是多线程环境
- 是否有共享数据
- 是否有多条语句操作共享数据


**如何解决多线程安全问题呢?**

- 基本思想：让程序没有安全问题的环境


**怎么实现呢?**

- 把多条语句操作共享数据的代码给锁起来，让任意时刻只能有一个线程执行即可
- Java提供了同步代码块的方式来解决

## 同步代码块 ##
- 格式：

		synchronized(任意对象) { 
		    多条语句操作共享数据的代码 
		}

- synchronized(任意对象)：就相当于给代码加锁了，任意对象就可以看成是一把锁
- 同步的好处和弊端： 
	- 好处：解决了多线程的数据安全问题
	- 弊端：当线程很多时，因为每个线程都会去判断同步上的锁，这是很耗费资源的，无形中会降低程序的运行效率

- 示例代码：

			public class SellTicket implements Runnable {
			    private int tickets = 100;
			    private Object obj = new Object();
			
			    @Override
			    public void run() {
			        while (true) {
			            //tickets = 100;
			            //t1,t2,t3
			            //假设t1抢到了CPU的执行权
			            //假设t2抢到了CPU的执行权
			            synchronized (obj) {
			                //t1进来后，就会把这段代码给锁起来
			                if (tickets > 0) {
			                    try {
			                        Thread.sleep(100);
			                        //t1休息100毫秒
			                    } catch (InterruptedException e) {
			                        e.printStackTrace();
			                    }
			                    //窗口1正在出售第100张票
			                    System.out.println(Thread.currentThread().getName() + "正在出售第" + tickets + "张票");
			                    tickets--; //tickets = 99;
			                }
			            }
			            //t1出来了，这段代码的锁就被释放了
			        }
			    }
			}
	
			public class SellTicketDemo {
			    public static void main(String[] args) {
			        SellTicket st = new SellTicket();
			
			        Thread t1 = new Thread(st, "窗口1");
			        Thread t2 = new Thread(st, "窗口2");
			        Thread t3 = new Thread(st, "窗口3");
			
			        t1.start();
			        t2.start();
			        t3.start();
			    }
			}

## 同步方法 ##
**同步方法：就是把synchronized关键字加到方法上**

- 格式：

		修饰符 synchronized 返回值类型 方法名(方法参数) { }


**同步方法的锁对象是什么呢?**

- this


**同步静态方法：就是把synchronized关键字加到静态方法上**

- 格式：

		修饰符 static synchronized 返回值类型 方法名(方法参数) { }


**同步静态方法的锁对象是什么呢?**

- 类名.class

		public class SellTicket implements Runnable {
		    private static int tickets = 100;
		    private Object obj = new Object();
		    private int x = 0;
		
		    @Override
		    public void run() {
		        while (true) {
		            if (x % 2 == 0) {
		                synchronized (SellTicket.class) {
		                    if (tickets > 0) {
		                        try {
		                            Thread.sleep(100);
		                        } catch (InterruptedException e) {
		                            e.printStackTrace();
		                        }
		                        System.out.println(Thread.currentThread().getName() + "正在出售第" + tickets + "张票");
		                        tickets--;
		                    }
		                }
		            } else {
		                sellTicket();
		            }
		            x++;
		        }
		    }
		
		    private static synchronized void sellTicket() {
		        if (tickets > 0) {
		            try {
		                Thread.sleep(100);
		            } catch (InterruptedException e) {
		                e.printStackTrace();
		            }
		            System.out.println(Thread.currentThread().getName() + "正在出售第" + tickets + "张票");
		            tickets--;
		        }
		    }
		}


		public class SellTicketDemo {
		    public static void main(String[] args) {
		        SellTicket st = new SellTicket();
		
		        Thread t1 = new Thread(st, "窗口1");
		        Thread t2 = new Thread(st, "窗口2");
		        Thread t3 = new Thread(st, "窗口3");
		
		        t1.start();
		        t2.start();
		        t3.start();
		    }
		}

----------
# 线程安全的类 #

## StringBuffer ##
- 线程安全，可变的字符序列
- 从版本JDK 5开始，被StringBuilder 替代。 通常应该使用**StringBuilder**类，因为它支持所有相同的操作，但它更快，因为它不执行同步


## Vector ##
- 从Java 2平台v1.2开始，该类改进了List接口，使其成为Java Collections Framework的成员。 
- 与新的集合实现不同， Vector被同步。 如果不需要线程安全的实现，建议使用**ArrayList**代替Vector


## Hashtable ##
- 该类实现了一个哈希表，它将键映射到值。 任何非null对象都可以用作键或者值
- 从Java 2平台v1.2开始，该类进行了改进，实现了Map接口，使其成为Java Collections Framework的成员。
-  与新的集合实现不同， Hashtable被同步。 如果不需要线程安全的实现，建议使用**HashMap**代替Hashtable

## 示例代码 ##
**在编辑器内查看源码**

	public class ThreadDemo {
	    public static void main(String[] args) {
	        StringBuffer sb = new StringBuffer(); //线程安全使用
	        StringBuilder sb2 = new StringBuilder();
	
	        Vector<String> v = new Vector<String>();
	        ArrayList<String> array = new ArrayList<String>();
	
	        Hashtable<String,String> ht = new Hashtable<String, String>();
	        HashMap<String,String> hm = new HashMap<String, String>();
	
	        //static <T> List<T> synchronizedList​(List<T> list) 返回由指定列表支持的同步（线程安全）列表
	        List<String> list = Collections.synchronizedList(new ArrayList<String>()); //包装成线程安全的
	    }
	}

----------
# Lock锁 #

	Lock中提供了获得锁和释放锁的方法
	    void lock()：获得锁
	    void unlock()：释放锁
	
	Lock是接口不能直接实例化，这里采用它的实现类ReentrantLock来实例化
	    ReentrantLock​()：创建一个ReentrantLock的实例

## Lock的使用 ##

	public class SellTicket implements Runnable {
	    private int tickets = 100;
	    private Lock lock = new ReentrantLock();
	
	    @Override
	    public void run() {
	        while (true) {
	            try {
	                lock.lock();//加锁
	                if (tickets > 0) {
	                    try {
	                        Thread.sleep(100);
	                    } catch (InterruptedException e) {
	                        e.printStackTrace();
	                    }
	                    System.out.println(Thread.currentThread().getName() + "正在出售第" + tickets + "张票");
	                    tickets--;
	                }
	            } finally {
	                lock.unlock();//释放锁
	            }
	        }
	    }
	}

## 生产者和消费者问题 ##
**生产者消费者模式是一个十分经典的多线程协作的模式，弄懂生产者消费者问题能够让我们对多线程编程的理解更加深刻**

- 所谓生产者消费者问题，实际上主要是包含了两类线程：
	- 一类是生产者线程用于生产数据
	- 一类是消费者线程用于消费数据
- 为了解耦生产者和消费者的关系，通常会采用共享的数据区域，就像是一个仓库
	- 生产者生产数据之后直接放置在共享数据区中，并不需要关心消费者的行为
	- 消费者只需要从共享数据区中去获取数据，并不需要关心生产者的行为


**为了体现生产和消费过程中的等待和唤醒，Java就提供了几个方法供我们使用，这几个方法在Object类中，
Object类的等待和唤醒方法：**

- void wait​()	导致当前线程等待，直到另一个线程调用该对象的 notify()方法或 notifyAll()方法
- void notify​()	唤醒正在等待对象监视器的单个线程
- void notifyAll​()	唤醒正在等待对象监视器的所有线程 

## 生产者和消费者案例 ##
**生产者消费者案例中包含的类：**

- 奶箱类(Box)：定义一个成员变量，表示第x瓶奶，提供存储牛奶和获取牛奶的操作
- 生产者类(Producer)：实现Runnable接口，重写run()方法，调用存储牛奶的操作
- 消费者类(Customer)：实现Runnable接口，重写run()方法，调用获取牛奶的操作
- 测试类(BoxDemo)：里面有main方法，main方法中的代码步骤如下
	1. 创建奶箱对象，这是共享数据区域
	2. 创建生产者对象，把奶箱对象作为构造方法参数传递，因为在这个类中要调用存储牛奶的操作
	3. 创建消费者对象，把奶箱对象作为构造方法参数传递，因为在这个类中要调用获取牛奶的操作
	4. 创建2个线程对象，分别把生产者对象和消费者对象作为构造方法参数传递
	5. 启动线程

- 示例代码：
		
	**奶箱类**

		public class Box {
		    //定义一个成员变量，表示第x瓶奶
		    private int milk;
		    //定义一个成员变量，表示奶箱的状态
		    private boolean state = false; //默认表示没有牛奶
		
		    //提供存储牛奶和获取牛奶的操作
		    public synchronized void put(int milk) {
		        //如果有牛奶，等待消费
		        if(state) {
		            try {
		                wait();
		            } catch (InterruptedException e) {
		                e.printStackTrace();
		            }
		        }
		
		        //如果没有牛奶，就生产牛奶
		        this.milk = milk;
		        System.out.println("送奶工将第" + this.milk + "瓶奶放入奶箱");
		
		        //生产完毕之后，修改奶箱状态
		        state = true;
		
		        //唤醒其他等待的线程
		        notifyAll();
		    }
		
		    public synchronized void get() {
		        //如果没有牛奶，等待生产
		        if(!state) {
		            try {
		                wait();
		            } catch (InterruptedException e) {
		                e.printStackTrace();
		            }
		        }
		
		        //如果有牛奶，就消费牛奶
		        System.out.println("用户拿到第" + this.milk + "瓶奶");
		
		        //消费完毕之后，修改奶箱状态
		        state = false;
		
		        //唤醒其他等待的线程
		        notifyAll();
		    }
		}
	
	**生产者类**

		public class Producer implements Runnable {
		    private Box b;
		
		    public Producer(Box b) {
		        this.b = b;
		    }
		
		    @Override
		    public void run() {
		        for(int i=1; i<=30; i++) {
		            b.put(i);
		        }
		    }
		}

	**消费者类**
	
		public class Customer implements Runnable {
		    private Box b;
		
		    public Customer(Box b) {
		        this.b = b;
		    }
		
		    @Override
		    public void run() {
		        while (true) {
		            b.get();
		        }
		    }
		}

	**测试类**

		public class BoxDemo {
		    public static void main(String[] args) {
		        //创建奶箱对象，这是共享数据区域
		        Box b = new Box();
		
		        //创建生产者对象，把奶箱对象作为构造方法参数传递，因为在这个类中要调用存储牛奶的操作
		        Producer p = new Producer(b);
		        //创建消费者对象，把奶箱对象作为构造方法参数传递，因为在这个类中要调用获取牛奶的操作
		        Customer c = new Customer(b);
		
		        //创建2个线程对象，分别把生产者对象和消费者对象作为构造方法参数传递
		        Thread t1 = new Thread(p);
		        Thread t2 = new Thread(c);
		
		        //启动线程
		        t1.start();
		        t2.start();
		    }
		}

----------
