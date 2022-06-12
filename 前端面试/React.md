## React面试题
### react的生命周期
* constructor()：初始化阶段
* static getDerivedStateFromProps(nextProps, prevState)：从props获取state，该函数必须有返回值，当出现变化时可以返回一个对象来更新state，如果返回null则不更新state
* shouldComponentUpdate(nextProps, nextState)：判断组件是否受state或props更改的影响，默认行为是state每次发生变化组件都会重新渲染，一般用来做优化，可以用PureComponent替代
* render()
* getSnapshotBeforeUpdate()：在最近一次渲染输出（提交到 DOM 节点）之前调用，它使得组件能在发生更改之前从 DOM 中捕获一些信息（例如，滚动位置）。此生命周期的任何返回值将作为参数传递给 componentDidUpdate()
* componentDidMount()：UI渲染完成，组件挂载后（插入 DOM 树中）立即调用，获取网络数据，实例化请求
* componentDidUpdate(prevProps, prevState, snapshot)：更新后会被立即调用。首次渲染不会执行此方法
* componentWillUnmount()：组件卸载及销毁之前直接调用

### react组件如何通信
* 父 -> 子组件使用props
* 子 -> 父组件自定义事件
* Redux和Context（顶层组件向下传递）

### jsx本质是什么
* jsx可以理解为vue的template模板，但不是模板，本身是js的语法糖
* jsx会通过react.createElement即h函数，返回vnode，第一个参数可能是组件也可能是html tag
* vnode通过path(elem，vnode)，path(vnode， newVnode)渲染和更新到页面中

### context是什么，有和用途
* 可以获取父级传递的内容，从而不用props一层一层向下传递

```javascript
// 父组件
import React, { Fragment } from 'react';
import ProtalsDemo from './ProtalsDemo';

// React.createContext：创建一个上下文的容器(组件)
export const { Provider,Consumer } = React.createContext() 
const BaseUser = () => {
  return (
  	{value：定义传递内容}
    <Provider value='上下文'>
        <ProtalsDemo children='Protals' />
    </Provider>
  )
}
export default BaseUser

// 子组件
import React from 'react'
import ReactDOM from 'react-dom'
// 调用父级的Consumer进行使用父级传递的值
import { Consumer } from './index'

class ProtalsDemo extends React.Component {
    constructor(props) {
        super(props)
    }
    render() {
        return (
          <div className="modal">
            <Consumer>
              {value => <p>{value}</p>}
            </Consumer>
          </div>
        ) 
    }
}
export default ProtalsDemo
```

### shouldComponentUpdate（scu）的用途
* React默认父组件有更新，子组件无条件更新
* SCU不一定每次都要使用
* 必须配合不可变值一起使用

### 描述redux单项数据流
* 组件通过dispatch发送action给store，store调用传入reducer中，reducer进行数据的更新然后返回给store一个新的store，然后store保存更新数据，view调用store数据

### 什么是纯函数
* 返回一个新值，没有副作用，输入数组返回数组，输入对象返回对象
* 不可变值

### 渲染列表，为何使用key
* 与vue类似，必须使用key且不能是index和random
* diff算法中通过tag和key来判断，是否是`sameNode`
* 减少渲染次数，提升渲染性能

### react中setState为何使用不可变值
* 之所以使用不可变值是因为在shouldMountUpdate生命周期中会将改变的值与之前的值做对比来确定是否改变视图，以这种方式优化性能。

### setState是同步还是异步
* 不可变值
* 可能是异步更新，正常使用是异步更新，但是在定时器和自定义的DOM事件上是同步更新
* 可能会合并，当连续的赋值对象时候，会进行合并，当连续的函数调用不会合并
* setState其实无所谓异步还是同步的，主要是看isBatchingUpdates是否为true，如果为true则是异步，如果为false则为同步，isBatchingUpdates在setState执行前设置为true，执行后设置为false

### React的合成事件机制
* 所有事件挂载到document上
* event不是原生的，是SyntheticEvent合成事件对象
* 和vue事件不同，和DOM事件也不同
* 为什么要合成事件机制：更好的兼容行和跨平台；挂载document，减少内存消耗，避免频繁解绑；方便事件的统一管理（比如事务机制）

### batchUpdate机制
* 如果命中batchUpdate机制的化为异步，反之为同步，react再执行函数时提前会默认设置一个 isBatchingUpdates = true，然后开始执行目标函数，函数执行完成后设置 isBatchingUpdates = false。也就是说当存在异步时比如setTimeout，先将isBatchingUpdates = true，然后执行函数中的setTimeout放入宏队列，然后将isBatchingUpdates = false后才会执行宏队列中的setTimeout，所以setTimeout不能命中batchUpdate机制

#### 那些能够命中batchUpdate机制
* 生命周期（以及调用的函数）
* React中注册的事件
* React可以管理的入口

#### 那些不能够命中batchUpdate机制
* setTimeout，setInterval等
* 自定义DOM事件
* React管不到的入口

### React组件渲染和更新过程
#### 渲染
* 生成数据结构（props，state）
* render()生成vnode
* path(elem, vnode)

#### 更新
* setState(newState) ---dirtyComponents（可能有子组件）
* render() 生成newVnode
* path(vnode，newVnode)

### react和vue的区别
####  共同点
* 都支持组件化
* 都是数据驱动视图
* 都使用vdom操作DOM
#### 不同点
* React使用JSX拥抱js，Vue使用模板拥抱html
* React函数式编程，Vue声明式编程
* React更多的需要自力更生，Vue把想要的都给开发者（eg：循环react使用map，vue使用v-for）

### React Hooks与生命周期的关系
* Hooks组件，即使用了Hooks的函数组件，有生命周期
|class组件|Hooks组件|
|-|-|
|constructor|useState|
|getDerivedStateFromProps|useState里面update函数|
|shouldComponentUpdate|useMemo|
|render|函数本身|
|componentDidMount|useEffect，useEffect(fn, [])|
|componentDidUpdate|useEffect|
|componentWillUnmount|useEffect|

### useMemo和useCallback的区别 及使用场景
* 相同点：useMemo和useCallback接收参数都一样的，第一个参数为回调，第二个参数为要依赖的数据，当依赖数据发生变化后才会重新计算结果，也就是起到缓存的作用
* 不同点：useMemo计算结果return回来的值，主要用于缓存计算结果的值，应用场景：计算的状态；useCallback主要用于缓存函数，应用场景如: 需要缓存的函数，因为函数式组件每次任何一个 state 的变化 整个组件 都会被重新刷新，一些函数是没有必要被重新刷新的，此时就应该缓存起来，提高性能，和减少资源浪费

### React中`<Link>`与a标签有什么区别
* link应用范围更广，不会刷新页面，
* a：刷新页面

### DIff算法
#### snabbdom-diff算法整理
* 第一步：h函数：输入‘标签’，‘data’，‘子元素’，输出VNode函数，通过VNode函数产生vdom（js对象）
* 第二步：init函数返回path函数。path函数可能有三种情况：

```javascript
patch(Element, VNode)
patch(VNode, NewVNode)
patch(NewVNode, null)
```
patch函数判断第一个参数不是VNode，创建一个空的VNode关联到DOM元素中（也就是挂载/更新到该dom元素上），如果两个参数相同的VNode（通过sameVnode方法（key，tag）进行比较，一般非循环是不设置key的，也就是undefined === undefined）执行patchVnode函数，如果不相同直接删掉重建（也就是sel（tag/标签）不同）

```javascript
function sameVnode (vnode1: VNode, vnode2: VNode): boolean {
  return vnode1.key === vnode2.key && vnode1.sel === vnode2.sel
}
```

* 第三步：patchVnode函数：
>先执行prepatch Hook（类似于钩子函数）
>设置elem，将旧的elem设置为新的elem，否则新的vnode不知道挂载到哪里
>获取旧的children和新的children，如果新旧vnode相同则直接return
>判断vnode.text === undefined，也就是vnode.children !== undefined，一般来说标签内有text就不会有children，如果成立，证明新节点有children
>>判断两者都有children，若成立执行**updateChildren**函数
>>如果新节点有children，旧节点无children，那么旧节点可能有text，若有text，调用`setTextContent`清空text，调用`addVnodes`函数添加children
>>如果旧节点有children，新节点无children，那么调用`removeVnodes`函数旧children
>>如果旧节点text有，新节点text无，那么调用`setTextContent`函数清空text

>判断vnode.text !== undefined，也就是vnode.children === undefined 且 oldVnode.text !== vnode.text成立，若旧节点存在children，调取`removeVnodes`函数移除旧的children，调取`setTextContent`函数添加text

* 第四步：updateChildren函数
>获取oldStartIdx，oldEndIdx，newStartIdx，newEndIdx，（旧children的首位和末尾，新children的首位和末尾）
>进行循环操作，当首尾重叠停止循环
>判断新旧节点的 开始和开始，结束和结束，开始和结束，结束和开始，四个条件如果命中任意一个则执行**patchVnode**函数（通过key和sel进行判断）
>如果以上都未命中，则拿新节点的key能否对应上旧children中的某个节点的key	
>>若没有对应上，证明旧children不存在该节点，则将该节点插入即可
>>若有对应上，获取对应key的节点，判断sel（tag/标签）是否相同
>>>若sel不相同，将该节点插入即可
>>>若sel相同，key相同，指向**patchVnode**函数

* 第五步：虚拟dom生成真实dom，当以上函数都调取完成后，最后返回到patch函数中，patch函数拿到了更新后的虚拟dom
>如果旧节点为空，直接拿到elem，若不为空，获取旧节点的父级节点
>判断tag是否为string，如果是string，证明是一个标签，创建这个标签，判断data属性，循环data，若key为style，再此循环style将style[key] = style[value] 挂载到elem上，若key是class则使用elem.className = class，否则使用setAttribute(key, value) 对tag的属性赋值，判断children，遍历children，创建children递归当前函数不断往下获取子元素，直到获取完成
>如果tag不是string，则是文本节点，创建这个文本
>最后使用insertBefore(新节点，旧节点的下一个节点)（意思就是将新节点插入到旧节点的下一个节点之前，为什么，因为旧节点后可能为script标签，尽可能将新节点插入到script标签之前，浏览器执行script标签会中断所以要插入到script之前），然后删除旧节点，完成虚拟dom到真实dom的转换。

### vue和react的diff算法区别
* vue对比节点，当节点元素类型相同，但是className不同，认为是不同类型的元素，删除重建，而react会认为是同类型节点，只是修改节点属性
* vue的列表对比，采用从两端到中间的对比方式，react采用从左到右依次比较的方式，当一个集合，只是把最后一个节点移动到第一个，react会把前面的节点依次移动，vue只会把最后一个节点移动到第一个。




