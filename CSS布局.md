# CSS布局

## 水平居中布局
### 第一种方法

```css
parent:{text-align: center;}
child:{display: inline-block;}
```
优点：
  浏览器兼容较好
缺点：
  text-algin属性具有继承性，导致子级元素的文本也是居中显示的

### 第二种方法

```css
child:{
	display: block/table;	//div默认block属性
	margin: 0 auto;
}
```
优点：
  只需要对子级元素进行设置就可以实现水平方向居中布局效果
缺点：
  如果子级元素脱离文档流，导致margin属性的值无效
  *脱离文档流*方式：

  ```css
  float: left/right;
  position: absolute;
  position: fixed;
  ```

### 第三种方法

```css
parend:{position: relative;}
child:{
position: absolute;
left: 50%;
transform: translateX(-50%);
}
```

优点：
  父级元素是否脱离文档流。不影响子级元素水平居中效果，不清楚宽高情况下不影响
缺点：
  transform浏览器支持情况不好

### 第四种方法

```css
parend:{
display: flex;
justify-content: center;
align-items: center;
}
```

## 垂直居中布局
### 第一种方法

```css
parend:{
display:table；
vertical-align: middle;
}
```

优点：
  浏览器兼容较好
缺点：
  vertical-align具有继承行，导致父级元素文本也是居中显示
### 第二种方法

垂直布局li

父级：display: flex
    flex-direction: column
    justify-content: center



* 上下左右居中

  ```javascript
  //父级
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
  ```

  
























## 解释
text-align属性：是为*文本内容*设置对齐方式
  left：左对齐
  center：居中对齐
  right： 右对齐
display属性
  block：块级元素
  inline：内联元素（text-align有效）
  inline-block：行内块级元素（块级+内联）
  table：设置当前元素为<table>元素
  table-cell：设置当前元素为<td>元素（单元格）
vertical-align属性：是为*文本内容*设置垂直方向对齐方式
  top：顶部对齐
  middle：居中对齐
  bottom： 底部对齐