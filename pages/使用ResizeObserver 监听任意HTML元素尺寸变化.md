- [MDN地址](https://developer.mozilla.org/zh-CN/docs/Web/API/ResizeObserver)
- 可以监听任意元素的尺寸变化，比如在react中可以结合ref来监听一个dom元素：
	- ```jsx
	      useEffect(() => {
	          // 第一种，监听元素的宽的变化
	          const ro = new ResizeObserver((entries, observer) => {
	              for (const entry of entries) {
	                  if (entry && entry.contentRect) {
	                      const { width } = entry.contentRect
	                      if (Math.abs(width - document.getElementsByTagName('canvas')[1].offsetWidth) > 5) {
	                          document.getElementsByTagName('canvas')[1].style.width = '100%'
	                      }
	                  }
	              }
	          })
	          ro.observe(chartContainer.current)
	      }, [])
	  ```