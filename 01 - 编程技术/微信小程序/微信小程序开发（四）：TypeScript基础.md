---

---
--- 
> 本篇笔记部分内容整理自网络中的一些文章及视频，详见文末的“参考资料”部分。存在部分AI生成的内容，请仔细甄别可能存在的错误。
--- 
# 零、如何运行TypeScript程序

（刚刚才发现好像还没研究怎么运行写好的TypeScript程序，在这里补充一下：）

虽然说Typescript是JavaScript的超集，兼容大多数JavaScript的写法与特性，但是




# 一、TypeScript中的类

## 1.类的声明与使用

以下是使用TypeScript定义一个`Greeter`类并使用的例子：

```ts
class Greeter {
    greeting: string;                   // 属性
    constructor(message: string) {      // 构造函数
        this.greeting = message;
    }
    greet() {                           // 方法
        return "Hello, " + this.greeting;
    }
}

let greeter = new Greeter("world");
```

这样的定义方法与C#或Java的写法类似，其中有三种成员：**属性**、**构造函数**和**方法**。

注意到我们在引用任何一个类成员的时候都用了`this`。 它表示我们访问的是类的成员。
最后一行，我们使用`new`构造了`Greeter`类的一个实例。 它会调用之前定义的构造函数，创建一个`Greeter`类型的新对象，并执行构造函数初始化它。

## 2.类的继承

在TypeScript里，我们可以使用常用的面向对象模式，包括通过继承来拓展现有的各种类。

```ts
class Animal {
    move(distanceInMeters: number = 0) {
        console.log(`Animal moved ${distanceInMeters}m.`);
    }
}

class Dog extends Animal {
    bark() {
        console.log('Woof! Woof!');
    }
}

const dog = new Dog();
dog.bark();
dog.move(10);
dog.bark();
```

Dog类通过`wxtern`关键字实现了对Animal**基类**的**派生**。派生类也称子类，基类也称作是超类。派生类继承了基类的属性和方法，也有着自己定义的独特的属性和方法。

### ① super()方法

``` ts
class Animal {
    name: string;
    constructor(theName: string) { this.name = theName; }
    move(distanceInMeters: number = 0) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}

class Snake extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 5) {
        console.log("Slithering...");
        super.move(distanceInMeters);
    }
}

class Horse extends Animal {
    constructor(name: string) { super(name); }
    move(distanceInMeters = 45) {
        console.log("Galloping...");
        super.move(distanceInMeters);
    }
}

let sam = new Snake("Sammy the Python");
let tom: Animal = new Horse("Tommy the Palomino");

sam.move();
tom.move(34);
```

这里的派生类包含了构造函数，必须调用`super()`方法来执行基类的构造函数。在构造函数中访问`this`的属性之前，**一定**要调用`super()`方法，这是TypeScript的重要规则。这里的例子在派生类中重写了父类的`move`方法，使得`move`方法根据不同的类具有不同的功能(实现了面向对象的**多态**特征)。

### ② 数据保护与可见性

在TypeScript中，未明确指定可见性的属性与方法，其可见性默认为`public`，也可以明确指定一些属性及方法为`public`：

```ts
class Animal {
    public name: string;
    public constructor(theName: string) { this.name = theName; }
    public move(distanceInMeters: number) {
        console.log(`${this.name} moved ${distanceInMeters}m.`);
    }
}
```

与C++类似，除标记为`public`外，属性与方法也可标记为`private`，此时这些属性与方法不能再声明它的类外部访问。标记为`protected`时，仅能在类内和派生类中可以访问。如：

```ts
class Animal {
    private name: string;
    constructor(theName: string) { this.name = theName; }
	
	public showName() {
		console.log(this.name);
	}
}

let my_pet = new Animal("Cat");   
let name = my_pet.name;           // 错误: 'name' 是私有的.
my_pet.showName();                // 正确，可以输出Cat
```

尝试访问非可见的方法或属性，无法通过编译：
![](20250804203545990.png)

除此之外，还可以使用`readonly`关键词将一些属性设定为只读的，它们必须在声明时或构造函数中被初始化，且后续无法修改。例如：

```ts
class Octopus {
    readonly name: string;
    readonly numberOfLegs: number = 8;
    constructor (theName: string) {
        this.name = theName;
    }
}
let dad = new Octopus("Man with the 8 strong legs");
dad.name = "Man with the 3-piece suit"; // 错误! name 是只读的.
```

> 题外话：《The Man with Eight Pairs of Legs》是 Leslie Kirk Campbell 的一部短篇小说，讲述了身材高挑、一生都饱受身体羞耻困扰的高中教师 Harriet Rogers，在当地酒吧结识了矿工 Callahan，Harriet 发现 Callahan 戴着假肢。Callahan 是当地矿井爆炸的受害者，随着两人关系的发展，他向 Harriet 展示了多对假肢，这也是书名中 “八对腿” 的由来。Callahan 想要参加佳能城的年度马拉松比赛，成为首位使用假肢参赛的选手，而他们的关系也在 Callahan 训练的过程中面临着各种挑战。
> 
> （上例中“Man with the 8 strong legs”或许来源于此？）

### ③ 参数属性

**参数属性**可以方便地让我们在一个地方定义并初始化一个成员。如上例中的构造函数，也可以写成这样：

```ts
class Animal {
	constructor(private name: string) { }                   // 使用参数属性的构造函数
	
	private name: string;                    
	constructor (theName: string) { this.name = theName; }  // 之前的构造函数
}
```


相当于`let a:number; a = 1;`和`let a: number = 1;`一样，直接把声明和赋值合并在一起，更加方便快捷。

