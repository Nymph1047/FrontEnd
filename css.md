## css3的新特性
- transition：过渡
- transform: 旋转、缩放、移动或倾斜
- animation: 动画
- gradient: 渐变
- box-shadow: 阴影
- border-radius: 圆角
- word-break: normal|break-all|keep-all; 文字换行(默认规则|单词也可以换行|只在半角空格或连字符换行)
- text-overflow: 文字超出部分处理
- text-shadow: 水平阴影，垂直阴影，模糊的距离，以及阴影的颜色。
- box-sizing: content-box|border-box 盒模型
- 媒体查询 @media screen and (max-width: 960px) {}还有打印print

## 圆？半圆？椭圆？
```css
 div {
  width: 100px;
  height: 100px;
  background-color: red;
  margin-top: 20px;
}
.box1 { /* 圆 */
    /*border-radius: 50%; */
    border-radius: 50px;
}
.box2 { /* 半圆 */
   height: 50px;
   border-radius: 50px 50px 0 0;
}
.box3 { /* 椭圆 */
    height: 50px;
    border-radius: 50px/25px; /* x轴/y轴 */
}
```

## 文字单超出显示省略号
```css 
div {
width: 200px;
overflow: hidden;
white-space: nowrap;
text-overflow: ellipsis;
}
```
文字多行超出显示省略号
```css
div {
	width: 200px;
	display: -webkit-box;
	-webkit-box-orient: vertical;
	-webkit-line-clamp: 3;
	overflow: hidden;
}
```
该方法适用于WebKit浏览器及移动端。
### 跨浏览器兼容方案：
```css
p {
    position:relative;
    line-height:1.4em;
    /* 3 times the line-height to show 3 lines */
    height:4.2em;
    overflow:hidden;
}
p::after {
    content:"...";
    font-weight:bold;
    position:absolute;
    bottom:0;
    right:0;
    padding:0 20px 1px 45px;
}
```
## 盒模型
`content（元素内容） + padding（内边距） + border（边框） + margin（外边距）`

延伸：box-sizing
- content-box：默认值，总宽度 = margin + border + padding + width
- border-box：盒子宽度包含 padding 和 border，总宽度 = margin + width
- inherit：从父元素继承 box-sizing 属性

## BFC
块级格式化上下文，是一个独立的渲染区域，让处于 BFC 内部的元素与外部的元素相互隔离，使内外元素的定位不会相互影响。

IE下为 Layout，可通过 zoom:1 触发
### 触发条件:
- 根元素
- position: absolute/fixed
- display: inline-block / table
- float 元素
- ovevflow !== visible
### 规则:
- 属于同一个 BFC 的两个相邻 Box 垂直排列
- 属于同一个 BFC 的两个相邻 Box 的 margin 会发生重叠
- BFC 中子元素的 margin box 的左边， 与包含块 (BFC) border box的左边相接触 (子元素 absolute 除外)
- BFC 的区域不会与 float 的元素区域重叠
- 计算 BFC 的高度时，浮动子元素也参与计算
- 文字层不会被浮动层覆盖，环绕于周围
### 应用:
- 阻止margin重叠
- 可以包含浮动元素 —— 清除内部浮动(清除浮动的原理是两个div都位于同一个 BFC 区域之中)
- 自适应两栏布局
- 可以阻止元素被浮动元素覆盖

## 垂直居中
- 定高：margin，position + margin(负值)
- 不定高：position + transform，flex，IFC + vertical-align:middle
```css
/*不定高方案1*/
.center{
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}

/*不定高方案2*/
.wrap{
  display: flex;
  align-items: center;
}
.center{
  width: 100%;
}

```
## 水平居中
- 行内元素：text-align：center
- 定宽块状元素：左右margin值为auto
- 不定宽块状元素：table布局，position+transform
```css
/*方案一*/
.wrap{
  text-align: center;
}
.center{
  display: inline;
}

/*方案二*/
.center{
  width: 100px;
  margin: 0 auto;
}

/*方案三*/
.wrap{
  position: relative;
}
.center{
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
}
```
## 两栏布局
- float+margin,float+calc
```css
.left{
  float:left;
  width: 200px;
}
.right{
  margin-left: 200px;
}

/*第二种方法*/
.left{
  float: left;
  width: 200px;
}
.right{
  width: calc(100% - 200px);
  float: left;
}

```
## 三栏布局
- float, float+calc,圣杯布局（设置BFC,margin负值法，flex）
```css
.wrap{
  width: 100%;
  height: 200px;
}
.wrap>div{
  height: 100%;
}
/*方案一*/
.left{
  width: 120px;
  float: left;
}
.right{
  width: 120px;
  float: right;
}
.center{
  margin: 0 120px;
}

/*第二种方法*/
.left{
  width: 120px;
  float: left;
}
.right{
  width: 120px;
  float: right;
}
.center{
  width: calc(100% - 240px);
  margin-left: 120px;
}

/*第三种方案*/
.wrap{
  display: flex;
}
.left{
  width: 120px;
}
.right{
  width: 120px;
}
.center{
  flex: 1;
}
```
## 伪类和伪元素
**css引入伪类和伪元素概念是为了格式化文档树以外的信息。也就是说，伪类和伪元素都是用来修饰不在文档树中的部分**
### 伪类
**伪类存在的意义是为了通过选择器找到那些不存在DOM树中的信息以及不能被常规CSS选择器获取到的信息**
- 获取不存在与DOM树中的信息。比如a标签的:link、visited等，这些信息不存在与DOM树结构中，只能通过CSS选择器来获取；
- 获取不能被常规CSS选择器获取的信息。比如：要获取第一个子元素，我们无法用常规的CSS选择器获取，但可以通过 :first-child 来获取到。
  获取不存在与DOM树中的信息。比如a标签的:link、visited等，这些信息不存在与DOM树结构中，只能通过CSS选择器来获取；
  获取不能被常规CSS选择器获取的信息。比如：要获取第一个子元素，我们无法用常规的CSS选择器获取，但可以通过 :first-child 来获取到。
### 伪元素
**伪元素用于创建一些不在文档树中的元素，并为其添加样式。比如说，
我们可以通过:before来在一个元素前增加一些文本，并为这些文本添加样式。
虽然用户可以看到这些文本，但是这些文本实际上不在文档树中。常见的伪元素有
：`::before，::after，::first-line，::first-letter，::selection、::placeholder`等**
因此，伪类与伪元素的区别在于：有没有创建一个文档树之外的元素
### ::after和:after的区别
- 在实际的开发工作中，我们会看到有人把伪元素写成:after，这实际是 CSS2 与CSS3新旧标准的规定不同而导致的。
- CSS2 中的伪元素使用1个冒号，在 CSS3 中，为了区分伪类和伪元素，规定伪元素使用2个冒号。所以，对于 CSS2 标准的老伪元素，比如:first-line，:first-letter，:before，:after，写一个冒号浏览器也能识别，但对于 CSS3 标准的新伪元素，比如::selection，就必须写2个冒号了
### CSS3新增伪类有那些？
- p:first-of-type 选择属于其父元素的首个`<p>`元素的每个`<p>` 元素。
- p:last-of-type 选择属于其父元素的最后 `<p>` 元素的每个`<p>` 元素。
- p:only-of-type 选择属于其父元素唯一的` <p>`元素的每个` <p>` 元素。
- p:only-child 选择属于其父元素的唯一子元素的每个` <p>` 元素。
- p:nth-child(2) 选择属于其父元素的第二个子元素的每个 `<p>` 元素。
- :after 在元素之前添加内容,也可以用来做清除浮动。
- :before 在元素之后添加内容
- :enabled
- :disabled 控制表单控件的禁用状态。
- :checked 单选框或复选框被选中
