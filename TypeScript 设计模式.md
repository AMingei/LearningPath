<style>body:not(code) { font-family: '华文中宋', 'Cambria', serif, sans-serif; } h2, h3, h4, h5, h6 { font-weight: bold; }</style>

# TypeScript 设计模式

> [1.创建型模式](#创建型模式) 对象实例化的模式，创建型模式用于解耦对象的实例化过程。
> > [1.1.工厂模式](#工厂模式)：工厂接口根据传入的参量决定所创建的实例。  
[1.2.单例模式](#单例模式)：类仅能有唯一实例，提供一个全局的访问点。  
1.3.抽象工厂模式：创建相关或依赖对象的家族，而无需明确指定具体类。  
1.4.建造者模式：封装一个复杂对象的创建过程，并可以按步骤构造。  
1.5.原型模式：通过复制现有的实例来创建新的实例。
> 
> 2.结构型模式 把类或对象结合在一起形成一个更大的结构。
> > 2.1.装饰器模式：动态的给对象添加新的功能。  
\[2.2.代理模式](#代理模式)：提供一个代理以便控制当前对象的访问。  
2.3.桥接模式：将抽象部分和它的实现部分分离，使它们都可以独立的变化。  
2.4.适配器模式：将一个类的方法接口转换成客户希望的另一个接口。  
2.5.组合模式：将对象组合成树形结构以表示“部分-整体”的层次结构。  
2.6.外观模式：对外提供一个统一的方法，来访问子系统中的一群接口。  
2.7.享元模式：通过共享技术来有效的支持大量细粒度的对象。
> 
> [3.行为型模式](#行为型模式) 类和对象如何交互，及划分责任和算法。
> 
> > 策略模式：定义一系列算法，把他们封装起来，并且使它们可以相互替换。  
模板模式：定义一个算法结构，而将一些步骤延迟到子类实现。  
[3.3.命令模式](#命令模式)：将命令请求封装为一个对象，使得可以用不同的请求来进行参数化。  
迭代器模式：一种遍历访问聚合对象中各个元素的方法，不暴露该对象的内部结构。  
观察者模式：对象间的一对多的依赖关系。  
仲裁者模式：用一个中介对象来封装一系列的对象交互。  
[3.7.备忘录模式](#备忘录模式)：在不破坏封装的前提下，捕获并保存对象的内部状态。  
解释器模式：给定一个语言，定义它的文法的一种表示，并定义一个解释器。  
状态模式：允许一个对象在其对象内部状态改变时改变它的行为。  
责任链模式：将请求的发送者和接收者解耦，使的多个对象都有处理这个请求的机会。  
访问者模式：不改变数据结构的前提下，增加作用于一组对象元素的新功能。  

参考：

> [【TypeScript】常见的设计模式_typescript设计模式_Sco_Jing1031的博客-CSDN博客](https://blog.csdn.net/m0_47109503/article/details/125868896)  
[设计模式 | 菜鸟教程](https://www.runoob.com/design-pattern/design-pattern-tutorial.html)  
[工厂方法 - 维基百科，自由的百科全书](https://zh.wikipedia.org/wiki/%E5%B7%A5%E5%8E%82%E6%96%B9%E6%B3%95)

<br />

### 设计模式的六大原则

**开闭原则**（Open Close Principle）

**对扩展开放，对修改关闭**。在程序需要进行拓展的时候，不去修改原有的代码，实现一个热插拔的效果。这样是为了使程序的扩展性好，易于维护和升级。想要达到这样的效果，我们需要使用接口和抽象类。

**里氏代换原则**（Liskov Substitution Principle）

任何基类可以出现的地方，子类一定可以出现。LSP 是继承复用的基石，只有当派生类可以替换掉基类，且软件单位的功能不受到影响时，基类才能真正被复用。  
**里氏代换原则是对开闭原则的补充**。实现开闭原则的关键步骤就是抽象化，而基类与子类的继承关系就是抽象化的具体实现，所以里氏代换原则是对实现抽象化的具体步骤的规范。

**依赖反转原则**（Dependence Inversion Principle）

**依赖反转原则是开闭原则的基础**。针对接口编程，依赖于抽象而不依赖于具体。

**接口隔离原则**（Interface Segregation Principle）

使用多个隔离的接口，比使用单个接口要好。它还有另外一个意思是：降低类之间的耦合度。由此可见，其实设计模式就是从大型软件架构出发、便于升级和维护的软件设计思想，它强调降低依赖，降低耦合。

**迪米特法则**（Demeter Principle）

又称最少知道原则：一个实体应当尽量少地与其他实体之间发生相互作用，使得系统功能模块相对独立。

**合成复用原则**（Composite Reuse Principle）

尽量使用合成 / 聚合的方式，而不是使用继承。

<br />

## 创建型模式

<hr />

创建型模式提供了在创建对象的同时隐藏创建逻辑的方式，而不是使用 `new` 运算符直接实例化对象。这使得程序在判断针对某个给定实例需要创建哪些对象时更加灵活。

### 工厂模式

**工厂模式**（Factory pattern），即通过工厂类 / 函数进行实例创建，而不直接使用 `new` 关键字。  
某些情况下创建实例前需要执行大量前置工作（如：判断创建合法性、对参数的预处理、对构造函数的解耦等），而将这些准备工作全部放入构造函数是非常危险的行为，有必要将 **创建实例的逻辑** 和 **使用实例的逻辑** 分开，方便以后扩展。

优点：屏蔽了产品的具体实现，当调用者想创建一个对象，只要知道其名称和工厂接口；  
缺点：每次增加一个产品时，都需同时实现工厂，在一定程度上增加了系统的复杂度，同时也增加了系统具体类的依赖。

**所有创建型模式都体现了工厂的思想**。

例如，当我们需要创建一个角色实例，需要通过字符串（`'warrior'` | `'rogue'`）判断角色类型（战士 | 盗贼），通过天赋点数（`talent`）为角色初始化属性（`strength` & `agility`）。以最直观的方式实现如下：

```ts
abstract class Actor {
	strength: number = 0;
	agility: number = 0;
	constructor() {}
}
class Warrior extends Actor {
	constructor(talent: number) {
		super();
		this.strength = talent;
	}
}
class Rogue extends Actor {
	constructor(talent: number) {
		super();
		this.agility = talent;
	}
}

let actor: Actor;
switch(type) {
	case 'warrior':
		actor = new Warrior(talent);
		break;
	case 'rogue':
		actor = new Rogue(talent);
		break;
	default:
		throw new Error('impossible error');
}
```

```ts
Warrior { strength: talent; aligity: 0 }
Rogue   { strength: 0; aligity: talent }
```

对于这种情况，为了提高可复用性，我们可以将创建角色实例的判断逻辑封装进 `actorFactory` 工厂函数。同时，对 `Warrior` 和 `Rogue` 的构造方法进行解耦：

```ts
abstract class Actor {
	constructor(public strength: number, public agility: number) {}
}
class Warrior extends Actor {}
class Rogue extends Actor {}

function actorFactory(type: 'warrior' | 'rogue', talent: number): Actor {
	switch(type) {
		case 'warrior':
			return new Warrior(talent, 0);
		case 'rogue':
			return new Rogue(0, talent);
		default:
			throw new Error('impossible error');
	}
}

let actor = actorFactory(type, talent); 
```

包括工厂模式在内，几乎所有的设计模式都会带来代码可读性下降的风险，因此需要找到代码可读性降低与可维护性和稳定性之间的平衡。

### 单例模式

**单例模式**（Singleton pattern），顾名思义单例对象的类必须保证只有一个实例存在。  
许多时候整个系统只需要拥有一个的全局对象，这样有利于我们协调系统整体的行为。

例如，项目中存在若干 actor 类的实例，但仅需要实现一个 actor 管理类的实例，此时就可以使用单例模式：

#### 饿汉方式

指全局的单例实例在类装载时即构建：

```ts
class ActorManager {
	private constructor() { ... }
	static Instance = new ActorManager();
}

let actorManager = ActorManager.Instance;
```

#### 懒汉方式

指全局的单例实例在第一次被使用时构建：

```ts
class ActorManager {
	private constructor() { ... }
	private static instance: ActorManager;

	static Instance() {
		if(!ActorManager.instance) {
			ActorManager.instance = new ActorManager();
		}
		return ActorManager.instance;
	}
}

let actorManager = ActorManager.Instance();
```

饿汉模式是绝对线程安全的，但程序执行中可能存在某些单例实例无需被调用的情况，这就浪费了部分内存空间。  
对于多数 OOP 语言，无锁的懒汉模式是不安全的，由于 JavaScript 是单线程的无需考虑线程安全问题。实际应用中推荐后者。

<br />

## 结构型模式

<hr />

结构型模式关注类和对象的组合。继承的概念被用来组合接口和定义组合对象获得新功能的方式。

> ### 代理模式
> 
> **代理模式**（Proxy Pattern），即创建具有现有对象的代理对象，以便向外界提供功能接口。  
在面向对象系统中，有些对象由于某些原因（比如对象创建开销很大，或者某些操作需要安全控制，或者需要进程外的访问），直接访问会给使用者或者系统结构带来很多麻烦，我们可以在访问此对象时加上一个对此对象的访问层。
> 
> 优点： 一、职责清晰；二、高扩展性；三、智能化。  
缺点： 一、由于增加了代理对象，因此有些类型的代理模式可能会造成请求的处理速度变慢；二、实现代理模式需要额外的工作，增加实现成本。
> 
> 例如，对 `actor` 对象的渲染：  
> 还没想到合适的例子……

<br />

## 行为型模式

<hr />

行为型模式特别关注对象之间的通信。

### 命令模式

**命令模式**（Command Pattern）是一种数据驱动的设计模式。用户请求以命令的形式包裹在对象中，并传给调用对象。调用对象寻找可以处理该命令的合适的对象，并把该命令传给相应的对象执行。 

使用场景：GUI 中每个交互操作、CMD 中每条指令都是一条命令。

例如，目前存在一个 `Actor` 类，我们想要对其实例对象进行操作，通常会这样实现：

```ts
class Actor {
	moveTo(direction: string) {
		console.log(`move ${direction}.`);
	}
}

let actor = new Actor();
actor.moveTo('left');
actor.moveTo('right');
```

如果我们将实例的每项行为都封装成为一条条命令，并且对 **命令发起者** 与 **命令实现者** 解耦进行解耦：

```ts
// 命令的封装
interface ICommand {
	execute(): void;
}
abstract class Command implements ICommand {
	constructor(public actor: Actor) {}
	abstract execute(): void;
}
class MoveLeftCommand extends Command {
	execute(): void {
		this.actor.moveTo('left');
	}
}
class MoveRightCommand extends Command {
	execute(): void {
		this.actor.moveTo('right');
	}
}

// Actor 是命令最终的实现者
class Actor {
	moveTo(direction: string) {
		console.log(`move ${direction}.`);
	}
}

// CommandExecutor 做为每条命令的发起者
class CommandExecutor {
	private commandQueue: Command[] = [];
	pushCommander(command: Command) {
		this.commandQueue.push(command);
	}
	execute() {
		// 遍历当前的命令队列发起命令
		while(this.commandQueue.length > 0) {
			// '!' 用于向编译器确保当前对象存在
			let command = this.commandQueue.shift()!;
			command.execute();
		}
	}
}

let actor = new Actor();
let executor = new CommandExecutor();
// 用户通过调用 CommandExecutor 录取命令队列、发起命令
executor.pushCommander(new MoveLeftCommand(actor));
executor.pushCommander(new MoveRightCommand(actor));
executor.execute();
```

命令模式下，使得对实例的操作更加复杂。但设想如果存在一个全局命令队列，所有的命令执行后进行入队操作，这样用户的整个操作流程就被完整的记录下来，这就是备忘录模式的核心思想。除了正向执行函数（`execute`），我们可以通过对命令类的拓展，添加撤回函数（`undo`）结合全局命令队列，实现命令的撤回操作。

### 备忘录模式

**忘录模式**（Memento Pattern）用于保存一个对象的某个状态，以便在适当的时候恢复对象。  
很多时候我们需要记录一个对象的内部状态，这样做的目的就是为了允许用户取消不确定或者错误的操作，恢复到原先的状态。如游戏存档、浏览器的后退功能、编辑器的撤回功能。

优点： 一、为用户提供了可以恢复状态的机制，可以回到某个特定历史状态。二、实现了信息的封装，使得用户不需要关心状态的保存细节。  
缺点：如果类的规模过于庞大，将会占用比较大的资源，而且每一次保存都会消耗一定的内存。

例如，当我们想记录下 `Actor` 的坐标变化：

```ts
// Memento 保存当前 Actor 状态的备份
class Memento {
	constructor(public x: number, public y: number) {}
}
// MementoManager 用于管理 Memento
class MementoManager {
	private memontoList: Memento[] = [];
	// 写入备份
	setMemonto(actor: Actor) {
		let memento = actor.recardAsMemento();
		this.memontoList.push(memento);
	}
	// 读出备份
	getMemonto(actor: Actor) {
		if(this.memontoList.length) {
			let memonto: Memento = this.memontoList.pop()!;
			actor.recoverFromMemento(memonto);
		} else {
			console.log('it\'s already the original state.');
		}
	}
}

class Actor {
	location: [number, number] = [0, 0];
	// Actor 的位移
	moveDelta(x: number, y: number) {
		this.location[0] += x;
		this.location[1] += y;
	}
	recardAsMemento() {
		return new Memento(this.location[0], this.location[1]);
	}
	recoverFromMemento(memento: Memento) {
		this.location = [memento.x, memento.y];
	}
}

let actor = new Actor();
let manager = new MementoManager();

actor.moveDelta(0, 1); // Actor { location: [ 0, 1 ] }
manager.setMemonto(actor);

actor.moveDelta(0, 2); // Actor { location: [ 0, 3 ] }
manager.setMemonto(actor);

manager.getMemonto(actor); // Actor { location: [ 0, 3 ] }
manager.getMemonto(actor); // Actor { location: [ 0, 1 ] }
manager.getMemonto(actor); // Actor { location: [ 0, 0 ] }
manager.getMemonto(actor); // it's already the original state.
```