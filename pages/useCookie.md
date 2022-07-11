- 首先想好useCookie 要暴露的方法
	- 获取Cookie
	- 更新Cookie
	- 删除Cookie
- 根据第一步，我们先写好框架
  ```tsx
  const useCookie = () => {
  
  	return [cookie, updateCookie, deleteCookie]
  }
  ```
-
-