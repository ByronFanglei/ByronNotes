## CSS部分

### CSS单位解释.
* px：绝对单位，页面按精确像素展示
* em：相对单位，基准为父节点字体的大小
* rem：相对单位：基准为html的字体大小
* vm：视窗宽度，1vm等于视窗宽度1%
* vh：视窗高度，1vh等于视窗高度1%

### 什么是响应式布局，响应式布局有哪些
* 响应式布局就是一个网站能够兼容多个终端，而不是每个终端都做一个特定的版本
* 等比例缩放，媒体查询

### bootstrap响应式布局原理
* 百分比布局+媒体查询

### BFC
* BFC：块级格式化上下文，它是一个独立的渲染区域，
* BFC规则：
>内部的box会在垂直方向，一个接一个的放置
>box垂直方向的距离有margin决定，属于同一个BFC的两个相邻box的margin会发生重叠
>每个元素的margin box的左边，与包含块border box的左边相接触 即使在浮动也如此
>BFC的区域不会与float box重叠
>BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之也如此
>计算BFC的高度时，浮动元素也参与计算

* 创建BFC的方法：
>position：absolute或者fixed
>float：不是none
>overflow： 不为visible
>display： inline-block或者flex，table-cell，table-caption，inline-flex
>根元素：html元素

### 盒子垂直居中的方法
* 表格居中
* flex居中
* 绝对定位居中
* grid布局

### link与@import区别
* link是html代码
* @import是css代码，可以写在html代码中，会阻塞页面的渲染，不推荐

### css常见的选择器
* id，class， 标签【p】
* 后代选择器【div p】，子代选择器【div>p】，紧邻选择器【div+p】，匹配任何在div元素之后的同级p元素【div~p】，分组选择器【div，p】
* 伪类选择器 :hover{}，伪元素选择器::after
* 属性选择器 div[a]

### css的优先级
* !important > 内联 > id选择器 > class选择器 > 标签选择器

### 浮动问题
* 清除浮动的方式：
1、在浮动的元素下面添加一个标签，添加属性clear：both；
2、添加clearfix样式，原理与上一样，添加after伪元素并设置 content: "";display: block; clear:both
3、父元素添加overflow:hidden
4、父元素 float

### 定位问题 position
* static：默认的
* absolute：绝对定位，相对于最近已定位的祖先元素，定位后空间释放，对行内元素使用后变为块级元素
* relative：相对定位，相对于自己初始位置，定位后空间不释放
* fixed：固定定位，相对于浏览器定位，定位后空间释放，对行内元素使用后变为块级元素
* sticky：粘性定位，相当与吸顶效果

### 盒模型
* 标准盒模型：width属性 = content宽度
* 怪异盒模型：width属性 = content + padding + border宽度
* box-sizing: content-box 标准盒模型，border-box怪异盒模型

### 外边距合并问题
* 父元素添加overflow:hidden
* 父元素浮动或定位
* 子元素浮动或定位
* 父元素添加边框

### 重绘 重排
* 重绘：一个元素的外边被改变，但是没有改变布局，的情况下发生，如改变visibility，outline，背景色等
* 重排/reflow（回流）：当浏览器某个部分发生了变化影响了布局，需要倒回去重新渲染，DOM的变化影响到了元素的几何属性，浏览器会重新计算元素的几何属性，如改变窗口的大小，文字的大小，内容的改变，浏览器窗口变化，style属性的改变等，重绘不会引起重排，但重排一定会引起重绘
* 那种情况导致重绘：visibility、outline、背景色等属性
* 那种情况导致重排：页面渲染初始化时、浏览器窗口改变尺寸、元素尺寸改变时、元素位置改变时、元素内容改变时、添加或删除可见的DOM 元素时，获取offsetLeft等
* 怎样减少重绘和重排：
>css部分：
1.避免频繁的样式操作，最好一次性的重写style或一次性更改class
2.避免设置多层内联样式
3.对具有复杂动画的元素进行绝对定位，使他脱离文档流，否则会引父元素及后续元素频繁回流
4.避免使用table布局
5.使用css3硬件加速，可以让`transform`、`opacity`、`filters`等动画效果不会引起重绘和重排

>js部分：
1.避免频繁操作DOM，可以使用createDocumentFragment创建子树，然后再拷贝到文档中
2.先隐藏元素，然后修改后再显示该元素，因为display:none上的DOM曹祖不会引起回流和重绘
3.避免循环读取offsetLeft等属性，在循环之前把他们存储起来

### display部分
* display：inline，block，inline-block，flex，table，table-cell

### 显示隐藏
* display：none，block	不占空间，没有事件
* opacity： 0， 1	占空间，有事件
* visibility： hidden， visible	占空间	没有事件

### viewport的作用
* 进行对移动端页面的适配，当加载页面后，先获取默认像素的页面，然后根据viewport的设置进行全局缩放，达到适配当前屏幕的页面

```html
<meta name="viewport" content="width=device-width,initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
```

### css画图形
* 画三角

```css
.one{
      width: 0;
      height: 0;
      border-left: 100px solid #ff6700;
      border-top: 50px solid transparent;
      border-right: 0;
      border-bottom: 50px solid transparent;
    }
```

* 画加号

```css
.one{
      width: 50px;
      height: 50px;
      border: 1px solid #ff6700;
      position: relative;
    }
    .one::before{
      content: "";
      position: absolute;
      left: 50%;
      top: 50%;
      width: 40px;
      border-top: 10px solid #ccc;
      transform: translate(-50%, -50%);
    }
    .one::after{
      content: "";
      position: absolute;
      left: 50%;
      top: 50%;
      height: 40px;
      border-left: 10px solid #ccc;
      transform: translate(-50%, -50%);
    }
```