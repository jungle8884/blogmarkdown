---
title: JobQuestions
categories:
  - 后端
  - 面经
tags:
  - 面试
author:
  - Jungle
date: 2021-08-16 15:26:09
---

# Java

## 基础

### == 和 equals

> 对于基本数据类型来说，==比较的是值。对于引用数据类型来说，==比较的是对象的内存地址

因为 Java 只有值传递，所以，对于 == 来说，不管是比较基本数据类型，还是引用数据类型的变量，其本质比较的都是值，只是引用类型变量存的值是对象的地址。

>  equals不重写比较的是引用

![image-20210915104435123](JobQuestions/image-20210915104435123.png)

> **为什么重写 `equals` 时必须重写 `hashCode` 方法？**

- hashCode: `hashCode()` 的作用是获取哈希码，也称为散列码.
  - 它实际上是返回一个 int 整数哈希码的作用是确定该对象在哈希表中的索引位置;
  - 另外需要注意的是： `Object` 的 hashcode 方法是本地方法，也就是用 c 语言或 c++ 实现的，该方法通常用来将对象的 内存地址 转换为整数之后返回。
- 散列表存储的是键值对(key-value)，它的特点是：能根据“键”快速的检索出对应的“值”。这其中就利用到了散列码！（可以快速找到所需要的对象）

**回答:**

- 如果两个对象相等，则 hashcode 一定也是相同的。

- 两个对象相等, 对两个对象分别调用 equals 方法都返回 true。
- 但是，**两个对象有相同的 hashcode 值，它们也不一定是相等的 。**
- **因此，equals 方法被覆盖过，则 `hashCode` 方法也必须被覆盖。**

> **为什么两个对象有相同的 hashcode 值，它们也不一定是相等的？**

- 因为 `hashCode()` 所使用的哈希算法也许刚好会让多个对象传回相同的哈希值。
- 越糟糕的哈希算法越容易碰撞，但这也与数据值域分布的特性有关（所谓碰撞也就是指的是不同的对象得到相同的 `hashCode`。

- `HashSet` 在对比的时候，同样的 hashcode 有多个对象，它会使用 `equals()` 来判断是否真的相同。也就是说 `hashcode` 只是用来缩小查找成本。

### 重载与重写

> 重载就是同样的一个方法能够根据输入数据的不同，做出不同的处理
>
> 重写就是当子类继承自父类的相同方法，输入数据一样，但要做出有别于父类的响应时，你就要覆盖父类方法

- 重载: 
  - 在同一个类里有2个或2个以上方法名相同，而方法签名不同的方法，
  - 方法签名就是**参数列表的不同**，包括**参数个数，顺序，类型**等，而**返回类型不包括在方法签名里**
  - 参数顺序不同不一定叫做重载, 关键是参数类型, 参数类型不同的情况下, 参数顺序不一样才叫重载
  - 重载就是**同一个类中多个同名方法根据不同的传参来执行不同的逻辑处理**
- 重写: 
  - 也就是覆盖的意思，是跟继承挂钩的，是指子类的方法与超类的方法看上去一样，即方法名，返回类型，参数列表都一样
  - **重写就是子类对父类方法的重新改造，外部样子不能改变，内部逻辑可以改变**

### 深拷贝和浅拷贝

![image-20210915124402189](JobQuestions/image-20210915124402189.png)

### [String StringBuffer 和 StringBuilder 的区别是什么? String 为什么是不可变的?](https://snailclimb.gitee.io/javaguide/#/docs/java/basis/Java基础知识?id=string-stringbuffer-和-stringbuilder-的区别是什么-string-为什么是不可变的)

> 可变性

![image-20210915124738706](JobQuestions/image-20210915124738706.png)

![image-20210915124757328](JobQuestions/image-20210915124757328.png)

> 线程安全性
>
> 线程安全。`AbstractStringBuilder` 是 `StringBuilder` 与 `StringBuffer` 的公共父类，定义了一些字符串的基本操作，如 `expandCapacity`、`append`、`insert`、`indexOf` 等公共方法。

-  `String` 中的对象是不可变的，也就可以理解为常量，线程安全。
- `StringBuffer` 对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。
- `StringBuilder` 并没有对方法进行加同步锁，所以是非线程安全的。

> 性能

- 每次对 `String` 类型进行改变的时候，都会生成一个新的 `String` 对象，然后将指针指向新的 `String` 对象。
- `StringBuffer` 每次都会对 `StringBuffer` 对象本身进行操作，而不是生成新的对象并改变对象引用。
- 相同情况下使用 `StringBuilder` 相比使用 `StringBuffer` 仅能获得 10%~15% 左右的性能提升，但却要冒多线程不安全的风险。

**使用总结:**

1. 操作少量的数据: 适用 `String`
2. 单线程操作字符串缓冲区下操作大量数据: 适用 `StringBuilder`
3. 多线程操作字符串缓冲区下操作大量数据: 适用 `StringBuffer`

### [Object 类的常见方法总结](https://snailclimb.gitee.io/javaguide/#/docs/java/basis/Java基础知识?id=object-类的常见方法总结)

> Object 类是一个特殊的类，是所有类的父类。它主要提供了以下 11 个方法：

```java
public final native Class<?> getClass()//native方法，用于返回当前运行时对象的Class对象，使用了final关键字修饰，故不允许子类重写。

public native int hashCode() //native方法，用于返回对象的哈希码，主要使用在哈希表中，比如JDK中的HashMap。
public boolean equals(Object obj)//用于比较2个对象的内存地址是否相等，String类对该方法进行了重写用户比较字符串的值是否相等。

protected native Object clone() throws CloneNotSupportedException//naitive方法，用于创建并返回当前对象的一份拷贝。一般情况下，对于任何对象 x，表达式 x.clone() != x 为true，x.clone().getClass() == x.getClass() 为true。Object本身没有实现Cloneable接口，所以不重写clone方法并且进行调用的话会发生CloneNotSupportedException异常。

public String toString()//返回类的名字@实例的哈希码的16进制的字符串。建议Object所有的子类都重写这个方法。

public final native void notify()//native方法，并且不能重写。唤醒一个在此对象监视器上等待的线程(监视器相当于就是锁的概念)。如果有多个线程在等待只会任意唤醒一个。

public final native void notifyAll()//native方法，并且不能重写。跟notify一样，唯一的区别就是会唤醒在此对象监视器上等待的所有线程，而不是一个线程。

public final native void wait(long timeout) throws InterruptedException//native方法，并且不能重写。暂停线程的执行。注意：sleep方法没有释放锁，而wait方法释放了锁 。timeout是等待时间。

public final void wait(long timeout, int nanos) throws InterruptedException//多了nanos参数，这个参数表示额外时间（以毫微秒为单位，范围是 0-999999）。 所以超时的时间还需要加上nanos毫秒。

public final void wait() throws InterruptedException//跟之前的2个wait方法一样，只不过该方法一直等待，没有超时时间这个概念

protected void finalize() throws Throwable { }//实例被垃圾回收器回收的时候触发的操作
```

### [异常](https://snailclimb.gitee.io/javaguide/#/docs/java/basis/Java基础知识?id=异常)

> Java异常类结构图

![结构图](JobQuestions/Java%E5%BC%82%E5%B8%B8%E7%B1%BB%E5%B1%82%E6%AC%A1%E7%BB%93%E6%9E%84%E5%9B%BE.png)

在 Java 中，所有的异常都有一个共同的祖先 `java.lang` 包中的 `Throwable` 类。`Throwable` 类有两个重要的子类 `Exception`（异常）和 `Error`（错误）。`Exception` 能被程序本身处理(`try-catch`)， `Error` 是无法处理的(只能尽量避免)。

`Exception` 和 `Error` 二者都是 Java 异常处理的重要子类，各自都包含大量子类。

- **`Exception`** :程序本身可以处理的异常，可以通过 `catch` 来进行捕获。`Exception` 又可以分为 受检查异常(必须处理) 和 不受检查异常(可以不处理)。
- **`Error`** ：`Error` 属于程序无法处理的错误 ，我们没办法通过 `catch` 来进行捕获 。例如，Java 虚拟机运行错误（`Virtual MachineError`）、虚拟机内存不够错误(`OutOfMemoryError`)、类定义错误（`NoClassDefFoundError`）等 。这些异常发生时，Java 虚拟机（JVM）一般会选择线程终止。



![image-20210915133645685](JobQuestions/image-20210915133645685.png)



---



## 集合

Java 集合， 也叫作容器，主要是由两大接口派生而来：一个是 `Collecton`接口，主要用于存放单一元素；另一个是 `Map` 接口，主要用于存放键值对。对于`Collection` 接口，下面又有三个主要的子接口：`List`、`Set` 和 `Queue`。

Java 集合框架如下图所示：

![Java 集合框架](JobQuestions/java-collection-hierarchy.png)

注：图中只列举了主要的继承派生关系，并没有列举所有关系。比方省略了`AbstractList`, `NavigableSet`等抽象类以及其他的一些辅助类，如想深入了解，可自行查看源码。



### LinkedList 和 ArrayList

![image-20210910141241224](JobQuestions/image-20210910141241224.png)

### [ArrayDeque 与 LinkedList 的区别](https://snailclimb.gitee.io/javaguide/#/docs/java/collection/Java集合框架常见面试题?id=_142-arraydeque-与-linkedlist-的区别)

`ArrayDeque` 和 `LinkedList` 都实现了 `Deque` 接口，两者都具有队列的功能，但两者有什么区别呢？

- `ArrayDeque` 是基于可变长的数组和双指针来实现，而 `LinkedList` 则通过链表来实现。
- `ArrayDeque` 不支持存储 `NULL` 数据，但 `LinkedList` 支持。
- `ArrayDeque` 是在 JDK1.6 才被引入的，而`LinkedList` 早在 JDK1.2 时就已经存在。
- `ArrayDeque` 插入时可能存在扩容过程, 不过均摊后的插入操作依然为 O(1)。虽然 `LinkedList` 不需要扩容，但是每次插入数据时均需要申请新的堆空间，均摊性能相比更慢。

从性能的角度上，选用 `ArrayDeque` 来实现队列要比 `LinkedList` 更好。此外，`ArrayDeque` 也可以用于实现栈。

### [说一说 PriorityQueue](https://snailclimb.gitee.io/javaguide/#/docs/java/collection/Java集合框架常见面试题?id=_143-说一说-priorityqueue)

`PriorityQueue` 是在 JDK1.5 中被引入的, 其与 `Queue` 的区别在于元素出队顺序是与优先级相关的，即总是优先级最高的元素先出队。

这里列举其相关的一些要点：

- `PriorityQueue` 利用了二叉堆的数据结构来实现的，底层使用可变长的数组来存储数据
- `PriorityQueue` 通过堆元素的上浮和下沉，实现了在 O(logn) 的时间复杂度内插入元素和删除堆顶元素。
- `PriorityQueue` 是非线程安全的，且不支持存储 `NULL` 和 `non-comparable` 的对象。
- `PriorityQueue` 默认是小顶堆，但可以接收一个 `Comparator` 作为构造参数，从而来自定义元素优先级的先后。

`PriorityQueue` 在面试中可能更多的会出现在手撕算法的时候，典型例题包括堆排序、求第K大的数、带权图的遍历等，所以需要会熟练使用才行。



### [HashMap和Hashtable的区别](https://snailclimb.gitee.io/javaguide/#/docs/java/collection/Java%E9%9B%86%E5%90%88%E6%A1%86%E6%9E%B6%E5%B8%B8%E8%A7%81%E9%9D%A2%E8%AF%95%E9%A2%98?id=_141-hashmap-%e5%92%8c-hashtable-%e7%9a%84%e5%8c%ba%e5%88%ab)

1. **线程是否安全：** `HashMap` 是非线程安全的，`HashTable` 是线程安全的,因为 `HashTable` 内部的方法基本都经过`synchronized` 修饰。（如果你要保证线程安全的话就使用 `ConcurrentHashMap` 吧！）；
2. **效率：** 因为线程安全的问题，`HashMap` 要比 `HashTable` 效率高一点。另外，`HashTable` 基本被淘汰，不要在代码中使用它；
3. **对 Null key 和 Null value 的支持：** `HashMap` 可以存储 null 的 key 和 value，但 null 作为键只能有一个，null 作为值可以有多个；HashTable 不允许有 null 键和 null 值，否则会抛出 `NullPointerException`。
4. **初始容量大小和每次扩充容量大小的不同 ：** ① 创建时如果不指定容量初始值，`Hashtable` 默认的初始大小为 **11**，之后每次扩充，容量变为原来的 **2n+1**。`HashMap` 默认的初始化大小为 **16**，之后每次扩充，容量变为原来的 2 倍。② 创建时如果给定了容量初始值，那么 **Hashtable 会直接使用你给定的大小**，而 **`HashMap` 会将其扩充为 2 的幂次方大小**（`HashMap` 中的`tableSizeFor()`方法保证，下面给出了源代码）。也就是说 `HashMap` 总是使用 2 的幂作为哈希表的大小,后面会介绍到为什么是 2 的幂次方。
5. **底层数据结构：** JDK1.8 以后的 `HashMap` 在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为 8）（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树）时，将链表转化为红黑树，以减少搜索时间。Hashtable 没有这样的机制。[**即链表长度大于8且数组长度大于64才转换成红黑树**]

> tableSizeFor

```java
/**
 * Returns a power of two size for the given target capacity.
 */
static final int tableSizeFor(int cap) {
    int n = cap - 1;
    n |= n >>> 1;
    n |= n >>> 2;
    n |= n >>> 4;
    n |= n >>> 8;
    n |= n >>> 16;
    return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
}
```



### [HashMap 1.8 怎么实现? 和1.7之前有什么区别? ](https://snailclimb.gitee.io/javaguide/#/docs/java/collection/Java%E9%9B%86%E5%90%88%E6%A1%86%E6%9E%B6%E5%B8%B8%E8%A7%81%E9%9D%A2%E8%AF%95%E9%A2%98?id=_145-hashmap-%e7%9a%84%e5%ba%95%e5%b1%82%e5%ae%9e%e7%8e%b0)

JDK1.8 之前 `HashMap` 底层是 **数组+链表**。

**HashMap 通过 key 的 hashCode 经过扰动函数(`(h = key.hashCode()) ^ (h >>> 16)`)处理过后得到 hash 值，然后通过 (n - 1) & hash 判断当前元素存放的位置（这里的 n 指的是数组的长度），如果当前位置存在元素的话，就判断该元素与要存入的元素的 hash 值以及 key 是否相同，如果相同的话，直接覆盖，不相同就通过拉链法解决冲突**

JDK1.8的`HashMap`底层是**数组+链表/红黑树**

HashMap在解决哈希冲突有了较大的变化，**当链表长度大于8且数组的长度大于等于64时，将链表转化为红黑树，以减少搜索时间**(链表转换为红黑树前会判断，如果当前数组的长度小于64，那么会选择先进行数组扩容，而不是转换为红黑树)

![image-20210915145022793](JobQuestions/image-20210915145022793.png)

### [HashMap 的长度为什么是 2 的幂次方](https://snailclimb.gitee.io/javaguide/#/docs/java/collection/Java集合框架常见面试题?id=_156-hashmap-的长度为什么是-2-的幂次方)

为了能让 HashMap 存取高效，尽量较少碰撞，也就是要尽量把数据分配均匀。我们上面也讲到了过了，Hash 值的范围值-2147483648 到 2147483647，前后加起来大概 40 亿的映射空间，只要哈希函数映射得比较均匀松散，一般应用是很难出现碰撞的。但问题是一个 40 亿长度的数组，内存是放不下的。所以这个散列值是不能直接拿来用的。用之前还要先做对数组的长度取模运算，得到的余数才能用来要存放的位置也就是对应的数组下标。

> 这个数组下标的计算方法是“ `(n - 1) & hash`”。（n 代表数组长度）。这也就解释了 HashMap 的长度为什么是 2 的幂次方。

**这个算法应该如何设计呢？**

我们首先可能会想到采用%取余的操作来实现。但是，重点来了：**“取余(%)操作中如果除数是 2 的幂次则等价于与其除数减一的与(&)操作（也就是说 hash%length==hash&(length-1)的前提是 length 是 2 的 n 次方；）。”** 

> 并且 **采用二进制位操作 &，相对于%能够提高运算效率，这就解释了 HashMap 的长度为什么是 2 的幂次方。**



### [多线程下操作HashMap,有哪些方式可以保证线程安全？](https://www.cnblogs.com/supiaopiao/p/9341625.html)

> 为什么线程不安全:
>
> - 首先如果`多个线程同时使用put方法添加元素`，而且假设正好存在两个put的key发生了碰撞(hash值一样)，那么根据HashMap的实现，这两个key会添加到数组的同一个位置，这样最终就会发生其中一个线程的put的数据被覆盖。
> - 第二就是如果`多个线程同时检测到元素个数超过 数组大小*loadFactor`，这样就会发生多个线程同时对Node数组进行扩容，都在重新计算元素位置以及复制数据，但是最终只有一个线程扩容后的数组会赋给table，也就是说其他线程的都会丢失，并且各自线程put的数据也丢失。

如何线程安全的使用HashMap:

- Hashtable

  > 当一个线程访问HashTable的同步方法时，其他线程如果也要访问同步方法，会被阻塞住。举个例子，当一个线程使用put方法时，另一个线程不但不可以使用put方法，连get方法都不可以，好霸道啊！！！so~~，效率很低，现在基本不会选择它了。

- ConcurrentHashMap

  > 采用 `synchronized` 和 CAS 来保证并发安全**, `synchronized`只锁定当前链表或红黑二叉树的**首节点**, 只要hash不冲突, 就不会产生并发, 效率提升N倍.

- Synchronized Map

  > 调用synchronizedMap()方法后会返回一个SynchronizedMap类的对象，而在SynchronizedMap类中使用了synchronized同步关键字来保证对Map的操作是线程安全的。

```java
//Hashtable
Map<String, String> hashtable = new Hashtable<>();
  
//synchronizedMap
Map<String, String> synchronizedHashMap = Collections.synchronizedMap(new HashMap<String, String>());
  
//ConcurrentHashMap
Map<String, String> concurrentHashMap = new ConcurrentHashMap<>();
```



---

### 介绍一下concurrentHashMap

> ConcurrentHashMap实现原理

- 底层数据结构: jdk1.7 之前底层采用**分段数组+链表**实现, jdk1.8之后采用的数据结构则是**数组+链表/红黑二叉树**实现.

- 实现线程安全的方式: 

  - jdk1.7 : 分段锁, 将数据分割分段, 每一段数据分配一把锁, 当一个线程占用了其中一段数据时, 不影响其他数据段被其他线程的访问.

  <img src="JobQuestions/image-20210817153941602.png" alt="image-20210817153941602" style="zoom:80%;" />

  - jdk1.8:  **JDK1.8 的时候已经，而取消了分段锁, 采用 `synchronized` 和 CAS 来保证并发安全**, `synchronized`只锁定当前链表或红黑二叉树的**首节点**, 只要hash不冲突, 就不会产生并发, 效率提升N倍.

    > [CAS](https://www.jianshu.com/p/34a10da2c0d5): compare and swap, 比较并变换.
    >
    > CAS(V, E, N): 若V值等于E值，则将V的值设为N; 若V值和E值不同, 则说明已经有其他线程做了更新, 则当前线程什么都不做.

  <img src="JobQuestions/image-20210817154032680.png" alt="image-20210817154032680" style="zoom:80%;" />



---



## JVM

### Java运行时数据区域, 堆是线程公有还是私有, 堆中有线程私有的区域么?

<img src="JobQuestions/image-20210817195602415.png" alt="image-20210817195602415" style="zoom: 80%;" />

> jdk 1.8之前: **程序计数器, 虚拟机栈**(在 HotSpot 虚拟机中本地方法栈和 Java 虚拟机栈合二为一), **堆**, **方法区(运行时常量池)**

<img src="JobQuestions/image-20210817194609186.png" alt="image-20210817194609186" style="zoom:80%;" />

> jdk 1.8: **程序计数器, 虚拟机栈**(在 HotSpot 虚拟机中本地方法栈和 Java 虚拟机栈合二为一), **堆(字符串常量池)**, **元空间(运行时常量池)**

<img src="JobQuestions/image-20210817194626708.png" alt="image-20210817194626708" style="zoom:80%;" />

堆中有线程私有的区域吗? (应该是**没有**的)

**线程私有的：**

- 程序计数器
- 虚拟机栈(在 HotSpot 虚拟机中本地方法栈和  虚拟机栈 合二为一)

**线程共享的：**

- **堆**: **所有线程共享的一块内存区域, 几乎所有的对象实例及数组都在这里分配内存.**

- **方法区 (静态变量, 常量, 类信息, 运行时常量池)**: **类的元数据**.

  

### JVM的内存模型中, 哪部分是线程私有的, 为什么?

主要是: **程序计数器** 和 **虚拟机栈**(在 HotSpot 虚拟机中 **本地方法栈** 和  **虚拟机栈** 合二为一), 原因如下:

- 程序计数器: 为了线程切换后能恢复到正确的执行位置, 每条线程都需要有一个独立的程序计数器, 各线程之间互不影响, 独立存储.
- 虚拟机栈: **每次方法调用的数据都是通过栈传递的**, 虚拟机栈由一个个栈帧组成, 每个栈帧包括: **局部变量表**, 操作数栈, 动态链接 和 方法出口信息. (**一个方法的执行就是开启了一个线程**)

**局部变量表主要存放了编译期可知的各种数据类型**（boolean、byte、char、short、int、float、long、double）、**对象引用**（reference 类型，它不同于对象本身，可能是一个指向对象起始地址的引用指针，也可能是指向一个代表对象的句柄或其他与此对象相关的位置）。

### 类加载机制: 加载, 验证, 准备, 解析, 初始化.

- 加载:

  - 获取定义此类的二进制字节流
  - 将字节流所代表的静态存储结构**转化为方法区的运行时数据结构**
  - **在内存中生成一个代表该类的Class对象**

- 验证: 

  - 文件格式验证 (Magic Number: **确定这个文件是否为一个能被虚拟机接收的 Class 文件**)

  - 元数据验证: 对字节码描述的信息进行语义分析

  - 字节码验证: **确定程序语义是合法的, 符合逻辑的**

  - [符号引用验证](https://snailclimb.gitee.io/javaguide/#/docs/java/jvm/%E7%B1%BB%E6%96%87%E4%BB%B6%E7%BB%93%E6%9E%84?id=_23-%e5%b8%b8%e9%87%8f%e6%b1%a0%ef%bc%88constant-pool%ef%bc%89): **确保解析动作能正确执行**

    <img src="JobQuestions/image-20210817205618092.png" alt="运行时常量池" style="zoom:80%;" />

- 准备: **正式为类变量分配内存并设置类变量初始值的阶段**，这些内存都将在方法区中分配.
  - 这里所设置的初始值"通常情况"下是数据类型默认的零值（如 0、0L、null、false 等），
  - 比如我们定义了`public static int value=111` ，那么 value 变量在**准备阶段**的初始值就是 0 而不是 111（初始化阶段才会赋值）。
  - 特殊情况：比如给 value 变量**加上了 final 关键字**`public static final int value=111` ，那么准备阶段 value 的值就被赋值为 111。

- 解析: 虚拟机**将常量池内的符号引用替换为直接引用**的过程。

  - 解析动作主要针对类或接口、
  - 字段、类方法、接口方法、方法类型、方法句柄和调用限定符 7 类符号引用进行。

  <img src="JobQuestions/image-20210817210852474.png" alt="image-20210817210852474" style="zoom:80%;" />

- 初始化: 初始化阶段是执行初始化方法 `<clinit> ()`方法的过程，是类加载的最后一步，这一步 JVM 才开始真正执行类中定义的 Java 程序代码(字节码)。

  <img src="JobQuestions/image-20210817211051125.png" alt="image-20210817211051125" style="zoom:80%;" />

- 解释: 

  - 什么时候初始化
    - new 即 实例化对象的时候
    - getstatic / setstatic 读取或设置静态字段时(static final静态常量除外)
    - invokestatic 调用类静态方法的时候
    - 使用反射Class.forName()进行反射调用的时候
  - 初始化顺序
    - 1 父类的静态变量和静态块赋值 (按声明顺序)
    - 2 自身的静态变量和静态块赋值 (按声明顺序)
    - 3 父类的成员变量和块赋值 (按声明顺序)
    - 4 父类构造器赋值
    - 5 自身的成员变量和块赋值 (按声明顺序)
    - 6 自身构造器赋值

### [Java对象的创建过程](https://snailclimb.gitee.io/javaguide/#/docs/java/jvm/Java%E5%86%85%E5%AD%98%E5%8C%BA%E5%9F%9F?id=_31-%e5%af%b9%e8%b1%a1%e7%9a%84%e5%88%9b%e5%bb%ba)

![image-20210824193529315](JobQuestions/image-20210824193529315.png)

[Step1:类加载检查](https://snailclimb.gitee.io/javaguide/#/docs/java/jvm/Java内存区域?id=step1类加载检查)

虚拟机遇到一条 new 指令时，首先将去检查这个指令的参数是否能在常量池中定位到这个类的符号引用，并且检查这个符号引用代表的类是否已被加载过、解析和初始化过。如果没有，那必须先执行相应的类加载过程。

[Step2:分配内存](https://snailclimb.gitee.io/javaguide/#/docs/java/jvm/Java内存区域?id=step2分配内存)

在**类加载检查**通过后，接下来虚拟机将为新生对象**分配内存**。对象所需的内存大小在类加载完成后便可确定，为对象分配空间的任务等同于把一块确定大小的内存从 Java 堆中划分出来。**分配方式**有 **“指针碰撞”** 和 **“空闲列表”** 两种，**选择哪种分配方式由 Java 堆是否规整决定，而 Java 堆是否规整又由所采用的垃圾收集器是否带有压缩整理功能决定**。

<img src="JobQuestions/image-20210824194122655.png" alt="image-20210824194122655"  />

![image-20210824194914963](JobQuestions/image-20210824194914963.png)[Step3:初始化零值](https://snailclimb.gitee.io/javaguide/#/docs/java/jvm/Java内存区域?id=step3初始化零值)

内存分配完成后，虚拟机需要将分配到的内存空间都初始化为零值（不包括对象头），这一步操作保证了对象的实例字段在 Java 代码中可以不赋初始值就直接使用，程序能访问到这些字段的数据类型所对应的零值。

 [Step4:设置对象头](https://snailclimb.gitee.io/javaguide/#/docs/java/jvm/Java内存区域?id=step4设置对象头)

初始化零值完成之后，**虚拟机要对对象进行必要的设置**，例如这个对象是哪个类的实例、如何才能找到类的元数据信息、对象的哈希码、对象的 GC 分代年龄等信息。 **这些信息存放在对象头中。** 另外，根据虚拟机当前运行状态的不同，如是否启用偏向锁等，对象头会有不同的设置方式。

[Step5:执行 init 方法](https://snailclimb.gitee.io/javaguide/#/docs/java/jvm/Java内存区域?id=step5执行-init-方法)

在上面工作都完成之后，从虚拟机的视角来看，一个新的对象已经产生了，但从 Java 程序的视角来看，对象创建才刚开始，`<init>` 方法还没有执行，所有的字段都还为零。所以一般来说，执行 new 指令之后会接着执行 `<init>` 方法，把对象按照程序员的意愿进行初始化，这样一个真正可用的对象才算完全产生出来。

## GC

### 聊一聊垃圾回收(总结性质的问题)

> [GC历史](https://blog.csdn.net/xiaojin21cen/article/details/106686506)
>
> ![image-20210818161316459](JobQuestions/image-20210818161316459.png)

1. **先说有哪些垃圾收集算法**

垃圾收集算法主要有: 标记-复制(新生代), 标记-整理(老年代), 标记-清除(老年代) 和 分代收集算法.

分代收集算法: 根据对象存活周期的不同将内存分为几块。一般将 java 堆分为新生代和老年代，这样我们就可以根据各个年代的特点选择合适的垃圾收集算法。**比如在新生代中，每次收集都会有大量对象死去，所以可以选择”标记-复制“算法，只需要付出少量对象的复制成本就可以完成每次垃圾收集。而老年代的对象存活几率是比较高的，而且没有额外的空间对它进行分配担保，所以我们必须选择“标记-清除”或“标记-整理”算法进行垃圾收集。**

- jdk1.7 默认垃圾收集器Parallel Scavenge（新生代）+Parallel Old（老年代）
- jdk1.8 默认垃圾收集器Parallel Scavenge（新生代）+Parallel Old（老年代）
- jdk1.9 默认垃圾收集器G1

2. **这些垃圾收集算法的应用**

新生代GC: 

- Serial [标记-复制算法]
- ParNew [标记-复制算法]
- Parallel Scavenge [标记-复制算法] : 关注吞吐量

老年代GC: 

- Serial Old [标记-整理算法]
- ParNew Old [标记-整理算法]
- CMS [标记-清除算法] : 关注响应时间(垃圾收集时用户的停顿时间)

G1---整堆垃圾收集器: 整体上看是[标记-整理算法], 局部上看是[标记-复制算法].

3. **垃圾收集器的适用场景**

[Serial和Serial Old], Serial收集器是所有收集器中额外内存消耗最小的, 所以**适用于内存资源受限的环境**, 如单核CPU或CPU核心数较少的环境(运行在客户端模式下的虚拟机来说是一个很好的选择).

[CMS可以和Serial, ParNew收集器配合工作], CMS**适用于互联网网站或者基于浏览器的B/S系统的服务端上**, 关注服务的响应速度, 希望系统停顿时间尽可能短, 以给用户带来良好的交互体验.

[Parallel Scavenge 和 Serial Old] , Parallel Scavenge 目标是达到一个可控制的吞吐量, 高吞吐量则可以高效率地利用处理器资源, 尽快完成程序的运算任务, **适合在后台运算而不需要太多交互的分析任务**.

G1适用于服务器端.

### 垃圾回收器: CMS, G1

- **CMS（Concurrent Mark Sweep）收集器是一种以获取最短回收停顿时间为目标的收集器**

  - **它非常符合在注重用户体验的应用上使用**

  - **是 HotSpot 虚拟机第一款真正意义上的并发收集器，它第一次实现了让垃圾收集线程与用户线程（基本上）同时工作**

  - 从名字中的**Mark Sweep**这两个词可以看出，CMS 收集器是一种 **“标记-清除”算法**实现的，它的运作过程相比于前面几种垃圾收集器来说更加复杂一些。整个过程分为四个步骤：

    ![image-20210818110722824](JobQuestions/image-20210818110722824.png)

    - **初始标记：** 暂停所有的其他线程，并**记录下直接与 GC Roots 相连的对象**，速度很快 ;
    - **并发标记：** **从GC Roots的直接关联对象开始遍历整个对象图的过程,** 这个过程耗时较长但是不需要停顿用户线程, 可以与垃圾收集线程一起并发运行;
    - **重新标记：** 为了**修正并发标记期间因为用户程序继续运行而导致标记产生变动的那一部分对象的标记记录**，这个阶段的停顿时间一般会比初始标记阶段的时间稍长，远远比并发标记阶段的时间短;
    - **并发清除：** 清理删除掉标记阶段判断的已经死亡的对象, 由于不需要移动存活对象, 所以这个阶段也是可以与用户线程同时并发的.

  - 从它的名字就可以看出它是一款优秀的垃圾收集器，主要优点：**并发收集、低停顿**。但是它有下面三个明显的缺点：

    - **对 CPU 资源敏感；**
    - **无法处理浮动垃圾；**
    - **它使用的回收算法-“标记-清除”算法会导致收集结束时会有大量空间碎片产生。**

- **G1 (Garbage-First) 是一款面向服务器的垃圾收集器**,主要针对配备多颗处理器及大容量内存的机器. 以极高概率满足 GC 停顿时间要求的同时,还具备高吞吐量性能特征. **(所谓吞吐量就是 CPU 中用于运行用户代码的时间与 CPU 总消耗时间的比值)**
  - 被视为 JDK1.7 中 HotSpot 虚拟机的一个重要进化特征。它具备以下特点：
    - **并行与并发**：G1 能充分利用 CPU、多核环境下的硬件优势，使用多个 CPU（CPU 或者 CPU 核心）来缩短 Stop-The-World 停顿时间。部分其他收集器原本需要停顿 Java 线程执行的 GC 动作，G1 收集器仍然可以通过并发的方式让 java 程序继续执行。
    - **分代收集**：虽然 G1 可以不需要其他收集器配合就能独立管理整个 GC 堆，但是还是保留了分代的概念。
    - **空间整合**：与 CMS 的“标记-清理”算法不同，G1 从整体来看是基于“标记-整理”算法实现的收集器；从局部上来看是基于“标记-复制”算法实现的。
    - **可预测的停顿**：这是 G1 相对于 CMS 的另一个大优势，降低停顿时间是 G1 和 CMS 共同的关注点，但 G1 除了追求低停顿外，还能建立可预测的停顿时间模型，能让使用者明确指定在一个长度为 M 毫秒的时间片段内。
  - G1 收集器的运作大致分为以下几个步骤：
    - **初始标记**: **仅仅只是标记一下GC Roots能直接关联到的对象**, 并且修改TAMS(Top at Mark Start)指针的值, 让下一阶段用户线程并发运行时, 能正确地在可用的Region中分配新对象. 这个阶段**需要停顿线程**, 但耗时很短, 而且是借用进行Minor GC的时候同步完成的, 所以G1收集器在这个阶段**实际并没有额外的停顿**.
    - **并发标记**: **从GC Roots开始对堆中对象进行可达性分析, 递归扫描整个堆里面的对象图, 找出要回收的对象**, 这段时间耗时较长, 但可与用户程序并发执行. 当对象图扫描完成以后, 还要重新处理**STAB(原始快照)记录**下的在并发时有引用的对象.
    - **最终标记**: **对用户线程做另一个短暂的暂停, 用于处理并发阶段结束后仍遗留下来的最后那少量的STAB记录**.
    - **筛选回收**: 负责更新Region的统计数据, 对各个Region的回收价值和成本进行排序, 根据用户期望的停顿时间来制定回收计划, 可以自由选择任意多个Region构成回收集, 然后**把决定回收的那一部分Region的存活对象赋值到空的Region中, 再清理掉整个旧Region的全部空间**. 这里的操作涉及存活对象的移动, 是**必须暂停用户线程**, 由多条收集器线程并行完成的. 
  - **G1 收集器在后台维护了一个优先列表，每次根据允许的收集时间，优先选择回收价值最大的 Region(这也就是它的名字 Garbage-First 的由来)** 。这种使用 Region 划分内存空间以及有优先级的区域回收方式，保证了 G1 收集器在有限时间内可以尽可能高的收集效率（把内存化整为零）。
  - G1收集器**除了并发标记以外, 其余阶段也是要完全暂停用户线程的**, 它的目标是在延迟可控的情况下获得尽可能高的吞吐量.

**并行和并发概念补充：**

- **并行（Parallel）** ：指多条垃圾收集线程并行工作，但此时用户线程仍然处于等待状态。
- **并发（Concurrent）**：指用户线程与垃圾收集线程同时执行（但不一定是并行，可能会交替执行），用户程序在继续运行，而垃圾收集器运行在另一个 CPU 上。



### 几大引用

> **强引用（StrongReference）**: 绝对不会回收

以前我们使用的大部分引用实际上都是强引用，这是使用最普遍的引用。如果一个对象具有强引用，那就类似于**必不可少的生活用品**，垃圾回收器绝不会回收它。当内存空间不足，Java 虚拟机宁愿抛出 OutOfMemoryError 错误，使程序异常终止，也不会靠随意回收具有强引用的对象来解决内存不足问题。



> **软引用（SoftReference）**: 如果内存不足就回收

如果一个对象只具有软引用，那就类似于**可有可无的生活用品**。如果内存空间足够，垃圾回收器就不会回收它，如果内存空间不足了，就会回收这些对象的内存。只要垃圾回收器没有回收它，该对象就可以被程序使用。软引用可用来实现内存敏感的高速缓存。

软引用可以和一个引用队列（ReferenceQueue）联合使用，如果软引用所引用的对象被垃圾回收，JAVA 虚拟机就会把这个软引用加入到与之关联的引用队列中。



> **弱引用（WeakReference）**: 一旦发现就回收

如果一个对象只具有弱引用，那就类似于**可有可无的生活用品**。弱引用与软引用的区别在于：只具有弱引用的对象拥有更短暂的生命周期。在垃圾回收器线程扫描它所管辖的内存区域的过程中，一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存。不过，由于垃圾回收器是一个优先级很低的线程， 因此不一定会很快发现那些只具有弱引用的对象。

弱引用可以和一个引用队列（ReferenceQueue）联合使用，如果弱引用所引用的对象被垃圾回收，Java 虚拟机就会把这个弱引用加入到与之关联的引用队列中。



> **虚引用（PhantomReference）**: 任何时候都可能被回收

"虚引用"顾名思义，就是形同虚设，与其他几种引用都不同，虚引用并不会决定对象的生命周期。如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收。

**虚引用主要用来跟踪对象被垃圾回收的活动**。



> **虚引用与软引用和弱引用的一个区别在于：** 虚引用必须和引用队列（ReferenceQueue）联合使用。

- 当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在回收对象的内存之前，把这个虚引用加入到与之关联的引用队列中。

- 程序可以通过判断引用队列中是否已经加入了虚引用，来了解被引用的对象是否将要被垃圾回收。程序如果发现某个虚引用已经被加入到引用队列，那么就可以在所引用的对象的内存被回收之前采取必要的行动。

**特别注意，在程序设计中一般很少使用弱引用与虚引用，使用软引用的情况较多，这是因为软引用可以加速 JVM 对垃圾内存的回收速度，可以维护系统的运行安全，防止内存溢出（OutOfMemory）等问题的产生。**



---

## 多线程

### [ReentrantLock 和 synchronized的区别](https://snailclimb.gitee.io/javaguide/#/docs/java/multi-thread/2020%E6%9C%80%E6%96%B0Java%E5%B9%B6%E5%8F%91%E8%BF%9B%E9%98%B6%E5%B8%B8%E8%A7%81%E9%9D%A2%E8%AF%95%E9%A2%98%E6%80%BB%E7%BB%93?id=_15-%e8%b0%88%e8%b0%88-synchronized-%e5%92%8c-reentrantlock-%e7%9a%84%e5%8c%ba%e5%88%ab)

-  [两者都是可重入锁](https://snailclimb.gitee.io/javaguide/#/docs/java/multi-thread/2020最新Java并发进阶常见面试题总结?id=_151-两者都是可重入锁)

  ![image-20210824132729621](JobQuestions/image-20210824132729621.png)

-  [synchronized 依赖于 JVM 而 ReentrantLock 依赖于 API](https://snailclimb.gitee.io/javaguide/#/docs/java/multi-thread/2020最新Java并发进阶常见面试题总结?id=_152synchronized-依赖于-jvm-而-reentrantlock-依赖于-api)

  ![image-20210824132743278](JobQuestions/image-20210824132743278.png)

- [ReentrantLock 比 synchronized 增加了一些高级功能](https://snailclimb.gitee.io/javaguide/#/docs/java/multi-thread/2020最新Java并发进阶常见面试题总结?id=_153reentrantlock-比-synchronized-增加了一些高级功能)

  ![image-20210824132754173](JobQuestions/image-20210824132754173.png)

![image-20210824132810560](JobQuestions/image-20210824132810560.png)

> 各种锁:
>
> - 公平锁 (先来后到) 和 非公平锁(可以插队)
> - [可重入锁](https://snailclimb.gitee.io/javaguide/#/docs/java/multi-thread/2020%E6%9C%80%E6%96%B0Java%E5%B9%B6%E5%8F%91%E8%BF%9B%E9%98%B6%E5%B8%B8%E8%A7%81%E9%9D%A2%E8%AF%95%E9%A2%98%E6%80%BB%E7%BB%93?id=_151-%e4%b8%a4%e8%80%85%e9%83%bd%e6%98%af%e5%8f%af%e9%87%8d%e5%85%a5%e9%94%81) (递归锁: 拿到外面的锁就可以拿到里面的锁, 自动获得; **一定要成对出现!**)
> - 自旋锁
> - 死锁

**ReentrantLock 默认是非公平锁, 并且可以自己设置** (同时, 本身是**可重入锁**)

```java
public ReentrantLock() {
    sync = new NonfairSync();
}

public ReentrantLock(boolean fair) {
    sync = fair ? new FairSync() : new NonfairSync();
}
```

**自旋锁** (自定义解释)

<img src="JobQuestions/image-20210824130341818.png" alt="image-20210824130341818" style="zoom:80%;" />

> **测试**

<img src="JobQuestions/image-20210824130542827.png" alt="image-20210824130542827"  />

![image-20210824130647404](JobQuestions/image-20210824130647404.png)

> **输出:**

![image-20210824130720101](JobQuestions/image-20210824130720101.png)

[**死锁**](https://snailclimb.gitee.io/javaguide/#/docs/java/multi-thread/2020%E6%9C%80%E6%96%B0Java%E5%B9%B6%E5%8F%91%E5%9F%BA%E7%A1%80%E5%B8%B8%E8%A7%81%E9%9D%A2%E8%AF%95%E9%A2%98%E6%80%BB%E7%BB%93?id=_81-%e8%ae%a4%e8%af%86%e7%ba%bf%e7%a8%8b%e6%ad%bb%e9%94%81)

![image-20210824131214522](JobQuestions/image-20210824131214522.png)

> 怎么排除死锁?
>
> **看堆栈信息**

jdk bin 目录下有一些工具:

- jps: 定位进程号: `jps -l`
- jstack: `jstack 进程号` 找到死锁问题

**产生死锁必须具备以下四个条件：**

1. 互斥条件：该资源任意一个时刻只由一个线程占用。
2. 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放。
3. 不剥夺条件:线程已获得的资源在未使用完之前不能被其他线程强行剥夺，只有自己使用完毕后才释放资源。
4. 循环等待条件:若干进程之间形成一种头尾相接的循环等待资源关系。

**如何预防死锁？** 破坏死锁的产生的必要条件即可：

1. **破坏请求与保持条件** ：一次性申请所有的资源。
2. **破坏不剥夺条件** ：占用部分资源的线程进一步申请其他资源时，如果申请不到，可以主动释放它占有的资源。
3. **破坏循环等待条件** ：靠按序申请资源来预防。按某一顺序申请资源，释放资源则反序释放。破坏循环等待条件。

[避免死锁就是在资源分配时，借助于算法（比如银行家算法）对资源分配进行计算评估，使其进入安全状态。](https://snailclimb.gitee.io/javaguide/#/docs/java/multi-thread/2020%E6%9C%80%E6%96%B0Java%E5%B9%B6%E5%8F%91%E5%9F%BA%E7%A1%80%E5%B8%B8%E8%A7%81%E9%9D%A2%E8%AF%95%E9%A2%98%E6%80%BB%E7%BB%93?id=_82-%e5%a6%82%e4%bd%95%e9%a2%84%e9%98%b2%e5%92%8c%e9%81%bf%e5%85%8d%e7%ba%bf%e7%a8%8b%e6%ad%bb%e9%94%81)

**安全状态**指的是系统能够按照某种进行推进顺序（P1、P2、P3.....Pn）来为每个进程分配所需资源，直到满足每个进程对资源的最大需求，使每个进程都可顺利完成。称<P1、P2、P3.....Pn>序列为安全序列。

---

### AQS和lock的比较









---

### CAS原理，ABA问题如何解决

1. [CAS](https://blog.csdn.net/u011506543/article/details/82392338)

**CAS: Compare And Set (比较并交换: 如果expect值和原来的值(内存地址中的值)相同, 则更新为update值)**

> 从`package java.util.concurrent.atomic`下的`AtomicInteger`入手:

```java
public final boolean compareAndSet(int expect, int update) {
    return unsafe.compareAndSwapInt(this, valueOffset, expect, update);
}

public final int getAndIncrement() {
    return unsafe.getAndAddInt(this, valueOffset, 1);
}
```

> 到`package sun.misc`包下的 `public final class Unsafe`
>
> ![image-20210824100740271](JobQuestions/image-20210824100740271.png)
>
> ![image-20210824100836152](JobQuestions/image-20210824100836152.png)

```java
public final native boolean compareAndSwapInt(Object var1, long var2, int var4, int var5);

// AtomicInteger的 compareAndSet 的原理 同 getAndIncrement:
public final int getAndAddInt(Object var1, long var2, int var4) {
    int var5;
    do { // 如果不相同, 就一直自旋; 否则, 就加1返回 (此时var4等于1)
        var5 = this.getIntVolatile(var1, var2); // 从内存地址中取值
    } while(!this.compareAndSwapInt(var1, var2, var5, var5 + var4)); // 此为自旋锁:var5 变为 var5+var4即加1

    return var5;
}
```

> 缺点
>
> - 循环会耗时
> - 一次只能保证一个共享变量的原子性
> - ABA问题



2. [ABA](https://www.jianshu.com/p/72d02353dc7e)

**ABA问题: 狸猫换太子.** 

> 在CAS算法中，需要取出内存中某时刻的数据（由用户完成），在下一时刻比较并替换（由CPU完成，该操作是原子的）。
>
> 这个时间差中，会导致数据的变化。

<img src="JobQuestions/image-20210824103133687.png" alt="image-20210824103133687" style="zoom:80%;" />

**假设如下事件序列：**

1. 线程 1 从内存位置V中取出A。
2. 线程 2 从位置V中取出A。
3. 线程 2 进行了一些操作，将B写入位置V。
4. 线程 2 将A再次写入位置V。
5. 线程 1 进行CAS操作，发现位置V中仍然是A，操作成功。

**尽管线程 1 的CAS操作成功，但不代表这个过程没有问题——*对于线程 1 ，线程 2 的修改已经丢失*。**

> 怎么解决?
>
> 加入版本号即可解决
>
> 例如: AtomicStampedReference (**原子引用**)

![AtomicStampedReference](JobQuestions/image-20210824115658307.png)

> - [遇到的坑: ](https://kangroo.gitee.io/ajcg/#/oop)
>
>   ![image-20210824123249369](JobQuestions/image-20210824123249369.png)
>
> - 解决: 将值改为 -128~127

![image-20210824121807902](JobQuestions/image-20210824121807902.png)

**输出成功:**

> // 与乐观锁的原理一样.
>
> ![image-20210824123056417](JobQuestions/image-20210824123056417.png)
>
> ![image-20210824122836814](JobQuestions/image-20210824122836814.png)



---

### 线程的状态, 状态转移条件, 创建方式, 和进程的区别

 



---

### 并发编程的三个重要特性

1. **原子性** : 一个的操作或者多次操作，要么所有的操作全部都得到执行并且不会收到任何因素的干扰而中断，**要么所有的操作都执行，要么都不执行**。`synchronized` 可以保证代码片段的原子性。
2. **可见性** ：当一个变量对共享变量进行了修改，那么另外的线程都是立即可以看到修改后的最新值。`volatile` 关键字可以**保证共享变量的可见性**。
3. **有序性** ：代码在执行的过程中的先后顺序，Java 在编译器以及运行期间的优化，代码的执行顺序未必就是编写代码时候的顺序。`volatile` 关键字可以**禁止指令进行重排序优化**。

### volatile的作用是什么?

**Volatile 是 Java虚拟机提供的轻量级同步机制**

> Volatile 是可以保证可见性的, 但是不能保证原子性, 由于内存屏障, 可以保证避免指令重排的现象产生. 
>
>  (具体应用见设计模式问题)

1. **保证可见性**  (把变量声明为 **`volatile`** ，这就指示 JVM，这个变量是共享且不稳定的，每次使用它都到主存中进行读取)

2. [不保证原子性](https://blog.csdn.net/xdzhouxin/article/details/81236356) (若要保证原子性, 可以使用Atomic类)

3. **禁止JVM的指令重排** (你写的程序, 计算机不一定时按照你写的那样去执行的)

![image-20210823142913281](JobQuestions/image-20210823142913281.png)

### 介绍一下synchronized的实现

> 相关博文:[深入理解synchronized底层原理，一篇文章就够了！](https://cloud.tencent.com/developer/article/1465413)

synchronized有两种形式上锁，一个是对方法上锁，一个是构造同步代码块。

他们的底层实现其实都一样，在进入同步代码之前先获取锁，获取到锁之后锁的计数器+1，同步代码执行完锁的计数器-1，如果获取失败就阻塞式等待锁的释放。只是他们在同步块识别方式上有所不一样，从class字节码文件可以表现出来，一个是通过方法flags标志，一个是monitorenter和monitorexit指令操作。

[底层实现原理:](https://snailclimb.gitee.io/javaguide/#/docs/java/multi-thread/2020%E6%9C%80%E6%96%B0Java%E5%B9%B6%E5%8F%91%E8%BF%9B%E9%98%B6%E5%B8%B8%E8%A7%81%E9%9D%A2%E8%AF%95%E9%A2%98%E6%80%BB%E7%BB%93?id=_13-%e8%ae%b2%e4%b8%80%e4%b8%8b-synchronized-%e5%85%b3%e9%94%ae%e5%ad%97%e7%9a%84%e5%ba%95%e5%b1%82%e5%8e%9f%e7%90%86)

1. **`synchronized` 同步语句块的实现使用的是 `monitorenter` 和 `monitorexit` 指令，其中 `monitorenter` 指令指向同步代码块的开始位置，`monitorexit` 指令则指明同步代码块的结束位置。**

   ![image-20210824191409898](JobQuestions/image-20210824191409898.png)

   ![image-20210824191604555](JobQuestions/image-20210824191604555.png)

>  当执行 `monitorenter` 指令时，线程试图获取锁也就是获取 **对象监视器 `monitor`** 的持有权。

![image-20210824190941954](JobQuestions/image-20210824190941954.png)

2. **`synchronized` 修饰的方法**并没有 `monitorenter` 指令和 `monitorexit` 指令，取得代之的确实是 **`ACC_SYNCHRONIZED` 标识**，该标识指明了该方法是一个同步方法。JVM 通过该 `ACC_SYNCHRONIZED` 访问标志来辨别一个方法是否声明为同步方法，从而执行相应的同步调用。

   ![image-20210824191259069](JobQuestions/image-20210824191259069.png)

3. 小结:

   ![image-20210824191126784](JobQuestions/image-20210824191126784.png)

**synchronized特性:** 

> - 原子性: **指一个操作或者多个操作，要么全部执行并且执行的过程不会被任何因素打断，要么就都不执行。**
>
>   ![image-20210824170616948](JobQuestions/image-20210824170616948.png)
>
> - 可见性: **指多个线程访问一个资源时，该资源的状态、值信息等对于其他线程都是可见的。**
>
>   ![image-20210824170453935](JobQuestions/image-20210824170453935.png)
>
> - 有序性: **指程序执行的顺序按照代码先后执行。**
>
>   ![image-20210824170433879](JobQuestions/image-20210824170433879.png)
>
> - 可重入性: **指的是自己可以再次获取自己的内部锁**
>
>   ![image-20210824170519928](JobQuestions/image-20210824170519928.png)

**`synchronized`关键字可以保证被它修饰的方法或者代码块在任意时刻只能有一个线程执行。**



### 说说 synchronized 关键字和 volatile 关键字的区别

`synchronized` 关键字和 `volatile` 关键字是两个互补的存在，而不是对立的存在！

- **`volatile` 关键字**是线程同步的**轻量级实现**，所以 **`volatile `性能肯定比`synchronized`关键字要好** 。但是 **`volatile` 关键字只能用于变量而 `synchronized` 关键字可以修饰方法以及代码块** 。
- **`volatile` 关键字能保证数据的可见性，但不能保证数据的原子性。`synchronized` 关键字两者都能保证。**
- **`volatile`关键字主要用于解决变量在多个线程之间的可见性，而 `synchronized` 关键字解决的是多个线程之间访问资源的同步性。**

### JMM

> [Java 内存模型](https://snailclimb.gitee.io/javaguide/#/docs/java/multi-thread/2020%E6%9C%80%E6%96%B0Java%E5%B9%B6%E5%8F%91%E8%BF%9B%E9%98%B6%E5%B8%B8%E8%A7%81%E9%9D%A2%E8%AF%95%E9%A2%98%E6%80%BB%E7%BB%93?id=_22-%e8%ae%b2%e4%b8%80%e4%b8%8b-jmmjava-%e5%86%85%e5%ad%98%e6%a8%a1%e5%9e%8b)
>
> [java内存模型JMM理解整理](https://www.cnblogs.com/null-qige/p/9481900.html)

1. 详细理解:

![image-20210823134251344](JobQuestions/image-20210823134251344.png)

> **内存交互操作有8种，虚拟机实现必须保证每一个操作都是原子的，不可在分的（对于double和long类型的变量来说，load、store、read和write操作在某些平台上允许例外）**

- - lock   （锁定）：作用于主内存的变量，把一个变量标识为线程独占状态
  - unlock （解锁）：作用于主内存的变量，它把一个处于锁定状态的变量释放出来，释放后的变量才可以被其他线程锁定
  - read  （读取）：作用于主内存变量，它把一个变量的值从主内存传输到线程的工作内存中，以便随后的load动作使用
  - load   （载入）：作用于工作内存的变量，它把read操作从主存中变量放入工作内存中
  - use   （使用）：作用于工作内存中的变量，它把工作内存中的变量传输给执行引擎，每当虚拟机遇到一个需要使用到变量的值，就会使用到这个指令
  - assign （赋值）：作用于工作内存中的变量，它把一个从执行引擎中接受到的值放入工作内存的变量副本中
  - store  （存储）：作用于主内存中的变量，它把一个从工作内存中一个变量的值传送到主内存中，以便后续的write使用
  - write 　（写入）：作用于主内存中的变量，它把store操作从工作内存中得到的变量的值放入主内存的变量中

　　JMM对这八种指令的使用，制定了如下规则：

- - 不允许read和load、store和write操作之一单独出现。即使用了read必须load，使用了store必须write
  - 不允许线程丢弃他最近的assign操作，即工作变量的数据改变了之后，必须告知主存
  - 不允许一个线程将没有assign的数据从工作内存同步回主内存
  - 一个新的变量必须在主内存中诞生，不允许工作内存直接使用一个未被初始化的变量。就是对变量实施use、store操作之前，必须经过assign和load操作
  - 一个变量同一时间只有一个线程能对其进行lock。多次lock后，必须执行相同次数的unlock才能解锁
  - 如果对一个变量进行lock操作，会清空所有工作内存中此变量的值，在执行引擎使用这个变量前，必须重新load或assign操作初始化变量的值
  - 如果一个变量没有被lock，就不能对其进行unlock操作。也不能unlock一个被其他线程锁住的变量
  - 对一个变量进行unlock操作之前，必须把此变量同步回主内存

> JMM对这八种操作规则和对[volatile的一些特殊规则](https://www.cnblogs.com/null-qige/p/8569131.html)就能确定哪里操作是线程安全，哪些操作是线程不安全的了。但是这些规则实在复杂，很难在实践中直接分析。所以一般我们也不会通过上述规则进行分析。
>
> **更多的时候，使用java的happen-before规则来进行分析。**

2. [面试回答:](https://snailclimb.gitee.io/javaguide/#/docs/java/multi-thread/2020%E6%9C%80%E6%96%B0Java%E5%B9%B6%E5%8F%91%E8%BF%9B%E9%98%B6%E5%B8%B8%E8%A7%81%E9%9D%A2%E8%AF%95%E9%A2%98%E6%80%BB%E7%BB%93?id=_22-%e8%ae%b2%e4%b8%80%e4%b8%8b-jmmjava-%e5%86%85%e5%ad%98%e6%a8%a1%e5%9e%8b)

![image-20210823134423346](JobQuestions/image-20210823134423346.png)

![image-20210823134446710](JobQuestions/image-20210823134446710.png)



---

### 线程池参数有哪些, 具体应该怎么配置

> [三大方法, 七大参数 和 四大策略](https://blog.csdn.net/kongsanjin/article/details/111218612)
>
> ![image-20210824210706134](JobQuestions/image-20210824210706134.png)
>
> [阿里巴巴开发手册](https://kangroo.gitee.io/ajcg/#/concurrent)
>
> [OOM](https://snailclimb.gitee.io/javaguide/#/docs/java/jvm/Java%E5%86%85%E5%AD%98%E5%8C%BA%E5%9F%9F?id=_22-java-%e8%99%9a%e6%8b%9f%e6%9c%ba%e6%a0%88)
>
> 面试问题:
>
> - 线程池的七大参数，及其工作过程
>
> - 线程池基本参数, 一个线程被提交到线程池的流程

**七大参数:**

- **`corePoolSize` :** 核心线程数线程数定义了**最小可以同时运行的线程数量**。
- **`maximumPoolSize` :** 当队列中存放的任务达到队列容量的时候，当前**可以同时运行的线程数量变为最大线程数**。
- **`keepAliveTime`**:当线程池中的线程数量大于 `corePoolSize` 的时候，如果这时没有新的任务提交，核心线程外的线程不会立即销毁，而是会等待，直到等待的时间超过了 `keepAliveTime`才会被回收销毁；
- **`unit`** : `keepAliveTime` 参数的时间单位。
- **`workQueue`:** 当新任务来的时候会先判断当前运行的线程数量是否达到核心线程数，如果达到的话，新任务就会被存放在队列中。
- **`threadFactory`** :executor 创建新线程的时候会用到。
- **`handler`** :拒绝策略。关于饱和策略下面单独介绍一下。

**线程池四大拒绝策略**

- **`ThreadPoolExecutor` 饱和策略定义:** 如果**当前同时运行的线程数量达到最大线程数量并且队列也已经被放满了**时，`ThreadPoolTaskExecutor` 定义一些策略:

- **`ThreadPoolExecutor.AbortPolicy`：** 直接抛出 RejectExecutionException 异常阻止系统正常运行
- **`ThreadPoolExecutor.CallerRunsPolicy`：** **“调用者运行” 一种调节机制，该策略既不会抛弃任务，也不会抛出异常，而是将某些任务回退到调用者，从而降低新任务的流量**
- **`ThreadPoolExecutor.DiscardPolicy`：** 该策略默默地丢弃无法处理的任务，不予任何处理也不抛出异常。如果允许任务丢失，这是最好的一种策略
- **`ThreadPoolExecutor.DiscardOldestPolicy`：** 抛弃队列中等待最后的任务，然后把当前任务加入队列中尝试再次提交当前任务

**怎么配置:**

> maximumPoolSize(线程池的最大线程数)的设置:
>
> - CPU密集型: (获取CPU的核数)Runtime.getRuntime().availableProcessors();
> - IO密集型: > 程序中十分耗IO的线程数, 一般为2倍CPU的核数
>
> ![image-20210824230916476](JobQuestions/image-20210824230916476.png)

**使用线程池的好处**

- **降低资源消耗**。通过重复利用已创建的线程降低线程创建和销毁造成的消耗。
- **提高响应速度**。当任务到达时，任务可以不需要的等到线程创建就能立即执行。
- **提高线程的可管理性**。线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一的分配，调优和监控。



[**一个线程被提交到线程池的流程:**](https://www.cnblogs.com/geekdc/p/11158438.html)

> **提交一个任务到线程池中，线程池的处理流程如下：**

1、判断线程池里的核心线程是否都在执行任务，如果不是（核心线程空闲或者还有核心线程没有被创建）则创建一个新的工作线程来执行任务。如果核心线程都在执行任务，则进入下个流程。

2、线程池判断工作队列是否已满，如果工作队列没有满，则将新提交的任务存储在这个工作队列里。如果工作队列满了，则进入下个流程。

3、判断线程池里的线程是否都处于工作状态，如果没有，则创建一个新的工作线程来执行任务。如果已经满了，则交给饱和策略来处理这个任务。



### 经典线程池有哪些？每个的底层原理是什么？

**数据库连接池, Http连接池**

> 可能会有答案的链接:
>
> - 博客: https://blog.csdn.net/Woo_home/article/details/104875575
> - 视频: https://www.bilibili.com/video/BV1We411x7nk?p=23

ThreadPoolExecutor源码分析:

```java
// 存放工作线程的集合
private final HashSet<Worker> workers = new HashSet<Worker>();

// 代表了一个工作的线程
private final class Worker
        extends AbstractQueuedSynchronizer
        implements Runnable 
{
    	final Thread thread; // 具体工作的线程
        
        Runnable firstTask; // 第一次要执行的任务
    
        // 构造器
    	Worker(Runnable firstTask) {
            setState(-1); // inhibit interrupts until runWorker
            this.firstTask = firstTask;
            //*** workers.thread 指向下一个worker类型的线程 (类似于链表的next)
            this.thread = getThreadFactory().newThread(this);
        }
    
    	public void run() {
            runWorker(this);
        }
    
} //Worker结束   


// 下面继续看ThreadPoolExecutor的源码:
		// w = 上面的 this
    	final void runWorker(Worker w) {
        Thread wt = Thread.currentThread();
        Runnable task = w.firstTask; // 第一个任务
        w.firstTask = null;
        w.unlock(); // allow interrupts
        boolean completedAbruptly = true;
        try {
            // 3 再次进来, task = getTask()---假如可以获取到任务就不会停止!
            // 这就是线程复用的原理!
            while (task != null || (task = getTask()) != null) {
                w.lock();
                // If pool is stopping, ensure thread is interrupted;
                // if not, ensure thread is not interrupted.  This
                // requires a recheck in second case to deal with
                // shutdownNow race while clearing interrupt
                if ((runStateAtLeast(ctl.get(), STOP) ||
                     (Thread.interrupted() &&
                      runStateAtLeast(ctl.get(), STOP))) &&
                    !wt.isInterrupted())
                    wt.interrupt();
                try {
                    beforeExecute(wt, task);
                    Throwable thrown = null;
                    try {
                        task.run(); // 1 传递进来的任务被执行
                    } catch (RuntimeException x) {
                        thrown = x; throw x;
                    } catch (Error x) {
                        thrown = x; throw x;
                    } catch (Throwable x) {
                        thrown = x; throw new Error(x);
                    } finally {
                        afterExecute(task, thrown);
                    }
                } finally {
                    task = null; // 2 设置为 null
                    w.completedTasks++;
                    w.unlock();
                }
            }
            completedAbruptly = false;
        } finally {
            processWorkerExit(w, completedAbruptly);
        }
    }

	// 4 获取哪里的任务? (我猜是workQueue队列)
	private Runnable getTask() {
        boolean timedOut = false; // Did the last poll() time out?

        for (;;) {
            int c = ctl.get();
            int rs = runStateOf(c);

            // Check if queue empty only if necessary.
            if (rs >= SHUTDOWN && (rs >= STOP || workQueue.isEmpty())) {
                decrementWorkerCount();
                return null;
            }

            int wc = workerCountOf(c);

            // Are workers subject to culling?
            boolean timed = allowCoreThreadTimeOut || wc > corePoolSize;

            if ((wc > maximumPoolSize || (timed && timedOut))
                && (wc > 1 || workQueue.isEmpty())) {
                if (compareAndDecrementWorkerCount(c))
                    return null;
                continue;
            }

            try {
                Runnable r = timed ?
                    // 4.1 临时线程使用上面的方法获取 (超时就为null)
                    workQueue.poll(keepAliveTime, TimeUnit.NANOSECONDS) :
                	// 4.2 核心线程使用下边的方法获取 (取不到就阻塞)
                    workQueue.take();
                if (r != null)
                    return r;
                timedOut = true;
            } catch (InterruptedException retry) {
                timedOut = false;
            }
        }
    }

	// 5. 什么时候创建Worker和执行呢?
	public void execute(Runnable command) {
        if (command == null)
            throw new NullPointerException();
        
        // c 代表线程池的工作状态
        int c = ctl.get();
        // 5.1 如果小于核心线程数
        if (workerCountOf(c) < corePoolSize) {
            //true: 添加到核心线程
            if (addWorker(command, true)) 
                return;
            c = ctl.get();
        }
        // 5.2 往workQueue添加一个worker(offer成功返回true; 否则返回false)
        if (isRunning(c) && workQueue.offer(command)) {
            int recheck = ctl.get();
            if (! isRunning(recheck) && remove(command))
                reject(command);
            else if (workerCountOf(recheck) == 0)
                addWorker(null, false);
        }
        // 5.3 --- false: 添加到临时线程
        else if (!addWorker(command, false)) //添加成功就不执行下面
            reject(command); //5.4 若添加失败, 执行拒绝任务策略
    }
	
	// 6 最后看看addWorker
	private boolean addWorker(Runnable firstTask, boolean core) {
        retry:
        for (;;) {
            int c = ctl.get();
            int rs = runStateOf(c);

            // Check if queue empty only if necessary.
            if (rs >= SHUTDOWN &&
                ! (rs == SHUTDOWN &&
                   firstTask == null &&
                   ! workQueue.isEmpty()))
                return false;
			
            // 6.1 判断
            for (;;) {
                int wc = workerCountOf(c); // 取出当前工作线程数量
                if (wc >= CAPACITY ||
                    wc >= (core ? corePoolSize : maximumPoolSize))
                    // core 为 true: wc 大于核心线程 返回 false
                    // core 为 false: wc 大于最大线程 返回 false
                    return false;
                if (compareAndIncrementWorkerCount(c))
                    break retry;
                c = ctl.get();  // Re-read ctl
                if (runStateOf(c) != rs)
                    continue retry;
                // else CAS failed due to workerCount change; retry inner loop
            }
        }

        boolean workerStarted = false;
        boolean workerAdded = false;
        Worker w = null;
        try {
            // 6.2 通过上面的判断后, 创建worker
            w = new Worker(firstTask);
            //*** t 指向构造器里的"next" 线程
            final Thread t = w.thread; 
            if (t != null) {
                final ReentrantLock mainLock = this.mainLock;
                mainLock.lock();
                try {
                    // Recheck while holding lock.
                    // Back out on ThreadFactory failure or if
                    // shut down before lock acquired.
                    int rs = runStateOf(ctl.get());

                    if (rs < SHUTDOWN ||
                        (rs == SHUTDOWN && firstTask == null)) {
                        if (t.isAlive()) // precheck that t is startable
                            throw new IllegalThreadStateException();
                        // HashSet<Worker> workers --- 见最上面
                        workers.add(w); // 6.3 添加到全局变量工作线程集合中
                        int s = workers.size();
                        if (s > largestPoolSize)
                            largestPoolSize = s;
                        workerAdded = true; // 设置为true
                    }
                } finally {
                    mainLock.unlock();
                }
                if (workerAdded) {
                    t.start(); // 6.4 最终启动线程
                    workerStarted = true;
                }
            }
        } finally {
            if (! workerStarted)
                addWorkerFailed(w);
        }
        return workerStarted;
    }
// 
```

![image-20210825010940401](JobQuestions/image-20210825010940401.png)



---



## Spring

### IOC

>**IoC（Inverse of Control:控制反转）** 是一种设计思想，而不是一个具体的技术实现。IoC 的思想就是将原本在程序中手动创建对象的控制权，交由 Spring 框架来管理。不过， IoC 并非 Spirng 特有，在其他语言中也有应用。
>
>**为什么叫控制反转？**
>
>- **控制** ：指的是对象创建（实例化、管理）的权力
>- **反转** ：控制权交给外部环境（Spring 框架、IoC 容器)

![image-20210909123545664](JobQuestions/image-20210909123545664.png)

将对象之间的相互依赖关系交给 IoC 容器来管理，并由 IoC 容器完成对象的注入。这样可以很大程度上简化应用的开发，把应用从复杂的依赖关系中解放出来。 IoC 容器就像是一个工厂一样，**当我们需要创建一个对象的时候，只需要配置好配置文件/注解即可**，完全不用考虑对象是如何被创建出来的。



[**DI:  依赖注入**](https://mp.weixin.qq.com/s?__biz=Mzg2NTAzMTExNg%3D%3D&chksm=ce61046ef9168d78c02be9a0edcc44f1f6ea17fb16e04b733052385a700f381b0e6bb243d47b&idx=1&mid=2247484109&scene=21&sn=a3bc263536e84c93b9eb862cfa4da319#wechat_redirect)

- 依赖: Bean 对象的创建依赖于容器
- 注入: Bean 对象中的所有属性, 由容器来注入



**怎么回答:**

> - IOC是spring的两大核心概念之一，IOC给我们提供了一个**IOC bean容器**，这个容器**会帮我们自动去创建对象**，不需要我们手动创建;
>
> - IOC实现创建的通过 **DI（Dependency Injection 依赖注入）**，我们可以通过写Java**注解**代码或者是XML配置方式，把我们想要注入对象所依赖的一些其他的bean，自动的注入进去，
>
> - 它是通过byName或byType类型的方式来帮助我们注入。
> - 正是因为有了依赖注入，使得IOC有这非常强大的好处，解耦。

**举例:** (还未理解...)

> - 可以举个例子，JdbcTemplate  或者 SqlSessionFactory 这种bean，
> - 如果我们要把他注入到容器里面，他是需要依赖一个数据源的，
> - 如果我们把JdbcTemplate 或者 Druid 的数据源强耦合在一起，会导致一个问题，
>   - 当我们想要使用jdbctemplate必须要使用Druid数据源，那么依赖注入能够帮助我们在Jdbc注入的时候，只需要让他依赖一个DataSource接口，不需要去依赖具体的实现，
>   - 这样的好处就是，将来我们给容器里面注入一个Druid数据源，他就会自动注入到JdbcTemplate如果我们注入一个其他的也是一样的。
>   - 比如说c3p0也是一样的，这样的话，JdbcTemplate和数据源完全的解耦了，不强依赖与任何一个数据源，在spring启动的时候，就会把所有的bean全部创建好，
>   - 这样的话，程序在运行的时候就不需要创建bean了，运行速度会更快，
>   - 还有IOC管理bean的时候默认是单例的，可以节省时间，提高性能，





---

### AOP

> AOP(Aspect-Oriented Programming:面向切面编程)  能够将那些与业务无关，却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可拓展性和可维护性。

Spring AOP 就是基于动态代理的，如果要代理的对象，实现了某个接口，那么 Spring AOP 会使用 **JDK Proxy**，去创建代理对象，而对于没有实现接口的对象，就无法使用 JDK Proxy 去进行代理了，这时候 Spring AOP 会使用 **Cglib** 生成一个被代理对象的子类来作为代理，如下图所示：

![image-20210909202545237](JobQuestions/image-20210909202545237.png)



---

### Bean

> 简单来说，bean 代指的就是那些被 IoC 容器所管理的对象。

我们需要告诉 IoC 容器帮助我们管理哪些对象，这个是通过配置元数据来定义的。

配置元数据可以是 XML 文件、注解或者 Java 配置类。

![image-20210909203444290](JobQuestions/image-20210909203444290.png)

![image-20210909203627725](JobQuestions/image-20210909203627725.png)

**如何配置 bean 的作用域呢？**

xml 方式：

```xml
<bean id="..." class="..." scope="singleton"></bean>
```

注解方式：

```java
@Bean
@Scope(value = ConfigurableBeanFactory.SCOPE_PROTOTYPE)
public Person personPrototype() {
    return new Person();
}
```

![image-20210909203831215](JobQuestions/image-20210909203831215.png)

![image-20210909204332740](JobQuestions/image-20210909204332740.png)

![image-20210909204404538](JobQuestions/image-20210909204404538.png)



---

### [Spring MVC](https://www.cnblogs.com/cielosun/articles/5752272.html)

![image-20210909204627103](JobQuestions/image-20210909204627103.png)

发展史：

![image-20210909205418945](JobQuestions/image-20210909205418945.png)

![image-20210909205442884](JobQuestions/image-20210909205442884.png)

![image-20210909205616207](JobQuestions/image-20210909205616207.png)

![image-20210909205748345](JobQuestions/image-20210909205748345.png)

> 访问页面时 controller, model 和 view 的过程 



---

### [Spring 框架中用到了哪些设计模式？](https://snailclimb.gitee.io/javaguide/#/docs/system-design/framework/spring/Spring常见问题总结?id=spring-框架中用到了哪些设计模式？)

![image-20210909205905196](JobQuestions/image-20210909205905196.png)

链接： [面试官:“谈谈Spring中都用到了那些设计模式?”](https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==&mid=2247485303&idx=1&sn=9e4626a1e3f001f9b0d84a6fa0cff04a&chksm=cea248bcf9d5c1aaf48b67cc52bac74eb29d6037848d6cf213b0e5466f2d1fda970db700ba41&token=255050878&lang=zh_CN#rd)



---

### [Spring 事务](https://snailclimb.gitee.io/javaguide/#/docs/system-design/framework/spring/Spring常见问题总结?id=spring-事务)

[Spring/SpringBoot 模块下专门有一篇是讲 Spring 事务的，总结的非常详细，通俗易懂。](https://snailclimb.gitee.io/javaguide/#/docs/system-design/framework/spring/Spring%E4%BA%8B%E5%8A%A1%E6%80%BB%E7%BB%93?id=_3-%e8%af%a6%e8%b0%88-spring-%e5%af%b9%e4%ba%8b%e5%8a%a1%e7%9a%84%e6%94%af%e6%8c%81)

![image-20210909210345478](JobQuestions/image-20210909210345478.png)



---



# 计算机网络

## [输出一个url发生了什么](https://www.cnblogs.com/liutianzeng/p/10456865.html)

​	1、浏览器的地址栏输入URL并按下回车。

　　2、浏览器查找当前URL是否存在缓存，并比较缓存是否过期。

　　3、DNS解析URL对应的IP。

　　4、根据IP建立TCP连接（三次握手）。

　　5、HTTP发起请求。

　　6、服务器处理请求，浏览器接收HTTP响应。

　　7、渲染页面，构建DOM树。

　　8、关闭TCP连接（四次挥手）。



## Tcp三次握手，四次挥手，过程及原因

<img src="JobQuestions/image-20210910163238173.png" alt="image-20210910163238173" style="zoom:67%;" />

> 三次握手

- 客户端–发送带有 SYN 标志的数据包–一次握手–服务端
- 服务端–发送带有 SYN/ACK 标志的数据包–二次握手–客户端
- 客户端–发送带有带有 ACK 标志的数据包–三次握手–服务端

原因:**三次握手的目的是建立可靠的通信信道，说到通讯，简单来说就是数据的发送与接收，而三次握手最主要的目的就是双方确认自己与对方的发送与接收是正常的。**

第一次握手：Client 什么都不能确认；**Server 确认了对方发送正常，自己接收正常**

第二次握手：**Client 确认了：自己发送、接收正常，对方发送、接收正常；**Server 确认了：对方发送正常，自己接收正常

第三次握手：Client 确认了：自己发送、接收正常，对方发送、接收正常；**Server 确认了：自己发送、接收正常，对方发送、接收正常**

所以三次握手就能确认双发收发功能都正常，缺一不可。

> 四次挥手

<img src="JobQuestions/image-20210910164141374.png" alt="image-20210910164141374" style="zoom:67%;" />

断开一个 TCP 连接则需要“四次挥手”：

- 客户端-发送一个 FIN，用来关闭客户端到服务器的数据传送
- 服务器-收到这个 FIN，它发回一 个 ACK，确认序号为收到的序号加 1 。和 SYN 一样，一个 FIN 将占用一个序号
- 服务器-关闭与客户端的连接，发送一个 FIN 给客户端
- 客户端-发回 ACK 报文确认，并将确认序号设置为收到序号加 1

**任何一方都可以在数据传送结束后发出连接释放的通知，待对方确认后进入半关闭状态。当另一方也没有数据再发送的时候，则发出连接释放通知，对方确认后就完全关闭了 TCP 连接。**

举个例子：A 和 B 打电话，通话即将结束后，A 说“我没啥要说的了”，B 回答“我知道了”，但是 B 可能还会有要说的话，A 不能要求 B 跟着自己的节奏结束通话，于是 B 可能又巴拉巴拉说了一通，最后 B 说“我说完了”，A 回答“知道了”，这样通话才算结束。



---



# 数据库

## [脏读，不可重复读，幻读](https://blog.csdn.net/neverever01/article/details/108237531)

1. 脏读：脏读是指一个事务在处理过程中读取了另一个还没提交的事务的数据。

   > 比如A向B转账100，A的账户减少了100，而B的账户还没来得及修改，此时一个并发的事务访问到了B的账户，就是脏读

2. 不可重复读(虚读)：不可重复读是对于数据库中的某一个字段，一个事务多次查询却返回了不同的值，这是由于在查询的间隔中，该字段被另一个事务修改并提交了。

   > 比如A第一次查询自己的账户有1000元，此时另一个事务给A的账户增加了1000元，所以A再次读取他的账户得到了2000的结果，跟第一次读取的不一样。
   > 不可重复读与脏读的不同之处在于，脏读是读取了另一个事务没有提交的脏数据，不可重复读是读取了已经提交的数据，实际上并不是一个异常现象。

3. 幻读：事务多次读取同一个范围的时候，查询结果的记录数不一样，这是由于在查询的间隔中，另一个事务新增或删除了数据。

   > 比如A公司一共有100个人，第一次查询总人数得到100条记录，此时另一个事务新增了一个人，所以下一次查询得到101条记录。
   > 不可重复度和幻读的不同之处在于，幻读是多次读取的结果行数不同，不可重复度是读取结果的值不同。



## 四大隔离级别

为了保证数据库事务一致性，解决脏读，不可重复读和幻读的问题，数据库的隔离级别一共有四种隔离级别：

1. read uncommitted：读未提交

   > 产生的问题：脏读、不可重复读、幻读

2. **read committed：读已提交 （Oracle默认）**

   > 产生的问题：不可重复读、幻读

3. **repeatable read：可重复读 （MySQL默认）**

   > 产生的问题：幻读

4. serializable：串行化

   > 可以解决所有的问题

Oracle的默认隔离级别是**读已提交**，实现了四种隔离级别中的读已提交和串行化隔离级别

MySQL的默认隔离级别是**可重复读**，并且实现了所有四种隔离级别



## 讲讲索引

> **索引是一种用于快速查询和检索数据的数据结构。**

索引的作用就相当于目录的作用。打个比方: 我们在查字典的时候，如果没有目录，那我们就只能一页一页的去找我们需要查的那个字，速度很慢。如果有目录了，我们只需要先去目录里查找字的位置，然后直接翻到那一页就行了。

**优点:**

- **使用索引可以大大加快 数据的检索速度**（大大减少检索的数据量）, 这也是创建索引的最主要的原因。
- 通过创建唯一性索引，可以保证数据库表中每一行数据的唯一性。

**缺点:**

- **创建索引和维护索引需要耗费许多时间**。当对表中的数据进行增删改的时候，如果数据有索引，那么索引也需要动态的修改，会降低 SQL 执行效率。
- 索引需要使用物理文件存储，也会耗费一定空间。



> 数据库的索引类型分为逻辑分类和物理分类
>
> 其中 唯一索引, 普通索引, 前缀索引 和 全文索引 是 [辅助索引](https://snailclimb.gitee.io/javaguide/#/docs/database/mysql/MySQL%E6%95%B0%E6%8D%AE%E5%BA%93%E7%B4%A2%E5%BC%95?id=%e4%ba%8c%e7%ba%a7%e7%b4%a2%e5%bc%95%e8%be%85%e5%8a%a9%e7%b4%a2%e5%bc%95)

**逻辑分类：**

- **主键索引(Primary Key)** : **当关系表中定义主键时会自动创建主键索引**。
  - 一张数据表有只能有一个主键，并且主键不能为 null，不能重复。
  - 在 MySQL 的 InnoDB 的表中，当没有显示的指定表的主键时，InnoDB 会自动先检查表中是否有唯一索引的字段，如果有，则选择该字段为默认的主键，否则 InnoDB 将会自动创建一个 6Byte 的自增主键。
- **唯一索引(Unique Key)** ： **数据列不能有重复，可以有空值**。
  - 一张表可以有多个唯一索引，但是每个唯一索引只能有一列; 如身份证，卡号等。
  - 建立唯一索引的目的大部分时候都是为了该属性列的数据的唯一性，而不是为了查询效率。
- **普通索引(Index)**  :**普通索引的唯一作用就是为了快速查询数据，一张表允许创建多个普通索引，并允许数据重复和 NULL。**
- **前缀索引(Prefix)** ：前缀索引只适用于字符串类型的数据。前缀索引是对文本的前几个字符创建索引，相比普通索引建立的数据更小， 因为只取前几个字符。
- **全文索引(Full Text)** ：全文索引主要是为了检索大文本数据中的关键字的信息，是目前搜索引擎数据库使用的一种技术。Mysql5.6 之前只有 MYISAM 引擎支持全文索引，5.6 之后 InnoDB 也支持了全文索引。

**物理分类：**

- **聚集索引（聚簇索引）**: **数据在物理存储中的顺序与索引中数据的逻辑顺序相同**，比如以ID建立聚集索引，数据库中id从小到大排列，那么物理存储中该数据的内存地址值也按照从小到大存储。一般是表中的主键索引，如果没有主键索引就会以第一个非空的唯一索引作为聚集索引。**一张表只能有一个聚集索引**。
- **非聚集索引** : **数据在物理存储中的顺序与索引中数据的逻辑顺序不同**。非聚集索引因为无法定位数据所在的行，所以**需要扫描两遍索引树**。第一遍扫描非聚集索引的索引树，确定该数据的主键ID，然后到主键索引（聚集索引）中寻找相应的数据。



## 数据库索引数据结构有哪几种, 分别有哪些优势

> **常见的索引结构有: B 树， B+树和 Hash。**

1. [Hash表](https://snailclimb.gitee.io/javaguide/#/docs/database/mysql/MySQL%E6%95%B0%E6%8D%AE%E5%BA%93%E7%B4%A2%E5%BC%95?id=hash%e8%a1%a8-amp-b%e6%a0%91)

哈希表是键值对的集合，通过键(key)即可快速取出对应的值(value)

> **为什么MySQL 没有使用其作为索引的数据结构呢？**
>
> 1. **Hash 冲突问题**
>2. **Hash 索引不支持顺序和范围查询(Hash 索引不支持顺序和范围查询是它最大的缺点：** 假如我们要对表中的数据进行排序或者进行范围查询，那 Hash 索引可就不行了。

- 优点: 
  - **可以快速检索数据 (接近 O(1) )**

- 缺点: 
  - Hash 冲突问题
  - 不适合范围查询

2. [B树 & B+树](https://snailclimb.gitee.io/javaguide/#/docs/database/mysql/MySQL%E6%95%B0%E6%8D%AE%E5%BA%93%E7%B4%A2%E5%BC%95?id=b-%e6%a0%91amp-b%e6%a0%91)

> B 树也称 B-树,全称为 **多路平衡查找树** ，B+ 树是 B 树的一种变体。
>
> B 树和 B+树中的 B 是 `Balanced` （平衡）的意思。
>
> **目前大部分数据库系统及文件系统都采用 B-Tree 或其变种 B+Tree 作为索引结构。** 

<img src="JobQuestions/image-20210825195012334.png" alt="image-20210825195012334" style="zoom:80%;" />

**B 树& B+树两者有何异同呢？**

- B 树的所有节点既存放键(key) 也存放 数据(data)，而 B+树只有叶子节点存放 key 和 data，其他内节点只存放 key。
- B 树的叶子节点都是独立的; B+树的叶子节点有一条引用链指向与它相邻的叶子节点。
- B 树的检索的过程相当于对范围内的每个节点的关键字做二分查找，可能还没有到达叶子节点，检索就结束了。而 B+树的检索效率就很稳定了，任何查找都是从根节点到叶子节点的过程，叶子节点的顺序检索很明显。

<img src="JobQuestions/image-20210825200106432.png" alt="image-20210825200106432" style="zoom: 67%;" />

在 MySQL 中，MyISAM 引擎和 InnoDB 引擎都是使用 B+Tree 作为索引结构，但是，两者的实现方式不太一样。（下面的内容整理自《Java 工程师修炼之道》）

> MyISAM 引擎中，B+Tree 叶节点的 data 域存放的是数据记录的地址。在索引检索的时候，首先按照 B+Tree 搜索算法搜索索引，如果指定的 Key 存在，则取出其 data 域的值，然后以 data 域的值为地址读取相应的数据记录。这被称为“非聚簇索引”。
>
> InnoDB 引擎中，其数据文件本身就是索引文件。相比 MyISAM，索引文件和数据文件是分离的，其表数据文件本身就是按 B+Tree 组织的一个索引结构，树的叶节点 data 域保存了完整的数据记录。这个索引的 key 是数据表的主键，因此 InnoDB 表数据文件本身就是主索引。这被称为“聚簇索引（或聚集索引）”，而其余的索引都作为辅助索引，辅助索引的 data 域存储相应记录主键的值而不是地址，这也是和 MyISAM 不同的地方。在根据主索引搜索时，直接找到 key 所在的节点即可取出数据；在根据辅助索引查找时，则需要先取出主键的值，在走一遍主索引。 因此，在设计表的时候，不建议使用过长的字段作为主键，也不建议使用非单调的字段作为主键，这样会造成主索引频繁分裂。

**B+树优点**：由于B+树的数据都存储在叶子结点中，分支结点均为索引，方便扫库，只需要扫一遍叶子结点即可，但是B树因为其分支结点同样存储着数据，我们要找到具体的数据，需要进行一次中序遍历按序来扫，所以**B+树更加适合在区间查询的情况**，所以**通常B+树用于数据库索引，而B树则常用于文件索引**。



## [为什么用B+树存储索引](https://www.cnblogs.com/tiancai/p/9024351.html)

1. **B+树的磁盘读写代价更低：B+树的内部节点并没有指向关键字具体信息的指针，因此其内部节点相对B树更小，如果把所有同一内部节点的关键字存放在同一盘块中，那么盘块所能容纳的关键字数量也越多，一次性读入内存的需要查找的关键字也就越多，相对IO读写次数就降低了。**
2. **B+树的查询效率更加稳定：由于非叶结点并不是最终指向文件内容的结点，而只是叶子结点中关键字的索引。所以任何关键字的查找必须走一条从根结点到叶子结点的路。所有关键字查询的路径长度相同，导致每一个数据的查询效率相当。**
3. 由于B+树的数据都存储在叶子结点中，分支结点均为索引，方便扫库，只需要扫一遍叶子结点即可，但是B树因为其分支结点同样存储着数据，我们要找到具体的数据，需要进行一次中序遍历按序来扫，所以B+树更加适合在区间查询的情况，所以通常B+树用于数据库索引。

采用B+树的**主要原因**是：**B树在提高了IO性能的同时并没有解决元素遍历的我效率低下的问题**，正是为了解决这个问题，B+树应用而生。**B+树只需要去遍历叶子节点就可以实现整棵树的遍历**。而且在数据库中基于范围的查询是非常频繁的，而B树不支持这样的操作或者说效率太低。

## 为什么非叶子结点只存储索引值

**磁盘读写代价更低**：B+树的内部节点并没有指向关键字具体信息的指针，因此其内部节点相对B树更小，如果把所有同一内部节点的关键字存放在同一盘块中，那么盘块所能容纳的关键字数量也越多，一次性读入内存的需要查找的关键字也就越多，相对IO读写次数就降低了。



## 一条慢查询语句, 怎么优化它

> https://blog.csdn.net/qq_35571554/article/details/82800463
>
> https://www.bilibili.com/video/BV1kv411N7AS?from=search&seid=8192419883792571800



## 说一下 MySQL 执行一条查询语句的内部执行过程？

1. 连接器：客户端先通过连接器连接到 MySQL 服务器。
2. 缓存：连接器权限验证通过之后，先查询是否有查询缓存，如果有缓存（之前执行过此语句）则直接返回缓存数据，如果没有缓存则进入分析器。
3. 分析器：分析器会对查询语句进行语法分析和词法分析，判断 SQL 语法是否正确，如果查询语法错误会直接返回给客户端错误信息，如果语法正确则进入优化器。
4. 优化器：优化器是对查询语句进行优化处理，例如一个表里面有多个索引，优化器会判别哪个索引性能更好。
5. 执行器：优化器执行完就进入执行器，执行器就开始执行语句进行查询比对了，直到查询到满足条件的所有数据，然后进行返回。



## 介绍一下MySQL事务





## Redis的数据结构即使用场景

> 这里的数据类型指的是 数据的 key 对应的 value 的数据类型
>
> - set/get
> - hset/hget
> - lpush/rpop
> - sadd/spop (随机弹出元素)
> - zadd (排行榜)

![image-20210910150026434](JobQuestions/image-20210910150026434.png)



---



# 其他

## [什么是序列化?什么是反序列化?](https://snailclimb.gitee.io/javaguide/#/docs/java/basis/Java基础知识?id=什么是序列化什么是反序列化)

> 如果我们需要持久化 Java 对象比如将 Java 对象保存在文件中，或者在网络传输 Java 对象，这些场景都需要用到序列化。

简单来说：

- **序列化**： 将数据结构或对象**转换成二进制字节流**的过程
- **反序列化**：将在序列化过程中所生成的二进制字节流的过程**转换成数据结构或者对象**的过程

对于 Java 这种面向对象编程语言来说，我们序列化的都是对象（Object）也就是实例化后的类(Class)，但是在 C++这种半面向对象的语言中，struct(结构体)定义的是数据结构类型，而 class 对应的是对象类型。

![image-20210915132722519](JobQuestions/image-20210915132722519.png)

## [Java 序列化中如果有些字段不想进行序列化，怎么办？](https://snailclimb.gitee.io/javaguide/#/docs/java/basis/Java基础知识?id=java-序列化中如果有些字段不想进行序列化，怎么办？)

> 对于不想进行序列化的变量，使用 `transient` 关键字修饰。

`transient` 关键字的作用是：阻止实例中那些用此关键字修饰的的变量序列化；当对象被反序列化时，被 `transient` 修饰的变量值不会被持久化和恢复。

关于 `transient` 还有几点注意：

- `transient` 只能修饰变量，不能修饰类和方法。
- `transient` 修饰的变量，在反序列化后变量值将会被置成类型的默认值。例如，如果是修饰 `int` 类型，那么反序列后结果就是 `0`。
- `static` 变量因为不属于任何对象(Object)，所以无论有没有 `transient` 关键字修饰，均不会被序列化。



## [Java 中 IO 流分为几种?](https://snailclimb.gitee.io/javaguide/#/docs/java/basis/Java基础知识?id=java-中-io-流分为几种)

- 按照流的流向分，可以分为输入流和输出流；
- 按照操作单元划分，可以划分为字节流和字符流；
- 按照流的角色划分为节点流和处理流。

**Java IO 流共涉及 40 多个类，这些类看上去很杂乱，但实际上很有规则，而且彼此之间存在非常紧密的联系， Java IO 流的 40 多个类都是从如下 4 个抽象类基类中派生出来的。**

- InputStream/Reader: 所有的输入流的基类，前者是字节输入流，后者是字符输入流。
- OutputStream/Writer: 所有输出流的基类，前者是字节输出流，后者是字符输出流。

**按操作方式分类结构图**：

![按操作方式分类结构图](JobQuestions/IO-%E6%93%8D%E4%BD%9C%E6%96%B9%E5%BC%8F%E5%88%86%E7%B1%BB.png)











**按操作对象分类结构图：**

![按操作对象分类结构图](JobQuestions/IO-%E6%93%8D%E4%BD%9C%E5%AF%B9%E8%B1%A1%E5%88%86%E7%B1%BB.png)



> **既然有了字节流为什么还要有字符流**

问题本质想问：**不管是文件读写还是网络发送接收，信息的最小存储单元都是字节，那为什么 I/O 流操作要分为字节流操作和字符流操作呢？**

回答：

- 字符流是由 Java 虚拟机将字节转换得到的，问题就出在这个过程还算是非常耗时，并且，如果我们不知道编码类型就很容易出现乱码问题。
- 所以， I/O 流就干脆提供了一个直接操作字符的接口，方便我们平时对字符进行流操作。

- 如果音频文件、图片等媒体文件用字节流比较好，如果涉及到字符的话使用字符流比较好。



## [Comparable 和 Comparator 的区别](https://snailclimb.gitee.io/javaguide/#/docs/java/collection/Java集合框架常见面试题?id=_131-comparable-和-comparator-的区别)

- `comparable` 接口实际上是出自`java.lang`包 它有一个 `compareTo(Object obj)`方法用来排序
- `comparator`接口实际上是出自 java.util 包它有一个`compare(Object obj1, Object obj2)`方法用来排序

```java
public interface Comparable<T> { // 放在某个类里面实现
    /**
     * Compares this object with the specified object for order.  Returns a
     * negative integer, zero, or a positive integer as this object is less
     * than, equal to, or greater than the specified object.
     *
     * @param   o the object to be compared.
     * @return  a negative integer, zero, or a positive integer as this object
     *          is less than, equal to, or greater than the specified object.
     *
     * @throws NullPointerException if the specified object is null
     * @throws ClassCastException if the specified object's type prevents it
     *         from being compared to this object.
     */
    public int compareTo(T o);
}
```

```java
@FunctionalInterface
public interface Comparator<T> { // 定制排序 : Collections.sort(arrayList, new Comparator<Integer>() {...}
    /**
     * Compares its two arguments for order.  Returns a negative integer,
     * zero, or a positive integer as the first argument is less than, equal
     * to, or greater than the second.<p>
     * 
     * @param o1 the first object to be compared.
     * @param o2 the second object to be compared.
     * @return a negative integer, zero, or a positive integer as the
     *         first argument is less than, equal to, or greater than the
     *         second.
     * @throws NullPointerException if an argument is null and this
     *         comparator does not permit null arguments
     * @throws ClassCastException if the arguments' types prevent them from
     *         being compared by this comparator.
     */
    int compare(T o1, T o2);
    
...
    
}
```

> 一般我们需要对一个集合使用自定义排序时，我们就要重写`compareTo()`方法或`compare()`方法，

- 当我们需要对某一个集合实现两种排序方式，比如一个 song 对象中的歌名和歌手名分别采用一种排序方法的话，
- 我们可以重写`compareTo()`方法和使用自制的`Comparator`方法
- 或者 以两个 Comparator 来实现歌名排序和歌星名排序，第二种代表我们只能使用两个参数版的 `Collections.sort()`.

## [Collections 工具类](https://snailclimb.gitee.io/javaguide/#/docs/java/collection/Java集合框架常见面试题?id=_16-collections-工具类)

1. 排序
2. 查找,替换操作

> 排序

```java
void reverse(List list)					//反转
void shuffle(List list)					//随机排序
void sort(List list) 					//按自然排序的升序排序
void sort(List list, Comparator c) 		//定制排序，由Comparator控制排序逻辑
void swap(List list, int i , int j) 	//交换两个索引位置的元素
void rotate(List list, int distance) 	//旋转。
// 1 当distance为正数时，将 list 后distance个元素 整体移到前面
// 2 当distance为负数时，将 list 前distance个元素 整体移到后面
```

> 查找, 替换操作

```java
int binarySearch(List list, Object key)//对List进行二分查找，返回索引，注意List必须是有序的
int max(Collection coll)//根据元素的自然顺序，返回最大的元素。 类比int min(Collection coll)
int max(Collection coll, Comparator c)//根据定制排序，返回最大元素，排序规则由Comparatator类控制。类比int min(Collection coll, Comparator c)
void fill(List list, Object obj)//用指定的元素代替指定list中的所有元素
int frequency(Collection c, Object o)//统计元素出现次数
int indexOfSubList(List list, List target)//统计target在list中第一次出现的索引，找不到则返回-1，类比int lastIndexOfSubList(List source, list target)
boolean replaceAll(List list, Object oldVal, Object newVal)//用新元素替换旧元素
```

## Arrays.sort() 源码分析

> 来源: https://leetcode-cn.com/leetbook/read/sort-algorithms/evzgrc/

Java 源码中的 Arrays.sort() 函数是由 Java 语言的几位设计者所编写的，它并没有采用某种单一的排序算法，而是通过分析所输入数据的规模、特点，对不同的输入数据采用不同的排序算法。

Arrays 类中有很多个 sort 函数：

```java
void sort (int[])
void sort (int[], int, int)
void sort (long[])
void sort (long[], int, int)
void sort (short)
void sort (short[], int, int)
void sort (char[])
void sort (char[], int, int)
void sort (byte[])
void sort (byte[], int, int)
void sort (float[])
void sort (float[], int, int)
void sort (double[])
void sort (double[], int, int)
void sort (Object[])
void sort (Object[], int, int)
void sort (T[], Comparator)
void sort (T[], int, int, Comparator)
```

这些 sort 函数可以分为两类：

- 对基本类型的排序（int、long、short、char、byte、float、double）
- 对非基本类型的排序（Object、T）

对基本类型的排序是通过调用对应的 DualPivotQuicksort.sort() 函数完成的。

对非基本类型的排序采用的是 TimSort 或者归并排序，在 JDK 1.7 之前，默认采用归并排序，JDK 1.7 及之后，默认采用 TimSort，但可以通过设置 JVM 参数 -Djava.util.Arrays.useLegacyMergeSort=true 继续使用归并排序。





---

