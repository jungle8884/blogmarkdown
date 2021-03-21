---
title: Java-Note13
categories:
  - Java
tags:
  - Collection
  - Iterator
  - Comparable
  - Comparator
date: 2020-10-19 16:11:14
author: Jungle

---
# 集合 #

## 集合与数组的区别 ##

- 数组的长度是固定的; 集合的长度是可变的.
- 数组中存储的是同一类型的元素, 可以存储基本数据类型值; 集合存储的都是对象, 而且对象的类型可以不一致. 开发当中一般当对象多的时候, 使用集合进行存储.

## 集合框架 ##

**子类共性抽取, 行成父类(接口).**

- Collection接口
	1. 定义的是所有单列集合中共性的方法
	2. 所有的单列集合都可以使用共性的方法
	3. 没有带索引的方法
		- List接口
			1. 有序的集合(存储和取出元素顺序相同)
			2. 允许存储重复的元素
			3. 有索引, 可以使用普通的for循环遍历
				- Vector集合
				- ArrayList集合
				- LinkedList集合
		- Set接口
			1. 不允许存储重复元素
			2. 没有索引(不能使用普通的for循环遍历)
			3. TreeSet和HashSet集合是无序的集合(存和取的顺序可能不一致), 而LinkedHashSet集合则是有序的集合.
				- TreeSet集合
				- HashSet集合
					- LinkedHashSet集合
- 集合框架的学习方式
	- 学习顶层: 学习顶层接口/抽象类中共性的方法, 所有的子类都可以使用.
	- 使用底层: 底层不是接口就是抽象类, 无法创建对象使用, 需要使用底层的子类创建对象使用.

## Collection ##
Collection是所有单列集合的父接口, 因此在Collection中定义了单列集合(List和Set)通用的一些方法, 这些方法可以用于操作所有的单列集合.

- public boolean add(E e): 把给定的对象添加到当前集合中.
- public void clear(): 清空集合中所有的元素.
- public boolean remove(E e): 把给定的对象在当前集合中删除.
- public boolean contains(E e): 判断当前集合中是否包含给定的对象.
- public boolean isEmpty(): 判断当前集合是否为空.
- public int size(): 返回集合中元素的个数.
- public Object[] toArray(): 把集合中的元素, 存储到数组中.


**创建集合对象, 可以使用多态:**
	
	比如:	Collection<String> coll = new  ArrayList<>();
			coll.add("...")
			...

----------
# Iterator迭代器 #

迭代: 即Collection集合元素的通用获取方式.

-  在取元素之前先要判断集合中有没有元素, 
- 如果有, 就把这个元素取出来, 
- 继续判断, 如果还有就再取出出来.
-  一直把集合中的所有元素全部取出, 这种取出方式专业术语成为迭代.


## Iterator接口 ##
常用方法如下:

- public E next(): 返回迭代的下一个元素
- public boolean hasNext(): 如果仍有元素可以迭代, 则返回true

Collection继承自Iterable, 可以成为 "foreach" 语句的目标:

- public interface Collection<E>extends Iterable<E>
	- 要使用Iterator迭代器, 要使用Collection接口中的一个方法 --> iterator().
		- Iterator<E> iterator() 
		- 返回在此 collection 的元素上进行迭代的迭代器.
	- public interface Iterable<T>实现这个接口允许对象成为 "foreach" 语句的目标。

**迭代器的使用步骤**

1. 使用集合中的 iterator() 获取迭代器对象, 使用 Iterator 接口接收(多态)
2. 使用 Iterator 接口中的方法 hasNext 判断还有没有下一个元素.
3. 使用 Iterator 接口中的方法 next 取出集合中的下一个元素.

	例子:
	Iterator<String> it = coll.iterator();
	while(it.hasNext()) {
		System.out.println(it.next());	
	}

## 增强for循环 ##

专门用来遍历数组和集合的, 它的内部原理其实是一个Iterator迭代器, 所以在遍历的过程中, 不能对集合中的元素进行增删操作.

**格式:**
	
	for(集合/数组的数据类型 变量名 : 集合名/数组名) {
		//写操作代码
	}

----------

# Comparator比较器

```
This is a functional interface and can therefore be used as the assignment target for a lambda expression or method reference.
```

该接口代表一个比较器，比较器具有可比性！

数组工具类和集合工具类中提供的sort方法sort就是使用Comparator接口来处理排序的

```
Arrays.sort(T[],Comparator<? super T> c);
Collections.sort(List<T> list,Comparator<? super T> c);
```

# Comparable和Comparator

-   Java中的排序是由Comparable和Comparator这两个接口来提供的。
-   Comparable表示可被排序的，实现该接口的类的对象自动拥有排序功能。
-   Comparator则表示一个比较器，实现了该接口的的类的对象是一个针对目标类的对象定义的比较器，一般情况，这个比较器将作为一个参数进行传递。

## Comparable

​	Comparable的中文意思就是可被排序的，代表本身支持排序功能。

​	只要我们的类实现了这个接口，那么这个类的对象就会自动拥有了可被排序的能力。

​	而且这个排序被称为类的自然顺序。

​	这个类的对象的列表可以被**Collections.sort**和**Arrays.sort**来执行排序。

```java
public interface Comparable<T> {
    public int compareTo(T o);
}
```

 从源码中可以看到，该接口只有一个抽象方法**compareTo**，这个方法主要就是为了定义我们的类所要排序的方式。compareTo方法用于比较当前元素a与指定元素b，结果为int值，如果a > b，int>0；如果a=b，int=0；如果a<b，int<0。

## Comparator

Comparator中文译为比较器，它可以作为一个参数传递到**Collections.sort**和**Arrays.sort**方法来指定某个类对象的排序方式。

同Comparable类似，指定比较器的时候一般也要保证比较的结果与equals结果一致。

```java
@FunctionalInterface
public interface Comparator<T> {
    // 唯一的抽象方法，用于定义比较方式（即排序方式）
    // o1>o2，返回1；o1=o2，返回0；o1<o2，返回-1
    int compare(T o1, T o2);
    boolean equals(Object obj);
    // 1.8新增的默认方法：用于反序排列
    default Comparator<T> reversed() {
        return Collections.reverseOrder(this);
    }
    // 1.8新增的默认方法：用于构建一个次级比较器，当前比较器比较结果为0，则使用次级比较器比较
    default Comparator<T> thenComparing(Comparator<? super T> other) {
        Objects.requireNonNull(other);
        return (Comparator<T> & Serializable) (c1, c2) -> {
            int res = compare(c1, c2);
            return (res != 0) ? res : other.compare(c1, c2);
        };
    }
    // 1.8新增默认方法：指定次级比较器的
    // keyExtractor表示键提取器，定义提取方式
    // keyComparator表示键比较器，定义比较方式
    default <U> Comparator<T> thenComparing(
            Function<? super T, ? extends U> keyExtractor,
            Comparator<? super U> keyComparator)
    {
        return thenComparing(comparing(keyExtractor, keyComparator));
    }
    // 1.8新增默认方法：用于执行键的比较，采用的是由键对象内置的比较方式
    default <U extends Comparable<? super U>> Comparator<T> thenComparing(
            Function<? super T, ? extends U> keyExtractor)
    {
        return thenComparing(comparing(keyExtractor));
    }
    // 1.8新增默认方法：用于比较执行int类型的键的比较
    default Comparator<T> thenComparingInt(ToIntFunction<? super T> keyExtractor) {
        return thenComparing(comparingInt(keyExtractor));
    }
    // 1.8新增默认方法：用于比较执行long类型的键的比较
    default Comparator<T> thenComparingLong(ToLongFunction<? super T> keyExtractor) {
        return thenComparing(comparingLong(keyExtractor));
    }
    // 1.8新增默认方法：用于比较执行double类型的键的比较
    default Comparator<T> thenComparingDouble(ToDoubleFunction<? super T> keyExtractor) {
        return thenComparing(comparingDouble(keyExtractor));
    }
    // 1.8新增静态方法：用于得到一个相反的排序的比较器，这里针对的是内置的排序方式（即继承Comparable）
    public static <T extends Comparable<? super T>> Comparator<T> reverseOrder() {
        return Collections.reverseOrder();
    }
    // 1.8新增静态方法：用于得到一个实现了Comparable接口的类的比较方式的比较器
    // 简言之就是将Comparable定义的比较方式使用Comparator实现
    @SuppressWarnings("unchecked")
    public static <T extends Comparable<? super T>> Comparator<T> naturalOrder() {
        return (Comparator<T>) Comparators.NaturalOrderComparator.INSTANCE;
    }
    // 1.8新增静态方法：得到一个null亲和的比较器，null小于非null，两个null相等，如果全不是null,
    // 则使用指定的比较器比较，若未指定比较器，则非null全部相等返回0
    public static <T> Comparator<T> nullsFirst(Comparator<? super T> comparator) {
        return new Comparators.NullComparator<>(true, comparator);
    }
    // 1.8新增静态方法：得到一个null亲和的比较器，null大于非null，两个null相等，如果全不是null,
    // 则使用指定的比较器比较，若未指定比较器，则非null全部相等返回0
    public static <T> Comparator<T> nullsLast(Comparator<? super T> comparator) {
        return new Comparators.NullComparator<>(false, comparator);
    }
    // 1.8新增静态方法：使用指定的键比较器用于执行键的比较
    public static <T, U> Comparator<T> comparing(
            Function<? super T, ? extends U> keyExtractor,
            Comparator<? super U> keyComparator)
    {
        Objects.requireNonNull(keyExtractor);
        Objects.requireNonNull(keyComparator);
        return (Comparator<T> & Serializable)
            (c1, c2) -> keyComparator.compare(keyExtractor.apply(c1),
                                              keyExtractor.apply(c2));
    }
    // 1.8新增静态方法：执行键比较，采用内置比较方式，key的类必须实现Comparable
    public static <T, U extends Comparable<? super U>> Comparator<T> comparing(
            Function<? super T, ? extends U> keyExtractor)
    {
        Objects.requireNonNull(keyExtractor);
        return (Comparator<T> & Serializable)
            (c1, c2) -> keyExtractor.apply(c1).compareTo(keyExtractor.apply(c2));
    }
    // 1.8新增静态方法：用于int类型键的比较
    public static <T> Comparator<T> comparingInt(ToIntFunction<? super T> keyExtractor) {
        Objects.requireNonNull(keyExtractor);
        return (Comparator<T> & Serializable)
            (c1, c2) -> Integer.compare(keyExtractor.applyAsInt(c1), keyExtractor.applyAsInt(c2));
    }
    // 1.8新增静态方法：用于long类型键的比较
    public static <T> Comparator<T> comparingLong(ToLongFunction<? super T> keyExtractor) {
        Objects.requireNonNull(keyExtractor);
        return (Comparator<T> & Serializable)
            (c1, c2) -> Long.compare(keyExtractor.applyAsLong(c1), keyExtractor.applyAsLong(c2));
    }
    // 1.8新增静态方法：用于double类型键的比较
    public static<T> Comparator<T> comparingDouble(ToDoubleFunction<? super T> keyExtractor) {
        Objects.requireNonNull(keyExtractor);
        return (Comparator<T> & Serializable)
            (c1, c2) -> Double.compare(keyExtractor.applyAsDouble(c1), keyExtractor.applyAsDouble(c2));
    }
}
```

## 区别于联系：

1.  Comparable可以看做是内部比较器，Comparator可以看做是外部比较器。
2.  一个类，可以通过实现Comparable接口来自带有序性，也可以通过额外指定Comparator来附加有序性。
3.  二者的作用其实是一致的，所以不要混用。
4. **Comparable为可排序的，实现该接口的类的对象自动拥有可排序功能。**
5. **Comparator为比较器，实现该接口可以定义一个针对某个类的排序方式。**
6. **Comparator与Comparable同时存在的情况下，前者优先级高。**

**Comparator与Comparable来源：https://www.jianshu.com/p/f9870fd05958**

