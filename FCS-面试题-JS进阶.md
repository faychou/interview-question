# fcs-前端面试-进阶

## 2、JavaScript

#### 手写 `apply`、`call`、`bind` 函数。

``` js
// call 重写
Function.prototype.MyCall = function(context, ...args) {
  context = context || window; // 因为传递过来的 context 有可能是 null
  context.fn = this; // 让 fn 的上下文为 context
  const result = context.fn(...args);
  delete context.fn;
  return result; // 因为有可能 this 函数会有返回值 return
}

// apply 重写
Function.prototype.MyApply = function(context, arr: any[]) {
  context = context || window; 
  context.fn = this; 
  const result = context.fn(...args);
  delete context.fn;
  return result; 
}

// bind
Function.prototype.MyBind = function(context, ...args) {
  var fn = this; 
  return function () {
    fn.call(context, ...args, ...arguments);
  }
}
```



#### 实现一个简易的模板引擎



#### promise 限制并发数



#### ES6 Modules 相对于 CommonJS 的优势是什么？



## 3、TypeScript

#### 你觉得typescript和javascript有什么区别



#### typescript你都用过哪些类型



#### typescript中type和interface的区别

## 4、算法

#### 实现36进制转换。



#### 冒泡排序--时间复杂度 n^2

``` js
function bubbleSort(arr) {
  // 缓存数组长度
  const len = arr.length;
  // 外层循环用于控制从头到尾的比较+交换到底有多少轮
  for (let i = 0; i < len; i++) {
    // 内层循环用于完成每一轮遍历过程中的重复比较+交换
    for (let j = 0; j < len - 1; j++) {
      // 若相邻元素前面的数比后面的大
      if (arr[j] > arr[j + 1]) {
        // 交换两者
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
      }
    }
  }
  // 返回数组
  return arr;
}
// console.log(bubbleSort([3, 6, 2, 4, 1]));
```



#### 选择排序--时间复杂度 n^2

``` js
function selectSort(arr) {
  // 缓存数组长度
  const len = arr.length;
  // 定义 minIndex，缓存当前区间最小值的索引，注意是索引
  let minIndex;
  // i 是当前排序区间的起点
  for (let i = 0; i < len - 1; i++) {
    // 初始化 minIndex 为当前区间第一个元素
    minIndex = i;
    // i、j分别定义当前区间的上下界，i是左边界，j是右边界
    for (let j = i; j < len; j++) {
      // 若 j 处的数据项比当前最小值还要小，则更新最小值索引为 j
      if (arr[j] < arr[minIndex]) {
        minIndex = j;
      }
    }
    // 如果 minIndex 对应元素不是目前的头部元素，则交换两者
    if (minIndex !== i) {
      [arr[i], arr[minIndex]] = [arr[minIndex], arr[i]];
    }
  }
  return arr;
}
// console.log(quickSort([3, 6, 2, 4, 1]));
```



#### 插入排序--时间复杂度 n^2

``` js
function insertSort(arr) {
  for (let i = 1; i < arr.length; i++) {
    let j = i;
    let target = arr[j];
    while (j > 0 && arr[j - 1] > target) {
      arr[j] = arr[j - 1];
      j--;
    }
    arr[j] = target;
  }
  return arr;
}
// console.log(insertSort([3, 6, 2, 4, 1]));

```



#### 快排--时间复杂度 nlogn~ n^2 之间

``` js
function quickSort(arr) {
  if (arr.length < 2) {
    return arr;
  }
  const cur = arr[arr.length - 1];
  const left = arr.filter((v, i) => v <= cur && i !== arr.length - 1);
  const right = arr.filter((v) => v > cur);
  return [...quickSort(left), cur, ...quickSort(right)];
}
// console.log(quickSort([3, 6, 2, 4, 1]));
```



#### 归并排序--时间复杂度 nlog(n)

``` js
function merge(left, right) {
  let res = [];
  let i = 0;
  let j = 0;
  while (i < left.length && j < right.length) {
    if (left[i] < right[j]) {
      res.push(left[i]);
      i++;
    } else {
      res.push(right[j]);
      j++;
    }
  }
  if (i < left.length) {
    res.push(...left.slice(i));
  } else {
    res.push(...right.slice(j));
  }
  return res;
}

function mergeSort(arr) {
  if (arr.length < 2) {
    return arr;
  }
  const mid = Math.floor(arr.length / 2);

  const left = mergeSort(arr.slice(0, mid));
  const right = mergeSort(arr.slice(mid));
  return merge(left, right);
}
// console.log(mergeSort([3, 6, 2, 4, 1]));

```



#### 二分查找--时间复杂度 log2(n)

如何确定一个数在一个有序数组中的位置。

``` js
function search(arr, target, start, end) {
  let targetIndex = -1;

  let mid = Math.floor((start + end) / 2);

  if (arr[mid] === target) {
    targetIndex = mid;
    return targetIndex;
  }

  if (start >= end) {
    return targetIndex;
  }

  if (arr[mid] < target) {
    return search(arr, target, mid + 1, end);
  } else {
    return search(arr, target, start, mid - 1);
  }
}
// const dataArr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
// const position = search(dataArr, 6, 0, dataArr.length - 1);
// if (position !== -1) {
//   console.log(`目标元素在数组中的位置:${position}`);
// } else {
//   console.log("目标元素不在数组中");
// }

```



#### 合并乱序区间



#### 树的遍历有几种方式，实现下层次遍历



#### 判断对称二叉树



#### 两个有序链表和并成一个有序链表



#### 介绍下深度优先遍历和广度优先遍历，如何实现？

深度优先遍历（Depth-First-Search），是搜索算法的一种，它沿着树的深度遍历树的节点，尽可能深地搜索树的分支。当节点 v 的所有边都已被探寻过，将回溯到发现节点 v 的那条边的起始节点。这一过程一直进行到已探寻源节点到其他所有节点为止，如果还有未被发现的节点，则选择其中一个未被发现的节点为源节点并重复以上操作，直到所有节点都被探寻完成。

简单的说，DFS 就是从图中的一个节点开始追溯，直到最后一个节点，然后回溯，继续追溯下一条路径，直到到达所有的节点，如此往复，直到没有路径为止。

``` js
Graph.prototype.dfs = function() {
  var marked = [];
  for (var i = 0; i < this.vertices.length; i++) {
    if (!marked[this.vertices[i]]) {
      dfsVisit(this.vertices[i]);
    }
  }

  function dfsVisit(u) {
    let edges = this.edges;
    marked[u] = true;
    console.log(u);
    var neighbors = edges.get(u);
    for (var i=0; i<neighbors.length; i++) {
      var w = neighbors[i];
      if (!marked[w]) {
        dfsVisit(w);
      }
    }
  }
}
```

广度优先遍历（Breadth-First-Search）是从根节点开始，沿着图的宽度遍历节点，如果所有节点均被访问过，则算法终止，BFS 同样属于盲目搜索，一般用队列数据结构来辅助实现 BFS。

BFS 从一个节点开始，尝试访问尽可能靠近它的目标节点。本质上这种遍历在图上是逐层移动的，首先检查最靠近第一个节点的层，再逐渐向下移动到离起始节点最远的层。

``` js
Graph.prototype.bfs = function(v) {
  var queue = [], 
    marked = [];
  marked[v] = true;
  queue.push(v); // 添加到队尾
  while(queue.length > 0) {
    var s = queue.shift(); // 从队首移除
    if (this.edges.has(s)) {
      console.log('visited vertex: ', s);
    }
    let neighbors = this.edges.get(s);
    for(let i = 0;i < neighbors.length; i++) {
      var w = neighbors[i];
      if (!marked[w]) {
        marked[w] = true;
        queue.push(w);
      }
    }
  }
}
```

#### 老师分饼干，每个孩子只能得到一块饼干，但每个孩子想要的饼干大小不尽相同。 目标是尽量让更多的孩子满意。 如孩子的要求是 1, 3, 5, 4, 2，饼干是1, 1， 最多能让1个孩子满足。如孩子的要求是 10, 9, 8, 7, 6，饼干是7, 6, 5，最多能 让2个孩子满足。



#### 给定一个正整数数列a, 对于其每个区间, 我们都可以计算一个X值; X值的定义如下: 对于任意区间, 其X值等于区间内最小的那个数乘上区间内所有数和; 现在需要你找出数列a的所有区间中, X值最大的那个区间; 如数列a为: 3 1 6 4 5 2; 则X值最大的区间为6, 4, 5, X = 4 * (6+4+5) = 60;


#### 我现在有一个数组 `[1,2,3,4]`，请实现算法，得到这个数组的全排列的数组，如`[2,1,3,4]`，`[2,1,4,3]`。你这个算法的时间复杂度是多少。



#### 我现在有一个背包，容量为 m，然后有 n 个货物，重量分别为 `w1,w2,w3...wn`，每个货物的价值是 `v1,v2,v3...vn`，w 和 v 没有任何关系，请求背包能装下的最大价值。



#### 实现一个有缓存的斐波拉契数列

``` js
var cache = { };
var count = 0;
function fib(n){
    count++;
    if(n === 1 || n === 2){
        return 1;
    }
    if(cache[n]){
        return cache[n];
    }else{
        var ret = fib(n - 1) + fib(n - 2);
        cache[n] = ret;
        return ret;
    }
}
```

#### 不同路径(一个机器人位于一个 `m * n` 网格的左上角 （起始点在下图中标记为“Start” ）。 机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角。

``` js
function robotPath(m，n) {
  if(m == 1 && n == 1) return 1;
  if(m == 1) return robotPath(m, n - 1);
  if(n == 1) return robotPath(m - 1, n);
  return robotPath(m-1, n) + robotPath(m, n - 1);
}
```



#### 将没有层级的扁平数据转换成树形结构的数据



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