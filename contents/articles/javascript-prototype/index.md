---
title: javascript的任脉－原型prototype
date: 2014-11-14 00:15:05
template: article.jade
tags: javascript
---

自从[Brendan Eich](https://twitter.com/BrendanEich)于1995年发明了javascript以来 对这门语言的争论就没有停止过
虽然我不是很喜欢javascript这个名字（曾经第一版叫mocha 后来也叫过livescript 无论哪个名字都比现在高大上有木有 最近es6出来后又开始讨论做javascript的超集atscript了）
但是这门10天写成的语言目前仍然是github最热门的语言 没有之一 那么必然是有其过人之处的 即便有很多人诟病js的各种缺陷 但有两样东西是基本大家都点赞的 可谓js的任督二脉
其一是`原型链(prototype chain)` 其二是`作用域链(scope chain)`

<span class="more"></span>

首先我们来弹弹原型这个东西 后续还会再弹作用域 在弹原型之前 我先扔两个鸡蛋OO(object oriented) 怎么又是这个东西 心很累啊...
设计面向对象的老师傅说 世界就是由对象组成的 对象是由属性和行为组成的 所以你们编程也要取舍对象的属性和对象发生行为（没读利索的请再读一遍）
然后世界上就多了一堆面向对象的语言 但大多数都是基于类(class)继承的 什么叫基于类的继承？比如我有一个男人和一个女人 并且我发现这两个人有很多相同的地方
比如都有一个头 每天都要吃饭 当然也有不同的地方 比如...脑补去 于是我把他们的共性抽象出来创造了一个叫人类的类 让男人和女人都去继承他 那么相同的东西就只保存一份 很划算对不对
现在问题来了 人类是啥呢？是看不见摸不着的抽象概念 而我们都喜欢看得见摸得着的东西 与其让类去继承类 为什么不让活生生的对象直接去继承对象呢？javascript就是这么干的！

下面言归正传

###对象(object)
弹原型前 先说说对象 对象是一系列属性和方法的集合 js中除了基本类型string, number, boolean, undefined, null之外的都是对象 函数虽然感觉上不大像 但他也是对象 他的其中一个属性就是函数体
想想用new Function()来创建函数就明白了 所有的对象创建时都有一个默认的内部属性`[[prototype]]` 一些`现代浏览器`将其实现为`__proto__` 这个属性里面便存放了一个指向其构造函数原型的引用

###原型(prototype)
原型是构造函数的一个叫prototype的属性（你还记得函数也是对象吗 是对象当然有属性）是构造函数被创建的同时默认创建的

空对象的构造函数是Object 那么这个空对象的[[prototype]]就指向Object.prototype
```javascript
{}.__proto__ === Object.prototype // true

```

函数的构造函数是Function 那么这个函数的[[prototype]]就指向Function.prototype
```javascript
function foo() {};
foo.__proto__ === Function.prototype // true
```

当对象需要获取一个属性或调用一个方法的时候 首先他会在自己体内找这个属性或方法 如果找不到 他会去[[prototype]]指向的原型里找
你会觉得原型看上去没啥用啊 把所有的属性和方法都挂在构造函数上 每个对象就都能拿到 何必绕弯去原型那里拿呢？
现在问题来了 假如创建了100个对象 每个对象都从构造函数那里拿到了属性 这很合情合理 但每个对象也从构造函数那里拿到了一样一样的方法 同样的方法被保存了100份...百份...份 这就说不过去了 
原型对构造函数说 “不要急 你把那些共享的方法都放在我这里 等你的对象们要的时候来我这拿 我肯定积极给” 于是原型就保存了唯一一份共享的方法 和对象们你是风儿我是沙 缠缠绵绵到天涯去了
```javascript
{}.foo // undefined
Object.prototype.foo = 'foo';
{}.foo // 'foo'
```


###原型链(prototype chain)
现在 你可能已经觉得原型有点牛了 但问题来了 如果原型里也没有对象想要的东西呢？别忘了原型本身是个对象 也有[[prototype]]这个属性 如果没有显示赋值过的话 是指向Object.prototype的
下例中foo找不到bar属性但却能找到toString方法 原因是虽然在foo.__proto__也就是Foo.prototype里面找不到toString 但会继续沿着Foo.prototype.__proto__指向的Object.prototype去找
而toString方法就被藏在了Object.prototype里面
```
function Foo() {};
var foo = new Foo();
foo.bar === undefined // true
foo.toString !== undefined // true
```
foo寻找bar的路线是这样的 找了一圈直到碰到null 最后还是没找到
`foo -> Foo.prototype(foo.__proto__) -> Object.prototype(Foo.prototype.__proto__, foo.__proto__.__proto__) -> null(foo.__proto__.__proto__.__proto__)`
foo寻找toString的路线是一样的 最后是在Object.prototype中找到了toString
看foo后面跟的那一串东东 这就是传说中的原型链了 原型链是javascript继承的基础

欲知继承如何 且听下回分解

 