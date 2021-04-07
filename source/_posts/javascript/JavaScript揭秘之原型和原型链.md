---
title: JavaScript揭秘之原型和原型链
top: true
cover: false
toc: true
mathjax: true
date: 2021-01-27 15:27:31
password: false
summary: false
tags:
    - 原型
    - javascript
    - prototyoe
categories:
    - JavaScript
---

## 前言
作为一名前端从业者，如果理不清JavaScript原型和原型链都显得不够专业，毕竟这是原生JavaScript的重难点和面试必考点，那么到底原型和原型链到底是什么呢？下面我们一起来谈谈吧。

> 开始之前我们先思考一下什么是原型链？究竟有什么特别之处呢？下面我分别为大家揭开它神秘的面纱

## 原型和原型链

首先我们看一段代码

```
 function Animal(){}
 
 let dog = new Animal();
 dog.name = 'black'
 console.log(dog.name) // 'black'
 
```

在这个代码中，我们创建了一个Animal的构造函数，然后通过构造函数new出来一个dog实例，那么此时的dog就是Animal的实例对象。


> 下面我们引入一个新的朋友：**prototype**


prototype是每一个函数都有的属性，它的作用是指向自己的原型，比如：

>上面的Animal构造函数，它有一个 prototype 属性，这个属性就是指向的它原型对象。
所以 Animial的原型就是 Animial.prototype


我们可以用一张图表示构造函数和实例原型之间的关系：


![](https://imgkr2.cn-bj.ufileos.com/e1fc6917-1168-430b-baa2-44b9db91284e.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=%252FgT7YQeTGMlzlTGrl15uDZrpWiQ%253D&Expires=1614339842)


既然Animial构造函数的原型 是 Animial.prototype，那它的实例dog的原型又是谁呢？

> 同样是： Animial.prototype, 可能你会好奇，暂时卖个关子，我们继续向下看，后面会一一给大家阐述为什么

我们看下一段代码

```
function Person(){}
// 此处我们用prototype属性在Person原型上创建一个属性name，值为Anny
Person.prototype.name = 'Anny'

// 然后我们实例化一个person

let person = new Person()

console.log(person.name) // 'Anny'

// 此时可以看出，我们并未在实例上定义name这个属性，为啥访问persone.name可以打印出原型的name属性值呢？

我们测试一下：

// 我们给person这个实例定义一个name属性，值为Sam
person.name = 'Sam'; 

console.log(person.name) // 'Sam' 此时我们打印的值为Sam

// 我们在删除person.name属性
delete person.name

console.log(person.name) // 'Anny' 此时值又成了 Anny了

通过上面的例子 我们发现，当实例中没有一个属性的时候，它就会从构造函数的 Person.prototype 原型中去寻找这个属性，然后将其的信息展示出来。

```

>从上面的例子看出，实例和构造函数拥有同一个原型 他们都是 Person.prototype


那么什么是**原型**呢？你可以这样理解：每一个JavaScript对象(null除外)在创建的时候就会与之关联另一个对象，这个对象就是我们所说的原型，每一个对象都会从原型"继承"属性


> 下面我们引入另一个新的朋友：**__ proto __**


这是每一个JavaScript对象(除了 null )都具有的一个属性，叫__proto__，这个属性会指向该对象的原型。

那么有了 **__ proto __** ，我们就知道我们实例的原型了,我们看下一段代码

```
  function Person(){}
  let person = new Person()
  
  // 我们通过 __proto__来指向person和Person的原型是不是同一个
  
  console.log(person.__proto__ == Person.prototype) // true
  
  // 由此可以看出，上面的说法是对的，person和Person的原型指向同一个，Person.prototype
```

得到以下关系图：

![](https://static01.imgkr.com/temp/58d7a4c1195a49aba1a306828e7acd8e.png)

既然实例对象和构造函数都可以指向原型，那么原型是否有属性指向构造函数或者实例呢？

> 指向实例倒是没有，因为一个构造函数可以生成多个实例，但是原型指向构造函数倒是有的，这就要讲到另一个属性：constructor，每个原型都有一个 constructor 属性指向关联的构造函数。

```
function Person(){}

console.log(Person === Person.prototype.constructor) // true

// Person.prototype 代表原型，Person.prototype.constructor代表原型的构造函数，所以上面的值为true
```

所以我们得到以下关系图

![](https://static01.imgkr.com/temp/591bf50b291946378d3f584d414cd479.png)

上面我们说到，实例读取实例的属性时，如果找不到，就会查找与对象关联的原型中的属性，那如果原型上也没有呢？那它会一直找，找原型的原型，一直找到最顶层为止。

### 原型的原型

在前面，我们已经说了，原型也是一个对象，既然是对象，那我们的原型的原型是不是Object的原型呢?下面我们看以下代码:

```
// 我们在Object的原型上定义一个name叫'Kangknag'
Object.prototype.name = 'kangkang'

// 定义一个Person构造函数
function Person(){}

// 在Person的原型上定义一个属性name 为 'Sam'
Person.prototype.name = 'Sam';

// 此时我们创建一个实例
let person = new Person() 

// 此处大家都知道，如果实例上没有name属性，回去它的原型上找，所以此处打印的是Sam
console.log(person.name) // 'Sam'
 
 // 此时我们删除Person上的属性name
 
 delete Person.prototype.name
 
 console.log(person.name) // 'KangKang'
 
 // 此时我们发现，当删除了构造函数Person的属性name后，我们在打印实例的值变成了'KangKang'，上面说道，实例的属性如果没有就会去原型上面找，如果原型上找不到就会去原型的原型上面去找，那此处的Person的原型就是Object.prototype，我们可以验证一下
 
 console.log(person.__proto__.__proto__ == Object.prototype) // true
 // 可以看出 person原型的原型就是Object.prototype
 
 
```

所以我们得到以下关系图

![](https://imgkr2.cn-bj.ufileos.com/f5fa8f4a-cfba-4f0d-8412-11a5bf599d59.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=sQU4vaF%252BOc%252FPR71zDyIvQqtjDmA%253D&Expires=1614391361)

person原型的原型就是Object.prototype，那么我们的Object.prototype的原型又是什么呢？
我们可以打印看一下：

```
 console.log(Object.prototype.__proto__) // null
```

那么null究竟代表了什么呢？，此处引用阮一峰老师的[《undefined与null的区别》](http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html)一文的说法：
![](https://static01.imgkr.com/temp/ea444fa2889145b490ca92cd35f080c9.png)

> null 表示"没有对象", 即该处不应该有值。

所以Object.prototype.__proto __ 的值为null跟Object.prototype没有原型其实表达了一个意思。null就是原型链的终点，所以一般查找到Object.prototype就可以停止查找了

此时我们可以得到一个关系

> person.__proto __ . __proto __ . __proto __ =  null;

那么什么我们就可以得出结论：

> 原型链：原型链就是从实例通过__proto __逐层向上链接原型形成的链条，叫做原型链。

所以我们可以得到以下关系图
![](https://static01.imgkr.com/temp/55c06ded7cf7404c8d096757779a0d64.png)

以上图中蓝色的箭头就是原型链。

> 大家都知道，其实原型链是JavaScript继承的核心，但是JavaScript 默认并不会复制对象的属性，相反，JavaScript 只是在两个对象之间创建一个关联，这样，一个对象就可以通过委托访问另一个对象的属性和函数，所以与其叫继承，委托的说法反而更准确些。


> 此处需要注意两个点：

- 当获取 person.constructor 时，其实 person 中并没有 constructor 属性,当不能读取到constructor 属性时，会从 person 的原型也就是 Person.prototype 中读取，正好原型中有该属性。

```
person.constructor === Person.prototype.constructor // true
```

- 其次是 __proto__ ，绝大部分浏览器都支持这个非标准的方法访问原型，然而它并不存在于 Person.prototype 中，实际上，它是来自于 Object.prototype ，与其说是一个属性，不如说是一个 getter/setter，当使用 obj.__proto__ 时，可以理解成返回了 Object.getPrototypeOf(obj)



> 那么 new 一个构造函数到底是做了什么操作呢？

new操作符新建了一个空对象，这个对象原型指向构造函数的prototype，执行构造函数后返回这个对象
- 创建一个空的对象
- 链接到原型
- 绑定this指向，执行构造函数
- 确保返回的是对象

### 文章最后

通过以上的知识梳理，是不是对原型和原型链有了更深的理解呢？是不是发现所谓的模棱两可的知识理解起来也不是那么难？对的，其实就是这样，只要知道它们的关系，发现原型链也不过如此。此文带有个人见解，如有错误还请大家指出，我及时更正。大家一起努力，加油。

> 我的博客：https://moonshinean.github.io/

文章借鉴：   
https://github.com/mqyqingfeng/Blog/issues/2   
http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html
