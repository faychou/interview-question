# JS 基础



## 3、JavaScript

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



#### 为什么要使用 TypeScript ? TypeScript 相对于 JavaScript 的优势是什么？



#### TypeScript 中 const 和 readonly 的区别？枚举和常量枚举的区别？接口和类型别名的区别？



#### TypeScript 中 any 类型的作用是什么？



#### TypeScript 中 any、never、unknown 和 void 有什么区别？



#### TypeScript 中 interface 可以给 Function / Array / Class（Indexable）做声明吗？



#### TypeScript 中可以使用 String、Number、Boolean、Symbol、Object 等给类型做声明吗？



#### TypeScript 中的 this 和 JavaScript 中的 this 有什么差异？



#### TypeScript 中使用 Unions 时有哪些注意事项？



#### TypeScript 如何设计 Class 的声明？



#### TypeScript 中如何联合枚举类型的 Key?



#### TypeScript 中 `?.`、`??`、`!.`、`_`、`**` 等符号的含义？



#### TypeScript 中预定义的有条件类型有哪些？



#### 简单介绍一下 TypeScript 模块的加载机制？



#### 简单聊聊你对 TypeScript 类型兼容性的理解？抗变、双变、协变和逆变的简单理解？



#### TypeScript 中对象展开会有什么副作用吗？



#### TypeScript 中 interface、type、enum 声明有作用域的功能吗？



#### TypeScript 中同名的 interface 或者同名的 interface 和 class 可以合并吗？



#### 如何使 TypeScript 项目引入并识别编译为 JavaScript 的 npm 库包？



#### TypeScript 的 tsconfig.json 中有哪些配置项信息？



#### TypeScript 中如何设置模块导入的路径别名？



## 4、ES NEXT

#### 4.1 `let`、`var`、`const` 有什么区别？

* `var` 是 `es5` 的语法，`let`、`const` 是 `es6` 语法
* `var` 有变量提升
* `var` 和 `let` 一般用于声明变量，`const` 声明的是常量
* `let`、`const` 有块级作用域，`var` 没有
* 使用 `var` 可以重复声明变量，但是 `let` 不允许在同一块作用域内重复声明同一个变量
* `const` 声明的变量必须经过初始化，且基本类型数据值不可修改

#### 4.2 手写用 `Promise` 加载一张图片。

``` js
function loadImg(src) {
  return new Promise(
    (resolve, reject) => {
      const img = document.createElement('img');
      img.onload = () => {
        resolve(img);
      }
      img.onerror = () => {
        const err = new Error(`图片加载失败 ${src}`);
        reject(err);
      }
      img.src = src;
    }
  );
}

const url = '';
loadImg(url).then(img => {
  console.log(img.width);
  return img;
}).then(img => {
  console.log(img.height)
}).catch(ex => console.error(ex));

```

#### 4.3 如何通过 `promise` 解决以下的回调地狱的问题？

``` js
// 回调地狱
// 获取第一个数据
$.get(url1, (data1) => {
  console.log(data1);
    
  // 获取第二个数据
  $.get(url2, (data2) => {
    console.log(data2);
        
    // 获取第三个数据
    $.get(url3, (data3) => {
      console.log(data3);
    });
  });
});

// Promise
function getData(url){
  return new Promise((resolve, reject) => {
    $.ajax({
      url,
      success(data) {
        resolve(data);
      },
      error(err) {
        reject(err);
      }
    });
  };
}

const url1 = '/data1.json';
const url2 = '/data2.json';
const url3 = '/data3.json';
getData(url1).then(data1=>{
  console.log(data1);
  return getData(url2);
}).then(data2 => {
  console.log(data2);
  return getData(url3);
}).then(data3 => {
  console.log(data3);
}).catch(err => console.error(err));
```

#### 4.4 介绍下 Set、Map 的区别。

Set

- 成员唯一、无序且不重复；
- [value, value]，键值与键名是一致的（或者说只有键值，没有键名）；
- 可以遍历，方法有：add、delete、has。

Map

- 本质上是键值对的集合，类似集合；
- 可以遍历，方法很多，可以跟各种数据格式转换。

#### 4.5 箭头函数跟普通函数的区别？

- 箭头函数是匿名函数，不能作为构造函数，不能使用 new
- 箭头函数不绑定 arguments，取而代之用 rest 参数 `（...rest）`
- 箭头函数不绑定 this，会捕获其所在的上下文的 this 值，作为自己的 this 值
- 箭头函数的 this 永远指向其上下文的 this ，任何方法都改变不了其指向，如 `call()` 、` bind()`、 `apply()`

#### 4.6 前端模块化的理解。



## 5、AJAX

#### 你知道哪些 `http` 头部



#### 你了解哪些请求方法，分别有哪些作用和不同



#### `options` 请求方法有什么用。



#### 你知道哪些状态码？



#### 同步和异步的区别是什么？

* 基于`js`是单线程语言；
* 异步不会阻塞代码的执行；
* 同步会阻塞代码的执行。

#### 前端使用异步的场景有哪些？

* 网络请求
* 定时任务

#### 实现原生 `AJAX`。

``` js
// get 请求
const xhr = new XMLHttpRequest();
xhr.open('GET','/api', true);
xhr.onreadystatechange = function() {
  // 这里的函数异步执行
  if (xhr.readyState === 4) {
    if(xhr.state === 200) {
      console.log(xhr.responseText);
    }
  }
}
xhr.send(null);

```

>  `xhr.readyState` 说明：

- 0为还没有调用 `send()` 方法
- 1为已调用 `send()` 方法，正在发送请求
- 2为 `send()` 方法执行完成，已经接收到全部响应内容
- 3为正在解析响应内容
- 4为响应内容解析完成，可以在客户端调用

#### 怎么与服务端保持连接



#### 什么是跨域、同源策略，你都知道哪些解决跨域的方法

同源就是：协议，域名，端口，一致。只要有一个不一样就会发生跨域。

解决方案：

* JSONP：script 标签的 src 属性不受同源策略限制，用此方式对非同源服务器请求资源，返回的 JS 代码会调用指定的函数，携带的参数就是所需的数据，这样就完成了跨域请求。

  ``` js
  let scriptDom = document.createElement("script");
  scriptDom.src="请求地址?callback=函数名";
  document.body.appendChild(scriptDom);
  ```

* CORS：跨域资源共享，是一种允许当前域（origin）的资源（比如html/js/web service）被其他域（origin）的脚本请求访问的机制。服务器端支持。

* nginx：是一款极其强大的 web 服务器，其优点就是轻量级、启动快、高并发。

#### 请说说浏览器的缓存策略。



#### 调式，抓包

chrome 调试工具：

* `Elements`

* `Network`

* `Console`

* `Application`

* `debugger`

移动端 h5 页，查看网络请求，可以使用 fiddler 工具来抓包， [fiddler 教程点击这里](https://www.cnblogs.com/yyhh/p/5140852.html)。

## 6、面向对象



## 7、Vue

#### 谈谈 `MV*` 的理解。

`MVVM` 是 `Model-View-ViewModel` 缩写，也就是把 `MVC` 中的 `Controller` 演变成 `ViewModel`，是 `View` 和 `Model` 层的桥梁，数据会绑定到 `viewModel` 层并自动将数据渲染到页面中，视图变化的时候会通知 `viewModel` 层更新数据。`Model` 层代表数据模型，`View` 代表 `UI` 组件。

#### Vue 的初始化，发生了什么？



#### Vue 模版编译原理知道吗，能简单说一下吗？

简单说，Vue 的编译过程就是将 `template` 转化为 `render` 函数的过程。

首先解析模版，生成 `AST语法树`(一种用 JavaScript 对象的形式来描述整个模板)。 使用大量的正则表达式对模板进行解析，遇到标签、文本的时候都会执行对应的钩子进行相关处理。

Vue 的数据是响应式的，但其实模板中并不是所有的数据都是响应式的。有一些数据首次渲染后就不会再变化，对应的 `DOM` 也不会变化。那么优化过程就是深度遍历 `AST树`，按照相关条件对树节点进行标记。这些被标记的节点(静态节点)我们就可以跳过对它们的比对，对运行时的模板起到很大的优化作用。

编译的最后一步是将优化后的 `AST树` 转换为可执行的代码。

#### Vue2.x 数据更新策略

Vue在初始化数据时，会使用 `Object.defineProperty` 重新定义 `data` 中的所有属性，当页面使用对应属性时，首先会进行依赖收集(收集当前组件的 `watcher` )，如果属性发生变化会通知相关依赖进行更新操作(发布订阅)。

#### **vue2.x中如何监测数组变化**

使用了函数劫持的方式，重写了数组的方法，Vue 将 `data` 中的数组进行了原型链重写，指向了自己定义的数组原型方法。这样当调用数组 `api` 时，可以通知依赖更新。如果数组中包含着引用类型，会对数组中的引用类型再次递归遍历进行监控。这样就实现了监测数组变化。

#### 你知道 Vue3.x 响应式数据原理吗？

Vue3.x 改用 `Proxy` 替代 `Object.defineProperty`。因为 `Proxy` 可以直接监听对象和数组的变化，并且有多达13种拦截方法。并且作为新标准将受到浏览器厂商重点持续的性能优化。

#### 说一下 Vue 的生命周期

`beforeCreate` 是 `new Vue()` 之后触发的第一个钩子，在当前阶段 `data`、`methods`、`computed` 以及 `watch` 上的数据和方法都不能被访问。

`created` 在实例创建完成后发生，当前阶段已经完成了数据观测，也就是可以使用数据，更改数据，在这里更改数据不会触发 `updated` 函数。可以做一些初始数据的获取，在当前阶段无法与 `Dom` 进行交互，如果非要想，可以通过 `vm.$nextTick` 来访问 `Dom`。

`beforeMount` 发生在挂载之前，在这之前 `template` 模板已导入渲染函数编译。而当前阶段虚拟 `Dom` 已经创建完成，即将开始渲染。在此时也可以对数据进行更改，不会触发 `updated`。

`mounted` 在挂载完成后发生，在当前阶段，真实的 `Dom` 挂载完毕，数据完成双向绑定，可以访问到 `Dom` 节点，使用 `$refs` 属性对 `Dom` 进行操作，一般会在这个阶段请求数据。

`beforeUpdate` 发生在更新之前，也就是响应式数据发生更新，虚拟 `Dom` 重新渲染之前被触发，你可以在当前阶段进行更改数据，不会造成重渲染。

`updated` 发生在更新完成之后，当前阶段组件 `Dom` 已完成更新。要注意的是避免在此期间更改数据，因为这可能会导致无限循环的更新。

`beforeDestroy` 发生在实例销毁之前，在当前阶段实例完全可以被使用，我们可以在这时进行善后收尾工作，比如清除计时器。

`destroyed` 发生在实例销毁之后，这个时候只剩下了 `Dom` 空壳。组件已被拆解，数据绑定被卸除，监听被移出，子实例也统统被销毁。

除了 `beforeCreate` 和 `created` 钩子之外，其他钩子均在服务器端渲染期间不被调用。

#### Vue中组件生命周期调用顺序说一下。

加载渲染过程：`父 beforeCreate -> 父 created -> 父 beforeMount -> 子 beforeCreate -> 子 created -> 子 beforeMount -> 子 mounted -> 父 mounted`。

子组件更新过程：`父 beforeUpdate -> 子 beforeUpdate -> 子 updated -> 父 updated`。

父组件更新过程：`父 beforeUpdate -> 父 updated`。

销毁过程：`父 beforeDestroy -> 子 beforeDestroy -> 子 destroyed -> 父 destroyed`。

#### 说一下 Computed 和 Watch

`Computed` 本质是一个具备缓存的 `watcher`，依赖的属性发生变化就会更新视图。 适用于计算比较消耗性能的计算场景。当表达式过于复杂时，在模板中放入过多逻辑会让模板难以维护，可以将复杂的逻辑放入计算属性中处理。

`Watch` 没有缓存性，更多的是观察的作用，可以监听某些数据执行回调。当我们需要深度监听对象中的属性时，可以打开 `deep：true` 选项，这样便会对对象中的每一项进行监听。这样会带来性能问题，优化的话可以使用 `字符串形式` 监听，如果没有写到组件中，不要忘记使用 `unWatch手动注销`。

#### 说一下 `v-if` 和 `v-show` 的区别

当条件不成立时，`v-if` 不会渲染 `DOM` 元素，`v-show` 操作的是样式 (`display`)，切换当前 `DOM` 的显示和隐藏。

#### 组件中的 `data` 为什么是一个函数？

一个组件被复用多次的话，也就会创建多个实例。本质上，这些实例用的都是同一个构造函数。如果 `data` 是对象的话，对象属于引用类型，会影响到所有的实例。所以为了保证组件不同的实例之间 `data` 不冲突，`data` 必须是一个函数。

#### 说一下 `v-model` 的原理

`v-model` 本质就是一个语法糖，可以看成是 `value + input` 方法的语法糖。 可以通过 `model` 属性的 `prop` 和 `event` 属性来进行自定义。原生的 `v-model`，会根据标签的不同生成不同的事件和属性。

#### Vue 事件绑定原理说一下

原生事件绑定是通过 `addEventListener` 绑定给真实元素的，组件事件绑定是通过 Vue 自定义的 `$on` 实现的。

#### `nextTick` 知道吗，实现原理是什么？

在下次 `DOM` 更新循环结束之后执行延迟回调。`nextTick` 主要使用了宏任务和微任务。根据执行环境分别尝试采用

- Promise
- MutationObserver
- setImmediate
- 如果以上都不行则采用 `setTimeout`

定义了一个异步方法，多次调用 `nextTick` 会将方法存入队列中，通过这个异步方法清空当前队列。

#### Vue 组件通信有哪些方式？

* 父子组件通信
  * 父 -> 子 `props`
  * 子 -> 父  `$on、$emit`
  * 获取父子组件实例 `$parent`、`$children`
  * ``Ref` 获取实例的方式调用组件的属性或者方法
  * `Provide`、`inject` 官方不推荐使用，但是写组件库时很常用

* 兄弟组件通信
  * Event Bus：`Vue.prototype.$bus = new Vue`

* 跨组件通信
  * Vuex

#### actions 和 mutations 有什么区别之类的。



#### 说一下虚拟 DOM

由于在浏览器中操作 DOM 是很昂贵的。频繁的操作 DOM，会产生一定的性能问题。这就是虚拟 DOM 的产生原因。

Virtual DOM 本质就是用一个原生的 JS 对象去描述一个 DOM 节点。是对真实 DOM 的一层抽象。

VirtualDOM 映射到真实 DOM 要经历 VNode 的 create、diff、patch 等阶段。

#### key 属性的作用

key 的作用是为了在 diff 算法执行时更快的找到对应的节点，提高 diff 速度。vue 和 react 都是采用 diff 算法来对比新旧虚拟节点，从而更新节点。

在 vue 的 diff 函数中，在交叉对比的时候，当新节点跟旧节点头尾交叉对比没有结果的时候，会根据新节点的 key 去对比旧节点数组中的 key，从而找到相应旧节点（这里对应的是一个 key => index 的 map 映射）。如果没找到就认为是一个新增节点。而如果没有 key，那么就会采用一种遍历查找的方式去找到对应的旧节点。

#### keep-alive 了解吗？

`keep-alive` 可以实现组件缓存，当组件切换时不会对当前组件进行卸载。

常用的两个属性 `include/exclude`，允许组件有条件的进行缓存。

两个生命周期 `activated/deactivated`，用来得知当前组件是否处于活跃状态。

keep-alive 的中还运用了 `LRU(Least Recently Used)` 算法。

#### hash 路由和 history 路由实现原理说一下。

hash 其实就是`location.hash` 的值实际就是 URL 中 `#` 后面的东西。

history 实际采用了 HTML5 中提供的 API 来实现，主要有 `history.pushState()` 和 `history.replaceState()`。

## 8、React





## 9、小程序



## 9、NodeJS

#### SSR 了解吗？

SSR 也就是服务端渲染，也就是将 Vue/React 在客户端把标签渲染成 HTML 的工作放在服务端完成，然后再把 HTML 直接返回给客户端。

SSR 有着更好的 SEO、并且首屏加载速度更快等优点。不过它也有一些缺点，比如我们的开发 Vue 项目时会受到限制，服务器端渲染只支持 `beforeCreate` 和 `created` 两个钩子，当我们需要一些外部扩展库时需要特殊处理，服务端渲染应用程序也需要处于 Node.js 的运行环境。还有就是服务器会有更大的负载需求。



#### node 如何处理错误的。



## 11、其他

#### 