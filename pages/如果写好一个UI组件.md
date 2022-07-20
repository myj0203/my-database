- 每个`自定义节点`都定义一个组件库前缀的className，方便使用的时候覆盖样式
	- e.g
		- ```jsx
		      <Root type={type} image={image} sx={sx}>
		        <div className="ImileConfirm-title">{title}</div>
		        <div className="ImileConfirm-subtitle">{subtitle}</div>
		        <div className="ImileConfirm-body">{body}</div>
		        <div className="ImileConfirm-button">
		          <SubmitButton as={Button} {...SubmitButtonProps} onSubmit={onSubmit}>
		            {okText}
		          </SubmitButton>
		          <Button onClick={onCancel}>{cancelText}</Button>
		        </div>
		      </Root>
		  ```
	-