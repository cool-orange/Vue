## vue

学习笔记和代码练习

### 入门练习

* todolist [练习](https://dazedsanjin.github.io/vue/todolist/dist/index.html)  

### Vue常规知识
* 通讯  
父子组件通讯props和$emit
组件间通讯-自定义事件 定义自定义事件文件export default new Vue() 发送的地方event.$emit() 接受的地方event.$son
* v-show和v-if的区别  
v-if若为真，则渲染dom元素；为false，则销毁dom元素。v-show相当于display：none与display：block的区别。若经常需要切换组件状态，则选用v-show，性能上更优。
* v-for中key的作用  
key的重要性。(不可乱写成index或者random()) 
* v-html有哪些隐患  
```
1. 具备 xss风险
2. 覆盖子组件
```
* computed和watch的区别  
computed有缓存，data不变则不会重新计算，因为缓存计算更快。watch针对引用类型如对象，拿不到旧值。
* vue事件特点  
```
1. event是原生的
2. 事件是被挂载到当前元素
```
* vue组件的生命周期
```
1. beforeCreate
2. created 还没开始渲染，vue实例初始化完成
3. beforeMounted
4. mounted 页面已经渲染完毕
5. beforeUpdate
6. updated
7. beforeDestroy 可用来解绑自定义事件和一些定时器，防止内存泄露
8. destroyd

生命周期（父子组件）
挂载周期：初始化实例的时候，由父组件到子组件（created）, 渲染Dom由子组件到父组件（mounted）
更新周期：先由父组件到子组件（beforeUpdate），再由子组件到父组件（updated）
销毁周期：先由父组件到子组件（beforeDestroy）,再由子组件到父组件（destroyed）
```
* $nextTick由什么特性  
vue是异步渲染的过程，$nextTick会在Dom的渲染之后被触发回调，以获取最新的Dom节点的相关信息，同时页面渲染时会将data的修改做整合，多次data修改只会触发一次
* slot的用法
父组件向子组件插入一些内容  
```
1. 基本使用  
```
/* 父组件 */
<template>
  <SlotDemo :url="website.url">
    {{website.title}}
  </SlotDemo>
</template>
/* 子组件 */
<template>
  <a :href="url">
    <slot>默认内容显示</slot>
  </a> 
</template>
父组件的website.title将显示在子组件的a标签中
```
2. 作用域插槽  
父组件访问子组件的数据
3. 具名插槽
有时需要多个插槽的情况，如下所示
```
//父组件
<template>
  <base-layout>
    <template v-slot:header>
      <h1>here</h1>
    </template>
    <template v-slot:footer>
      <h2>there</h2>
    </template> 
  </base-layout>
</template>
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```
* 动态组件  
is特性，动态加载不同的组件
* 异步组件  
```
1. import 函数
2. 按需加载，异步加载大组件
```
* keep-alive函数
```md
缓存组件，频繁切换，不需要重复渲染。
原理
1. 获取keep-alive包裹着的第一个子组件对象及组件名
2. 如果name不在白名单（include）中或者在黑名单（exclude）中，直接返回vNode
3. 根据组件的id和tag值生成缓存的key值，并在缓存对象查找是否存在，存在则直接取出缓存值并更新当前key在this.keys中的位置（更新key主要是实现LRU置换策略），用于删除最近最久未使用的实例（缓存大小有限制）。不存在则在缓存中存储该组件实例并保存key值，之后检验缓存实例数量有无超过缓存最大值max。
4. 最后将该组件keep-alive标志位设为true，表示渲染成真实的dom元素
```
* vue组件如何抽离公共逻辑
```
mixin将多个组件相同逻辑部分，抽离出来
弊端：
1. 变量来源不明确，不利于阅读
2. 存在覆盖问题，造成命名冲突
3. 出现多对多的关系，复杂度升高
```

### vue原理知识

#### 组件化
* 数据驱动
封装了数据和dom对象的映射，只需关心数据的逻辑处理，不用取频繁地操作dom元素
* MVC和MVVM的区别   
```
Mvc是Model-View-Controller的简写，即模型-视图-控制器。M负责数据的管理，V即页面，C即同步M的数据和页面展示的内容。  
Mvvm是Model-View-ViewModel的简写、即模型-视图-视图模型。
VM的出现因为现有业务功能变得复杂，然而数据解析放在Controller层，会显得臃肿。而且C的职责是管理自己生命周期，处理一些业务跳转，实现Controller容器等并不包含数据解析。
因此，出现了VM。VM抽离了C中展示业务数据解析展示的一部分，如页面大量Dom操作渲染导致性能降低，和Model数据频繁变换导致开发者需主动更新数据到View。
```

####  vue响应式
* Object.defineProperty  
将data中声明的属性使用Object.defineProperty转化为getter和setter进行监听（watcher），
因此，每个组件都有watcher实例，在组件渲染的过程中把接触过的数据记录为依赖，之后当依赖项setter触发，通知watcher，从而使关联组件重新渲染。
```js
const data = {}
const name = 'orange'
Object.defineProperty(data, 'name', {
  get:function() {
    return name
  },
  set:function(newVal) {
    console.log('set')
    name = newVal
  }
})
console.log(data.name)
data.name = 'apple'

//缺陷如下
//1. 监听对象和数组需要深度监听，需要递归到底，一次性计算量大
//2. 无法监听新增属性和删除属性
//3. 不具备监听数组的能力，需要特殊处理
```
* v-model（数据的双向绑定）实现原理
```
// 父组件封装了CustomVModel组件
<p>{{ name }}</p>
<CustomVModel v-model="name">
// 子组件如下
<template>
    <!-- 例如：vue 颜色选择 -->
    <input type="text"
        :value="text1"
        @input="$emit('change1', $event.target.value)"
    >
    <!--
        1. 上面的 input 使用了 :value 而不是 v-model
        2. 上面的 change1 和 model.event1 要对应起来
        3. text1 属性对应起来
    -->
</template>

<script>
export default {
    model: {
        prop: 'text1', // 对应 props text1
        event: 'change1'
    },
    props: {
        text1: String,
        default() {
            return ''
        }
    }
}
</script>
```
#### 虚拟Dom和diff  
* 虚拟Dom
```
虚拟Dom即用JS模拟DOM结构，计算出最小的变更，操作虚拟DOM，计算变更主要是运用diff算法，模拟的是snabbdom算法
虚拟Dom结构如下所示
let VNode = {
  tag: 'div',
  props: {id: 'orange', style: 'background-color: red'}
  children: [
    {tag: 'p', text: '123'}
  ]
}
```
* diff算法
```md
diff算法即对比，不考虑深度遍历（On3），时间复杂度过高，优化如下（On）
1. 只比较同一层级，不跨级比较
2. tag不相同，则直接删掉重建，不再深度比较
3. tag和key，两者都相同，则认为是相同节点，不再深度比较  

步骤如下
1. 首先进行patch(oldVnode, Vnode)对比，tag和key不相同，则进行删除重建，相同进行patchVnode(oldVNode, vNode)
2. 找到真实的dom进行绑定，判断新旧Vnode是否指向同一个Vnode，相同则直接return
3. 若文本节点不相等，替换为Vnode的文本节点
4. 若oldVnode有子节点，Vnode没有，删除el的子节点
5. 若oldVnode没有子节点，Vnode有，那么将Vnode的子节点真实化添加到父元素中
6. 若都有子节点，且不相同，则进行updateChildren(el, oldCh, ch)
7. updateChildren会将新旧children添加oldS、oldE、S、E四个指针，若匹配上（开始与开始，开始与结束，结束与开始，结束与结束），则将OldVnode按Vnode的位置移动，若均未匹配，则拿新的Vnode的S指针与剩余的旧的OldVnode进行对比，匹配上则移动到最前面，匹配不上，则插入Vnode在oldVnode S的位置，最后还需移动新的指针.
8. 循环结束，若oldVnode指针未重合过则移除剩余的节点，若Vnode没有重合过，按照索引位置将Vnode剩余节点插入到OldVnode中取
```
#### 组件是如何渲染和更新的
* 模板编译  
首先模板不是html，要转化成js代码，首先利用parse函数转换未AST语法树，再转换为render函数的字符串，返回vnode，基于vnode再执行patch和diff函数。
* 渲染过程  
```md
//初次渲染过程
1. 解析模板为render函数（或在开发环境已完成，vue-loader）
2. 触发响应式，监听data属性getter setter
3. 执行render函数，生成vnode，patch（elem，vnode）生成真实dom元素

//更新过程
1. 修改data，触发setter
2. 重新执行render函数，生成newVnode
3. patch(vndoe, newVnode)

//异步渲染
```
#### vue-router（hash）  
不会刷新页面，SPA必需的特点，和服务端没有关联
利用window.onhashchange来监听
#### vue-router（history）  
用url规范的路由，但跳转时不刷新页面
用history.pushState、history.replaceState来实现和window.onpopstate，需要后端支持，因为没有'#',
当用户刷新页面之类的操作，依旧会给服务器发送请求，所以需要服务器将所有的路由都重定向到根页面。
#### vuex
vuex有几种状态  
state、mutation、getter、action、module