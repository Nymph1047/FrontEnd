##css3的新特性
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

##圆？半圆？椭圆？
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

##文字单超出显示省略号
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
###跨浏览器兼容方案：
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
##盒模型
`content（元素内容） + padding（内边距） + border（边框） + margin（外边距）`

延伸：box-sizing
- content-box：默认值，总宽度 = margin + border + padding + width
- border-box：盒子宽度包含 padding 和 border，总宽度 = margin + width
- inherit：从父元素继承 box-sizing 属性

##BFC
块级格式化上下文，是一个独立的渲染区域，让处于 BFC 内部的元素与外部的元素相互隔离，使内外元素的定位不会相互影响。

IE下为 Layout，可通过 zoom:1 触发
###触发条件:
- 根元素
- position: absolute/fixed
- display: inline-block / table
- float 元素
- ovevflow !== visible
###规则:
- 属于同一个 BFC 的两个相邻 Box 垂直排列
- 属于同一个 BFC 的两个相邻 Box 的 margin 会发生重叠
- BFC 中子元素的 margin box 的左边， 与包含块 (BFC) border box的左边相接触 (子元素 absolute 除外)
- BFC 的区域不会与 float 的元素区域重叠
- 计算 BFC 的高度时，浮动子元素也参与计算
- 文字层不会被浮动层覆盖，环绕于周围
###应用:
- 阻止margin重叠
- 可以包含浮动元素 —— 清除内部浮动(清除浮动的原理是两个div都位于同一个 BFC 区域之中)
- 自适应两栏布局
- 可以阻止元素被浮动元素覆盖