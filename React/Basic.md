# React 基础

### 问：React 组件间的通信方式

答：
1. 父传子：使用 props
2. 子传父：使用 回调函数、ref
3. 兄弟组件：可使用变量提升，将公用的 state 提到相同的父组件
4. 跨级组件：使用 context ，可避免一级一级通过 props 向下传
5. redux 等全局 Store

### 问：React16.4 最新的生命周期方法都有什么

答：

1. 创建时
   1. constructor()
   2. getDerivedStateFromProps()
   3. render()
   4. componentDidMount
2. 更新时
   1. getDerivedStateFromProps()
   2. shouldComponentUpdate()
   3. render()
   4. getSnapshotBeforeUpdate()
   5. componentDidUpdate()
3. 卸载
   1. componentWillUnmount()

### 问：React Lazy 和 Suspense 的作用是什么

答：React Lazy 可以定义一个动态加载的组件。它会在组件首次渲染的时候自动加载组件

```javascript
const OtherComponent = React.lazy(() => import('./OtherComponent'));
```

然后可以在 Suspense 组件中渲染 Lazy 组件:

```javascript
<Suspense fallback={<div>Loading...</div>}>
  <OtherComponent />
</Suspense>
```

如果 OtherComponent 没有被加载完成,Suspense 会显示 fallback 元素来代替。

React Lazy 和 Suspense 的作用主要有:

1. 惰性加载组件,优化页面加载性能。只有用户访问到 Lazy 组件时,才会触发加载。
2. 可以实现按需加载、代码分离,减小 React 应用的文件大小。
3. 配合 Suspense,可以优雅地处理组件的加载中状态,给用户很好的体验。
4. 有助于实现路由级代码分割,比如 React Router 使用 React Lazy 和 Suspense 来实现延迟加载路由组件。

### 问：React 中 setState 是同步还是异步的

答：setState 本身的代码和过程都是 **同步** 的。它能表现出 **异步** 的现象是因为在某些情况下调用顺序在更新之前，导致获取的 state 是更新前的值。

- 同步：setTimeout、原生事件
- 异步：合成事件、钩子函数、promise.then()

setState 的**批量更新优化**也是建立在异步之上，在同步上是不会批量更新的。

batchingStrategy：批量更新策略，通过事务的方式实现 state 的批量更新

isBatchingUpdates 是事务 batchingStrategy 的一个标记，如果为 true,把当前调用 setState 的组件放入 dirtyComponents 数组中，做存储处理，不会立即更新,如果为 false，将 enqueueUpdate 作为参数传入 batchedUpdates 方法中，在 batchedUpdates 中执行更新操作。

但是在 React 18之后，全部都会进行批处理

### 问：React Hooks 的优点是什么

答：

1. 使函数式组件也有了状态和生命周期
2. 解决了 Class 组件中状态逻辑难以复用的问题。
3. 使组件更加清晰易于理解。Hooks 将组件的状态逻辑抽出来,避免了组件和生命周期方法的混杂,简化了组件。

### 问：React Hooks 性能优化

答：

React Hooks 中使用 useMemo 和 useCallback 进行性能优化

useCallback：

```javascript
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```

根据官网的例子可知，当 a, b 两个依赖不变的情况下，memoizedCallback 的引用不变。

即：useCallback 的第一个参数（回调函数）会被缓存，从而达到性能优化的目的。

```javascript
import { useState, useEffect, useRef } from "react";

const usePrevProps = (value) => {
  const ref = useRef();
  useEffect(() => {
    ref.current = value;
  });
  return ref.current;
};

export default function App() {
  const [count, setCount] = useState(0);
  const [total, setTotal] = useState(0);
  const handleCount = () => setCount(count + 1);
  const handleTotal = () => setTotal(total + 1);
  const prevHandleCount = usePrevProps(handleCount);

  console.log("两次处理函数是否相等：", prevHandleCount === handleCount);

  return (
    <div>
      <div>Count is {count}</div>
      <div>Total is {total}</div>
      <br />
      <div>
        <button onClick={handleCount}>Increment Count</button>
        <button onClick={handleTotal}>Increment Total</button>
      </div>
    </div>
  );
}
```

每次 App 组件渲染时这个 handleCount 都是重新创建的一个新函数。

```javascript
console.log("两次处理函数是否相等：", prevHandleCount === handleCount);
```

所以这行会打印的是 false

问题就在于，如果将 handleCount 通过 props 传给其他组件会导致 PureComponent、shouldComponentUpdate、memo 等失效（原因就是函数的引用不同）。

为了解决上述的问题，我们就需要引入 useCallback，通过使用它的依赖缓存功能，在合适的时候将handleCount 缓存起来。

```javascript
import { useState, useEffect, useRef, memo, useCallback } from "react";

const usePrevProps = (value) => {
  const ref = useRef();
  useEffect(() => {
    ref.current = value;
  });
  return ref.current;
};

export default function App() {
  const [count, setCount] = useState(0);
  const [total, setTotal] = useState(0);
	- const handleCount = () => setCount(count + 1);
  + const handleCount = useCallback(() => setCount((count) => count + 1), []);
  const handleTotal = () => setTotal(total + 1);
  const prevHandleCount = usePrevProps(handleCount);

  console.log("两次处理函数是否相等：", prevHandleCount === handleCount);

  return (
    <div>
      <div>Count is {count}</div>
      <div>Total is {total}</div>
      <br />
      <div>
        <button onClick={handleCount}>Increment Count</button>
        <button onClick={handleTotal}>Increment Total</button>
      </div>
      + <OtherComponent onClick={handleCount} />
    </div>
  );
}

const OtherComponent = memo(function OtherComponent({ onClick }) {
  console.log("OtherComponent 组件渲染");
  return <button onClick={onClick}>OtherComponent - Increment Count</button>;
});
```

重点看修改后的 handleCount 方法

```javascript
const handleCount = useCallback(() => setCount((count) => count + 1), []);
```

使用 useCallback 将其包裹起来，当 useCallback 的依赖不变时，那么 handleCount 就会被缓存，当依赖为空数组时，表示这个函数在组件的生命周期内会**永久缓存**。

useMemo

```javascript
useCallback(fn, deps) 相当于 useMemo(() => fn, deps)
```

```javascript
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```

根据官网的例子可知，当 a, b 两个依赖不变的情况下，memoizedValue 的引用不变。

即：useMemo 的第一个参数（回调函数）不会被执行，从而达到性能优化的目的。

useMemo 和 useCallback 99% 相同，不同就在于 useMemo 缓存的是计算后的值（回调函数就不会被执行），从而达到节约内存消耗
