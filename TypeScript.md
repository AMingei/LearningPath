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
[4.2.接口](#接口) [只读属性](#e58faae8afbbe5b19ee680a7-1) [可选属性](#可选属性) [任意属性](#任意属性)  
[4.3.实现](#实现)  
[4.4.继承](#继承) [接口继承类](#接口继承类)
> 
> [5.TS 新增类型](#ts-新增类型)
> > [5.1.字面量类型](#字面量类型)  
[5.2.枚举](#枚举)  
[5.3.元组](#元组)

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

关键字 `never` 表示些永不存在的值的类型，没有类型是 `never` 的子类型或可以赋值给 `never` 类型，返回never的函数必须存在无法达到的终点：

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

可知，通过双重断言 `Value as any as Type` 可以在任何变量类型间互相断言  
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

基本上，ES6 的类可以看作只是一个语法糖，它的绝大部分功能，ES5 都可以做到，新的class写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。

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
	public constructor(public name) {
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

字面量即常量，通过 `const` 声明。

**注意** 在 JavaScript 中的常量和一般语言中的常量含义不同。JavaScript 中的常量的含义是：“只能在初始化时候赋值一次，从此都不可以再次赋值的变量”，举个最简单的例子：

```ts
// Date 对象并不是编译期可以确定的量，但声明后，不能修改，只能读取
const a = new Date()
```

### 枚举



### 元组