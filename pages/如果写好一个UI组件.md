title:: 如果写好一个UI组件

- 每个`自定义节点`都定义一个组件库前缀的className，方便使用的时候覆盖样式
	- e.g
		- ```jsx
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
		  ```
	-
-