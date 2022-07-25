- 以一个类型为基础支持剔除某些属性，然后返回一个新类型。
	- ```jsx
	  type Person = {
	      name: string;
	      age: string;
	      location: string;
	  };
	  
	  type PersonWithoutLocation = Omit<Person, 'location'>;
	  
	  // PersonWithoutLocation equal to QuantumPerson
	  type QuantumPerson = {
	      name: string;
	      age: string;
	  };
	  
	  ```
- https://zhuanlan.zhihu.com/p/397398407