---
title: Stream
categories:
  - 后端
  - Java
tags:
  - Stream
author:
  - Jungle
date: 2023-03-24 09:42:49
---



# Stream学习及使用

> Java中的集合（Collection）是一组对象的容器，而Java 8引入了Stream API，提供了一些强大的操作集合的方法。使用Stream可以对集合进行函数式编程风格的操作，使用Lambda表达式可以使代码更简洁、易读。下面是一些常用的Stream操作及其Lambda表达式的示例：

## 1. filter() 方法：过滤集合中的元素

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");
List<String> filteredNames = names.stream()
                                  .filter(name -> name.length() > 4)
                                  .collect(Collectors.toList());

```

上面的代码将过滤掉长度小于等于4的名字，输出结果为`[Alice, Charlie]`。



## 2. map() 方法：对集合中的每个元素应用一个函数，得到一个新的元素

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4);
List<Integer> squares = numbers.stream()
                                .map(number -> number * number)
                                .collect(Collectors.toList());

```

上面的代码将集合中的每个元素都平方，并返回一个新的集合`[1, 4, 9, 16]`。



## 3. sorted() 方法：对集合中的元素进行排序

```java
List<Integer> numbers = Arrays.asList(3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5);
List<Integer> sortedNumbers = numbers.stream()
                                      .sorted()
                                      .collect(Collectors.toList());

```

上面的代码将集合中的元素升序排列，输出结果为`[1, 1, 2, 3, 3, 4, 5, 5, 5, 6, 9]`。



## 4. distinct() 方法：去除集合中的重复元素

```java
List<Integer> numbers = Arrays.asList(3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5);
List<Integer> distinctNumbers = numbers.stream()
                                         .distinct()
                                         .collect(Collectors.toList());

```

上面的代码将集合中的重复元素去除，输出结果为`[3, 1, 4, 5, 9, 2, 6]`。



## 5. limit() 方法：获取集合中的前n个元素

```java
List<Integer> numbers = Arrays.asList(3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5);
List<Integer> limitedNumbers = numbers.stream()
                                        .limit(5)
                                        .collect(Collectors.toList());

```

上面的代码将集合中的前五个元素取出，输出结果为`[3, 1, 4, 1, 5]`。



## 6. skip() 方法：跳过集合中的前n个元素

```java
List<Integer> numbers = Arrays.asList(3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5);
List<Integer> skippedNumbers = numbers.stream()
                                        .skip(5)
                                        .collect(Collectors.toList());

```

上面的代码将集合中的前五个元素跳过，输出结果为`[9, 2, 6, 5, 3, 5]`。



## 7. forEach() 方法：对集合中的每个元素执行操作

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4);
numbers.stream()
        .forEach(number -> System.out.print(number + " "));

```

上面的代码将集合中的每个元素输出，输出结果为`1 2 3 4`。

---



## 8. reduce() 方法：将集合中的元素组合起来，得到一个结果

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4);
int sum = numbers.stream()
                 .reduce(0, (a, b) -> a + b);

```

上面的代码将集合中的元素相加，输出结果为`10`。

> 详细解释：

### 加法

> 当我们需要将一个集合中的元素组合起来得到一个结果时，可以使用`reduce()`方法。`reduce()`方法接收两个参数，第一个参数是初始值，第二个参数是一个Lambda表达式，用于指定如何组合集合中的元素。
>
> Lambda表达式接收两个参数，第一个参数表示已经组合好的结果，第二个参数表示需要组合的元素。Lambda表达式需要返回一个新的组合结果。例如，如果我们需要将一个集合中的所有元素相加，可以使用以下代码：

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4);
int sum = numbers.stream()
                 .reduce(0, (a, b) -> a + b);

```

在上面的代码中，`reduce()`方法的第一个参数是初始值，这里我们指定为0。Lambda表达式`(a, b) -> a + b`用于将集合中的元素相加，第一个参数`a`表示已经组合好的结果，第二个参数`b`表示需要组合的元素。Lambda表达式返回一个新的组合结果，即`a + b`。整个代码的作用是将集合中的所有元素相加，得到最终的结果。

### 乘法

> 在Lambda表达式中，我们还可以使用其他的组合操作。例如，如果我们需要将一个集合中的所有元素相乘，可以使用以下代码：

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4);
int product = numbers.stream()
                     .reduce(1, (a, b) -> a * b);

```

在上面的代码中，`reduce()`方法的第一个参数是初始值，这里我们指定为1。Lambda表达式`(a, b) -> a * b`用于将集合中的元素相乘，第一个参数`a`表示已经组合好的结果，第二个参数`b`表示需要组合的元素。Lambda表达式返回一个新的组合结果，即`a * b`。整个代码的作用是将集合中的所有元素相加，得到最终的结果。

### 其他：

> 当需要将一个集合中的所有元素汇总为一个值时，可以使用 Stream API 中的 reduce() 方法。reduce() 方法接收一个初始值和一个 BinaryOperator<T> 类型的函数作为参数，然后将初始值和集合中的元素一个个传递给这个函数，最终得到一个结果。

reduce() 方法有两种重载形式：

1. `Optional<T> reduce(BinaryOperator<T> accumulator)`：对集合中的元素进行归约操作，返回一个 Optional 对象，如果集合为空则返回一个空的 Optional 对象，否则返回一个包含归约结果的 Optional 对象。
2. `T reduce(T identity, BinaryOperator<T> accumulator)`：对集合中的元素进行归约操作，返回归约结果。

下面是一个求和的例子：

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4);
int sum = numbers.stream().reduce(0, (a, b) -> a + b);
System.out.println(sum); // 输出 10

```

在这个例子中，初始值是 0，BinaryOperator 函数是 (a, b) -> a + b。reduce() 方法将集合中的第一个元素和初始值相加得到 1，然后将 1 和第二个元素相加得到 3，以此类推，最终得到了 10。

reduce() 方法还可以使用方法引用来传递 BinaryOperator 函数。

例如，上面的代码也可以写成下面这样：

```java
int sum = numbers.stream().reduce(0, Integer::sum);

```

> 这里使用了 Integer 类中的 sum() 方法，它是一个静态方法，用于将两个 int 类型的数相加。在使用方法引用时，可以直接传递静态方法名，而不需要使用 lambda 表达式。

除了求和，reduce() 方法还可以用于求最大值、最小值、平均值等操作。例如，下面的代码用于求集合中的最大值：

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4);
Optional<Integer> max = numbers.stream().reduce(Integer::max);
System.out.println(max.orElse(0)); // 输出 4

```

在这个例子中，使用了 Integer 类中的 max() 方法，它是一个静态方法，用于比较两个 int 类型的数的大小，返回较大的那个数。使用 Optional 对象来包装结果可以避免 null 值的出现。如果集合为空，则返回一个空的 Optional 对象。使用 orElse() 方法可以在 Optional 对象为空时返回一个默认值。

---



## 9. anyMatch() 方法：判断集合中是否有任何一个元素满足给定的条件

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4);
boolean hasEven = numbers.stream()
                         .anyMatch(number -> number % 2 == 0);

```

上面的代码将判断集合中是否有任何一个偶数，输出结果为`true`。



## 10. allMatch() 方法：判断集合中是否所有元素都满足给定的条件

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4);
boolean allEven = numbers.stream()
                          .allMatch(number -> number % 2 == 0);

```

上面的代码将判断集合中是否所有元素都是偶数，输出结果为`false`。



## 11. noneMatch() 方法：判断集合中是否没有任何一个元素满足给定的条件

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4);
boolean noneOdd = numbers.stream()
                          .noneMatch(number -> number % 2 == 1);

```

上面的代码将判断集合中是否没有任何一个奇数，输出结果为`false`。



> 小结：这些操作只是Stream API中的一部分，还有许多其他的操作可以使用。通过组合这些操作，可以轻松地编写出复杂的数据处理代码。同时，使用Lambda表达式可以使代码更简洁、易读。
