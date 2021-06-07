# Vue
## 基础知识
### 插值
>文本 {{}}，双花括号内部可以理解为是js代码
>纯html，使用v-html，一定要使用信任的标签

```javascript
<body>
  <div id="app">
    <p>{{10+20}}</p>
    <p>{{10>20?'大':'小'}}</p>
    <p>{{message}}</p>
    <p>{{test}}</p>
		<span v-text='message'></span>
    <p v-html='test'></p>
  </div>
  <div>{{10+20}}</div>
</body>
<script src="./vue.js"></script>
<script>
  var app = new Vue({
    el: '#app',
    data: {
      message: 'byron',
      test: '<b>111111111111</b>'
    }
  })
</script>
```

### v-text
*  v-text可以理解为进阶版的插值表达式，当网速较慢时，使用插值页面可能会先出现{{message}}在出现对应的值，若使用v-text='message'后，页面不会出现双花括号，进而不会影响用户体验

### v-if,v-show
>v-if：动态的创建和删除节点，为false时标签被删除
>v-show：动态的显示和隐藏节点，为false时自动为标签添加display:none

### v-model：双向绑定
>单选绑定绑布尔值
>多选绑定绑数组

* v-model.lazy：失去焦点那一刻进行同步
* v-model.trim：去除两边空格
* v-model.number：只能输入数字类型

### v-pre：原样输出
```javascript
// byron
<p>{{message}}</p>
// {{message}}
<p v-pre>{{message}}</p>
var app = new Vue({
  el: '#app',
  data: {
    message: 'byron'
  }
})
```

### v-cloak：渲染完成后才显示
### v-once：只在第一次渲染时进行渲染，之后不进行渲染

### v-class：动态绑定class属性名，简写 :class
>绑定对象与数组的区别，对象不能直接添加class属性名，即使添加成功，也无效，数组方式可以自由的添加

```javascript
<p :class="isClass?'red':'blue'" @click='handChangeColor'>点击切换颜色</p>
<p :class='{classA:isClass}'>111111</p>
<p :class='classobj'>111111</p>
<p :class='classarr'>22222</p>
var app = new Vue({
    el: '#app',
    data: {
      isClass: true,
      classobj: {
        a: true,
        b: true
      },
      classarr: ['a','b']
    },
    methods: {
      handChangeColor() {
        this.isClass = !this.isClass
      }
    },
  })
```

### v-style：动态绑定style属性，与v-class类似
### 条件渲染，v-if，v-else-if，v-else
### 列表渲染：v-for，支持数组与对象，:key值的设置要保证是唯一的，不建议使用index
### 数组更新检测
>可以检测变动的方法：push，pop，shift，unshift，splice，sort，reverse
>不能检测变动的方法：filter，concat，slice，map
>通过索引值修改：Vue.set（data，index，newValue） / app.$set

### 事件处理的写法

```html
<!-- 写法2是为了更好的传参 -->
<button @click='handclick'>click</button>
<button @click='handclick()'>click</button>
<button @click='isShow = !isShow'>click</button>
<div v-show='isShow'>11111111</div>
```

### 事件修饰符，阻止冒泡

```html
<ul @click.self='handUlClick()'>  <!-- 给当前阻止冒泡事件，即不是从内部触发的 -->
  <li @click.stop='handLiClick()'>111</li>  <!--阻止冒泡事件-->
  <li @click.once='handLiClick()'>222</li>  <!--添加单次绑定事件，只能触发一次-->
  <li @click.capture='handLiClick()'>333</li> <!--添加事件捕获模式，即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 -->
</ul>
<a href="http:www.baidu.com" @click.prevent='handAClick($event)' >跳转</a> <!--阻止默认跳转-->
```

### 按键修饰符
```html
<input type="text" @keyup.enter='handEnter()'>
<input type="text" @keyup.13='handEnter()'>
<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
<button @click.ctrl="onClick">A</button>
<!-- 有且只有 Ctrl 被按下的时候才触发 -->
<button @click.ctrl.exact="onCtrlClick">A</button>
<!-- 没有任何系统修饰符被按下的时候才触发 -->
<button @click.exact="onClick">A</button>
```

### 鼠标按键修饰符
```html
<button @click.left='onleft'>left</button>
<button @click.right='onright'>right</button>
<button @click.middle='onmiddle'>middle</button>
```

### 计算属性(computed)与方法(methods)，watch的区别
>methods中写的是一些方法，用于处理业务逻辑
>computed的本质是一个方法，性能更高，会优先使用缓存，会先对data数据做变化后再使用
>watch用于监听data中的以及其他（比如路由）的数据的变化，当被监听的内容发生变化时，触发相应的函数，从而进行某些逻辑操作。可以认为是methods和computed的结合体

### watch
```javascript
watch: {
    name (old, newdata) {
      console.log(old, newdata)
    },
    info: {
      // handler是watch中需要具体执行的方法
      handler (old, newdata) {
        console.log(old, newdata)  // 获取不到old
      },
      deep: true, // 深度监听，数组的变化不需要深度watch
      immediate: true // 立即执行，而不是对应值变化后执行
    }
  }
```

### ref
>放在标签上，获取原生DOM
>放在组件上，拿到组件对象，可以对子组件进行随意的控制，但是不建议使用，耦合度高

```javascript
<input type="text" ref='inp'>
this.$refs.inp.value // 获取指定DOM的value
```

### 事件总线(bus总线)，非父子通信
```javascript
// 子组件一传递数据给子组件二
<body>
  <div id="box">
    <author></author>
    <user></user>
  </div>
</body>
<script>
  // 第一步
  var bus = new Vue()   // 设置总线，类似于第三方
	// 子组件一
  Vue.component('author', {
    template: `
      <div>
        <input type='text' ref='myref' />
        <button @click='handClick'>发布</button>
      </div>
    `,
    methods: {
      handClick() {
        // author组件使用$emit向监听message的组件传递值
        bus.$emit('message', this.$refs.myref.value)
      }
    },
  })
  // 子组件二
  Vue.component('user', {
    template: `
      <div>
        用户接收：{{dataAuthor}}
      </div>
    `,
    data() {
      return {
        dataAuthor: ''
      }
    },
    mounted() {
      // user组件使用$on监听message，获取author的值
      bus.$on('message', (data) => {
        this.dataAuthor = data
      })
    },
    beforeDestroy() {
      // 及时销毁，否则可能造成内存泄露
      bus.$off('message', this.dataAuthor)
    }
  })

  new Vue({
    el: '#box'
  })
</script>
```

### 动态组件
* component元素，动态绑定多个组件到他的is属性中
* keep-alive：保留状态，避免重新渲染，对组件进行缓存

```javascript
<body>
  <div id="app">
    <button @click='who="home"'>home</button>
    <button @click='who="list"'>list</button>
    <button @click='who="shop"'>shop</button>
    <keep-alive>
      <component :is="who"></component>
    </keep-alive>
  </div>
</body>
<script>
  var app = new Vue({
    el: '#app',
    data: {
      who: ''
    },
    components:{
      'home': {
        template: `<div>home <input type='text' /></div>`
      },
      'list': {
        template: '<div>list</div>'
      },
      'shop': {
        template: '<div>shop</div>'
      },
    }
  })
</script>
```

### transition过渡
* 单元素过渡

```javascript
//style代码一下案例通用
  <style>
    .fade-enter-active,
    .fade-leave-active {
      transition: all .5s;
    }

    .fade-enter,
    .fade-leave-to{
      transform: translateX(100px);
      opacity: 0;
    }
  </style>
<body>
  <div id="app">
    <button @click='isShow=!isShow'>click</button>
    <transition name='fade'>
      <p v-show='isShow'>1111111111111</p>
    </transition>
  </div>
</body>
<script>
  var app = new Vue({
    el: '#app',
    data: {
      isShow: false
    }
  })
</script>
```

* 多个元素过渡（设置key）：当有相同标签名的元素切换时，需要通过key特性设置唯一的值来标定以让Vue区分它们，否则Vue为了效率只会替换相同标签内部的内容

```javascript
<body>
  <div id="app">
    <button @click='isShow=!isShow'>click</button>
    <!-- mode:out-in(先走再来)，in-out(先来再走) -->
    <transition name='fade' mode='out-in'>
        <p v-if='isShow' key='1'>1111111111111</p>
        <p v-else key='2'>22222222222222</p>
    </transition>
  </div>
</body>
<script>
  var app = new Vue({
    el: '#app',
    data: {
      isShow: false
    }
  })
</script>
```

* 多个组件过渡：对组件component进行包裹
* 列表过渡(设置key)：transition-group不同于transition，他是一个真实的元素呈现，默认为span标签，也可以通过tag特性更换其他元素

```javascript
<body>
  <div id="app">
    <input type="text" v-model='inputValue'>{{inputValue}}
    <button @click='handAdd'>添加</button>

      <transition-group name='byron' tag="ul">
        <li v-for='(data,index) of inputList' :key="data">{{data}}
          <button @click='handDel(index)'>删除</button>
        </li>
      </transition-group>
  </div>
</body>
<script>
  var app = new Vue({
    el: '#app',
    data: {
      inputValue: '',
      inputList: []
    },
    methods: {
      handAdd() {
        this.inputList.push(this.inputValue)
        this.inputValue = ''
      },
      handDel(index) {
        this.inputList.splice(index, 1)
      }
    },
  })
</script>
```

### vue自定义指令用法
* 操作底层DOM

```javascript
<body>
  <div id="app">
    <div v-hello="'skyblue'">1111111111</div>
  </div>
</body>
<script>
  //创建hello后可以直接使用v-hello，v-hello可以设置单个字符串或者对象
  Vue.directive('hello', {
    //指令-生命周期-创建
    inserted(el,bind) {
      console.log(el) //获取v-hello的DOM
      el.style.background=bind.value
    },
    //指令-生命周期-更新
    updated(el,bind) {
      el.style.background=bind.value
    },
  })

  var app = new Vue({
    el: '#app'
  })
</script>
  
directives: {
  hello: {
    inserted(el, bind) {
      el.style.background = bind.value
    },
      update(el, bind) {
        console.log(el, bind)
        el.style.background = bind.value
      }
  }
}
```

### vue过滤

```javascript
//方法一
{{ message | capitalize }}	//:src属性不用家花括号
Vue.filter('capitalize', function (value) {
  if (!value) return ''
  value = value.toString()
  return value.charAt(0).toUpperCase() + value.slice(1)
})
//方法二
<div class="pro-price">{{item.price | filter}}</div>
filters: {
    filter (value) {
      if (!value) return '0.00'
      //toFixed四舍五入两位
      return '￥' + value.toFixed(2) + '元'
    }
  }
```

## 高级特性
### 自定义v-model

```javascript
// 父组件
<template>
  <div>
    <p>自定义v-model</p>
    <p>{{name}}</p>
    <advance v-model="name" />
  </div>
</template>

<script>
import Advance from './Advance'
export default {
  components: {
    Advance
  },
  data () {
    return {
      name: 'byron'
    }
  }
}
</script>
// 子组件
<template>
  <input  type="text"
          :value="text"
          @input="$emit('change', $event.target.value) "
  />
</template>

<script>
export default {
  model: {
    prop: 'text', // 对应 props text1
    event: 'change'
  },
  props: {
    text: String,
    default () {
      return ''
    }
  }
}
</script>

```

### $nextTick
* vm.$nextTick：将回调延迟到下次 DOM 更新循环之后执行。在修改数据之后立即使用它，然后等待 DOM 更新。它跟全局方法 Vue.nextTick 一样，不同的是回调的 this 自动绑定到调用它的实例上，大意就是当DOM更新后执行该方法

```javascript
<template>
  <div>
    <ul ref="lil">
      <li v-for="(data, index) in list" :key="index">{{data}}</li>
    </ul>
    <button @click="addItem">添加一项</button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      list: [1, 2, 3]
    }
  },
  methods: {
    addItem () {
      this.list.push(`${Date.now()}`)
      this.list.push(`${Date.now()}`)
      this.list.push(`${Date.now()}`)
      // 异步渲染，$nextTick 待 DOM 渲染完再回调
      // 页面渲染时会将 data 的修改做整合，多次 data 修改只会渲染一次
      // 异步渲染，返回promise
      this.$nextTick(() => {
        const ulEle = this.$refs.lil
        console.log(ulEle.childNodes.length)
      })
      // 或者这样
      const elem = this.$refs.ull
      this.$nextTick().then(function () {
        console.log(elem.childNodes.length)
      })
    }
  }
}
</script>
```
### 插槽（slot）
* 使用插槽预存标签

```javascript
<body>
  <div id="app">
    <home>
      <div>123</div>
    </home>
  </div>
</body>
<script>
  var app = new Vue({
    el: '#app',
    components: {
      'home': {
        template: `
          <div>
            <slot></slot>
            home
          </div>
        `
      }
    }
  })
</script>
```

* 具名插槽

```javascript
<body>
  <div id="app">
    <home>
      <template>
        <div slot="a">aaaaaaa</div>
      </template>
      <div slot="b">bbbbbbb</div>
    </home>
  </div>
</body>
<script>
  var app = new Vue({
    el: '#app',
    components: {
      'home': {
        template: `
          <div>
            <slot name='a'></slot>
            home
            <slot name='b'></slot>
          </div>
        `
      }
    }
  })
</script>
```

### 动态，异步组件
* :is-"compontent-name" 用法
* 根据数据，动态渲染的场景，即组件类型不确定

```javascript
<template>
  <div>
    <!-- 查看is对应组件名称再进行data的对比，从而做到动态的渲染组件 -->
    <component :is='NextTickName' />
    <component :is='WatchDemoName' />
  </div>
</template>

<script>
// import NextTick from './NextTick'
import WatchDemo from './WatchDemo'
export default {
  data () {
    return {
      NextTickName: 'NextTick',
      WatchDemoName: 'WatchDemo'
    }
  },
  components: {
    // 异步加载，路由中也是同理
    NextTick: () => import('./NextTick'),
    WatchDemo
  }
}
</script>
```

### keep-alive(缓存)
* keep-alive包裹的组件中，会缓存不活动的组件实例，而不是销毁他们，也就是说不会执行destroyed钩子函数，从而提高性能

### mixin
* 多个组件有相同的逻辑，抽离出来

```javascript
// 举个例子
// mixinl.js
export default {
  methods: {
    onClick () {
      console.log(`mixin:${this.name}`)
    }
  },
  mounted () {
    console.log('我再mixin中挂载完啦')
  }
}
// 组件中
<template>
  <div>
    <button @click="onClick">name</button>
  </div>
</template>

<script>
import MiXin from './mixin.js'
export default {
  mixins: [MiXin], // 可以添加多个，会自动合并起来
  data () {
    return {
      name: 'byron'
    }
  },
  mounted () {
    console.log('我挂载完啦')
  }
}
</script>
```

* 缺点：
>变量来源不明确，可阅读性底
>多个mixin可能会造成命名冲突
>mixin和组件可能出现多对多的关系，复杂度较高

* 解决办法：Vue3提出了Composition API旨在解决这些问题

## 数据请求
### fetch
### axios

## 虚拟DOM
* 同层级对比，同key值对比，同组件对比

## 组件化
* 组件编写与vue实例的区别
>自定义组件需要有一个root element
>父子组件的data是无法共享的
>组件可以有data，methods，computed但是data必须是一个函数

### 全局组件
```javascript
  Vue.component('navbar', {
    template: `
    <div>
      <button @click='handClick()'>开始</button>
        nav
      <button>结束</button>
    </div>
    `,
    methods: {
      handClick() {
        console.log('begin')
      }
    }
  })
```

### 局部组件
* 在全局中定义的组件
```javascript
  Vue.component('navbar', {
    template: `
    <div>
      <button @click='handClick()'>开始</button>
        nav
        <navbarChild></navbarChild>
      <button>结束</button>
    </div>
    `,
    methods: {
      handClick() {
        console.log('begin')
      }
    },
    //局部组件定义
    components: {
      navbarChild: {
        template: `<div>navchild</div>`
      }
    }
  })
```

### 父传子
* 通过属性值向下传参
* 接收父组件与传递js对象需要使用v-bind进行动态绑定

```javascript
<body>
  <div id="app">
    <navbar :myname="home" :myshow="false"></navbar>
    <navbar :myname="list" :myshow="true"></navbar>
    <navbar :myname="parentName" :myshow="true"></navbar>
  </div>
</body>
<script>
  Vue.component('navbar',{
    template:`
      <div v-show='myshow'>
        nav---{{myname}}
      </div>
    `,
    // props:['myname', 'myshow']  //接收父组件传递的属性
    props: {  //属性验证
      myname: String,
      myshow: Boolean
    }
  })

  new Vue({
    el: '#app',
    data: {
      parentName: '123'
    }
  })
</script>
```

### 子传父
* 使用事件像上传递，子组件添加自定义事件并且通过$emit触发自定义事件，随之自定义事件出发后内容返回给父组件中

```javascript
<body>
  <div id="app">
    <navbar @myname='handParent($event)'></navbar>
  </div>
</body>
<script>
  Vue.component('navbar', {
    template: `
      <div>
        nav
        <button @click='handChild'>click</button>
      </div>
    `,
    data() {
      return {
        child: 'Child'
      }
    },
    methods: {
      handChild() {
        this.$emit('myname',this.child)
      }
    },
  })

  new Vue({
    el: '#app',
    methods: {
      handParent(e) {
        console.log(e)
      }
    }
  })
</script>
```

## 生命周期
### vue的生命周期有哪些
* beforeCreate：创建之前，初始化vue示例，data和methods中的数据都未初始化
* created：创建完成后 // 初始化实例，并没有开始渲染，data和methods初始化完成
* beforeMount：挂载之前，vue中的指令执行最终生成编译好的字符串模寄存在虚拟dom中，并没有真正生成到页面上
* mounted：挂载完了 // 页面绘制完成，一般在该函数下获取网络数据，将虚拟dom挂载到页面上
* beforeUpdate：更新之前
* updated：更新完了
* beforeDestroy：销毁之前 // 解除自定义事件event.$off，清除定时器，解除绑定的DOM事件（scroll等）
* destroyed：销毁完了

### vue父子组件生命周期怎么执行
#### 创建与挂载
```shell
父组件 create
子组件 create
子组件 mounted
父组件 mounted
```

#### 更新前与更新
```shell
父组件 beforeUpdate
子组件 beforeUpdate
子组件 updated
父组件 updated
```

#### 销毁前和销毁
```shell
父组件 beforeDestroy
子组件 beforeDestroy
子组件 destroyed
父组件 destroyed
```


## 跨域
* 跨域是浏览器为了安全而做出的限制策略
* 浏览器请求必须遵循同源政策：同域名，同端口，同协议

### CORS 跨域
* 服务端设置，前端直接调用，后台允许某个接口进行访问
* Access-Control-Allow-Origin：代表只接受某个地址的访问
* Access-Control-Allow-Credentials: true：代表跨域传递cookie，一般跨域cookie是带不过去的

### JSONP跨域
* 前端适配，后端配合
* 不是一个请求，本质上是一个js脚本

```shell
npm install jsonp
```

* 基本使用

```javascript
<script>
import jsonp from 'jsonp'
export default {
  data () {
    return {
      dataq: {}
    }
  },
  mounted () {
    const url = 'https://u.y.qq.com/cgi-bin/musics.fcg?-=getUCGI7109439530124504&g_tk=5381&sign=zzagamo8jjyko702f3fef04bda3a286eeb40136f6b80f4a&loginUin=0&hostUin=0&format=json&inCharset=utf8&outCharset=utf-8&notice=0&platform=yqq.json&needNewCode=0&data=%7B%22comm%22%3A%7B%22ct%22%3A24%2C%22cv%22%3A0%7D%2C%22singerList%22%3A%7B%22module%22%3A%22Music.SingerListServer%22%2C%22method%22%3A%22get_singer_list%22%2C%22param%22%3A%7B%22area%22%3A-100%2C%22sex%22%3A-100%2C%22genre%22%3A-100%2C%22index%22%3A-100%2C%22sin%22%3A0%2C%22cur_page%22%3A1%7D%7D%7D'
    jsonp(url, (erro, res) => {
      const result = res
      this.dataq = result
    })
  }
}
</script>
```

### 反向代理
* 解决跨域问题，大概意思就是设置一个代理在对接口进行访问
* 通过修改nginx服务配置来实现，前端修改，后台不动
* vue设置反向代理

```javascript
// 创建vue.config.js文件
module.exports = {
  devServer: {
  	host: 'localhost',
  	port: 8080,
    proxy: {
      // 当访问/api的时候代理到target地址中
      '/api/': {
        // 设置代理地址  切记添加https://或http://
        target: 'https://api.douban.com',
        changeOrigin: true,
        pathRewrite: {
        // 当访问开头为api时，将/api设置为空在进行访问
          '/api': ''
        }
      }
    }
  }
}
// ajax请求数据
mounted () {
    var req = '/api/movie/top250?apikey=0b2bdeda43b5688921839c8ecb20399b'
    axios.get(req).then((value) => {
      this.datalist = value.data.subjects
      console.log(value)
    }).catch((reason) => {
      console.log(reason)
    })
  }
```

## 路由配置
### 一级路由
* router-view可以实现局部加载
* router文件中
```javascript
import Vue from 'vue'
import VueRouter from 'vue-router'
import File from '@/views/File'

Vue.use(VueRouter)

const routes = [{
  path: '/file',
  component: File
}]
export default router
```
* 主文件页面，访问对应的路径即可获取对应路由页面
```javascript
<template>
  <div>
  <router-view></router-view>
  </div>
</template>
```

### 二级路由
* 针对一级路由添加子对象
```javascript
import Vue from 'vue'
import VueRouter from 'vue-router'
import File from '@/views/File'

Vue.use(VueRouter)

const routes = [{
  path: '/file',
  component: File,
  children: [{
    path: 'fileone',
    component: Fileone
  }, {
    path: 'filetwo',
    component: Filetwo
  }, {
    // 默认路由
    path: '',
    redirect: 'fileone'
  }]
}]
export default router
// path后面加/和不加区别：添加/标识根路径，多次点击时不会叠加路径，不添加/，则使用的是相对路径，在路由嵌套时，当多次点击使用了子组件的路由时相对路径会叠加，从而使得项目因缺乏路径而崩溃
```

### 声明式导航
* router-link是vue提供的路由标签，tag可以指定自定义标签，activeClass当页面选中时添加的class属性，to是跳转到指定的路径，replace添加后页面切换时不会留下历史记录，exact开启router-link的严格模式

```html
<template>
  <div>
    <ul>
        <router-link to="/file" tag="li" activeClass='myactive'>file</router-link>
        <router-link to="/cinema" tag="li" activeClass='myactive'>cinema</router-link>
        <router-link to="/center" tag="li" activeClass='myactive'>center</router-link>
    </ul>
  </div>
</template>

<style lang='scss' scoped>
  .myactive {
    color: skyblue;
  }
</style>
```

### 路由重定向
* 当没有找到对应的路径时，重定向到默认路径

```javascript
import Vue from 'vue'
import VueRouter from 'vue-router'
import File from '@/views/File'

Vue.use(VueRouter)

const routes = [{
  path: '/file',
  component: File
}, {
// 路由重定向
  path: '*',
  redirect: '/file'
}]
export default router
```

### 动态路由
* 动态获取对应参数，从而返回指定的数据
* 路由设置

```javascript
 {
  // 动态路由
  path: '/detail/:id',
  component: Detail,
  name: 'det'
}
```

* 路由跳转

```javascript
<template>
  <div>
    <ul>
      <li v-for="data of datalist" :key="data" @click="handChange(data)">{{data}}</li>
    </ul>
  </div>
</template>

<script>
export default {
  data () {
    return {
      datalist: ['11', '22', '33']
    }
  },
  methods: {
    handChange (data) {
      // 编程式路由-路径跳转
      // this.$router.push('/detail/' + data)

      // 编程式路由-名字跳转
      this.$router.push({ name: 'det', params: { id: data } })
    }
  }
}
</script>
```

* 目标页面获取数据

```javascript
<template>
  <div>Detail</div>
</template>

<script>
export default {
  mounted () {
    console.log(this.$route.params.id)
  }
}
</script>
```

### 路由history
* 作用：取消路径的#
* 缺点：强制会向后端发送请求，需要后端自行配置

```javascript
const router = new VueRouter({
  mode: 'history'
})
```

### 路由守卫
* 作用：拦截不必要的跳转
#### 全局守卫
* 代码写入router文件下

```javascript
const flag = false
// 全局守卫
router.beforeEach((to, from, next) => {
  /**
   * to:即将要进入的目标 路由对象
   * from:当前导航正要离开的路由
   * next:向下之行
   */
  if (to.fullPath === '/center') {
    if (flag) {
      next()
    } else {
      next('/login')
    }
  } else {
    next()
  }
})
```

#### 局部守卫
* 代码写入单个组件内部

```javascript
const flag = true
export default {
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
    if (flag) {
      next()
    } else {
      next('/login')
    }
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```

#### 路由传参
* query传参：就是get传参

```javascript
this.$router.push({
          path: 'index',
          query: {
            from: 'login'
          }
        })
// 地址栏显示：http://localhost:8080/index?from=login
```

* params传参：就是post传参

```javascript
this.$router.push({
          name: 'index',
          params: {
            from: 'login'
          }
        })
// 地址栏显示：http://localhost:8080/index
```

## vue插件
### Better-scroll
* 作用：更好的进行滚动
* 安装

```shell
npm install better-scroll
```

* 使用

```javascript
//目录结构
<div class="wrapper">
  <ul class="content">
    <li>...</li>
    <li>...</li>
    ...
  </ul>
  <!-- 这里可以放一些其它的 DOM，但不会影响滚动 -->
</div>
//js
import BetterScroll from 'better-scroll'
new BetterScroll('.wrapper', {
//滚动条
  scrollbar: {
    fade: true,
    interactive: false
  }
})
```

###  vue-touch
* 作用：支持手势操作
* 安装

```shell
npm install vue-touch@next
```

* 使用

```javascript
<template>
  <div>
    <v-touch @swipeleft="onSwipeLeft">
      <div style="height:100px;width:100%;background:#bfc">123</div>
    </v-touch>
  </div>
</template>

<script>
import Vue from 'vue'
var VueTouch = require('vue-touch')
Vue.use(VueTouch, { name: 'v-touch' })
const flag = true
export default {
  methods: {
    onSwipeLeft () {
      console.log('left')
    }
  }
}
</script>
```

###  vue-lazyload
* 实现懒加载

### vue-awesome-swiper
* 实现轮播

### vue-axios
* 可以将axios挂载到vue实例中，可以直接this调用

### vue-cookie
* 前后端交互使用到cookie

###  sass

```shell
npm install node-sass sass-loader
```

### qrcode
* 链接转为二维码

```shell
npm install qrcode
```

### vue-infinite-scroll
* 滚动加载插件

```shell
npm install vue-infinite-scroll
```

## vuex
* 作用：状态管理，共享数据
* 功能：状态管理（非父子通信），数据快照（可以缓存数据），方便管理调试
* 使用

```javascript
import Vue from 'vue'
import Vuex from 'vuex'
import axios from 'axios'
Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    // 自定义的共享状态
    isTarbarShow: true,
    list: []
  },
  // 处理state中的数据
  getters: {
    listGetter (state) {
      return state.list.filter((item, index) => index < 3)
    }
  },
  // 修改状态的地方，只能在这里修改状态
  mutations: {
    TaberHide (state, data) {
      state.isTarbarShow = data
    }
  },
  // 异步处理，需要其他组件使用dispatch发送函数
  actions: {
    //自动传递state参数
    getTwoAction (state) {
      axios({
        url: 'https://m.maizuo.com/gateway?cityId=110100&pageNum=1&pageSize=10&type=2&k=3079586',
        headers: {
          'X-Client-Info': '{"a":"3000","ch":"1002","v":"5.0.4","e":"15890017383624952397998","bc":"110100"}',
          'X-Host': 'mall.film-ticket.film.list'
        }
      }).then((value) => {
        // 使用state调用commit来向mutations发送函数，以便更改状态
        state.commit('TwoList', value.data.data.films)
        console.log(value.data.data.films)
      })
    }
  },
  // 区分模块
  modules: {}
})

// 使用commit发送函数
beforeMount () {
    // 调用commit向mutations发送函数,第一参数为mutations的函数名
    this.$store.commit('TaberHide', false)
  }

// 使用dispatch发送函数
mounted () {								// 方法一
    if (this.$store.state.list.length === 0) {
      // 使用dispatch向actions发送函数
      this.$store.dispatch('getTwoAction')
    } else {
      console.log('缓存')
    }
  }

import { mapActions } from 'vuex'	// 方法二
methods: {
  login () {
      // 解构this获取值
      const { username, password } = this
      this.axios.post('/user/login', {
        username,
        password
      }).then(value => {
        this.$cookie.set('userId', value.id, { expires: '1M' })
        this.getUsername(value.username)
        this.$router.push('/index')
      }).catch(reason => {
        console.log(`reason: ${reason}`)
      })
    },
  ...mapActions(['getUsername'])
}

// 获取store数据
this.$store.state.list	// 方法一

import { mapState } from 'vuex'
computed: {				// 方法二
    ...mapState(['isTarbarShow'])// 直接使用isTarbarShow即可的到store中数据
  }
// 获取getters数据
this.$store.getters.listGetter	//方法一

import { mapGetters } from 'vuex'	//方法二
computed: {
    ...mapGetters(['listGetter'])// 直接使用listGetter即可的到getters中数据
  }
```

## 服务端渲染
* vue ssr：将vue在服务端渲染
* 使用方法，结合node服务的express框架

```javascript
const Vue = require('vue')
const server = require('express')()
const renderer = require('vue-server-renderer').createRenderer()

server.get('*', (req, res) => {
  const app = new Vue({
    data: {
      url: req.url
    },
    template: `<div>访问的 URL 是： {{ url }}</div>`
  })

  renderer.renderToString(app, (err, html) => {
    if (err) {
      res.status(500).end('Internal Server Error')
      return
    }
    res.writeHead(200,{'Content-type':'text/html;charset=utf8'})
    res.end(`
      <!DOCTYPE html>
      <html lang="en">
        <head><title>Hello</title></head>
        <body>${html}</body>
      </html>
    `)
  })
})

server.listen(8080)
```

## NuxtJS
* 作用：基于vue的服务端渲染框架
* 安装

```shell
npx create-nuxt-app <项目名>
yarn create nuxt-app <项目名>
# 启动
npm run dev
```

### 路由
#### 基础
* Nuxt.js依据pages目录结构自动生成vue-router模块的路由配置
* 在pager文件下按照文件的层及自动生成路由
* 要在页面之间使用路由，使用nuxt-link与router-link相似，支持actionClass

```javascript
<nuxt-link tag="li" to="/film" active-class="colorli">film</nuxt-link>
```

* 二级路由：需要添加一个Vue文件，同时添加一个与该文件同名的目录用来存放子视图组件，使用nuxt-child，与router-view相似

#### 重定向
##### 使用nuxt.config.js文件
```javascript
router: {
    extendRoutes (routes) {
      routes.push({
        path: '/',
        redirect: '/film/nowplaying'
      })
    }
  }
```

##### 使用中间件实现
* middleware文件下创建redirect.js文件
```javascript
export default function({isHMR,app,store,route,params,error,redirect}) {
  if (isHMR) return
  if (route.fullPath == '/') {
    return redirect('/film/nowplaying')
  }
}
```

* nuxt.config.js进行路由配置

```javascript
router: {
    middleware: 'redirect',
  }
```

#### 动态路由
* 如果想要在跳转路由detail中添加一个id值，那么需要创建detail文件并在内部创建`_id.vue`文件，即可实现含参路由跳转
* 参数发送与获取与原生vue相同

### 视图
* 在layout中，写好default.vue。可以认为是指定的组件模板，当然这是所有的组件的根模板，但是当有一个组件想要替换跟模板的话需要这样子做，原生vue可以通过vueX的状态进而改变组件的内容
```javascript
// 在需要替换模板的vue文件中写入
<script>
export default {
//detailtrmplate 对应的layouts文件下的detailtrmplate.vue文件
  layout: 'detailtrmplate'
}
</script>
```

### 异步获取数据
* 原生vue一般在mounted中获取异步数据，但是在nuxt中不可以这么做，因为一旦也在mounted中获取数据，意味着也是在浏览器端获取数据的，不利于SEO的优化，所以需要使用asyncData 方法
* asyncData：
>在服务器端调用asyncData时，您可以访问用户请求的req和res对象
>在当前页面刷新，服务端执行此数据
>从其他页面跳转过来，客户端执行此函数

* 使用方法：

```javascript
<script>
import axios from 'axios'
export default {
  data () {
    return {
      datalist: []
    }
  },
  methods: {
    handDetail (id) {
    // 传递参数，detaile页面使用asyncData方法调用data.params.id可以获取传递id值
      this.$router.push(`/detaile/${id}`)
    }
  },
  asyncData(data) {
    console.log(data)
    // 别忘记加return
    return axios({
      url: 'https://m.maizuo.com/gateway?cityId=440100&pageNum=1&pageSize=10&type=1&k=4896706',
      headers: {
        'X-Client-Info': '{"a":"3000","ch":"1002","v":"5.0.4","e":"15892725003856880631941","bc":"440100"}',
        'X-Host': 'mall.film-ticket.film.list'
      }
    }).then((value) => {
      // 别忘记加return
      return {
        // 将data中的datalist进行赋值
        datalist: value.data.data.films
      }
    })
  },
}
</script>
```

### 反向代理（重启服务器）
* 安装

```shell
npm install @nuxtjs/proxy
```

* 使用

```javascript
// 在nuxt.config.js文件中进行配置
modules: [
    '@nuxtjs/proxy'  //配置nuxtjs/proxy模块
  ],
  axios: {
    proxy: true
  },
  proxy: {
    '/v2/': {
      target: 'https://api.douban.com/',
      changeOrigin: true
    }
  },
```

* 要点注意：服务端渲染不需要进行跨域，可以通过process.server判断是否在服务端渲染Boolean值

```javascript
asyncData(data) {
    // process.server标识是否在服务端运行
    console.log(process.server)
    const url = process.server ? 'https://api.douban.com/v2/movie/top250?apikey=0b2bdeda43b5688921839c8ecb20399b&start=0&count=5' : '/v2/movie/top250?apikey=0b2bdeda43b5688921839c8ecb20399b&start=0&count=5'
    return axios.get(url).then((value) => {
      console.log(value.data)
    })
  }
```

## vue小技巧
* 大图片放入public中，小图片放入assetc中

### vue项目如何刷新当前页面
* 第一：最直接整个页面重新刷新

```javascript
location.reload()		---第一种
this.$router.go(0)		---第二种
// 缺点：这两种都可以刷新当前页面的，缺点就是相当于按ctrl+F5 强制刷新那种，整个页面重新加载，会出现一个瞬间的空白页面，体验不好
````

* 第二：新建一个空白页面supplierAllBack.vue，点击确定的时候先跳转到这个空白页，然后再立马跳转回来

```javascript
// 需要刷新的页面
methods:{
  click () {
    this.$router.replace({
      path: '/supplierAllBack.vue',
      name: 'supplierAllBack'
    })
  }
}
// 空白页面supplierAllBack.vue
data () {
  this.$router.replace({
    path: '需要刷新的页面'，
    name: '需要刷新的页面'
  })
}
// 这个方式，相比第一种不会出现一瞬间的空白页，只是地址栏有个快速的切换的过程，可采用
```

* 第三：provide / inject 组合，首先，要修改app.vue

```javascript
// app.vue
<template>
  <div id="app">
    <router-view v-if="isRouterCiew"/>
  </div>
</template>

<script>
export default {
  name: 'app',
  provide () {
    return {
      reload: this.reload
    }
  },
  data () {
    return {
      isRouterCiew: true
    }
  },
  methods: {
    reload () {
      this.isRouterCiew = false
      // DOM更新后后执行$nextTick
      this.$nextTick(() => {
        this.isRouterCiew = true
      })
    }
  }
}
</script>
// 其他组件调用
export default {
  inject: ['reload'],
  methods: {
    click () {
      this.reload()
    }
  }
}
```

## Mock方法
### 本地创建json

### easy-mock平台

### 集成Mock API

## 双向绑定原理
* 通过Object.defineProperty()，对data的每个属性进行get，set拦截
* 观察者模式：一对多模式，一代表修改某一个data数据，多代表凡是页面上都使用到这个数据的地方都更新


## Vue2与Vue3区别
* vue3的template支持多个根标签，vue2不支持
* vue3有createApp()。而vue是new Vue()
* createApp(组件)，new Vue({template, render})


















