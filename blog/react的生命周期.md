# react组件生命周期

### 什么是组件生命周期

- 首先说明什么是组件

  组件是指在UI界面中，可以被独立划分的、可复用的、独立的模块。每个组件都有自己独立的生命周期。

- 组件生命周期是指什么

  每个组件都有自己的props和state。
  每一个组件都有几个你可以重写以让代码在处理环节的特定时期运行的“生命周期方法”。方法中带有前缀 will 的在特定环节之前被调用，而带有前缀 did 的方法则会在特定环节之后被调用。


### react的生命周期有哪些

下面这些方法会在组件实例被创建和插入DOM中时被调用

- `constructor`
- `static getDerivedStateFromProps()`
- `componentWillMount()/ UNSAFE_componentWillMount()`
- `render()`
- `componentDidMount()`

属性或状态的改变会触发一次更新。当一个组件在被重渲时，这些方法将会被调用
- `componentWillReceiveProps() / UNSAFE_componentWillReceiveProps()`
- `static getDerivedStateFromProps()`
- `shouldComponentUpdate()`
- `componentWillUpdate() / UNSAFE_componentWillUpdate()`
- `render()`
- `getSnapshotBeforeUpdate()`
- `componentDidUpdate()`

当一个组件被从DOM中移除时，该方法被调用

- `componentWillUnmount()`

渲染中出现错误，调用下面方法

- `componentDidCatch()`

每一个组件还提供了其他的API

- `setState()`
- `forceUpdate()`

实例属性

- `props`
- `state`


### 组件生命周期图片预览

![生命周期](https://raw.githubusercontent.com/lixiaraodd01/lisa/master/images/component_lifecycle_01.jpg)



### 一个组件中必须要用到的生命周期方法

```js
  render()
``` 

当被调用时，其应该检查`this.props` 和 `this.state`并返回以下类型中的一个
- React元素。 通常是由 JSX 创建。该元素可能是一个原生DOM组件的表示，如`<div />`，或者是一个你定义的合成组件。
- 字符串和数字。 这些将被渲染为 DOM 中的 text node。 `return 'aaa'`
- null。 什么都不渲染
- Protal. `reactDOM.creatProtal`创建的对象。 
  新API，在一般的 React 结构中，组件的嵌套关系和渲染出来的 DOM 的嵌套关系是一致的（子组件渲染出的 DOM 一定是在父组件渲染出的 DOM 的内部的）。但某些情况下，这样的限制会导致问题，例如实现一个模态框（Modal），虽然模态框所在的组件在它的父组件内部，但是通常需要被渲染在 body 元素下。  [文档参考](https://reactjs.org/docs/portals.html)

  ```js
    render() {
      // React does *not* create a new div. It renders the children into `domNode`.
      // `domNode` is any valid DOM node, regardless of its location in the DOM.
      return ReactDOM.createPortal(
        this.props.children,
        domNode,
      );
    }
  ```

- 布尔值。 什么都不渲染。通常存在于` return test && <Child />`写法，其中 test 是布尔值。

- 数组. dom数组。之前的版本`render` 多个元素需要用`<div></div>`包起来。现在可一直使用

```jsx
render() {
  return [
    <li key="key_1"> 1</li>,
    <li key="key_2"> 2</li>,
    <li key="key_3"> 3</li>,
  ]
}
```
> *若 shouldComponentUpdate()返回false，render()函数将不会被调用。*



### 生命周期方法详细说明

一.  组件装载过程中(Mount)生命周期有以下几个


```js 
  constructor()
```

实例化之前调用。当为一个React.Component子类定义构造函数时，你应该在任何其他的表达式之前调用super(props)。否则，this.props在构造函数中将是未定义，并可能引发异常。
如果不需要初始化状态且不绑定方法，那你也不需要为你的React组件定义一个构造函数。

```js
  static getDerivedStateFromProps()
```

组件实例化后和接受新属性时将会调用`getDerivedStateFromProps`。它应该返回一个对象来更新状态，或者返回null来表明新属性不需要更新任何状态。

*注意，如果父组件导致了组件的重新渲染，即使属性没有更新，这一方法也会被调用。如果你只想处理变化，你可能想去比较新旧值。调用this.setState() 通常不会触发 getDerivedStateFromProps()。*

```js
  componentWillMount()/ UNSAFE_componentWillMount()
```

在此函数中设置`state`也不会生效。因为这个方法在`render()`前调用，不会再次触发`render()`重新渲染。所以不建议异步数据请求放在此方法中。
`componentWillMount()`是v16.3及之前版本可以用。v16.3使用别名`UNSAFE_componentWillMount()`。v17版前都还可以使用。


```js
  render()
```

前面已经详细描述了

```js
  componentDidMount()
```

初始化DOM节点render真实DOM前会执行这个方法。官方推荐数据加载，`ajax` `fetch`数据等放在这个方法中。在这个function中`setState`会重新触发`render`。在浏览器渲染时，用户察觉不到2次render。


二.  组件更新(update)

```js
  componentWillReceiveProps(nextProps) / UNSAFE_componentWillReceiveProps(nextProps)
```

该函数在组件进行更新以及父组件`render`函数（不管数据是否发生了改变）被调用后执行，`this.props`取得当前的`props`，`nextProps`传入的是要更新的`props`。通常使用`this.props` 和 `nextProps` 进行比较进行其他逻辑处理
此属性在v16.3版本提出删除，官方建议使用`getDerivedStateFromProps()`
这一生命周期之前叫做`componentWillReceiveProps`。这一名字在17版前都有效。

```js
  static getDerivedStateFromProps()
```

组件实例化后和接受新属性时将会调用`getDerivedStateFromProps`。它应该返回一个对象来更新状态，或者返回null来表明新属性不需要更新任何状态。

调用`this.setState()` 通常不会触发 `getDerivedStateFromProps()`。

*注意，如果父组件导致了组件的重新渲染，即使属性没有更新，这一方法也会被调用。如果你只想处理变化，你可能想去比较新旧值。*

```js
  shouldComponentUpdate(nextProps, nextState)
```

返回bool值，true表示要更新，false表示不更新，使用得当将大大提高React组件的性能，避免不需要的渲染。
该方法并不会在初始化渲染或当使用`forceUpdate()`时被调用

```js
  componentWillUpdate(nextProps, nextState)
```

初始化不会调用。更新渲染前被立即调用。不能在此方法`this.setState()`。react v17版被删除

```js
  getSnapshotBeforeUpdate(prevProps, prevState)
```

更新重新渲染render后DOM输出前调用。在异步渲染中，优化有很大的作用。

```js
  componentDidUpdate(prevProps, prevState)
```

同初始化这个`componentDidMount`。此方法在组件更新中触发。


三、卸载中(unmounting)

```js
  componentWillUnmount()
```
在组件被卸载和销毁之前立刻调用。可以在该方法里处理任何必要的清理工作，例如解绑定时器，取消网络请求，
清理任何在componentDidMount环节创建的DOM元素。

四、 错误处理

```js
  componentDidCatch(error, info)
```
组件render出错catch错误的一个函数。
是指在子组件树的任何地方捕获JavaScript错误的响应组件，记录这些错误，并显示回退的UI，而不是崩溃的组件树。错误catch在绘制过程中捕获错误，在生命周期方法中，以及在它们下面的整棵树的构造函数中捕获错误。




### 项目中经常那些生命周期乱用

在商业后台项目中用的最多的生命周期函数是`componentWillReceiveProps()`和`componentDidMount()`,
还用过`componentWillMount()`。

`componentWillReceiveProps()``componentWillReceiveProps()`和`componentDidMount()` 使用没问题。`componentWillReceiveProps()`多用于路由切换或子组件中重新设置`state`。react v16.3版本后 建议使用 `static getDerivedStateFromProps()`。

`componentDidMount()`用于异步数据请求。setState后会重新渲染。


错误用法指出

- dva中 在`componentWillReceiveProps()`生命周期函数中dispatch数据，导致请求循环。因设置了dva 中state会出发重新渲染，导致进入死循环。

- tree.js中
`componentWillMount()`使用在设置静态state参数，动态设置是没用的。正确做法应该可以直接在state中直接设置。


### 生命周期一览显示代码

```js
container.js

import React from "react";
import ReactDOM from "react-dom";
import LifeCycle from './lifecycle';
import styles from './index.css';
class Container extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      num: Math.random()*100, 
    };
  }
  propsChange() {
    this.setState({
      num: Math.random()*100
    });
  }

  setLifeCycleState() {
    this.refs.rLifeCycle.setTheState();
  }

  forceLifeCycleUpdate() {
    this.refs.rLifeCycle.forceItUpdate();
  }

  unmountLifeCycle() {
    ReactDOM.unmountComponentAtNode(document.getElementById('index'));
  }

  parentForceUpdate() {
    this.forceUpdate();
  }

  render() {
    return [
      <a key="propschange-button" className={styles.button} onClick={this.propsChange.bind(this)}>propschange</a>,
      <a key="setState-button" className={styles.button} onClick={this.setLifeCycleState.bind(this)}>setState</a>,
      <a key="forceUpdate-button" className={styles.button} onClick={this.forceLifeCycleUpdate.bind(this)}>forceUpdate</a>,
      <a key="unmount-button" className={styles.button} onClick={this.unmountLifeCycle.bind(this)}>unmount</a>,
      <a key="parentForceUpdateWithoutChange-button" className={styles.button} onClick={this.parentForceUpdate.bind(this)}>parentForceUpdateWithoutChange</a>,
      <LifeCycle key="lifecycle" ref="rLifeCycle" num={this.state.num}></LifeCycle>,
    ]
  }

}

export default Container;

```


```js
lifecycle.js

import React from "react";
import ReactDOM from "react-dom";

class Lifecycle extends React.Component {
  constructor(props) {
    // 组件初始化
    console.log('initial render');
    console.log('1. constructor');
    super(props);
    this.state = {
      str: 'hello'
    }
  }
  // static getDerivedStateFromProps(nextProps, prevState) {
  //   console.log('2. this is getDerivedStateFromProps function');
  //   console.log('getDerivedStateFromProps nextprops:', nextProps);
  //   console.log('getDerivedStateFromProps prevState:', prevState);
  //   return {str: nextProps.str };
  // }

  componentWillMount() {
    this.setState({str: 'hello world!!'})
  }

  componentDidMount() {
    console.log('4. componentDidMount function');
  }

  UNSAFE_componentWillReceiveProps(nextProps) {
    console.log('5. UNSAFE_componentWillReceiveProps');
    console.log('UNSAFE_componentWillReceiveProps:', nextProps);
  }
  shouldComponentUpdate(nextProps, nextState) {
    // 不会在初始化渲染或当使用forceUpdate()时被调用。
    // 如果shouldComponentUpdate()返回false，而后UNSAFE_componentWillUpdate()，render()， 和 componentDidUpdate()将不会被调用。
    // 这个方法一般用作 react渲染性能优化。详细请查阅  Component 和 PureComponent 
    console.log('shouldComponentUpdate');
    return true;
  }

  componentWillUpdate() {
    console.log('componentWillUpdate');
  }

  // v16.4
  // getSnapshotBeforeUpdate() {
  //   console.log('getSnapshotBeforeUpdate');
  // }

  componentDidUpdate() {
    console.log('componentDidUpdate');
  }

  componentWillUnmount() {
    console.log('componentWillUnmount');
  }

  forceItUpdate() {
    this.forceUpdate();
  }

  setTheState() {
    let words = 'hello react';
    if(this.state.str === words) {
      words = 'HELLO REACT';
    }
    this.setState({
      str: words
    });
  }

  render() {
    console.log('3. render function');
    return (
      <div>
        <p className="lifecycle-p">show props: <em>{parseInt(this.props.num)}</em></p>
        <p className="lifecycle-p">show state: <em>{this.state.str}</em></p>
      </div>
    );
  }

}


export default Lifecycle;

```

### 参考文档
- [react component](https://reactjs.org/docs/react-component.html)
