# 认识HTML5

## 为什么推出HTML5？

- 现有的html不够规范化标准化，html5在总结经验的基础上推出统一标准，各大主流浏览器将遵循这个标准，提高整个web生态的竞争力。
- html5的文档结构比html4更简洁明确，增加了一些结构化的标签，比如header、footer、nav等。
- 赋予web应用更强大的功能，增加了一系列api，如音视频多媒体、动画、缓存等

## HTML 5与HTML 4的区别？

### 语法改变

示例：DOCTYPE 声明方式

- HTML5

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>HTML5的DOCTYPE声明</title>
</head>
```

- HTML4

```html

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">

```

### 标签、属性改变

如：`<u>`下划线标签，HTML5去掉了该标签

如：`hidden`隐藏元素属性，HTML5新增了该属性

### api调整

如：新增了getCurrentPosition() 方法来获得用户的位置

```js

navigator.geolocation.getCurrentPosition(callback, errorhandler)

```

## HTML5标签

结构标签：`article`,`nav`,`header`,`footer`,`section`,`hgroup`,`figure`,`aside`等

表单标签：`email`,`number`,`range`,`search`,`color`,`url`等

媒体标签：`video`,`audio`,`embed`等

其他功能：`mark`,`progress`,`time`,`ruby`,`rt`,`wbr`,`canvas`,`command`,`details`,`datelist`,`keyen`等

## HTML5属性

`script`标签，支持`defer`和`async`属性指定执行方式，延迟执行和立即执行

`meta`标签，支持`charset`属性指定文档编码集

`link`标签，支持`rel="icon"`时使用`sizes`指定图标大小

`a`标签，支持`media`针对指定设备优化，`hreflang`指示目标网址使用的语言，`ref`指示目标网址是否是外部链接

`ol`标签，支持`reserved`序号是否倒序，`start`起始序号

`style`标签，支持`scoped`css生效作用域

### 全局属性

参见 <http://www.w3school.com.cn/html5/html5_ref_globalattributes.asp>

`contenteditable`,是否允许用户编辑内容

`contextmenu`,自定义属性

`draggable`,是否允许用户拖动元素

`dropzone`,当被拖动的项目/数据被拖放到元素中时会发生什么

`hidden`,隐藏元素

`Spenllecheck`,是否必须对元素进行拼写或语法检查

## HTML5事件

事件（除window事件外）都适用于所有html元素，表单和媒介只是使用情况较多的元素。

### Window 事件属性

window 对象触发的事件，适用于 `<body>` 标签：

| 属性 | 描述 |
|------|------|
|||
|onblur|当窗口失去焦点时运行脚本|
|onfocus|当窗口获得焦点时运行脚本|
|onload|当文档加载时运行脚本|
|||
|新增事件||
|onafterprint|在文档打印后运行脚本|
|onbeforeprint|在文档打印之前运行脚本|
|onbeforeonload|在文档加载之前运行脚本|
|onerror|当错误发生时运行脚本|
|onhaschange|当文档改变时运行脚本|
|onmessage|当触发消息时运行脚本|
|onoffline|当文档离线时运行脚本|
|ononline|当文档上线时运行脚本|
|onpagehide|当窗口隐藏时运行脚本|
|onpageshow|当窗口可见时运行脚本|
|onpopstate|当窗口历史记录改变时运行脚本|
|onredo|当文档执行再执行操作（redo）时运行脚本|
|onresize|当调整窗口大小时运行脚本|
|onstorage|当文档加载加载时运行脚本|
|onundo|当 Web Storage 区域更新时（存储空间中的数据发生变化时）|
|onunload|当用户离开文档时运行脚本|

### 表单事件

由 HTML 表单内部的动作触发的事件
| 属性 | 描述 |
|------|------|
|||
|onblur|当元素失去焦点时运行脚本|
|onchange|当元素改变时运行脚本|
|onfocus|当元素获得焦点时运行脚本|
|onreset|当表单重置时运行脚本。HTML 5 废除。|
|onselect|当选取元素时运行脚本|
|onsubmit|当提交表单时运行脚本|
|||
|新增事件||
|oncontextmenu|当触发上下文菜单时运行脚本|
|onformchange|当表单改变时运行脚本|
|onforminput|当表单获得用户输入时运行脚本|
|oninput|当元素获得用户输入时运行脚本|
|oninvalid|当元素无效时运行脚本|

### 键盘事件

由键盘触发的事件
| 属性 | 描述 |
|------|------|
|onkeydown|当按下按键时运行脚本|
|onkeypress|当按下并松开按键时运行脚本|
|onkeyup|当松开按键时运行脚本|

### 鼠标事件

由鼠标或相似的用户动作触发的事件
| 属性 | 描述 |
|------|------|
|onclick|当单击鼠标时运行脚本|
|ondblclick|当双击鼠标时运行脚本|
|onmousedown|当按下鼠标按钮时运行脚本|
|onmousemove|当鼠标指针移动时运行脚本|
|onmouseout|当鼠标指针移出元素时运行脚本|
|onmouseover|当鼠标指针移至元素之上时运行脚本|
|onmouseup|当松开鼠标按钮时运行脚本|
|||
|新增事件||
|ondrag|当拖动元素时运行脚本|
|ondragend|当拖动操作结束时运行脚本|
|ondragenter|当元素被拖动至有效的拖放目标时运行脚本|
|ondragleave|当元素离开有效拖放目标时运行脚本|
|ondragover|当元素被拖动至有效拖放目标上方时运行脚本|
|ondragstart|当拖动操作开始时运行脚本|
|ondrop|当被拖动元素正在被拖放时运行脚本|
|onmousewheel|当转动鼠标滚轮时运行脚本|
|onscroll|当滚动元素滚动元素的滚动条时运行脚本|

### 媒介事件

由视频、图像以及音频等媒介触发的事件。
| 属性 | 描述 |
|------|------|
|onabort|当发生中止事件时运行脚本|
|||
|新增事件||
|oncanplay|当媒介能够开始播放但可能因缓冲而需要停止时运行脚本|
|oncanplaythrough|当媒介能够无需因缓冲而停止即可播放至结尾时运行脚本|
|ondurationchange|当媒介长度改变时运行脚本|
|onemptied|当媒介资源元素突然为空时（网络错误、加载错误等）运行脚本|
|onended|当媒介已抵达结尾时运行脚本|
|onerror|当在元素加载期间发生错误时运行脚本|
|onloadeddata|当加载媒介数据时运行脚本|
|onloadedmetadata|当媒介元素的持续时间以及其他媒介数据已加载时运行脚本|
|onloadstart|当浏览器开始加载媒介数据时运行脚本|
|onpause|当媒介数据暂停时运行脚本|
|onplay|当媒介数据将要开始播放时运行脚本|
|onplaying|当媒介数据已开始播放时运行脚本|
|onprogress|当浏览器正在取媒介数据时运行脚本|
|onratechange|当媒介数据的播放速率改变时运行脚本|
|onreadystatechange|当就绪状态（ready-state）改变时运行脚本|
|onseeked|当媒介元素的定位属性 [1] 不再为真且定位已结束时运行脚本|
|onseeking|当媒介元素的定位属性为真且定位已开始时运行脚本|
|onstalled|当取回媒介数据过程中（延迟）存在错误时运行脚本|
|onsuspend|当浏览器已在取媒介数据但在取回整个媒介文件之前停止时运行脚本|
|ontimeupdate|当媒介改变其播放位置时运行脚本|
|onvolumechange|当媒介改变音量亦或当音量被设置为静音时运行脚本|
|onwaiting|当媒介已停止播放但打算继续播放时运行脚本|

## web存储

### cookie

HTML5以前方法，浏览器可以将数据存储在服务器上。
缺点：不适合大量数据的存储；由每个对服务器的请求来传递，性能较低。

### localstorage和sessionStorage

HTML5新增方法。

只在请求时使用数据，能够满足大量数据的存储。

不同的网站数据存储独立，且只能访问自己的数据。

使用JavaScript来存储和访问。

其中localstorage没有**时间限制**，sessionStorage针对一个session，关闭浏览器就会丢失。

## websocket协议

### 简介

以tcp协议为基础，与http兼容的web协议。相比http协议，最大特点是服务器能够主动向客户端推送信息。

其他特点包括：

- 建立在 TCP 协议之上，服务器端的实现比较容易。

- 与 HTTP 协议有着良好的兼容性。默认端口也是80和443，并且握手阶段采用 HTTP 协议，因此握手时不容易屏蔽，能通过各种 HTTP 代理服务器。

- 数据格式比较轻量，性能开销小，通信高效。

- 可以发送文本，也可以发送二进制数据。

- 没有同源（协议、域名、端口）限制，客户端可以与任意服务器通信。

- 协议标识符是ws（如果加密，则为wss），服务器网址就是 URL。

### 使用

WebSocket对象api参见[MDN](https://developer.mozilla.org/zn-CN/docs/Web/API/WebSocket)

客户端代码示例：

```js
var ws = new WebSocket("wss://echo.websocket.org");

ws.onopen = function(evt) {
  console.log("Connection open ...");
  ws.send("Hello WebSockets!");
};

ws.onmessage = function(evt) {
  console.log( "Received Message: " + evt.data);
  ws.close();
};

ws.onclose = function(evt) {
  console.log("Connection closed.");
};
```

服务端：搭建ws协议服务器

绑定特定事件响应即可。
示例代码（来自[WebSocket-Node](https://github.com/theturtle32/WebSocket-Node)）：

```js
#!/usr/bin/env node
var WebSocketServer = require('websocket').server;
wsServer = new WebSocketServer({
    httpServer: server,
    autoAcceptConnections: false
});
wsServer.on('request', function(request) {
    var connection = request.accept('echo-protocol', request.origin);
    console.log((new Date()) + ' Connection accepted.');
    connection.on('message', function(message) {
        if (message.type === 'utf8') {
            console.log('Received Message: ' + message.utf8Data);
            connection.sendUTF(message.utf8Data);
        }
    });
    connection.on('close', function(reasonCode, description) {
        console.log((new Date()) + ' Peer ' + connection.remoteAddress + ' disconnected.');
    });
});
```

## HTML图形

### Canvas vs. SVG

Canvas | SVG
---------|----------
使用js渲染 | 使用XML描述
逐像素，无法获取DOM | 可以获取DOM元素，可以为单个元素添加事件
图形绘制完成之后浏览器不管，改变位置时整个图形会重绘 | SVG对象改变浏览器自动重现图形
依赖分辨率|不依赖分辨率
不支持事件处理器|支持事件处理器
文本渲染能力较弱|强于大块区域渲染（如地图）
可以保存为.jpg或.png格式|渲染复杂图形效率低
适合图像密集的场景，如游戏|不适合游戏

### Canvas使用

浏览器支持：不支持IE版本<=8

具体API参考[W3C文档](http://www.w3school.com.cn/html5/html5_ref_canvas.asp)

示例代码：

```js
var icanvas=document.getElementById("iCanvas");
var ictx=icanvas.getContext("2d");

ctx.beginPath();
ctx.lineWidth=10;
ctx.lineJoin="round";//线条相交样式
ctx.moveTo(10,10);
ctx.lineTo(120,110);//一条线段
ctx.lineTo(10,110);//折线
ctx.closePath();//空心三角形
ctx.fill();//填充
ctx.arc(100,75,50,0,2*Math.PI);//曲线
ctx.rect(20,20,150,100);//矩形
ctx.shadowColor="black";//阴影
ctx.scale(2,2);//宽高缩放，后面stroke的图形都会被缩放，坐标也会缩放
//渐变，addColorStop() 方法与 createLinearGradient() 或 createRadialGradient() 一起使用。
var grd=ctx.createLinearGradient(0,0,170,0);
grd.addColorStop(0,"black");
grd.addColorStop(1,"white");
ctx.fillStyle=grd;

ctx.stroke();//渲染图形到画布
```

## Web Worker

后台运行的js，脚本独立，不影响页面性能

浏览器支持：IE不支持

代码示例：

- 需要后台运行的js：注意postMessage()方法

```js
var i=0;
function timedCount()
{
i=i+1;
postMessage(i);
setTimeout("timedCount()",500);
}
timedCount();
```

- 主页面：注意**onmessage**事件

```js
...
if(typeof(w)=="undefined"){
    w=new Worker("demo_workers.js");
}
w.onmessage = function (event) {
document.getElementById("result").innerHTML=event.data;
};
...
```

由于 web worker 位于外部文件中，它们无法访问`window 对象`、`document 对象`和`parent 对象`
