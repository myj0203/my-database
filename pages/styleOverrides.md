- ```jsx
  components：{
    组件名称 || root: {
  	variants: [ // 
        {
          props: {variant: 'xxx'} // 类似type，同个组件不同的功能
          style: {...}
        }
  	]，
      defaultProps: {
        // 改变默认的props
      },
      styleOverrides: {
        组件名称 || root: {
          // 样式
        }
      }
    }
  }
  ```