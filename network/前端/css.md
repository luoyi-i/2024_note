
@[toc]
## CSS doc
https://www.w3schools.com/cssref/pr_class_position.php
https://www.w3schools.com/css/tryit.asp?filename=trycss_position_relative

## CSS 盒子模型

Margin 外边框
Border，边框
Padding, 内边距
content， 内容
看名词的含义就很容易理解了
但看下面文档的解释，就变得混乱了
奇怪的解释不用管

> 混乱的名词解释
    Margin(外边距) - 清除边框外的区域，外边距是透明的。
    Border(边框) - 围绕在内边距和内容外的边框。
    Padding(内边距) - 清除内容周围的区域，内边距是透明的。
    Content(内容) - 盒子的内容，显示文本和图像。

对元素的设置只是针对内容
不同版本的浏览器对元素设置的定义有区别
为解决旧的 IE 版本不兼容问题，在 HTML 页面声明 <!DOCTYPE html>


## flex 布局
https://www.ruanyifeng.com/blog/2015/07/flex-grammar.html
定义了 display:flex 的元素，称为 flex 容器。
它的所有子元素， flex item, 称为项目
容器默认存在两根轴，水平的主轴 main axis 和垂直的交叉轴 cross axis.
主轴的开始叫做 main start, 结束叫 main end;
交叉轴的开始叫 cross start, 结束叫 cross end;

### 容器的属性
* flex-direction
有 4 个选项， row/row-reverse/column/column-reverse
eg, column-revserse, 主轴为垂直方向，起点在下沿

* flex-wrap
定义了如果一条轴排不下，如何换行
nowrap： 不换行
wrap: 换行，第一行在上方
wrap-reverse： 换行，第一行在下方

* flex-flow 
flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap

* justify-content
定义了项目在主轴上的对齐方式。
4 个值
flex-start 左对齐，默认
flex-end 
center 
space-between 两端对齐，项目之间的间隔都相等
space-around 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。
定义元素在主轴上的排列方式

* align-items
定义项目在交叉轴上如何对齐。
flex-start | flex-end | center | baseline | stretch;
base line, 项目中文本的基线在同一高度
stretch: 项目高度为容器高度

* align-content
定义了多根轴线的对齐方式

### 项目的属性

*order          定义项目的排列顺序。数值越小，排列越靠前，默认为0。
*flex-grow      属性定义项目的放大比例，默认为0，不放大。
                如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。
*flex-shrink    定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。
*flex-basis     定义了在分配多余空间之前，项目占据的主轴空间
                它可以设为跟width或height属性一样的值，则项目将占据固定空间。
*flex           flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto
                该属性有两个快捷值：auto (1 1 auto) 和 none (0 0 auto)。
*align-self     允许单个项目有与其他项目不一样的对齐方式


## 内联样式
style：静态的样式统一写到 class 中。
style 接收动态的样式，在运行时会进行解析，请尽量避免将静态的样式写进 style 中，以免影响渲染速度。
```
<view style="color:{{color}};" />
```
## 选择器
.class
#id 
element: eg, view
element,element: view, text
::after:    eg, view::after  在 view 组件后边插入内容
::before:   eg, view::before 在 view 组件前边插入内容

定义在 app.wxss 中的样式为全局样式，作用于每一个页面。
在 page 的 wxss 文件中定义的样式为局部样式，作用于对应的页面，并会覆盖 app.wxss 中相同的选择器。

## CSS 语法列举
boarder 的设置
- border: 0.3px solid #e2e2e2;
- white-space: nowrap;
white space 可以用来设置文字在一行，或自动换行，或每一个空格另起一行
- width： 可以设置省 100%， 或 35 rpx
- justify-content: left;
元素靠左
- 标签 text width 是固定的，不能调，用 view 标签就可以了
- first-child
p:first-child  满足条件的第一项
eg
.className:first-child  
p:first-child  
## 排版
* 让元素居中
```
.center-flex {
    display: flex;
    align-items: center; /* 垂直居中 */
    justify-content: center; /* 水平居中 */
    height: 100px; /* 需要设置高度 */
}
```
* 水平居中
对于行内元素（如文本或链接），
将下面的类应用到一个包裹行内元素的块级元素上。
```
.center-text {
    text-align: center;
}
```
对于块级元素（如div），
确保设置了宽度，margin: 0 auto; 会使得左右边距自动调整以居中元素。
```
.center-div {
    margin: 0 auto;
    width: 50%; /* 或者其它宽度 */
}
```

eg, 让自定义组件居中
wxss
```
.cls-select-container{
  margin: 0 auto;
  width: 90%;
  border-width:1px;
  border-color:darkgray;
  height: 80rpx;
}

cls-selector{
  border: 50px solid rgb(0, 0, 0);
}
```

## 未解的问题
.cls-btn{
    height: 80rpx;
    font-size: 14px;
    width: 40%;
    margin-top:25rpx;
    margin-bottom:25rpx;
    align-items: center;
    /* 加了下面这一行会显示四个边框，不加右边的边框显示不出来 */
    margin-left:40rpx; 
    background-color:#e1f2fe;
}


