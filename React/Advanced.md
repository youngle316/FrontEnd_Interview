# React 高级

## 问：React 为什么要有一个 Key

答：

1. 辨别 React 元素,以便在更新过程中保持元素的身份和保存其状态。
2. 帮助 React 识别哪些元素发生了变化、删除或添加,以更新 DOM 高效。
3. 如果不指定 key,React 会使用元素索引来判断元素的变化,这可能会导致性能问题或错误的渲染输出。

## 问：谈谈对虚拟 DOM 的理解

答：

虚拟 DOM,就是 JavaScript 对象,它描述了真实 DOM 树的结构。每当数据变化时,React 会通过比较新旧两个虚拟 DOM 树来决定如何高效地更新 UI。

虚拟 DOM 主要有以下优点:

1. 性能更高。由于比较的是 JavaScript 对象,所以比直接修改真实 DOM 的性能更高。
2. 跨平台。React Native 中也使用了同样的虚拟 DOM,所以学习一次,可以编写 Web 和 Native 应用。
3. 使渲染更容易调试。在开发环境下,React 会保存虚拟 DOM 的快照,方便我们确认视图是否更改。
4. 轻松实现服务端渲染。在服务器上也可以渲染出虚拟 DOM,然后将其转换为字符串,一并发送给客户端,客户端继续合并虚拟 DOM 和客户端数据,减少了回流。

具体而言,虚拟 DOM 就是 React 元素树。它以 JavaScript 对象的形式表达界面,包含:

- 元素类型(div、span 等)
- 一组props(属性)
- 可能包含的子元素
举例来说,下面的 JSX:

```javascript
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

会被转换为以下虚拟 DOM 对象:

```javascript
js
const element = {
  type: 'h1',
  props: {
    className: 'greeting', 
    children: 'Hello, world'
  }
}
```

当数据变化导致元素树发生变化时,React 会比较新旧两棵虚拟 DOM 树,然后用算法算出如何有效地更新实际 DOM。

## 问：fiber 的实现原理

答：

依赖浏览器提供的 API requestIdleCallback() 实现任务调度 -- 优先级高的任务优先执行，优先级低的任务尽量在浏览器空闲时执行。

Fiber 的主要目的有:

1. 增强对动画和手势的支持。以前的架构下,这会导致 frame rate 的下降和掉帧。
2. 可中断和恢复的任务执行。以支持优先级,可以让高优任务优先执行。
3. 利用空闲时间执行低优任务。

Fiber 架构最关键的设计是将渲染过程分为更小的步骤,每个步骤的工作量更小,requestIdleCallback然后让这些步骤可以在两个渲染 frame 之间断续执行。
具体而言,Fiber 包含两个阶段:

1. 协调(Reconciliation)阶段:构建 Fiber 节点树来更新屏幕。它会计算出一棵 Fiber 节点树,描述屏幕上所需要的变更。
2. 提交(Commit)阶段:将变更应用到 DOM 树上。

在两阶段之间,渲染任务可以中断让出主线程,以便进行其他任务。然后在下一个 frame 从中断的地方继续执行,这保证了动画和交互的流畅性。

## 问：React 中的合成事件

React 的合成事件系统层在原生事件之上,对事件进行了包装和抽象。它的目的是使事件在不同浏览器下有一致的性能。
React 合成事件系统的主要工作原理是:

1. 捕获所有通过 DOM 注册的事件,包装成合成事件对象 SyntheticEvent。
2. 这些合成事件对象在所有浏览器中都有一致的接口,屏蔽了浏览器差异。
3. 然后根据事件委托机制,将事件派发到 React 元素树中正确的处理函数。
4. 处理函数通过 event.nativeEvent 可以访问原生事件对象。

主要特点如下:

1. 兼容性:在所有浏览器中都有一致的事件处理方式,不用考虑浏览器差异。
2. 高性能:通过事件委托机制丰富了事件系统,较少的事件 handlers 被注册到 DOM 中。
3. 可被打断:同一类型的合成事件对象在回调期间被触发多次的话,除了最后一次之外其他都会被忽略。这与原生事件不同。
4. 支持事件池:React 会重用 SyntheticEvent 对象,所以回调中对 event 的修改会影响到后续回调的参数。需要使用 event.persist() 手动解除事件池。

事件处理的工作流程主要如下:

1. 当 React 加载或事件注册时,会调用 addEventListner 向 DOM 注册事件。
2. 当事件触发时,React 会调用 dispatchEvent 将事件包装成 SyntheticEvent 对象。
3. 根据事件委托,findDOMNode 找到事件源 React 元素。然后根据 props.onXXX 找到正确的事件处理函数。
4. 将 SyntheticEvent 对象作为参数调用事件处理函数。
5. 处理函数可通过事件对象的 persist 方法解除事件池,加入对 event 的修改。
6. React 会在合适的时机对未解除事件池的事件对象进行复用。

所以,React 合成事件系统通过包装原生事件,提供了跨浏览器一致的事件处理方式,使我们可以更专注于业务逻辑。理解其工作原理,也有助于我们在实践中更好地处理事件与做出性能优化。
