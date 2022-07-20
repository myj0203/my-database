- 简单来说CSS-in-JS就是**将应用的CSS样式写在JavaScript文件里面**，而不是独立为一些.css，.scss或者less之类的文件，这样你就可以在CSS中使用一些属于JS的诸如模块声明，变量定义，函数调用和条件判断等语言特性来提供灵活的可扩展的样式定义。
- 方案
	- [[@emotion/css]]
	  [[@emotion/react]]
- [[@emotion/css]] vs [[@emotion/react]]
	- > 总结：
	  普通的样式：`className`需要定义样式时用 [[@emotion/css]] 的css
	  需要用`styled`、全局样式`Global`时用 [[@emotion/react]] 的css
	- `@emotion/css` :
		- use vanilla（原生） react  `className`  prop
		- no custom jsx parser
		- no babel config or file pragmas
		- no extra typescript config
		- generate class names outside of react components
	- `@emotion/react` :
		- zero config for  `@emotion/cache`
		- easy server side rendering
		- easy access to theme through css function
-