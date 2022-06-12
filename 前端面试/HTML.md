## HTML部分

### HTML常见的块级元素与行级元素
* 行级：不能设置宽高（内容撑开），水平排列【span，a，i，b，strong，em】
* 块级：可以设置宽高，垂直排列，独占一行【div，p，ul，li，h1，ol】
* 行内块：元素在一行显示，可以给行内块元素设置宽度和高度【input，img】
* display：block，inline，inline-block	即可以设置宽高，又可以水平分布
### HTML5 有哪些新特性？
* 不需要任何插件就可以实现动画，视频，图形等效果，替代flash插件
* 声明方式
>HTML4 规定了三种声明方式，分别是：严格模式Strict、过渡模式Transitional  和 框架集模式Frameset
>而HTML5因为不是SGML的子集，只需要<!DOCTYPE html>就可以了

* 语义化更好的内容标签：header，footer，article，aside，section，details，summary，dialog，nav，有助于SEO的优化
* 音频，视频：audio，video，【提醒，MP4在h5可能存在声音和视频不同步的问题，大概是4s】
* HTML5 拥有多个新的表单输入类型。这些新特性提供了更好的输入控制和验证：color，date，datetime，datetime-local，email，month，number，range，search，tel，time，url，week
* 本地存储/离线存储API：
>长期存储数据的 localStorage	比较常用
>临时存储的 sessionStorage，浏览器关闭后自动删除	不常用

* 地理API：Geolocation
* 拖拽释放API
* Web Workers
* websocket（不断开链接）很适合做网页版的聊天工具

### HTML5 引入什么新的表单属性
* form：
>autocomplete:属性规定表单是否应该启用自动完成功能  《form autocomplete="on|off"》
>novalidate:如果使用该属性，则提交表单时不进行内容的验证	novalidate="novalidate"

* input:
>autocomplete：属性规定表单是否应该启用自动完成功能，大意可以查看之前输入的内容
>autofocus：加载页面后，自动获取光标
>list：引用包含输入字段的预定义选项的 datalist 
>min 和 max：min 属性与 max 属性配合使用，可创建合法值范围，两个要一对用
>pattern ：规定用于验证输入字段的正则表达式
>placeholder：提供可描述输入字段预期值的提示信息
>required：规定必需在提交之前填写输入字段。 如果使用该属性，则字段是必填（或必选）的
>step：为输入域规定合法的数字间隔。 如果 step=“3”，则合法的数是 -3,0,3,6 等

### 与 HTML4 相比 HTML5 废弃了哪些元素
* basefont、big、center、font、s、strike、tt、u、frame、frameset、noframes

###	如何处理 HTML5 新标签的浏览器兼容问题
```javascript
<!--[if lt IE 9]>
<script type="text/javascript" src="js/html5shiv.js"></script>
<![endif]-->
```

### URI与URL的区别
* URI：统一资源标识符，标识一个资源
* URL：统一资源定位器，就是一个具体的url
* URL包含（协议，ip或域名，端口，目录，文件名）



### 说说你对WebCodecs API的理解

* 允许 Web 程序对视频，音频进行进行编码的 API


