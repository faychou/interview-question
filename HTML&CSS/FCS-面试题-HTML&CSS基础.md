# HTML&CSS基础



## 1、HTML

#### 从输入 `url` 到渲染出页面的整个过程。

* 输入地址，浏览器解析网址。

* `DNS` 解析，查询到 `IP`，返回对应的 `IP`。

* `TCP` 连接(`TCP` 三次握手 80 端口)。[`https` 在 `TCP` 连接之前 `SSL/TLS` 做了加密，防劫持，需要证书，端口不同 443]

* 发送 `http` 请求。

* 返回 `http` 响应。

* 浏览器解析渲染页面。

* 断开连接(四次挥手)。



#### 谈谈你对浏览器缓存的理解？

[实践这一次，彻底搞懂浏览器缓存机制](https://segmentfault.com/a/1190000017962411)

[代码实践带你掌握 强缓存、协商缓存](https://mp.weixin.qq.com/s/pU2gPJPDqo2sF1vp80YAQg)



#### `cookie` 有哪些属性？



#### `cookie`、`session`、`localStorage`、`sessionStorage` 有什么区别？

* `cookie` 存储大小，最大 4kb；
* `cookie` 会随着请求发送到服务器端，增加请求数据量；
* `localStorage` 数据会永远存储，除非代码回手动删除；
* `sessionStorage`数据只存在于当前会话，浏览器关闭则清空；
* `localStorage` 和 `sessionStorage` 是 `html5`专门为存储而设计的，最大可为5M；
* `localStorage` 和 `sessionStorage` 不会随着 `http` 请求被发送出去。

#### 怎么禁止 `JS` 访问 `cookie`。



#### 我现在有一个canvas，上面随机布着一些黑块，请实现方法，计算 canvas 上有多少个黑块。

使用 `getImageData` 获取像素数组。

## 2、CSS

#### `position` 有哪些属性，他们之间有什么区别？



#### 谈谈 BFC?

[什么是BFC？看这一篇就够了](https://blog.csdn.net/sinat_36422236/article/details/88763187)



`BFC`（Block Formatting Contexts）块级格式化上下文，它是页面 `CSS` 视觉渲染的一部分，用于决定块级盒的布局及浮动相互影响范围的一个区域。

以下元素会创建 `BFC`：

- 根元素(`html`)
- 浮动元素（`float` 不为 `none`）
- 绝对定位元素（`position` 为 `absolute` 或 `fixed`）
- 表格的标题和单元格（`display` 为 `table-caption`，`table-cell`）
- 匿名表格单元格元素（`display` 为 `table` 或 `inline-table`）
- 行内块元素（`display` 为 `inline-block`）
- `overflow` 的值不为 `visible` 的元素
- 弹性元素（`display` 为 `flex` 或 `inline-flex` 的元素的直接子元素）
- 网格元素（`display` 为 `grid` 或 `inline-grid` 的元素的直接子元素）

`BFC` 的范围包含创建它的元素的所有子元素, 但不包括创建了新`BFC`的子元素的内部元素。简单来说，子元素如果又创建了一个新的 `BFC`，那么它里面的内容就不属于上一个 `BFC` 了。

`BFC` 的特性：

* `BFC` 内部的块级盒会在垂直方向上一个接一个排列 
* 同一`BFC`下的相邻块级元素可能发生外边距折叠，创建新的`BFC`可以避免外边距折叠
* 每个元素的外边距盒（margin box）的左边与包含块边框盒（border box）的左边相接触（从右向左的格式化，则相反），即使存在浮动也是如此
* 浮动盒的区域不会和`BFC`重叠
* 计算`BFC`的高度时，浮动元素也会参与计算



#### 如何判断一个元素 CSS 样式溢出，从而可以选择性的加 title 或者 Tooltip?

待定。

#### 如何让 CSS 元素左侧自动溢出（... 溢出在左侧）？

待定。

#### 如何实现一个上中下三行布局，顶部和底部最小高度是 100px，中间自适应?



#### 讲讲圣杯布局或者双飞翼布局。



#### `less`、`sass` 它们的作用是什么？


