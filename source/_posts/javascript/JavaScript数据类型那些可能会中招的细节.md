---
title: Javascript中数据类型那些可能会中招的细节
top: true
cover: false
toc: true
mathjax: true
date: 2021-01-27 15:27:31
password: false
summary: false
tags:
    - 数据类型
    - javascript
categories:
    - JavaScript
---

## 前言
Javascript的数据类型对于大家来说一点都不默认，主要基本数据局类型和引用数据类型，都是入门必学的知识点，而且在日常开发中，频繁使用。大家是否都掌握其中的一些细节呢？下面我们就详细探讨一下。

### 一、number类型注意事项

number类型包括：正数、负数、0、小数、NaN

> NaN：意思是not a number 不是一个有效数字，但是它是属于number类型的
> 
#### (1) = 和 == 和 ===的区别

- = 是赋值
- == 是判断左右两边的值是否相等(非严格判断，只要字面相等则相等)
- === 是判断左右两边是否相等,严格判断(包含数据类型,类型和字面相等才相等)

#### (2) NaN

- NaN 和 NaN 是不相等的,NaN == NaN返回的是false
- isNaN();检测一个值不是有效数字的命题是否成立，是有效数字则返回false，不是有效数字返回的才是true

> isNaN() 如果检测的值不是number类型，浏览器会默认把值转换为number类型，然后在判断是否为有效数字
 
```
 例如：
      console.log(isNaN("123"))  // 打印结果是 true
 步骤: 
    1、首先把"123"转换成number类型的,使用Number()转换方法
    2、然后判断number类型的值是否满足isNaN的条件
 ```

> - Number()方法 强制将其他数据类型转为number类型(强制数据类型转换)

```
  Number()方法 强制将其他数据类型转为number类型，
  要求：如果是字符串，字符串中一定都需要是数字才可以转换
  例如：Number("12")返回的结果是12，Number("12px")返回的结果就是NaN
 ```

> - 非强制数据类型转换 parseInt()/ parseFloat()
 
```
  parseInt: 从左到右，一个个字符查找，把是数字的转为有效数字，中途如果遇到了一个非有效数字，就不在继续查找了
  parseFloat: 和上面一样，可以多识别一个小数点。

  例如：parseInt('12px')的值为 12
       parseFloat('12.5px')的值为 12.5
 ```

### 二、数据类型的转换规则

> 常用的boolean转换符号

- **!**  一个感叹号是取反，首先将值转化为布尔类型的值，然后取反
- **!!**  两个感叹号是将其他的数据类型转换为  boolean 类型，相当于Boolean()

#### 转换规则：
##### 1、如果只有一个值，判断这个值是真还是假，遵循：只有 0 NaN "" null undefined 这五个是假，其余的都是真

```
例如：
    console.log(!3)  // false
    console.log(![]) // false
    console.log(!{}) // false
    
    console.log(!null) // true
    console.log(!0) // true
    console.log(!undefined) //true
    console.log(!"")) //true
```

> **注意：** 此处 数字0才为假，如果是字符串的'0',同样为真


```
例如：
  if(0){
    console.log("为真")
  }else{
    console.log("为假")
  }
  if('0'){
     console.log("为真")
  }else{
     console.log("为假")
  }
  
  第一个输出 为假，第二个输出 为真
```

##### 2、如果是两个值比较是否相等，遵循这个规则：

val1 == val2 两个值可能不是同一数据类型的，如果是 == 比较的话，会进行默认的数据类型转换
- 1)、对象 == 对象，永远不相等
- 2)、对象 == 字符串  现将对象转换为字符串(调用toString的方法)，然后在进行比较

```
  [] 转换为字符串 ""
  {} 转换为字符串 "[object Object]" 
  
  所以:  [] == "" 为 true
        {} == "" 为 false
```

- 3)、对象 == 布尔类型  对象先转为字符串(toString)，然后把字符串转换为数字(Number),布尔类型也转换为数字(true是1 false 是0)，最后让两个数字比较

```
    例如：
        console.log([] == false) // 为 true
        解析：首先 []转为字符串"",然后字符串转为数字类型number，Number("")结果为0，false
        转为数字类型，Number(false) 结果也为0，所以 [] == false, 就解析成了 0与0的比较，所以相等，
        返回true
```

- 4)、对象 == 数字 对象先转为字符串(toString)，然后把字符串转换为数字(Number)
- 5)、数字 == 布尔 布尔类型转换为数字
- 6)、数字 == 字符串，字符串转换为数字

```
  例如：5 == '5' // 为 true
```

- 7)、字符串 == 布尔  都转换为数字
- 8)、null === undefined 结果是true

```
    console.log(null == undefined) // true
```

- 9)、null和undefined 和其他任何数据类型都不相等

```
 console.log(null == 0) // false
 console.log(undefined == 0) // false
```

##### 3、除了== 是比较，===也是比较(绝对比较)，如果数据类型不一样肯定不相等

```
  例如：
      console.log(0 == false) // true
      console.log(0 === fasle) // false
      
      console.log(5 == "5") // true
      console.log(5 === "5") // false
      
      console.log(null == undefined) // true
      console.log(null === undefined) // false
```

### 三、typeof(数据类型检测)

> typeof 用来检测数据类型的，用法：typeof + 要检查的值,
返回一个字符串，包含了数据类型的字符( "number","string","boolean","undefined","function","object")

- **typeof null** 返回的结果是 **"object"**
- typeof undefined 返回的结果是 "undefined"

> 虽然null 和 undefined 同为number数据类型，但是通过typeof检测的值不是number，而且也不相同。

- typeof null 结果是 "object"
- typeof [] 结果是 "object"

>注意： 同为对象数据类型的 数组、正则、对象的检测类型都是"object"
 ****

>**typeof 局限性**：不能具体的检查object下细分的类型

```
  console.log(typeof typeof typeof typeof []) // "string"
  此处打印的是"string"
```

> tip：因为typeof 返回的值就是一个字符串，如果用到了两个以及两个以上的typeof 返回的都是 "string"类型

### 四、基本数据类型和引用数据类型的本质区别
例子：

```
情景一:
    var num1 = 12;
    var num2 = num1;
    num2++;
    console.log(num1);
   
    
情景二:
    var obj1 = {"name":"张三"};
    var obj2 = obj1;
    obj2.name = "李四";
    console.log(obj1.name);

    情景一打印的值为 12, 情景二打印的值为 "李四"
```

> 分析：基本数据类型的值是具体的值，此处的var num2=num1;就是将num1的值 12给num2，此时num2和num1的值同为12，但是和num1的num2的值互不相关，所以不论num2怎么变，num1都是12，都不会改变。引用数据类型，存储的是数据内存的地址，var obj1 = {"name":"张三"} 是开辟一个空间来存放{"name":"张三"}值，然后将obj1指向存储值的地址，此时，var obj2 = obj1,就是将obj1指向内存空间的地址赋值给obj2，两个都指向同一个内存地址，对应的同一个值。所以后面obj2更改内存空间里面name的值后，obj1的值也会改变。

### 写到最后
重新回顾基础的知识，发现以前很懵懂的知识似乎瞬间明白了许多，比如上述的引用数据类型指向的是内存空间的地址，相赋值操作后，操作赋值后的值会影响原有的值的结果。所以才有了日常生活中经常使用的深拷贝和浅拷贝，就是为了规避改变两个值互相影响的情况；以前都最顾着自己快速的去上手做项目，学习框架的内容，却忽视了这些最为基本的东西。当基础掌握的足够扎实的时候，也就会看什么都豁然开朗了。

> 个人博客：https://moonshinean.github.io/     
欢迎来访，希望大家一起加油一起努力，奔赴自己的目标。
