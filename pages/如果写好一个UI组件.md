## 通用规则
	- 每个组件内部的大部分有布局含义的 dom 都应该加 `className`，方便用户覆盖样式
	- 每个涉及 UI 的组件都应该接收 `className` 并且合并后传入最外层 dom
	- 全量传递 props 需谨慎，避免传递不必要的 props，通常解构出子组件不需要的 props，再把剩余 props 传给子组件
	- 每个组件都应该导出一个 Props 类型，如 Xxx 组件应该导出一个 XxxProps
## 开发者角度
- 如果你希望编写的组件变得足够通用，适应各个场景时，如：
	- 1. 一个大组件是很多小组件的上层封装，又不希望封装的太死导致只能在一个地方使用
	- 2. 一个组件在不同场景下的样式很不一样，但其他逻辑不变
	- 请参考如下步骤：
		- ### 支持全局覆盖组件默认 props
		  collapsed:: true
			- 在  `packages/components/src/styles/props.d.ts`  里维护上  `组件 => 组件 Props`  的映射，用于全局主题覆盖组件 props。
			  然后在组件内部使用  `useThemeProps`  得到合并『用户输入 props』+『 主题内覆盖的组件默认 props』 后的 props，如编写一个 Hello组件，应该这样写：
			  ```jsx
			  import React from 'react';
			  import { useThemeProps } from '../styles';
			  
			  export interface HelloProps {
			    // ...
			  }
			  
			  export const Hello: React.FC<HelloProps> = inputProps => {
			    const props = useThemeProps({ name: 'ImileHello', props: inputProps })  // 合并后的 props
			  
			  
			    return <>...</>
			  }
			  ```
		- ### 支持全局覆盖样式的组件
		  collapsed:: true
			- 在  `packages/components/src/styles/overrides.d.ts`  里加上  `组件 => 组件可被覆盖的 class 的联合类型`  的映射，用于全局主题覆盖组件 props。
			-
			-
			-
			-
		- ### 支持接收 props.classes
		  collapsed:: true
			- 当一个组件由好几个小组件组成，但用户使用组件时只是简单的使用根组件，那么如何允许用户自定义每个小组件的 className 呢？
			  可以这样：允许用户可以传入一个 classses 对象，用于分发给各个小组件的 className，从而允许用户自由地覆盖样式。利用上一步在  `packages/components/src/styles/overrides.d.ts`  里维护的映射。
			- ### 完整参考 demo
			  ```jsx
			  import { useUtilityClasses, useThemeProps, styled } from '../styles';
			  import clsx from 'clsx';
			  
			  const Root = styled('div', {
			    name: 'ImileHello',
			    slot: 'Root',
			    overridesResolver: (props, style) => [style.root] // 允许被主题上的 styleOverrides.root 覆盖
			  })({})
			  
			  export const Hello = inputProps => {
			    const props = useThemeProps({ name: 'ImileHello', props: inputProps })  // 合并后的 props
			    const classes = useUtilityClasses({
			      name: 'ImileHello',
			      classes: props.classes,
			      slots: [
			        'root'
			        // ...自定义 slot（小组件）
			      ]
			    })
			  
			    return (
			      <Root className={clsx(classes.root, props.className)}>
			        // ...
			      </Root>
			    )
			  }
			  ```
-