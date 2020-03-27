# React API REFERENCE

[React 官网](https://zh-hans.reactjs.org/)

- ES6+npm 引入 React

  - `import React from 'react'`引入 React

- ES5+npm 引入 React
  - `var React = require('react')`

## 创建 React 组件

- React.Component
- React.PureComponent
  - React 组件被赋予相同的 props 和 state 时，render()会渲染相同的内容，那么在某些情况下使用该组件可提高性能
- 不使用 ES6 的 class，可以使用`create-react-class`来替代
- 两者区别
  - React.PureComponent 实现了`shouldComponentUpdate()`，而 React.Component 没实现
- React.memo，高阶组件，适用于函数组件，不适用于 class 组件，与 PureComponent 类似。

  - 如果函数组价在给定 props 的情况下渲染相同的结果，就可以将其包装在 memo 中调用，React 在渲染组件时，会直接复用最近一次的渲染结果，提高组件的性能表现
  - React.memo 仅检查 props 变更。且其实现中拥有 useState 或 useContext 的 Hook，当 context 发生变化时，它仍会渲染
  - 默认情况下只会对其复杂对象做浅层对比，如果想要控制对比过程，需要自定义比较函数传入第二个参数
  - 此方法只能作为性能优化方式存在，不能用来阻止渲染，会出bug
  - **注意**：与 class 组件中 shouldComponentUpdate() 方法不同的是，如果 props 相等，areEqual 会返回 true；如果 props 不相等，则返回 false。这与 shouldComponentUpdate 方法的返回值相反。

  ```javascript
  //const MyComponent = React.memo(function MyComponent(props)){
      //使用props渲染
  //}

  function MyComponent(props) {
      //使用props渲染
  }

  function areEqual(prevProps, nextProps) {
    /*
    如果把 nextProps 传入 render 方法的返回结果与
    将 prevProps 传入 render 方法的返回结果一致则返回 true，
    否则返回 false
    */
  }
  export default React.memo(MyComponent, areEqual);
  ```

## 创建 React 元素

- 推荐使用 JSX 来编写 UI 组件，每个 JSX 元素都是调用 React.createElement()的语法糖

## 转换元素

- 几个操作元素的 API

  - cloneElement(): 以`element`元素为样板克隆并返回新的 React 元素，返回的 props 是将新的 props 与原始元素 props 浅层合并后的结果
  - isValidElement()：验证对象是否为 React 元素，返回值为 true 或 false`React.isValidElement(object)`
  - `React.Children`，提供了用于处理 this.props.children 不透明数据结构的实用方法

    - React.Children.map，返回一个 React 数组

      ```JavaScript
      React.Children.map(children, functon[(thisArg)]);
      ```

      - 在 children 里每个直接子节点上调用一个函数，并将 this 设置成 thisArg。如果 children 是一个数组，它将被遍历并为每个数组中的每个子节点调用该函数。如果子节点为 null 或是 undefined，则此方法将返回 null 或是 undefined，而不会返回数组

      - 如果 children 是一个 Fragment 对象，它将被视为单一子节点情况处理，而不会被遍历

    - React.Children.forEach 同 React.Children.map

    ```javascript
    React.Children.forEach(children, function[(thisArg)])
    ```

    - React.Children.count，返回 children 中组件的总数量，等同于 map 或 forEach 调用回调函数的次数

    ```javascript
    React.Children.count(children);
    ```

    - React.Children.only，验证 children 是否只有 1 个子节点（一个 React 元素），如果有则返回该节点，否则会报错
      - **注意**：React.Children.only 不接受 React.Children.map 的返回值，因为它是一个数组而不是 React 元素

    ```javascript
    React.Children.only(children);
    ```

    - React.Children.toArray
      - 将 children 以数组的方式返回，并且为每一个子节点分配一个 key。
      - 当你想要在渲染函数中操作子节点的集合时，会比较实用

    ```javascript
    React.Children.toArray(children);
    ```

## Fragments

- 用来减少不必要的嵌套

  - HTML 可能是这样写

    ```html
    Some text.
    <h2>A heading</h2>
    More text.
    <h2>Another heading</h2>
    Even more text.
    ```

  - React 可以用 Fragment 来包裹

    ```javascript
    render() {
    return (
        <Fragment>
        Some text.
        <h2>A heading</h2>
        More text.
        <h2>Another heading</h2>
        Even more text.
        </Fragment>
    );
    }
    ```

## Refs

- 给组件添加 ref，React.createRef

```javascript
class MyComponent extends React.Component {
    constructor(props){
        super(props);
        this.inputRef = React.createRef();
    }

    render() {
        return <input type="text" ref={this.inputRef}>
    }

    componentDidMount() {
        this.inputRef.current.focus();
    }
}
```

- React.forwardRef，ref 转发（还不太理解）

  - 适用场景
    - 转发 refs 到 DOM 组价
    - 在高阶组件中转发 refs

```javascript
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));

// You can now get a ref directly to the DOM button:
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;
```

- 在上述的示例中，React 会将`<FancyButton ref={ref}>` 元素的 ref 作为第二个参数传递给 React.forwardRef 函数中的渲染函数。该渲染函数会将 ref 传递给 `<button ref={ref}>` 元素。
  因此，当 React 附加了 ref 属性之后，ref.current 将直接指向 `<button>` DOM 元素实例。

## Suspense：使得组件等待某些操作后，再进行渲染

- 适用场景

  - 使用 React.lazy 动态加载组件

- React.lazy

  - 允许动态加载一个组件，可以帮助缩减 bundle 的体积，延迟加载在初次渲染时未用到的组件

  ```javascript
  const SomeComponent = React.lazy(() => import("./SomeComponent"));
  ```

  - **注意**：渲染 lazy 组件依赖该组件渲染树上层的`<React.Suspense>`组件。

  - 类似 vue 的延迟加载组件

  ```javascript
  //router.js
  //第一种 require加载
  {
      path: '/test/index',
      name: 'index',
      component: resolve => require(['../views/index.vue'], resolve)
  }

  //第二种 import加载
  {
      path: '/test/index',
      name: 'index',
      component: () => import('../views/index.vue')
  }
  ```

- React.Suspense

  - 可以指定加载指示器（loading indicator）。目前，懒加载组件是`<React.Suspense>`支持的唯一用例：

  ```javascript
  const OtherComponent = React.lazy(() => import("./OtherComponent"));

  function MyComponent() {
    return (
      //显示<Spinner>组件直至 OtherComponent 加载完成
      <React.Suspense fallback={<Spinner />}>
        <div>
          <OtherComponent />
        </div>
      </React.Suspense>
    );
  }
  ```
