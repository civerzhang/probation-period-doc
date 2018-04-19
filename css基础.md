# css基础

## 选择器

* 元素选择器：p、div
* ID选择器：#itemID
* 类选择器：.className
* 属性选择器：[attributeName]
* 伪类/后代：a:link、a:visited、a:hover、a:active、p:first-child、div > p:first-child、p:first-child i、a.next:visited、q:lang(en)

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

