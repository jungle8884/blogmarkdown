---
title: Java-Note18
categories:
  - Java
tags:
  - Map
date: 2020-11-09 20:12:02
author: Jungle

---
# java.util.Map<k,v>集合 #
**Map集合特点:**

1. Map集合是一个双列集合, 一个元素包含两个值(一个key, 另一个value);
2. Map集合中的元素, key和value的数据类型可以相同, 也可以不同;
3. Map集合中的元素, key是不允许重复的, value是可以重复的;
4. Map集合中的元素, key和value是一一对应的.

**常用方法:**

- public V put(K key, V value): 把指定的键与指定的值添加到Map集合中
	- 返回值: V
		- 存储键值对的时候, key不重复, 返回值V是null
		- 存储键值对的时候, key重复, 会用新的value替换map中重复的value, 返回被替换的value值
	- 示例:
		
				package demo;
				
				import java.util.HashMap;
				import java.util.Map;
				
				public class demoMap {
				    public static void main(String[] args) {
				        Map<String, String> map = new HashMap<>();
				        String v1 = map.put("周杰伦", "昆凌");
				        System.out.println(v1);
				        String v2 = map.put("周杰伦", "who?");
				        System.out.println(v2);
				        System.out.println(map);
				    }
				}
		
				输出:
					null
					昆凌
					{周杰伦=who?}
					
					Process finished with exit code 0

- public V remove(Object key): 把指定的键 所对应的键值对元素 在Map集合中删除, 返回被删除的值
	- 返回值: V
		- key存在, V返回被删除的值
		- key不存在, V返回null
	- 示例: 
	
		        Map<String, Integer> map1 = new HashMap<>();
		        map1.put("迪丽热巴", 165);
		        map1.put("杨幂", 168);
		        map1.put("林志玲", 178);
		        Integer v3 = map1.remove("林志玲");
		        System.out.println(v3);
		        Integer v4 = map1.remove("杨超越");
		        System.out.println(v4);
		        System.out.println(map1);
			
			输出:
				178
				null
				{迪丽热巴=165, 杨幂=168}
				
				Process finished with exit code 0

- public V get(Object key): 根据指定的键, 在Map集合中获取对应的值
	- 返回值V:
		- key存在, 返回对应的value值
		- key不存在, V返回null
	- 示例:

		        Map<String, Integer> map1 = new HashMap<>();
		        map1.put("迪丽热巴", 165);
		        map1.put("杨幂", 168);
		        map1.put("林志玲", 178);
		        Integer v3 = map1.remove("林志玲");
		        System.out.println(v3);
		        Integer v4 = map1.remove("杨超越");
		        System.out.println(v4);
		        System.out.println(map1);
		        System.out.println("-----------------------");
		        Integer v5 = map1.get("迪丽热巴");
		        System.out.println(v5);
		        Integer v6 = map1.get("林志玲");
		        System.out.println(v6);
		        System.out.println(map1);
		        System.out.println("-----------------------");
			输出:
				178
				null
				{迪丽热巴=165, 杨幂=168}
				-----------------------
				165
				null
				{迪丽热巴=165, 杨幂=168}
				-----------------------

				Process finished with exit code 0

- boolean containsKey(Object key): 判断集合中是否包含指定的键
	- 返回值: 包含返回true; 不包含返回false.
	- 示例:

			boolean b1 = map1.containsKey("迪丽热巴");
			System.out.println(b1);
			boolean b2 = map1.containsKey("迪丽热");
			System.out.println(b2);
			
			输出:
				true
				false

- public Set<K> keySet(): 获取Map集合中所有的键, 存储到Set集合中
	- 第一种遍历方法： 通过键找值的方式 
		- 实现步骤：
			1. 使用Map集合中的方法keySet()，把Map集合所有的key取出来，存储到一个Set集合中

					Map<String, Integer> map = new HashMap<>();
			        map.put("赵丽颖", 168);
			        map.put("杨颖", 165);
			        map.put("林志玲", 178);
			
			        Set<String> set = map.keySet(); //将map中所有的key取出来, 存储到一个Set集合中.
				
			2. 遍历Set集合，获取Map集合中的每一个key
			3. 通过Map集合中的方法get(key)，通过key找到value
				- 通过迭代器：
	
				        Iterator<String> it = set.iterator();
				        while (it.hasNext()){
				            String key = it.next();
				            Integer value = map.get(key);
				            System.out.println(key + "=" + value);
				        }		
				- 使用增强for循环：
								
						for (String key : map.keySet()) {
				            Integer value = map.get(key);
				            System.out.println(key + "=" + value);
				        }
			

- public Set<Map.Entry<K, V>> entrySet(): 获取到Map集合中所有的键值对对象的集合(Set集合) 
	- 在Map接口中有一个内部接口Entry
	- 当Map集合一创建，那么就会在Map集合中创建一个Entry对象，用来记录键值对对象
	- 实现步骤：
		1. 使用Map集合中的方法entrySet()，把Map集合中的多个Entry对象取出来，存储到一个Set集合中

				Set<Map.Entry<String, Integer>> entrySet = map.entrySet();			

		2. 遍历Set集合，获取每一个Entry对象
		3. 使用Entry对象中的方法getKey()和getValue()获取键与值
			- 迭代器：
				
					Iterator<Map.Entry<String, Integer>> it = entrySet.iterator();
			        while (it.hasNext()){
			            Map.Entry<String, Integer> entry = it.next();
			            String key = entry.getKey();
			            Integer value = entry.getValue();
			            System.out.println(key + "=" + value);
			        }

			- 增强for:

					for (Map.Entry<String, Integer> entry : entrySet) {
			            String key = entry.getKey();
			            Integer value = entry.getValue();
			            System.out.println(key + "=" + value);
			        }


----------
## java.util.HashMap<k,v>集合 implements Map<k,v>接口 ##
**HashMap集合的特点:**

1. HashMap集合底层是哈希表: 查询的速度特别快
	- JDK1.8之前: 数组+单向链表
	- JDK1.8之后: 数组+单向链表/红黑树(链表的长度超过8): 提高查询的速度
2. HashMap集合是一个无序的集合, 存储元素和取出元素的顺序有可能不一致
3. HashMap存储自定义类型键值
	1. Map集合保证key是唯一的：作为key的元素，必须重写hashCode方法和equals方法，以保证key唯一
	2. key: string类型
		- String类重写hashCode方法和equals方法，可以保证key唯一
	3. value: Person类型
		- value可以重复（同名同年龄的人视为同一个）
	4. 实现代码：
	
				package demo;
		
				import java.util.HashMap;
				import java.util.LinkedHashMap;
				import java.util.Set;
				
				public class demoHashMapSavePerson {
				    public static void main(String[] args) {
				
				        HashMap<String, Person> map = new HashMap<>();
				        map.put("北京", new Person("张三", 18));
				        map.put("上海", new Person("李四", 19));
				        map.put("广州", new Person("王五", 20));
				        map.put("深圳", new Person("李三", 18));
				        Set<String> set = map.keySet();
				        for (String key : set) {
				            Person value = map.get(key);
				            System.out.println(key + "=" + value);
				        }
				
				    }
				}
			
				
	5. 输出：

				上海=Person{name='李四', age=19}

				广州=Person{name='王五', age=20}
				
				北京=Person{name='张三', age=18}
				
				深圳=Person{name='李三', age=18}
				

----------
## java.util.LinkedHashMap<k,v>集合 extends HashMap<k,v>集合 ##
**LinkedHashMap集合的特点:**

1. LinkedHashMap集合底层是哈希表+链表(保证迭代的顺序)
2. LinkedHashMap集合是一个**有序的集合**, 存储元素和取出元素的顺序是一致的
3. LinkedHashMap存储自定义类型键值同上
4. 实现代码：


		package demo;
		
		import java.util.HashMap;
		import java.util.LinkedHashMap;
		import java.util.Set;
		
		public class demoHashMapSavePerson {
		    public static void main(String[] args) {
		
		        HashMap<String, Person> map = new LinkedHashMap<>();
		        map.put("北京", new Person("张三", 18));
		        map.put("上海", new Person("李四", 19));
		        map.put("广州", new Person("王五", 20));
		        map.put("深圳", new Person("李三", 18));
		        Set<String> set = map.keySet();
		        for (String key : set) {
		            Person value = map.get(key);
		            System.out.println(key + "=" + value);
		        }
		
		    }
		}

5. 输出： 

		北京=Person{name='张三', age=18}
		
		上海=Person{name='李四', age=19}
		
		广州=Person{name='王五', age=20}
		
		深圳=Person{name='李三', age=18}

----------
## java.util.Hashtable<K,V>集合 implement Map<K,V>接口 ##

- HashMap:底层是一个哈希表，是一个线程不安全的集合，是多线程的集合，速度快
	1. HashMap集合（也包括之前学过的所有集合）可以存储null值，null值。
- Hashtable: 底层也是一个哈希表，是一个线程安全的集合，是单线程集合，速度慢
	1. Hashtable集合不能存储null值，null值。
	2. Hashtable和Vector集合一样，在jdk1.2版本之后被更先进的集合（HashMap,ArrayList）取代了
	3. Hashtable的子类Properities依然活跃在历史舞台
	4. Properties集合是一个唯一的和IO流相结合的集合

- 实现代码：


		package demo;/*
		  @author: LeBro
		  @DESCRIPTION: 测试hashtable
		  @creteTime: 2020/11/18  
		*/
		
		import java.util.HashMap;
		import java.util.Hashtable;
		
		public class demoHashtable {
		    public static void main(String[] args) {
		
		        HashMap<String, String> map = new HashMap<>();
		        map.put(null, "a");
		        map.put("b", null);
		        map.put(null, null);
		        System.out.println(map);
		        System.out.println("-----------------------------");
		        Hashtable<String, String> table = new Hashtable<>();
		        //不允许存储null: Exception in thread "main" java.lang.NullPointerException
		        table.put(null, "a");
		        table.put("b", null);
		        table.put(null, null);
		        System.out.println(table);
		    }
		}



- 输出：

		Exception in thread "main" {null=null, b=null}
		-----------------------------
		java.lang.NullPointerException
			at java.base/java.util.Hashtable.put(Hashtable.java:480)
			at demo.demoHashtable.main(demoHashtable.java:21)

----------
