---
title: "SSE技术详解 使用http将服务端数据推送到客户端  [可作为Websocket的轻量化替换]"
date: 2016-11-21T14:20:46+08:00
categories: ["All","Web","WebSocket","SSE"]
tags: ["Web","WebSocket","SSE"]
toc: true
author: "Jermine"
author_homepage:  "/"
weight: 70
keywords: ["Web","WebSocket","SSE","HTTP","HTTP2.0","HTTPS"]
description: "SSE技术详解 使用http将服务端数据推送到客户端  [可作为Websocket的轻量化替换]"
---

# SSE技术详解-使用http将服务端数据推送到客户端

SSE （ Server-sent Events ）是 WebSocket 的一种轻量代替方案，使用 HTTP 协议。

　　严格地说，HTTP 协议是没有办法做服务器推送的，但是当服务器向客户端声明接下来要发送流信息时，客户端就会保持连接打开，SSE 使用的就是这种原理。

## 一、SSE 能做什么？
　　理论上， SSE 和 WebSocket 做的是同一件事情。当你需要用新数据局部更新网络应用时，SSE 可以做到不需要用户执行任何操作，便可以完成。

　　举例我们要做一个统计系统的管理后台，我们想知道统计数据的实时情况。类似这种更新频繁、 低延迟的场景，SSE 可以完全满足。

　　其他一些应用场景：例如邮箱服务的新邮件提醒，微博的新消息推送、管理后台的一些操作实时同步等，SSE 都是不错的选择。

## 二、SSE vs. WebSocket
　　SSE 是单向通道，只能服务器向客户端发送消息，如果客户端需要向服务器发送消息，则需要一个新的 HTTP 请求。 这对比 WebSocket 的双工通道来说，会有更大的开销。这么一来的话就会存在一个「什么时候才需要关心这个差异？」的问题，如果平均每秒会向服务器发送一次消息的话，那应该选择 WebSocket。如果一分钟仅 5 - 6 次的话，其实这个差异并不大。

　　在浏览器兼容方面，两者差不多。在较早之前，每当需要建立双向 Socket 时就会使用 Flash，在 移动浏览器不支持 Flash 的情况下，WebSocket 的兼容是比较难做的。

　　SSE 我认为最大的优势是便利：

* 实现一个完整的服务仅需要少量的代码；
* 可以在现有的服务中使用，不需要启动一个新的服务；
* 可以用任何一种服务端语言中使用；
* 基于 HTTP ／ HTTPS 协议，可以直接运行于现有的代理服务器和认证技术。
　　有了这些优势，在选择使用 SSE 时就已经为自己的项目节约了不少成本。

## 三、SSE（Server-sent Events）在HTML 5中的技术规范和定义
　　Server-sent Events 规范是 HTML 5 规范的一个组成部分，具体的规范文档见参考资源。该规范比较简单，主要由两个部分组成：

　　第一个部分是服务器端与浏览器端之间的通讯协议，

　　第二部分则是在浏览器端可供 JavaScript 使用的 EventSource 对象。

　　通讯协议是基于纯文本的简单协议。服务器端的响应的内容类型是“text/event-stream”。响应文本的内容可以看成是一个事件流，由不同的事件所组成。

　　每个事件由类型和数据两部分组成，同时每个事件可以有一个可选的标识符。不同事件的内容之间通过仅包含回车符和换行符的空行（“\r\n”）来分隔。每个事件的数据可能由多行组成。

　　下面代码给出了服务器端响应的示例：

```
data: first event
 
data: second event
id: 100
 
event: myevent
data: third event
id: 101
 
: this is a comment
data: fourth event
data: fourth event continue
```

　　如上所示，每个事件之间通过空行来分隔。对于每一行来说，冒号（“:”）前面表示的是该行的类型，冒号后面则是对应的值。可能的类型包括：

* 类型为空白，表示该行是注释，会在处理时被忽略。
* 类型为 data，表示该行包含的是数据。以 data 开头的行可以出现多次。所有这些行都是该事件的数据。
* 类型为 event，表示该行用来声明事件的类型。浏览器在收到数据时，会产生对应类型的事件。
* 类型为 id，表示该行用来声明事件的标识符。
* 类型为 retry，表示该行用来声明浏览器在连接断开之后进行再次连接之前的等待时间。
  
　　在上述代码中，第一个事件只包含数据“first event”，会产生默认的事件；第二个事件的标识符是 100，数据为“second event”；第三个事件会产生类型为“myevent”的事件；最后一个事件的数据为“fourth event\nfourth event continue”。**当有多行数据时，实际的数据由每行数据以换行符连接而成。**

　　**如果服务器端返回的数据中包含了事件的标识符，浏览器会记录最近一次接收到的事件的标识符。如果与服务器端的连接中断，当浏览器端再次进行连接时，会通过 HTTP 头“Last-Event-ID”来声明最后一次接收到的事件的标识符。服务器端可以通过浏览器端发送的事件标识符来确定从哪个事件开始来继续连接。**

　　对于服务器端返回的响应，浏览器端需要在 `JavaScript` 中使用 `EventSource` 对象来进行处理。`EventSource` 使用的是标准的事件监听器方式，只需要在对象上添加相应的事件处理方法即可。EventSource 提供了三个标准事件

　　如之前所述，服务器端可以返回自定义类型的事件。对于这些事件，可以使用 `addEventListener` 方法来添加相应的事件处理方法。如下代码给出了 `EventSource` 对象的使用示例。

```
var es = new EventSource('events');
es.onmessage = function(e) {
    console.log(e.data);
};
 
es.addEventListener('myevent', function(e) {
    console.log(e.data);
});
```

　　在指定 URL 创建出 EventSource 对象之后，可以通过 `onmessage` 和 `addEventListener` 方法来添加事件处理方法。当服务器端有新的事件产生，相应的事件处理方法会被调用。`EventSource` 对象的 `onmessage` 属性的作用类似于 addEventListener( ‘ message ’ )，不过 onmessage 属性只支持一个事件处理方法。

## 四、简单示例
　　下面是一个简单的示例，实现一个 SSE 服务。

　　1、服务端

```
'use strict';

const http = require('http');

http.createServer((req, res) => {

  // 服务器声明接下来发送的是事件流
  res.writeHead(200, {
    'Content-Type': 'text/event-stream',
    'Cache-Control': 'no-cache',
    'Connection': 'keep-alive',
    'Access-Control-Allow-Origin': '*',
  });

  // 发送消息
  setInterval(() => {
    res.write('event: slide\n'); // 事件类型
    res.write(`id: ${+new Date()}\n`); // 消息 ID
    res.write('data: 7\n'); // 消息数据
    res.write('retry: 10000\n'); // 重连时间
    res.write('\n\n'); // 消息结束
  }, 3000);

  // 发送注释保持长连接
  setInterval(() => {
    res.write(': \n\n');
  }, 12000);
}).listen(2000);
```
　　服务器首先向客户端声明接下来发送的是事件流（ text/event-stream ）类型的数据，然后就可以向客户端多次发送消息。

　　事件流是一个简单的文本流，仅支持 UTF-8 格式的编码。每条消息以一个空行作为分隔符。

　　在规范中为消息定义了 4 个字段：

**event** 消息的事件类型。客户端收到消息时，会在当前的 EventSource 对象上触发一个事件，这个事件的名称就是这个字段的值，如果消息没有这个字段，客户端的 EventSource 对象就会触发默认的 message 事件。

**id** 这条消息的 ID。客户端接收到消息后，会把这个 ID 作为内部属性 Last-Event-ID，在断开重连 成功后，会把 Last-Event-ID 发送给服务器。

**data** 消息的数据字段。 客户端会把这个字段解析为字符串，如果一条消息有多个 data 字段，客户端会自动用换行符 连接成一个字符串。

**retry** 指定客户端重连的时间。只接受整数，单位是毫秒。如果这个值不是整数则会被自动忽略。

一个很有意思的地方是，规范中规定以冒号开头的消息都会被当作注释，一条普通的注释（:\n\n）对于服务器来说只占 5 个字符，但是发送到客户端上的时候不会触发任何事件，这对客户端来说是非常友好的。所以注释一般被用于维持服务器和客户端的长连接。**

　　效果如下：

![1158910-20180618204245498-264853663.png](/img/web/sse/1158910-20180618204245498-264853663.png)


　　2、客户端

　　我们创建了一个 EventSource 对象，传入参数：url。并且根据服务器的状态和发送的信息作出响应。

```
'use strict';

if (window.EventSource) {
  // 创建 EventSource 对象连接服务器
  const source = new EventSource('http://localhost:2000');

  // 连接成功后会触发 open 事件
  source.addEventListener('open', () => {
    console.log('Connected');
  }, false);

  // 服务器发送信息到客户端时，如果没有 event 字段，默认会触发 message 事件
  source.addEventListener('message', e => {
    console.log(`data: ${e.data}`);
  }, false);

  // 自定义 EventHandler，在收到 event 字段为 slide 的消息时触发
  source.addEventListener('slide', e => {
    console.log(`data: ${e.data}`); // => data: 7
  }, false);

  // 连接异常时会触发 error 事件并自动重连
  source.addEventListener('error', e => {
    if (e.target.readyState === EventSource.CLOSED) {
      console.log('Disconnected');
    } else if (e.target.readyState === EventSource.CONNECTING) {
      console.log('Connecting...');
    }
  }, false);
} else {
  console.error('Your browser doesn\'t support SSE');
}
```

　　EventSource从父接口 EventTarget 中继承了属性和方法，其内置了 3 个 EventHandler 属性、2 个只读属性和 1 个方法：

**EventHandler 属性**
　　EventSource.onopen 在连接打开时被调用。

　　EventSource.onmessage 在收到一个没有 event 属性的消息时被调用。

　　EventSource.onerror 在连接异常时被调用。

**只读属性**
　　EventSource.readyState 一个 unsigned short 值，代表连接状态。可能值是 CONNECTING (0), OPEN (1), 或者 CLOSED (2)。

　　EventSource.url 连接的 URL。

方法
　　EventSource.close() 关闭连接

　　效果：

![1158910-20180618215720129-2125344216.png](/img/web/sse/1158910-20180618215720129-2125344216.png)
   

## 五、SSE使用注意事项
1、SSE 如何保证数据完整性

　　客户端在每次接收到消息时，会把消息的 id 字段作为内部属性 Last-Event-ID 储存起来。

　　SSE 默认支持断线重连机制，在连接断开时会 触发 EventSource 的 error 事件，同时自动重连。再次连接成功时 EventSource 会把 Last-Event-ID 属性作为请求头发送给服务器，这样服务器就可以根据这个 Last-Event-ID 作出相应的处理。

　　这里需要注意的是，id 字段不是必须的，服务器有可能不会在消息中带上 id 字段，这样子客户端就不会存在 Last-Event-ID 这个属性。所以为了保证数据可靠，我们需要在每条消息上带上 id 字段。

2、减少开销

　　在 SSE 的草案中提到，"text/event-stream" 的 MIME 类型传输应当在静置 15 秒后自动断开。在实际的项目中也会有这个机制，但是断开的时间没有被列入标准中。

　　为了减少服务器的开销，我们也可以有目的的断开和重连。

　　简单的办法是服务器发送一个 关闭消息并指定一个重连的时间戳，客户端在触发关闭事件时关闭当前连接并创建 一个计时器，在重连时把计时器销毁 。

```
'use strict';

function connectSSE() {
  if (window.EventSource) {
    const source = new EventSource('http://localhost:2000');
    let reconnectTimeout;

    source.addEventListener('open', () => {
      console.log('Connected');
      clearTimeout(reconnectTimeout);
    }, false);

    source.addEventListener('pause', e => {
      source.close();
      const reconnectTime = +e.data;
      const currentTime = +new Date();
      reconnectTimeout = setTimeout(() => {
        connectSSE();
      }, reconnectTime - currentTime);
    }, false);
  } else {
    console.error('Your browser doesn\'t support SSE');
  }
}

connectSSE();
```

3、浏览器兼容

![1158910-20180618205141152-972356492.png](/img/web/sse/1158910-20180618205141152-972356492.png)

　　向下兼容：早些时候，为了实现数据实时更新最常见的方法就是轮询。

　　轮询是以一个固定频率向服务器发送请求，服务器在有 数据更新时 返回新的数据，以此来管理数据的更新。这种轮询的方式不但开销大，而且更新的效率和频率有关，也不能达到及时更新的目的。

　　接着便出现了长轮询的方式：客户端向服务器发送请求之后，服务器会暂时把请求挂起，等到有数据更新时再返回最新的数据给客户端，客户端在接收到新的消息后再向服务器发送请求。与常规轮询的不同之处是：数据可以做到实时更新，可以减少不必要的开销。

　　这里有一个「选择长轮询还是常规轮询？」的命题，长轮询是不是总比常规轮询占有优势？我们可以从带宽占用的角度分析，如果一个程序数据更新太过频繁，假设每秒 2 次更新，如果使用长轮询的话每分钟要发送 120 次 HTTP 请求。如果使用常规轮询，每 5 秒发送一次请求的话， 一分钟才 20 次，从这里看，常规轮询更占有优势。

　　长轮询和 SSE 最关键的区别在于，每一次数据更新都需要一次 HTTP 请求。和 WebSocket 还有 SSE 一样，长轮询也会 占用一个 socket。在数据更新效率上和 SSE 差不多，一有数据更新就能检测到。加上所有浏览器都支持，是一个不错的 SSE 替代方案。

　　文章介绍了 SSE 的用法及使用过程中的一些技巧。对比 WebSocket，SSE 在开发时间和成本上占有较大的优势。做数据推送服务，除了 WebSocket，SSE 也是一个不错的选择，希望对大家有所帮助。