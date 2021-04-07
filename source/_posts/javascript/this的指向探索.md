---
title: javascript中this指向探索
top: true
cover: false
toc: true
mathjax: true
date: 2021-01-27 15:27:31
password: false
summary: false
tags:
    - this
    - javascript
categories:
    - JavaScript
---

[comment]: <> (::: tip 书籍简介)

[comment]: <> (书名：我亦飘零久  )

[comment]: <> (作者：独木舟  )

[comment]: <> (分类：旅行随笔  )

[comment]: <> (第一次读于 2018.08.30  )

[comment]: <> (第二次读于 2019.07.22 至 2020.02.28  )

[comment]: <> (:::)
<!-- more -->

### 一、普通函数调用
1、使用let定义
```
// 普通函数调用
let username = "hello world"
function test(){
  console.log(this.username)
}
```
> 使用node运行，输出的结果是: undefined。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8b1f5477c2954fd38efb32cb4baf9ecb~tplv-k3u1fbpfcp-zoom-1.image)

> 使用html在谷歌浏览器中运行结果，输出的也是：undefined

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/703a3ee5807a4f749526a8096593fedf~tplv-k3u1fbpfcp-zoom-1.image)

> 总结：使用let定义全局的变量在函数内使用this指向为undefined

2、使用var定义
```
// 普通函数调用(使用var定义)
var username = "hello world"
function test(){
   console.log(this.username)
}
test();
```
> 使用node运行，输出的结果是：undefine

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c5be17839641405091cb93deca9ceb4d~tplv-k3u1fbpfcp-zoom-1.image)


> 使用谷歌浏览器中运行的结果是："hello world"

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4d7835ef5d814b6c9dab34663c4483fe~tplv-k3u1fbpfcp-zoom-1.image)

> ps: 大家会发现在node运行环境和浏览器运行环境下，this的指向不太相同；这是因为在node环境下 this指向的是全局global对象，在浏览器环境下this指向的是window对象。


>猜想：此处在浏览器环境下，var定义的全局变量会被window管控？而let定义的变量不会被window掌控。

验证：

1)、使用var 定义全局变量，在函数中打印window对象,如果在window对象中能找到定义的全局变量，就表示定义的全局变量被window接管了。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a9d271b212e44103acbeb495b49ebd46~tplv-k3u1fbpfcp-zoom-1.image)
此时我们打印发现，window对象有很多属性，我们搜索一下刚才定义的username全局变量

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c481a0d161cb43b08f036b1d02daf508~tplv-k3u1fbpfcp-zoom-1.image)
在全局属性中找到了username这个全局变量，表示我们的username这个全局变量被window接管了。既然window接管了username这个全局变量，那么我们就可以通过window.username打印该属性参数。
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ecee224a32ca47018ecca29553ab9162~tplv-k3u1fbpfcp-zoom-1.image)
那么this指向window,window接管了属性username，那么var定义的全局属性就可以通过this来获取了。

2）、同理：我们使用let来定义一个变量，打印window对象，看看数据是否被window接管。

![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7b00731e9c5943daab2c922055ab5cbc~tplv-k3u1fbpfcp-zoom-1.image)
搜索发现，username属性不存在于window对象中，原来，var定义的“全局“变量会被window接管，而let定义的变量不会。
> 注意：此处var是定义的全局变量，才会被接管（局部变量是不会被window对象接管的），跟作用域有关系。

> 我们都知道let ES6引入新的定义方式，它的用法类似于var，但是申明的变量，只在let命令所在的代码块内有效，也就是说它的作用域是有范围的，是块级作用域，不会指向全局作用域，所以不会被window接管，而且会有暂时性死区的约束。

> **暂时性死区：指的是只要块级作用域内存在let命令，它所声明的变量就绑定了这个区域，不在受外部影响。

> 结论：在浏览器环境下只有var定义的作用域为全局的变量才会被window接管，let定义的块级作用域或者var定义的局部变量都不会被window接管。普通函数中，因为this指向的是window对象，所以this可以获取到被window接管的全局变量。在node环境中，this指向的是全局global对象，var或者let定义的变量是不会被global对象接管,所以this获取不到定义的属性值。

ps: node环境下，需要使用global挂载属性，才能使用this打印出来。
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d813fa9ae4824a54885e19dc0c88508e~tplv-k3u1fbpfcp-zoom-1.image)

### 二、对象函数的调用
```
// 对象函数调用
let str = 'hello world'
let obj = {
    id: 123,
    test: function(){
        console.log(this.str);
        console.log(this.id)
    }
}
obj.test();
```
运行结果如下：
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/894862f0cec245318184ae61f846cb1d~tplv-k3u1fbpfcp-zoom-1.image)
打印this看一下
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f5dbb1bc5a99484b90e1e0fd75784bad~tplv-k3u1fbpfcp-zoom-1.image)

从图中不难看出，this指向的是该obj对象，所以上面的this.str为undefined。因为定义的str不属于obj对象的属性，所以this不能拿到指定的属性值。
***
***以下情况需要注意***
```
let str = 'hello world'
let obj = {
    id: 111,
}
let obj1 = {
    id: 123,
    test: function(){
        console.log(this.id);
    }
}
obj.test = obj1.test;
obj.test();
```
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ff998260495e4de5864c5da9d2f613d5~tplv-k3u1fbpfcp-zoom-1.image)
其实不难看出，因为obj1将函数赋值给了obj，那么对象obj就有了test函数，此时调用obj的test函数，this应该指向的是obj，那么this.id就是obj的属性id值，所以为111。
> 结论: 对象函数调用中，那个对象的函数调用this，this就指向那个对象。

### 三、构造函数调用

```
let constructorClass = function(name,age){
    this.name = name;
    this.age = age;
}
let obj1 = new constructorClass('张三', 18)
console.log(obj1);
```
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bbc58558cc1842008e62236ffd061762~tplv-k3u1fbpfcp-zoom-1.image)
此处的this指的是当前的构造函数的对象。

***注意：在构造函数里面返回一个对象，会直接返回这个对象，而不是指向构造函数后面创造的对象***
![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cd189415cf144a3881eeba5f01b6139d~tplv-k3u1fbpfcp-zoom-1.image)

> 在构造函数里面使用的this 指向的是当前对象

### 四、apply和call调用
1、apply和call 会改变传入函数的this指向。
```
let obj1 ={
    name: '张三'
}
let obj2 = {
    name: '李四',
    test: function(){
        console.log(this.name);
    }
}

obj2.test.call(obj1);
```

![](https://imgkr2.cn-bj.ufileos.com/674cbadc-e2d6-49b3-9d6b-760a74502f7a.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=fcPwLJE%252FRkjT0ngxUL%252FPfh9uIEo%253D&Expires=1609473050)
此时虽然是使用了obj2的方法，打印本来this是指向obj2的，但是使用了call动态的把this指向了obj1，所以此时打印的this.name 就相当于 obj1.name。

> call和apply的两个主要用途
> - 改变this的指向(把this从obj2指向obj1)
> - 方法借用(obj1没有test方法，只是借用了obj2方法)

> call 和 apply的区别
> - call 和 apply的作用，完全一样，唯一的区别就是在参数上面。
> - call 接收的参数不固定，第一个参数是函数体内this的指向，第二个参数以下是传入的参数。
> - apply接收两个参数，第一个参数也是函数体内的this指向，第二个参数是一个集合对象(数组或者类数组)

> bind和call、apply 一样，也是改变this指向的，他的传参方式和call一样，但是它返回的是一个函数，用于后面调用，而apply和call会直接执行。

### 五、箭头函数
> 箭头函数：
> - 出现的作用除了让函数的书写变得很简洁，可读性很好外；最大的优点是解决了this执行环境所造成的一些问题。比如：解决了匿名函数this指向的问题（匿名函数的执行环境具有全局性），包括setTimeout和setInterval中使用this所造成的问题。
> - 箭头函数里面，没有this,箭头函数里面的this是继承于外面的环境。
```
let obj = {
    str: 'hello',
    test: function(){
        setTimeout(function(){
            console.log(this.str);
        })
    }
}
obj.test();
```

![](https://imgkr2.cn-bj.ufileos.com/ca2b6790-ca91-44dd-a89f-7abbbf49cbe0.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=g%252B1ChG4lHePautdBPZvIBkMCrA0%253D&Expires=1609475712)
从前面演示的结果发现，在test函数中的this指向的是obj，但是传给setTimeout的是普通函数，this指向的是全局 window，window是没有改对象的str属性的，所以为undifined。

![](https://imgkr2.cn-bj.ufileos.com/63c275f7-78f1-4c9a-909e-600d294d9e9e.png?UCloudPublicKey=TOKEN_8d8b72be-579a-4e83-bfd0-5f6ce1546f13&Signature=lEi6AqesYY9Gsl8%252FFxV8BryrwOQ%253D&Expires=1609475924)
此时将setTimeout的普通函数改成箭头函数，然后箭头函数的this继承于外部的环境，也就是test函数的this指向，所以，test函数的this指向是obj，则setTimeout的箭头函数的this也是指向obj，所以可以找到str属性，输出hello。

> 结论：箭头函数里面，没有this,箭头函数里面的this是继承于外面的环境。

> 以上属于个人愚见，文章有不对的地方希望大家指出，共同改进，共同学习。

文章借鉴参考：   
https://www.cnblogs.com/chengxs/p/8679313.html,   
https://blog.csdn.net/qq_41485414/article/details/81481519,
https://www.cnblogs.com/fly_dragon/p/8669057.html
