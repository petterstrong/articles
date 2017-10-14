# 简述 iframe 的信道跨域通信



由于浏览器的同源策略，跨域之间的iframe的web通信是无法直接实现的。

作为开发者，我们需要试着去突破这些浏览器的限制，完成子iframe与父页面之间信息通信。还好HTML5 提供给window对象一个`postMessge()`方法，本文主要是简述它是实现子iframe与父页面信道通信（channel messaging）。其他的方案请查看底部 [五、相关链接](#五、相关链接)



## 一、postMessage 简述

**window.postMessage()** 方法可以安全的实现跨域通信，提供一种受控机制以规避同源策略的限制。

### 1.1 语法

```javascript

otherWindow.postMessage(message, targetOrigin, [transfer])

```

- **otherWindow **

  其他窗口的一个引用，比如子iframe的contentWindow属性、执行window.open返回的窗口对象、或是命名过或索引的window.frames

- **message**

  将要发送到父页面的数据，它将会被结构化克隆算法序列化，这就意味着你现在就可以不用做什么处理的讲数据对象安全的传送（*在 Gecko 6.0 (Firefox 6.0 / Thunderbird 6.0 / SeaMonkey 2.3)之前， **参数 message 必须是一个字符串**。*）

  ​

- **targetOrigin**

  指定哪些窗口能接收到消息事件，其值为字符串"*"表示任意窗口可以接受。***注意:当传送消息重要时，请指定有确切值的targetOrigin，否则会将数据泄露到任何站点（包括恶意站点）！***

- **transfer**

  同时和message传递的[Transferable](https://developer.mozilla.org/zh-CN/docs/Web/API/Transferable)对象。这些对象的所有权将被转移给消息的接收方（这里指的是我们的父页面），下文中我们将会传入**MessagePort**

  ​
### 1.2 示例

  - [点击查看案例](https://ilifediary.com)






## 二、MessageChannel



### 1.1 简介

> MessageChannel接口是信道通信API的一个接口，它允许我们创建一个新的信道并通过信道的两个MessagePort属性来传递数据



简单来说，MessageChannel创建了**一个通信的管道**，这个管道有两个口子，每个口子都可以通过postMessage发送数据，而一个口子只要绑定了onmessage回调方法，就可以接收从另一个口子传过来的数据。



### 1.2 用法

在以下代码块中，您可以看到使用MessageChannel（）构造函数创建的新通道。

```javascript

var channel = new MessageChannel();
var para = document.querySelector('p');
    
var ifr = document.querySelector('iframe');
var otherWindow = ifr.contentWindow;

ifr.addEventListener("load", iframeLoaded, false);
    
function iframeLoaded() {
  otherWindow.postMessage('Hello from the main page!', '*', [channel.port2]);
}

channel.port1.onmessage = handleMessage;
function handleMessage(e) {
  para.innerHTML = e.data;
}
```



## 三、相关链接

[MessageChannel]: https://developer.mozilla.org/zh-CN/docs/Web/API/MessageChannel	"MessageChannel - MDN"
[postMessage]: https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage	"postMessage - MDN"

