- 本文主要介绍ts一些高级语法、可能之前大家用的都不是很多，希望通过这篇文章能带大家提升对ts的认知
- 为了让大家觉得这些高级用法不只是躺在文档里面的生硬文字，我们设立一个场景方便大家更好的消化和理解：
	- > 比如日常我们会把闲置品挂在闲鱼上交易或者去拍别人的商品，商品的基本信息都是一样的：
	- ```tsx
	  interface IGoodsInfo {
	  	name: string;
	  	price: number;
	  }
	  ```
	- ## 枚举类型
		- > 需求：除了基本信息，我们还希望区分商品的类型，比如一个商品是属于电子产品、服饰或书籍等等
		- 增加type字段
		  ```tsx
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
- ## keyof 关键字
	- > 需求：有些贵重商品需要线下交易、方便验货等
	- `keyof`  有点像  `Object.keys`  不过  `keyof`  取的是  `interface`  的键（Key）,比如下面这样：
	- ```tsx
	  interface IPoint {
	    x: number;
	    y: number;
	  }
	  // 等同于 type Axis = "x" | "y"
	  type Axis = keyof IPoint;
	  ```
	- 计算线段中点坐标值，具体哪个坐标轴可选
	  ```tsx
	  function cal(a: IPoint, b: IPoint, axis: Axis): number {
	    return (a[axis] + b[axis]) / 2;
	  }
	  ```
## as 类型断言
	- > 需求：为了代码复用，我们希望原来的JS代码能迁移到TS中
	- ```tsx
	  const foo = {};
	  foo.bar = 123; // Error: 'bar' 属性不存在于 ‘{}’
	  foo.bas = 'hello'; // Error: 'bas' 属性不存在于 '{}'
	  ```
	- 使用 as 关键字，断言类型：
	- ```tsx
	  interface Foo {
	       bar: number;
	       bas: string;
	  }
	  const foo = {} as Foo;
	  foo.bar = 123;
	  foo.bas = 'hello';
	  ```
	- > 需要注意的是，除了代码迁移用到 as 语法，大部分其他场景应该避免使用，类型断言纯粹是编译时语法。类型推断应该尽量使用 interface / type / 基础类型。（断言被认为是有害的）
- # 工具泛型
	- Typescript 提供的工具泛型源码可以在  `lib.es5.d.ts`  文件上找到，对常见类型判断需求进行封装，我们可以直接使用，省去自行编写维护的麻烦。
	- ## Partial
		- > 需求: 很多用户会在闲鱼上分享心得，支持用户修改更新，但不希望全部字段都更新
		- `Partial` 将传入的所有属性变为可选
		  ```tsx
		  type Partial<T> = { [P in keyof T]?: T[P] };
		  ```
		- 比如更新一篇文章，并不需要更新全部字段：
		- ```tsx
		  interface Article {
		      articleId: string;
		      title: string;
		      content: string;
		      status: number;
		  }
		  function update(article: Partial<Article>) {
		      // .. do update;
		  }
		  update({ articleId: '123', title: 'title '});
		  
		  // Partial<Article> 等价为以下接口，所有字段都是可选的
		  interface Article1 {
		      articleId?: string;
		      title?: string;
		      content?: string;
		      status?: number;
		  }
		  ```
## Required
	- 将传入的所有属性变为必选项，这个和 Partial 相反，不举例了。
	- ```tsx
	  type Required<T> = { [P in keyof T]-?: T[P] };
	  ```
## Readonly
	-