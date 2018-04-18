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

