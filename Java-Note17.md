---
title: Java-Note17
categories:
  - Java
tags:
  - Collections
  - Comparable
  - Comparator
date: 2020-11-06 20:04:51
author: Jungle

---
# Collections 集合工具类#

**java.utils.Collections 是集合工具类, 用来对集合进行操作.**

- public static <T> boolean addAll(Collection<T> c, T... elements): 往集合中添加一些元素
- public static void shuffle(List<?> list): 打乱集合顺序
- public static <T> void sort(List<T> list): 将集合中元素按照默认规则排序
- public static <T> void sort(List<T> list, Comparator<? super T> c): 将集合中元素按照指定规则排序
	
- 注意:
	- sort(List<T> list) 使用前提是: 
		- 被排序的集合里存储的元素必须实现Comparable, 重写接口中的方法compareTo定义排序的顺序.
		- Comparable接口的排序规则:	 this - 参数 --> 升序; 反之, 降序.
	- sort(List<T> list, Comparator<? super T> c): 
		- 需要自定义排序规则
		- 相当于第三方裁判

----------
## Person 要重写Comparable<Person>中的compareTo(Person o)方法


	package demo;
	
	import java.util.Objects;
	
	public class Person implements Comparable<Person> {
	@Override
	public int compareTo(Person o) {
	    //return 0; //相同返回 0
	    //return this.getAge() - o.getAge(); //排序规则--升序排序
	    return o.getAge() - this.getAge(); //降序
	}
	
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
	            '}' + '\n';
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




----------
## 根据年龄排序


	package demo;
	
	import java.util.ArrayList;
	import java.util.Collections;
	import java.util.Comparator;
	
	public class demoCollections {
	    public static void main(String[] args) {
	        ArrayList<String> sList = new ArrayList<>();
	        Collections.addAll(sList, "a", "b", "c", "d");
	        Collections.shuffle(sList);
	        Collections.sort(sList);
	        System.out.println(sList);
	
	        ArrayList<Person> pList = new ArrayList<>();
	        pList.add(new Person("楼满风", 19));
	        pList.add(new Person("林水瑶", 16));
	        pList.add(new Person("寒千洛", 18));
	        pList.add(new Person("洛时秋", 17));
	        //Collections.sort(pList);
	        Collections.sort(pList, new Comparator<Person>() {
	            //重写比较规则 --> 升序
	            @Override
	            public int compare(Person o1, Person o2) {
	                return o1.getAge() - o2.getAge();
	            }
	        });
	        System.out.println(pList);
	    }
	}


----------
## 组合排序: 年龄+首字母


	package demo;
	
	import java.util.ArrayList;
	import java.util.Collections;
	import java.util.Comparator;
	
	public class demoCollections {
	    public static void main(String[] args) {
	        ArrayList<String> sList = new ArrayList<>();
	        Collections.addAll(sList, "a", "b", "c", "d");
	        Collections.shuffle(sList);
	        Collections.sort(sList);
	        System.out.println(sList);
	
	        ArrayList<Person> pList = new ArrayList<>();
	        pList.add(new Person("楼满风", 19));
	        pList.add(new Person("林水瑶", 16));
	        pList.add(new Person("寒千洛", 16));
	        pList.add(new Person("洛时秋", 17));
	        //Collections.sort(pList);
	        Collections.sort(pList, new Comparator<Person>() {
	            //重写比较规则 --> 升序
	            @Override
	            public int compare(Person o1, Person o2) {
	                int result = o1.getAge() - o2.getAge();
	                //组合排序 --> 当年龄相同时, 根据首字母(姓氏)排序
	                if (result == 0){
	                    result = o1.getName().charAt(0) - o2.getName().charAt(0);
	                }
	                return result;
	            }
	        });
	        System.out.println(pList);
	    }
	}

----------
