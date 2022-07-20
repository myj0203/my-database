- ###  [[cloneElement()]]
	- ```jsx
	  React.cloneElement(
	    element,
	    [config],
	    [...children]
	  )
	  ```
	- 以  `element`  元素为样板克隆并返回新的 React 元素。 `config`  中应包含新的 props， `key`  或  `ref` 。返回元素的 props 是将新的 props 与原始元素的 props 浅层合并后的结果。新的子元素将取代现有的子元素，如果在  `config`  中未出现  `key`  或  `ref` ，那么原始元素的  `key`  和  `ref`  将被保留。
	  collapsed:: true
	  `React.cloneElement()`  几乎等同于：
		- ```jsx
		  <element.type {...element.props} {...props}>{children}</element.type>
		  ```
	- ### 使用场景
		- 把一个未知的react组件，赋上新的props，返回一个新的组件，比如下面这段代码，可以给[[Spin]]传任意的容器（图标）
		  代码`第14行`
			- ```jsx
			  export const Spin = React.forwardRef<HTMLDivElement, SpinProps>((inputProps, ref) => {
			    const {
			      spinning = true,
			      tip,
			      indicator = <Loading />,
			      indicatorSx,
			      className,
			      children,
			      ...props
			    } = useThemeProps({ name: 'ImileSpin', props: inputProps })
			  
			    const loadingNode = (
			      <LoadingBox className="ImileSpin-loadingBox">
			        {React.cloneElement(indicator, { sx: indicatorSx })}
			      </LoadingBox>
			    )
			  
			    return (
			      // @ts-ignore
			      <Root
			        {...props}
			        className={cx('ImileSpin-root', className)}
			        ownerState={{ hasChildren: !!children }}
			        ref={ref}
			      >
			        {children ? (
			          <>
			            {spinning && (
			              <LoadingMask className="ImileSpin-loadingMask">
			                {loadingNode}
			                {tip && <Tip className="ImileSpin-tip">{tip}</Tip>}
			              </LoadingMask>
			            )}
			            <Mask spinning={spinning} className="ImileSpin-mask">
			              {children}
			            </Mask>
			          </>
			        ) : (
			          loadingNode
			        )}
			      </Root>
			    )
			  })
			  ```
	-
	-
	-
-
-