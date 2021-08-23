---
title: 单例模式
categories:
  - 后端
  - 设计模式
tags:
  - 设计模式
  - 单例模式
author:
  - Jungle
date: 2021-04-10 
---

# 单例模式 Singleton

> 单例模式非常常见，某个对象全局只需要一个实例时，就可以使用单例模式。

**它的优点也显而易见：**

- 它能够避免对象重复创建，节约空间并提升效率
- 避免由于操作不同实例导致的逻辑错误

**单例模式有两种实现方式：饿汉式和懒汉式。**



## 饿汉式

- 饿汉式：变量在声明时便初始化。

```java
public class Singleton {
  
    private static Singleton instance = new Singleton();

    private Singleton() {
    }

    public static Singleton getInstance() {
        return instance;
    }
}
```

可以看到，我们将构造方法定义为 private，这就保证了其他类无法实例化此类，必须通过 getInstance 方法才能获取到唯一的 instance 实例，非常直观。

但饿汉式有一个**弊端**，那就是即使这个单例不需要使用，它也会在**类加载之后立即创建出来**，占用一块内存，并增加类初始化时间。

就好比一个电工在修理灯泡时，先把所有工具拿出来，不管是不是所有的工具都用得上。就像一个饥不择食的饿汉，所以称之为饿汉式。



## 懒汉式

- 懒汉式：先声明一个空变量，需要用时才初始化。
- **一般的建议是：对于构建不复杂，加载完成后会立即使用的单例对象，推荐使用饿汉式**。
- 例如：

```java
public class Singleton {
  
    private static Singleton instance = null;
  
    private Singleton() {
    }
    
    public static Singleton getInstance(){
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

我们先声明了一个初始值为 null 的 instance 变量，当需要使用时判断此变量是否已被初始化，没有初始化的话才 new 一个实例出来。

就好比电工在修理灯泡时，开始比较偷懒，什么工具都不拿，当发现需要使用螺丝刀时，才把螺丝刀拿出来。当需要用钳子时，再把钳子拿出来。就像一个不到万不得已不会行动的懒汉，所以称之为懒汉式。

懒汉式解决了饿汉式的弊端，好处是按需加载，避免了内存浪费，减少了类初始化时间。

> 上述代码的懒汉式单例乍一看没什么问题，但其实它不是线程安全的。

如果有多个线程同一时间调用 getInstance 方法，instance 变量可能会被实例化多次。

为了保证线程安全，我们需要给判空过程加上锁：

```java
public class Singleton {

    private static Singleton instance = null;

    private Singleton() {
    }

    public static Singleton getInstance() {
        synchronized (Singleton.class) {
            if (instance == null) {
                instance = new Singleton();
            }
        }
        return instance;
    }
}
```

这样就能保证多个线程调用 getInstance 时，一次最多只有一个线程能够执行判空并 new 出实例的操作，所以 instance 只会实例化一次。但这样的写法仍然有问题，当多个线程调用 getInstance 时，每次都需要执行 synchronized 同步化方法，这样会严重影响程序的执行效率。

> 所以更好的做法是在同步化之前，再加上一层检查：

```java
public class Singleton {
    
    private static Singleton instance = null;

    private Singleton() {
    }

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

这样增加一种检查方式后，如果 instance 已经被实例化，则不会执行同步化操作，大大提升了程序效率。

> 上面这种写法也就是我们平时较常用的双检锁方式实现的线程安全的单例模式。

但这样的懒汉式单例仍然有一个问题，JVM 底层为了优化程序运行效率，可能会对我们的代码进行**指令重排序**，在一些特殊情况下会导致出现**空指针**，为了防止这个问题，更进一步的优化是**给 instance 变量加上 volatile 关键字**。

> instance = new Singleton(); //不是一个原子性操作
>
> 1. 分配内存空间
> 2. 执行构造方法, 初始化对象
> 3. instance 指向分配的内存空间的地址
>
> 指令重排: 123 -> 132 
>
> 线程A执行1和3时, 此时instance指向了分配的内存空间的地址, 不为null; 
>
> 线程B执行`getInstance()`发现instance不为null, 因此, 返回instance, 但此时instance还未初始化.;
>
> 即 获取到`不为 null，但还没有执行初始化`的 instance 对象，发生空指针异常。

有读者可能会有疑问，我们在外面检查了 instance == null, 那么锁里面的空检查是否可以去掉呢？

**有读者可能会有疑问，我们在外面检查了 instance == null, 那么锁里面的空检查是否可以去掉呢？**

答案是不可以。如果里面不做空检查，可能会有两个线程同时通过了外面的空检查，然后在一个线程 new 出实例后，第二个线程进入锁中又 new 出一个实例，导致创建多个实例。

> 除了双检锁方式外，还有一种比较常见的静态内部类方式保证懒汉式单例的线程安全：（常用）
>
> - **对于构建过程耗时较长，并不是所有使用此类都会用到的单例对象，推荐使用懒汉式。**

```java
public class Singleton {
    
    private static class SingletonHolder {
        public static Singleton instance = new Singleton();
    }

    private Singleton() {
    }

    public static Singleton getInstance() {
        return SingletonHolder.instance;
    }
}
```

> 虽然我们经常使用这种静态内部类的懒加载方式，但其中的原理不一定每个人都清楚。
>
> 接下来我们便来分析其原理，搞清楚两个问题：

- 静态内部类方式是怎么实现懒加载的
- 静态内部类方式是怎么保证线程安全的

> Java 类的加载过程包括：加载、验证、准备、解析、初始化。

- 初始化阶段即执行类的 clinit 方法（clinit = class + initialize），包括为类的静态变量赋初始值和执行静态代码块中的内容。
  - 但不会立即加载内部类，**内部类会在使用时才加载**。
  - 所以当此 Singleton 类加载时，SingletonHolder 并不会被立即加载，所以不会像饿汉式那样占用内存。

- 另外，Java 虚拟机规定，**当访问一个类的静态字段时，如果该类尚未初始化，则立即初始化此类**。
  - 当调用Singleton 的 getInstance 方法时，
  - 由于其使用了 SingletonHolder 的静态变量 instance，所以这时才会去初始化 SingletonHolder，
  - 在 SingletonHolder 中 new 出 Singleton 对象。这就实现了懒加载。

> 第二个问题的答案是 Java 虚拟机的设计是非常稳定的，早已经考虑到了多线程并发执行的情况。

- 虚拟机在加载类的 clinit 方法时，会保证 clinit 在多线程中被正确的加锁、同步。
- 即使有多个线程同时去初始化一个类，一次也只有一个线程可以执行 clinit 方法，其他线程都需要阻塞等待，从而保证了线程安全。
- [其他回答:](https://blog.csdn.net/panchang199266/article/details/103222344)
  - 当第一次访问类中的静态变量时，会触发类加载，并且**同一个类只加载一次**
  - 静态内部类也是如此，类加载过程由类加载器负责加锁，从而保证线程安全。此种单例模式更加简洁明了，不容易出错
- 拓展: 反射可以破坏单例! 怎么解决?
  - 可以看下载的视频, 找思路;
  - flag, 枚举...
  - 未完待续...

懒加载方式在平时非常常见，比如打开我们常用的美团、饿了么、支付宝 app，应用首页会立刻刷新出来，但其他标签页在我们点击到时才会刷新。这样就减少了流量消耗，并缩短了程序启动时间。

再比如游戏中的某些模块，当我们点击到时才会去下载资源，而不是事先将所有资源都先下载下来，这也属于懒加载方式，避免了内存浪费。

> 但懒汉式的**缺点**就是将程序加载时间从启动时延后到了运行时，虽然启动时间缩短了，但我们浏览页面时就会看到数据的 loading 过程。如果用饿汉式将页面提前加载好，我们浏览时就会特别的顺畅，也不失为一个好的用户体验。

比如我们常用的 QQ、微信 app，作为即时通讯的工具软件，它们会在启动时立即刷新所有的数据，保证用户看到最新最全的内容。

**著名的软件大师 Martin 在《代码整洁之道》一书中也说到：不提倡使用懒加载方式，因为程序应该将构建与使用分离，达到解耦。**

> 饿汉式在声明时直接初始化变量的方式也更直观易懂。
>
> 所以在使用饿汉式还是懒汉式时，需要权衡利弊。

**一般的建议是：对于构建不复杂，加载完成后会立即使用的单例对象，推荐使用饿汉式**。

**对于构建过程耗时较长，并不是所有使用此类都会用到的单例对象，推荐使用懒汉式。**

> 问：双检锁单例模式中，volatile 主要用来防止哪几条指令重排序？如果发生了重排序，会导致什么样的错误？

**答案：**

1、instance = new Singleton();

2、这一行代码中，执行了三条重要的指令：

- 分配对象的内存空间
- 初始化对象
- 将变量 instance 指向刚分配的内存空间

3、在这个过程中，如果第二条指令和第三条指令发生了重排序，可能导致 instance 还未初始化时，其他线程提前通过双检锁外层的 null 检查，获取到`不为 null，但还没有执行初始化`的 instance 对象，发生空指针异常。



------

链接：https://leetcode-cn.com/leetbook/read/design-patterns/99sx01/
来源：力扣（LeetCode）