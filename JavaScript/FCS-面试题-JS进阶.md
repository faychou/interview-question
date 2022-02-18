# fcs-前端面试-进阶

#### JavaScript 是如何运行的？解释型语言和编译型语言的差异是什么？



#### 什么是函数式编程？什么是响应式编程？什么是函数响应式编程？



#### 类数组转化为数组的方法

``` js
const arrayLike=document.querySelectorAll('div')

// 1.扩展运算符
[...arrayLike]

// 2.Array.from
Array.from(arrayLike)

// 3.Array.prototype.slice
Array.prototype.slice.call(arrayLike)

// 4.Array.apply
Array.apply(null, arrayLike)

// 5.Array.prototype.concat
Array.prototype.concat.apply([], arrayLike)
```



#### 实现一个简易的模板引擎？



#### promise 限制并发数？



#### Promise 图片懒加载？

``` jsx
function loadImg (src) {
    var promise = new Promise(function (resolve, reject) {
        var img = document.createElement('img')
        img.onload = function () {
            resolve(img)
        }
        img.onerror = function () {
            reject('图片加载失败')
        }
        img.scr = src
    })
    retrun promise
}

const url = 'www.faychou.com';
loadImg(url).then(img => {
  console.log(img.width);
  return img;
}).then(img => {
  console.log(img.height)
}).catch(ex => console.error(ex));
```



#### ES6 Modules 相对于 CommonJS 的优势是什么？



#### 什么是前端路由，原理是什么？

[彻底弄懂前端路由](https://juejin.cn/post/6844903890278694919)

前端路由分为 hash 路由和 history 路由。hash 其实就是`location.hash` 的值实际就是 URL 中 `#` 后面的东西。

history 实际采用了 HTML5 中提供的 API 来实现，主要有 `history.pushState()` 和 `history.replaceState()`。


