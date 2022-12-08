---
title: "React"
tags:
- tech
---

## Hooks
- [Introducing Hooks](https://reactjs.org/docs/hooks-intro.html)
- [puxiao / react-hook-tutorial](https://github.com/puxiao/react-hook-tutorial)
- [Youtube - Codevolution](https://www.youtube.com/playlist?list=PLC3y8-rFHvwisvxhZ135pogtX7_Oe3Q3A)
- Rules of Hooks
	- Only Call Hooks at the Top Level
	- Only Call Hooks from React Functions
#### useEffect
- useEffect 执行时会建立闭包，使用 setState 时需要注意参数的值
	[useEffect 高级用法](https://github.com/puxiao/react-hook-tutorial/blob/master/05%20useEffect%E9%AB%98%E7%BA%A7%E7%94%A8%E6%B3%95.md)
	>有时候，你的 effect 可能会使用一些频繁变化的值。你可能会忽略依赖列表中 state，但这通常会引起 Bug，传入空的依赖数组 []，意味着该 hook 只在组件挂载时运行一次，并非重新渲染时。但如此会有问题，在 setInterval 的回调中，count 的值不会发生变化。因为当 effect 执行时，我们会创建一个闭包，并将 count 的值被保存在该闭包当中，且初值为 0。每隔一秒，回调就会执行 setCount(0 + 1)，因此，count 永远不会超过 1。
- 当 useEffect 闭包无法获取最新的 state 值时，可以考虑使用 useRef
- 组件多次渲染时，在执行下一个 effect 之前，上一个 effect 会被清除
	```js
	useEffect(() => {
	  const subscription = props.source.subscribe();
	  return () => {
		// 清除订阅
		subscription.unsubscribe();
	  };
	});
	```
- 为避免阻塞视觉更新 `useEffect` 在浏览器完成布局与绘制之后的延迟事件中调用，需绘制前同步执行可使用 `useLayoutEffect`
#### useState
- `setState` 为 obj 时不会 auto merge，所以需要使用扩展运算符
	`setState(prev => { ...prev, attr: newVal })`
#### useReducer
- useState is built using useReducer
-  ![react-useStatevsuseReducer.png](/content/notes/images/react-useStatevsuseReducer.png)
#### useCallback
- React.memo() 确保组件只在相关 props 或 state 变化时重渲染
	```jsx
	import React from 'react'
	
	fuction Title () {
		return <h2>Title</h2>;
	}
	
	export default React.memo(Title)
	```
- 组件重渲染时，内部的函数会被重新创建，所以传入函数的子组件也会因为有变化而重渲染
- useCallback 用来 cache 依据传参而变化的函数
#### useMemo
- useCallback cache function, useMemo cache result of function
#### useRef
- 操纵 dom 变化
- as a container to hold mutable value in global
#### custom Hook
- [Doc](https://zh-hans.reactjs.org/docs/hooks-custom.html)
- 以 use 开头的函数，内部包含对其他 Hook 的调用，用于提取公共逻辑
- 自定义 Hook 是一种重用状态逻辑的机制，调用时其中 state 和副作用完全隔离
---
#### 'Slot' in React = props.children
```html
// father-component
<LoadableBtn loading={loading} onClick={submit}>
	Submit
</LoadableBtn>
```

```html
// sub-component
<div className="loadable-btn-inner">
	{props.children}
</div>
```
---
#### Airbnb React/JSX Style Guide
- [Doc](https://github.com/airbnb/javascript/tree/master/react)
- propTypes and defaultProps of React component
	```jsx
	// good
	function SFC({ foo, bar, children }) {
	  return <div>{foo}{bar}{children}</div>;
	}
	SFC.propTypes = {
	  foo: PropTypes.number.isRequired,
	  bar: PropTypes.string,
	  children: PropTypes.node,
	};
	SFC.defaultProps = {
	  bar: '',
	  children: null,
	};
	```
- Filter out unnecessary props when possible
	```jsx
	// good
	render() {
	  const { irrelevantProp, ...relevantProps } = this.props;
	  return <WrappedComponent {...relevantProps} />
	}
	```