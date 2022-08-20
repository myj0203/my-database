- 本文主要介绍ts一些高级语法、可能之前大家用的都不是很多，希望通过这篇文章能带大家提升对ts的认知
- 为了让大家觉得这些高级用法不只是躺在文档里面的生硬文字，我们设立一个场景方便大家更好的消化和理解：
	- > 比如日常我们会把闲置品挂在闲鱼上交易或者去拍别人的商品，商品的基本信息都是一样的：
	- ```
	  interface IGoodsInfo {
	  	name: string;
	  	price: number;
	  }
	  ```
	- ## 枚举类型
		- > 需求：除了基本信息，我们还希望区分商品的类型，比如一个商品是属于电子产品、服饰或书籍等等
		- 增加type字段
		  ```
		  enum GoodType {
		  	ElectronicProducts = 'ElectronicProducts',
		  	Clothing = 'Clothing',
		  	Books = 'Books',
		  }
		  
		  interface IGoodsInfo {
		      name: string;
		      price: number;
		      type: keyof typeof GoodType;
		  }
		  
		  function addGoods(good: IGoodsInfo) {
		  	console.log(good)
		  }
		  
		  addGoods({ 
		      name: 'iphone',
		      price: 8888,
		      type: GoodType.ElectronicProducts
		  });
		  
		  ```
-
-