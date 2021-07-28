# 面试题-算法

#### 实现36进制转换。



#### 冒泡排序--时间复杂度 n^2

```js
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

```js
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

```js
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

```js
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

```js
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

```js
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



#### LRU 算法

运用你所掌握的数据结构，设计和实现一个LRU（最近最少使用）缓存机制，它应该支持以下操作：获取数据`get`和写入数据`put`。

* 获取数据 `get(key)` ：如果密钥（`key`）存在于缓存中，则获取密钥的值（总是正数），否则返回 `-1`；

* 写入数据（`put(key, value)`）：如果密钥已经存在，则变更其数据值；如果密钥不存在，则插入该组【密钥/数据值】，当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。
* 进阶：你是否可以在 `O(1)` 时间复杂度内完成这两种操作？

``` js
//  一个Map对象在迭代时会根据对象中元素的插入顺序来进行
// 新添加的元素会被插入到map的末尾，整个栈倒序查看
class LRUCache {
  constructor(capacity) {
    this.secretKey = new Map();
    this.capacity = capacity;
  }
  get(key) {
    if (this.secretKey.has(key)) {
      let tempValue = this.secretKey.get(key);
      this.secretKey.delete(key);
      this.secretKey.set(key, tempValue);
      return tempValue;
    } else return -1;
  }
  put(key, value) {
    // key存在，仅修改值
    if (this.secretKey.has(key)) {
      this.secretKey.delete(key);
      this.secretKey.set(key, value);
    }
    // key不存在，cache未满
    else if (this.secretKey.size < this.capacity) {
      this.secretKey.set(key, value);
    }
    // 添加新key，删除旧key
    else {
      this.secretKey.set(key, value);
      // 删除map的第一个元素，即为最长未使用的
      this.secretKey.delete(this.secretKey.keys().next().value);
    }
  }
}
// let cache = new LRUCache(2);
// cache.put(1, 1);
// cache.put(2, 2);
// console.log("cache.get(1)", cache.get(1))// 返回  1
// cache.put(3, 3);// 该操作会使得密钥 2 作废
// console.log("cache.get(2)", cache.get(2))// 返回 -1 (未找到)
// cache.put(4, 4);// 该操作会使得密钥 1 作废
// console.log("cache.get(1)", cache.get(1))// 返回 -1 (未找到)
// console.log("cache.get(3)", cache.get(3))// 返回  3
// console.log("cache.get(4)", cache.get(4))// 返回  4
```



#### 合并乱序区间



#### 树的遍历有几种方式，实现下层次遍历



#### 判断对称二叉树



#### 两个有序链表和并成一个有序链表



#### 介绍下深度优先遍历和广度优先遍历，如何实现？

深度优先遍历（Depth-First-Search），是搜索算法的一种，它沿着树的深度遍历树的节点，尽可能深地搜索树的分支。当节点 v 的所有边都已被探寻过，将回溯到发现节点 v 的那条边的起始节点。这一过程一直进行到已探寻源节点到其他所有节点为止，如果还有未被发现的节点，则选择其中一个未被发现的节点为源节点并重复以上操作，直到所有节点都被探寻完成。

简单的说，DFS 就是从图中的一个节点开始追溯，直到最后一个节点，然后回溯，继续追溯下一条路径，直到到达所有的节点，如此往复，直到没有路径为止。

```js
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

```js
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

```js
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

[参考](https://github.com/lgwebdream/FE-Interview/issues/9)



#### 不同路径(一个机器人位于一个 `m * n` 网格的左上角 （起始点在下图中标记为“Start” ）。 机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角。

```js
function robotPath(m，n) {
  if(m == 1 && n == 1) return 1;
  if(m == 1) return robotPath(m, n - 1);
  if(n == 1) return robotPath(m - 1, n);
  return robotPath(m-1, n) + robotPath(m, n - 1);
}
```



#### 将没有层级的扁平数据转换成树形结构的数据

``` js
function listToTree(data) {
  let temp = {};
  let treeData = [];
  for (let i = 0; i < data.length; i++) {
    temp[data[i].id] = data[i];
  }
  for (let i in temp) {
    if (+temp[i].parentId != 0) {
      if (!temp[temp[i].parentId].children) {
        temp[temp[i].parentId].children = [];
      }
      temp[temp[i].parentId].children.push(temp[i]);
    } else {
      treeData.push(temp[i]);
    }
  }
  return treeData;
}
```



#### 树形结构转成扁平数组

``` js
function treeToList(data) {
  let res = [];
  const dfs = (tree) => {
    tree.forEach((item) => {
      if (item.children) {
        dfs(item.children);
        delete item.children;
      }
      res.push(item);
    });
  };
  dfs(data);
  return res;
}
```



#### 动态规划求解硬币找零问题

给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。

``` js
示例1：
输入: coins = [1, 2, 5], amount = 11
输出: 3
解释: 11 = 5 + 5 + 1

示例2：
输入: coins = [2], amount = 3
输出: -1
```

代码实现：

``` js
const coinChange = function (coins, amount) {
  // 用于保存每个目标总额对应的最小硬币个数
  const f = [];
  // 提前定义已知情况
  f[0] = 0;
  // 遍历 [1, amount] 这个区间的硬币总额
  for (let i = 1; i <= amount; i++) {
    // 求的是最小值，因此我们预设为无穷大，确保它一定会被更小的数更新
    f[i] = Infinity;
    // 循环遍历每个可用硬币的面额
    for (let j = 0; j < coins.length; j++) {
      // 若硬币面额小于目标总额，则问题成立
      if (i - coins[j] >= 0) {
        // 状态转移方程
        f[i] = Math.min(f[i], f[i - coins[j]] + 1);
      }
    }
  }
  // 若目标总额对应的解为无穷大，则意味着没有一个符合条件的硬币总数来更新它，本题无解，返回-1
  if (f[amount] === Infinity) {
    return -1;
  }
  // 若有解，直接返回解的内容
  return f[amount];
};
```

