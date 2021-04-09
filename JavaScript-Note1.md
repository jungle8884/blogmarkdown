---
title: JavaScript-Note1
categories:
  - 前端
tags:
  - 前端
  - JavaScript
author:
  - Jungle
date: 2021-04-09 14:27
---

# 1、什么是JavaScript？

JavaScript（简称“JS”） 是一种具有函数优先的轻量级，解释型或即时编译型的[编程语言]。

JavaScript在1995年由[Netscape](https://baike.baidu.com/item/Netscape/2778944)公司的Brendan Eich，在网景导航者浏览器上首次设计实现而成。因为Netscape与[Sun](https://baike.baidu.com/item/Sun/69463)合作，Netscape管理层希望它外观看起来像Java，因此取名为JavaScript。

JavaScript的标准是[ECMAScript ](https://baike.baidu.com/item/ECMAScript /1889420)。截至 2012 年，所有浏览器都完整的支持ECMAScript 5.1，旧版本的浏览器至少支持ECMAScript 3 标准。2015年6月17日，ECMA国际组织发布了ECMAScript的第六版，该版本正式名称为 ECMAScript 2015，但通常被称为ECMAScript 6 或者ES2015。 

**ECMAScript**是一种由[Ecma国际](https://baike.baidu.com/item/Ecma国际)（前身为[欧洲计算机制造商协会](https://baike.baidu.com/item/欧洲计算机制造商协会/2052072)，European Computer Manufacturers Association）通过ECMA-262标准化的脚本[程序设计语言](https://baike.baidu.com/item/程序设计语言)。这种语言在[万维网](https://baike.baidu.com/item/万维网)上应用广泛，它往往被称为[JavaScript](https://baike.baidu.com/item/JavaScript)或[JScript](https://baike.baidu.com/item/JScript)，所以它可以理解为是JavaScript的一个标准,但实际上后两者是ECMA-262标准的实现和扩展。

最新版本已经到es6版本~

但是大部分浏览器还停留在es5代码上！开发环境与线上环境，版本不一致。

# 2、快速入门

## 2.1、引入JavaScript

1、内部引入

```javascript
<script>
	alert('hello jungle');    
</script>
```

2、外部引入

```javascript
<script src="js/test.js"></script>

<!-- test.js: --->
alert('hello jungle');
```

## 2.2、定义变量

```javascript
// 严格区分大小写！

// 变量类型 变量名 = 变量值
var num = 1;
alert(num);
// 在浏览器控制台打印变量
console.log(num);
```

## 2.3、数据类型

> 数字

```javascript
// js不区分小数和整数，Number
123 // 整数
123.1 // 小数
-99 // 负数
NaN // not a Number
Infinity //表示无线大
```



> 字符串

```javascript
// 普通字符串：
'abc' 
"abc"
```

- 多行字符串

```javascript
'use strict'
let msg = `hello
            world
            jungle`
alert(msg);
console.log(msg)
```

- 模板字符串

```javascript
let name = 'jungle'
let msg = `hello ${name}`
alert(msg);
console.log(msg)
```

- 字符串长度：msg.length
- 字符串内容不可变



> 布尔值

```javascript
true, false
```



> 逻辑运算

```javascript
&& 与
|| 或
!  非
```



> 比较运算符：Js缺陷，坚持不要使用 '==' ！

```javascript
= 
==	等于（类型不一样，值一样，也会判断为true）
===	绝对等于（类型一样，值一样，结果为true）
```

```javascript
NaN === NaN，这个与所有的数值都不想等，包括自己
只能通过 isNaN(NaN) 来判断这个数是否是 NaN
```



> 浮点数问题：

- 精度问题！

```javascript
console.log((1/3) === (1-2/3))
VM666:1 false
```

- 解决办法：

```javascript
console.log(Math.abs((1/3) - (1-2/3)) < 0.000001)
VM760:1 true
```



> null和undefined

```javascript
null：空
undefined：未定义
```



> 数组：存储数据（常见下标存取）
>
> - Java 数组是相同类型，而JavaScript 数组可以包含任意的数据类型
>
> - 数组用 [] 
> - 数组长度：arr.length
>   - 若给 arr.length赋值，数组大小就会发生变化~
>   - 若赋值过小，元素就会丢失
>   - 所以，尽量别这样操作！
> - slice() 类似与字符串中的 subString()
> - indexof()
> - push()：压入到尾部 （尾部为栈顶）
> - pop()：弹出尾部的一个元素（尾部为栈顶）
> - unshift()：压入到头部
> - shift()：弹出头部的一个元素
> - sort()
> - reverse()
> - concat()：拼接成一个新的数组！
> - join()：返回使用特定的字符串拼接后的字符串

```javascript
var arr = [1, 2, 3, 4, 5]
```

> 取数组下标越界后：

```javascript
undefined
```



> 对象
>
> - 对象用 {}
> - var 对象名 = {属性名1: 属性值1, ..., 属性名n: 属性值n}
> - 属性以键值对形式描述
> - 动态删减属性：delete person.name
> - 动态添加属性：person.gender = 'male'
> - 属性值是否在对象中：'age' in person
> - 对象自身是否拥有某个属性值：person.hasOwnProperty('age')

```javascript
var person = {name: "jungle", age: 26}
// 取值
person.age
26
person.name
"jungle"
```

- 注意：使用一个不存在的对象属性，不会报错，只会 undefined！



# 3、严格检查模式

> 'use strict'：预防JavaScript的随意性导致产生的一些问题。
>
> - 局部变量建议使用 let 去定义
> - 前提：IDEA 设置支持 ES6 语法

```javascript
'use strict'
 i = 1;
alert(i)
console.log(i)
```

> test.js:2 Uncaught ReferenceError: i is not defined
>     at test.js:2



# 4、循环

> for 循环

```javascript
for (let i = 0; i < 100; i++) {
	console.log(i)
}
```

> foreach 循环 （5.1引入）

```javascript
age.forEach(function(value){
	console.log(value)
})
```

> for ... in （建议使用for ... of，见后面）

```javascript
var age = [3, 4, 5]
for (var x in age) {
    console.log(x)
}
// 输出索引：
0
1
2

// var index in object
var age = [3, 4, 5]
for (var num in age) {
	if (age.hasOwnProperty(num)) {
        console.log(age[num])
    }
}
// 输出值
3
4
5
```



# 5、Map 和 Set

> Map

```javascript
var map = new Map([['tom', 100], ['jack', 90], ['jungle', 80]])
var name = map.get('tom');
console.log(name);
map.set('admin', 123456)

map
Map(4) {"tom" => 100, "jack" => 90, "jungle" => 80, "admin" => 123456}
[[Entries]]
0: {"tom" => 100}
1: {"jack" => 90}
2: {"jungle" => 80}
3: {"admin" => 123456}
size: (...)
__proto__: Map
clear: ƒ clear()
constructor: ƒ Map()
delete: ƒ delete()
entries: ƒ entries()
forEach: ƒ forEach()
get: ƒ ()
has: ƒ has()
keys: ƒ keys()
set: ƒ ()
size: (...)
values: ƒ values()
Symbol(Symbol.iterator): ƒ entries()
Symbol(Symbol.toStringTag): "Map"
get size: ƒ size()
__proto__: Object
```



> Set：无序不重复的集合

```
var set = new Set([3, 1, 2, 2, 2])
set.add(4)
set.delete(4)
console.log(set.has(3))

set
Set(3) {3, 1, 2}
[[Entries]]
0: 3
1: 1
2: 2
size: (...)
__proto__: Set
add: ƒ add()
clear: ƒ clear()
constructor: ƒ Set()
delete: ƒ delete()
entries: ƒ entries()
forEach: ƒ forEach()
has: ƒ has()
keys: ƒ values()
size: (...)
values: ƒ values()
Symbol(Symbol.iterator): ƒ values()
Symbol(Symbol.toStringTag): "Set"
get size: ƒ size()
__proto__: Object
```



# 6、iterator

```javascript
var arr = [3, 4, 5]
for (var x of arr) {
    console.log(x)
}
// 输出值
3
4
5

var map = new Map([['tom', 100], ['jack', 90], ['jungle', 80]])
for (let m of map) {
    console.log(m)
}
// 输出值
["tom", 100]
["jack", 90]
["jungle", 80]



var set = new Set([3, 1, 2, 2, 2])
for (let s of set) {
    console.log(s)
}
// 输出值
3
1
2
```