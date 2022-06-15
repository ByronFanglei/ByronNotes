## HTML部分

### 1. HTML常见的块级元素与行级元素
* 行级：不能设置宽高（内容撑开），水平排列【span，a，i，b，strong，em】
* 块级：可以设置宽高，垂直排列，独占一行【div，p，ul，li，h1，ol】
* 行内块：元素在一行显示，可以给行内块元素设置宽度和高度【input，img】
* display：block，inline，inline-block	即可以设置宽高，又可以水平分布


### 2. HTML5 有哪些新特性？
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


### 3. HTML5 引入什么新的表单属性
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


### 4. 与 HTML4 相比 HTML5 废弃了哪些元素
* basefont、big、center、font、s、strike、tt、u、frame、frameset、noframes


###	5. 如何处理 HTML5 新标签的浏览器兼容问题
```javascript
<!--[if lt IE 9]>
<script type="text/javascript" src="js/html5shiv.js"></script>
<![endif]-->
```

### 6. URI与URL的区别
* URI：统一资源标识符，标识一个资源
* URL：统一资源定位器，就是一个具体的url
* URL包含（协议，ip或域名，端口，目录，文件名）


### 7. 说说你对WebCodecs API的理解

* 允许 Web 程序对视频，音频进行进行编码的 API


### 8. DOCTYPE 的作用是什么？

    IE5.5 引入了文档模式的概念，而这个概念是通过使用文档类型（DOCTYPE）切换实现的。

    <!DOCTYPE>声明位于 HTML 文档中的第一行，处于 <html> 标签之前。告知浏览器的解析器用什么文档标准解析这个文档。

    DOCTYPE 不存在或格式不正确会导致文档以兼容模式呈现。


### 9. 标准模式与兼容模式各有什么区别?

* 标准模式：渲染方式与js引擎解析都是按照最高标准进行的
* 兼容模式：页面已宽松向后兼容方式显示，模拟老式浏览器行为以防止网站无法工作


### 10. HTML5 为什么只需要写 <\!DOCTYPE HTML>，而不需要引入 DTD？
* HTML5 不基于 SGML（标准通用标记语言），所以不用引入 DTD ，但是需要引入DOCTYPE来规范浏览器的行为，而 HTML4.01 基于 SGML ，所以需要对 DTD 进行引用，才能告知浏览器文档所使用的文档类型


### 11. SGML 、 HTML 、DTD、XML 和 XHTML 的区别？
* SGML（Standard Generalized Markup language）是标准通用标记语言，是一种定义电子文档结构和描述其内容的国际标准语言，是所有电子文档标记语言的起源。

* HTML（HyperText Markup Language）是超文本标记语言，主要是用于规定怎么显示网页。

* DTD（ Document Type Definition 文档类型定义）是一组机器可读的规则，它们定义 XML 或 HTML 的特定版本中所有允许元素及它们的属性和层次关系的定义。在解析网页时，浏览器将使用这些规则检查页面的有效性并且采取相应的措施，DTD 是对 HTML 文档的声明，还会影响浏览器的渲染模式（工作模式）

* XML（Extensible Markup Language）是可扩展标记语言是未来网页语言的发展方向，XML 和 HTML 的最大区别就在于 XML 的标签是可以自己创建的，数量无限多，
而 HTML 的标签都是固定的而且数量有限。

* XHTML（Extensible Hypertext Markup Language）也是现在基本上所有网页都在用的标记语言，他其实和 HTML 没什么本质的区别，标签都一样，用法也都一样，就是比 HTML 
更严格，比如标签必须都用小写，标签都必须有闭合标签等。


### 12. 行内元素定义

    元素被分为行内元素（inline）和 块元素（block），一个行内元素只占据它对应标签的的边框所包含的空间，常见的行内元素有 a b span img strong sub sup button input label select textarea


### 13. 块级元素定义
    块级元素占据父容器的整个宽度，因此创建了一个块
    常见的块级元素有  div ul ol li dl dt dd h1 h2 h3 h4 h5 h6 p 


### 14. 行内元素与块级元素的区别
* 格式上，默认情况下，行内元素不会以新行开始，而块级元素会新起一行

* 内容上，默认情况下，行内元素只能包含文本和其他行内元素。而块级元素可以包含行内元素和其他块级元素

* 行内元素与块级元素属性的不同，主要是盒模型属性上：行内元素设置 width 无效，height 无效（可以设置 line-height），设置 margin 和 padding 的上下不会对其他元素产生影响


### 15. HTML5 元素的分类
* 因为在HTML4中经常吧行内与块元素进行替换，所以简单的分为行内跟块级不符合实际需求了

* HTML5中，元素主要分为7类：Metadata Flow Sectioning Heading Phrasing Embedded Interactive


### 16. 空元素定义
* 标签内没有内容的 HTML 标签被称为空元素，空元素一般都是单标签，常见的空元素有：br hr img input link meta


### 17. link 标签定义
* link 标签定义文档与外部资源的关系

* link 元素是空元素，它仅包含属性。 此元素只能存在于 head 部分，不过它可出现任何次数，也就可以引入多个外部资源

* link 标签中的 rel 属性定义了当前文档与被链接文档之间的关系。常见的 stylesheet 指的是定义一个外部加载的样式表，icon 指图标，preload 指浏览器应该预先加载资源，


### 18. 页面导入样式时，使用 link 和 @import 有什么区别

* 从属关系区别。 @import 是 CSS 提供的语法规则，只有导入样式表的作用；link 是 HTML 提供的标签，不仅可以加载 CSS 文件，还可以定义 RSS、rel 连接属性、引入网站图标等

* 加载顺序区别。加载页面时，link 标签引入的 CSS 被同时加载；@import 引入的 CSS 将在页面加载完毕后被加载，所以建议使用 link 加载css

* 兼容性区别。@import 是 CSS2.1 才有的语法，故只可在 IE5+ 才能识别；link 标签作为 HTML 元素，不存在兼容性问题

* DOM 可控性区别。可以通过 JS 操作 DOM ，插入 link 标签来改变样式；由于 DOM 方法是基于文档的，无法使用 @import 的方式插入样式


### 19. 你对浏览器的理解
>