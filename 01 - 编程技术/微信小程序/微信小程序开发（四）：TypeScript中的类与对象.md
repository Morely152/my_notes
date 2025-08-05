---

---
--- 
> 本篇笔记部分内容整理自网络中的一些文章及视频，详见文末的“参考资料”部分。存在部分AI生成的内容，请仔细甄别可能存在的错误。
--- 
# 〇、如何将TypeScript链接到HTML中

（刚刚才发现好像还没研究怎么运行写好的TypeScript程序，在这里补充一下：）

虽然说Typescript是JavaScript的超集，兼容大多数JavaScript的写法与特性，但是TypeScript并不能像JavaScript一样，直接通过`<script>`连接到html文件中，
![](20250804222824572.png#sc)

想要在HTML中链接ts程序，需要通过tsc（TypeScript Compiler，TypeScript编译器）将ts编译成js文件，然后将js链接到HTML。

首先运行以下命令，通过npm安装tsc:
``` bash
npm install -g typescript
```

然后`cd`到目标TypeScript文件所在的文件夹，执行以下命令：
```bash
tsc 文件名
```

于是在ts文件旁边会生成一个同名的js文件，即为ts文件编译之后的JavaScript程序，可将其链接到HTML中。

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

## 3.super()方法

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

## 4.数据保护与可见性

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
![](20250804203545990.png#bc)

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

### ④ 存取器

TypeScript支持通过getters/setters来截取对对象成员的访问。通过关键字`get`与`set`指定这些方法，能够帮助我们有效地控制对成员的访问。

```ts
const fullNameMaxLength = 10;

class Employee {
    private _fullName: string;

    get fullName(): string {
        return "Employee's name is" + this._fullName;
    }

    set fullName(newName: string) {
        if (newName && newName.length > fullNameMaxLength) {
            throw new Error("fullName has a max length of " + fullNameMaxLength);
        }

        this._fullName = newName;
    }
}

let employee = new Employee();
employee.fullName = "Bob Smith";

if (employee.fullName) {
    alert(employee.fullName);
}
```

正确情况下，输出的结果：（alert弹窗提醒）
![](20250804214722750.png#sc)

setter捕获到错误的情况下，输出的结果：（无alert弹窗，控制台报错）
![](20250804214924992.png![](20250804214924992.png#sc)#sc)

上例中，为_fullName的读取和修改操作定义了getter和setter，有点类似于C++中的cin/cout运算符重载，修改了对对象元素进行访问和修改时的逻辑，如访问时不仅返回属性值，还加上“Employee's name is”这样的信息；修改元素时先判定长度是否小于10，否则报告错误等。

### ⑤ 静态属性

我们也可以通过`static`关键字创建类的静态成员，这些属性存在于类本身上面而不是类的实例上。静态属性通常有两种用途：
- 定义所有对象的公共特征，如平面直角坐标系中的图形对象，拥有相同的原点，可以定义`static origin:number[] = [0, 0];`
- 统计某个类的对象个数，通过静态属性`static cnt = 0;`然后分别在类的构造函数与析构函数中分别使其++或--，此时访问任何一个对象的`cnt`属性，都等于这一类的对象个数。

### ⑥ 抽象类（虚基类）

抽象类做为其它派生类的基类使用。它们一般不会直接被实例化（类似于C++中的虚基类）。不同于接口，抽象类可以包含成员的实现细节（抽象类中除抽象函数（虚函数）之外，其他函数可以包含具体实现）。`abstract`关键字是用于定义抽象类和在抽象类内部定义抽象方法。

``` ts
abstract class Animal {
    abstract makeSound(): void;
    move(): void {
        console.log("roaming the earth...");
    }
}
```

抽象类中的抽象方法不包含具体实现并且必须在派生类中实现。 抽象方法的语法与接口方法相似。两者都是定义方法签名但不包含方法体。然而，抽象方法必须包含`abstract`关键字并且可以包含访问修饰符。

``` ts
abstract class Department {
    constructor(public name: string) { }
    
    printName(): void {
        console.log('Department name: ' + this.name);
    }
    
    abstract printMeeting(): void; // 必须在派生类中实现
}

class AccountingDepartment extends Department {
    constructor() {
        super('Accounting and Auditing'); // 在派生类的构造函数中必须调用 super()
    }
    
    printMeeting(): void {
        console.log('The Accounting Department meets each Monday at 10am.');
    }
    
    generateReports(): void {
        console.log('Generating accounting reports...');
    }
}

let department: Department; // 允许创建一个对抽象类型的引用

department = new Department(); // 错误: 不能创建一个抽象类的实例
department = new AccountingDepartment(); // 允许对一个抽象子类进行实例化和赋值

department.printName();
department.printMeeting();
department.generateReports(); // 错误: 方法在声明的抽象类中不存在
```