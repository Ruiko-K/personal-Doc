# nodejs部分基础 #

## 1. js部分 ##
### 1.1. 旧版js ###
#### 1.1.1.对象，作用域概念 #### [(参考资料)对象，作用域概念][1]
js中，创建对象的方法有2种

```
eg1:
var Vehicle = function () {
  this.price = 1000;
};

eg2:
function Vehicle () {
  this.price = 1000;
};

var v = new Vehicle();
```
js的创建象都是引用对象，通过new付给变量v的是Vehicle对象实例的引用地址。     
通过new关键字创建的出的实例，默认返回的是this对象。所以引出了this作用域的概念。
```
function Vehicle () {
    this.price = 1000;
  };
var v = new Vehicle();

console.log(Vehicle.price); //undefined
console.log(v.price);   //1000
```
如果没有通过new关键字，直接调用原始对象的this，则this指向的是原始对象所在的运行对象，也就是window对象。上述结果中。直接调用Vehicle.prices，this则指向Vehicle的运行环境对象-window对象，而window对象没有price属性，所以出现了undefined的结果

##### 避免多层 this
由于this的指向是不确定的，所以切勿在函数中包含多层的this
```
var o = {
  f1: function () {
    console.log(this);
    var f2 = function () {
      console.log(this);
    }();
  }
}

o.f1()
// Object
// Window
```
上面代码包含两层this，结果运行后，第一层指向对象o，第二层指向全局对象，因为实际执行的是下面的代码。
```
var temp = function () {
  console.log(this);
};

var o = {
  f1: function () {
    console.log(this);
    var f2 = temp();
  }
}
```
一个解决方法是在第二层改用一个指向外层this的变量。

```
var o = {
  f1: function() {
    console.log(this);
    var that = this;
    var f2 = function() {
      console.log(that);
    }();
  }
}

o.f1()
// Object
// Object
```
##### 绑定 this 的方法
this的动态切换，固然为 JavaScript 创造了巨大的灵活性，但也使得编程变得困难和模糊。有时，需要把this固定下来，避免出现意想不到的情况。JavaScript 提供了call、apply、bind这三个方法，来切换/固定this的指向。

call方法可以改变this的指向，指定this指向对象obj，然后在对象obj的作用域中运行函数f   
call方法的参数，应该是一个对象。如果参数为空、null和undefined，则默认传入全局对象。
```
var n = 123;
var obj = { n: 456 };

function a() {
  console.log(this.n);
}

a.call() // 123
a.call(null) // 123
a.call(undefined) // 123
a.call(window) // 123
a.call(obj) // 456
```

a.call(obj) -> a() this = obj

#### 1.1.2.包·模块管理，IIFE概念 ##### 
##### 了解IIFE [(参考资料1)IIFE][2] [(参考资料2)][3] [参考资料3][4]

通常情况下，只对匿名函数使用这种“立即执行的函数表达式”。
它的目的有两个：

- 一是不必为函数命名，避免了污染全局变量；

- 二是IIFE内部形成了一个单独的作用域，可以封装一些外部无法读取的私有变量。

要理解立即执行函数(IIFE)，需要先理解一些函数的基本概念：函数声明、函数表达式
```
// 声明函数f1
function f1() {
    console.log("f1");
}
// 通过()来调用此函数
f1();
```
```
//函数表达式，被赋值给变量f2:
var f2 = function() {
    console.log("f2");
}
//通过()来调用此函数
f2();
```
函数表达式可以在表达式后加()达到立刻执行的效果，推导过程补充。参考如下：
```
var f2 = function(){
    console.log('IIFE输出！');
}();
```

通过IIFE，进而可以封装js的包和功能模块,并使模块拥有相应的私有属性和方法.
```
var module1 = (function () {
　var _count = 0;
　var m1 = function () {
　  //...
　};
　var m2 = function () {
　　//...
　};
　return {
　　m1 : m1,
　　m2 : m2
　};
})();
```

### 1.2. 新版js(es6)

****
## 2. nodejs部分 ##
### 2.1. nodejs概念(是什么，能做什么，与es6的关系)
#### 2.2. 安装，运行，vscode编译器
#### 2.2. 基础用法，语法介绍
****

## 3. webpack部分
### 3.1. webpack是什么，能做什么
### 3.2. npm介绍
### 3.2. 基本配置
****
## 4. 拓展
    4.1. vue,react,angularjs,ionic是什么，区别


  [1]: https://wangdoc.com/javascript/oop/index.html
  [2]: https://segmentfault.com/a/1190000003985390
  [3]: https://segmentfault.com/a/1190000003902899
  [4]: https://segmentfault.com/a/1190000003031456