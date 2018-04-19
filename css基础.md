# css基础

- [css基础](#css%E5%9F%BA%E7%A1%80)
  - [选择器](#%E9%80%89%E6%8B%A9%E5%99%A8)
  - [盒模型](#%E7%9B%92%E6%A8%A1%E5%9E%8B)
  - [定位](#%E5%AE%9A%E4%BD%8D)
    - [三栏布局，左右定宽，中间自适应](#%E4%B8%89%E6%A0%8F%E5%B8%83%E5%B1%80%EF%BC%8C%E5%B7%A6%E5%8F%B3%E5%AE%9A%E5%AE%BD%EF%BC%8C%E4%B8%AD%E9%97%B4%E8%87%AA%E9%80%82%E5%BA%94)
  - [css3高级](#css3%E9%AB%98%E7%BA%A7)
    - [渐变色](#%E6%B8%90%E5%8F%98%E8%89%B2)
      - [线性渐变](#%E7%BA%BF%E6%80%A7%E6%B8%90%E5%8F%98)
      - [径向渐变](#%E5%BE%84%E5%90%91%E6%B8%90%E5%8F%98)
    - [阴影](#%E9%98%B4%E5%BD%B1)
    - [过渡](#%E8%BF%87%E6%B8%A1)
    - [2D转换](#2d%E8%BD%AC%E6%8D%A2)
    - [3D转换](#3d%E8%BD%AC%E6%8D%A2)
    - [动画](#%E5%8A%A8%E7%94%BB)

## 选择器

- 元素选择器：p、div;
- ID选择器：#itemID
- 类选择器：.className
- 属性选择器：[attributeName]
- 伪类/后代：a:link、a:visited、a:hover、a:active、p:first-child、div > p:first-child、p:first-child i、a.next:visited、q:lang(en)

```c
提示： a:link 或 a:visited 之后a:hover，再之后a:active
提示：伪类名称对大小写不敏感。
```

## 盒模型

content，padding，margin，border，width，height

## 定位

`position` 属性值的含义：

**static（默认）**
    元素框正常生成。块级元素生成一个矩形框，作为文档流的一部分，行内元素则会创建一个或多个行框，置于其父元素中。

**relative**
    元素框偏移某个距离。元素仍保持其未定位前的形状，它原本所占的空间仍保留。

**absolute**
    元素框从文档流完全删除，并相对于其`包含块`定位。包含块可能是文档中的另一个元素或者是初始包含块。元素原先在正常文档流中所占的空间会关闭，就好像元素原来不存在一样。元素定位后生成一个块级框，而不论原来它在正常流中生成何种类型的框。
    `包含块指最近的一个已定位块`

**fixed**
    元素框的表现类似于将 position 设置为 absolute，不过其包含块是视窗本身。

提示：相对定位实际上被看作普通流定位模型的一部分，因为元素的位置相对于它在普通流中的位置。

### 三栏布局，左右定宽，中间自适应

窄窗口情况下，position方法会出现中间块内容被遮挡，float方法中间块内容会自动下移，中间块高度增加。

## css3高级

### 渐变色

浏览器支持：IE 10.0、Chrome 26、Firefox 16、Safari 6.1、Opera 12.1

#### 线性渐变

`background: linear-gradient(direction, color-stop1, color-stop2, ...);`

默认从上到下，可以指定方向

```css
background: linear-gradient(red, blue);
background: linear-gradient(to right, red , blue);
```

使用角度

上0度，下180度，左-90度，右90度

```css
background: linear-gradient(180deg, red, blue);
```

使用RGBA可以渲染透明色

```css
background: linear-gradient(to right, rgba(255,0,0,0), rgba(255,0,0,1));
```

多种颜色过渡，可以指定比例，可以重复渲染

```css
background: linear-gradient(to bottom right, red , blue);
background: linear-gradient(red 10%, green 85%, blue 90%);
background: repeating-linear-gradient(red, yellow 10%, green 20%);
```

#### 径向渐变

`background: radial-gradient(center, shape size, start-color, ..., last-color);`

默认情况下，渐变的中心是 center（表示在中心点），渐变的形状是 ellipse（表示椭圆形），渐变的大小是 farthest-corner（表示到最远的角落）。

设置颜色比例

```css
background: radial-gradient(red, green);
background: radial-gradient(red 5%, green 15%, blue 60%);
```

设置形状

circle 表示圆形，ellipse 表示椭圆形

```css
background: radial-gradient(circle, red, yellow, green);
```

设置大小

size 参数定义了渐变的大小。表示渐变的颜色的终点，它可以是以下四个值：

`closest-side`、`farthest-side`、`closest-corner`、`farthest-corner`，
`最近的边框`、`最远的边框`、`最近的角落`、`最远的角落`，

重复颜色

```css
background: repeating-radial-gradient(red, yellow 10%, green 15%);
```

### 阴影

支持文本阴影或者块阴影，使用逗号添加浏览器会重复渲染阴影（加深）

```css
box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.1),0 4px 8px 0 rgba(0, 0, 0, 0.1);
```

### 过渡

浏览器支持：IE 10.0、Chrome 26.0、Firefox 16.0、Safari 6.1、Opera 12.1

四个值依次为指定`变化的属性`、`变化耗时`（默认0）、`过渡效果的时间曲线`（默认ease）、`过渡效果延迟`（默认0），至少需要明确指定第一个。

可以依次指定，也可以单独指定,同时指定多个属性用逗号分隔。

变化的时机，见下面代码中的`div:hover`，表示变化后的属性。

```css
div:hover
{
  width: 200px;
  height: 200px;
}

div
{
  width: 100px;
  height: 100px;
  background: red;

  transition: width 1s linear 2s,height 1s;

  transition-property: width;
  transition-duration: 1s;
  transition-timing-function: linear;
  transition-delay: 2s;
}

```

### 2D转换

浏览器支持：IE 10.0、Chrome 36.0、Firefox 16.0、Safari 9、Opera 23.0

skew 不要写90度或-90度

```css
transform: translate(50px,100px); /* 平移 */
transform: rotate(30deg); /* 旋转 */
transform: scale(2,3); /* 缩放倍数 */
transform: skew(30deg,20deg); /* 倾斜 */
```

### 3D转换

浏览器支持：edge 12.0、Chrome 36.0、Firefox 16.0、Safari 9、Opera 23.0

3D旋转，配合`transform-style`使用可以使元素在3D空间旋转

```css
transform: rotateX(120deg);
transform: rotateY(130deg);
transform-style: preserve-3d;
```

### 动画

使用`@keyframes`或`@-webkit-keyframes`等前缀创建动画规则，使用animation将规则绑定到元素。

当动画完成时，会变回初始的样式。

```css
div
{
  width:100px;
  height:100px;
  background:red;
  animation:myfirst 5s; /* 指定规则名和动画时长 */
  animation: myfirst 5s linear 2s infinite alternate; /* 带其他参数的动画 */
  -moz-animation:myfirst 5s; /* Firefox */
  -webkit-animation:myfirst 5s; /* Safari and Chrome */
  -o-animation:myfirst 5s; /* Opera */
}
@keyframes myfirst
{
  0%   {background: red; left:0px; top:0px;}
  25%  {background: yellow; left:200px; top:0px;}
  50%  {background: blue; left:200px; top:200px;}
  75%  {background: green; left:0px; top:200px;}
  100% {background: red; left:0px; top:0px;}
}
@-webkit-keyframes myfirst
{
  0%   {background: red; left:0px; top:0px;}
  25%  {background: yellow; left:200px; top:0px;}
  50%  {background: blue; left:200px; top:200px;}
  75%  {background: green; left:0px; top:200px;}
  100% {background: red; left:0px; top:0px;}
}
```

其他可以自定义属性包括：

- `animation-duration`  规定动画完成一个周期所花费的秒或毫秒。默认是 0；
- `animation-timing-function`  规定动画的速度曲线。默认是 "ease"；
- `animation-delay`  规定动画何时开始。默认是 0；
- `animation-iteration-count`  规定动画被播放的次数。默认是 1；
- `animation-direction`  规定动画是否在下一周期逆向地播放。默认是 "normal"；
- `animation-play-state`  规定动画是否正在运行或暂停。默认是 "running"。
