---
title: Javaer's development
categories:
  - Java
  - Job
tags:
  - Job
author:
  - Jungle
date: 2023-09-05
---

# Java后端技术体系

## Java基础

![image-20230824161549038](./4Job/image-20230824161549038.png)

## Java高级

![image-20230824162155166](./4Job/image-20230824162155166.png)

![image-20230824163755286](./4Job/image-20230824163755286.png)

![image-20230824163819172](./4Job/image-20230824163819172.png)

![image-20230824164633202](./4Job/image-20230824164633202.png)



![image-20230824164938666](./4Job/image-20230824164938666.png)

## JavaWEB

![image-20230825153740131](./4Job/image-20230825153740131.png)

## 主流的框架和项目管理

![image-20230825154605779](./4Job/image-20230825154605779.png)

## 分布式 微服务 并行架构

![image-20230825164443881](./4Job/image-20230825164443881.png)



![image-20230825164501291](./4Job/image-20230825164501291.png)

## 开发运维一体化（DevOps）

![image-20230825181114990](./4Job/image-20230825181114990.png)

## 大数据技术

![image-20230825181349973](./4Job/image-20230825181349973.png)

## 项目

![image-20230825181534292](./4Job/image-20230825181534292.png)

## 大厂面试题

高级部分

主流框架部分

中间件



# 面经

## 打招呼

```xml
您好，本人2022年硕士计算机毕业，一年一个月Java开发经验，并且熟悉微服务项目开发，拥有扎实的数据库交互基础，具有一定的业务逻辑思维。工作期间，参与过生态仓项目，该项目使用了SSM+MQ等技术实现，为消费者提供了良好的使用体验；还负责日常需求迭代的开发及自测，可以按需完成任务。如果您感兴趣，可以随时回复我，一定不会让您失望。
```



## 双亲委派机制

![image-20230822160516082](./4Job/image-20230822160516082.png)



## lock和synchronized的区别

![image-20230823102153725](./4Job/image-20230823102153725.png)

## 锁的本质

![image-20230823101404133](./4Job/image-20230823101404133.png)

![image-20230823101306013](./4Job/image-20230823101306013.png)

**自旋锁**（轻量级锁：加锁之前进行重试）

![image-20230823102126641](./4Job/image-20230823102126641.png)

## 重写`equals`的时候为什么要重写`hashcode`方法

> 结论

```xml

```





>  Object 的 equals 比较的是同一对象。

![image-20230902223617252](./4Job/image-20230902223617252.png)

>  String重写equals后：

![image-20230902223817462](./4Job/image-20230902223817462.png)



>  为什么还要重写`hashCode()`

![image-20230902224021230](./4Job/image-20230902224021230.png)

**相等的对象必须具备相等的哈希码。**

![image-20230902224133217](./4Job/image-20230902224133217.png)



>  分析: Object 类中的`hashCode`和`equals`约定如下

![image-20230902224242137](./4Job/image-20230902224242137.png)



>  所以  String中重写 `hashCode` 满足了规定：
>
> `如果两个对象x.equals(y)方法返回true, 则这两个对象的hashCode必须相等。`

![image-20230902224531900](./4Job/image-20230902224531900.png)



---



# 刷题

## 输入输出

![image-20230824105647450](./4Job/image-20230824105647450.png)

## 快捷键

**Ctrl + j**



## IDEA设置

IDEA默认情况下，DEBUG显示的数据是简化后的，如果希望看到完整的数据，需要做如下设置：

![image-20230830110255938](./4Job/image-20230830110255938.png)





# 集合

1 集合框架

![image-20230828233603601](./4Job/image-20230828233603601.png)



2 集合图

2.1 单列集合

![image-20230828235121734](./4Job/image-20230828235121734.png)

2.2 双列集合

![image-20230828235347930](./4Job/image-20230828235347930.png)

## Collection

![image-20230829000342578](./4Job/image-20230829000342578.png)

![image-20230829001901269](./4Job/image-20230829001901269.png)



### **迭代器 Iterator**

![image-20230829002003448](./4Job/image-20230829002003448.png)

![image-20230829002200846](./4Job/image-20230829002200846.png)

![image-20230829002529606](./4Job/image-20230829002529606.png)

**编译看左边，运行看右边**【编译是Object类型，运行是Collection类型】

![image-20230829003421943](./4Job/image-20230829003421943.png)

**输出**：

![image-20230829003207448](./4Job/image-20230829003207448.png)



### 增强for

既可以在集合中使用，也可以在数组中使用。

**其底层也是迭代器。**

![image-20230829004007124](./4Job/image-20230829004007124.png)

**输出：**

![image-20230829003932715](./4Job/image-20230829003932715.png)



## List

### 常用方法

![image-20230829110039900](./4Job/image-20230829110039900.png)

指定位置插入：

![image-20230829114254108](./4Job/image-20230829114254108.png)

输出：

![image-20230829114236162](./4Job/image-20230829114236162.png)

`indexOf ` 

![image-20230829114817253](./4Job/image-20230829114817253.png)

`lastIndexOf`

`remove`

`set` 

![image-20230829115416751](./4Job/image-20230829115416751.png)

`set` 即 替换，效果如下图：

![image-20230829115307100](./4Job/image-20230829115307100.png)

`subList`

![image-20230829115924478](./4Job/image-20230829115924478.png)

返回子集合：[0, 2)

![image-20230829115847417](./4Job/image-20230829115847417.png)



### `ArrayList` --> 1.5倍扩容 (10)

线程不安全的。

![image-20230829120830252](./4Job/image-20230829120830252.png)



**transient 表示该属性不会被序列化**

![image-20230829121208583](./4Job/image-20230829121208583.png)



**源码分析：**

![image-20230829122242003](./4Job/image-20230829122242003.png)

**`modCount` 表示集合被修改的次数。**

![image-20230829170846821](./4Job/image-20230829170846821.png)

![image-20230829170928648](./4Job/image-20230829170928648.png)

![image-20230829170901278](./4Job/image-20230829170901278.png)



最后一步：

![image-20230829170548949](./4Job/image-20230829170548949.png)



### Vector --> 2倍扩容 (10)

比较：

### ![image-20230830150659379](./4Job/image-20230830150659379.png)

add: synchronized 线程安全的。

![image-20230830151746255](./4Job/image-20230830151746255.png)



![image-20230830151544739](./4Job/image-20230830151544739.png)

如果需要的数组大小不够用了，就进行扩容：

`capacityIncrement` 一般为0，除非你自己指定。

```
int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
                                 capacityIncrement : oldCapacity);
```



![image-20230830151616205](./4Job/image-20230830151616205.png)



### LinkedList

#### 双向链表

![image-20230830154044665](./4Job/image-20230830154044665.png)

#### 双端队列

![image-20230830154212711](./4Job/image-20230830154212711.png)

#### 底层结构

**`LinedList`即是双向链表也是双端队列。**

- 添加和删除方便
- 可以作为队列 （先进先出）

![image-20230830153642433](./4Job/image-20230830153642433.png)



**自定义实现：**

1 内部类 Node：

![image-20230830154536078](./4Job/image-20230830154536078.png)



2 模拟实现：

![image-20230830154920812](./4Job/image-20230830154920812.png)

遍历：

![image-20230830163018843](./4Job/image-20230830163018843.png)

![image-20230830163056679](./4Job/image-20230830163056679.png)

#### 底层源码

1 add

![image-20230830164042687](./4Job/image-20230830164042687.png)

`LinkedLast` 尾插： 将新结点加入到双向链表的最后。

![image-20230830164137019](./4Job/image-20230830164137019.png)

![image-20230830164259801](./4Job/image-20230830164259801.png)

效果：

![image-20230830164346353](./4Job/image-20230830164346353.png)

![image-20230830164942301](./4Job/image-20230830164942301.png)

2 remove() 实际上调用的是 `removeFirst()`

![image-20230830165928427](./4Job/image-20230830165928427.png)

![image-20230830170018980](./4Job/image-20230830170018980.png)

根据内容与下标删除：



![image-20230830170356207](./4Job/image-20230830170356207.png)

3 set 修改

4 get 获取

5 遍历

>  实现了List接口，因此可以使用迭代器与for及增强for循环

![image-20230830170633921](./4Job/image-20230830170633921.png)



## List集合选择 (非并发)

> 查改多 --> `ArrayList`
>
> 增删多 --> `LinkedList`
>
> 一般情况下，以查询为主，偏向 `ArrayList`

![image-20230830170748203](./4Job/image-20230830170748203.png)



## Set 

**无序且不重复**

![image-20230831205752428](./4Job/image-20230831205752428.png)



![image-20230831205932641](./4Job/image-20230831205932641.png)



![image-20230831210036083](./4Job/image-20230831210036083.png)



不能通过索引来获取值，但可以通过迭代器或增强for。

![image-20230901114937649](./4Job/image-20230901114937649.png)

### HashSet --> 2倍扩容 (16)

HashSet底层是HashMap

**注：取出的顺序是无序的，但是是固定的。** 【因为顺序是通过Hash算法确定的。】

![image-20230901121132050](./4Job/image-20230901121132050.png)

#### 案例

 案例一：

![image-20230901123529798](./4Job/image-20230901123529798.png)

案列二：Dog没有重写`HashCode和toString` 。

![image-20230901124225751](./4Job/image-20230901124225751.png)



#### 底层机制 --> HashMap --> 2倍扩容 (16)

>  HashSet的底层是HashMap，HashMap底层是（数组+链表+红黑树）。



![image-20230901125554361](./4Job/image-20230901125554361.png)

模拟实现：

![image-20230901125202928](./4Job/image-20230901125202928.png)



table[2] 为一个链表。

![image-20230901125952247](./4Job/image-20230901125952247.png)



**结论：**

![image-20230901152331557](./4Job/image-20230901152331557.png)

##### **add 过程：**

  ![image-20230901161407136](./4Job/image-20230901161407136.png)

去key的`hashCode`的高16位。

![image-20230901160826188](./4Job/image-20230901160826188.png)



![image-20230901161502471](./4Job/image-20230901161502471.png)

该索引上为null时：

![image-20230901161540975](./4Job/image-20230901161540975.png)

![image-20230901161611327](./4Job/image-20230901161611327.png)

不为null时：【进入链表】

hash相等 且 （key相等或key内容相等）则不加入。

![image-20230901172759831](./4Job/image-20230901172759831.png)

加入红黑树

![image-20230901173112703](./4Job/image-20230901173112703.png)

依次判断是否加入：

![image-20230901173217563](./4Job/image-20230901173217563.png)



**不加入**返回当前元素值：【`oldValue`】

![image-20230901173307365](./4Job/image-20230901173307365.png)

**加入**成功，返回【null】

![image-20230901173319340](./4Job/image-20230901173319340.png)

#### 树化条件

> 在Java8中，如果一条链表的元素个数达到 TREEIFY_THRESHOLD(默认是8)，并且table的大小 >= MIN_TREEIFY_CAPACITY(默认64)，就会进行树化(红黑树)，否则仍然采用数组扩容机制。【容量(阈值)：16(12)->32(24)->64(48)->128(96)】



模拟：当hashCode为一固定值时，就会往 `(n - 1) & hash` 

![image-20230902232202817](./4Job/image-20230902232202817.png)

![image-20230902230514131](./4Job/image-20230902230514131.png)

eg:  100 & (8-1) == 4；100 & (16-1) == 4；100 & (32-1) == 4；100 & (64-1) == 36；

扩容到64时，且链表长度大于8 --> 转换为红黑树

![image-20230902233338625](./4Job/image-20230902233338625.png)



#### 扩容注意事项

```java
// length(threshold): 16(12)
if (++size > threshold)
      resize();
```

当size大于12时就进行扩容。若table[i]为链表时，size就统计当前table[i]的链表长度。





### LinkedHashSet

> 底层是 数组+双向链表

![image-20230903011554047](./4Job/image-20230903011554047.png)

底层机制示意图

> 双向链表 让Set的加入看上去有序了

![image-20230903011912778](./4Job/image-20230903011912778.png)

关键代码：

```java
/** 内部类
 * HashMap.Node subclass for normal LinkedHashMap entries.
 */
static class Entry<K,V> extends HashMap.Node<K,V> {
    Entry<K,V> before, after;
    Entry(int hash, K key, V value, Node<K,V> next) {
        super(hash, key, value, next);
    }
}

/**
 * The head (eldest) of the doubly linked list.
 */
transient LinkedHashMap.Entry<K,V> head;
/**
 * The tail (youngest) of the doubly linked list.
 */
transient LinkedHashMap.Entry<K,V> tail;
```



---

### TreeSet

> 底层是TreeMap

![image-20230903232057041](./4Job/image-20230903232057041.png)

![image-20230903233014207](./4Job/image-20230903233014207.png)

**解析：**

![image-20230903235019272](./4Job/image-20230903235019272.png)

![image-20230903235323430](./4Job/image-20230903235323430.png)



> **String 的 CompareTo 方法：**

- 负数【negative】：在参数之前 【this {@code String} object **precedes** the argument string】

- 正数【positive】：在参数之后 【this {@code String} object **follows** the argument string】
- 0 ：equals

![image-20230903232152239](./4Job/image-20230903232152239.png)



























---

## Map 

### 分析理解

![image-20230903014217665](./4Job/image-20230903014217665.png)

 ![image-20230903015856991](./4Job/image-20230903015856991.png)



>  k-v 放在 Node 中，`keySet --> k, values --> v, entrySet --> (k,v) ` 都是指向它的引用。

```java
static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        Node<K,V> next;

        Node(int hash, K key, V value, Node<K,V> next) {
            this.hash = hash;
            this.key = key;
            this.value = value;
            this.next = next;
        }
 ...
 transient Set<K>        keySet;
 transient Collection<V> values;
 public abstract Set<Entry<K,V>> entrySet();
```



![image-20230903102122417](./4Job/image-20230903102122417.png)

`entrySet`

![image-20230903104243361](./4Job/image-20230903104243361.png)

`keySet` and `values`

![image-20230903104416203](./4Job/image-20230903104416203.png)



### 常用方法

> 增 put
>
> 删 remove
>
> 查 get
>
> 改 put

![image-20230903104611838](./4Job/image-20230903104611838.png)

### Map的遍历

> - `keySet` : 增强for 或 迭代器
> - `values` : 所有Collections的使用遍历方法

![image-20230903153504238](./4Job/image-20230903153504238.png)

`keySet`

![image-20230903153857132](./4Job/image-20230903153857132.png)

`values`

![image-20230903154204920](./4Job/image-20230903154204920.png)

`entrySet`

![image-20230903154524118](./4Job/image-20230903154524118.png)

 ![image-20230903154916417](./4Job/image-20230903154916417.png)

### HashMap小结 --> 2倍扩容 (16)

![image-20230903163913467](./4Job/image-20230903163913467.png)

### HashMap底层机制及源码剖析

![image-20230903164212452](./4Job/image-20230903164212452.png)



![image-20230903164911185](./4Job/image-20230903164911185.png)



### HashTable --> 2N+1 扩容  (11)

> k 和 v 都不能为null, 线程安全。

![image-20230903215654787](./4Job/image-20230903215654787.png)

![image-20230903221551703](./4Job/image-20230903221551703.png)

![image-20230903221633989](./4Job/image-20230903221633989.png)



### Properties

> 是 Hashtable 的子类
>
> k 和 v 都不能为null, 线程安全。

![image-20230903221738231](./4Job/image-20230903221738231.png)

![image-20230903222231311](./4Job/image-20230903222231311.png)

 

### TreeMap 

> 无参构造：默认是按照key的字典顺序，排序有序。
>
> 存取无序：指 插入和取出顺序不一致

![image-20230903235905012](./4Job/image-20230903235905012.png)

自定义比较方法：按照长度排序，逆序

![image-20230904000548887](./4Job/image-20230904000548887.png)

![image-20230904000714130](./4Job/image-20230904000714130.png)

![image-20230904000829250](./4Job/image-20230904000829250.png)

![image-20230904001125189](./4Job/image-20230904001125189.png)

自定义比较时的关键代码：

```java
int cmp;
Entry<K,V> parent;
// split comparator and comparable paths
Comparator<? super K> cpr = comparator;
if (cpr != null) {
    do {
        parent = t;
        cmp = cpr.compare(key, t.key);
        if (cmp < 0)
            t = t.left;
        else if (cmp > 0)
            t = t.right;
        else
            return t.setValue(value);
    } while (t != null);
}
```

> ` treeMap.put ()`

第一次进来：

![image-20230904001220341](./4Job/image-20230904001220341.png)

第二次进来：【比较取决于 `comparator.compare()` 方法】

![image-20230904001526717](./4Job/image-20230904001526717.png)

测试：按照长度进行排序

![image-20230904001811355](./4Job/image-20230904001811355.png)

输出：虽然加入不了，但是value会被替换掉：(tom, 汤姆) -> (tom, 韩顺平)。

![image-20230904001823840](./4Job/image-20230904001823840.png)





## 集合选择

> - 单列 Collection
>   - 允许重复：List
>     - LinkedList: 增删多 【双向链表】
>     - ArrayList: 查改多
>   - 不允许重复：Set
>     - HashSet：无序 【指输入和取出顺序不一致】
>     - TreeSet：**排序**
>     - LinkedHashSet：插入与取出顺序一致 【双向链表】
> - 双列 Map
>   - HashMap：【Key 】无序
>   - TreeMap：【Key】**排序**
>   - LinkedHashMap：【Key】插入和取出顺序一致 【双向链表】
>   - Properties：读取文件

![image-20230903223544012](./4Job/image-20230903223544012.png)



## Collections工具类

![image-20230904002215754](./4Job/image-20230904002215754.png)

其他：

- max
- min
- `frequency(Collection<?> c, Object o) `
- `copy(List<? super T> dest, List<? extends T> src)`
- `replaceAll(List<T> list, T oldVal, T newVal)`

![image-20230904015057782](./4Job/image-20230904015057782.png)



---

## 多线程

程序： 为了完成特定任务、用某种语言编写的一组指令的集合。

进程：指运行中的程序，即程序的一次执行过程。

线程：由进程创建的，是进程的一个实体。 

单线程：同一时刻，只允许执行一个线程。

多线程：同一时刻，可以执行多个线程。

**并发：同一时刻，多个任务交替执行。**【单核CPU实现的多任务就是并发】

并行：同一时刻，多个任务同时进行。【多核CPU同时进行】



### 开启线程

1. 实现 Runnable 接口 `myThread implements Runnable`
2. new Thread(Runnable target, String name).start();

```java
@SuppressWarnings("all")
public class myThread implements Runnable{

    private volatile static int ticketNum = 10;

    private boolean flag = true;

    private synchronized void buy() throws InterruptedException {
        if (ticketNum < 1) {
            flag = false;
            return;
        }
        Thread.sleep(100);
        System.out.println(Thread.currentThread().getName() + "拿到了第" + ticketNum-- + "张票!");
    }

    @Override
    public void run() {
        while (flag) {
            try {
                buy();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        }
    }
}


public class Main {
    public static void main(String[] args) {
        myThread thread = new myThread();

        new Thread(thread, "小明").start();
        new Thread(thread, "老师").start();
        new Thread(thread, "黄牛党").start();
    }
}
```

3. 也可以用 lambda 代替Runnable 

```java
new Thread(()->{
    //todo...
}).start();

()->{...} 即是thread【实现了Runnable 的myThread】
```



### 线程池

> ThreadPoolExecutor

```java
@SuppressWarnings("all")
public class Main {
    private static final int CORE_POOL_SIZE = 5;
    private static final int MAX_POOL_SIZE = 10;
    private static final int QUEUE_CAPACITY = 100;
    private static final Long KEEP_ALIVE_TIME = 1L;

    public static void main(String[] args) throws InterruptedException {
        myThread thread = new myThread();

        // 单个线程
//        new Thread(thread, "小明").start();
//        new Thread(thread, "老师").start();
//        new Thread(thread, "黄牛党").start();

        // 线程池方式
        ThreadPoolExecutor executor = new ThreadPoolExecutor(
                // 核心线程数为 5
                CORE_POOL_SIZE,
                // 最大线程数 10
                MAX_POOL_SIZE,
                // 大于核心线程数时,核心线程外的线程等待时间: 1L
                KEEP_ALIVE_TIME,
                // 等待时间的单位为 TimeUnit.SECONDS
                TimeUnit.SECONDS,
                // 任务队列
                new ArrayBlockingQueue<>(QUEUE_CAPACITY),
                // 饱和策略
                new ThreadPoolExecutor.CallerRunsPolicy());
        // 执行
        for (int i = 0; i < 10; i++) {
            executor.execute(thread);
        }
        // 终止
        executor.shutdown();
        while (!executor.isTerminated()) {
            // 线程等待
        }
        System.out.println("All threads is finished.");
    }
}
```

































