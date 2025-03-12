# CSS八股

## 1、对盒子模型的理解

##### 一、是什么

当对一个文档进行布局（layout）的时候，浏览器的渲染引擎会根据标准之一的 CSS 基础框盒模型（CSS basic box model），将所有元素表示为一个个矩形的盒子（box）

一个盒子由四个部分组成：`content`、`padding`、`border`、`margin`

``content`，即实际内容，显示文本和图像

`boreder`，即边框，围绕元素内容的内边距的一条或多条线，由粗细、样式、颜色三部分组成

`padding`，即内边距，清除内容周围的区域，内边距是透明的，取值不能为负，受盒子的`background`属性影响

`margin`，即外边距，在元素外创建额外的空白，空白通常指不能放其他元素的区域



在`CSS`中，盒子模型可以分成：

- W3C 标准盒子模型
- IE 怪异盒子模型

默认情况下，盒子模型为`W3C` 标准盒子模型

##### 二、标准盒子模型

标准盒子模型，是浏览器默认的盒子模型

- 盒子总宽度 = width + padding + border + margin;
- 盒子总高度 = height + padding + border + margin

也就是，`width/height` 只是内容高度，不包含 `padding` 和 `border`值

所以上面问题中，设置`width`为200px，但由于存在`padding`，但实际上盒子的宽度有240px

##### 三、怪异盒子模型

- 盒子总宽度 = width + margin;
- 盒子总高度 = height + margin;

也就是，`width/height` 包含了 `padding`和 `border`值

##### 四、Box-sizing

CSS 中的 box-sizing 属性定义了引擎应该如何计算一个元素的总宽度和总高度

语法：

```css
box-sizing: content-box|border-box|inherit:
```

- content-box 默认值，元素的 width/height 不包含padding，border，与标准盒子模型表现一致
- border-box 元素的 width/height 包含 padding，border，与怪异盒子模型表现一致



## 2、选择器及其优先级

优先级最高的是!important

其次是内联属性（通过style设置的）

| **选择器**     | **格式**      | **优先级权重** |
| -------------- | ------------- | -------------- |
| id选择器       | #id           | 100            |
| 类选择器       | #classname    | 10             |
| 属性选择器     | a[ref=“eee”]  | 10             |
| 伪类选择器     | li:last-child | 10             |
| 标签选择器     | div           | 1              |
| 伪元素选择器   | li:after      | 1              |
| 相邻兄弟选择器 | h1+p          | 0              |
| 子选择器       | ul>li         | 0              |
| 后代选择器     | li a          | 0              |
| 通配符选择器   | *             | 0              |

关于`css`属性选择器常用的有：

- id选择器（#box），选择id为box的元素
- 类选择器（.one），选择类名为one的所有元素
- 标签选择器（div），选择标签为div的所有元素
- 后代选择器（#box div），选择id为box元素内部所有的div元素
- 子选择器（.one>one_1），选择父元素为.one的所有.one_1的元素
- 相邻同胞选择器（.one+.two），选择紧接在.one之后的所有.two元素
- 群组选择器（div,p），选择div、p的所有元素

- 伪类选择器

```css
:link ：选择未被访问的链接
:visited：选取已被访问的链接
:active：选择活动链接
:hover ：鼠标指针浮动在上面的元素
:focus ：选择具有焦点的
:first-child：父元素的首个子元素
```

- 伪元素选择器

```css
:first-letter ：用于选取指定选择器的首字母
:first-line ：选取指定选择器的首行
:before : 选择器在被选元素的内容前面插入内容
:after : 选择器在被选元素的内容后面插入内容
```

- 属性选择器

```css
[attribute] 选择带有attribute属性的元素
[attribute=value] 选择所有使用attribute=value的元素
[attribute~=value] 选择attribute属性包含value的元素
[attribute|=value]：选择attribute属性以value开头的元素
```

在`CSS3`中新增的选择器有如下：

- 层次选择器（p~ul），选择前面有p元素的每个ul元素
- 伪类选择器

```css
:first-of-type 表示一组同级元素中其类型的第一个元素
:last-of-type 表示一组同级元素中其类型的最后一个元素
:only-of-type 表示没有同类型兄弟元素的元素
:only-child 表示没有任何兄弟的元素
:nth-child(n) 根据元素在一组同级中的位置匹配元素
:nth-last-of-type(n) 匹配给定类型的元素，基于它们在一组兄弟元素中的位置，从末尾开始计数
:last-child 表示一组兄弟元素中的最后一个元素
:root 设置HTML文档
:empty 指定空的元素
:enabled 选择可用元素
:disabled 选择被禁用元素
:checked 选择选中的元素
:not(selector) 选择与 <selector> 不匹配的所有元素
```

- 属性选择器

```css
[attribute*=value]：选择attribute属性值包含value的所有元素
[attribute^=value]：选择attribute属性开头为value的所有元素
[attribute$=value]：选择attribute属性结尾为value的所有元素
```

## 3、em/px/rem/vh/vw区别

#####  px 像素

px，表示像素，所谓像素就是呈现在我们显示器上的一个个小点，每个像素点都是大小等同的，所以像素为计量单位被分在了**绝对长度单位中**

有些人会把`px`认为是相对长度，原因在于在移动端中存在设备像素比，`px`实际显示的大小是不确定的

这里之所以认为`px`为绝对单位，在于`px`的**大小和元素的其他属性无关**

##### em

em是相对长度单位。相对于**当前对象内文本的字体尺寸**。如当前对行内文本的字体尺寸未被人为设置，则相对于浏览器的默认字体尺寸（`1em = 16px`）

##### rem

rem，相对单位，相对的只是**HTML根元素`font-size`的值**

同理，如果想要简化`font-size`的转化，我们可以在根元素`html`中加入`font-size: 62.5%

这样页面中1rem=10px、1.2rem=12px、1.4rem=14px、1.6rem=16px;使得视觉、使用、书写都得到了极大的帮助

##### vh、vw

vw ，就是根据窗口的宽度，分成100等份，100vw就表示满宽，50vw就表示一半宽。（vw 始终是针对窗口的宽），同理，`vh`则为窗口的高度

这里的窗口分成几种情况：

- 在桌面端，指的是浏览器的可视区域
- 移动端指的就是布局视口

像`vw`、`vh`，比较容易混淆的一个单位是`%`，不过百分比宽泛的讲是相对于父元素：

- 对于普通定位元素就是我们理解的父元素
- 对于position: absolute;的元素是相对于已定位的父元素
- 对于position: fixed;的元素是相对于 ViewPort（可视窗口）

##### % 百分比

宽度或高度相对于父盒子占百分之多少

##### 总结

**px**：绝对单位，页面按精确像素展示

**em**：相对单位，基准点为父节点字体的大小，如果自身定义了`font-size`按自身来计算，整个页面内`1em`不是一个固定的值

**rem**：相对单位，可理解为`root em`, 相对根节点`html`的字体大小来计算

**vh、vw**：主要用于页面视口大小布局，在页面布局上更加方便简单

## 4、css中，有哪些方式可以隐藏页面元素？区别?

通过`css`实现隐藏元素方法有如下：

- display:none
- visibility:hidden
- opacity:0

##### display:none

设置元素的`display`为`none`是最常用的隐藏元素的方法

```css
.hide {
    display:none;
}
```

将元素设置为`display:none`后，元素在页面上将**彻底消失**

元素本身占有的空间就会被其他元素占有，也就是说它会导致浏览器的**重排**和**重绘**

消失后，自身绑定的**事件不会触发**，也不会有过渡效果

特点：元素**不可见**，**不占据空间**，**无法响应**点击事件



##### [#](https://vue3js.cn/interview/css/hide_attributes.html#visibility-hidden)visibility:hidden

设置元素的`visibility`为`hidden`也是一种常用的隐藏元素的方法

从页面上**仅仅是隐藏该元素**，DOM结果均会**存在**，只是当时在一个**不可见的状态**，**不会触发重排**，但是会触发**重绘**

```css
.hidden{
    visibility:hidden
}
```

给人的效果是隐藏了，所以他自身的事件不会触发

特点：元素**不可见**，**占据**页面空间，**无法响应**点击事件



##### [#](https://vue3js.cn/interview/css/hide_attributes.html#opacity-0)opacity:0

`opacity`属性表示元素的透明度，将元素的透明度设置为0后，在我们用户眼中，元素也是隐藏的

不会引发重排，一般情况下也会引发重绘

> 如果利用 animation 动画，对 opacity 做变化（animation会默认触发GPU加速），则只会触发 GPU 层面的 composite，不会触发重绘

```css
.transparent {
    opacity:0;
}
```

由于其仍然是存在于页面上的，所以他自身的的事件仍然是可以触发的，但被他遮挡的元素是不能触发其事件的

需要注意的是：其子元素不能设置opacity来达到显示的效果

特点：改变元素透明度，元素不可见，**占据页面空间**，可以**响应点击事件**

## 5、CSS中的BFC概念与应用

CSS中的BFC（Block Formatting Context，块级格式化上下文）是页面上的一个独立渲染区域，具有特定的布局规则。它使得内部元素的布局与外部隔离，**避免**了**样式冲突**或意外影响。

##### **触发BFC的条件**

元素满足以下任意条件即可创建BFC：



​	根元素（`<html>`）

- `float` 不为 `none`（如 `left`/`right`）
- `position` 为 `absolute` 或 `fixed`
- `overflow` 不为 `visible`（如 `hidden`/`auto`/`scroll`）
- `display` 为 `inline-block`、`table-cell`、`flex`、`grid`、`flow-root(这个无副作用，就代表一个BFC容器)` 等
- `contain` 为 `layout`、`content`、`paint`（较新属性）

##### BFC的特性与作用

##### **1.阻止垂直外边距合并（Margin Collapse）**

- **问题场景**：标准流中，相邻块级元素的垂直外边距会合并（取最大值）。例如，上下两个 `<div>` 的 `margin: 20px` 会合并为 20px 的间距，而不是 40px。

- **BFC 的作用**：将某一个元素放入不同的 BFC 容器中，可阻止外边距合并。

- **示例代码**：

  ```js
  <div class="box">上盒子（margin: 20px）</div>
  <div class="container">
    <div class="box">下盒子（margin: 20px）</div>
  </div>
  <style>
    .container { overflow: hidden; } /* 触发 BFC */
    .box { margin: 20px; }
  </style>
  ```

  **效果**：两个 `.box` 之间的间距变为 40px（20px + 20px），而非合并后的 20px。

------

##### 2. **包含浮动元素（清除浮动）**

- **问题场景**：父元素未设置高度时，若子元素浮动，父元素会高度塌陷（高度变为 0）。

- **BFC 的作用**：触发 BFC 的父元素会计算浮动子元素的高度，避免塌陷。

- **示例代码**：

  html

  复制

  ```js
  <div class="parent">
    <div class="float-child"></div>
  </div>
  <style>
    .parent { overflow: hidden; } /* 触发 BFC，包裹浮动元素 */
    .float-child { float: left; width: 100px; height: 100px; }
  </style>
  ```

  

  运行 HTML

  **效果**：父元素 `.parent` 高度变为 100px（与子元素一致），而非 0。

------

##### 3. **阻止元素被浮动元素覆盖**

- **问题场景**：浮动元素会脱离文档流，后续非浮动元素可能环绕或部分被覆盖。

- **BFC 的作用**：触发 BFC 的元素不会与浮动元素重叠，而是并列显示。

- **示例代码**：

  ```js
  <div class="float-left"></div>
  <div class="bfc-container"></div>
  <style>
    .float-left { float: left; width: 100px; height: 100px; }
    .bfc-container { overflow: hidden; } /* 触发 BFC，避免与浮动元素重叠 */
  </style>
  ```

  **效果**：`.bfc-container` 会占据浮动元素右侧剩余空间，形成两栏布局。

------

##### 4. **独立布局环境**

- **特性说明**：BFC 内部元素的布局不会影响外部元素，反之亦然。

- **典型应用**：避免外部浮动影响内部布局，或内部浮动影响外部。

- **示例场景**：

  ```js
  <div class="float-parent">
    <div class="child"></div>
  </div>
  <style>
    .float-parent { float: left; }
    .child { height: 200px; } /* 父元素浮动可能导致外部布局混乱 */
  </style>
  ```

  **修复方案**：为父元素触发 BFC（如 `overflow: hidden`），使其成为一个独立容器。

##### 二、BFC 的实际应用场景

##### 1. **自适应两栏布局**

- **需求**：左侧固定宽度，右侧自适应填充。

- **实现**：

  ```
  <div class="left">左侧浮动</div>
  <div class="right">右侧自适应</div>
  <style>
    .left { float: left; width: 200px; }
    .right { overflow: hidden; } /* 触发 BFC，避免与浮动元素重叠 */
  </style>
  ```

------

##### 2. **避免浮动导致的文字环绕**

- **问题**：浮动图片后，文字会环绕图片。

- **解决**：为文字容器触发 BFC，使其独立成块。

  ```
  <div class="image">浮动图片</div>
  <div class="text">一段很长的文字...</div>
  <style>
    .image { float: left; }
    .text { overflow: hidden; } /* 触发 BFC，避免文字环绕 */
  </style>
  ```

------

#### 3. **隔离第三方组件样式**

- **场景**：引入第三方组件时，防止其样式影响父容器。
- **方案**：为组件容器触发 BFC（如 `display: flow-root`），形成独立作用域。



## 6、元素水平垂直居中的方法有哪些

实现元素水平垂直居中的方式：

- 根据元素标签的性质，可以分为：

  - 行内元素居中布局
  - 块级元素居中布局

  ### [#](https://vue3js.cn/interview/css/center.html#内联元素居中布局)元素内部行内元素居中布局

  水平居中

  - 行内元素可设置：text-align: center
  - flex布局设置父元素：display: flex; justify-content: center

  垂直居中

  - 单行文本父元素确认高度：height === line-height，

  - 多行文本父元素确认高度：display: flex; align-items: center; 

    ​						vertical-align: middle

  ### [#](https://vue3js.cn/interview/css/center.html#块级元素居中布局)块级元素居中布局

  水平居中

  - 给定宽度: margin: 0 auto

    ​		绝对定位+left: 50%+margin-left: - 自身宽度/2

  - 不给定宽度 绝对定位+left: 50%+transform: translate(-50%,-50%);

  垂直居中相同

## 7、如何实现两栏布局，右侧自适应？三栏布局中间自适应呢？

### 两栏布局

两栏布局非常常见，往往是以一个定宽栏和一个自适应的栏并排展示存在

实现思路也非常的简单：

- 使用 float 左浮左边栏
- 右边模块使用 margin-left 撑出内容块做内容展示

还有一种更为简单的使用则是采取：flex弹性布局

父盒子display: flex

左侧子盒子规定固定宽度，右侧子盒子flex: 1

### 三栏布局

实现三栏布局中间自适应的布局方式有：

- 两边使用 float，中间使用 margin
- 两边使用 absolute，中间使用 margin

## 8、说说flexbox（弹性盒布局模型）,以及适用场景

### 属性

关于`flex`常用的属性，我们可以划分为容器属性和容器成员属性

容器属性有：

- flex-direction
- flex-wrap
- flex-flow
- justify-content
- align-items
- align-content

### [#](https://vue3js.cn/interview/css/flexbox.html#flex-direction)flex-direction

决定主轴的方向(即项目的排列方向)

```css
.container {   
    flex-direction: row | row-reverse | column | column-reverse;  
} 
```

属性对应如下：

- row（默认值）：主轴为水平方向，起点在左端
- row-reverse：主轴为水平方向，起点在右端
- column：主轴为垂直方向，起点在上沿。
- column-reverse：主轴为垂直方向，起点在下沿

如下图所示：

![img](https://static.vue-js.com/0c9abc70-9838-11eb-ab90-d9ae814b240d.png)

### [#](https://vue3js.cn/interview/css/flexbox.html#flex-wrap)flex-wrap

弹性元素永远沿主轴排列，那么如果主轴排不下，通过`flex-wrap`决定容器内项目是否可换行

```css
.container {  
    flex-wrap: nowrap | wrap | wrap-reverse;
}  
```

应如下：

- nowrap（默认值）：不换行
- wrap：换行，第一行在下方
- wrap-reverse：换行，第一行在上方

默认情况是不换行，但这里也不会任由元素直接溢出容器，会涉及到元素的弹性伸缩

### [#](https://vue3js.cn/interview/css/flexbox.html#flex-flow)flex-flow

是`flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap`

```css
.box {
  flex-flow: <flex-direction> || <flex-wrap>;
}
```

### [#](https://vue3js.cn/interview/css/flexbox.html#justify-content)justify-content

定义了项目在主轴上的对齐方式

```css
.box {
    justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

属性对应如下：

- flex-start（默认值）：左对齐
- flex-end：右对齐
- center：居中
- space-between：两端对齐，项目之间的间隔都相等
- space-around：两个项目两侧间隔相等

效果图如下：

![img](https://static.vue-js.com/2d5ca950-9838-11eb-85f6-6fac77c0c9b3.png)

### [#](https://vue3js.cn/interview/css/flexbox.html#align-items)align-items

定义项目在交叉轴上如何对齐

```css
.box {
  align-items: flex-start | flex-end | center | baseline | stretch;
}
```

属性对应如下：

- flex-start：交叉轴的起点对齐
- flex-end：交叉轴的终点对齐
- center：交叉轴的中点对齐
- baseline: 项目的第一行文字的基线对齐
- stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度

### [#](https://vue3js.cn/interview/css/flexbox.html#align-content)align-content

定义了多根轴线的对齐方式。如果项目只有一根轴线，该属性不起作用

```css
.box {
    align-content: flex-start | flex-end | center | space-between | space-around | stretch;
}
```

属性对应如吓：

- flex-start：与交叉轴的起点对齐
- flex-end：与交叉轴的终点对齐
- center：与交叉轴的中点对齐
- space-between：与交叉轴两端对齐，轴线之间的间隔平均分布
- space-around：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍
- stretch（默认值）：轴线占满整个交叉轴

效果图如下：

![img](https://static.vue-js.com/39bcb0f0-9838-11eb-ab90-d9ae814b240d.png)

容器成员属性如下：

- `order`
- `flex-grow`
- `flex-shrink`
- `flex-basis`
- `flex`
- `align-self`

### [#](https://vue3js.cn/interview/css/flexbox.html#order)order

定义项目的排列顺序。数值越小，排列越靠前，默认为0

```css
.item {
    order: <integer>;
}
```

1
2
3

### [#](https://vue3js.cn/interview/css/flexbox.html#flex-grow)flex-grow

上面讲到当容器设为`flex-wrap: nowrap;`不换行的时候，容器宽度有不够分的情况，弹性元素会根据`flex-grow`来决定

定义项目的放大比例（容器宽度>元素总宽度时如何伸展）

默认为`0`，即如果存在剩余空间，也不放大

```css
.item {
    flex-grow: <number>;
}
```

如果所有项目的`flex-grow`属性都为1，则它们将等分剩余空间（如果有的话）

![img](https://static.vue-js.com/48c8c5c0-9838-11eb-ab90-d9ae814b240d.png)

如果一个项目的`flex-grow`属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍

![img](https://static.vue-js.com/5b822b20-9838-11eb-ab90-d9ae814b240d.png)

弹性容器的宽度正好等于元素宽度总和，无多余宽度，此时无论`flex-grow`是什么值都不会生效

### [#](https://vue3js.cn/interview/css/flexbox.html#flex-shrink)flex-shrink

定义了项目的缩小比例（容器宽度<元素总宽度时如何收缩），默认为1，即如果空间不足，该项目将缩小

```css
.item {
    flex-shrink: <number>; /* default 1 */
}
```

如果所有项目的`flex-shrink`属性都为1，当空间不足时，都将等比例缩小

如果一个项目的`flex-shrink`属性为0，其他项目都为1，则空间不足时，前者不缩小

![img](https://static.vue-js.com/658c5be0-9838-11eb-85f6-6fac77c0c9b3.png)

在容器宽度有剩余时，`flex-shrink`也是不会生效的

### [#](https://vue3js.cn/interview/css/flexbox.html#flex-basis)flex-basis

设置的是元素在主轴上的初始尺寸，所谓的初始尺寸就是元素在`flex-grow`和`flex-shrink`生效前的尺寸

浏览器根据这个属性，计算主轴是否有多余空间，默认值为`auto`，即项目的本来大小，如设置了`width`则元素尺寸由`width/height`决定（主轴方向），没有设置则由内容决定

```css
.item {
   flex-basis: <length> | auto; /* default auto */
}
```

当设置为0的是，会根据内容撑开

它可以设为跟`width`或`height`属性一样的值（比如350px），则项目将占据固定空间

### [#](https://vue3js.cn/interview/css/flexbox.html#flex)flex

`flex`属性是`flex-grow`, `flex-shrink` 和 `flex-basis`的简写，默认值为`0 1 auto`，也是比较难懂的一个复合属性

```css
.item {
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```

一些属性有：

- flex: 1 = flex: 1 1 0%
- flex: 2 = flex: 2 1 0%
- flex: auto = flex: 1 1 auto
- flex: none = flex: 0 0 auto，常用于固定尺寸不伸缩

`flex:1` 和 `flex:auto` 的区别，可以归结于`flex-basis:0`和`flex-basis:auto`的区别

当设置为0时（绝对弹性元素），此时相当于告诉`flex-grow`和`flex-shrink`在伸缩的时候不需要考虑我的尺寸

当设置为`auto`时（相对弹性元素），此时则需要在伸缩时将元素尺寸纳入考虑

注意：建议优先使用这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值

### [#](https://vue3js.cn/interview/css/flexbox.html#align-self)align-self

允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`属性

默认值为`auto`，表示继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch`

```css
.item {
    align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

效果图如下：

![img](https://static.vue-js.com/6f8304a0-9838-11eb-85f6-6fac77c0c9b3.png)

## [#](https://vue3js.cn/interview/css/flexbox.html#三、应用场景)三、应用场景

在以前的文章中，我们能够通过`flex`简单粗暴的实现元素水平垂直方向的居中，以及在两栏三栏自适应布局中通过`flex`完成，这里就不再展开代码的演示

包括现在在移动端、小程序这边的开发，都建议使用`flex`进行布局

## 9、CSS3新增了哪些新特性？

### **一、布局与盒模型**

1. **弹性布局（Flexbox）**
   支持一维布局，灵活控制子元素的排列、对齐和空间分配，适用于组件内部排列36。
   - 示例：`display: flex; justify-content: space-between;`
2. **网格布局（Grid）**
   二维布局系统，支持行和列的定义，适合复杂页面结构36。
   - 示例：`display: grid; grid-template-columns: repeat(3, 1fr);`
3. **盒模型调整**
   - `box-sizing`: 支持 `content-box`（默认）和 `border-box`（包含边框和内边距），简化尺寸计算310。
   - 多列布局：通过 `column-count` 和 `column-gap` 实现类似报纸的分栏效果7[citation:9] 。

### 二、选择器

- 层次选择器（p~ul），选择前面有p元素的每个ul元素
- 伪类选择器

```css
:first-of-type 表示一组同级元素中其类型的第一个元素
:last-of-type 表示一组同级元素中其类型的最后一个元素
:only-of-type 表示没有同类型兄弟元素的元素
:only-child 表示没有任何兄弟的元素
:nth-child(n) 根据元素在一组同级中的位置匹配元素
:nth-last-of-type(n) 匹配给定类型的元素，基于它们在一组兄弟元素中的位置，从末尾开始计数
:last-child 表示一组兄弟元素中的最后一个元素
:root 设置HTML文档
:empty 指定空的元素
:enabled 选择可用元素
:disabled 选择被禁用元素
:checked 选择选中的元素
:not(selector) 选择与 <selector> 不匹配的所有元素
```

- 属性选择器

```css
[attribute*=value]：选择attribute属性值包含value的所有元素
[attribute^=value]：选择attribute属性开头为value的所有元素
[attribute$=value]：选择attribute属性结尾为value的所有元素
```

## 

### [#](https://vue3js.cn/interview/css/css3_features.html#三、新样式)三、新样式

#### [#](https://vue3js.cn/interview/css/css3_features.html#边框)边框

`css3`新增了三个边框属性，分别是：

- border-radius：创建圆角边框
- box-shadow：为元素添加阴影
- border-image：使用图片来绘制边框

##### [#](https://vue3js.cn/interview/css/css3_features.html#box-shadow)box-shadow

设置元素阴影，设置属性如下：

- 水平阴影
- 垂直阴影
- 模糊距离(虚实)
- 阴影尺寸(影子大小)
- 阴影颜色
- 内/外阴影

其中水平阴影和垂直阴影是必须设置的

#### [#](https://vue3js.cn/interview/css/css3_features.html#背景)背景

新增了几个关于背景的属性，分别是`background-clip`、`background-origin`、`background-size`和`background-break`

##### [#](https://vue3js.cn/interview/css/css3_features.html#background-clip)background-clip

用于确定背景画区，有以下几种可能的属性：

- background-clip: border-box; 背景从border开始显示
- background-clip: padding-box; 背景从padding开始显示
- background-clip: content-box; 背景显content区域开始显示
- background-clip: no-clip; 默认属性，等同于border-box

通常情况，背景都是覆盖整个元素的，利用这个属性可以设定背景颜色或图片的覆盖范围

##### [#](https://vue3js.cn/interview/css/css3_features.html#background-origin)background-origin

当我们设置背景图片时，图片是会以左上角对齐，但是是以`border`的左上角对齐还是以`padding`的左上角或者`content`的左上角对齐? `border-origin`正是用来设置这个的

- background-origin: border-box; 从border开始计算background-position
- background-origin: padding-box; 从padding开始计算background-position
- background-origin: content-box; 从content开始计算background-position

默认情况是`padding-box`，即以`padding`的左上角为原点

##### [#](https://vue3js.cn/interview/css/css3_features.html#background-size)background-size

background-size属性常用来调整背景图片的大小，主要用于设定图片本身。有以下可能的属性：

- background-size: contain; 缩小图片以适合元素（维持像素长宽比）
- background-size: cover; 扩展元素以填补元素（维持像素长宽比）
- background-size: 100px 100px; 缩小图片至指定的大小
- background-size: 50% 100%; 缩小图片至指定的大小，百分比是相对包 含元素的尺寸

##### [#](https://vue3js.cn/interview/css/css3_features.html#background-break)background-break

元素可以被分成几个独立的盒子（如使内联元素span跨越多行），`background-break` 属性用来控制背景怎样在这些不同的盒子中显示

- background-break: continuous; 默认值。忽略盒之间的距离（也就是像元素没有分成多个盒子，依然是一个整体一样）
- background-break: bounding-box; 把盒之间的距离计算在内；
- background-break: each-box; 为每个盒子单独重绘背景

#### [#](https://vue3js.cn/interview/css/css3_features.html#文字)文字

##### [#](https://vue3js.cn/interview/css/css3_features.html#word-wrap)word-wrap

语法：`word-wrap: normal|break-word`

- normal：使用浏览器默认的换行
- break-all：允许在单词内换行

##### [#](https://vue3js.cn/interview/css/css3_features.html#text-overflow)text-overflow

`text-overflow`设置或检索当当前行超过指定容器的边界时如何显示，属性有两个值选择：

- clip：修剪文本
- ellipsis：显示省略符号来代表被修剪的文本

##### [#](https://vue3js.cn/interview/css/css3_features.html#text-shadow)text-shadow

`text-shadow`可向文本应用阴影。能够规定水平阴影、垂直阴影、模糊距离，以及阴影的颜色

##### [#](https://vue3js.cn/interview/css/css3_features.html#text-decoration)text-decoration

CSS3里面开始支持对文字的更深层次的渲染，具体有三个属性可供设置：

- text-fill-color: 设置文字内部填充颜色
- text-stroke-color: 设置文字边界填充颜色
- text-stroke-width: 设置文字边界宽度

#### [#](https://vue3js.cn/interview/css/css3_features.html#颜色)颜色

```
css3`新增了新的颜色表示方式`rgba`与`hsla
```

- rgba分为两部分，rgb为颜色值，a为透明度
- hala分为四部分，h为色相，s为饱和度，l为亮度，a为透明度

### [#](https://vue3js.cn/interview/css/css3_features.html#四、transition-过渡)四、transition 过渡

`transition`属性可以被指定为一个或多个`CSS`属性的过渡效果，多个属性之间用逗号进行分隔，必须规定两项内容：

- 过度效果
- 持续时间

语法如下：

```css
transition： CSS属性，花费时间，效果曲线(默认ease)，延迟时间(默认0)
```

上面为简写模式，也可以分开写各个属性

```css
transition-property: width; 
transition-duration: 1s;
transition-timing-function: linear;
transition-delay: 2s;
```

### [#](https://vue3js.cn/interview/css/css3_features.html#五、transform-转换)五、transform 转换

`transform`属性允许你旋转，缩放，倾斜或平移给定元素

```
transform-origin`：转换元素的位置（围绕那个点进行转换），默认值为`(x,y,z):(50%,50%,0)
```

使用方式：

- transform: translate(120px, 50%)：位移
- transform: scale(2, 0.5)：缩放
- transform: rotate(0.5turn)：旋转
- transform: skew(30deg, 20deg)：倾斜

### [#](https://vue3js.cn/interview/css/css3_features.html#六、animation-动画)六、animation 动画

动画这个平常用的也很多，主要是做一个预设的动画。和一些页面交互的动画效果，结果和过渡应该一样，让页面不会那么生硬

animation也有很多的属性

- animation-name：动画名称
- animation-duration：动画持续时间
- animation-timing-function：动画时间函数
- animation-delay：动画延迟时间
- animation-iteration-count：动画执行次数，可以设置为一个整数，也可以设置为infinite，意思是无限循环
- animation-direction：动画执行方向
- animation-paly-state：动画播放状态
- animation-fill-mode：动画填充模式

## [#](https://vue3js.cn/interview/css/css3_features.html#七、渐变)七、渐变

颜色渐变是指在两个颜色之间平稳的过渡，`css3`渐变包括

- linear-gradient：线性渐变

> background-image: linear-gradient(direction, color-stop1, color-stop2, ...);

- radial-gradient：径向渐变

> linear-gradient(0deg, red, green);

### 八、媒体查询

1. **媒体查询（Media Queries）**
   根据设备特性（如屏幕宽度）调整样式，支持响应式设计36。
   - 示例：`@media (max-width: 768px) { ... }`
2. **滤镜效果（Filter）**
   支持模糊（`blur`）、亮度（`brightness`）等图像处理效果6。

## 10、css3动画有哪些？

![img](https://static.vue-js.com/d12e2380-9c0a-11eb-ab90-d9ae814b240d.png)

### [#](https://vue3js.cn/interview/css/animation.html#一、是什么)一、是什么

CSS动画（CSS Animations）是为层叠样式表建议的允许可扩展标记语言（XML）元素使用CSS的动画的模块

即指元素从一种样式逐渐过渡为另一种样式的过程

常见的动画效果有很多，如平移、旋转、缩放等等，复杂动画则是多个简单动画的组合

`css`实现动画的方式，有如下几种：

- transition 实现渐变动画
- transform 转变动画
- animation 实现自定义动画

### [#](https://vue3js.cn/interview/css/animation.html#二、实现方式)二、实现方式

#### [#](https://vue3js.cn/interview/css/animation.html#transition-实现渐变动画)transition 实现渐变动画

`transition`的属性如下：

- property:填写需要变化的css属性
- duration:完成过渡效果需要的时间单位(s或者ms)
- timing-function:完成效果的速度曲线
- delay: 动画效果的延迟触发时间

其中`timing-function`的值有如下：

| 值                            | 描述                                                         |
| ----------------------------- | ------------------------------------------------------------ |
| linear                        | 匀速（等于 cubic-bezier(0,0,1,1)）                           |
| ease                          | 从慢到快再到慢（cubic-bezier(0.25,0.1,0.25,1)）              |
| ease-in                       | 慢慢变快（等于 cubic-bezier(0.42,0,1,1)）                    |
| ease-out                      | 慢慢变慢（等于 cubic-bezier(0,0,0.58,1)）                    |
| ease-in-out                   | 先变快再到慢（等于 cubic-bezier(0.42,0,0.58,1)），渐显渐隐效果 |
| cubic-bezier(*n*,*n*,*n*,*n*) | 在 cubic-bezier 函数中定义自己的值。可能的值是 0 至 1 之间的数值 |

注意：并不是所有的属性都能使用过渡的，如`display:none<->display:block`

举个例子，实现鼠标移动上去发生变化动画效果

```html
<style>
       .base {
            width: 100px;
            height: 100px;
            display: inline-block;
            background-color: #0EA9FF;
            border-width: 5px;
            border-style: solid;
            border-color: #5daf34;
            transition-property: width, height, background-color, border-width;
            transition-duration: 2s;
            transition-timing-function: ease-in;
            transition-delay: 500ms;
        }

        /*简写*/
        /*transition: all 2s ease-in 500ms;*/
        .base:hover {
            width: 200px;
            height: 200px;
            background-color: #5daf34;
            border-width: 10px;
            border-color: #3a8ee6;
        }
</style>
<div class="base"></div>
```

#### [#](https://vue3js.cn/interview/css/animation.html#transform-转变动画)transform 转变动画

包含四个常用的功能：

- translate：位移
- scale：缩放
- rotate：旋转
- skew：倾斜

一般配合`transition`过度使用

注意的是，`transform`不支持`inline`元素，使用前把它变成`block`

举个例子

```html
<style>
    .base {
        width: 100px;
        height: 100px;
        display: inline-block;
        background-color: #0EA9FF;
        border-width: 5px;
        border-style: solid;
        border-color: #5daf34;
        transition-property: width, height, background-color, border-width;
        transition-duration: 2s;
        transition-timing-function: ease-in;
        transition-delay: 500ms;
    }
    .base2 {
        transform: none;
        transition-property: transform;
        transition-delay: 5ms;
    }

    .base2:hover {
        transform: scale(0.8, 1.5) rotate(35deg) skew(5deg) translate(15px, 25px);
    }
</style>
 <div class="base base2"></div>
```

可以看到盒子发生了旋转，倾斜，平移，放大

#### [#](https://vue3js.cn/interview/css/animation.html#animation-实现自定义动画)animation 实现自定义动画

`animation`是由 8 个属性的简写，分别如下：

| 属性                                   | 描述                                                         | 属性值                                        |
| -------------------------------------- | ------------------------------------------------------------ | --------------------------------------------- |
| animation-duration                     | 指定动画完成一个周期所需要时间，单位秒（s）或毫秒（ms），默认是 0 |                                               |
| animation-timing-function              | 指定动画计时函数，即动画的速度曲线，默认是 "ease"            | linear、ease、ease-in、ease-out、ease-in-out  |
| animation-delay                        | 指定动画延迟时间，即动画何时开始，默认是 0                   |                                               |
| animation-iteration-count              | 指定动画播放的次数，默认是 1                                 |                                               |
| animation-direction 指定动画播放的方向 | 默认是 normal                                                | normal、reverse、alternate、alternate-reverse |
| animation-fill-mode                    | 指定动画填充模式。默认是 none                                | forwards、backwards、both                     |
| animation-play-state                   | 指定动画播放状态，正在运行或暂停。默认是 running             | running、pauser                               |
| animation-name                         | 指定 @keyframes 动画的名称                                   |                                               |

`CSS` 动画只需要定义一些关键的帧，而其余的帧，浏览器会根据计时函数插值计算出来，

通过 `@keyframes` 来定义关键帧

因此，如果我们想要让元素旋转一圈，只需要定义开始和结束两帧即可：

```css
@keyframes rotate{
    from{
        transform: rotate(0deg);
    }
    to{
        transform: rotate(360deg);
    }
}
```

`from` 表示最开始的那一帧，`to` 表示结束时的那一帧

也可以使用百分比刻画生命周期

```css
@keyframes rotate{
    0%{
        transform: rotate(0deg);
    }
    50%{
        transform: rotate(180deg);
    }
    100%{
        transform: rotate(360deg);
    }
}
```

定义好了关键帧后，下来就可以直接用它了：

```css
animation: rotate 2s;
```

### [#](https://vue3js.cn/interview/css/animation.html#三、总结)三、总结

| 属性               | 含义                                                         |
| ------------------ | ------------------------------------------------------------ |
| transition（过度） | 用于设置元素的样式过度，和animation有着类似的效果，但细节上有很大的不同 |
| transform（变形）  | 用于元素进行旋转、缩放、移动或倾斜，和设置样式的动画并没有什么关系，就相当于color一样用来设置元素的“外表” |
| translate（移动）  | 只是transform的一个属性值，即移动                            |
| animation（动画）  | 用于设置动画属性，他是一个简写的属性，包含6个属性            |

## 11、怎么理解回流跟重绘？什么场景下会触发？

### 一、是什么

在`HTML`中，每个元素都可以理解成一个盒子，在浏览器解析过程中，会涉及到回流与重绘：

- 回流：布局引擎会根据各种样式计算每个盒子在页面上的**大小**与**位置**
- 重绘：当计算好盒模型的位置、大小及其他属性后，浏览器根据**每个盒子特性进行绘制**

具体的浏览器解析渲染机制如下所示：

![img](https://static.vue-js.com/2b56a950-9cdc-11eb-ab90-d9ae814b240d.png)

- 解析HTML，生成DOM树，解析CSS，生成CSSOM树
- 将DOM树和CSSOM树结合，生成渲染树(Render Tree)
- Layout(回流):根据生成的渲染树，进行回流(Layout)，得到节点的几何信息（位置，大小）
- Painting(重绘):根据渲染树以及回流得到的几何信息，得到节点的绝对像素
- Display:将像素发送给GPU，展示在页面上

在页面初始渲染阶段，回流不可避免的触发，可以理解成页面一开始是空白的元素，后面添加了新的元素使页面布局发生改变

当我们对 `DOM` 的修改引发了 `DOM`几何尺寸的变化（比如修改元素的宽、高或隐藏元素等）时，浏览器需要重新计算元素的几何属性，然后再将计算的结果绘制出来

当我们对 `DOM`的修改导致了样式的变化（`color`或`background-color`），却并未影响其几何属性时，浏览器不需重新计算元素的几何属性、直接为该元素绘制新的样式，这里就仅仅触发了重绘

### [#](https://vue3js.cn/interview/css/layout_painting.html#二、如何触发)二、如何触发

要想减少回流和重绘的次数，首先要了解回流和重绘是如何触发的

#### [#](https://vue3js.cn/interview/css/layout_painting.html#回流触发时机)回流触发时机

回流这一阶段主要是计算节点的位置和几何信息，那么当页面布局和几何信息发生变化的时候，就需要回流，如下面情况：

- 添加或删除可见的DOM元素
- 元素的位置发生变化
- 元素的尺寸发生变化（包括外边距、内边框、边框大小、高度和宽度等）
- 内容发生变化，比如文本变化或图片被另一个不同尺寸的图片所替代
- 页面一开始渲染的时候（这避免不了）
- 浏览器的窗口尺寸变化（因为回流是根据视口的大小来计算元素的位置和大小的）

还有一些容易被忽略的操作：获取一些特定属性的值

> offsetTop、offsetLeft、 offsetWidth、offsetHeight、scrollTop、scrollLeft、scrollWidth、scrollHeight、clientTop、clientLeft、clientWidth、clientHeight

这些属性有一个共性，就是需要通过即时计算得到。因此浏览器为了获取这些值，也会进行回流

除此还包括`getComputedStyle`方法，原理是一样的

#### [#](https://vue3js.cn/interview/css/layout_painting.html#重绘触发时机)重绘触发时机

触发回流一定会触发重绘

可以把页面理解为一个黑板，黑板上有一朵画好的小花。现在我们要把这朵从左边移到了右边，那我们要先确定好右边的具体位置，画好形状（回流），再画上它原有的颜色（重绘）

除此之外还有一些其他引起重绘行为：

- 颜色的修改
- 文本方向的修改
- 阴影的修改

#### [#](https://vue3js.cn/interview/css/layout_painting.html#浏览器优化机制)浏览器优化机制

由于每次重排都会造成额外的计算消耗，因此大多数浏览器都会通过**队列化修改并批量执行**来优化重排过程。浏览器会将修改操作放入到队列里，直到过了一段时间或者操作达到了一个**阈值**，才清空队列

当你获取布局信息的操作的时候，会强制队列刷新，包括前面讲到的`offsetTop`等方法都会返回最新的数据

因此浏览器不得不清空队列，触发回流重绘来返回正确的值

### [#](https://vue3js.cn/interview/css/layout_painting.html#三、如何减少)三、如何减少

#### **1. 避免频繁操作样式**

- **合并样式修改**：直接操作元素的 `style` 属性会触发多次回流/重绘。改用 `classList` 或一次性修改 `cssText`：

  ```
  // 不推荐（多次触发）
  element.style.width = '100px';
  element.style.height = '200px';
  
  // 推荐（一次触发）
  element.style.cssText = 'width:100px; height:200px;';
  // 或通过切换类名
  element.classList.add('new-style');
  ```

- **使用 `requestAnimationFrame`**：将动画相关的样式修改放在 `requestAnimationFrame` 中，确保修改在浏览器渲染前完成。

------

#### **2. 批量操作 DOM**

- **离线操作**：将元素暂时脱离文档流，修改完成后再重新插入：

  ```
  const fragment = document.createDocumentFragment();
  // 批量操作 fragment 中的元素
  document.body.appendChild(fragment);
  ```

- **隐藏元素后操作**：先设置 `display: none`，修改完再显示（仅触发两次回流）。

------

#### **3. 避免强制同步布局（Layout Thrashing）**

- **缓存布局信息**：避免在修改样式后立即读取布局属性（如 `offsetTop`、`getComputedStyle`），这会导致浏览器强制同步计算布局：

  ```
  // 不推荐（强制同步布局）
  const width = element.offsetWidth; // 读取
  element.style.width = width + 10 + 'px'; // 写入
  const height = element.offsetHeight; // 再次读取（触发回流）
  
  // 推荐：先读取后写入
  const width = element.offsetWidth;
  const height = element.offsetHeight;
  element.style.width = width + 10 + 'px';
  element.style.height = height + 10 + 'px';
  ```

------

#### **4. 使用 CSS 优化**

- **优先使用 `transform` 和 `opacity`**：这两个属性不会触发回流/重绘（由 GPU 加速合成层）。

  ```
  .box {
    transform: translateX(100px); /* 使用 transform 替代 left */
    opacity: 0.5; /* 使用 opacity 替代 visibility */
  }
  ```

- **避免使用表格布局**：表格的渲染成本较高，局部更新可能触发全局回流。

- **使用 `flex` 或 `grid` 布局**：现代布局方式性能更优。

------

#### **5. 优化 CSS 选择器**

- 减少嵌套层级，避免过于复杂的选择器：

  ```
  /* 不推荐 */
  div > ul > li > a { ... }
  
  /* 推荐 */
  .link { ... }
  ```

------

#### **6. 使用硬件加速**

- 将动画元素提升到独立的合成层，减少重绘范围：

  ```
  .animate {
    will-change: transform; /* 提前告知浏览器 */
    transform: translateZ(0); /* 触发 GPU 加速 */
  }
  ```

------

#### **7. 其他优化**

- **避免逐帧修改样式**：例如通过 `setInterval` 频繁调整尺寸，改用 CSS 动画或过渡（`transition`/`animation`）。
- **使用防抖/节流**：控制 `resize`、`scroll` 等高频事件的回调频率。
- **虚拟 DOM 框架**：React/Vue 等框架通过 Diff 算法批量 DOM 更新。



但有时候，我们会无可避免地进行回流或者重绘，我们可以更好使用它们

例如，多次修改一个把元素布局的时候，我们很可能会如下操作

```js
const el = document.getElementById('el')
for(let i=0;i<10;i++) {
    el.style.top  = el.offsetTop  + 10 + "px";
    el.style.left = el.offsetLeft + 10 + "px";
}
```

每次循环都需要获取多次`offset`属性，比较糟糕，可以使用变量的形式缓存起来，待计算完毕再提交给浏览器发出重计算请求

```js
// 缓存offsetLeft与offsetTop的值
const el = document.getElementById('el')
let offLeft = el.offsetLeft, offTop = el.offsetTop

// 在JS层面进行计算
for(let i=0;i<10;i++) {
  offLeft += 10
  offTop  += 10
}

// 一次性将计算结果应用到DOM上
el.style.left = offLeft + "px"
el.style.top = offTop  + "px"
```

我们还可避免改变样式，使用类名去合并样式

```js
const container = document.getElementById('container')
container.style.width = '100px'
container.style.height = '200px'
container.style.border = '10px solid red'
container.style.color = 'red'
```

使用类名去合并样式

```html
<style>
    .basic_style {
        width: 100px;
        height: 200px;
        border: 10px solid red;
        color: red;
    }
</style>
<script>
    const container = document.getElementById('container')
    container.classList.add('basic_style')
</script>
```

前者每次单独操作，都去触发一次渲染树更改（新浏览器不会），

都去触发一次渲染树更改，从而导致相应的回流与重绘过程

合并之后，等于我们将所有的更改一次性发出

我们还可以通过通过设置元素属性`display: none`，将其从页面上去掉，然后再进行后续操作，这些后续操作也不会触发回流与重绘，这个过程称为离线操作

```js
const container = document.getElementById('container')
container.style.width = '100px'
container.style.height = '200px'
container.style.border = '10px solid red'
container.style.color = 'red'
```

离线操作后

```js
let container = document.getElementById('container')
container.style.display = 'none'
container.style.width = '100px'
container.style.height = '200px'
container.style.border = '10px solid red'
container.style.color = 'red'
...（省略了许多类似的后续操作）
container.style.display = 'block'
```

## 12、什么是响应式设计？响应式设计的基本原理是什么？如何做？

## 一、是什么

响应式网站设计（Responsive Web design）是一种网络页面设计布局，页面的设计与开发应当根据用户行为以及设备环境(系统平台、屏幕尺寸、屏幕定向等)进行相应的响应和调整

描述响应式界面最著名的一句话就是“Content is like water”

大白话便是“如果将屏幕看作容器，那么内容就像水一样”

响应式网站常见特点：

- 同时适配PC + 平板 + 手机等
- 标签导航在接近手持终端设备时改变为经典的抽屉式导航
- 网站的布局会根据视口来调整模块的大小和位置

![img](https://static.vue-js.com/ae68be30-9dba-11eb-85f6-6fac77c0c9b3.png)

## [#](https://vue3js.cn/interview/css/responsive_layout.html#二、实现方式)二、实现方式

响应式设计的基本原理是通过媒体查询检测不同的设备屏幕尺寸做处理，为了处理移动端，页面头部必须有`meta`声明`viewport`

```html
<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no”>
```

属性对应如下：

- width=device-width: 是自适应手机屏幕的尺寸宽度
- maximum-scale:是缩放比例的最大值
- inital-scale:是缩放的初始化
- user-scalable:是用户的可以缩放的操作

实现响应式布局的方式有如下：

- 媒体查询
- 百分比
- vw/vh
- rem

### [#](https://vue3js.cn/interview/css/responsive_layout.html#媒体查询)媒体查询

`CSS3`中的增加了更多的媒体查询，就像`if`条件表达式一样，我们可以设置不同类型的媒体条件，并根据对应的条件，给相应符合条件的媒体调用相对应的样式表

使用`@Media`查询，可以针对不同的媒体类型定义不同的样式，如：

```css
@media screen and (max-width: 1920px) { ... }
```

当视口在375px - 600px之间，设置特定字体大小18px

```css
@media screen (min-width: 375px) and (max-width: 600px) {
  body {
    font-size: 18px;
  }
}
```

通过媒体查询，可以通过给不同分辨率的设备编写不同的样式来实现响应式的布局，比如我们为不同分辨率的屏幕，设置不同的背景图片

比如给小屏幕手机设置@2x图，为大屏幕手机设置@3x图，通过媒体查询就能很方便的实现

### [#](https://vue3js.cn/interview/css/responsive_layout.html#百分比)百分比

通过百分比单位 " % " 来实现响应式的效果

比如当浏览器的宽度或者高度发生变化时，通过百分比单位，可以使得浏览器中的组件的宽和高随着浏览器的变化而变化，从而实现响应式的效果

`height`、`width`属性的百分比依托于父标签的宽高，但是其他盒子属性则不完全依赖父元素：

- 子元素的top/left和bottom/right如果设置百分比，则相对于直接非static定位(默认定位)的父元素的高度/宽度
- 子元素的padding如果设置百分比，不论是垂直方向或者是水平方向，都相对于直接父亲元素的width，而与父元素的height无关。
- 子元素的margin如果设置成百分比，不论是垂直方向还是水平方向，都相对于直接父元素的width
- border-radius不一样，如果设置border-radius为百分比，则是相对于自身的宽度

可以看到每个属性都使用百分比，会照成布局的复杂度，所以不建议使用百分比来实现响应式

### [#](https://vue3js.cn/interview/css/responsive_layout.html#vw-vh)vw/vh

`vw`表示相对于视图窗口的宽度，`vh`表示相对于视图窗口高度。 任意层级元素，在使用`vw`单位的情况下，`1vw`都等于视图宽度的百分之一

与百分比布局很相似，在以前文章提过与`%`的区别，这里就不再展开述说

### [#](https://vue3js.cn/interview/css/responsive_layout.html#rem)rem

在以前也讲到，`rem`是相对于根元素`html`的`font-size`属性，默认情况下浏览器字体大小为`16px`，此时`1rem = 16px`

可以利用前面提到的媒体查询，针对不同设备分辨率改变`font-size`的值，如下：

```css
@media screen and (max-width: 414px) {
  html {
    font-size: 18px
  }
}

@media screen and (max-width: 375px) {
  html {
    font-size: 16px
  }
}

@media screen and (max-width: 320px) {
  html {
    font-size: 12px
  }
}
```

为了更准确监听设备可视窗口变化，我们可以在`css`之前插入`script`标签，内容如下：

```js
//动态为根元素设置字体大小
function init () {
    // 获取屏幕宽度
    var width = document.documentElement.clientWidth
    // 设置根元素字体大小。此时为宽的10等分
    document.documentElement.style.fontSize = width / 10 + 'px'
}

//首次加载应用，设置一次
init()
// 监听手机旋转的事件的时机，重新设置
window.addEventListener('orientationchange', init)
// 监听手机窗口变化，重新设置
window.addEventListener('resize', init)
```

无论设备可视窗口如何变化，始终设置`rem`为`width`的1/10，实现了百分比布局

除此之外，我们还可以利用主流`UI`框架，如：`element ui`、`antd`提供的栅格布局实现响应式

### [#](https://vue3js.cn/interview/css/responsive_layout.html#小结)小结

响应式设计实现通常会从以下几方面思考：

- 弹性盒子（包括图片、表格、视频）和媒体查询等技术
- 使用百分比布局创建流式布局的弹性UI，同时使用媒体查询限制元素的尺寸和内容变更范围
- 使用相对单位使得内容自适应调节
- 选择断点，针对不同断点实现不同布局和内容展示

## [#](https://vue3js.cn/interview/css/responsive_layout.html#三、总结)三、总结

响应式布局优点可以看到：

- 面对不同分辨率设备灵活性强
- 能够快捷解决多设备显示适应问题

缺点：

- 仅适用布局、信息、框架并不复杂的部门类型网站
- 兼容各种设备工作量大，效率低下
- 代码累赘，会出现隐藏无用的元素，加载时间加长
- 其实这是一种折中性质的设计解决方案，多方面因素影响而达不到最佳效果
- 一定程度上改变了网站原有的布局结构，会出现用户混淆的情况

## 13、如果要做优化，CSS提高性能的方法有哪些？

实现方式有很多种，主要有如下：

- 内联首屏关键CSS
- 异步加载CSS
- 资源压缩
- 合理使用选择器
- 减少使用昂贵的属性
- 不要使用@import

### [#](https://vue3js.cn/interview/css/css_performance.html#内联首屏关键css)内联首屏关键CSS

在打开一个页面，页面首要内容出现在屏幕的时间影响着用户的体验，而通过内联`css`关键代码能够使浏览器在下载完`html`后就能立刻渲染

而如果外部引用`css`代码，在解析`html`结构过程中遇到外部`css`文件，才会开始下载`css`代码，再渲染

所以，`CSS`内联使用使渲染时间提前

注意：但是较大的`css`代码并不合适内联（初始拥塞窗口、没有缓存），而其余代码则采取外部引用方式

### [#](https://vue3js.cn/interview/css/css_performance.html#异步加载css)异步加载CSS

在`CSS`文件请求、下载、解析完成之前，`CSS`会阻塞渲染，浏览器将不会渲染任何已处理的内容

前面加载内联代码后，后面的外部引用`css`则没必要阻塞浏览器渲染。这时候就可以采取异步加载的方案，主要有如下：

- 使用javascript将link标签插到head标签最后

```js
// 创建link标签
const myCSS = document.createElement( "link" );
myCSS.rel = "stylesheet";
myCSS.href = "mystyles.css";
// 插入到header的最后位置
document.head.insertBefore( myCSS, document.head.childNodes[ document.head.childNodes.length - 1 ].nextSibling );
```

1
2
3
4
5
6

- 设置link标签media属性为noexis，浏览器会认为当前样式表不适用当前类型，会在不阻塞页面渲染的情况下再进行下载。加载完成后，将`media`的值设为`screen`或`all`，从而让浏览器开始解析CSS

```html
<link rel="stylesheet" href="mystyles.css" media="noexist" onload="this.media='all'">
```

1

- 通过rel属性将link元素标记为alternate可选样式表，也能实现浏览器异步加载。同样别忘了加载完成之后，将rel设回stylesheet

```html
<link rel="alternate stylesheet" href="mystyles.css" onload="this.rel='stylesheet'">
```

1

### [#](https://vue3js.cn/interview/css/css_performance.html#资源压缩)资源压缩

利用`webpack`、`gulp/grunt`、`rollup`等模块化工具，将`css`代码进行压缩，使文件变小，大大降低了浏览器的加载时间

### [#](https://vue3js.cn/interview/css/css_performance.html#合理使用选择器)合理使用选择器

`css`匹配的规则是从右往左开始匹配，例如`#markdown .content h3`匹配规则如下：

- 先找到h3标签元素
- 然后去除祖先不是.content的元素
- 最后去除祖先不是#markdown的元素

如果嵌套的层级更多，页面中的元素更多，那么匹配所要花费的时间代价自然更高

所以我们在编写选择器的时候，可以遵循以下规则：

- 不要嵌套使用过多复杂选择器，最好不要三层以上
- 使用id选择器就没必要再进行嵌套
- 通配符和属性选择器效率最低，避免使用

### [#](https://vue3js.cn/interview/css/css_performance.html#减少使用昂贵的属性)减少使用昂贵的属性

在页面发生重绘的时候，昂贵属性如`box-shadow`/`border-radius`/`filter`/透明度/`:nth-child`等，会降低浏览器的渲染性能

### [#](https://vue3js.cn/interview/css/css_performance.html#不要使用-import)不要使用@import

css样式文件有两种引入方式，一种是`link`元素，另一种是`@import`

`@import`会影响浏览器的并行下载，使得页面在加载时增加额外的延迟，增添了额外的往返耗时

而且多个`@import`可能会导致下载顺序紊乱

比如一个css文件`index.css`包含了以下内容：`@import url("reset.css")`

那么浏览器就必须先把`index.css`下载、解析和执行后，才下载、解析和执行第二个文件`reset.css`

### [#](https://vue3js.cn/interview/css/css_performance.html#其他)其他

- 减少重排操作，以及减少不必要的重绘
- 了解哪些属性可以继承而来，避免对这些属性重复编写
- cssSprite，合成所有icon图片，用宽高加上backgroud-position的背景图方式显现出我们要的icon图，减少了http请求
- 把小的icon图片转成base64编码
- CSS3动画或者过渡尽量使用transform和opacity来实现动画，不要使用left和top属性

## 14、如何实现单行／多行文本溢出的省略样式？

## 一、前言

在日常开发展示页面，如果一段文本的数量过长，受制于元素宽度的因素，有可能不能完全显示，为了提高用户的使用体验，这个时候就需要我们把溢出的文本显示成省略号

对于文本的溢出，我们可以分成两种形式：

- 单行文本溢出
- 多行文本溢出

## [#](https://vue3js.cn/interview/css/single_multi_line.html#二、实现方式)二、实现方式

### [#](https://vue3js.cn/interview/css/single_multi_line.html#单行文本溢出省略)单行文本溢出省略

理解也很简单，即文本在一行内显示，超出部分以省略号的形式展现

实现方式也很简单，涉及的`css`属性有：

- text-overflow：规定当文本溢出时，显示省略符号来代表被修剪的文本
- white-space：设置文字在一行显示，不能换行
- overflow：文字长度超出限定宽度，则隐藏超出的内容

`overflow`设为`hidden`，普通情况用在块级元素的外层隐藏内部溢出元素，或者配合下面两个属性实现文本溢出省略

`white-space:nowrap`，作用是设置文本不换行，是`overflow:hidden`和`text-overflow：ellipsis`生效的基础

`text-overflow`属性值有如下：

- clip：当对象内文本溢出部分裁切掉
- ellipsis：当对象内文本溢出时显示省略标记（...）

`text-overflow`只有在设置了`overflow:hidden`和`white-space:nowrap`才能够生效的

举个例子

```html
<style>
    p{
        overflow: hidden;
        line-height: 40px;
        width:400px;
        height:40px;
        border:1px solid red;
        text-overflow: ellipsis;
        white-space: nowrap;
    }
</style>
<p 这是一些文本这是一些文本这是一些文本这是一些文本这是一些文本这是一些文本这是一些文本这是一些文本这是一些文本这是一些文本</p >
```

效果如下：

![img](https://static.vue-js.com/bb3048e0-a0e9-11eb-85f6-6fac77c0c9b3.png)

可以看到，设置单行文本溢出较为简单，并且省略号显示的位置较好

### [#](https://vue3js.cn/interview/css/single_multi_line.html#多行文本溢出省略)多行文本溢出省略

多行文本溢出的时候，我们可以分为两种情况：

- 基于高度截断
- 基于行数截断

#### [#](https://vue3js.cn/interview/css/single_multi_line.html#基于高度截断)基于高度截断

#### [#](https://vue3js.cn/interview/css/single_multi_line.html#伪元素-定位)伪元素 + 定位

核心的`css`代码结构如下：

- position: relative：为伪元素绝对定位
- overflow: hidden：文本溢出限定的宽度就隐藏内容）
- position: absolute：给省略号绝对定位
- line-height: 20px：结合元素高度,高度固定的情况下,设定行高, 控制显示行数
- height: 40px：设定当前元素高度
- ::after {} ：设置省略号样式

代码如下所示：

```html
<style>
    .demo {
        position: relative;
        line-height: 20px;
        height: 40px;
        overflow: hidden;
    }
    .demo::after {
        content: "...";
        position: absolute;
        bottom: 0;
        right: 0;
        padding: 0 20px 0 10px;
    }
</style>

<body>
    <div class='demo'>这是一段很长的文本</div>
</body>
```

实现原理很好理解，就是通过伪元素绝对定位到行尾并遮住文字，再通过 `overflow: hidden` 隐藏多余文字

这种实现具有以下优点：

- 兼容性好，对各大主流浏览器有好的支持
- 响应式截断，根据不同宽度做出调整

一般文本存在英文的时候，可以设置`word-break: break-all`使一个单词能够在换行时进行拆分

#### [#](https://vue3js.cn/interview/css/single_multi_line.html#基于行数截断)基于行数截断

纯`css`实现也非常简单，核心的`css`代码如下：

- -webkit-line-clamp: 2：用来限制在一个块元素显示的文本的行数，为了实现该效果，它需要组合其他的WebKit属性）
- display: -webkit-box：和1结合使用，将对象作为弹性伸缩盒子模型显示
- -webkit-box-orient: vertical：和1结合使用 ，设置或检索伸缩盒对象的子元素的排列方式
- overflow: hidden：文本溢出限定的宽度就隐藏内容
- text-overflow: ellipsis：多行文本的情况下，用省略号“…”隐藏溢出范围的文本

```html
<style>
    p {
        width: 400px;
        border-radius: 1px solid red;
        -webkit-line-clamp: 2;
        display: -webkit-box;
        -webkit-box-orient: vertical;
        overflow: hidden;
        text-overflow: ellipsis;
    }
</style>
<p>
    这是一些文本这是一些文本这是一些文本这是一些文本这是一些文本
    这是一些文本这是一些文本这是一些文本这是一些文本这是一些文本
</p >
```

可以看到，上述使用了`webkit`的`CSS`属性扩展，所以兼容浏览器范围是`PC`端的`webkit`内核的浏览器，由于移动端大多数是使用`webkit`，所以移动端常用该形式

需要注意的是，如果文本为一段很长的英文或者数字，则需要添加`word-wrap: break-word`属性

还能通过使用`javascript`实现配合`css`，实现代码如下所示：

css结构如下：

```css
p {
    position: relative;
    width: 400px;
    line-height: 20px;
    overflow: hidden;

}
.p-after:after{
    content: "..."; 
    position: absolute; 
    bottom: 0; 
    right: 0; 
    padding-left: 40px;
    background: -webkit-linear-gradient(left, transparent, #fff 55%);
    background: -moz-linear-gradient(left, transparent, #fff 55%);
    background: -o-linear-gradient(left, transparent, #fff 55%);
    background: linear-gradient(to right, transparent, #fff 55%);
}
```

javascript代码如下：

```js
$(function(){
 //获取文本的行高，并获取文本的高度，假设我们规定的行数是五行，那么对超过行数的部分进行限制高度，并加上省略号
   $('p').each(function(i, obj){
        var lineHeight = parseInt($(this).css("line-height"));
        var height = parseInt($(this).height());
        if((height / lineHeight) >3 ){
            $(this).addClass("p-after")
            $(this).css("height","60px");
        }else{
            $(this).removeClass("p-after");
        }
    });
})
```

## **15、display的block、inline和inline-block的区别**

（1）**block：** 会独占一行，多个元素会另起一行，可以设置width、height、margin和padding属性；

（2）**inline：** 元素不会独占一行，设置width、height属性无效。但可以设置水平方向的margin和padding属性，不能设置垂直方向的padding和margin；

（3）**inline-block：** 将对象设置为inline对象，但对象的内容作为block对象呈现，之后的内联对象会被排列在同一行内。

对于行内元素和块级元素，其特点如下：

**（1）行内元素**

- 设置宽高无效；
- 可以设置水平方向的margin和padding属性，不能设置垂直方向的padding和margin；
- 不会自动换行；

**（2）块级元素**

- 可以设置宽高；
- 设置margin和padding都有效；
- 可以自动换行；
- 多个块状，默认排列从上到下。

## 16、CSS如何画一个三角形？原理是什么



## 17、说说对Css预编语言的理解？有哪些区别?

![img](https://static.vue-js.com/81cca1c0-a42c-11eb-85f6-6fac77c0c9b3.png)

## [#](https://vue3js.cn/interview/css/sass_less_stylus.html#一、是什么)一、是什么

`Css` 作为一门标记性语言，语法相对简单，对使用者的要求较低，但同时也带来一些问题

需要书写大量看似没有逻辑的代码，不方便维护及扩展，不利于复用，尤其对于非前端开发工程师来讲，往往会因为缺少 `Css` 编写经验而很难写出组织良好且易于维护的 `Css` 代码

`Css`预处理器便是针对上述问题的解决方案

#### [#](https://vue3js.cn/interview/css/sass_less_stylus.html#预处理语言)预处理语言

扩充了 `Css` 语言，增加了诸如变量、混合（mixin）、函数等功能，让 `Css` 更易维护、方便

本质上，预处理是`Css`的超集

包含一套自定义的语法及一个解析器，根据这些语法定义自己的样式规则，这些规则最终会通过解析器，编译生成对应的 `Css` 文件

## [#](https://vue3js.cn/interview/css/sass_less_stylus.html#二、有哪些)二、有哪些

`Css`预编译语言在前端里面有三大优秀的预编处理器，分别是：

- sass
- less
- stylus

### [#](https://vue3js.cn/interview/css/sass_less_stylus.html#sass)sass

2007 年诞生，最早也是最成熟的 `Css`预处理器，拥有 Ruby 社区的支持和 `Compass` 这一最强大的 `Css`框架，目前受 `LESS` 影响，已经进化到了全面兼容 `Css` 的 `Scss`

文件后缀名为`.sass`与`scss`，可以严格按照 sass 的缩进方式省去大括号和分号

### [#](https://vue3js.cn/interview/css/sass_less_stylus.html#less)less

2009年出现，受`SASS`的影响较大，但又使用 `Css` 的语法，让大部分开发者和设计师更容易上手，在 `Ruby`社区之外支持者远超过 `SASS`

其缺点是比起 `SASS`来，可编程功能不够，不过优点是简单和兼容 `Css`，反过来也影响了 `SASS`演变到了`Scss` 的时代

### [#](https://vue3js.cn/interview/css/sass_less_stylus.html#stylus)stylus

`Stylus`是一个`Css`的预处理框架，2010 年产生，来自 `Node.js`社区，主要用来给 `Node` 项目进行 `Css` 预处理支持

所以`Stylus` 是一种新型语言，可以创建健壮的、动态的、富有表现力的`Css`。比较年轻，其本质上做的事情与`SASS/LESS`等类似

## [#](https://vue3js.cn/interview/css/sass_less_stylus.html#三、区别)三、区别

虽然各种预处理器功能强大，但使用最多的，还是以下特性：

- 变量（variables）
- 作用域（scope）
- 代码混合（ mixins）
- 嵌套（nested rules）
- 代码模块化（Modules）

因此，下面就展开这些方面的区别

### [#](https://vue3js.cn/interview/css/sass_less_stylus.html#基本使用)基本使用

less和scss

```css
.box {
  display: block;
}
```

sass

```css
.box
  display: block
```

stylus

```css
.box
  display: block
```

### [#](https://vue3js.cn/interview/css/sass_less_stylus.html#嵌套)嵌套

三者的嵌套语法都是一致的，甚至连引用父级选择器的标记 & 也相同

区别只是 Sass 和 Stylus 可以用没有大括号的方式书写

less

```css
.a {
  &.b {
    color: red;
  }
}
```

### [#](https://vue3js.cn/interview/css/sass_less_stylus.html#变量)变量

变量无疑为 Css 增加了一种有效的复用方式，减少了原来在 Css 中无法避免的重复「硬编码」

`less`声明的变量必须以`@`开头，后面紧跟变量名和变量值，而且变量名和变量值需要使用冒号`:`分隔开

```css
@red: #c00;

strong {
  color: @red;
}
```

`sass`声明的变量跟`less`十分的相似，只是变量名前面使用`@`开头

```css
$red: #c00;

strong {
  color: $red;
}
```

```
stylus`声明的变量没有任何的限定，可以使用`$`开头，结尾的分号`;`可有可无，但变量与变量值之间需要使用`=
```

在`stylus`中我们不建议使用`@`符号开头声明变量

```css
red = #c00

strong
  color: red
```

### [#](https://vue3js.cn/interview/css/sass_less_stylus.html#作用域)作用域

`Css` 预编译器把变量赋予作用域，也就是存在生命周期。就像 `js`一样，它会先从局部作用域查找变量，依次向上级作用域查找

`sass`中不存在全局变量

```css
$color: black;
.scoped {
  $bg: blue;
  $color: white;
  color: $color;
  background-color:$bg;
}
.unscoped {
  color:$color;
} 
```

编译后

```css
.scoped {
  color:white;/*是白色*/
  background-color:blue;
}
.unscoped {
  color:white;/*白色（无全局变量概念）*/
} 
```

所以，在`sass`中最好不要定义相同的变量名

`less`与`stylus`的作用域跟`javascript`十分的相似，首先会查找局部定义的变量，如果没有找到，会像冒泡一样，一级一级往下查找，直到根为止

```css
@color: black;
.scoped {
  @bg: blue;
  @color: white;
  color: @color;
  background-color:@bg;
}
.unscoped {
  color:@color;
} 
```

编译后：

```css
.scoped {
  color:white;/*白色（调用了局部变量）*/
  background-color:blue;
}
.unscoped {
  color:black;/*黑色（调用了全局变量）*/
} 
```

### [#](https://vue3js.cn/interview/css/sass_less_stylus.html#混入)混入

混入（mixin）应该说是预处理器最精髓的功能之一了，简单点来说，`Mixins`可以将一部分样式抽出，作为单独定义的模块，被很多选择器重复使用

可以在`Mixins`中定义变量或者默认参数

在`less`中，混合的用法是指将定义好的`ClassA`中引入另一个已经定义的`Class`，也能使用够传递参数，参数变量为`@`声明

```css
.alert {
  font-weight: 700;
}

.highlight(@color: red) {
  font-size: 1.2em;
  color: @color;
}

.heads-up {
  .alert;
  .highlight(red);
}
```

编译后

```css
.alert {
  font-weight: 700;
}
.heads-up {
  font-weight: 700;
  font-size: 1.2em;
  color: red;
}
```

`Sass`声明`mixins`时需要使用`@mixinn`，后面紧跟`mixin`的名，也可以设置参数，参数名为变量`$`声明的形式

```css
@mixin large-text {
  font: {
    family: Arial;
    size: 20px;
    weight: bold;
  }
  color: #ff0000;
}

.page-title {
  @include large-text;
  padding: 4px;
  margin-top: 10px;
}
```

`stylus`中的混合和前两款`Css`预处理器语言的混合略有不同，他可以不使用任何符号，就是直接声明`Mixins`名，然后在定义参数和默认值之间用等号（=）来连接

```css
error(borderWidth= 2px) {
  border: borderWidth solid #F00;
  color: #F00;
}
.generic-error {
  padding: 20px;
  margin: 4px;
  error(); /* 调用error mixins */
}
.login-error {
  left: 12px;
  position: absolute;
  top: 20px;
  error(5px); /* 调用error mixins，并将参数$borderWidth的值指定为5px */
} 
```

### [#](https://vue3js.cn/interview/css/sass_less_stylus.html#代码模块化)代码模块化

模块化就是将`Css`代码分成一个个模块

`scss`、`less`、`stylus`三者的使用方法都如下所示

```css
@import './common';
@import './github-markdown';
@import './mixin';
@import './variables';
```