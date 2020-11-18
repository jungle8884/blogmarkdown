---
title: Java-Note15
categories:
  - Java
tags:
  - List
  - Set
date: 2020-11-04 18:57:20
author: Jungle

---
# java.util.List extends Collection #

## List接口的特点 ##
1. 有序的集合, 存储元素和取出元素的顺序是一致的
2. 有索引, 包含了一些带索引的方法
3. 允许存储重复的元素

## List接口中带有索引的方法 ##
- public void add(int index, E element) :  在列表的指定位置插入指定元素
- public  E get(int index) :  返回列表中指定位置的元素。 
- public E remove(int index) :  移除列表中指定位置的元素, 返回的是被移除的元素.
- public  E set(int index, E element) :  用指定元素替换列表中指定位置的元素, 返回的是被替代的元素.

- 注意: 操作索引时, 要防止索引越界异常.
	- IndexOutOfBoundsException: 索引越界异常 --> 集合
	- ArrayIndexOutOfBoundsException: 数组索引越界异常 
	- StringIndexOutOfBoundsException: 字符串索引越界异常 

			package demo;
			
			import java.util.ArrayList;
			import java.util.Iterator;
			import java.util.List;
			
			public class demoList {
			    public static void main(String[] args) {
			        List<String> sList = new ArrayList<>();
			
			        sList.add("a");
			        sList.add("b");
			        sList.add("c");
			        sList.add("d");
			        System.out.println(sList);
			
			        //sList.set(2, "替代c");
			        //System.out.println(sList);
			
			        // 遍历方式 1
			        for (int i = 0; i < sList.size(); i++) {
			            System.out.println(sList.get(i));
			        }
			
			        // 迭代器方式 2
			        Iterator it = sList.iterator();
			        while (it.hasNext()){
			            System.out.println(it.next());
			        }
			
			        // 增强for 3
			        for (String s : sList) {
			            System.out.println(s);
			        }
			    }
			}


## ArrayList ## 
- java.util.ArrayList 集合数据存储的结构是**数组结构**.
- 元素增删慢, 
- **查找快**.
- 由于日常开发中使用最多的功能是查寻数据、遍历数据, 所以 ArrayList 是最常用的集合.

## LinkedList ##
- java.util.LinkedList 集合 implement List 接口
- 底层是一个链表结构: 查询慢, 而增删快.
- 注意: 使用LinkedList集合特有的方法, 不能使用多态.

- public void addFirst(E e): 将指定元素插入此列表的开头。
- public void push(E e): 将元素推入此列表所表示的堆栈。
	- 换句话说，将该元素插入此列表的开头。 
	- 此方法等效于 addFirst(E)。 
- public void addLast(E e): 将指定元素添加到此列表的结尾。
	- 等效于 add() 方法

- public E getFirst(): 返回此列表的第一个元素。
- public E getLast(): 返回此列表的最后一个元素。
- public void clear(): 从此列表中移除所有元素。

- public E removeFirst(): 移除并返回此列表的第一个元素。 
- public E pop(): 从此列表所表示的堆栈处弹出一个元素。
	- 换句话说，移除并返回此列表的第一个元素。
	-  此方法等效于 removeFirst()。
- public E removeLast(): 移除并返回此列表的最后一个元素。
 
- public boolean isEmpty(): 如果此 列表 不包含元素，则返回 true。 

		package demo;
		
		import java.util.ArrayList;
		import java.util.Iterator;
		import java.util.LinkedList;
		import java.util.List;
		
		public class demoList {
		    public static void main(String[] args) {
		        LinkedList<String> linked = new LinkedList<>();
		
		        linked.add("a");
		        linked.add("b");
		        linked.add("c");
		        linked.add("d");
		        System.out.println(linked);
		
		        //linked.addFirst("addFirst");
		        //linked.push("push");
		        //System.out.println(linked);
		        //linked.addLast("addLast");
		        //System.out.println(linked);
		
		        //linked.clear();
		        //System.out.println(linked.getFirst());
		        //System.out.println(linked.getLast());
		        /*
		        if (!linked.isEmpty()){
		            System.out.println(linked.getFirst());
		            System.out.println(linked.getLast());
		        }
		        */
		
		        //System.out.println(linked.removeFirst());
		        System.out.println(linked.pop());
		        System.out.println(linked.removeLast());
		        System.out.println(linked);
		
		    }
		}

## Vector  ##
- Vector 类可以实现可增长的对象数组。
-  已经被 ArrayList 取代
- 与 ArrayList 实现不同，Vector 是同步的(单线程)。

----------
# java.util.Set 接口 extends Collection 接口 #
**set 接口的特点:**

1. 不允许存储重复的元素
2. 没有索引, 没有带索引的方法, 也没有不能使用普通的for循环遍历.

## java.util.HashSet 集合 implement Set 接口 ##
**HashSet特点:**

1. 不允许存储重复的元素
2. 没有索引, 没有带索引的方法, 也没有不能使用普通的for循环遍历.
3. 是一个无序的集合, 存储元素和取出元素的顺序有可能不一致
4. 底层是一个哈希表结构(查询的速度非常的快)

		package demo;
		
		import java.util.HashSet;
		import java.util.Iterator;
		import java.util.Set;
		
		public class demoSet {
		    public static void main(String[] args) {
		        Set<Integer> set = new HashSet<>();
		        set.add(1);
		        set.add(2);
		        set.add(3);
		        set.add(1); // 不允许存储重复元素
		        //Sytem.out.println(set);
		
		        // 使用迭代器遍历
		        Iterator<Integer> it = set.iterator();
		        while(it.hasNext()) {
		            System.out.println(it.next());
		        }
		
		        // 增强for遍历
		        for (Integer integer : set) {
		            System.out.println(integer);
		        }
		    }
		}

## 哈希值 ##
- 一个十进制的数, 由系统随机给出
- 就是对象的一个地址值, 
- 是一个逻辑地址, 模拟出来的地址, 不是数据实际存储的物理地址
- 在Object类有一个方法, 可以获取对象的哈希值 --> int hashCode() : 返回该对象的哈希码值.


- Object类中的toString()源码

		public String toString() {
		        return getClass().getName() + "@" + Integer.toHexString(hashCode());
		}


## 红黑树 ##
- 二叉树
- 二叉排序树: 左小右大
- 平衡树: 左右孩子数量相等
- 红黑树: 
	- 特点: 趋近于平衡树, 查询的速度非常快, 查询叶子节点最大次数和最小次数不能超过2倍.
		- 2倍: 红黑树从根到叶子的最长路径不会超过最短路径的2倍
	- 约束: 	
		1. 节点可以是红色或者黑色的
		2. 根节点是黑色的
		3. 叶子结点(特指空节点)是黑色的
		4. 每个红色节点的子节点都是黑色的, 也就是说不可能出现两个连续的红色节点，不过两个连续的黑色节点是可能出现的
		5. 任何一个节点到其每一个叶子节点的所有路径上黑色节点数相同
- 红黑树是自平衡二叉树，每次插入新的数据的时候，会变色和旋转，来让高度保持在 log2 这个合高度内
	

## 哈希表 ##
- JDK1.8之前,哈希表是由数组+链表实现的,
- JDK1.8后,新增红黑树部分
- 特点: 查询速度快.
- 数据结构:
	- 把元素进行了分组(相同哈希值的元素是一组),
	- 链表/红黑树结构把相同哈希值的元素连接到一起.
	- 当链表树超过8的时候, 链表会转换为红黑树可以提高查询效率.

## 为什么不会存储重复元素? ##
- Set集合在调用add方法的时候, add方法会调用元素的hashCode方法和equals方法, 判断元素是否重复.
	- 通过hashCode判断哈希值是否重复
	- 通过equals判断内容是否重复
- 前提是: 存储的元素**必须重写hashCode方法和equals方法**.

- HashSet存储自定义类型元素 
	- 自定义类 Person, 重写了hashCode方法和equals方法

			package demo;
			
			import java.util.Objects;
			
			public class Person {
			    private String name;
			    private int age;
			
			    public Person(String name, int age) {
			        this.name = name;
			        this.age = age;
			    }
			
			    @Override
			    public String toString() {
			        return "Person{" +
			                "name='" + name + '\'' +
			                ", age=" + age +
			                '}';
			    }
			
			    @Override
			    public boolean equals(Object o) {
			        if (this == o) return true;
			        if (o == null || getClass() != o.getClass()) return false;
			        Person person = (Person) o;
			        return age == person.age &&
			                Objects.equals(name, person.name);
			    }
			
			    @Override
			    public int hashCode() {
			
			        return Objects.hash(name, age);
			    }
			
			    public String getName() {
			        return name;
			    }
			
			    public void setName(String name) {
			        this.name = name;
			    }
			
			    public int getAge() {
			        return age;
			    }
			
			    public void setAge(int age) {
			        this.age = age;
			    }
			}

- 测试代码:

			package demo;
			
			import java.util.HashSet;
			import java.util.Iterator;
			import java.util.Set;
			
			public class demoSet {
			    public static void main(String[] args) {
			        /*
			        * 要求: 同名同龄的人, 只存储一次.
			        * */
			        HashSet<Person> set = new HashSet<>();
			        Person p1 = new Person("靓女", 18);
			        Person p2 = new Person("靓女", 18);
			        Person p3 = new Person("靓仔", 25);
			        set.add(p1);
			        set.add(p2);
			        set.add(p3);
			        for (Person person : set) {
			            System.out.println(person + "   " + person.hashCode());
			        }
			        System.out.println(p1 == p2); //比较两个对象的地址值
			        System.out.println(p1.equals(p2)); // 重写后比较属性值
			    }
			}

- 运行结果:

			Person{name='靓女', age=18}   37939027
			Person{name='靓仔', age=25}   37854745
			false
			true
			
			Process finished with exit code 0

----------
## java.util.LinkedHashSet 集合 extends HashSet 集合 ##
**LinkedHashSet 集合特点:**

- 底层是一个哈希表 (数组+链表/红黑树) + **链表**: 多了一条链表 (记录元素的存储顺序), 保证元素有序.
- 也不允许重复


