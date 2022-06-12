## Vue面试题
### v-show与v-if的区别
* v-show通过css display控制显示和隐藏
* v-if组件真正的渲染和销毁，而不是显示和隐藏
* 频繁切换显示状态用v-show，否则使用v-if

### 为什么v-for中用key
* 必须使用key，且不能使用index和random
* diff算法中通过tag和key来判断，是否是`sameNode`
* 减少渲染次数，提升渲染性能

### computed有何特点
* 缓存，data不变不会重新计算
* 提高性能

### 如何将所有的props传递给子组件
* 使用$props
```javascript
<user v-bind='$props' />
```

### vue的生命周期
* beforeCreate：创建之前，初始化vue示例，data和methods中的数据都未初始化
* created：创建完成后 // 初始化实例，并没有开始渲染，data和methods初始化完成
* beforeMount：挂载之前，vue中的指令执行最终生成编译好的字符串模寄存在虚拟dom中，并没有真正生成到页面上
* mounted：挂载完了 // 页面绘制完成，一般在该函数下获取网络数据，将虚拟dom挂载到页面上
* beforeUpdate：更新之前
* updated：更新完了
* beforeDestroy：销毁之前 // 解除自定义事件event.$off，清除定时器，解除绑定的DOM事件（scroll等）
* destroyed：销毁完了
#### 父子组件间生命周期执行顺序
* 创建与挂载
>父组件：create
>子组件：create
>子组件：mounted
>父组件：mounted

* 更新前与更新
>父组件：beforeUpdate
>子组件：beforeUpdate
>子组件：updated
>父组件：updated

* 销毁前与销毁
>父组件：beforeDestroy
>子组件：beforeDestroy
>子组件：destroyed
>父组件：destoryed

### 如何理解MVVM
数据驱动视图： vue（MVVM），react（setState）
传统组件，只是静态渲染，更新还需要依赖操作DOM

### vue组件如何通讯
* 父到子：props
* 子到父：$emit
* 兄弟组件：bus总线，event.$on,event.$off,event.$emit
* vuex

### 为什么组件data必须是一个函数
* vue编译完成后实际上是一个class，当使用该组件时在进行实例化，实例化后data是唯一的
* 当使用组件时候，执行data，如果data不是一个函数的化，那么每一个组件的data都共享了

### 多个组件有相同的逻辑，如何抽离
* 使用mixin

### 何时使用异步组件
* 加载大组件
* 路由异步加载

### 何时使用keep-alive
* 缓存组件，不需要重复渲染
* 多个静态tab页面切换
* 优化性能

### 何时使用beforeDestroy
* 解绑自定义事件 event.$off
* 清除定时器
* 解绑自定义DOM事件，如window scroll

### 描述组件渲染的更新过程，模板编译
* 编写template模板，然后通过某种方式（比如：vue-template-compiler）生成render函数（with函数），然后执行render函数生成vnode，然后再通过patch函数（diff算法对比新旧dom）渲染到页面，可以使用webpack vue-loader会在开发环境下编译模板，如果不适用的化会在浏览器渲染时进行编译代码，影响性能。
* vue组件可以使用render代替template
* 初次渲染过程：
1、解析模板为render函数（或在开发环境已完成，vue-loader）
2、触发响应式，监听data属性getter setter
3、执行render函数，生成vnode，path(elem，vnode)
* 更新过程
1、修改data，触发setter（此前getter已被监听）
2、重新执行render函数，生成newVnode
3、path(vnode，newVnode)
* 异步渲染（$nextTick）：汇总data的修改，一次性更新视图，减少DOM操作次数，提高性能

### 双向绑定v-model的实现原理，响应式
* input元素的value = this.name
* 绑定input事件this.name = $event.target.value
* data更新触发render
* 核心API：Object.defineProperty（vue3.0启用Proxy）
* Object.defineProperty缺点1：对原始数据进行克隆，需要分别给对象中的每一个属性设置监听
* Object.defineProperty缺点2：深度监听，需要递归到底，以此因计算量大
* Object.defineProperty缺点3：无法监听新增与删除属性，所以有了Vue.set,Vue.delete

### vue如何监听数组变化
* Object.defineProperty不能监听数组的变化，因为Object.defineProperty是对象方法
* 重新定义原型，重写push，pop等方法，实现监听
* Proxy可以原生支持监听数组的变化

### vdom与diff算法，vdom
#### vdom
* vdom：用js模拟DOM结构，计算出最小的变更，操作DOM
* js模拟dom结构为VNode

```javascript
// 模板
<div id='div1' class="container">
  <p>vdom</p>
  <ul style="font-size: 20px;">
    <li>a</li>
  </ul>
</div>
// VNode描述
{
    tag: 'div',
    props： {
      id: 'div1',
      className: 'container'
    },
    children: [{
      tag: 'p',
      children: 'vdom'
    },{
      tag: 'ul',
      props: {
        style: 'font-size: 20px'
      },
      children: [{
        tag: 'li',
        children: 'a'
      }]
    }]
  }
```

#### diff算法
* 只比较同一层，不跨级比较
* tag不相同，则直接删掉重建，不再深度比较
* tag和key两者都相同，则认为是相同节点，不再深度比较

### vue路由
* hash路由：
>hash变化会触发页面跳转，即浏览器的前进，后退
>hash变化不会刷新页面，SPA（单页面）必需的特点
>hash永远不会提交到server端（前端自生自灭）
>一般用于toB的系统，简单易用，对url不敏感

```javascript
// 手写hash
// hash 变化，包括：
    // a. JS 修改 url
    // b. 手动修改 url 的 hash
    // c. 浏览器前进、后退
    window.onhashchange = (event) => {
      console.log(`old url:${event.oldURL}`)
      console.log(`new url:${event.newURL}`)
      console.log(`hash:${location.hash}`)
      console.log(event)
    }

    document,addEventListener('DOMContentLoaded', () => {
      console.log(`hash:${location.hash}`)
    })
    btn1.addEventListener('click', () => {
      location.href = '#/user'
    })
```

* history路由（需要后端的支持）
>用url规范的路由，但跳转时不刷新页面
>toC的系统可以考虑使用，但需要服务端的支持

```javascript
// 手写history
// 页面初次加载，获取path
  document.addEventListener('DOMContentLoaded', () => {
    console.log(`load:${location.pathname}`)
  })

  // 打开一个新的路由
  // 注意哦 用pushState方式，浏览器不会刷新页面
  btn1.addEventListener('click', () => {
    const state = { name: 'page1' }
    console.log('切换路由到page1')
    history.pushState(state, '', 'page1')
  })

  // 监听浏览器的前进 后退
  window.onpopstate = (event) => {
    console.log(event)
    console.log(`onpopstate`, event.state, location.pathname)
  }
```

### vue中Object.defineProperty 为什么不能检测数组的改变
* 新增数据由于属性名（索引）增加而无法被Object.defineProperty所检测到
* 在存储栈区的只有一个指向堆区的指针，数据的改变不会引起指向其指针的变化

### vue常见的性能优化
* 合理使用v-show和v-if
* 合理使用computed
* v-for中添加key，避免与v-if的同时使用
* 自定义事件，DOM事件及时销毁
* 合理使用异步组件
* 合理使用keep-alive
* data层级尽量不要太深
* 使用vue-loader在开发环境做模板编译（预编译）
* 使用ssr
* 懒加载等等

### 路由权限
1. uid -> 后端API -> 路由权限API
2. 后端 -> 用户对应路由权限列表 -> 前端 -> JSON
3. JSON -> 树形结构化
4. 树形结构化的数据 -> vue路由结构
5. 路由结构动态 -> 静态路由
6. 树形结构化的数据 -> 菜单组件

### vue中scoped的原理
* 当使用scoped后：
1. 给DOM节点加一个不重复属性 data-v-5db9451a 标志唯一性
2. 使每个样式选择器后添加类似于"不重复属性"的字段, 类似于作用域的作用,不影响全局
3. 如果组件内部还有组件,只会给最外层的组件里的标签加上唯一属性字段,不影响组件内部引用的组件
4. 父组件无scoped属性,子组件带有scoped,父组件是无法操作子组件的
5. 父组件有scoped属性,子组件无scoped.父组件也无法设置子组件样式.因为父组件的所有标签都会带有data-v-5db9451a唯一标志，但子组件不会带有这个唯一标志属性
6. 父子组建都有，同理也无法设置样式，更改起来增加代码量

### vue3升级内容
* 全部使用ts重写（响应式，vdom，模板编译）
* 性能提升，代码量减少
* 会调整部分API
* Proxy实现响应式，深度监听，性能更好，可监听新增/删除属性，可监听数组变化

### vue3快体现在哪些
* 引入tree-shaking打包，通过编译阶段的静态分析，找到没有引入的模块并打上标记。利用 tree-shaking 技术，如果你在项目中没有引入无关的组件，那么它们对应的代码就不会打包，这样也就间接达到了减少项目引入的 Vue.js 包体积的目的
* 数据劫持优化，vue2使用 Object.defineProperty 数据劫持优化，vue3使用Proxy，
  >1.  Object.defineProperty 缺点第一点：需要知道拦截的key是什么，所以不能检测对象属性的添加删除，当前vue提供了 $set $delete, 第二点：拦截层级比较深，由于vue并不能判断你运行时会用到那个属性，所以对于层级较深的对象，如果要劫持深层次对象变换，那么就需要遍历整个对象，然后就会把每一层对象都变成响应式的，毫无疑问就会造成相当大的性能负担
  >2. 优化defineProperty第一点：劫持整个对象，那么自然可以对于对象的属性增加，删除进行检测到，第二点：Proxy也不能监听到内部深层次的对象变化，但是Proxy可以真正访问到内部对象才会变成响应式，而不是无脑递归，从而提高性能

* 编译优化：vue2更新粒度为 组件级别，vue3更新粒度：动态内容的数量相关
* vnode更新性能：vue2更新性能跟模版大小正相关，vue3更新性能与动态内容的数量相

### vue3中监听url中hash值的变化

```javascript
setup(props, ctx: SetupContext) {
    // 监听hash变化刷新页面，更换头部导航颜色
    const hashChange = () => {
      window.addEventListener(
        'hashchange',
        () => {
          console.log('hash变化');
        },
        false
      );
    };
hashChange();
```

### vue-router的跳转与location.href有什么不同？
1. vue-router使用pushState进行路由跳转，页面不会刷新；window.location.href会更新一次页面
2. vue-router使用diff算法，按需加载，减少了dom操作
3. vue-router为路由跳转或同一个页面跳转，window.location会跳转到其他页面
4. vue-router异步加载$nexttick(()=>{获取url})，window.location同步加载
