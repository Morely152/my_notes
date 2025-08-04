---

---
--- 
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

### super()方法

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

这里的派生类包含了构造函数，必须调用`super()`方法来执行基类的构造函数。在够赞函数中访问