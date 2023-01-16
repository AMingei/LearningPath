<style>body:not(code) { font-family: '华文中宋', 'Cambria', serif, sans-serif; } h2, h3, h4, h5, h6 { font-weight: bold; }</style>

# TypeScript 入门

> [1.类型系统基础](#类型系统基础)
> > [1.1.原始数据类型](#原始数据类型) [布尔类型](#布尔类型) [数字类型](#数字类型) [字符串类型](#字符串类型) [Null 和 Undefined](#null-和-undefined)  
[1.2.类型推论](#类型推论)  
[1.3.原始数据类型-Ex](#原始数据类型-ex) [Void](#空类型) [Any](#任意类型) [Unknown](#未知类型) [Never](#底层类型)  
[1.4.联合类型](#联合类型)  
[1.5.类型兼容](#类型兼容) [对象类型的兼容](#对象类型的兼容) [函数类型的兼容](#函数类型的兼容)  
[1.6.类型断言](#类型断言) [断言的应用](#断言的应用) [双重断言](#双重断言)  
[1.7.类型别名](#类型别名)
> 
> [2.数组的类型](#数组的类型)
> > [2.1.数组的表示](#数组的表示) [方括号表示法](#方括号表示法) [泛型表示](#泛型表示) [接口表示](#接口表示)
> 
> [3.函数的类型](#函数的类型)
> > [3.1.函数的表示](#函数的表示)  
[3.2.函数的拓展](#函数的拓展) [可选参数](#可选参数) [默认参数](#默认参数) [剩余参数](#剩余参数) [函数重载](#函数重载)
> 
> [4.对象的类型](#对象的类型)
> > [4.1.类的拓展](#类的拓展) [访问修饰符](#访问修饰符) [只读属性](#只读属性) [参数属性](#参数属性) [类的抽象](#类的抽象)  
[4.2.接口](#接口) [只读属性](#只读属性-1) [可选属性](#可选属性) [任意属性](#任意属性)  
[4.3.实现](#实现)  
[4.4.继承](#继承) [接口继承类](#接口继承类)
> 
> [5.TS 新增类型](#ts-新增类型)
> > [5.1.字面量类型](#字面量类型)  
[5.2.元组](#元组)  
[5.3.枚举](#枚举) [数字赋值](#数字赋值) [非数字赋值](#非数字赋值) [常数枚举](#常数枚举) [外部枚举](#外部枚举)
> 
> [6.泛型](#泛型)
> > [6.1.泛型的应用](#泛型的应用) [泛型函数](#泛型函数) [泛型接口](#泛型接口) [泛型类](#泛型类)  
[6.2.泛型约束](#泛型约束)  
[6.3.默认参数](#默认参数)
> 
> [7.声明合并](#声明合并)
> > [7.1.合并的应用](#合并的应用) [函数的合并](#函数的合并) [类与接口的合并](#类与接口的合并)
> 
> [8.内置对象](#内置对象)
> > [8.1.ECMAScript 内置对象](#ecmascript-内置对象)  
[8.2.DOM 内置对象](#dom-内置对象)  
[8.3.TypeScript 核心库的定义文件](#typescript-核心库的定义文件)
> 
> [.声明文件](#声明文件)
> > [.1.全局变量](#全局变量) [声明类型](#声明类型) [声明命名空间](#声明命名空间) [Interface 与 Type](#interface-与-type) [命名冲突](#命名冲突)  
[.2.直接扩展全局变量](#直接扩展全局变量)  
[.3.npm 包](#npm-包)  
[.4.UMD 库](#umd-库)  
[.5.npm 包 | UMD 库中扩展全局变量](#npm-包--umd-库中扩展全局变量)  
[.6.模块插件](#模块插件)  
[.7.声明文件依赖](#声明文件依赖)  
[.8.自动声明文件](#自动声明文件)

参考：

> [TypeScript 入门教程 - xcatliu](http://ts.xcatliu.com/)  
[ES6 入门教程 - ECMAScript 6入门 - 阮一峰](https://es6.ruanyifeng.com/)  
[TypeScript 中文网](https://www.tslang.cn/docs/handbook/basic-types.html)  
[TypeScript中的never类型具体有什么用？ - 知乎 - 尤雨溪](https://www.zhihu.com/question/354601204)  
[《TS中any、unknown、never 的区别是什么？》 - 知乎](https://zhuanlan.zhihu.com/p/482033186)  
[TypeScript 笔记（二） 字面量类型 - 知乎](https://zhuanlan.zhihu.com/p/60231515)  
[JavaScript 标准内置对象 - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects)  
[ES5/标准 ECMAScript 内置对象 - HTML5 Chinese  Interest Group Wiki](https://www.w3.org/html/ig/zh/wiki/ES5/%E6%A0%87%E5%87%86_ECMAScript_%E5%86%85%E7%BD%AE%E5%AF%B9%E8%B1%A1)  
[文档对象模型 (DOM) - Web API 接口参考 | MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Document_Object_Model)  


<br />

## 类型系统基础

<hr />

JavaScript 的类型分为两种：**原始数据类型**（Primitive data types）和**对象类型**（Object types）。  
JavaScript 原始数据类型包括：`boolean`、`number`、`string`、`null`、`undefined`、`Symbol`（ES6）、`BigInt`（ES10）。

TypeScript 新增原始数据类型：`void`、`any`、`unknown`、`never`。

### 原始数据类型

#### 布尔类型

```ts
let isReady: boolean = true;
let isReadyEx: boolean = Boolean(1);
```

注意：使用构造函数 `Boolean` 创造的对象不是布尔值。

> ```ts
> /* F */ let isReadyEx: boolean = new Boolean(1);
> ```

#### 数字类型

```ts
let decLiteral: number = 6;
let hexLiteral: number = 0xf00d;
// ES6 二进制表示法 =编译=> 十进制数
let binLiteral: number = 0b1010;
// ES6 八进制表示法 =编译=> 十进制数
let octLiteral: number = 0o744;
let notANumber: number = NaN;
let infinityNumber: number = Infinity;
```

#### 字符串类型

```ts
let myName: string = 'Tom';
let myAge: number = 25;

// ES6 模板字符串 =编译=> 拼接字符串
let sentence: string = `Hello, my name is ${myName}.
I'll be ${myAge + 1} years old next month.`;
```

#### Null 和 Undefined

TypeScript 中，可以使用 `null` 和 `undefined` 来定义这两个原始数据类型：

```ts
let u: undefined = undefined;
let n: null = null;
```
`undefined` 和 `null` 是所有类型的子类型，可以复制给任意类型变量：

```ts
let u: boolean = undefined;
let n: boolean = null;
```

### 类型推论

对于未指定类型变量将默认为任意类型，若已赋初值，那么 TypeScript 会依照类型推论（Type Inference）的规则推断出一个类型。

```ts
let unknown = 'seven';
// 两者等价
let unknown: string = 'seven';
```

### 原始数据类型-Ex

#### 空类型

关键字 `void` 表示不属于任何类型，用以表示没有任何返回值的函数：
```ts
function func(): void { ... }
```

空类型的变量只能赋值为 `undefined` 和 `null`：

```ts
let u: void = undefined;
let n: void = null;
```

#### 任意类型

关键字 `any` 用来表示允许赋值为任意类型，不进行类型检查：

```ts
let myFavoriteNumber: any = 'seven';
myFavoriteNumber = 7;
```

声明一个变量为任意值之后，**对它的任何操作，返回的内容的类型都是任意值。**
对任意类型变量，访问任何属性、调用任何方法都是允许的：

```ts
let anyThing: any = 'hello';
console.log(anyThing.myName);
console.log(anyThing.myName.firstName);

anyThing.setName('Jerry');
anyThing.setName('Jerry').sayHello();

anyThing.myName.setFirstName('Cat');
```

对于未指定类型变量将默认为任意类型：

```ts
let something;
// 两者等价
let something: any;
```

#### 未知类型

关键字 `unknown` 表示未知类型，比 `any` 安全性更高，在访问前需要进行类型收窄：

```ts
const value: unknown = "Hello World"; 
const someString: string = value as string;
```

#### 底层类型

关键字 `never` 表示些永不存在的值的类型，没有类型是 `never` 的子类型或可以赋值给 `never` 类型，返回 `never` 的函数必须存在无法达到的终点：

```ts
function infiniteLoop(): never {
	while(true) {}
}

function error(message: string): never {
	throw new Error(message);
}

function fail(): never {
	return error('Something failed');
}
```

[底层类型可以被应用于类型判断-知乎 尤雨溪](https://www.zhihu.com/question/354601204/answer/888551021)：

```ts
interface A { type: 'a' }
interface B { type: 'b' }
type All = A | B
function handleValue(val: All) {
	switch (val.type) {
		case 'a':
			// val 被收窄为 A
			break
		case 'b':
			// val 被收窄为 B
			break
		default:
			// val 在这里是 never
			const exhaustiveCheck: never = val
			break
	}
}
```

注意在 default 里面我们把被收窄为 `never` 的 `val` 赋值给一个显式声明为 `never` 的变量。如果一切逻辑正确，那么这里应该能够编译通过。但是假如后来有一天你的同事改了 `All` 的类型：

```ts
type All = A | B | C
```

然而他忘记了在 `handleValue` 里面加上针对 `C` 的处理逻辑，这个时候在 default branch 里面 `val` 会被收窄为 `C`，导致无法赋值给 `never`，产生一个编译错误。所以通过这个办法，你可以确保 `handleValue` 总是穷尽 (exhaust) 了所有 `All` 的可能类型。

### 联合类型

联合类型（Union Types）表示取值可以为多种类型中的一种，在访问属性或方法时，将通过类型推论判断是否合法：

```ts
let myFavoriteNumber: string | number;
myFavoriteNumber = 'seven';
console.log(myFavoriteNumber.length); 
myFavoriteNumber = 7;
console.log(myFavoriteNumber.toFixed()); 
```

当 TypeScript 不确定一个联合类型的变量到底是哪个类型的时候，只能访问此联合类型的所有类型里共有的属性或方法：

```ts
function getString(something: string | number): string {
	return something.toString();
}
```

### 类型兼容

传统的面向对象语言中，数据类型的兼容性或等价性基于 **名义类型**，通过明确的声明和/或类型的名称来决定的。  
由于 JavaScript 里广泛地使用匿名对象（函数表达式和对象字面量），在 TypeScript 中类型兼容性基于 **结构子类型**。结构类型是一种只使用其成员来描述类型的方式。

#### 对象类型的兼容

如果类型 `A` 兼容 `B`，那么 `B` 拥有与 `A` 相同的属性与方法：

```ts
let x: { name: string };
let y = { name: 'Tom', age: 12};
x = y;
console.log(x); // { name: 'Tom', age: 12 }
```

#### 函数类型的兼容

如果函数 `A` 兼容 `B`，那么 `A` 拥有与 `B` 相同类型的参数和返回类型：

```ts
let x: (a: number, b: string) => void;
let y = (c: number) => {};
x = y;
console.log(x); // [Function: y]
```

**注意** 目前为止，我们使用了“兼容性”，它在语言规范里没有定义。

### 类型断言

当类型 `A` 兼容 `B` 或类型 `B` 兼容 `A` 时，可以使用 **类型断言**（Type Assertion）手动将 `A` 类型变量指定为 `B` 类型：

```ts
Value as Type
// 两者等价
<Type>Value
```

**实际应用中推荐使用** `Value as Type` **方式**。

**注意** 类型断言只能够「欺骗」TypeScript 编译器，而无法避免运行时的错误。

#### 断言的应用

**父类的断言**：当我们仅确定当前子类/接口继承的所继承的父类，就需要访问其中一个类/接口特有的属性或方法时：

```ts
interface ApiError extends Error {
	code: number;
}
interface HttpError extends Error {
	statusCode: number;
}

function isApiError(error: Error) {
	if (typeof (error as ApiError).code === 'number') {
		return true;
	}
	return false;
}
```

**联合类型的断言**：当我们在还不确定一个联合类型变量的具体类型，就需要访问其中一个类型特有的属性或方法时：

```ts
interface Cat {
	run(): void;
}
interface Fish {
	swim(): void;
}

function isFish(animal: Cat | Fish) {
    if (typeof (animal as Fish).swim === 'function') {
        return true;
    }
    return false;
}
```

**任意类型的断言**：在日常的开发中，我们不可避免的需要处理 any 类型的变量，它们可能是由于第三方库未能定义好自己的类型，或是历史遗留问题，或是受到 TypeScript 类型系统的限制而无法精确定义类型的场景。我们可以通过类型断言及时的把 any 断言为精确的类型，亡羊补牢：

```ts
function getCacheData(key: string): any {
	return (window as any).cache[key];
}

interface Cat {
	name: string;
	run(): void;
}

const tom = getCacheData('tom') as Cat;
tom.run();
```

**as any 断言**：实际来开发中，当我们需要将 window 上挂载一个新的属性时，TypeScript 将会报错：

> ```ts
> /* F */ window.foo = 1;
> ```

此时我们可以使用 `as any` 临时将 `window` 断言为任意类型：

```ts
(window as any).foo = 1;
```

#### 双重断言

已知：
- 任何类型变量可被断言为 any 类型
-  any 类型变量可以被断言为任意类型

可知，通过双重断言 `Value as any as Type` 可以在任何变量类型间互相断言。  
**当你使用这种了断言方式**，**很可能是因为代码逻辑出现了错误**。

### 类型别名

类型别名使用 `type` 给一个类型起个新名字。

```ts
type Name = string;
type NameResolver = () => string;
type NameOrResolver = Name | NameResolver;
function getName(n: NameOrResolver): Name {
	if (typeof n === 'string') {
		return n;
	} else {
		return n();
	}
}
```

<br />

## 数组的类型

<hr />

### 数组的表示

#### 方括号表示法

最简单的方法是使用「类型 + 方括号」来表示数组，数组的项中不允许出现其他的类型：

```ts
let fibonacci: number[] = [1, 1, 2, 3, 5];
```

数组的一些方法的参数也会根据数组在定义时约定的类型进行限制：

> ```ts
> let fibonacci: number[] = [1, 1, 2, 3, 5];
> /* F */ fibonacci.push('8');
> ```

#### 泛型表示

我们也可以使用数组泛型（Array Generic） `Array<elemType>` 来表示数组：

```ts
let fibonacci: Array<number> = [1, 1, 2, 3, 5];
```

#### 接口表示

接口也可以用来描述数组：

```ts
interface NumberArray {
	[index: number]: number;
}
let fibonacci: NumberArray = [1, 1, 2, 3, 5];
```

通常接口被用来表示 **类数组**（Array-like Object），如：

```ts
interface IArguments {
	[index: number]: any;
	length: number;
	callee: Function;
}

let args: IArguments = {
	'0': 'zero',
	'1': 'one',
	'2': 'two',
	length: 3,
	callee: function() { ... }
};
```

<br />

## 函数的类型

<hr />

JavaScript 中，有两种常见的定义函数的方式—— **函数声明**（Function Declaration）和 **函数表达式**（Function Expression）：

```js
// 函数声明（Function Declaration）
function sum(x, y) { return x + y; }

// 函数表达式（Function Expression）
let mySum = function (x, y) { return x + y; };
```

### 函数的表示

```ts
function sum(x: number, y: number): number {
	return x + y;
}

let mySum: (x: number, y: number) => number = function (x: number, y: number): number {
	return x + y;
};

// 接口表示
interface ISearchFunc {
	(source: string, subString: string): boolean;
}

let mySearch: ISearchFunc;
mySearch = function(source: string, subString: string) {
	return source.search(subString) !== -1;
}
```

**注意** 不要混淆了 TypeScript 中的 `=>` 和 ES6 中的 `=>`。  
在 TypeScript 的类型定义中，`=>` 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。

采用函数表达式 | 接口定义函数的方式时，对等号左侧进行类型限制，可以保证以后对函数名赋值时保证参数个数、参数类型、返回值类型不变。

### 函数的拓展

#### 可选参数

函数内可选参数通过 `?` 表示，**可选参数必须接在必需参数后面**：

```ts
function buildName(firstName: string, lastName?: string) { ... }
```

#### 默认参数

ES6 中，允许为函数的参数添加默认值，TypeScript 会将添加了默认值的参数识别为可选参数：

```ts
function buildName(firstName: string, lastName: string = 'Cat') { ... }
```

#### 剩余参数

ES6 中，可以使用 `...rest` 的方式获取函数中的 **剩余参数**（rest 参数），**rest 参数必须放在最后**。事实上，rest 参数是一个数组：

```ts
function push(array: any[], ...items: any[]) {
	items.forEach(function(item) {
		array.push(item);
	});
}
```

#### 函数重载

重载允许一个函数接受不同数量或类型的参数时，作出不同的处理。

可以通过重复定义重载函数，前若干次作为函数定义，最后一次作为函数实现。TypeScript 会优先从最前面的函数定义开始匹配，所以多个函数定义如果有包含关系，需要优先把精确的定义写在前面。

```ts
// 实现反转函数 reverse
// 输入数字 123 时，输出数字 321
// 输入字符串 'hello' 时，输出字符串 'olleh'
function reverse(x: number): number;
function reverse(x: string): string;
function reverse(x: number | string): number | string | void {
	if (typeof x === 'number') {
		return Number(x.toString().split('').reverse().join(''));
	} else if (typeof x === 'string') {
		return x.split('').reverse().join('');
	}
}
```

<br />

## 对象的类型

<hr />

ES6 提供了更接近传统语言的写法，引入了 **类**（Class）这个概念，作为对象的模板。通过 `class` 关键字，可以定义类。  
基本上，ES6 的类可以看作只是一个语法糖，它的绝大部分功能，ES5 都可以做到，新的 class 写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。

### 类的拓展

#### 访问修饰符

TypeScript 可以使用三种 **访问修饰符**（Access Modifiers）修饰属性变量和方法。
- `public` 修饰的属性或方法是公有的，可以在任何地方被访问到，默认所有的属性和方法都是共有的；
- `private` 修饰的属性或方法是私有的，不能在声明它的类的外部访问；
- `protected` 修饰的属性或方法是受保护的，和 private 类似，但允许在子类中被访问的。

**注意** TypeScript 编译之后的代码中，并没有限制 private 属性在外部的可访问性。

当构造函数修饰为 `protected` 时，该类不允许被 **实例化**：

> ```ts
> class Animal {
> 	public name;
> 	protected constructor(name) {
> 		this.name = name;
> 	}
> }
> /* F */ let a = new Animal('Jack');
> ```

当构造函数修饰为 `private` 时，该类不允许被 **继承** 或者 **实例化**：

> ```ts
> class Animal {
> 	public name;
> 	private constructor(name) {
> 		this.name = name;
> 	}
> }
> /* F */
> class Cat extends Animal {
> 	constructor(name) {
> 		super(name);
> 	}
> }
> let a = new Animal('Jack');
> ```

#### 只读属性

只读属性关键字（readonly）只允许出现在属性声明或索引签名或构造函数中，如果 readonly 和其他访问修饰符同时存在，需要写在最后。

```ts
class Animal {
	public readonly name;
	public constructor(name) {
		this.name = name;
	}
}
```

#### 参数属性

修饰符和 `readonly` 可以使用在构造函数参数中，等同于类中定义该属性同时给该属性赋值，使代码更简洁。

```ts
class Animal {
	// public name: string;
	public constructor(public name: string) {
		// this.name = name;
	}
}
```

#### 类的抽象

`abstract` 关键字用于定义 **抽象类** 和其中的 **抽象方法**。

- 抽象类是不允许被实例化的；
- 抽象类中的抽象方法必须被子类实现；

```ts
abstract class Animal {
	public name;
	public constructor(name) {
		this.name = name;
	}
	public abstract sayHi();
}

class Cat extends Animal {
	public sayHi() {
		console.log(`Meow, My name is ${this.name}`);
	}
}

let cat = new Cat('Tom');
```

**注意** 即使是抽象方法，TypeScript 的编译结果中，仍然会存在这个类。

### 接口

**接口**（Interfaces）是对行为的抽象，而具体如何行动需要由 **类**（classes）去 **实现**（implement）。

TypeScript 中的接口是一个非常灵活的概念，除了可用于对类的一部分行为进行抽象以外，也常用于对「对象的形状（Shape）」进行描述。

```ts
// 建议在接口前添加 I 用以区分类
interface IPerson {
	name: string;
	age: number;
}

let tom: IPerson = {
	name: 'Tom',
	age: 25
};
```

#### 只读属性

当要求对象中的某些字段只能在创建时被赋值，可以时用 `readonly` 关键字定义只读属性：

```ts
interface Person {
	readonly id: number;
	name: string;
}
```

**注意** 只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候。

#### 可选属性

```ts
interface Person {
	name: string;
	age?: number;
}
```

#### 任意属性

使用 `[propName: string]` 定义了任意属性类型的取值。

**注意** 一旦定义了任意属性，那么确定属性和可选属性的类型都必须是它的类型的子集：

```ts
interface Person {
	name: string;
	age?: number;
	[propName: string]: string | number;
}
```

### 实现

一般来讲，一个类只能继承自另一个类。对于不同类之间可以有一些共有的特性，可以将特性提取成 **接口**（interfaces），用 `implements` 关键字来实现，大大提高了面向对象的灵活性。

一个类可以实现多个接口。

> 例如，门是一个类，防盗门是门的子类。如果防盗门有一个报警器的功能，我们可以简单的给防盗门添加一个报警方法。这时候如果有另一个类，车，既能报警，也能开关车灯，就可以考虑把报警器和车灯提取出来，作为接口，按需实现它：

```ts
interface IAlarm {
	alert(): void;
}

interface ILight {
	switchLight(): void;
}

class Door {
}

class SecurityDoor extends Door implements IAlarm {
	alert() {
		console.log('SecurityDoor alert');
	}
}

class Car implements IAlarm, ILight {
	alert() {
		console.log('Car alert');
	}
	switchLight() {
		console.log('Light switch');
	}
}
```

### 继承

TypeScript 中继承的类型有 **接口继承接口**、**类继承类**、**接口继承类**。

#### 接口继承类

常见的面向对象语言中，接口是不能继承类的，但是在 TypeScript 可以：

```ts
class Point {
	x: number;
	y: number;
	constructor(x: number, y: number) {
		this.x = x;
		this.y = y;
	}
}

interface Point3d extends Point {
    z: number;
}

let point3d: Point3d = {x: 1, y: 2, z: 3};
```

当我们在声明 `class Point` 时，除了会创建一个名为 Point 的类之外，同时也创建了一个名为 Point 的类型（实例的类型）：

```ts
// 既可以将使用 new Point 创建类的实例
const p = new Point(1, 2);

// 也可以使用 : Point 表示参数的类型
function printPoint(p: Point) {
	console.log(p.x, p.y);
}
printPoint(new Point(1, 2));
```

换句话说，对 Point 类的声明等价于：

```ts
class Point {
	x: number;
	y: number;
	constructor(x: number, y: number) {
		this.x = x;
		this.y = y;
	}
}

// 新声明的 IPoint 类型，与声明 class Point 时创建的 Point 类型是等价的
interface IPoint {
	x: number;
	y: number;
}

const p = new Point(1, 2);

function printPoint(p: IPoint) {
	console.log(p.x, p.y);
}
printPoint(new IPoint(1, 2));
```

**注意** 声明 Point 类时创建的 Point 类型是不包含构造函数的。另外，除构造函数外，静态属性或静态方法也是不被包含的。

<br />

## TS 新增类型

<hr/>

### 字面量类型

字面量（Literal Type）即常量，通过 `const` 声明。

**注意** 在 JavaScript 中的常量和一般语言中的常量含义不同。JavaScript 中的常量的含义是：**只能在初始化时候赋值一次，从此都不可以再次赋值的变量**。例如：

```ts
// Date 对象并不是编译期可以确定的量，但声明后，不能修改，只能读取
const a = new Date()
```

TypeScript 中字面量主要分为布尔字面量类型、数字字面量类型、枚举字面量类型、大整数字面量类型和字符串字面量类型。字面量的类型格式：

```ts
const a: 2 = 2
const b: 0b10 = 2
const c: number = 2
const d: unknown = 2

const e: 'electron' = 'electron'
const f: string = 'fastjson'
```

实际应用中，**字符串字面量类型** 和 **联合类型** 能够很好的搭配，更便捷地实现枚举功能：

```ts
type EventNames = 'click' | 'scroll' | 'mousemove';
function handleEvent(ele: Element, event: EventNames) {
    // do something
}

handleEvent(document.getElementById('hello'), 'scroll');
```

### 元组

数组合并了相同类型的对象，而 **元组**（Tuple）合并了不同类型的对象。  
而在编译时，元组将被编译为 JavaScript 数组，因此元组拥有和数组相似的特征。

元组允许直接赋值，或者对其中某项赋值：

```ts
let tom: [string, number];
tom[0] = 'Tom';
tom = ['Tom', 25];
```

当添加越界的元素时，新元素的类型会被限制为元组中每个类型的联合类型：

> ```ts
> let tom: [string, number] = ['Tom', 25];
> /* F */ tom.push(true);
> ```

### 枚举

**枚举**（Enum）类型用于取值被限定在一定范围内的场景，使用 `enum` 关键字来定义：

```ts
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};

//枚举成员会被赋值为从 0 开始递增的数字，同时也会对枚举值到枚举名进行反向映射
console.log(Days["Sun"] === 0); // true
console.log(Days[0] === "Sun"); // true
```

枚举类型实际上将被编译为：

```ts
var Days;
(function (Days) {
	Days[Days["Sun"] = 0] = "Sun";
	Days[Days["Mon"] = 1] = "Mon";
		...
	Days[Days["Sat"] = 6] = "Sat";
})(Days || (Days = {}));
```

#### 数字赋值

我们也可以用 `number` 类型常量为枚举项手动赋值，未手动赋值的枚举项会接着上一个枚举项以 1 为步长递增。  
如果未手动赋值的枚举项与手动赋值项，后者将会覆盖前者：

```ts
enum Days {Sun = 3, Mon = 1, Tue, Wed, Thu, Fri = 6.5, Sat};

console.log(Days['Sun'], Days['Mon'], Days['Tue'], Days['Wed'], Days['Thu']); // 3 1 2 3 4
console.log(Days[3]); // Wed
console.log(Days['Fri'], Days['Sat']); // 6.5 7.5
```

枚举项有两种类型：**常数项**（constant member）和 **计算项**（computed member）。  
以上的例子是手动赋值常数项，而赋值计算项后则不能再出现未赋值项：

```ts
enum Days {Sun, Mon, Tue, Wed, Thu, Fri = 'Friday'.length, Sat = 7};

console.log(Days['Fri'], Days['Sat']); // 6 7
```

对于常数项与计算项的定义  
当满足以下条件时，枚举成员被当作是常数：
- 不具有初始化函数 & 前项是常数。（首个枚举元素的初始值默认为 0）
- 枚举成员使用常数枚举表达式初始化。常数枚举表达式是 TypeScript 表达式的子集，它可以在编译阶段求值。当一个表达式满足下面条件之一时，它就是一个常数枚举表达式：
	- 数字字面量  
	`enmu Color { R, G, B = 3 }`
	- 引用之前定义的常数枚举成员。如果这个成员是在同一个枚举类型中定义的，可以使用非限定名来引用  
	`enmu Color { R, G, B = R }`  
	`enmu Print { C, M, Y = Color.R }`
	- 带括号的常数枚举表达式  
	`enmu Color { R, G, B = (3) }`
	- `+`, `-`, `~`, `++`, `--` 一元运算符应用于常数枚举表达式
	- `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `>>>`, `&`, `|`, `^` 二元运算符，常数枚举表达式做为其一个操作对象。若常数枚举表达式求值后为 `NaN` 或 `Infinity`，则会在编译阶段报错

所有其它情况的枚举成员被当作是需要计算得出的值。

#### 非数字赋值

通过类型断言，tsc 将无视类型检查，编译出的 js 仍然是可用的：

```ts
enum Days {Sun, Mon, Tue, Wed, Thu, Fri = <any>false, Sat = <any>'S'};

console.log(Days[<any>false], Days['Fri']); // Fri false
console.log(Days[<any>'S'], Days['Sat']); // Sat S
```

同样的，非 number 类型的的赋值后不应再出现未负值项。

#### 常数枚举

常数枚举是使用 `const enum` 组合定义的枚举类型，其中不能包含计算型赋值：

```ts
const enum Directions { Up, Down, Left, Right = 4 }

let directions = [ Directions.Up, Directions.Down, Directions.Left, Directions.Right ];
```

常数枚举将被在编译阶段被优化（删除）：

```ts
var directions = [0 /* Directions.Up */, 1 /* Directions.Down */, 2 /* Directions.Left */, 4 /* Directions.Right */];
```

#### 外部枚举

外部枚举（Ambient Enums）是使用 `declare enum` 定义的枚举类型：

```ts
declare enum Directions { Up, Down, Left, Right = 4 }

let directions = [ Directions.Up, Directions.Down, Directions.Left, Directions.Right ];
```

`declare` 定义的类型只会用于编译时的检查，编译结果中会被删除：

```ts
let directions = [ Directions.Up, Directions.Down, Directions.Left, Directions.Right ];
```

组合使用 `declare const enum` 时，编译结果和常数枚举相同。

<br />

## 泛型

<hr />

**泛型**（Generics）是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。

### 泛型的应用

#### 泛型函数

实现函数 `createArray` 的过程中，我们使用了 [数组泛型](#泛型表示) 来定义返回值的类型。`Array<any>` 允许数组的每一项都为任意类型：

```ts
function createArray(length: number, value: any): Array<any> {
	let result = [];
	for (let i = 0; i < length; i++) {
		result[i] = value;
	}
	return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
```

但是我们预期的是，数组中每一项都应是参数 `value` 的类型。  
我们可以通过在函数名后添加 `<T>`，以 `T` 用来指代任意输入的类型：

```ts
function createArray<T>(length: number, value: T): Array<T> {
	let result: T[] = [];
	for (let i = 0; i < length; i++) {
		result[i] = value;
	}
	return result;
}
```

在调用时，可以指定它具体的类型，也可以让类型推论自动推算：

```ts
createArray(3, 'x'); // ['x', 'x', 'x']
createArray<string>(3, 'x'); // ['x', 'x', 'x']
```

定义泛型的时候，可以一次定义多个类型参数：

```ts
function swap<T, U>(tuple: [T, U]): [U, T] {
	return [tuple[1], tuple[0]];
}

swap([7, 'seven']); // ['seven', 7]
```

#### 泛型接口

函数类型可以 [使用接口表示](#函数的表示)，同样可以使用带有泛型的接口表示：

```ts
interface CreateArrayFunc<T> {
	(length: number, value: T): Array<T>;
}

let createArray: CreateArrayFunc<any>;
createArray = function<T>(length: number, value: T): Array<T> {
	let result: T[] = [];
	for (let i = 0; i < length; i++) {
		result[i] = value;
	}
	return result;
}

createArray(3, 'x'); // ['x', 'x', 'x']
```

**注意** 此时在使用泛型接口的时候，需要定义泛型的类型。

#### 泛型类

与泛型接口类似，泛型也可以用于类的类型定义中：

```ts
class GenericNumber<T> {
	zeroValue: T;
	add: (x: T, y: T) => T;
}

let myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function(x, y) { return x + y; };
```

### 泛型约束

在函数内部使用泛型变量时，由于事先不知道它的具体类型，所以不能随意的操作它的属性或方法。  
我们可以对泛型进行约束，例如，只允许这个函数传入那些包含 `length` 属性的变量：

```ts
interface Lengthwise {
	length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
	console.log(arg.length);
	return arg;
}
```

在特定需求下，多个类型参数之间也可以互相约束：

```ts
function copyFields<T extends U, U>(target: T, source: U): T {
	for (let id in source) {
		target[id] = (<T>source)[id];
	}
	return target;
}

copyFields({ a: 1, b: 2, c: 3, d: 4 }, { b: 10, d: 20 });
```

### 默认类型

TypeScript 2.3 以后，我们可以为泛型中的类型参数指定默认类型。  
当使用泛型时没有在代码中直接指定类型参数，从实际值参数中也无法推测出时，这个默认类型就会起作用：

```ts
function createArray<T = string>(length: number, value: T): Array<T> {
	let result: T[] = [];
	for (let i = 0; i < length; i++) {
		result[i] = value;
	}
	return result;
}
```

<br />

## 声明合并

<hr />

如果定义了两个相同名字的函数、接口或类，那么它们会合自动合并。

### 合并的应用

#### 函数的合并

函数的合并，即 [函数重载](#函数重载)。

#### 类与接口的合并

对于拥有不同的属性和方法的同名类或接口，将会直接合并到一个类或接口中：

```ts
interface Alarm {
	price: number;
	alert(s: string): string;
}
interface Alarm {
	weight: number;
	alert(s: string, n: number): string;
}
```

将合并为：

```ts
interface Alarm {
	price: number;
	weight: number;
	alert(s: string): string;
	alert(s: string, n: number): string;
}
```

同名且相同类型的属性将视为同一属性；同名而类型不一致时，将会报错：

> ```ts
> interface Alarm {
> 	price: number;
> }
> interface Alarm {
> 	/* F */ price: string;
> 	weight: number;
> }

<br />

## 内置对象

<hr />

内置对象是指根据标准在全局作用域（Global）上存在的对象。这里的标准是指 ECMAScript 和其他环境（比如 DOM）的标准。

### ECMAScript 内置对象

由 ECMAScript 标准提供的内置对象。分为：  
- 全局对象：全局对象的值属性（`NaN`、`Infinity`）、全局对象的函数属性（`eval(x)`、`parseInt(string, radix)`）、处理 URI 的函数属性（` encodeURI (uri)`、` decodeURI (encodedURI)`）等。  
- Object 对象、Function 对象、Array 对象、String 对象、Boolean 对象、 Number 对象、Math 对象、Date 对象、RegExp 对象、Error对象、JSON 对象。

在 TypeScript 中的使用：

```ts
let b: Boolean = new Boolean(1);
let e: Error = new Error('Error occurred');
let d: Date = new Date();
let r: RegExp = /[a-z]/;
```

### DOM 内置对象

由对象文档模型（Document Object Model）提供的内置对象。分为：

- DOM 接口（`document`、`Event`、`Node`）;  
- HTML 接口（`HTMLElement`）；  
- SVG 接口（`SVGElement`）。

在 TypeScript 中的使用：

```ts
let body: HTMLElement = document.body;
let allDiv: NodeList = document.querySelectorAll('div');
document.addEventListener('click', function(e: MouseEvent) {
	// Do something
});
```

### TypeScript 核心库的定义文件

TypeScript 核心库的定义文件中定义了所有浏览器环境需要用到的类型，并且是预置在 TypeScript 中的。

**注意** TypeScript 核心库的定义中不包含 Node.js 部分。  
如果想用 TypeScript 写 Node.js，则需要引入第三方声明文件：

```shell
npm install @types/node --save-dev
```

<br />

## 声明文件

<hr />

当使用第三方库时，我们需要引用它的声明文件，才能获得对应的代码补全、接口提示等功能。  
三方库的使用场景主要有以下几种：
- 全局变量：通过 `<script>` 标签引入后，注入全局变量
- 直接扩展全局变量：通过 `<script>` 标签引入后，改变一个全局变量的结构
- npm 包：通过 `import` 导入，符合 ES6 模块规范
- UMD 库：既可以通过 `<script>` 标签引入，又可以通过 `import` 导入
- npm 包 | UMD 库扩展全局变量：引用 npm 包或 UMD 库后，改变一个全局变量的结构
- 模块插件：通过 `<script>` 或 `import` 导入后，改变另一个模块的结构

当我们在 HTML 中通过 `<script>` 标签引入 jQuery，后就可以使用全局变量 `$` 或 `jQuery` 了。而 TypeScript 的编译过程中不接触 HTML 文件，会认为这种方式非法。  
这时，我们需要使用 **声明语句** `declare var` 来定义它的类型：

```ts
declare var jQuery: (selector: string) => any;

// 正常使用
jQuery('#foo');
```

通常我们会把声明语句放到一个单独的 **声明文件**（如 jQuery.d.ts）中：

```ts
// src/jQuery.d.ts

declare var jQuery: (selector: string) => any;
```

**注意 声明文件名命必须以 .d.ts 结尾**。

```
+ src
| + index.ts
| + jQuery.d.ts
+ tsconfig.json
```

### 全局变量

全局变量即第一个示例中展示的引用方式，有以下几种声明方式。

#### 声明类型

由于 `any` 类型不进行类型检查，`declare var` 与 `declare let` 效用相同；  
同时，全局变量通常没有修改需求，使用 `declare const` 效用相同。

当全局变量是一个函数、类或枚举时，可以使用 `declare function`、`declare class`、`declare enum` 进行定义。  
jQuery 实际上是一个函数，因此可以使用 `declare function` 来定义。在函数类型的声明语句中，同样支持函数重载：

```ts
// src/jQuery.d.ts

declare function jQuery(selector: string): any;
declare function jQuery(domReadyCallback: () => any): any;
```

```ts
// src/index.ts

jQuery('#foo');
jQuery(function() { alert('Dom Ready!'); });
```

引用类时，类似于接口，不能定义具体的实现：

```ts
// src/Animal.d.ts

declare class Animal {
	name: string;
	constructor(name: string);
	sayHi(): string;
}
```

#### 声明命名空间

`namespace` 是 TypeScript 早期时为了解决模块化而使用的关键字。  
由于历史遗留原因，ES6 出现前，TypeScript 提供了一种使用 `module` 关键字表示内部模块的方案。但由于 ES6 也使用了 `module` 关键字，TypeScript 为了兼容 ES6，使用 `namespace` 作为替代。

`namespace` 被淘汰了，但是在声明文件中，`declare namespace` 还是比较常用的，它用来表示全局变量是一个对象，包含很多子属性。  
比如 `jQuery` 是一个全局变量，它是一个对象，提供了 `jQuery.ajax` 等方法与属性，那么我们就可以使用 `declare namespace` 来声明这个拥有多个子属性的全局变量：

```ts
// src/jQuery.d.ts

declare namespace jQuery {
	function ajax(url: string, settings?: any): void;
	const version: number;
	class Event {
		blur(eventType: EventType): void
	}
}
```

如果对象拥有深层的层级，则需要用嵌套的 `namespace` 来声明深层的属性的类型：

```ts
// src/jQuery.d.ts

declare namespace jQuery {
	function ajax(url: string, settings?: any): void;
	namespace fn {
		function extend(object: any): void;
	}
}
```

```ts
// src/index.ts

jQuery.ajax('/api/get_something');
jQuery.fn.extend({
	check: function() {
		return this.each(function() {
			this.checked = true;
		});
	}
});
```

假如 jQuery 下仅有 fn 这一个属性,则可以不需要嵌套 `namespace`：

```ts
// src/jQuery.d.ts

declare namespace jQuery.fn {
	function extend(object: any): void;
}
```

#### interface 与 type

除了全局变量之外，可能有一些类型我们也希望能暴露出来。在类型声明文件中，我们可以直接使用 `interface` 或 `type` 来声明一个全局的接口或类型，这样的话，在其他文件中也可以使用这个接口或类型了：

```ts
// src/jQuery.d.ts

interface AjaxSettings {
	method?: 'GET' | 'POST'
	data?: any;
}
declare namespace jQuery {
	function ajax(url: string, settings?: AjaxSettings): void;
}
```

```ts
// src/index.ts

let settings: AjaxSettings = {
	method: 'POST',
	data: {
		name: 'foo'
	}
};
jQuery.ajax('/api/post_something', settings);
```

#### 命名冲突

暴露在最外层的 `interface` 或 `type` 会作为全局类型作用于整个项目中，我们应该尽可能的减少全局变量或全局类型的数量。故最好将他们放到 `namespace` 下：

```ts
// src/jQuery.d.ts

declare namespace jQuery {
	interface AjaxSettings {
		method?: 'GET' | 'POST'
		data?: any;
	}
	function ajax(url: string, settings?: AjaxSettings): void;
}
```

```ts
// src/index.ts

let settings: jQuery.AjaxSettings = {
	method: 'POST',
	data: {
		name: 'foo'
	}
};
jQuery.ajax('/api/post_something', settings);
```

### 直接扩展全局变量

有的第三方库扩展了一个全局变量，可是此全局变量的类型却没有相应的更新过来，就会导致 ts 编译错误，此时就需要扩展全局变量的类型。

比如扩展 `String` 类型，通过声明合并，使用 `interface String` 即可给 `String` 添加属性或方法：

```ts
interface String {
	prependHello(): string;
}

'foo'.prependHello();
```

> 也可以使用 `declare namespace` 给已有的命名空间添加类型声明：
> ```ts
> // types/jquery-plugin/index.d.ts
> 
> declare namespace JQuery {
> 	interface CustomOptions {
> 		bar: string;
> 	}
> }
> 
> interface JQueryStatic {
> 	foo(options: JQuery.CustomOptions): string;
> }
> ```
> 
> ```ts
> // src/index.ts
> 
> jQuery.foo({
> 	bar: ''
> });
> ```
> 没看懂……

### npm 包

### UMD 库

### npm 包 | UMD 库中扩展全局变量

### 模块插件

### 声明文件依赖

### 自动声明文件