- 翻译：变种，类似type，一个组件，功能一样，可以有不同的样式，最经典的就是Button，通过variants区分
- 用法：
  ```jsx
    variants: [
          {
            props: { variant: 'dashed', color: 'primary' },
            style: {
              border: '1px dashed darkblue',
            },
          },
          {
            props: { variant: 'dashed', color: 'secondary' },
            style: {
              border: '1px dashed darkred',
            },
          },
        ],
  ```
	- > 这样，当`Button`组件这样用时`<Button variant='dashed' color='primary' />` 就能得到一个边框属性为 `1px dashed darkblue` 的 Button，相当于拓展了`Button`组件
	-