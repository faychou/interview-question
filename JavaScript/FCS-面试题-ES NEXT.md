# 面试题-ES NEXT

#### 4.1 `let`、`var`、`const` 有什么区别？

- `var` 是 `es5` 的语法，`let`、`const` 是 `es6` 语法
- `var` 有变量提升
- `var` 和 `let` 一般用于声明变量，`const` 声明的是常量
- `let`、`const` 有块级作用域，`var` 没有
- 使用 `var` 可以重复声明变量，但是 `let` 不允许在同一块作用域内重复声明同一个变量
- `const` 声明的变量必须经过初始化，且基本类型数据值不可修改



#### 4.3 如何通过 `promise` 解决以下的回调地狱的问题？

```js
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