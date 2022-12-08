---
title: "Styled-Component"
tags:
- frontend
---

[styled-components doc](https://styled-components.com/docs)

## Basics
- styled-component  = styled React component
	>It removes the mapping between components and styles. This means that when you're defining your styles, you're actually creating a normal React component, that has your styles attached to it.
- styled.div VS style.section
	```html
	// styled.div
	<div class="xxx"/>
	
	// styled.section
	<section class="xxx" />
	```
- ["as" polymorphic prop](https://styled-components.com/docs/api#as-polymorphic-prop)
	实际渲染为 as={Component} 内容，但复用 WrapperComponent 样式
	```js
	const Button = styled.button`
	  display: inline-block;
	  color: palevioletred;
	  font-size: 1em;
	  margin: 1em;
	  padding: 0.25em 1em;
	  border: 2px solid palevioletred;
	  border-radius: 3px;
	  display: block;
	`;
	
	const ReversedButton = props => <Button {...props} children={props.children.split('').reverse()} />
	
	render(
	  <div>
		<Button>Normal Button</Button>
		<Button as={ReversedButton}>Custom Button with Normal Button styles</Button>
	  </div>
	);
	```
- 在 render 函数外定义 styled-component，否则在每次 render 时都会重新创建
	>It is important to define your styled components outside of the render method, otherwise it will be recreated on every single render pass. Defining a styled component within the render method will thwart caching and drastically slow down rendering speed, and should be avoided.
- 使用 `stylis` 预处理器，支持类 scss 语法
- [Attaching additional props](https://styled-components.com/docs/basics#attaching-additional-props)
	- attrs 中可使用函数返回结果 + 动态 props
		```js
		const Input = styled.input.attrs(props => ({
		  // we can define static props
		  type: "text",
		
		  // or we can define dynamic ones
		  size: props.size || "1em",
		}))`
		  color: palevioletred;
		  font-size: 1em;
		  border: 2px solid palevioletred;
		  border-radius: 3px;
		
		  /* here we use the dynamically computed prop */
		  margin: ${props => props.size};
		  padding: ${props => props.size};
		`;
		
		render(
		  <div>
		    <Input placeholder="A small text input" />
		    <br />
		    <Input placeholder="A bigger text input" size="2em" />
		  </div>
		);
		```
	- attrs 可被 override，越精准优先级越高

## Advanced
- Theming
	- 可使用 Props 传参或函数，实现主题色变化或 revert
	- 外层组件可使用 `withTheme` `useContext` `useTheme` 等方式获取 theme 信息
- Refs
	- 给 styled-component 传入 ref 参数可获得当前 Dom 节点或对应 React component 实例
- Together with existing CSS
	styled-components 生成实际的样式表并且通过 `className` 属性附加到 DOM 节点中
	- 使用 `styled(MyComponent)` 并使用 `this.props.className` 将样式表传入 DOM 节点
		```js
		class MyComponent extends React.Component {
		  render() {
		    // Attach the passed-in className to the DOM node
		    return <div className={`some-global-class ${this.props.className}`} />
		  }
		}
		```
	- 当 styled-components 定义的样式属性与全局冲突时，由于 styled componens class 会被插入至 DOM 节点末端所以会占有较高的优先级
	- 避免与第三方样式冲突：外层设置命名空间
- Tagged Template Literals
	- Tagged Template Literals in ES6
		```js
		// These are equivalent:
		fn`some string here`;
		fn(['some string here']);
		
		// These are equivalent:
		const aVar = 'good';
		fn`this is a ${aVar} day`;
		fn(['this is a ', ' day'], aVar);
		```
	- 可以使用 && 因为 styled-components 会忽略返回为 undefined, null, false, or ""
		```js
		const Title = styled.h1`
		  /* Text centering won't break if props.upsidedown is falsy */
		  ${props => props.upsidedown && 'transform: rotate(180deg);'}
		  text-align: center;
		`;
		```
- [Referring to other components](https://styled-components.com/docs/advanced#referring-to-other-components)