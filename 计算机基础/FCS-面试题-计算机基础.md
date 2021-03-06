# 面试题-计算机基础



## 1、计算机基础

#### 列举你所了解的计算机存储设备类型？



#### 一般代码存储在计算机的哪个设备中？代码在 CPU 中是如何运行的？



#### 高级程序设计语言是如何编译成机器语言的？



#### 编译器一般由哪几个阶段组成？数据类型检查一般在什么阶段进行？



#### 编译过程中虚拟机的作用是什么？



#### 什么是沙箱？浏览器的沙箱有什么作用？



#### 如何处理浏览器中表单项的密码自动填充问题？



#### 什么是对称密钥（共享密钥）加密？什么是非对称密钥（公开密钥）加密？哪个更加安全？



#### `DNS` 解析？



#### `TCP/IP` 协议的三次握手，四次挥手，具体是怎么通信的？



#### 操作系统中进程和线程怎么通信

进程：应用程序的执行实例，每一个进程都是由私有的虚拟地址空间、代码、数据和其他系统资源所组成。

线程：线程是进程内的一个独立执行单元，在不同的线程之间是可以共享进程资源的。

进程拥有独立的堆栈空间和数据段，每当启动一个新的进程必须分配给它独立的地址空间，建立众多的数据表来维护它的代码段、堆栈段和数据段。

线程拥有独立的堆栈空间，但是共享数据段，它们彼此之间使用相同的地址空间，共享大部分数据，比进程更节俭，开销比较小，切换速度也比进程快，效率高。



#### 计算机网络的7层结构？每一层做了什么事情？



#### 介绍chrome 浏览器的几个版本

[参考](https://github.com/lgwebdream/FE-Interview/issues/11)



浏览器缓存分为强缓存和协商缓存，浏览器在请求数据的时候，会先请看强缓存是否命中，然后再走协商缓存。

强缓存：

- 强缓存是Expires和Cache-Control,Cache-Control的优先级更高,是因为Expires是HTTP/1.0而Cache-Control是HTTP/1.1
- Expires的参数一个过期时间，浏览器通过这个过期时间判断是否走缓存
- Cache-Control的参数是最长时间max-age=1000，就是在多长时间内走缓存
- Cache-Control可设置不缓存no-store，是否跳过强缓存no-cache，只有浏览器可缓存代理服务器不可缓存private，和代理服务器缓存时间s-maxage

协商缓存：

- 在没有命中强缓存之后，浏览器会在请求头中带If-Modified-Since属性最后修改时间或者If-None-Match属性根据内容生成的hash来判断是否命中缓存
- If-Modified-Since对应服务器设置返回头Last-Modified
- If-None-Match对应服务器设置返回头ETag
- ETag的优先级比Last-Modified更高
- ETag精度教高，是存的文件内容生成的hash
- Last-Modified性能较好， 因为是存最后修改时间, 只能感知秒级别的修改
- 命中缓存返回304


