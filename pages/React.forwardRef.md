- `React.forwardRef`  会创建一个React组件，这个组件能够将其接受的 [ref](https://zh-hans.reactjs.org/docs/refs-and-the-dom.html) 属性转发到其组件树下的另一个组件中。这种技术并不常见，但在以下两种场景中特别有用：
	- [转发 refs 到 DOM 组件](https://zh-hans.reactjs.org/docs/forwarding-refs.html#forwarding-refs-to-dom-components)
	- [在高阶组件中转发 refs](https://zh-hans.reactjs.org/docs/forwarding-refs.html#forwarding-refs-in-higher-order-components)
- `React.forwardRef`  接受渲染函数作为参数。React 将使用  `props`  和  `ref`  作为参数来调用此函数。此函数应返回 React 节点。
	- ```jsx
	  const FancyButton = React.forwardRef((props, ref) => (
	    <button ref={ref} className="FancyButton">
	      {props.children}
	    </button>
	  ));
	  
	  // You can now get a ref directly to the DOM button:
	  const ref = React.createRef();
	  <FancyButton ref={ref}>Click me!</FancyButton>;
	  ```
	- > 在上述的示例中，React 会将  `<FancyButton ref={ref}>`  元素的  `ref`  作为第二个参数传递给  `React.forwardRef`  函数中的渲染函数。该渲染函数会将  `ref`  传递给  `<button ref={ref}>`  元素。
	  因此，当 React 附加了 ref 属性之后， `ref.current`  将直接指向  `<button>`  DOM 元素实例。