## JavaScript部分
### JavaScript 中有几种数据类型
* 基本数据类型：String、Number、Boolean 、Undefined、Null、Symbol（es6）、BigInt（es10）
  * Symbol：代表创建后独一无二且不可变的数据类型，它的出现我认为主要是为了解决可能出现的全局变量冲突的问题
  * BigInt：是一种数字类型的数据，它可以表示任意精度格式的整数，使用 BigInt 可以安全地存储和操作大整数，即使这个数已经超出了 Number 能够表示的安全整数范围。
* 引用数据类型：Object，其中Object可以分为：Array，Date 但是typeof可以识别出Function

### 查看数据类型
* typeof：只可以分辨出基本数据类型和引用数据类型，引用数据类型都为object

```javascript
console.log(typeof '') // string
console.log(typeof 1) // number
console.log(typeof true) // boolean
console.log(typeof Symbol()) // symbol
console.log(typeof undefined) // undefined
console.log(typeof function () {}) // function
console.log(typeof 11n) // bigint, 需要直接在浏览器中执行
console.log(typeof null) // object
console.log(typeof []) // object
console.log(typeof {}) // object
console.log(typeof new Date()) // object
console.log(typeof new RegExp()) // object
```

* instanceof：可以精准的检测引用数据类型

```javascript
console.log({} instanceof Object) // true
console.log([] instanceof Array) // true
console.log([] instanceof Object) // true
console.log('1' instanceof String) // false
console.log(new String('1') instanceof String) // true
```

```javascript
// 手写instanceof
function instance_of(L, R) {
    // 如果为基本数据类型直接返回false
    const arr = ['string', 'number', 'boolean', 'symbol', 'bigint', 'undefined']
    if (arr.includes(typeof L)) {
      return false
    }
    // 获取隐式原型
    L = L.__proto__
    // 获取显示原型
    const RR = R.prototype

    while(true) {
      // 当隐式原型为null 查找结束且没有找到
      if (L === null) {
        return false
      }
      // 当显示原型全等于隐式原型则返回true
      if (L === RR) {
        return true
      }
      // 没有找到继续向上一层原型查找
      L = L.__proto__
    }
  }
  console.log(instance_of([], Object))
```

* Object.prototype.toString.call()：可以精准的检测所有的数据类型

```javascript
console.log(Object.prototype.toString.call('')) // [object String]
console.log(Object.prototype.toString.call(1)) // [object Number]
console.log(Object.prototype.toString.call(true)) // [object Boolean]
console.log(Object.prototype.toString.call(Symbol())) // [object Symbol]
console.log(Object.prototype.toString.call(undefined)) // [object Undefined]
console.log(Object.prototype.toString.call(null)) // [object Null]
console.log(Object.prototype.toString.call(function () {})) // [object Function]
console.log(Object.prototype.toString.call([])) // [object Array]
console.log(Object.prototype.toString.call({})) // [object Object]
console.log(Object.prototype.toString.call(11n)) // [object BigInt], 需要直接在浏览器中执行
```

### 值类型和引用类型的区别
* 存储位置不同：值类型：栈，引用类型：堆
* 复制方式不同：值类型是深拷贝，引用类型则是浅拷贝
* 值类型无法添加属性，引用类型可以添加属性
* 值类型是比较值，引用类型是比较引用地址比较

### 类型转换
* 一般判断null，undefined使用== 其他一律使用===

### ==规则
* 对象==字符串：对象.toString()变为字符串
* null==underfined相等，但是和其他值比较就不在相等了
* NaN==NaN不相等
* 剩下的都转换为数组

### 列举强制类型转换和隐式类型转化
* 强制：parseInt，parseFloat，toString
* 隐式：if， 逻辑，==，+拼接字符串

### JavaScript 数据类型，null不存在的原因
* 不存在的原因：方法不存在，对象不存在，字符串变量不存在，接口类型对象没初始化
* 解决方法：做判断处理的时候，放在设定值的最前面
* null本质：null不是一个空引用，而是一个原始值，它只是期望此处用引用一个对象

```javascript
null == null // true
null === null // true
undefined === undefined // true
```

### JavaScript 什么叫全局变量?什么叫局部变量了?是如何定义出来的?
* 全局变量是在函数外部定义的变量,在JS中全局变量属于window对象,其作用域是整个源程序,全局变量全部存放在静态存储区,在程序开始执行时给全局变量分配存储区,程序运行完毕就释放。
  局部变量是相对与全局变量而言的,在特定过程或函数中可以访问的变量,作用域较小,当函数运行结束释放局部变量。

### JavaScript 书写规范/原则
>1.不要在同一行声明多个变量。
>2.请使用 === /!==来比较true/false或者数值
>3.使用对象字面量替代new Array这种形式
>4.尽量不使用全局函数。
>5.switch语句必须带有default分支
>6.函数不应该有时候有返回值，有时候没有返回值。
>7.for循环必须使用大括号
>8.if语句必须使用大括号
>9.for-in循环中的变量 应该使用let或const关键字明确限定作用域，从而避免作用域污染。

### JavaScript 说一说html代码,css代码和js代码的注释写法?
>HTML：<!--注释的内容-->
>CSS：/* 注释内容 */
>JS：//注释内容			/*注释内容*/

### JavaScript NaN是什么意思?这个值有什么特点?
>NaN 表示不是一个数,但是它本身是 number 类型。
>NaN 和 NaN 不相等

### JavaScript new操作符具体干了什么呢?
>创建一个空对象,并且 this 变量引用该对象,同时还继承了该函数的原型
>属性和方法被加入到 this 引用的对象中
>新创建的对象由 this 所引用,并且最后隐式的返回 this

```javascript
function Person(name) {
    this.name = name,
    this.say = function() {
      console.log(this.name)
    }
  }

  function _new(fun, ...arg) {
    // 1、创建一个空对象
    let obj = new Object()
    // 2、修改obj原型为fun的原型
    obj.__proto__ = fun.prototype
    // 3、修改this指向，并把参数传递过去
    let res = fun.call(obj, ...arg)
    // 4、根据规范，返回null和undefined不处理，依然返回obj
    return res instanceof Object ? res : obj
  }

  let byte = _new(Person, '字节跳动')
  byte.say()
```

### js装箱与拆箱
* 装箱：把基本数据类型转换为对应的引用数据类型的操作，（创建一个对象，在实例上调用方法，销毁实例）
* 拆箱：把引用类型转换为基本数据类型的操作

### 对JSON的了解?
>JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式
>它是基于JavaScript的一个子集
>数据格式简单, 易于读写, 传输占用带宽小
>JSON对象包含两个方法: 用于解析 JSON 的 parse() 方法，以及将对象/值转换为 JSON字符串的 stringify() 方法	深拷贝
>除了这两个方法, JSON这个对象本身并没有其他作用，也不能被调用或者作为构造函数调用
>json的key和value支持的数据类型为：数组(array)、对象(object)、字符串(string)、数字(number)、布尔型(boolean)、NULL

### JavaScript document.write 和 innerHTML的区别?
>document.write 只能重绘整个页面
>innerHTML 可以重绘页面的一部分

### JavaScript 哪些操作会造成内存泄露?
* 什么是内存泄露：内存泄漏指任何对象在你不再拥有或需要它之后仍然存在
* 哪些操作会造成内存泄露
>setTimeout 的第一个参数使用字符串而非函数的话,会引发内存泄漏
>闭包
>控制台日志
>循环引用（在两个对象彼此引用且彼此保留时,就会产生一个循环）
>DOM插入顺序

* 如何检测
>最简单的检测内存泄漏的方式是用任务管理器检查内存使用情况。在Chrome浏览器的新选项卡中打开应用并查看内存使用量是不是越来越多

### JavaScript 什么是"use strict"?使用它的好处和坏处分别是什么?
* 严格模式
* 优点：
>消除Javascript语法的一些不合理、不严谨之处,减少一些怪异行为
>消除代码运行的一些不安全之处,保证代码运行的安全
>提高编译器效率,增加运行速度
>为未来新版本的Javascript做好铺垫，	经过测试 IE6,7,8,9 均不支持严格模式

* 缺点
>现在网站的 JS 都会进行压缩,一些文件用了严格模式,而另一些没有
>这时这些本来是严格模式的文件,被 merge 后,这个串就到了文件的中间,不仅没有指示严格模式,反而在压缩后浪费了字节

### JavaScript eval是做什么的?
>eval()函数,这个函数可以把一个字符串当作一个JavaScript表达式一样去执行它
>它的功能是把对应的字符串解析成JS代码并运行
>应该避免使用eval,不安全,耗性能（2次,一次解析成js语句,一次执行）

### javascript的垃圾回收机制（GC）
* 标记清除法：当变量进入执行环境（函数中声明变量）的时候，垃圾回收机制将其标记为‘进入变量’，当变量离开环境的时候（函数执行结束）将其标记为‘离开环境’，在离开环境之后还有的变量则是需要被删除的变量，标记方式不定，可以是某个特殊位的反转或维护一个列表等。
* 引用计数法：该方法容易引起内存泄漏，机制就是跟踪一个值的引用次数，当声明一个变量并将一个引用类型赋值给该变量时该值引用次数加1，当这个变量指向其他一个时该值的引用次数便减一。当该值引用次数为0时就会被回收。

### 浏览器的严格模式与混杂模式
* 浏览器解析时到底使用严格模式还是混杂模式，与网页中的 DTD(Document Type Definition) 直接相关
>1、如果文档包含严格的 DOCTYPE ，那么它一般以严格模式呈现。（严格 DTD ——严格模式）
>2、包含过渡 DTD 和 URI 的 DOCTYPE ，也以严格模式呈现，但有过渡 DTD 而没有 URI （统一资源标识符，就是声明最后的地址）会导致页面以混杂模式呈现。（有 URI 的过渡 DTD ——严格模式；没有 URI 的过渡 DTD ——混杂模式）
>3、DOCTYPE 不存在或形式不正确会导致文档以混杂模式呈现。（DTD不存在或者格式不正确——混杂模式）
>4、HTML5 没有 DTD ，因此也就没有严格模式与混杂模式的区别，HTML5 有相对宽松的语法，实现时，已经尽可能大的实现了向后兼容。（ HTML5 没有严格和混杂之分）

### JavaScript 谈谈 this 对象的理解
>this 指的是调用函数的那个对象
>this 在没有运行之前不能知道代表谁;js的this 指向是不确定的；和定义没有关系，和执行有关
>执行的时候，点前面是谁，this 就是谁；自执行函数里面的this 代表的是 window
>定时器书写的时候，window可以省略掉；定时器执行的时候，里面的this 代表的也是 window
>this 是js的一个关键字，随着函数使用场合不同，this 的值会发生变化

### JavaScript 什么是window对象? 什么是document对象?
>document 是 window 的一个对象属性
>window 对象表示浏览器中打开的窗口
>如果文档包含框架（frame 或 iframe 标签），浏览器会为 HTML 文档创建一个 window 对象，并为每个框架创建一个额外的 window 对象
>所有的全局函数和对象都属于Window 对象的属性和方法
>document 对 Document 对象的只读引用

### JavaScript原型，原型链 ? 有什么特点
* 原型
>在JavaScript中,一共有两种类型的值,原始值和对象值.每个对象都有一个内部属性 prototype,我们通常称之为原型
>原型的值可以是一个对象,也可以是null
>访问一个对象的原型可以使用Object.getPrototypeOf方法,或者`__proto__`属性

* 原型链：原型向上查找的过程属于原型链
* 特点：
>可以向上查找，不能向下查找
>原型链的作用是用来实现继承的

### Javascript中，有一个函数，执行时对象查找时，永远不会去查找原型，这个函数是?
* javaScript中hasOwnProperty函数方法是返回一个布尔值，指出一个对象是否具有指定名称的属性。object.hasOwnProperty(proName)

### 把 Script 标签 放在页面的最底部的body封闭之前 和封闭之后有什么区别？浏览器会如何解析它们？
* 从实际效果来看，是没有区别的，但是放在 html 结束标签之前应该是不规范的(通不过 HTML 验证)，放在 body 结束之前才对，

### script标签的位置会影响首屏时间么？
* 不影响但有可能截断首屏的内容，使其只显示上面一部分

### cookie字段
* name：字段为一个cookie的名字
* value：字段为一个cookie的值
* domain：字段为可以访问cookie的域名
* path：字段为可以访问此cookie的页面路径
* expires：字段为cookie的超时时间，不设置默认为session
* size：字段为cookie大小
* http：字段cookie的httponly属性，若此属性为true，则只有在http请求头中会带有此cookie的信息，而不能通过document.cookie来访问此cookie
* secure：字段设置是否能通过https来传递此条cookie

### cookie的有效作用域（通过domain来设置作用域的）
* 设置cookie时省略domain参数，那么domain默认为当前域名
* domain可以设置父域名或自身，但不能设置其他域名，包括子域名，否则cookie不起作用

### cookie，localstorage，sessionstorage区别
* 存储大小：cookie4k，storage5M
* 有效期：cookie拥有有效期，sessionstorage会话存储，关闭浏览器消失，localstorage永久存储
* cookie会发送到服务端，存储在内存中，sessionstorage存储在内存，localstorage只存储在浏览器端
* 路径：cookie有路径限制，storage只存储在域名下
* API：cookie没有特定的API，storage有特定的API
* 作用域：不同浏览器无法共享localstorage，sessionstorage，相同浏览器的不同页面间可以共享相同的localstorage，不同页面或标签页间无法共享sessionstorage，

### typeof可以区分哪些类型
* 识别所有值类型
* 识别函数
* 判断是否是引用类型（不可再区分）

### 浏览器事件机制中，事件触发的三个阶段
* 捕获阶段：事件从根节点流向目标节点，途中流经各个DOM节点，在各个节点上触发捕获事件，直到达到目标节点
* 目标阶段：事件到达目标节点时，就到了目标阶段，事件在目标节点上被触发
* 冒泡阶段：事件在目标节点上触发后，不会终止，一层层向上冒，回溯到根节点

### javascript中for of和for in的区别
* 推荐循环对象使用for in，遍历数组使用for of
* for in循环出是key，for of循环出是value
* for of是ES6引入修复了for in的不足
* for of不能遍历普通对象，需要通过和`Object.keys()`（遍历key值），`Object.values()`（遍历value值），`Object.entries()`（key与value同时遍历）搭配使用

### 同步机制有哪些
* 临界区，互斥对象，信号量，事件对象

### 同步与异步的区别
* 异步基于js单线程
* 异步不会阻塞代码的执行，同步会阻塞代码的执行
* 执行顺序：同步 —> 微任务（promise） —> 宏任务（settimeout）
|宏任务|浏览器|Node|
|:-:|:-:|:-:|
|setTimeout|有|有|
|setInterval|有|有|
|setImmediate|无|有|
|requestAnimationFrame|有|无|

|微任务|浏览器|Node|
|:-:|:-:|:-:|
|process.nextTick|无|有|
|MutationObserve|有|无|
|promise|有|有|

### DOM是那种数据结构
* DOM树，树形结构

### attribute和propert的区别
* propert：修改对象属性，不会体现到html结构中
* attribute：修改html属性，会改变html结构
* 两者都可能引起DOM重新渲染，尽量使用propert

### 一次性插入多个DOM，怎样优化
* 可以使用createDocumentFragment进行操作
* 考虑dom节点的缓存，尽可能先获取所有的然后进行缓存，然后通过缓存进行操作，尽可能不边操作边获取dom

### 如何减少DOM操作
* 缓存DOM查询结果
* 多次DOM操作，合并到一次插入


### 编写一个通用的监听函数
```javascript
function bindEvent (elem, type, select, fun) {
  // 判断是否有回调
  if (fun == null) {
    fun = select // 没有回调，fun等于第三个参数
    select = null // 第三个参数为null
  }
  elem.addEventListener(type, event => {
    // 判断是否有第三个参数
    if (select) {
      // 代理绑定，matches：判断一个dom元素是否符合与一个css选择器
      if (event.target.matches(select)) {
        fun.call(elem, event)
      }
    } else {
      // 普通绑定
      fun.call(elem, event)
    }
  })
}

bindEvent(div, 'click', 'button', () => {
  console.log('button')
})
bindEvent(div, 'click', () => {
  console.log('1')
})
```

### 描述冒泡事件流程
* 基于DOM树形结构
* 事件会顺着触发元素向上冒泡
* 应用场景：代理

### 无限下拉的图片列表，如何监听每个图片的点击
* 事件代理
* 用e.target获取触发元素
* 用matches来判断是否触发元素

### 手写ajax
* get

```javascript
const xhr = new XMLHttpRequest()
xhr.open('GET', 'city.json', true)
xhr.onreadystatechange = () => {
  if (xhr.readyState === 4) {
    if (xhr.status === 200) {
      console.log(JSON.parse(xhr.responseText))
    }
  }
}
xhr.send(null)
```

* post：发送内容为字符串而不是json

```javascript
const xhr = new XMLHttpRequest()
// false：同步。true：异步
xhr.open('POST', 'city.json', true)
xhr.onreadystatechange = () => {
  if (xhr.readyState === 4) {
    if (xhr.status === 200) {
      console.log(JSON.parse(xhr.responseText))
    }
  }
}
const data = {
  name: 'byron',
  password: '123'
}
xhr.setRequestHeader("content-type","application/x-www-form-urlencoded")
xhr.send(JSON.stringify(data))
```

* xhr.readyState:
0：未初始化，还没有调用send()方法
1：载入，以调用send()方法，正在发送请求
2：载入完成 send()方法执行完成，已经接收到全部响应内容
3:   交互，正在解析响应内容
4：完成，响应内容解析完成，可以在客户端调用

* xhr.status：http状态码
2xx：表示成功的处理
3xx：需要重定向，浏览器直接跳转，301，302，304
4xx：客户端请求错误，404，403
5xx：服务端错误

### 跨域部分
* 同源策略：ajax请求时，浏览器要求当前网页和server必须同源（安全）同源：协议，域名，端口，三者必须一致
* 加载img，css，js可以无视同源策略，jsonp
* 哪些情况会发生跨域现象：AJAX，cookie，iframe

### 解释 JSONP原理，为何不是真正的ajax
* script 可以绕过浏览器的跨域限制，jsonp只能使用get
* 服务器可以任意动态拼接数据返回
* script 可以获得跨域的数据，只要服务端愿意返回
* script，img标签都可以绕过跨域

```javascript
// 手写简易jsonp
// 第一步创建jsonp.js文件并添加服务（跑起来后假如地址为：http://172.20.10.12:8080）
abc(
  {
    name: 'byron'
  }
)
// 第二步在html文件中使用script调用该jsonp.js的地址（说加载脚本，实则获取数据）（重新跑这个页面，肯定与jsonp.js文件的地址不同，然后就可以进行跨域了）
<!DOCTYPE html>
<html>
<head>
  <title>jsonp</title>
</head>
<body>
<script>
  window.abc = (data) => {
    console.log(data)
  }
</script>
<script type="text/javascript" src="http://172.20.10.12:8080/jsonp.js?user=byron&callback=abc"></script>
</body>
</html>
```

### CORS
* 在后端进行设置，前端可以直接进行访问

```javascript
// 第二个参数填写允许跨域名称，不建议直接写'*'
Response.setHeader('Access-Control-Allow-Origin', 'http://localhost:8081')
Response.setHeader('Access-Control-Allow-Header', 'X-Requested-With')
Response.setHeader('Access-Control-Allow-Methods', 'PUT,POST,GET,DELETE,OPTIONS')
// 接收跨域的cookie
Response.setHeader('Access-Control-Allow-Credentials', 'true')
```

### 描述cookie，localstorage，sessionstorage区别
* cookie：本身用于浏览器和server通讯，被借用到本地来存储，使用document.cookie进行访问赋值，最大存储4kb
* localstorage（永久存储），sessionstorage（会话存储）最大存储5M，不会随着http请求发送吗，setItem，getItem为api

### 经典面试题
```javascript
let test = {
  name: 'test',
  say: function () {
    console.log(this.name) // test
  },
  says: () => {
    console.log(this.name) // undefined
  }
}
test.say()
test.says()
```

### window.onload和DOMContentLoaded的区别
* window.onload：页面全部资源加载完成才会执行，包括图片，视频等
* DOMContentLoaded：DOM渲染完即可执行，此时图片，视频可能还没有加载完

### 防抖（频繁操作，最后进行触发）
* 监听一个输入框，文字变化后触发change事件
* 直接用keyup事件，则会频繁触发change事件
* 防抖：当用户输入结束或暂停时，才会触发change事件

```javascript
// 手写防抖函数
    function debounce(fun, delay = 500) {
      let timer = null
      return function () {
        if (timer) {
          clearTimeout(timer)
        }
        timer = setTimeout(() => {
          fun.apply(this, arguments)
          timer = null
        }, delay)
      }
    }

    input1.addEventListener('keyup',debounce(function () {
      console.log(this.value)
    }, 2000))
```

### 节流（频繁操作，保持一个频率连续触发）
* 拖拽一个元素时，要随时拿到元素被拖拽的位置
* 直接用drag事件，则会实时触发，很容易导致卡顿
* 节流：无论拖拽速度多快，都会每隔100ms触发一次

```javascript
// 手写节流函数
    function throttle(fun, delay = 500) {
      let timer = null
      return function () {
        if (timer) {
          return
        }
        timer = setTimeout(() => {
          fun.apply(this, arguments)
          timer = null
        }, delay)
      }
    }
    div1.addEventListener('drag', throttle(function (e) {
      console.log(e.offsetX, e.offsetY)
    }, 100))
```

### ES5和ES6实现每隔一秒打印一个数字
```javascript
  // ES6
  for (let i = 0; i < arr.length; i++) {
    setTimeout(() => {
        console.log(arr[i])
    }, 1000 * i)
  }
  // ES5
  for (var i = 0; i < arr.length; i++) {
    (function(i){
      setTimeout(function(){
        console.log(arr[i])
      }, 1000 * i)
    })(i)
  }
```

### 函数call和apply的区别
* 传参方式不同，call是依次传递参数，apply是传递一个数组

### 闭包是什么，有何特性，有何影响
* 闭包就是能够访问其他函数作用域中的变量
* 延长了变量的作用范围，加强封装性，可以达到对变量的保护
* 变量会常驻内存，得不到释放，闭包不要乱用

### 函数声明和函数表达式的区别
* 函数声明：function fun () {}
* 函数表达式：const fun = function () {}
* 函数声明会在代码执行前预加载（类似与变量预加载），而函数表达式不会

### new Object()和Object.create()区别
* {}等同于new Object()，原型Object.prototype
* Object.create(null)没有原型
* Object.create({})可以指定原型，且当前变量的`__proto__`等于指定变量

```javascript
const obj1 = {
    a: 10,
    b: 20,
    sum () {
      return this.a + this.b
    }
  }

  const obj2 = new Object({
    a: 10,
    b: 20,
    sum () {
      return this.a + this.b
    }
  })

  // const obj2 = new Object(obj1) // obj1 === obj2  true
  const obj3 = Object.create(null) // {} 没有原型
  const obj4 = new Object() // {} 有原型
  const obj5 = Object.create( { // {} 有原型,且内容在原型中
    a: 10,
    b: 20,
    sum () {
      return this.a + this.b
    }
  })
  const obj6 = Object.create(obj1) // obj6.__proto__ === obj1 true
```

### http方法有哪些
* get/post/put/delete/head/options

### get/post的区别
* 参数：
>get传递参数只能在url后面，文本格式为QueryString
>post可以传递application/x-www-form-urlencoded的类似QueryString、multipart/form-data的二进制报文格式，一般用于文件传输

* 用途：
>get用于从服务器端获取数据，包括静态资源（html/js/css/image等），动态资源（列表数据，详情数据）
>post用于向服务器提交数据，比如增删改数据，提交一个表单或者新建/修改一个用户

* 缓存：
>get默认会复用前面的请求数据作为缓存结果返回，解决方案：可以在url后面添加一个随机参数来进行更新缓存
>post一般不会被缓存因素影响

* 安全性:
>get：明文传输
>post：密文传输，相对来说更安全，但是安全的前提是正确使用二者

### ES6中import和commonJS中require的区别
* import是ES6写法，最终会转换为require，require遵循commonJS模块化解决方案
* ES6模块是编译时输出接口，commonJS模块是运行时加载
* ES6模块是动态引用，即导入和导出的值都指向同一个内存地址，所以导入的值会随着导出值变化。CommonJs的模块是对象。导出时是指拷贝，就算导出的值变化了，导入的值也不会变化，如果想要更新值就要重新导入
* import语句导入同一个模块如果加载多次只执行一次，require语句导入次数和实际执行次数相同
* import必须用在当前模块的顶层，如果在局部作用域内，会报错，es6这样的设计可以提高编译器效率，但没法实现运行时加载。require可以用在代码的任何地方
* import是关键词，require不是关键词
* require支持动态引入，也就是require(${path}/xx.js)，import目前不支持，但是已有提案

### 手写字符串trim写法
```javascript
String.prototype.trim = function() {
    return this.replace(/^\s+/, '').replace(/\s+$/, '')
  }
```

### 如何捕获js中的异常
* try{} catch(e){}
* window.onerror：对跨域的js，如CDN不会有详情信息，对于压缩js还需要配合sourceMap反查为压缩代码的行、列

### 获取url参数
```javascript
function query(name){
    const search = location.search
    const p = new URLSearchParams(search)
    return p.get(name)
  }
  // http://127.0.0.1:5501/jsInterview/32-query.html?b=%27123%27
  console.log(query('b')) // '123'
```

### 手写flag函数
```javascript
function flat(arr) {
    // 判断是否有二维数组
    let isDeep = arr.some(item => item instanceof Array)
    if(!isDeep) {
      return arr
    }
    const res = Array.prototype.concat.apply([], arr)
    return flat(res)
  }

  let arr = [1, [2, [3, [4, [5]]]]]
  console.log(flat(arr))
```

### 手写深度比较（全相等）
```javascript
// 判断是否为object
  function isObj(obj) {
    return typeof obj === 'object' && obj !== null
  }
  // 深度比较（全相等）
  function isEqual(obj1, obj2) {
    // 查看参数是否为obj
    if (!isObj(obj1) && !isObj(obj2)) {
      // 如果都不是object那么就是值类型，一般不会是函数
      return obj1 === obj2
    }
    // 如果传递的是一个对象
    if (obj1 === obj2) {
      return true
    }
    // 判断参数个数
    const obj1keys = Object.keys(obj1)
    const obj2keys = Object.keys(obj2)
    if (obj1keys.length !== obj2keys.length) {
      return false
    }

    for (let key in obj1) {
      const res = isEqual(obj1[key], obj2[key])
      if (!res) {
        return false
      }
    }
    return true
  }
```

### 有以下3个判断数组的方法，分别输出什么
```javascript
let arr = [1, 2, 3]
// Object.prototype.toString.call()
console.log(Object.prototype.toString.call(arr)) // [object Array]
// instanceof 既可以判断父类也可以判断子类
console.log(arr instanceof Array) // trur
// Array.isArray(arr) // 可以用于prototype
console.log(Array.isArray(arr)) // trur
```

### 介绍RAF requestAnimationFrame
* 要想动画流畅，更新频率要60帧/s，即16.67ms更新一次视图
* setTimeout要手动控制频率，而RAF浏览器会自动控制
* 后台标签或隐藏iframe中，RAF会暂停，而setTimeout依然执行

### 手写call
```javascript
function person (a, b, c) {
  console.log(this.name)
  console.log(a, b, c)
}
const egg = {
  name: 'byron'
}

Function.prototype.myCall = function(obj) {
  // 第一步：判断当前obj是否为空，若为空设置为window
  var obj = obj || window;
  // 第二步：对形参添加一个函数指向this也就是调用该方法的函数
  obj.p = this
  // 第三步：设置一个存放除this以外参数的数组
  let newArr = []
  // 第四步：循环arguments，填充newArr
  for (let i = 1; i < arguments.length; i++) {
    // 这里要存储的类型为arguments[1],arguments[2]...
    newArr.push(`arguments[${i}]`)
  }
  // 第五步：使用eval函数解析字符串并执行
  let request = eval(`obj.p(${newArr})`)
  // 第六步：移除绑定
  delete obj.p
  // 返回内容
  return request
}

person.myCall(egg, 'egg', 'tianji', 'gua')
```

### 手写apply
```javascript
Function.prototype.myApply = function (obj, arr) {
  var obj = obj || window;
  let result
  obj.p = this
  
  if (!arr) {
    result = obj.p()
  } else {
    let newArr = []
    for (let i = 0; i < arr.length; i++) {
      newArr.push(`arr[${i}]`)
    }
    result = eval(`obj.p(${newArr})`)
  }

  delete obj.p
  return result
}
```

### 手写bind
```javascript
Function.prototype.myBind = function(obj) {
  // 判断是调取是否为函数
  if (typeof this !== 'function') {
    throw new TypeError('不是function')
  }
  // 保存当前this
  let that = this
  // 获取除arguments[0]的其他参数转换为数组（第一个括号内）
  let arr1 = Array.prototype.slice.call(arguments, 1)
  // 设置一个空函数，用来进行桥接prototype
  let o = function() {}
  // 定义返回方法
  newf =  function () {
    // 获取arguments参数并转换为数组(第二个括号内)
    let arr2 = Array.prototype.slice.call(arguments)
    // 将第一个数组与第二个数组合并
    let arr = [...arr1, ...arr2]
    // 如果new后 this指向空
    if (this instanceof newf) {
      that.myApply(this, arr)
    } else {
      // 反之则指向当前对象
      that.myApply(obj, arr)
    }
  }
  // newf.prototype = that.prototype
  // 设置空函数的原型为调用函数的prototype
  o.prototype = that.prototype
  // 设置返回函数的原型为空函数的原型，这样做的目的是提高性能
  newf.prototype = new o
  // 返回函数
  return newf
}
person.myBind(egg, 'egg', 'tianji', 'gua')('hello')
```

### 手写sleep
```javascript
// 测试函数
  function fn() {
      console.log('测试延迟')
    }
  // 第一种：回调函数
  function sleep(callback, time) {
    if (typeof callback === 'function') {
      setTimeout(callback, time)
    }
  }
  sleep(fn, 3000)


  // 第二种：promise
  function sleep(timer) {
    return new Promise(resolve => setTimeout(resolve, timer))
  }
  sleep(3000).then(() => {
    fn()
  })

  // 第三种：Generator
  function * sleep(timer) {
    yield new Promise(resolve => setTimeout(resolve, timer))
  }
  sleep(3000).next().value.then(value => {
    fn()
  })
```

### async/await原理
* async函数就是generator函数的语法糖，async就是将generator函数的 * 换成了async，将yield换成了await
* async针对generator做了一下改进：
1. 内置执行器，不需要使用next()手动执行
2. await后可以是一个promise对象或者原始类型的值，yield后只能是函数或者promise对象
3. 返回非promise时，async会将返回值包裹为promise

###  js中 0.1+0.2 为什么不等于0.3 怎样解决
* 在JavaScript中的二进制的浮点数0.1和0.2并不是十分精确，在他们相加的结果并非正好等于0.3，而是一个比较接近的数字 0.30000000000000004 ，所以条件判断结果为false
* 解决方法：设置一个误差范围值，Number.EPSILON，这个值正等于2^-52，这时候就可以这样写 (0.1+0.2)-0.3小于Number.EPSILON，在这个误差的范围内就可以判定0.1+0.2===0.3为true。

### 你在类中使用super是做什么
1. 一般当一个类把父类的方法覆盖掉后，如果这个时候还想调用父类方法，那就可以使用super调用父类方法
2. 如果子类继承父类后，子类需要在constructor中手中执行super()

### 小题
1. 

```javascript
// 数字与字符串进行比较：
// 1：如果是纯数字与数字型字符串比较("999"<1000)，那么字符串会转化为数字，parseInt()
// 2：数字与其他字符串之间比较(222<'abc')，字符串转换为数字，parseInt('abc')，解析不到数字返回一个NAN
// 3：数字型字符串比较("999"<"1000")，比较ASCll码值，依次取每个字符，字符转为ASCll码进行比较，ASCll码先大的即为大
// 4：其他字符串进行比较('d'>'abc')，字符串比较为ASCII码比较
"999" < 1000 //true
999 < "1000" //true
"999" < "1000" // false
"999" == 999 //true
"999" === 999 //false

console.log([1,2,3,4,5].splice(1,2,3,4,5)) // [2,3]
console.log([1,2,3,4,5].slice(1,2,3,4,5)) // [2]

var bar = [1,2,3]
for(var i in bar) {
  setTimeout(() => {
    console.log(bar[i]) // 3 3 3
  },0)
  console.log(bar[i]) // 1 2 3
}

Object.prototype.foo = 'Object'
Function.prototype.foo = 'Function'
function Animal() {}
var cat = new Animal()
console.log(cat.foo) // Object
console.log(Animal.foo) // Function
```
2. 倒计时一般都会放在后端进行处理，前端一般是防不住的
3. 用户登录状态存放在localStorage，cookie中
4. 网页一键换肤方法：替换类名方法，样式表方法，参数请求（url传参给后端，后端根据参数判断返回对应的html 服务端渲染）
5. 有时候会碰到这样的问题，存放图片的盒子要比图片本身大，前提是图片是100%宽或高，遇到这样的问题就把img变为块级元素，完美解决，因为img是文本类型inline-block，所以img总会有一点文本的高度

### 如何获取元素的兄弟节点

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--如何获取元素的兄弟节点？-->
  <div class="item">
    <div>1</div>
    <div>2</div>
    <div class="item3">3</div>
    <div>4</div>
    <div>5</div>
  </div>
</body>
<script>
  const item3 = document.querySelector('.item3')
  // 获取当前上一个元素
  const preItem = item3.previousElementSibling
  console.log(`上一个节点： ${preItem.innerHTML}`)
  // 返回下一个元素
  const nextItem = item3.nextElementSibling
  console.log(`下一个节点： ${nextItem.innerHTML}`)
  // 只读对象
  console.log(item3.previousSibling, item3.nextSibling)
  
  // 获取父级元素
  const parentItem = item3.parentElement
  // 获取所有的兄弟节点
  const allChildren = Array.from(parentItem.children)?.filter(item => item !== item3)
  console.log(...allChildren)
</script>
</html>

```


### 使用js写一个方法获取某个元素中所有class和id

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--使用js写一个方法获取某个元素中所有class和id-->
  <div class="item" id="id">
    <div class="item1" id="id1">1</div>
    <div class="item2" id="id2">2</div>
    <div class="item3" id="id3">3</div>
    <div class="item4" id="id4">4</div>
    <div class="item5" id="id5">5</div>
  </div>
</body>
<script>
  const allCLass = (Element) => {
      let ids = [],
          clas = []
      const AllChildren = Array.from(Element.children)
      for (const allChild of AllChildren) {
          const attrs = allChild.attributes
          ids.push(attrs.id.value)
          clas.push(attrs.class.value)
      }
      return {
          ids,
          clas
      }
  }

  const all = allCLass(document.querySelector('.item'))
  console.log(all)
</script>
</html>

```

### 网页加载过程
#### 从输入url到渲染页面的整个过程
* DNS解析：域名 -> ip地址

* 浏览器根据ip地址向服务器发起http请求

* 服务器处理http请求，并返回给浏览器

* 根据HTML代码生成DOM Tree

* 根据css代码生成CSSOM

* 根据DOM Tree和CSSOM整合形成Render Tree

* 根据Render Tree渲染页面

* 遇到script后分三种：
  1.未设置async，defer，浏览器继续加载js，并阻塞，等待js加载并执行完成，然后继续解析文档。
  2.设置async，异步加载脚本，脚本加载完立即执行脚本。（如果有多个async，谁先加载完谁先执行）
  3.设置defer，异步加载脚本，文档解析完成后，DOMContentLoaded事件调用前执行脚本，（如果有多个defer则按顺序进行加载，当所有defer加载完成后，按顺序执行脚本，即使前面脚本有延迟）

* 文档解析完成，此时document.readyState = 'interactive'

* 设置defer的js脚本执行

* document触发DOMContentLoaded事件，标志着程序执行由同步脚本阶段转换为事件驱动阶段

* 文档和所有资源加载完成，document.readyState = 'complete'，window触发onload事件

* 直至把Render Tree渲染完成

### 性能优化
* 多使用内存，缓存其他方法，（图片懒加载）
* 减少cpu计算量，减少网络加载耗时
* 加载更快
>减少资源体积，压缩代码
>减少访问次数，合并代码，ssr服务器端渲染，缓存
>使用更快的网络：CDN

* 渲染更快
>css放在header，js放在body最下面
>懒加载
>尽早开始执行js，用DOMContentLoaded触发
>对DOM查询进行缓存
>频繁DOM操作，合并到一起插入到DOM结构
>节流throttle	防抖debounce

### 常见的web前端攻击方式有哪些
* XSS跨转请求攻击

```shell
#模拟场景
#1:一个博客网站，我发表一篇博客并嵌入<script>脚本
#2:脚本内容：获取cookie，发送到我的服务器，我的服务器配合跨域
#3:发布这篇博客后，有人查看他，从而我获取到访问者的cookie
#预防XSS，可以使用npm的xss包来做这些事情
#1:替换特殊字符，如<变为&lt; >变为&gt;
#2:<script>变为&lt;script&gt;，直接显示，而不会被作为脚本运行
#3:前后端都需要做替换
```

* CSRF/XSRF跨站请求伪造

```shell
#模拟场景
#1:你正在购物，看中了某个商品，商品id是100
#2:付费接口是xxx.com/pay?id=100, 但是没有验证
#3:我是攻击者，我看中一个商品，id是200
#4:我向你发送一封电子邮件
#5:邮件正文隐藏这<img src=xxx.com/pay?id=200 />
#6:你一查看邮件，就帮我购买了id为200的商品
#预防XSRF
#1:使用POST接口
#2:增加验证，例如密码，短信验证，指纹，人脸识别
```


### 什么是堆？什么是栈？它们之间有什么区别和联系？
堆和栈的概念存在于数据结构中和操作系统内存中

* 在数据结构中，栈中的数据是先进后出，堆是一个优先队列，是按照优先级来进行排列的，优先级可以按照大小来规定

* 在操作系统中，栈内存由编译器自动分配释放，存放函数的参数值，局部变量的值等。其操作方式类似于数据结构中的栈，堆内存一般由个人释放，如果没有释放在程序结束时可能由垃圾回收机制回收


### 内部属性 [[Class]] 是什么
所有 type 返回 object 的对象内部都有一个 [[Class]] 的属性，这个属性一般是无法直接访问的，需要使用 Object.prototype.toString 来查看

```javascript
Object.prototype.toString.call([]) // '[object Array]'

class a {}
aa = new a()
Object.prototype.toString.call(aa) // '[object Object]'

// 我们自己创建的类使用 toString 就找不到对应的类型，因为 toString 找不到 toStringTag 所以返回的 Object

class b {
  get [Symbol.toStringTag]() {
    return "classB"
  }
}
bb = new b()
Object.prototype.toString.call(bb) // '[object classB]'

```

### 介绍 js 有哪些内置对象（全局的对象）?

js 中的内置对象主要指的是在程序执行前存在全局作用域里的由 js 定义的一些全局值属性、函数和用来实例化其他对象的构造函
数对象

这里所说的全局的对象不是我们自定义的 global 的对象，而是 js 的全局的对象

* 值属性：这些全局的属性返回一个简单的值，没有自己的属性方法，比如，Infinity、NaN、undefined、null

* 函数属性：全局函数可以直接调用的，不需要在调用的时候指定对象，并将返回值返回调用者，比如：eval()、parseFloat()、parseInt()

* 基本对象：基本对象是定义或使用其他对象的基础，比如：Object、Function、Boolean、Symbol、Error

* 工具类的对象：比如，Number、Math、Date

* 字符串对象：比如，String、RegExp

* 可索引的集合对象，也就是我们所说的数组，Array

* 键值对集合对象，也就是 key value 类型的，比如，Map、Set、WeakMap、WeakSet

* 矢量集合，SIMD 矢量集合中的数据会被组织为一个数据序列

* 结构话数据，这些对象用来表示和操作结构化的缓冲区数据，或使用 JSON 编码的数据，比如，JSON

* 控制抽象对象：比如，Promise、Generator

* 反射：比如，Reflect、Proxy

* 国际化：为了支持多语言处理而加入 ECMAScript 的对象，比如，Intl、Intl.Collator

* WebAssembly

* arguments


### undefined 与 undeclared 的区别

已经在作用域中声明但是未赋值的变量为 undefined，相反，没有在作用域中声明就使用的变量是undeclared（未声明的）

对于 undeclared 变量的引用，浏览器会报引用错误，如 ReferenceError: xxx is not defined, 但是我们可以使用 typeof 的安全防范机制来避免报错，因为对于 undeclared（或者 not defined ）变量，typeof 会返回 "undefined"


### null 和 undefined 的区别

undefined 与 null 都是基本数据类型

undefined 代表已声明但未赋值的变量，null 代表的是一个空对象，一般变量已声明未赋值会返回 undefined，null 主要用于赋值给可能返回对象的变量来做初始值

undefined 在 js 中不是一个保留字，这意味着我们可以使用 undefined 来作为一个变量名，这样的做法是非常危险的，它
会影响我们对 undefined 值的判断。但是我们可以通过一些方法获得安全的 undefined 值，比如说 void 0

```javascript
function a () {
  // 如果一个作用域内声明了 undefined 变量，那么就很危险了
  const undefined = 123
  console.log(undefined) // 123
}
console.log(undefined) // undefined

```

当我们对两种类型使用 typeof 进行判断的时候，Null 类型化会返回 “object”，这是一个历史遗留的问题。当我们使用双等
号对两种类型的值进行比较时会返回 true，使用三个等号时会返回 false

```javascript
null == undefined // true
null === undefined // false

```


### 如何获取安全的 undefined 值

我们可以使用 void xxx，一般都是用 void 0 来获取安全的 undefined

```javascript
void 0 // undefined
void 1 // undefined

```


### 说几条写 JavaScript 的基本规范

* if 、for 语句必须使用大括号

* switch 必须有 default 分支

* 进行比较时使用全等

* 不要使用魔法数字，尽可能使用枚举

* 变量使用 let const，少使用 var

* 不要在内置对象的原型上添加方法，如 Array, Date


### JavaScript 原型，原型链？ 有什么特点

* 原型
js 中使用构造函数创建一个对象，每一个构造函数都有一个 prototype 对象，这个对象包含了可以由该构造函数的所有实例共享的属性和方法。当我们使用构造函数新建一个对象后，在这个对象的内部将包含一个指针，这个指针指向构造函数的 prototype 属性对应的值，在 ES5 中这个指针被称为对象的**原型**。一般来说我们是不应该能够获取到这个值的，但是现在浏览器中都实现了 __proto__ 属性来我们访问这个属性，但是我们最好不要使用这个属性，因为它不是规范中规定的。ES5 中新增了一个 Object.getPrototypeOf() 方法，我们可以通过这个方法来获取对象的原型。

* 原型链
当我们访问一个对象的属性时，如果这个对象内部不存在这个属性，那么它就会去它的原型对象里找这个属性，这个原型对象又会有自己的原型，于是就这样一直找下去，也就是原型链的概念。原型链的尽头一般来说都是 Object.prototype 所以这就是我们新建的对象为什么能够使用 toString() 等方法的原因。

* 特点
JavaScript 对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变。


### js 获取原型的方法？

* p.__proto__

* p.constructor.prototype

* Object.getPrototypeOf()


### js 中不同进制数字的表示方式

* 以 0X、0x 开头的表示为十六进制。

* 以 0、0O、0o 开头的表示为八进制。

* 以 0B、0b 开头的表示为二进制格式。


### js 中整数的安全范围是多少

安全整数指的是，在这个范围内的整数转化为二进制存储的时候不会出现精度丢失

安全整数范围为 +-2^53 - 1，在 ES6 中被定义为 Number.MAX_SAFE_INTEGER 和 Number.MIN_SAFE_INTEGER，

可以使用isFinite来判断是否在安全整数区间，如果计算超过了最大值那么就会转换为 Infinity 值，然后就无法进行计算了，可以用 BigInt 来处理这样的数字


### typeof NaN 的结果是什么

```javascript
typeof NaN  // 'number'
NaN !== NaN // true

```

NaN 是一个特殊值，他和自身并不相等，是唯一一个非自反的值，用于指出数字类型中的错误情况，即“执行数学运算没有成功，这是失败后返回的结果”。


### isNaN 和 Number.isNaN 函数的区别

函数 isNaN 接收参数后，会尝试将这个参数转换为数值，任何不能被转换为数值的的值都会返回 true，因此非数字值传入也会返回 true ，会影响 NaN 的判断

```javascript
isNaN('q') // true
isNaN('-@#') // true
isNaN(NaN) // true

```

函数 Number.isNaN 会首先判断传入参数是否为数字，如果是数字再继续判断是否为 NaN ，这种方法对于 NaN 的判断更为准确

```javascript
Number.isNaN('q') // false
Number.isNaN('-@#') // false
Number.isNaN(NaN) // true

```


### Array 构造函数只有一个参数值时的表现

Array 只有一个参数的时候会被当作数据的预设长度来使用，不会重当数组中的元素

```javascript
Array(10) // [empty × 10]
Array(1,2) // [1, 2]

```


### 常见的值转换字符串的规则

```javascript
String(null) // "null"
String(undefined) // "undefined"
String(true) // "true"
String(false) // "false"
String(Symbol()) // "Symbol()"
String(1) // "1"
String(1.1) // "1.1"
String(NaN) // "NaN"

```


### 常见的值转换数字的规则

```javascript
Number(null) // 0
Number(undefined) // NaN
Number(false) // 0
Number(true) // 1
Number("") // 0
Number("123") // 123
Number("123a") // NaN
Number(Symbol()) // TypeError: Cannot convert a Symbol value to a number
Number([]) // 0
Number([1]) // 1
Number([1,2]) // NaN
Number(["a"]) // NaN
Number({}) // NaN

```


### 常见的值转换布尔值的规则

```javascript
Boolean(undefined) // false
Boolean(null) // false
Boolean(false) // false
Boolean(0) // false
Boolean(-0) // false
Boolean(+0) // false
Boolean(NaN) // false
Boolean("") // false
Boolean({}) // true
Boolean([]) // true
Boolean("false") // true
Boolean(1) // true

```


### {} 和 [] 的 valueOf 和 toString 的结果是什么

* {} 的 valueOf 结果为 {} ，toString 的结果为 "[object Object]"


* [] 的 valueOf 结果为 [] ，toString 的结果为 ""



### 什么是假值对象

浏览器在某些特定的情况下，在常规的 javascript 语法基础上自己创建了一些外来值，这些就是 假值对象，假值对象看起来和普通对象并无二致（都有属性，等等），但将它们强制类型转换为布尔值时结果为 false 最常见的例子是 document.all，它是一个类数组对象，包含了页面上的所有元素，由 DOM（而不是 JavaScript 引擎）提供给 JavaScript 程序使用

```javascript
const aa = document.all
Boolean(aa) // false
console.log(aa) //  HTMLAllCollection(301) [html, head, meta, meta, title, meta, meta, meta...

```


### ~ 操作符的作用
~ 返回 2 的补码，并且会将数字转换为32位整数，因此我们可以使用 ~ 来进行取整，~x 大致等同于 -(x+1)

```javascript
~1 // -2
~2 // -3
~2.5 // -3
~2.2 // -3
~0 // -1

```


### 解析字符串（parseInt）与强制转换数字（Number）的区别是什么

解析字符串（parseInt）： 如果内部包含非数字，那么将从左往右进行解析。遇到非数字停止，如果开头就是非数子，那么 NaN

强制转换数字（Number）：不允许内部包含非数字，否值直接 NaN

```javascript
parseInt('100') // 100
parseInt('100a') // 100
parseInt('10a0a') // 10
parseInt('a100a') // NaN

Number('100') // 100
Number('100a') // NaN
Number('a100') // NaN

```


### `+` 操作符什么时候用于字符串的拼接

如果操作符其中一方是 对象（包括数组），则首先调用 toPrimitive 抽象操作，该抽象到做再调用 DefaultValue，以数字作为上下文。如果不能转换为字符串，则会将其转换为数字类型来进行计算

如果 + 的其中一个操作符是字符串，则执行字符串的拼接，否则执行数字的加法

对于除了加法运算符外，只要其他一方是数字，那么另一方就会转换为数字


### 什么情况下会发生布尔值的隐式强制类型转换

1. if 语句条件判断表达式
2. for 语句中的条件判断表达式 第二个
3. while 和 do while 循环中的条件判断表达式
4. 三元表达式
5. 逻辑运算符 || and && 的 左边的操作符（作为条件判断表达式）


### || 和 && 操作符的返回值

|| 和 && 首先会对第一个操作数指定条件判断，如果不是布尔值就先进行判断

* ||
  如果第一个值条件判断为 true 就返回第一个操作数的值，如果为 false 就返回第二个操作数的值

* &&
  如果第一个值条件判断为 true 就返回第二个操作数的值，如果为 false 就返回第一个操作数的值

|| 和 && 返回它们其中一个操作数的值，而非条件判断的结果


### Symbol 值的强制类型转换

Symbol 值不能被强制转换为数字（显式根隐式都会报错），但可以转换为布尔值（显式和隐式结果都是 true ）


### == 操作符的强制类型转换规则

1. 字符串和数字比较，将字符串转换为数字之后再进行比较
2. 其他类型与布尔值相等比较，先将布尔值转换为数字后，在应用其他规则进行比较
3. null 和 undefined 之间的相等比较，结果为真。其他值和它们进行比较都返回假值
4. 对象和非对象之间的相等比较，对象先调用 ToPrimitive 抽象操作后，再进行比较
5. 如果一个操作值为 NaN ，则相等比较返回 false（ NaN 本身也不等于 NaN ）
6. 如果两个操作值都是对象，则比较它们是不是指向同一个对象。如果两个操作数都指向同一个对象，则相等操作符返回 true，否则，返回 false


### 如何将字符串转化为数字

1. 使用 Number，前提是我包含字符串不能有不合法字符
2. 使用 parseInt，解析字符串，并返回一个整数，第二个参数可以设置要解析数字的基数，可以理解为进制，注意第二个参数 10 不是默认值，而是 parseInt 会根据字符串来判断基数

```javascript
parseInt('0xf') // 15
parseInt('0xf', 16) // 15

```

3. 使用 parseFloat，解析字符串返回一个浮点数
4. 使用 + 操作符的隐士转换，前提是所包含的字符串不包含不合法字符


### 如何将浮点数点左边的数每三位添加一个逗号，如 12000000.11 转化为『12,000,000.11』

```javascript
const num = '12000000.11' // 注意这是字符串

function format(num){
	return num && num.replace(/(?!^)(?=(\d{3})+\.)/g, ",")
}

const num1 = 12000000.11 // 注意这里是数字

function format1(num){
  // toLocaleString 可以将数字替换为不同国家使用习惯的方式
  return num.toLocaleString('en')
}

```


### 




















