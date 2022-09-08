- 渐进式拆分Module
	- [Example](https://www.remnote.com/doc/IaiXC15z9TIsuz8oT)
		- 1. 有个页面，一开始只是个没有弹框的页面，所以创建了一个index.module.ts，可能伴随着一个index.service.ts
		- 2. 后面加了一个弹框，此时弹框的逻辑不是很复杂，所以继续在index.module.ts（此时 500 行）里迭代
		- 3. 后面弹框内容越来越多，有 2 种选择：
			- collapsed:: true
			  1. 由于迭代时间紧张，没精力拆分module，暂时继续在index.module.ts里开发『开发后变 800 行』
				- 提测或上线后，抽空把index.module.ts里的关于弹框的逻辑都剥离到apply.module.ts（假设这里是一个申请xxx功能）
					- 剥离步骤：
					  collapsed:: true
						- 1. 新建apply.module.ts（目标从index.module.ts中剥离 300 行）
						- 2. **只剥离effect/reducer，先保留state在index.module.ts 不变**，相当于把申请相关的effect/reducer逻辑外包到apply.module.ts
						- 3. apply.module.ts注入index.module.ts
							- ```
							  constructor(
							    private readonly pageModule: PageModule,
							  ) {
							    super()
							  }
							  ```
						- 4. 抽到apply.module.ts的effect内部所访问的effect/reducer如果还在index.module.ts不准备剥离，那么的this.getActions()需要改成this.pageModule.getActions()，this.state$ 同理改成 this.pageModule.state$
						- 5. 根据index.module.ts的被剥离的effect方法的引用，在具体页面里访问的dispatcher需要换module，还完后确认effect不再有地方引用后，即可删除
						- 6. 如果state 也想剥离出去，按上面的思路，先把相关state挪到新的module，然后依次根据vscode提示的引用关系，找出所有原先用到state的组件/module，换成新的module里引入，确认无引用后删除原先module的相关state代码，删除即可
						- 7. 以上步骤全部在ts加持下，可不跑项目放心改造
					- 注意事项：
						- module的注入是在定义Module时完成的，所以也要和 es 模块不能互相import一样，不能互相注入，就像组件的单向数据流一样。如果有希望互相访问state的情况，那么可以把state提到页面主module上
			- 2. 在弹框刚开始开发的时候就划分一个Module给它，后续步骤参考上面剥离思路。同时也建议，最好一开始就拆分Module，不要吝啬新建 Module
		- CRM审批管理页面的Module拆分：
			- 一开始只有一个PageModule，下面 2 个Module是随着迭代新拆出去的，中间花费的时间很少
			- 箭头方向为注入方向
			- ![](https://remnote-user-data.s3.amazonaws.com/CjFnqVoMGSSM4zGGsDeK0Eg9JSeNZcqD7A8wsVE_fNLqVfV7S7tfNbFBnzmNDimuyF4yL55sTTOzEyF_S3ebD2nD1Ew5fZ-sxk2Q0ZVet1bDlktEyZYIH4NJSYoy4lct.png)
- 拆分Module的形式
	- collapsed:: true
	  1. 主 Module 注入到副 Module
		- 一个有state的主Module搭配n个没state或少量state的副Modoule
		  collapsed:: true
			- 这种形式的副Module有点像外包，只负责拆分副作用逻辑，被组件直接调用dispatch。副Module在有需要的时候调用主Module的reducer/effect，组件也可直接调用主Module的 reducer/effect
		- ![](https://remnote-user-data.s3.amazonaws.com/vQD2myj5Q7tQyTb3g8THI-hD6qOLFy7F0-eN44cLZda5Djg0MVB7Sa7LYPoXfmCgJiW4Lcx-Hw1N7lWz8HuW-TasAlvG8vHg8sVO8QVUP_uGdDn1vk1yvidyIRgeh5c6.png)
			- [Excalidraw | Hand-drawn look & feel • Collaborative • Secure](https://www.remnote.com/doc/MOWdFDbFA4TXUuRcr)
- [总结](https://www.remnote.com/doc/4XCDQRLnBE1fNWMt1)―以上只是示例，副 Module之间也可以互相引入，==其实只要避免Module/service之间互相注入==，怎么使用都可以，但尽量合理地拆分Module会更让人容易理解的和维护