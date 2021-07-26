# fcs-前端面试-进阶

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



#### 实现一个简易的模板引擎



#### promise 限制并发数



#### ES6 Modules 相对于 CommonJS 的优势是什么？



## 3、TypeScript

#### 你觉得typescript和javascript有什么区别



#### typescript你都用过哪些类型



#### typescript中type和interface的区别



## 5、框架

#### 手写一个简易的`jquery`，考虑插件和扩展性？

``` js
class jQuery {
    constructor(selector){
        const result = document.querySelectorAll(selector)
        const length = result.length
        for(let i=0; i<length; i++){
            this[i] = result[i]
        }
        this.length = length
    }
    get(index){
        return this[index]
    }
    each(fn){
        for(let i=0;i<this.length;i++){
            const elem = this[i]
            fn(elem)
        }
    }
    on(type,fn){
        return this.each(elem=>{
            elem.addEventListener(type,fn,false)
        })
    }
}

// 插件的扩展性
jQuery.prototype.dialog = function(info) {
    alert(info)
}

// 复写机制：
class myJQuery extends jQuery {
    constructor(selector) {
        super(selector)
    }
    // 扩展自己的方法
    study(){
        
    }
}

```



#### Vue 原理（手写代码，实现数据劫持）



#### vue-router 源码



#### Vue2.x 和 Vue3.x 渲染器的 diff 算法分别说一下。

简单来说，diff 算法有以下过程

- 同级比较，再比较子节点
- 先判断一方有子节点一方没有子节点的情况(如果新的 children 没有子节点，将旧的子节点移除)
- 比较都有子节点的情况(核心 diff)
- 递归比较子节点

正常 diff 两个树的时间复杂度是 `O(n^3)`，但实际情况下我们很少会进行 `跨层级的移动 DOM`，所以 Vue 将 diff 进行了优化，从 `O(n^3) -> O(n)`，只有当新旧 children 都为多个子节点时才需要用核心的 diff 算法进行同层级比较。

Vue2 的核心 diff 算法采用了`双端比较`的算法，同时从新旧 children 的两端开始进行比较，借助 key 值找到可复用的节点，再进行相关操作。相比 React 的 diff 算法，同样情况下可以减少移动节点次数，减少不必要的性能损耗，更加的优雅。

Vue3.x 借鉴了 `ivi算法` 和 `inferno算法`。在创建 VNode 时就确定其类型，以及在 `mount/patch` 的过程中采用`位运算`来判断一个 VNode 的类型，在这个基础之上再配合核心的 diff 算法，使得性能上较 Vue2.x 有了提升。该算法中还运用了`动态规划`的思想求解最长递归子序列。

#### 为什么请求放在 `useEffect` 里，放在外面和放里面有什么区别？在 `useEffect` 里想使用 `async/await` 怎么用？



## 6、NodeJS

#### 6.1 node 开启进程的方法有哪些，区别是什么



#### 你了解 node 多进程吗，node 中 cluster 是怎样开启多进程的，并且一个端口可以被多个进程监听吗？



#### node 进程中怎么通信？



#### 什么是调用堆栈？它是 V8 的一部分吗?

调用肯定是V8的一部分。它是V8用于保存函数调用轨迹的一种数据结构。每次我们运行一个函数，V8都会将该函数的引用放入调用堆栈，并对该函数中嵌套的其他函数进行相同的操作。这也包括递归调用的函数。

当嵌套的函数运行结束，V8将一次弹出一个函数并用它的返回值替换它的位置。

为什么这对于Node很重要？ 因为每个`Node`进程仅获得一个调用堆栈。 如果使该调用堆栈处于繁忙状态，则整个`Node`进程都处于繁忙状态。

## 7、数据库



## 8、工程化

#### 前端项目标准？



#### 组件库集成？组件库建设的目的？



#### git 提交规范



#### 代码检查规范 eslint？



#### webpack 插件原理，如何写一个插件？



#### webpack 的 require 是如何查找依赖的？



#### webpack 如何实现动态加载？



#### webpack `treeShaking` 原理，是靠什么才能实现 (ES6 模块的静态导出)。



#### webpack 的构建原理，loader 和 plugin 的区别。



## 14、优化

#### webpack 编译优化？



#### webpack 打包构建优化？



#### 如何避免频繁的操作 `DOM`。

* 对 `DOM` 查询做缓存：

  ``` js
  // 缓存dom查询结果
  const lis = document.getElementsByTagName('li');
  const length = lis.length;
  for(let i = 0; i<length; i++) {
    // 缓存length,只进行一次 dom 查询
  }
  ```

* 将频繁的操作修改为一次性操作：

  ``` js
  // 创建一个文档片段，此时没有插入到 dom 树中
  const frag = document.createDocumentFragment();
  
  // 执行插入
  for(let x = 0; x < 10; x++) {
    // ...
  }
  
  // 都完成后，再插入到 dom 树中
  document.body.appendChild(frag);
  
  ```

#### 前端性能优化。

* 减少资源体积（合并代码、压缩代码、splitChunks 抽离公共文件、服务端开启 gzip 压缩）
* 减少 `http` 请求（合并 `http` 请求、合并资源、图片懒加载、`cookie` 优化、`http` 请求和资源加载的区分优化）
* 减少计算
* 预渲染、`ssr` 服务器端渲染
* 对 `dom` 查询进行缓存，将频繁的 `dom` 操作合并到一起插入 `dom` 结构，
* 节流防抖
* `cs s` 放头部、`js`  放底部
* 使用 cdn 加载第三方模块
* 长列表滚动到可视区域动态加载
* 骨架屏

#### 页面渲染的时候时常会遇到白屏的时候，我们怎么优化这种情况？

* 使用缓存
* 服务端渲染 ssr
* 按需加载
* 路由分割 

#### 前端如何做性能监控、异常监控、前端用户埋点 sdk ？

性能监控：

* `http` 的方面，在后端 `log` 日志，流入 `kafka`，然后在 `kafka` 消费数据，可以准确的监控到哪些接口有异常？异常率是多少？
* 前端的 `Performance` 的 `api`

异常监控：

* `window.onerror` 是进行 `js` 异常的监听事件，但在 IE 中是不支持的
* IE 的监控，要使用 `try catch` 的方式进行捕获
* 如何用 `try catch` 捕获异步的异常

前端 `sdk` 埋点：（统计用户的 UV/PV 分析，用户的转化率等等）



## 10、安全

#### 前端 web 常见的攻击方式有哪些？

* xss 跨站请求攻击（场景、原理、解决方案）
* xsrf 跨站请求伪造
* csrf
* sql 注入
* cookie 安全
* 密码安全

#### 有哪些安全策略，保护用户信息 (cookie 安全性，token 验证用户登录信息) ?



#### `cookie` 使用的安全问题。



## 11、兼容





## 14、Linux

线上一般都是 `linux` 来作为服务器，所以也应该懂些 `Linux` 基本操作：

* 常见命令
* node 部署
* web 部署
* nginx 部署

## 13、其他

#### webview 在性能提升方面，可以做哪些？什么是离线包？



#### 腾讯 x5 内核的优势是什么？我们用了 x5 内核，可以避免什么问题？