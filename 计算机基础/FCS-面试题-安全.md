# 面试题-安全

#### XSS

跨站脚本攻击（Cross Site Scripting）是最常见和基本的攻击 WEB 网站方法，攻击者通过注入非法的 html 标签或者 javascript 代码，从而当用户浏览该网页时，控制用户浏览器。



#### 设置 Cookie 时候如何防止 XSS 攻击？



#### CSRF

跨站点请求伪造（Cross-Site Request Forgeries），冒充用户发起请求（在用户不知情的情况下）， 完成一些违背用户意愿的事情（如修改用户信息，删初评论等）。

**防御：**

- 验证码；强制用户必须与应用进行交互，才能完成最终请求。
- 尽量使用 post ，限制 get 使用；get 太容易被拿来做 csrf 攻击；
- 请求来源限制，此种方法成本最低，但是并不能保证 100% 有效，因为服务器并不是什么时候都能取到 Referer，而且低版本的浏览器存在伪造 Referer 的风险。
- token 验证 CSRF 防御机制是公认最合适的方案。



#### 点击劫持

点击劫持，是指利用透明的按钮或连接做成陷阱，覆盖在 Web 页面之上。然后诱使用户在不知情的情况下，点击那个连接访问内容的一种攻击手段。这种行为又称为界面伪装(UI Redressing) 。

点击劫持一般有如下两种方式实现：

- 击者使用一个透明 iframe，覆盖在一个网页上，然后诱使用户在该页面上进行操作，此时用户将在不知情的情况下点击透明的 iframe 页面；
- 攻击者使用一张图片覆盖在网页，遮挡网页原有的位置含义。



**防御：**

方案一：X-FRAME-OPTIONS

X-FRAME-OPTIONS HTTP 响应头是用来给浏览器指示允许一个页面可否在 `<frame>`, `<iframe>` 或者 `<object>` 中展现的标记。
网站可以使用此功能，来**确保自己网站内容没有被嵌到别人的网站中去**，也从而避免点击劫持的攻击

X-FRAME-OPTIONS 有三个值：

- DENY：表示页面不允许在 frame 中展示，即便是在相同域名的页面中嵌套也不允许。
- SAMEORIGIN：表示该页面可以在相同域名页面的 frame 中展示。
- ALLOW-FROM url：表示该页面可以在指定来源的 frame 中展示。

方案二：使用 Javascript 防御

判断顶层视口的域名是不是和本页面的域名一致，如果不一致就让恶意网页自动跳转到我方的网页。

``` jsx
if (top.location.hostname !== self.location.hostname) {    
    alert("您正在访问不安全的页面，即将跳转到安全页面！")   
    top.location.href = self.location.href;
}
```

