# React 面试题

#### React 有什么特点？

* 在 React 中，一切都是组件；

* 使用虚拟 DOM；
* 可以用服务器端渲染；
* 遵循单向数据流；



#### 什么是声明式编程

声明式编程是一种编程范式，它关注的是你**要做什么**，而不是**如何做**。它表达逻辑而不显式地定义步骤。这意味着我们需要根据逻辑的计算来声明要显示的组件。它没有描述控制流步骤。声明式编程的例子有HTML、SQL等。



#### 声明式编程 vs 命令式编程

声明式编程的编写方式描述了应该做什么，而命令式编程描述了如何做。在声明式编程中，让编译器决定如何做事情。声明性程序很容易推理，因为代码本身描述了它在做什么。



#### 什么是函数式编程

函数式编程是声明式编程的一部分。javascript中的函数是第一类公民，这意味着函数是数据，你可以像保存变量一样在应用程序中保存、检索和传递这些函数。



#### React class 组件有哪些周期函数？分别有什么作用？

* componentWillMount：组件即将被装载、渲染到页面上；
* render：组件渲染 DOM 节点；
* componentDidMount：组件装载之后；
* shouldComponentUpdate：组件接受到新属性或者新状态的时候（可以返回 false，接收数据后不更新，阻止 render 调用，后面的函数不会被继续执行了）；
* componentWillUpdate：组件即将更新，此时不能修改属性和状态；
* componentDidUpdate：组件更新完成；
* componentWillUnmount：组件即将销毁，它将用于取消任何传出的网络请求，或删除与该组件关联的所有事件侦听器。
* getDerivedStateFromError：这用于在组件树中出现错误时呈现回退UI；
* componentDidCatch：用于在组件树中出现错误时记录错误；



#### React Class 组件中请求可以在 componentWillMount 中发起吗？为什么？



#### 函数式组件和类组件有什么区别？

函数组件和类组件当然是有区别的，而且函数组件的性能比类组件的性能要高，因为类组件使用的时候要实例化，而函数组件直接执行函数取返回结果即可。为了提高性能，尽量使用函数组件。



#### React 合成事件和原生事件区别？

React 所有事件挂载到 document 上，event 不是原生的，是`SyntheticEvent`合成事件对象。

为了解决跨浏览器的兼容性问题，`SyntheticEvent` 实例将被传递给你的事件处理函数，`SyntheticEvent`是 React 跨浏览器的浏览器原生事件包装器，它还拥有和浏览器原生事件相同的接口，包括 `stopPropagation()` 和 `preventDefault()`。

React 实际上并不将事件附加到子节点本身。React 使用单个事件侦听器侦听顶层的所有事件。这对性能有好处，也意味着 React 在更新 DOM 时不需要跟踪事件监听器。

好处：

- 更好的兼容性和跨平台，摆脱传统DOM事件
- 挂载到document，减少内存消耗，避免频繁解绑
- 方便事件的统一管理，如：事务机制



#### 为什么类方法需要绑定到类实例？

在 JS 中，`this` 值会根据当前上下文变化。在 React 类组件方法中，开发人员通常希望 `this` 引用组件的当前实例，因此有必要将这些方法绑定到实例。



#### 在构造函数调用 `super` 并将 `props` 作为参数传入的作用是啥？

在调用 `super()` 方法之前，子类构造函数无法使用`this`引用，ES6 子类也是如此。将 `props` 参数传递给 `super()` 调用的主要原因是在子构造函数中能够通过`this.props`来获取传入的 `props`。



#### state 和 props 有什么区别？

* props 是一个从外部传进组件的参数，主要作为就是从父组件向子组件传递数据，它具有可读性和不变性，只能通过外部组件主动传入新的 props 来重新渲染子组件；
* state 的主要作用是用于组件保存、控制以及修改自己的状态，算是组件的私有属性，不可通过外部访问和修改。



#### 什么是 prop drilling，如何避免？

在构建 React 应用程序时，在多层嵌套组件来使用另一个嵌套组件提供的数据。最简单的方法是将一个 `prop` 从每个组件一层层的传递下去，从源组件传递到深层嵌套组件，这叫做**prop drilling**。

`prop drilling`的主要缺点是原本不需要数据的组件变得不必要地复杂，并且难以维护。

为了避免`prop drilling`，一种常用的方法是使用**React Context**。通过定义提供数据的`Provider`组件，并允许嵌套的组件通过`Consumer`组件或`useContext` Hook 使用上下文数据。



#### setState

- 有时异步（普通使用），有时同步（setTimeout, DOM事件中使用）
- 有时合并（对象形式），有时不合并（函数形式），比较好理解（类似 Object.assign），函数无法合并



#### React 中的 refs 作用是什么？

Refs 是 React 提供给我们的安全访问 DOM 元素或者某个组件实例的句柄。
我们可以为元素添加 ref 属性然后在回调函数中接受该元素在 DOM 树中的句柄，该值会作为回调函数的第一个参数返回。



``` jsx
// 创建方法
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  render() {
    return <div ref={this.myRef} />;
  }
}

// 或者
class MyComponent extends Component {
  handleSubmit = () => {
    console.log("Input Value is: ", this.input.value)
  }
  render () {
    return (
      <form onSubmit={this.handleSubmit}>
        <input
          type='text'
          ref={(input) => this.input = input} /> 
        <button type='submit'>Submit</button>
      </form>
    )
  }
}

// 函数组件
const MyComponent = () => {
  const myRef = React.useRef();
    
  return <div ref={myRef} />;
}
```



#### 什么是受控组件和非受控组件？

在HTML当中，像`<input>`,`<textarea>`, 和 `<select>`这类表单元素会维持自身状态，并根据用户输入进行更新。但在 React 中，可变的状态通常保存在组件的状态属性中，并且只能用 `setState()` 方法进行更新。

而**受控组件**是在 React 中处理输入表单的一种技术。表单元素通常维护它们自己的状态，而 React 则在组件的状态属性中维护状态。我们可以将两者结合起来控制输入表单。这称为受控组件。因此，在受控组件表单中，数据由 React 组件处理。。

``` jsx
class Demo extends Component {
    constructor(props) {
        super(props);
        this.state = {
            value: ''
        }
    }

    handleChange(e) {
        this.setState({
            value: e.target.value
        })
    }

    render() {
        return (
            <input value={this.state.value} onChange={e => this.handleChange(e)} />
        )
    }
}
```



**非受控组件**，即组件的状态不受 React 控制的组件，大多数情况下，建议使用受控组件：

``` jsx
import React, { Component } from 'react';

class Demo extends Component {
    render() {
        return (
            <input />
        )
    }
}
```



#### 什么是 PropTypes？

随着时间的推移，应用程序会变得越来越大，因此类型检查非常重要。`PropTypes`为组件提供类型检查，并为其他开发人员提供很好的文档。如果react项目不使用 Typescript，建议为组件添加 `PropTypes`。

``` jsx
import React from 'react';
import PropTypes from 'prop-types';

export const UserDisplay = ({name, address, age}) => {

    UserDisplay.defaultProps = {
        name: 'myname',
        age: 100,
        address: "0000 onestreet"
    };

    return (
        <>
            <div>
                <div class="label">Name:</div>
                <div>{name}</div>
            </div>
            <div>
                <div class="label">Address:</div>
                <div>{address}</div>
            </div>
            <div>
                <div class="label">Age:</div>
                <div>{age}</div>
            </div>
        </>
    )
}

UserDisplay.propTypes = {
    name: PropTypes.string.isRequired,
    address: PropTypes.objectOf(PropTypes.string),
    age: PropTypes.number.isRequired
}
```



#### 什么是高阶组件?

高阶组件是一个以组件为参数并返回一个新组件的函数。HOC 运行你重用代码、逻辑和引导抽象。最常见的可能是 Redux 的 connect 函数。除了简单分享工具库和简单的组合，HOC 最好的方式是共享 React 组件之间的行为。如果你发现你在不同的地方写了大量代码来做同一件事时，就应该考虑将代码重构为可重用的 HOC。



#### React context 是什么？

简单说就是，当你不想在组件树中通过逐层传递`props`或者`state`的方式来传递数据时，可以使用`Context`来实现**跨层级**的组件数据传递。



#### 使用 React Hooks 有什么优势？

Hooks 是一个特殊的函数，是 React 16.8 引入的特性，他允许你在不写 class 的情况下操作 state 和 React 的其他特性。
Hooks 只是多了一种写组件的方法，使编写一个组件更简单更方便，同时可以自定义 Hook 把公共的逻辑提取出来，让逻辑在多个组件之间共享。

Hooks 通常支持提取和重用跨多个组件通用的有状态逻辑，而无需承担高阶组件或渲染 `props` 的负担。`Hooks` 可以轻松地操作函数组件的状态，而不需要将它们转换为类组件。

使用 React 的 Hooks 可以无需复杂的 DOM 结构，简洁易懂。



#### React 中高阶函数和自定义 Hook 的优缺点？



#### 简要说明 React Hook 中 useState 和 useEffect 的运行原理？



#### React 中 useState 是如何做数据初始化的？



#### React Hook 中 useEffect 有哪些参数，如何检测数组依赖项的变化？



#### React 的 useEffect 是如何监听数组依赖项的变化的？



#### React Hook 和闭包有什么关联关系？



#### React 数据通信？

* 父级传递子级：把数据挂载子组件的属性上，子组件通过 `this.props` 来接收父组件的数据；

* 子级传递父级：父级需要定义一个修改数据的方法，把修改数据的方法传给子组件，当子组件需要修改父级数据时，调用父级传过来的修改方法；
* context
* redux



#### Redux 工作原理？

在 React中，组件连接到 redux ，如果要访问 redux，需要派出一个 `action`。action 中的 `type` 是必选的， `payload` 是可选的，action 将其转发给 Reducer。

当`reducer`收到`action`时，通过 `swithc...case` 语法比较 `action` 中`type`。 匹配时，更新对应的内容返回新的 `state`。

当`Redux`状态更改时，连接到`Redux`的组件将接收新的状态作为`props`。当组件接收到这些`props`时，它将进入更新阶段并重新渲染 UI。



#### Virtual DOM 与 Real DOM 的区别？

Virtual DOM 无法直接更新 HTML，有效降低大面积（真实DOM节点）的重绘与排版。

真实DOM频繁排版与重绘的效率是相当低的。

Virtual DOM 只不过是 Real DOM 的 JavasSript 对象表示，它最初只是 real DOM 的副本。它是一个节点树，它将元素、它们的属性和内容作为对象及其属性。 React 的渲染函数从 React 组件中创建一个节点树。然后它响应数据模型中的变化来更新该树。

Virtual DOM 在每当有更新时，它都会维护两个虚拟 DOM，以比较之前的状态和当前状态，并确定哪些对象已被更改，将只用实际更改的内容一次性更新到 Real DOM。



#### React 的虚拟 DOM 是怎么实现的

首先说说为什么要使用 Virturl DOM，因为操作真实 DOM 的耗费的性能代价太高，所以 react 内部使用 js 实现了一套 dom 结构，在每次操作在和真实 dom 之前，使用实现好的 diff 算法，对虚拟 dom 进行比较，递归找出有变化的 dom 节点，然后对其进行更新操作。为了实现虚拟 DOM，我们需要把每一种节点类型抽象成对象，每一种节点类型有自己的属性，也就是 prop，每次进行 diff 的时候，react 会先比较该节点类型，假如节点类型不一样，那么react 会直接删除该节点，然后直接创建新的节点插入到其中，假如节点类型一样，那么会比较 prop 是否有更新，假如有 prop 不一样，那么 react 会判定该节点有更新，那么重渲染该节点，然后在对其子节点进行比较，一层一层往下，直到没有子节点。



#### React中 的 StrictMode 是什么？

- 验证内部组件是否遵循某些推荐做法，如果不在控制台中，则会发出警告。
- 验证不赞成使用的方法，如果使用了严格模式，则会在控制台中警告您。
- 通过识别潜在风险来帮助您预防某些副作用。



#### React 中 key 的重要性是什么？

- diff 算法中通过 tag 和 key 判断，是否是同一个节点；
- 减少渲染次数，提升渲染性能。



#### React Fiber是什么？

**Fiber** 是 React 16 中新的协调引擎或重新实现核心算法。它的主要目标是支持虚拟DOM的增量渲染。**React Fiber** 的目标是提高其在动画、布局、手势、暂停、中止或重用等方面的适用性，并为不同类型的更新分配优先级，以及新的并发原语。

React Fiber 的目标是增强其在动画、布局和手势等领域的适用性。它的主要特性是增量渲染:能够将渲染工作分割成块，并将其分散到多个帧中。



#### 当调用`setState`时，React `render` 是如何工作的？

可以将"`render`"分为两个步骤：

1. 虚拟 DOM 渲染:当`render`方法被调用时，它返回一个新的组件的虚拟 DOM 结构。当调用`setState()`时，`render`会被再次调用，因为默认情况下`shouldComponentUpdate`总是返回`true`，所以默认情况下 React 是没有优化的。
2. 原生 DOM 渲染:React 只会在虚拟DOM中修改真实DOM节点，而且修改的次数非常少——这是很棒的React特性，它优化了真实DOM的变化，使React变得更快。



#### 列举你常用的 React 性能优化技巧？

* 适当地使用`shouldComponentUpdate`生命周期方法。 它避免了子组件的不必要的渲染。 如果树中有100个组件，则不重新渲染整个组件树来提高应用程序性能。
* class 类组件中用`PureComponent`，无状态组件(无状态)中用`memo`。
* 使用`create-react-app`来构建项目，这会创建整个项目结构，并进行大量优化。
* 不可变性是提高性能的关键。不要对数据进行修改，而是始终在现有集合的基础上创建新的集合，以保持尽可能少的复制，从而提高性能。
* 减少函数 bind this 的次数。
* 在显示列表或表格时始终使用 `Keys`，这会让 React 的更新速度更快。
* 代码分离是将代码插入到单独的文件中，只加载模块或部分所需的文件的技术。
* 在页面中使用了`setTimout()`、`addEventListener()`等，要及时在`componentWillUnmount()`中销毁。
* 使用异步组件。
* 合理使用不可变值 ImmutableJS。



#### React 如何发现重渲染、什么原因容易造成重渲染、如何避免重渲染？

- `React.memo()`:这可以防止不必要地重新渲染函数组件
- `PureComponent`:这可以防止不必要地重新渲染类组件

这两种方法都依赖于对传递给组件的`props`的浅比较，如果 `props` 没有改变，那么组件将不会重新渲染。虽然这两种工具都非常有用，但是浅比较会带来额外的性能损失，因此如果使用不当，这两种方法都会对性能产生负面影响。



#### 在 React 中如何识别一个表单项里的表单做到了最小粒度 / 代价的渲染？



#### 在 React 的开发的过程中你能想到哪些控制渲染成本的方法？

