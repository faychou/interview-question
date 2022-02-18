# Vue 面试题
### 单页应用（SPA）的原理与优缺点？
缺点：

* 不利于 SEO 的优化
* 第一次加载首页耗时相对长一些；
* 不可以使用浏览器的导航按钮需要自行实现前进、后退。



### MVVM 框架设计理念，与 MVC 的区别？
`MVC` 是 `Model-View- Controller` 的简写。即模型(`model`) - 视图(`view`) - 控制器(`controller`)。`MVC` 是单向通信。也就是 `view` 跟 `model`，必须通过 `controller` 来承上启下。

* **Model（模型）**：是应用程序中用于处理应用程序数据逻辑的部分。通常模型对象负责在数据库中存取数据

* **View（视图）**：是应用程序中处理数据显示的部分。通常视图是依据模型数据创建的

* **Controller（控制器）**：是应用程序中处理用户交互的部分。通常控制器负责从视图读取数据，控制用户输入，并向模型发送数据。

`MVVM` 是 `Model-View-ViewModel` 的简写，即模型 - 视图 - 视图模型。在 MVVM 的框架下视图和模型是不能直接通信的。它们通过 `ViewModel` 来通信，`ViewModel` 通常要实现一个 `observer` 观察者，当数据发生变化，`ViewModel` 能够监听到数据的这种变化，然后通知到对应的视图做自动更新，而当用户操作视图，ViewModel 也能监听到视图的变化，然后通知数据做改动，这实际上就实现了数据的双向绑定。

* **`ViewModel`**：是 `MVVM` 模式的核心，它是连接 `view` 和 `model` 的桥梁。它有两个方向：一是将【模型】转化成【视图】，即将数据转化成所看到的页面。实现的方式是：数据绑定。二是将【视图】转化成【模型】，即将所看到的页面转化成数据。实现的方式是：DOM 事件监听。

`MVC` 和 `MVVM` 的区别并不是 `VM` 完全取代了 `C`，`ViewModel` 存在目的在于抽离 `controller` 中展示的业务逻辑，而不是替代 `controller`，其它视图操作业务等还是应该放在 `controller` 中实现。也就是说 `MVVM` 实现的是业务逻辑组件的重用。

整体看来，MVVM 比 MVC 精简很多，不仅简化了业务与界面的依赖，还解决了数据频繁更新的问题，不用再用选择器操作 DOM 元素。因为在 MVVM 中，View 不知道 Model 的存在，Model 和 ViewModel 也观察不到 View，这种低耦合模式提高代码的可重用性。



### vue.js 的核心是什么？



### 为什么 data 是一个函数？

组件中的 data 写成一个函数，数据以函数返回值形式定义，这样每复用一次组件，就会返回一份新的 data，类似于给每个组件实例创建一个私有的数据空间，让各个组件实例维护各自的数据。而单纯的写成对象形式，就使得所有组件实例共用了一份 data，就会造成一个变了全都会变的结果。



#### Vue 的初始化，发生了什么？



#### Vue 模版编译原理知道吗，能简单说一下吗？

简单说，Vue 的编译过程就是将 `template` 转化为 `render` 函数的过程。

首先解析模版，生成 `AST语法树`(一种用 JavaScript 对象的形式来描述整个模板)。 使用大量的正则表达式对模板进行解析，遇到标签、文本的时候都会执行对应的钩子进行相关处理。

Vue 的数据是响应式的，但其实模板中并不是所有的数据都是响应式的。有一些数据首次渲染后就不会再变化，对应的 `DOM` 也不会变化。那么优化过程就是深度遍历 `AST树`，按照相关条件对树节点进行标记。这些被标记的节点(静态节点)我们就可以跳过对它们的比对，对运行时的模板起到很大的优化作用。

编译的最后一步是将优化后的 `AST树` 转换为可执行的代码。

#### Vue2.x 数据更新策略

Vue在初始化数据时，会使用 `Object.defineProperty` 重新定义 `data` 中的所有属性，当页面使用对应属性时，首先会进行依赖收集(收集当前组件的 `watcher` )，如果属性发生变化会通知相关依赖进行更新操作(发布订阅)。

#### **vue2.x中如何监测数组变化**

使用了函数劫持的方式，重写了数组的方法，Vue 将 `data` 中的数组进行了原型链重写，指向了自己定义的数组原型方法。这样当调用数组 `api` 时，可以通知依赖更新。如果数组中包含着引用类型，会对数组中的引用类型再次递归遍历进行监控。这样就实现了监测数组变化。

#### 你知道 Vue3.x 响应式数据原理吗？

Vue3.x 改用 `Proxy` 替代 `Object.defineProperty`。因为 `Proxy` 可以直接监听对象和数组的变化，并且有多达13种拦截方法。并且作为新标准将受到浏览器厂商重点持续的性能优化。

#### 说一下 Vue 的生命周期

`beforeCreate` 是 `new Vue()` 之后触发的第一个钩子，在当前阶段 `data`、`methods`、`computed` 以及 `watch` 上的数据和方法都不能被访问。

`created` 在实例创建完成后发生，当前阶段已经完成了数据观测，也就是可以使用数据，更改数据，在这里更改数据不会触发 `updated` 函数。可以做一些初始数据的获取，在当前阶段无法与 `Dom` 进行交互，如果非要想，可以通过 `vm.$nextTick` 来访问 `Dom`。

`beforeMount` 发生在挂载之前，在这之前 `template` 模板已导入渲染函数编译。而当前阶段虚拟 `Dom` 已经创建完成，即将开始渲染。在此时也可以对数据进行更改，不会触发 `updated`。

`mounted` 在挂载完成后发生，在当前阶段，真实的 `Dom` 挂载完毕，数据完成双向绑定，可以访问到 `Dom` 节点，使用 `$refs` 属性对 `Dom` 进行操作，一般会在这个阶段请求数据。

`beforeUpdate` 发生在更新之前，也就是响应式数据发生更新，虚拟 `Dom` 重新渲染之前被触发，你可以在当前阶段进行更改数据，不会造成重渲染。

`updated` 发生在更新完成之后，当前阶段组件 `Dom` 已完成更新。要注意的是避免在此期间更改数据，因为这可能会导致无限循环的更新。

`beforeDestroy` 发生在实例销毁之前，在当前阶段实例完全可以被使用，我们可以在这时进行善后收尾工作，比如清除计时器。

`destroyed` 发生在实例销毁之后，这个时候只剩下了 `Dom` 空壳。组件已被拆解，数据绑定被卸除，监听被移出，子实例也统统被销毁。

除了 `beforeCreate` 和 `created` 钩子之外，其他钩子均在服务器端渲染期间不被调用。

#### Vue中组件生命周期调用顺序说一下。

加载渲染过程：`父 beforeCreate -> 父 created -> 父 beforeMount -> 子 beforeCreate -> 子 created -> 子 beforeMount -> 子 mounted -> 父 mounted`。

子组件更新过程：`父 beforeUpdate -> 子 beforeUpdate -> 子 updated -> 父 updated`。

父组件更新过程：`父 beforeUpdate -> 父 updated`。

销毁过程：`父 beforeDestroy -> 子 beforeDestroy -> 子 destroyed -> 父 destroyed`。

#### 说一下 Computed 和 Watch

`Computed` 本质是一个具备缓存的 `watcher`，依赖的属性发生变化就会更新视图。 适用于计算比较消耗性能的计算场景。当表达式过于复杂时，在模板中放入过多逻辑会让模板难以维护，可以将复杂的逻辑放入计算属性中处理。

`Watch` 没有缓存性，更多的是观察的作用，可以监听某些数据执行回调。当我们需要深度监听对象中的属性时，可以打开 `deep：true` 选项，这样便会对对象中的每一项进行监听。这样会带来性能问题，优化的话可以使用 `字符串形式` 监听，如果没有写到组件中，不要忘记使用 `unWatch手动注销`。

#### 说一下 `v-if` 和 `v-show` 的区别

当条件不成立时，`v-if` 不会渲染 `DOM` 元素，`v-show` 操作的是样式 (`display`)，切换当前 `DOM` 的显示和隐藏。

#### 组件中的 `data` 为什么是一个函数？

一个组件被复用多次的话，也就会创建多个实例。本质上，这些实例用的都是同一个构造函数。如果 `data` 是对象的话，对象属于引用类型，会影响到所有的实例。所以为了保证组件不同的实例之间 `data` 不冲突，`data` 必须是一个函数。

#### 说一下 `v-model` 的原理

`v-model` 本质就是一个语法糖，可以看成是 `value + input` 方法的语法糖。 可以通过 `model` 属性的 `prop` 和 `event` 属性来进行自定义。原生的 `v-model`，会根据标签的不同生成不同的事件和属性。

#### Vue 事件绑定原理说一下

原生事件绑定是通过 `addEventListener` 绑定给真实元素的，组件事件绑定是通过 Vue 自定义的 `$on` 实现的。

#### `nextTick` 知道吗，实现原理是什么？

在下次 `DOM` 更新循环结束之后执行延迟回调。`nextTick` 主要使用了宏任务和微任务。根据执行环境分别尝试采用

- Promise
- MutationObserver
- setImmediate
- 如果以上都不行则采用 `setTimeout`

定义了一个异步方法，多次调用 `nextTick` 会将方法存入队列中，通过这个异步方法清空当前队列。

#### Vue 组件通信有哪些方式？

- 父子组件通信
  - 父 -> 子 `props`
  - 子 -> 父  `$on、$emit`
  - 获取父子组件实例 `$parent`、`$children`
  - ``Ref` 获取实例的方式调用组件的属性或者方法
  - `Provide`、`inject` 官方不推荐使用，但是写组件库时很常用
- 兄弟组件通信
  - Event Bus：`Vue.prototype.$bus = new Vue`
- 跨组件通信
  - Vuex

#### actions 和 mutations 有什么区别之类的。



#### 说一下虚拟 DOM

由于在浏览器中操作 DOM 是很昂贵的。频繁的操作 DOM，会产生一定的性能问题。这就是虚拟 DOM 的产生原因。

Virtual DOM 本质就是用一个原生的 JS 对象去描述一个 DOM 节点。是对真实 DOM 的一层抽象。

VirtualDOM 映射到真实 DOM 要经历 VNode 的 create、diff、patch 等阶段。

#### key 属性的作用

key 的作用是为了在 diff 算法执行时更快的找到对应的节点，提高 diff 速度。vue 和 react 都是采用 diff 算法来对比新旧虚拟节点，从而更新节点。

在 vue 的 diff 函数中，在交叉对比的时候，当新节点跟旧节点头尾交叉对比没有结果的时候，会根据新节点的 key 去对比旧节点数组中的 key，从而找到相应旧节点（这里对应的是一个 key => index 的 map 映射）。如果没找到就认为是一个新增节点。而如果没有 key，那么就会采用一种遍历查找的方式去找到对应的旧节点。

#### keep-alive 了解吗？

`keep-alive` 可以实现组件缓存，当组件切换时不会对当前组件进行卸载。

常用的两个属性 `include/exclude`，允许组件有条件的进行缓存。

两个生命周期 `activated/deactivated`，用来得知当前组件是否处于活跃状态。

keep-alive 的中还运用了 `LRU(Least Recently Used)` 算法。



### 细说说一下对 Vue 生命周期的理解，几个阶段，各自有什么作用？

* **beforeCreate**：在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用。在当前阶段 data、methods、computed 以及 watch 上的数据和方法都不能被访问；

* **created**：实例已经创建完成之后被调用。在这一步，实例已完成以下的配置：数据观测(data observer)，属性和方法的运算， watch/event 事件回调。这里没有$el,如果非要想与 Dom 进行交互，可以通过 vm.$nextTick 来访问 Dom；可以请求数据，如果异步请求不需要依赖 Dom 推荐在这个阶段中调用异步请求（因为能更快获取到服务端数据，减少页面 loading 时间，ssr 不支持 beforeMount 、mounted）；

* **beforeMount**：在挂载开始之前被调用：相关的 render 函数首次被调用，可以请求数据；

* **mounted**：在挂载完成后发生，在当前阶段，真实的 Dom 挂载完毕，数据完成双向绑定，可以访问到 Dom 节点，可以请求数据；

* **beforeUpdate**：数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁（patch）之前。可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程；

* **updated**：发生在更新完成之后，当前阶段组件 Dom 已完成更新。要注意的是避免在此期间更改数据，因为这可能会导致无限循环的更新，该钩子在服务器端渲染期间不被调用；

* **beforeDestroy**：实例销毁之前调用。在这一步，实例仍然完全可用。我们可以在这时进行善后收尾工作，比如清除计时器；

* **destroyed**：Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。 该钩子在服务器端渲染期间不被调用；

* **activated**：keep-alive 专属，组件被激活时调用；

* **deactivated**：keep-alive 专属，组件被销毁时调用。



### 第一次页面加载会触发哪几个钩子？

**beforeCreate**、**created**、**beforeMount**、**mounted**。



### Vue 的父子组件生命周期钩子函数执行顺序

- 加载渲染过程

父 beforeCreate -> 父 created -> 父 beforeMount -> 子 beforeCreate -> 子 created -> 子 beforeMount -> 子 mounted -> 父 mounted

- 子组件更新过程

父 beforeUpdate -> 子 beforeUpdate -> 子 updated -> 父 updated

- 父组件更新过程

父 beforeUpdate -> 父 updated

- 销毁过程

父 beforeDestroy -> 子 beforeDestroy -> 子 destroyed -> 父 destroyed



### 说出至少4种 vue 当中的指令和它的用法？

* **v-bind**：动态更新 HTML 上的属性；
* **v-html**：相当于 innerHTML；
* **v-text**：相当于 innerText；
* **v-model**：双向绑定（处理value与input的语法糖）；
* **v-cloak**：解决初始化慢导致页面闪动；
* **v-on**：事件绑定；
* **v-once**：定义组件或者元素只渲染一次；
* **v-if**：条件渲染；
* **v-show**：显示与隐藏元素；
* **v-for**：列表渲染；
* **v-pre**：跳过这个元素以及子元素的编译过程，以此来加快整个项目的编译速度。



### v-for 与 v-if 的优先级?

v-for 和 v-if 不要在同一个标签中使用,因为解析时先解析 v-for 再解析 v-if。如果遇到需要同时使用时可以考虑写成计算属性的方式或者嵌套的方式。

> 注意 vue3.x 中 v-if 优先级高于 v-for。



### v-if 与 v-show 的区别与应用场景?

v-if 在编译过程中会被转化成三元表达式,条件不满足时不渲染此节点。

v-show 会被编译成指令，条件不满足时控制样式将对应节点隐藏 （display:none）

**使用场景**

v-if 适用于在运行时很少改变条件，不需要频繁切换条件的场景

v-show 适用于需要非常频繁切换条件的场景



### 动态绑定 class 的方法？



### 什么是 computed，以及和 watch 的区别？

computed 是计算属性，依赖其他属性计算值，并且 computed 的值有缓存，只有当计算值变化才会返回内容，它可以设置 getter 和 setter。

watch 监听到值的变化就会执行回调，在回调中可以进行一些逻辑操作。

计算属性一般用在模板渲染中，某个值是依赖了其它的响应式对象甚至是计算属性计算而来；而侦听属性适用于观测某个值的变化去完成一段复杂的业务逻辑。



### 什么是 slot 插槽？

设置在自组件内部的 插槽 像一个盒子，位置由子组件决定，放什么内容由父组件决定。

实现了内容分发，提高了组件自定义的程度，让组件变的更加灵活。



### 谈谈你对 keep-alive 的了解？

keep-alive 是 Vue 内置的一个组件，可以实现组件缓存，当组件切换时不会对当前组件进行卸载。

- 常用的两个属性 include/exclude，允许组件有条件的进行缓存。
- 两个生命周期 activated/deactivated，用来得知当前组件是否处于活跃状态。
- keep-alive 的中还运用了 LRU(最近最少使用) 算法，选择最近最久未使用的组件予以淘汰。

LRU 的核心思想是如果数据最近被访问过，那么将来被访问的几率也更高，所以我们将命中缓存的组件 key 重新插入到 this.keys 的尾部，这样一来，this.keys 中越往头部的数据即将来被访问几率越低，所以当缓存数量达到最大值时，我们就删除将来被访问几率最低的数据，即 this.keys 中第一个缓存的组件。



### 什么阶段才能访问 DOM ？



### 单文件组件中 css 只在当前组件起作用？



### vue 常用的修饰符？

**事件修饰符**

- .stop 阻止事件继续传播
- .prevent 阻止标签默认行为
- .capture 使用事件捕获模式,即元素自身触发的事件先在此处处理，然后才交由内部元素进行处理
- .self 只当在 event.target 是当前元素自身时触发处理函数
- .once 事件将只会触发一次
- .passive 告诉浏览器你不想阻止事件的默认行为

**v-model 的修饰符**

- .lazy 通过这个修饰符，转变为在 change 事件再同步
- .number 自动将用户的输入值转化为数值类型
- .trim 自动过滤用户输入的首尾空格

**键盘事件的修饰符**

- .enter
- .tab
- .delete (捕获“删除”和“退格”键)
- .esc
- .space
- .up
- .down
- .left
- .right

**系统修饰键**

- .ctrl
- .alt
- .shift
- .meta

**鼠标按钮修饰符**

- .left
- .right
- .middle



### v-on 可以监听多个方法吗？



### v-for 中 key 值的作用?

key的作用主要是为了高效的更新虚拟DOM，其原理是vue在patch过程中通过key可以精准判断两个节点是否是同一个，从而避免频繁更新不同元素，使得整个patch过程更加高效，减少DOM操 作量，提高性能。

另外，若不设置key还可能在列表更新时引发一些隐蔽的bug

vue中在使用相同标签名元素的过渡切换时，也会使用到key属性，其目的也是为了让vue可以区分它们，否则vue只会替换其内部属性而不会触发过渡效果。



### vue 事件中如何使用 event 对象？



### vue 中如何编写可复用的组件？



### vue 更新数组时触发视图更新的方法？

数组考虑性能原因没有用 defineProperty 对数组的每一项进行拦截，而是选择对 7 种数组（push,shift,pop,splice,unshift,sort,reverse）方法进行重写(AOP 切片思想)

所以在 Vue 中修改数组的索引和长度是无法监控到的。需要通过以上 7 种变异方法修改数组才会触发数组对应的 watcher 进行更新或者使用 Vue.set 方法。



### vue 中对象更改检测的注意事项?



### 怎样理解 Vue 的单向数据流？

数据总是从父组件传到子组件，子组件没有权利修改父组件传过来的数据，只能请求父组件对原始数据进行修改。这样会防止从子组件意外改变父级组件的状态，从而导致你的应用的数据流向难以理解。

如果实在要改变父组件的 prop 值 可以再 data 里面定义一个变量 并用 prop 的值初始化它 之后用 $emit 通知父组件去修改。

> 注意：在子组件直接用 v-model 绑定父组件传过来的 prop 这样是不规范的写法 开发环境会报警告。



### Vue 组件通信方案？

- prop：父组件向子组件传递数据；
- $emit：子组件传递数据给父组件；
- $parent || $children： 获取当前组件的父组件或者当前组件的子组件；
- $attrs  || $listeners： A->B->C；
- provide / inject：父组件中通过 provide 来提供变量，然后在子组件中通过 inject 来注入变量；
- $refs：获取组件实例；
- eventBus：兄弟组件数据传递；
- vuex：状态管理。



### 什么是路由，如何实现？

[原生JS实现一个路由](https://juejin.im/post/5c5014ef51882525aa50a25b?utm_source=gold_browser_extension)



### router-link 与 a 标签的区别?



### `$route` 和 `$router` 的区别?

- $route是`路由信息对象`，包括path，params，hash，query，fullPath，matched，name等路由信息参数。
- 而$router是`路由实例`对象包括了路由的跳转方法，钩子函数等。



### vue-router 如何响应 路由参数 的变化？

通过 watch 监听路由参数再发请求：

``` js
watch: { //通过watch来监听路由变化

 "$route": function(){
 this.getData(this.$route.params.xxx);
 }
}
```

用 :key 来阻止复用：

``` html
<router-view :key="$route.fullPath" />
```



### 嵌套路由怎么定义？



### 怎么定义 vue-router 的动态路由? 怎么获取传过来的值?

定义：

``` vue
const User = {
  template: "<div>User</div>",
};

const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: "/user/:id", component: User },
  ],
});
```

获取值：

``` js
// this.$route.params.id
```



### vue-router 有哪几种导航钩子，执行顺序是什么？

路由钩子的执行流程, 钩子函数种类有:全局守卫、路由守卫、组件守卫

**完整的导航解析流程:**

1. 导航被触发。
2. 在失活的组件里调用 beforeRouteLeave 守卫。
3. 调用全局的 beforeEach 守卫。
4. 在重用的组件里调用 beforeRouteUpdate 守卫 (2.2+)。
5. 在路由配置里调用 beforeEnter。
6. 解析异步路由组件。
7. 在被激活的组件里调用 beforeRouteEnter。
8. 调用全局的 beforeResolve 守卫 (2.5+)。
9. 导航被确认。
10. 调用全局的 afterEach 钩子。
11. 触发 DOM 更新。
12. 调用 beforeRouteEnter 守卫中传给 next 的回调函数，创建好的组件实例会作为回调函数的参数传入。



### vue-cli 中如何使用 JSON 数据模拟？



### vue-cli 中 http 请求的统一管理?



### vue-cli 中如何使用背景图片？



### 如何配置 vue 打包生成文件的路径？



### 如何自定义指令(如：v-check, v-focus)? 它有哪些钩子函数? 还有哪些钩子函数参数?

指令本质上是装饰器，是 vue 对 HTML 元素的扩展，给 HTML 元素增加自定义功能。vue 编译 DOM 时，会找到指令对象，执行指令的相关方法。

自定义指令有五个生命周期（也叫钩子函数），分别是 bind、inserted、update、componentUpdated、unbind。

* **bind**：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。

* **inserted**：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。

* **update**：被绑定于元素所在的模板更新时调用，而无论绑定值是否变化。通过比较更新前后的绑定值，可以忽略不必要的模板更新。

* **componentUpdated**：被绑定元素所在模板完成一次更新周期时调用。

* **unbind**：只调用一次，指令与元素解绑时调用。

**原理**

1.在生成 ast 语法树时，遇到指令会给当前元素添加 directives 属性

2.通过 genDirectives 生成指令代码

3.在 patch 前将指令的钩子提取到 cbs 中,在 patch 过程中调用对应的钩子

4.当执行指令对应钩子函数时，调用对应指令定义的方法



### vue 如何自定义一个过滤器？



### vuex 是什么？怎么使用？哪种功能场景使用它？

vuex 是专门为 vue 提供的全局状态管理系统，用于多个组件中数据共享、数据缓存等。（无法持久化、内部核心原理是通过创造一个全局实例 new Vue）。

* **State**：定义了应用状态的数据结构，可以在这里设置默认的初始状态。

* **Getter**：允许组件从 Store 中获取数据，mapGetters 辅助函数仅仅是将 store 中的 getter 映射到局部计算属性。

* **Mutation**：是唯一更改 store 中状态的方法，且必须是同步函数。

* **Action**：用于提交 mutation，而不是直接变更状态，可以包含任意异步操作。

* **Module**：允许将单一的 Store 拆分为多个 store 且同时保存在单一的状态树中。



### vuex 的 getter 有什么作用，怎么用，那些场景下用?
* getter 可以对 state 进行计算操作，它就是 store 的计算属性
* 虽然在组件内也可以做计算属性，但是 getters 可以在多给件之间复用
* 如果一个状态只在一个组件内使用，是可以不用 getters



### 页面刷新时实现 vuex 缓存?

需要做 vuex 数据持久化 一般使用本地存储的方案来保存数据 可以自己设计存储方案 也可以使用第三方插件

推荐使用 vuex-persist 插件，它就是为 Vuex 持久化存储而生的一个插件。不需要你手动存取 storage ，而是直接将状态保存至 cookie 或者 localStorage 中。



### vue 中 ajax 请求代码应该写在组件的methods中还是 vuex 的 action 中?
如果请求来的数据不是要被其他组件公用，仅仅在请求的组件内使用，就不需要放入 vuex 里。

如果被其他地方复用，请将请求放入 action 里，方便复用，并包装成 promise 返回。



### Vuex 为什么要分模块并且加命名空间？

**模块**:由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。为了解决以上问题，Vuex 允许我们将 store 分割成模块（module）。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块。

**命名空间**：默认情况下，模块内部的 action、mutation 和 getter 是注册在全局命名空间的——这样使得多个模块能够对同一 mutation 或 action 作出响应。如果希望你的模块具有更高的封装度和复用性，你可以通过添加 namespaced: true 的方式使其成为带命名空间的模块。当模块被注册后，它的所有 getter、action 及 mutation 都会自动根据模块注册的路径调整命名。



### 函数式组件使用场景和原理？

**函数式组件与普通组件的区别：**

1.函数式组件需要在声明组件是指定 functional:true
2.不需要实例化，所以没有this,this通过render函数的第二个参数context来代替
3.没有生命周期钩子函数，不能使用计算属性，watch
4.不能通过$emit 对外暴露事件，调用事件只能通过context.listeners.click的方式调用外部传入的事件
5.因为函数式组件是没有实例化的，所以在外部通过ref去引用组件时，实际引用的是HTMLElement
6.函数式组件的props可以不用显示声明，所以没有在props里面声明的属性都会被自动隐式解析为prop,而普通组件所有未声明的属性都解析到$attrs里面，并自动挂载到组件根元素上面(可以通过inheritAttrs属性禁止)。



**优点：** 

1.由于函数式组件不需要实例化，无状态，没有生命周期，所以渲染性能要好于普通组件；

2.函数式组件结构比较简单，代码结构更清晰。



**使用场景：**

一个简单的展示组件，作为容器组件使用 比如 router-view 就是一个函数式组件；

“高阶组件”——用于接收一个组件作为参数，返回一个被包装过的组件。



### v-model 原理？

v-model 只是语法糖而已。

v-model 在内部为不同的输入元素使用不同的 property 并抛出不同的事件：

- text 和 textarea 元素使用 value property 和 input 事件；
- checkbox 和 radio 使用 checked property 和 change 事件；
- select 字段将 value 作为 prop 并将 change 作为事件。

普通标签上：

``` html
<input v-model="sth" />  <!-- 这一行等于下一行 -->
<input v-bind:value="sth" v-on:input="sth = $event.target.value" />
```

在组件上：

``` vue
<currency-input v-model="price"></currentcy-input>
<!--上行代码是下行的语法糖
 <currency-input :value="price" @input="price = arguments[0]"></currency-input>
-->

<!-- 子组件定义 -->
Vue.component('currency-input', {
 template: `
  <span>
   <input
    ref="input"
    :value="value"
    @input="$emit('input', $event.target.value)"
   >
  </span>
 `,
 props: ['value'],
})
```



### vue如何优化首屏加载速度？

减少入口文件体积，静态资源本地缓存，开启Gzip压缩，使用SSR。



### 你都做过哪些 Vue 的性能优化？

* 对象层级不要过深，否则性能就会差

* 不需要响应式的数据不要放到 data 中（可以用 Object.freeze() 冻结数据）

* v-if 和 v-show 区分使用场景

* computed 和 watch 区分使用场景

* v-for 遍历必须加 key，key 最好是 id 值，且避免同时使用 v-if

* 大数据列表和表格性能优化-虚拟列表/虚拟表格

* 防止内部泄漏，组件销毁后把全局变量和事件销毁

* 图片懒加载

* 路由懒加载

* 第三方插件的按需引入

* 适当采用 keep-alive 缓存组件

* 防抖、节流运用

* 服务端渲染 SSR or 预渲染



### 使用过 Vue SSR 吗，说说 SSR？

SSR 也就是服务端渲染，也就是将 Vue 在客户端把标签渲染成 HTML 的工作放在服务端完成，然后再把 html 直接返回给客户端。

**优点：**

SSR 有着更好的 SEO、并且首屏加载速度更快

**缺点：** 开发条件会受到限制，服务器端渲染只支持 beforeCreate 和 created 两个钩子，当我们需要一些外部扩展库时需要特殊处理，服务端渲染应用程序也需要处于 Node.js 的运行环境。

服务器会有更大的负载需求。



#### 如何理解 Vue 是一个渐进式框架？



### Vue2.0 响应式数据的原理？

整体思路是数据劫持+观察者模式。

vue 实现数据双向绑定主要是：采用数据劫持结合发布者-订阅者模式的方式，通过Object.defineProperty（）来劫持各个属性（只会劫持已经存在的属性），数组则是通过重写数组方法来实现。在数据变动时发布消息给订阅者，触发相应监听回调。当把一个普通 Javascript 对象传给 Vue 实例来作为它的 data 选项时，Vue 将遍历它的属性，用 Object.defineProperty 将它们转为 getter/setter。用户看不到 getter/setter，但是在内部它们让 Vue 追踪依赖，在属性被访问和修改时通知对应的 watcher 去更新(派发更新)。

vue 的数据双向绑定 将 MVVM 作为数据绑定的入口，整合 Observer，Compile 和 Watcher 三者，通过 Observer 来监听自己的 model 的数据变化，通过 Compile 来解析编译模板指令（vue 中是用来解析 {{}}），最终利用 watcher 搭起 observer 和 Compile 之间的通信桥梁，达到数据变化 —>视图更新；视图交互变化（input）—>数据 model 变更双向绑定效果。

[vue双向数据绑定实现](https://juejin.im/entry/5923973da22b9d005893805a)



#### Vue 中响应式数据是如何做到对某个对象的深层次属性的监听的？



#### 简要说明 Vue 2.x 的全链路运作机制？



### Vue3.0 和 2.0 的响应式原理区别？

Vue3.x 改用 Proxy 替代 Object.defineProperty。因为 Proxy 可以直接监听对象和数组的变化，并且有多达 13 种拦截方法。



### virtual dom 原理？

由于在浏览器中操作 DOM 是很昂贵的。频繁的操作 DOM，会产生一定的性能问题。这就是虚拟 Dom 的产生原因。Vue2 的 Virtual DOM 借鉴了开源库 snabbdom 的实现。Virtual DOM 本质就是用一个原生的 JS 对象去描述一个 DOM 节点，是对真实 DOM 的一层抽象。



### Diff 算法的内部实现?

[diff 算法](https://juejin.im/post/5affd01551882542c83301da)

[diff 算法原理](https://juejin.cn/post/6953433215218483236)



### 从 template 转换成真实 DOM 的实现机制（模板编译原理）?

Vue 的编译过程就是将 template 转化为 render 函数的过程 分为以下三步：

* 第一步是将 模板字符串 转换成 element ASTs（解析器）
* 第二步是对 AST 进行静态节点标记，主要用来做虚拟DOM的渲染优化（优化器）
* 第三步是 使用 element ASTs 生成 render 函数代码字符串（代码生成器）



### Vue 事件绑定原理（事件机制）？

原生事件绑定是通过 addEventListener 绑定给真实元素的，组件事件绑定是通过 Vue 自定义的$on 实现的。如果要在组件上使用原生事件，需要加.native 修饰符，这样就相当于在父组件中把子组件当做普通 html 标签，然后加上原生事件。

$on、$emit 是基于发布订阅模式的，维护一个事件中心，on 的时候将事件按名称存在事件中心里，称之为订阅者，然后 emit 将对应的事件进行发布，去执行事件中心里的对应的监听器。



### Vue.mixin 的使用场景和原理？

在日常的开发中，我们经常会遇到在不同的组件中经常会需要用到一些相同或者相似的代码，这些代码的功能相对独立，可以通过 Vue 的 mixin 功能抽离公共的业务逻辑，原理类似“对象的继承”，当组件初始化时会调用 mergeOptions 方法进行合并，采用策略模式针对不同的属性进行合并。当组件和混入对象含有同名选项时，这些选项将以恰当的方式进行“合并”。



### nextTick 使用场景和原理？

nextTick 中的回调是在下次 DOM 更新循环结束之后执行的延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。主要思路就是采用微任务优先的方式调用异步方法去执行 nextTick 包装的方法。



### Vue.set 方法原理？

当给对象新增不存在的属性 首先会把新的属性进行响应式跟踪 然后会触发对象__ob__的 dep 收集到的 watcher 去更新，当修改数组索引时我们调用数组本身的 splice 方法去更新数组。



### Vue.extend 作用和原理？

官方解释：Vue.extend 使用基础 Vue 构造器，创建一个“子类”。参数是一个包含组件选项的对象。

其实就是一个子类构造器 是 Vue 组件的核心 api 实现思路就是使用原型继承的方法返回了 Vue 的子类 并且利用 mergeOptions 把传入组件的 options 和父类的 options 进行了合并。



### 生命周期钩子是如何实现的？

Vue 的生命周期钩子核心实现是利用发布订阅模式先把用户传入的的生命周期钩子订阅好（内部采用数组的方式存储）然后在创建组件实例的过程中会一次执行对应的钩子方法（发布）。



### vue 中使用了哪些设计模式？

1.工厂模式 - 传入参数即可创建实例

虚拟 DOM 根据参数的不同返回基础标签的 Vnode 和组件 Vnode

2.单例模式 - 整个程序有且仅有一个实例

vuex 和 vue-router 的插件注册方法 install 判断如果系统存在实例就直接返回掉

3.发布-订阅模式 (vue 事件机制)

4.观察者模式 (响应式数据原理)

5.装饰模式: (@装饰器的用法)

6.策略模式 策略模式指对象有某个行为,但是在不同的场景中,该行为有不同的实现方案-比如选项的合并策略



### vue3.0 用过吗 了解多少？

- 响应式原理的改变 Vue3.x 使用 Proxy 取代 Vue2.x 版本的 Object.defineProperty；
- 组件选项声明方式 Vue3.x 使用 Composition API setup 是 Vue3.x 新增的一个选项， 他是组件内使用 Composition API 的入口；
- 模板语法变化 slot 具名插槽语法 自定义指令 v-model 升级；
- 其它方面的更改 Suspense 支持 Fragment（多个根节点）和 Protal（在 dom 其他部分渲染组建内容）组件，针对一些特殊的场景做了处理。 基于 treeshaking 优化，提供了更多的内置功能。



#### Vue 原理（手写代码，实现数据劫持）



#### vue-router 源码



#### Vue2.x 和 Vue3.x 渲染器的 diff 算法分别说一下。

简单来说，diff 算法有以下过程

- 同级比较，再比较子节点
- 先判断一方有子节点一方没有子节点的情况(如果新的 children 没有子节点，将旧的子节点移除)
- 比较都有子节点的情况(核心 diff)
- 递归比较子节点

正常 diff 两个树的时间复杂度是 `O(n^3)`，但实际情况下我们很少会进行 `跨层级的移动 DOM`，所以 Vue 将 diff 进行了优化，从 `O(n^3) -> O(n)`，只有当新旧 children 都为多个子节点时才需要用核心的 diff 算法进行同层级比较。

Vue2 的核心 diff 算法采用了`双端比较`的算法，同时从新旧 children 的两端开始进行比较，借助 key 值找到可复用的节点，再进行相关操作。相比 React 的 diff 算法，同样情况下可以减少移动节点次数，减少不必要的性能损耗，更加的优雅。

Vue3.x 借鉴了 `ivi算法` 和 `inferno算法`。在创建 VNode 时就确定其类型，以及在 `mount/patch` 的过程中采用`位运算`来判断一个 VNode 的类型，在这个基础之上再配合核心的 diff 算法，使得性能上较 Vue2.x 有了提升。该算法中还运用了`动态规划`的思想求解最长递归子序列。

