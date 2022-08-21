- >本文主讲 Typescript 一些实用，但不太常见的技巧。每个技巧都有一个需求对应，不会太枯燥，按简单到复杂的顺序讲解，方便读者理解吸收。看完例子，你就知道为什么要使用这些骚操作，代码会提升一个境界。
  
  背景：小明发现近年来宠物市场发展迅猛，于是想做一个在线宠物交友平台，宠物信息格式如下：
  
  ```tsx
  interface PetInfo {
    name: string;
    age: string;
  }
  ```
- ## 枚举类型
  
  > 需求：小明希望根据宠物成长阶段，定向推荐商品，因为根据年龄判断不准确，不同种类宠物不一样。
  
  可以通过增加字段 stage 或 phase，用枚举表示，方便拓展。
  
  ```tsx
  enum PetStage {
      Infancy = 'Infancy',     // 幼年
      Childhood = 'Childhood',   // 童年
      Adolescence = 'Adolescence', // 青春期
      Adulthood = 'Adulthood',   // 成年
  }
  
  interface PetInfo {
  	name: string;
  	age: string;
  	stage: keyof typeof PetStage;
  }
  
  function addPet(p: PetInfo) {...}
  
  addPet({ 
      name: 'kity',
      age: 2,
      stage: PetStage.Infancy
  });
  ```
- ##  字典
  
  > 需求：平台需要国际化，页面信息要支持中英文
  
  ```tsx
  const dictionary: { [key: string]: string; } = { key1: '1' };
  ```
  
  可以这样实现：
  
  ```tsx
  const LOCALE_ZH_CN: { [key: string]: string } = {
      cat: '猫',
      dog: '狗'
  };
  
  const LOCALE_EN: { [key: string]: string } = {
      cat: 'Cat',
      dog: 'Dog'
  };
  ```
  
  像字典一样，可以查对应的语言。
- ## keyof 关键字
  
  > 需求：平台有线下交友功能，需要根据用户 GPS 位置信息，计算聚会地点。
  
   `keyof`  有点像  `Object.keys`  不过  `keyof`  取的是  `interface`  的键（Key）,比如下面这样：
  
  ```tsx
  interface Point {
      x: number;
      y: number;
  }
  
  // 等同于 type Axis = "x" | "y"
  type Axis = keyof Point;
  ```
  
  计算线段中点坐标值，具体哪个坐标轴可选
  
  ```tsx
  function cal(a: Point, b: Point, axis: Axis): number {
      return (a[axis] + b[axis]) / 2;
  }
  ```
  
   `keyof`  关键字非常实用，后面可以看到很多工具泛型都是使用 keyof 实现的。
- ##  as 类型断言
  
  > 需求：为了代码复用，小明希望原先的 js 代码能够迁移到 ts 中，增加类型推断
  
  ```tsx
  const foo = {};
  foo.bar = 123; // Error: 'bar' 属性不存在于 ‘{}’
  foo.bas = 'hello'; // Error: 'bas' 属性不存在于 '{}'
  ```
  
  使用 as 关键字，断言类型：
  
  ```tsx
  interface Foo {
      bar: number;
      bas: string;
  }
  
  const foo = {} as Foo;
  foo.bar = 123;
  foo.bas = 'hello';
  ```
  
  需要注意的是，除了代码迁移用到 as 语法，大部分其他场景应该避免使用，类型断言纯粹是编译时语法。类型推断应该尽量使用 interface / type / 基础类型。（断言被认为是有害的）
- #  工具泛型
  
  Typescript 提供的工具泛型源码可以在  `lib.es5.d.ts`  文件上找到，对常见类型判断需求进行封装，我们可以直接使用，省去自行编写维护的麻烦。（下面的内容需要读者有一定的泛型基础）
- ## Partial
  
  > 需求：很多用户在宠物平台上分享心得文章，支持用户修改更新，但不希望全部字段都 update
  
  Partial 将传入的所有属性变为可选
  
  ```tsx
  type Partial<T> = { [P in keyof T]?: T[P] };
  ```
  
  比如更新一篇文章，并不需要更新全部字段：
  
  ```tsx
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
- ## Required
  
  将传入的所有属性变为必选项，这个和 Partial 相反，不举例了。
  
  ```tsx
  type Required<T> = { [P in keyof T]-?: T[P] };
  ```
- ## Readonly
  
  > 需求：APP 上有宠物食品的商品信息（价格等），避免程序员错误地在客户端修改
  
  Readonly 将传入的属性变为只读选项，ts 原理如下：
  
  ```tsx
  type Readonly<T> = { readonly [P in keyof T]: T[P] };
  ```
  
  可以这样解决：
  
  ```tsx
  interface GoodsInfo {
      id: string;
      price: number;
      description: string;
      images: string[];
  }
  
  type RGoodsInfo = Readonly<GoodsInfo>;
  
  function getGoods(): RGoodsInfo {...}
  ```
- ## Record
  
  > 需求：一个用户有很多种宠物，希望一次性提取，基本信息都一样，但是宠物种类很多。为了方便拓展和维护，希望这两部分解耦（种类和基本信息），同时有类型检查
  
  该类型可以将 K 中所有的属性的值转化为 T 类型
  
  ```tsx
  type Record<K extends keyof any, T> = {
      [P in K]: T;
  };
  ```
  
  上面的需求我们可以这样实现：
  
  ```tsx
  type PetType = 'dog' | 'cat' | 'fish';
  
  interface PetInfo {
      name: string;
      age: number;
  }
  
  type Pets = Record<PetType, PetInfo>;
  
  const pets: Pets = {
      dog: { name: 'didi', age: 1 },
      cat: { name: 'cici', age: 2 },
      fish: { name: 'fifi', age: 3 }
  };
  ```
  
  当然，你可以这样做：
  
  ```tsx
  interface Pets {
      dog: PetInfo;
      cat: PetInfo;
      fish: PetInfo;
  }
  ```
  
  使用 Recode 的好处是关注点分离，特别是当代码由不同的人实现，或者部分代码来自第三方时，就比较实用。
- ##  Pick
  
  > 需求：对于非 VIP 用户，只能查看他人一部分的宠物信息
  
  从 T 中取出 一系列 K 的属性，ts 原理：
  
  ```tsx
  type Pick<T, K extends keyof T> = {
        [P in K]: T[P];
  };
  ```
  
  可以这样实现，指定只能查看部分字段
  
  ```tsx
  interface PetInfo {
      name: string;	
      age: string;
  }
  
  type NonVipInfo = Pick<PetInfo, 'name'>;
  ```
  
  按以往的习惯，你可能这样写：
  
  ```tsx
  interface NonVipInfo {
      name: string;
  }
  ```
  
  直接编码的坏处就是无法体现字段依赖关系，其他人不知道 name 字段哪里来，当 PetInfo 字段 name 重构为 nickname 时，可能就忘记改动这里了。
- ##  Exclude
  
  > 需求：宠物平台已经有了放假日的活动推广，小明想增加工作日的推广。
  
  Exclude 将某个类型中属于另一个的类型移除掉。
  
  ```tsx
  type Exclude<T, U> = T extends U ? never : T;
  ```
  
  平常日剔除休息日就是工作日
  
  ```tsx
  // 一周（平常日）
  type Weekday = 'Monday' | 'Tuesday' | 'Wednesday' | 'Thursday' | 'Friday' | 'Saturday' | 'Sunday';
  // 休息日
  type DayOff = 'Saturday' | 'Sunday';
  // 工作日
  type WorkDay = Exclude<Weekday, DayOff>; 
  
  const day: WorkDay = 'Monday';
  ```
- ## Extract
  
  > 需求：小明想增加会员日和休息日的双重折扣活动
  
  和 Exclude 相反，提取共同的类型：
  
  ```tsx
  type Extract<T, U> = T extends U ? T : never;
  ```
  
  提取重叠的时间即可：
  
  ```tsx
  // 休息日
  type DayOff = 'Saturday' | 'Sunday';
  // 会员日
  type VipDay = 'Sunday' | 'Monday';
  
  // 等价与 type DoubleDiscountDay = 'Sunday';
  type DoubleDiscountDay = Extract<DayOff, VipDay>;
  
  const day0: DoubleDiscountDay = 'Sunday'; // Success
  const day1: DoubleDiscountDay = 'Monday'; // Error
  ```
- ## Omit
  
  > 需求：对于非 VIP 用户，只能查看他人一部分的宠物信息（上面用 Pick 实现）
  
  Pick 和 Exclude 进行组合, 实现忽略对象某些属性功能
  
  ```tsx
  type Omit<T, K extends keyof any> = Pick<T, Exclude<keyof T, K>>;
  ```
  
  忽略某些属性，不显示：
  
  ```tsx
  interface PetInfo {
      name: string;
      age: string;
  }
  
  type NonVipInfo = Omit<PetInfo, 'age'>;
  ```
  
  这个和 Pick 功能相反，当要隐藏的字段较少时，用 Omit 方便，当要显示的信息较少时，用 Pick 方便。
- ## NonNullable
  
  避免使用了可能为“空”的类型，这个比较简单，不举例了。
  
  ```tsx
  type NonNullable<T> = T extends null | undefined ? never : T;
  ```
- ## infer
  
  这个对新手稍微有点难理解，本来是放在前面，和后面几种工具泛型相关，放在这里解释较好。 在之前 ts 大多数类型都是已经“确定”的，但有时候我们需要根据一些条件才能确定类型，像上面的  `NonNullable`  。不过这还不够，想象一下，我们有类型 T，当 T 是一个数组时，提取 T 的基础类型，当 T 不是数组时，返回类型 T，如何实现呢？
  
   `infer`  关键字就是为了实现类似需求，做到“模式匹配”，最早出现在[这里](https://github.com/Microsoft/TypeScript/pull/21496) & [这里](https://github.com/microsoft/TypeScript/pull/21316)，通常需要配合  `extends`  关键字使用，看下例子：
  
  ```tsx
  type R<T> = T extends (infer U)[] ? U : T;
  
  type A = number[];
  
  // 等同于 type B = number;
  type B = R<A>;
  
  const test: B = 123;
  ```
- ##  ReturnType
  
  > 需求：小明的宠物平台越做越好，想要对接供应商平台，做到自动补货，清点。供应商给了一个函数 F 用于获取信息，并提示说由于他们升级接口，后续返回的数据格式可能会改变。小明想在升级接口时自动发现代码改动点，同步检查。
  
  ReturnType：提取函数的返回类型，ts 原理如下：
  
  ```tsx
  type ReturnType<T extends (...args: any) => any> = T extends (...args: any) => infer R ? R : any;
  ```
  
  小明可以这样实现：
  
  ``` tsx
  import { F } from 'vendor';
  
  type Res = ReturnType<F>;
  
  function myFun(res: Res) {...}
  ```
  
  在使用 ReturnType 之前，你可能这样做：
  
  ``` tsx
  function a(): number {
  // ...
  }
  
  const res: number = a();
  ```
  
  直接编码的坏处就是丢失了类型的依赖关系信息，number 类型是依赖于 a() 函数的，如果 a 的返回类型修改，res 的类型也应该修改，不然就埋下了一个定时炸弹，bug 指不准哪天就出现了。
- ## Parameters
  
  > 需求：还是供应商的接口，请求参数可能会改变，小明需要同步更改
  
  提取函数的参数类型：
  
  ```tsx
  type Parameters<T extends (...args: any) => any> = T extends (...args: infer P) => any ? P : never;
  ```
- ## InstanceType
  
  提取实例类型：
  
  ```tsx
  type InstanceType<T extends new (...args: any) => any> = T extends new (...args: any) => infer R ? R : any;
  ```
- ## 结语：构建类型依赖关系
  
  在以前，你的 Typescript 可能一直停留在无限 interface 上面，很少思考类型之间的依赖关系，顶多就是使用 & 、| 、extends 拼接，但从上面的 Pick 泛型到关键字 infer，可以看到，构建类型的依赖关系，对于以后代码重构，升级好处多多，一处更改，IDE 同时提示报错相应改动点，大多数 Bug 在编译时就能发现并解决。