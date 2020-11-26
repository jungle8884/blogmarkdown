---
title: Event_publisher_subscriber
categories:
  - 计算机技术
tags:
  - C#
  - 委托与事件
date: 2020-09-21 15:17:32
author: Jungle

---
# 事件 #
**事件是一个特殊的委托实例**

注：

1. 事件和事件处理程序必须有共同的签名和返回类型，他们通过委托类型进行描述。
2. **事件是类或结构的成员**
3. 事件成员被隐式自动初始化为null


		-------关键字--委托类型--------事件名
		public event EventHandler CountedADozen; 
		EventHandler 专门用于系统事件;
		任何附加到事件的处理程序都必须与委托类型的签名和返回类型匹配;
		 namespace System
		 {
		     //
		     // 摘要:
		     //     表示将用于处理不具有事件数据的事件的方法。
		     //
		     // 参数:
		     //   sender: 用来保存触发事件对象的引用。
		     //     事件源。
		     //      
		     //   e: 用来保存状态信息。
		     //     不包含事件数据的对象。
		     [ComVisible(true)]
		     public delegate void EventHandler(object sender, EventArgs e);
		 }

----------
## 自定义委托 ##

		delegate void Handler(); //声明委托---①

----------

	    /// <summary>
	    /// 发布者
	    /// </summary>
	    class Incrementer
	    {
	
	        /// <summary>
	        /// 事件成员被隐式自动初始化为null
	        /// </summary>
	        public event Handler CountedADozen;  //创建事件并发布---②
	
	        //触发事件的代码---③
	        public void DoCount()
	        {
	            for (int i = 1; i < 100; i++)
	            {
	                if (i % 12 == 0 && CountedADozen != null)
	                    CountedADozen(); //每增加十二个计数-->触发事件一次
	            }
	        }

----------

		 	/// <summary>
		    /// 订阅者
		    /// </summary>
		    class Dozens
		    {
		        public int DozensCount { get; private set; }
		
		        public Dozens(Incrementer incrementer)
		        {
		            DozensCount = 0;
		            incrementer.CountedADozen += IncrementDozensCount; //订阅事件---④
		        }
		
		        //声明事件处理程序---⑤
		        void IncrementDozensCount()
		        {
		            DozensCount++; 
		        }
		    }

----------
**测试代码：**

			#region 测试事件
            {
                Incrementer incrementer = new Incrementer(); //发布者
                Dozens doZensCounter = new Dozens(incrementer); //订阅者, 在调用构造函数时，便已经订阅了 事件处理程序。

                incrementer.DoCount(); //设置触发器
                Console.WriteLine( "Numbers of dozens = {0}", doZensCounter.DozensCount);
            }
            #endregion

----------
**输出：**

			Numbers of dozens = 8

----------

## 使用系统委托 ##

		    /// <summary>
		    /// 发布者
		    /// </summary>
		    class Incrementer
		    {
		
		        /// <summary>
		        /// 事件成员被隐式自动初始化为null
		        /// </summary>
		        //public event Handler CountedADozen;  //创建事件并发布---①
		
		        /*
		         * public delegate void EventHandler(object sender, EventArgs e);
		         * EventArgs被设计成为不能传递任何数据：用于不需要传递数据的事件处理程序---通常会被忽略掉。
		         * 如果你希望传递数据，必须声明一个派生自EventArgs的类，使用合适的字段来保存需要传递的数据。
		         * object 和 EventArgs 总是基类，所以 EventHandler 就能提供一个对所有事件和事件处理器都通用的签名，
		         * 只允许两个参数，而不是各自都有不同的签名。
		         */
		        public event EventHandler CountedADozen;
		
		        //触发事件的代码---②
		        public void DoCount()
		        {
		            for (int i = 1; i < 100; i++)
		            {
		                if (i % 12 == 0 && CountedADozen != null)
		                    CountedADozen(this, null); //每增加十二个计数-->触发事件一次
		            }
		        }
		        
		    }

----------

		 	/// <summary>
		    /// 订阅者
		    /// </summary>
		    class Dozens
		    {
		        public int DozensCount { get; private set; }
		
		        public Dozens(Incrementer incrementer)
		        {
		            DozensCount = 0;
		            incrementer.CountedADozen += IncrementDozensCount; //订阅事件---③
		        }
		
		        //声明事件处理程序---④
		        void IncrementDozensCount(object sender, EventArgs e)
		        {
		            DozensCount++; 
		        }
		    }

----------
**测试代码：**

			#region 测试事件
            {
                Incrementer incrementer = new Incrementer(); //发布者
                Dozens doZensCounter = new Dozens(incrementer); //订阅者, 在调用构造函数时，便已经订阅了 事件处理程序。

                incrementer.DoCount(); //设置触发器
                Console.WriteLine( "Numbers of dozens = {0}", doZensCounter.DozensCount);
            }
            #endregion

**输出与上面一致**

----------

## 自定义类派生自EventArgs ##

		/// <summary>
	    /// 自定义类派生自EventArgs
	    /// </summary>
	    public class IncrementerEventArgs : EventArgs
	    {
	        public int IterationCount { get; set; } //存储一个整数
	    }

----------

		/// <summary>
	    /// 发布者
	    /// </summary>
	    class Incrementer
	    {
	
	        /// <summary>
	        /// 事件成员被隐式自动初始化为null
	        /// </summary>
	        //public event Handler CountedADozen;  //创建事件并发布---②
	
	        /*
	         * public delegate void EventHandler(object sender, EventArgs e);
	         * EventArgs被设计成为不能传递任何数据：用于不需要传递数据的事件处理程序---通常会被忽略掉。
	         * 如果你希望传递数据，必须声明一个派生自EventArgs的类，使用合适的字段来保存需要传递的数据。
	         * object 和 EventArgs 总是基类，所以 EventHandler 就能提供一个对所有事件和事件处理器都通用的签名，
	         * 只允许两个参数，而不是各自都有不同的签名。
	         */
	        public event EventHandler<IncrementerEventArgs> CountedADozen;
	
	        //触发事件的代码---③
	        public void DoCount()
	        {
	            IncrementerEventArgs args = new IncrementerEventArgs(); //使用自定义类的泛型委托。
	
	            for (int i = 1; i < 100; i++)
	            {
	                if (i % 12 == 0 && CountedADozen != null)
	                {
	                    args.IterationCount = i;
	                    CountedADozen(this, args); //在触发事件时传递参数
	                }
	            }
	        }
	        
	    }

----------

		/// <summary>
	    /// 订阅者
	    /// </summary>
	    class Dozens
	    {
	        public int DozensCount { get; private set; }
	
	        public Dozens(Incrementer incrementer)
	        {
	            DozensCount = 0;
	            incrementer.CountedADozen += IncrementDozensCount; //订阅事件---④
	        }
	
	        //声明事件处理程序---⑤
	        void IncrementDozensCount(object sender, IncrementerEventArgs e)
	        {
	            Console.WriteLine("Incremented at iteration: {0} in {1}", e.IterationCount, e.ToString());
	            DozensCount++; 
	        }
	    }

----------
**测试代码：**

			#region 测试事件
            {
                Incrementer incrementer = new Incrementer(); //发布者
                Dozens doZensCounter = new Dozens(incrementer); //订阅者, 在调用构造函数时，便已经订阅了 事件处理程序。

                incrementer.DoCount(); //设置触发器
                Console.WriteLine( "Numbers of dozens = {0}", doZensCounter.DozensCount);
            }
            #endregion

----------
**输出:**

	Incremented at iteration: 12 in Test.TestEvent.IncrementerEventArgs
	Incremented at iteration: 24 in Test.TestEvent.IncrementerEventArgs
	Incremented at iteration: 36 in Test.TestEvent.IncrementerEventArgs
	Incremented at iteration: 48 in Test.TestEvent.IncrementerEventArgs
	Incremented at iteration: 60 in Test.TestEvent.IncrementerEventArgs
	Incremented at iteration: 72 in Test.TestEvent.IncrementerEventArgs
	Incremented at iteration: 84 in Test.TestEvent.IncrementerEventArgs
	Incremented at iteration: 96 in Test.TestEvent.IncrementerEventArgs
	Numbers of dozens = 8

----------
