# 面试题-AJAX

#### 你知道哪些 `http` 头部



#### 你了解哪些请求方法，分别有哪些作用和不同



#### `options` 请求方法有什么用。



#### 你知道哪些状态码？



#### 同步和异步的区别是什么？

- 基于`js`是单线程语言；
- 异步不会阻塞代码的执行；
- 同步会阻塞代码的执行。

#### 前端使用异步的场景有哪些？

- 网络请求
- 定时任务



#### 实现原生 `AJAX`。

```js
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

> `xhr.readyState` 说明：

- 0为还没有调用 `send()` 方法
- 1为已调用 `send()` 方法，正在发送请求
- 2为 `send()` 方法执行完成，已经接收到全部响应内容
- 3为正在解析响应内容
- 4为响应内容解析完成，可以在客户端调用



#### 怎么与服务端保持连接



#### 什么是跨域、同源策略，你都知道哪些解决跨域的方法

同源就是：协议，域名，端口，一致。只要有一个不一样就会发生跨域。

解决方案：

- JSONP：script 标签的 src 属性不受同源策略限制，用此方式对非同源服务器请求资源，返回的 JS 代码会调用指定的函数，携带的参数就是所需的数据，这样就完成了跨域请求。

  ```js
  let scriptDom = document.createElement("script");
  scriptDom.src="请求地址?callback=函数名";
  document.body.appendChild(scriptDom);
  ```

- CORS：跨域资源共享，是一种允许当前域（origin）的资源（比如html/js/web service）被其他域（origin）的脚本请求访问的机制。服务器端支持。

- nginx：是一款极其强大的 web 服务器，其优点就是轻量级、启动快、高并发。



#### 调式，抓包

chrome 调试工具：

- `Elements`
- `Network`
- `Console`
- `Application`
- `debugger`

移动端 h5 页，查看网络请求，可以使用 fiddler 工具来抓包， [fiddler 教程点击这里](https://www.cnblogs.com/yyhh/p/5140852.html)。



#### 请讲讲 cookie

[前世今生：Cookie和Session体系](https://www.cnblogs.com/qianduanpiaoge/p/14279365.html)



#### 简述 `https` 原理，以及与 `http` 的区别。



#### HTTPS 相比 HTTP 为什么更加安全可靠？



#### WebSocket 使用的是 TCP 还是 UDP 协议？



#### 什么是单工、半双工和全双工通信？



#### 简单描述 HTTP 协议发送一个带域名的 URL 请求的协议传输过程？（DNS、TCP、IP、链路）



#### 什么是正向代理？什么是反向代理？



#### 什么是代理？什么是网关？代理和网关的作用是什么？



#### 什么是 `CA` 证书？



#### `http2` 了解过吗，`http2` 和 `http1` 的区别和好处，`http2` 的头部压缩的原理。



#### 整个网站进行验证的流程是什么？



#### HTTP 中提升传输速率的方式有哪些？常用的内容编码方式有哪些？



#### 你觉得 HTTP 协议目前存在哪些缺点？



