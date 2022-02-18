# JS 基础

#### JS 数据类型有哪些？



#### `typeof` 能判断哪些类型？



#### 如何判断一个变量是不是数组？

``` js
// 方式一
function isArray(obj) {
  return Object.prototype.toString.call( obj ) === '[object Array]';
}

// 方式二：利用原型
function isArray(obj) {
  return obj.__proto__ === Array.prototype;
}
```



#### 举例强制类型转换？

两种不同的内置类型间的转换被称为强制转换，可以分为显式和隐式

显式强制转换，如：`Number`、`parseInt`、`parseFloat`、`toString` 等。

隐式强制转换：

``` js
var a = "42";
var b = a * 1; // "42" 隐式转型成 42 
```



#### null 和 undefined 的区别。

- 尚未初始化的东西：`undefined`
- 目前不可用的东西：`null`



#### 数组去重。

``` js
function unique(arr) {
  const set = new Set(arr);
  return [...set];
}
```



#### JavaScript 中的 const 数组可以进行 push 操作吗？为什么？



#### JavaScript 中对象的属性描述符有哪些？分别有什么作用？



#### Object.defineProperty 有哪几个参数？各自都有什么作用？



#### Object.defineProperty 和 ES6 的 Proxy 有什么区别？



#### ES6 中 Symbol、Map、Decorator 的使用场景有哪些？或者你在哪些库的源码里见过这些 API 的使用？



#### 如何在 JavaScript 中比较两个对象？

对于两个非原始值，比如两个对象（包括函数和数组），`==` 和 `===` 比较都只是检查它们的引用是否匹配，并不会检查实际引用的内容。



#### 解析 ['1', '2', '3'].map(parseInt)。

答案是 [1, NaN, NaN]，原因是：

``` js
parseInt(string, radix) // 接收两个参数，第一个表示被处理的值，第二个表示为几进制。

parseInt('1', 0)  // radix 为 0 时，且 string 参数不以“0x”和“0”开头时，按照10进制处理，返回 1；
parseInt('2', 1)  // 1 进制，无法解析，返回 NaN；
parseInt('3', 2)  // 2 进制，无法解析，返回 NaN。
```



#### 如何捕获 JS 程序中的异常？

``` js
try {
  // todo
} catch(error) {
  console.error(error)
} finally {
    
}
```



#### 什么是变量提升？



#### JS 的严格模式是什么？

`use strict` 出现在 JavaScript 代码的顶部或函数的顶部，可以帮助你写出更安全的 JavaScript 代码。如果你错误地创建了全局变量，它会通过抛出错误的方式来警告你。



#### `DOM` 操作的常用 `API`。



#### `attribute` 和 `property` 的区别。

* `property` 修改对象属性，不会体现到 `html` 结构中
* `attribute` 修改 `html` 属性，会改变 `html` 结构
* 两种都有可能引起 `DOM` 重新渲染



#### `window.onload` 和 `DOMContentLoaded` 的区别。

``` js
window.addEventListener('load', function() {
  // 页面的全部资源加载完才会执行，包括图片，视频等
});
document.addEventListener('DOMContentLoad', function() {
  // dom渲染完既可执行，此时图片，视频还可能没有加载完 
});

// ready 也是 dom 渲染完既可执行，图片，视频等还没有加载完
```



#### 创建10个标签，点击后弹出对应的序号。

``` js
// 基础
for(let i = 0; i < 10; i++) {
  let a = document.createElement('a');
  a.innerHTML = i + '<br>';
  a.addEventListener('click', function(e) {
    e.preventDefault();
    alert(i);
  });
  document.body.appendChild(a);
}
// 进阶：事件委托实现
```



#### 说说事件流吧，谁先执行？

事件流分为两种，捕获事件流和冒泡事件流。 捕获事件流从根节点开始执行，一直往子节点查找执行，直到查找执行到目标节点。 冒泡事件流从目标节点开始执行，一直往父节点冒泡查找执行，直到查到到根节点。

DOM事件流分为三个阶段，一个是捕获节点，一个是处于目标节点阶段，一个是冒泡阶段。



#### JavaScript 中的作用域是指什么？

在 JavaScript 中，每个函数都有自己的作用域。作用域基本上是变量以及如何通过名称访问这些变量的规则的集合。只有函数中的代码才能访问函数作用域内的变量。

同一个作用域中的变量名必须是唯一的。一个作用域可以嵌套在另一个作用域内。如果一个作用域嵌套在另一个作用域内，最内部作用域内的代码可以访问另一个作用域的变量。



#### `this` 有几种指向？

在 JavaScript 中，this 是指正在执行的函数的“所有者”.

普通函数中 `this` 取什么值，是在函数执行的时候确定的，不是函数定义的时候确定的。



#### `call`、`apply` 和 `bind` 的区别。



#### 闭包是什么，有什么特性，有什么负面影响，应用场景是什么？

闭包是在另一个函数（称为父函数）中定义的函数，并且可以访问在父函数作用域中声明和定义的变量。

闭包不要乱用，变量会常驻内容，不会释放。



#### `new Object()` 和 `Object.create()` 的区别。

* `{}` 等同 `new Object()`，原型为 `Object.prototype`；
* ``Object.create(null)` 没有原型；
* ``Object.create({...})` 指定原型。 



#### 什么是深浅拷贝，对应的实现方式。

``` js
// 深拷贝
function deepClone(obj = {}){
  if(typeof obj !== 'object' || obj == null) {
    // obj 是 null，或者不是对象情况，直接返回
    return obj;
  }
    
  // 初始化返回结果
  let result;
  if(obj instanceof Array) {
    result = [];
  } else {
    result = {};
  }
    
  for (let key in obj) {
    // 保证 Key 不是原型的属性
    if(obj.hasOwnProperty(key)) {
      // 递归调用
      result[key] = deepClone(obj[key]);
    }
  }

  // 返回结果
  return result;
}
```



#### 设计模式（功能、代码实现、使用场景）

单例模式、原型模式、工厂模式、观察者模式、策略模式、代理模式等等。



#### 什么是垃圾回收？

`JavaScript` 引擎中有一个后台进程称为垃圾回收器，它监视所有对象，并删除那些不可访问的对象（垃圾）。

我们在创建一个字符串、数组等都看作对象（不管是基本类型还是引用类型）都会为这个对象开辟一个内存空间来保存这个变量。如果访问不到这个对象的时候（没用了）就是垃圾。



#### 什么是内存泄露？

对于不再用到的内存如果没有及时释放就叫内存泄露。

以下几种情况会导致内存泄露：

- 意外的全局变量(严格模式解决)

- 被遗忘的定时器和回调函数

  * 解决方法： 在定时器完成工作的时候，手动清除定时器。`timeout=null`

- 脱离 DOM 的引用

  ``` js
  const div = document.getElementById('fa');
  document.body.removeChild(div); // 这里只是删除了 DOM
  div = null; // 还需要将 DOM 的引用变量 div 清除
  ```

- 闭包（变量引用指向 null 解决）



#### 什么是原型及原型链？

`JavaScript` 是一种基于原型的语言，每个函数在创建之后就会生成一个与之相关联的原型对象，即 `prototype`，这个原型对象中拥有一个 `constructor` 属性，该属性指向这个函数。

所有的对象都拥有 `__proto__` 属性，指向实例的原型。。

对象以它的原型作为模版、从原型上可以继承属性和方法。原型对象也是对象，也有它自己的原型对象，并从中继承属性和方法。一层一层，以此类推……

对象在访问属性或方法时，先检查自己的实例，如果存在就直接使用。如果不存在那么就去原型对象上去找，存在就直接使用，如果没有就顺着原型链一直往上查找，找到即使用，找不到就重复该过程直到原型链的顶端，如果还没有找到相应的属性或方法，就返回`undefined`，报错。



#### 简单对比一下 Callback、Promise、Generator、Async 几个异步 API 的优劣？



#### 什么是 event loop ？浏览器端的 event loop 和 node 端的 event loop 有什么区别？

[带你彻底弄懂Event Loop](https://segmentfault.com/a/1190000016278115)



