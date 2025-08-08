---

---
--- 
# 一、函数
## 1.介绍

> 函数是JavaScript应用程序的基础。它帮助你实现抽象层，模拟类，信息隐藏和模块。在TypeScript里，虽然已经支持类，命名空间和模块，但函数仍然是主要的定义**行为**的地方。TypeScript为JavaScript函数添加了额外的功能，让我们可以更容易地使用。

## 2.函数声明

和JavaScript一样，TypeScript函数可以创建有名字的函数和匿名函数。 你可以随意选择适合应用程序的方式，不论是定义一系列API函数还是只使用一次的函数。

```ts
// 有名函数（相对匿名而言，不是说这个函数有多出名…）
function add(x, y) {
    return x + y;
}

// 匿名函数
let myAdd = function(x, y) { return x + y; };
```

在JavaScript里，函数可以使用函数体外部的变量。 当函数这么做时，我们说它‘捕获’了这些变量。

```ts
let z = 100;

function addToZ(x, y) {
	return x + y + z;
}
```

## 3.函数类型

函数的类型，通常指返回值的数据类型；当我们指定函数类型后，return的数据类型将自动成为函数的类型。需要注意的是，即使函数没有返回值，也要写明函数类型为void，不可以忽略。

```ts
function add(x: number, y: number): number {
    return x + y;
}

let myAdd = function(x: number, y: number): number { return x + y; };

// 还可以这样写：
let myAdd: (x:number, y:number) => number =
    function(x: number, y: number): number { return x + y; };
```

## 4.类型推断

就算仅在等式的一侧带有类型，TypeScript编译器仍可正确识别类型；这种“按上下文归类”，是类型推论的一种。 它帮助我们更好地为程序指定类型。

```ts
// 这个myAdd函数拥有完整的函数类型
let myAdd = function(x: number, y: number): number { return x + y; };

// 这里的x和y同样会推断出是number型
let myAdd: (baseValue: number, increment: number) => number =
    function(x, y) { return x + y; };
```

## 5.可选参数和默认参数

TypeScript里的每个函数参数都是必须的。可以将`null`或`undefined`作为参数，但是编译器会检查我们是否为每个参数都传入了值。即实参与形参必须一一对应。在TypeScript里我们可以在参数名旁使用`?`实现可选参数的功能（有点像C#？）。可选参数必须跟在必须参数后面。即如果想让last name是可选的，那么应当把last name放在后面。

```ts
function buildName(firstName: string, lastName?: string) {
    if (lastName)
        return firstName + " " + lastName;
    else
        return firstName;
}

let result1 = buildName("Bob");                    // √，可以省略后一个参数
let result2 = buildName("Bob", "Adams", "Sr.");    // ×，参数太多了
let result3 = buildName("Bob", "Adams");           // √，正好
```

在TypeScript里，当用户没有传递这个参数或传递的值是`undefined`时，我们也可以为参数提供一个默认值。它们叫做有默认初始化值的参数。

```ts
function buildName(firstName: string, lastName = "Smith") {
    return firstName + " " + lastName;
}

let result1 = buildName("Bob");                  // √
let result2 = buildName("Bob", undefined);       // √
let result3 = buildName("Bob", "Adams", "Sr.");  // ×，参数太多了
let result4 = buildName("Bob", "Adams");         // √，也可以不用默认值
```

## 6.剩余参数（不定量参数）

有时难以确定参数的个数，可以使用`···`接收一系列个数不确定的参数，存储在数组中。

```ts
function buildName(firstName: string, ...restOfName: string[]) {
  return firstName + " " + restOfName.join(" ");
  // 这里join方法用于将数组的各个元素使用空格串起来，即”Samuel Lucas Mackinzie“
}

let employeeName = buildName("Joseph", "Samuel", "Lucas", "MacKinzie");
```

## 7. 函数重载

JavaScript本身是个动态语言。 JavaScript支持同名函数根据传入不同的参数而返回不同类型的数据，即支持函数重载。

```ts
function add_things(par_1:number, par_2:number):number {
	return a + b;
}


```

# 二、泛型

在像C#和Java这样的语言中，可以使用`泛型`来创建可重用的组件，一个组件可以支持多种类型的数据。 这样用户就可以以自己的数据类型来使用组件。

有一个方法`identity`，负责返回输入的内容（类似于`echo`）：

```ts
function identity(arg: any): any {
    return arg;
}
```

这样做存在问题，返回值会一直是any类型，即丢失了参数的数据类型信息。因此，我们需要一种方法使返回值的类型与传入参数的类型是相同的。 这里，我们使用了**类型变量**，它是一种特殊的变量，只用于表示类型而不是值。





# 三、接口



--- 
# 参考资料

[^1]: TSDoc.TS手册指南v1-函数\[EB/OL].(2023-04-28)\[2025-08-05]. https://fxzer.github.io/tsdoc-vitepress/zh/handbooks/handbook-v1/Functions
[^2]: TSDoc.TS手册指南v1-泛型\[EB/OL].(2023-04-28)\[2025-08-05]. https://fxzer.github.io/tsdoc-vitepress/zh/handbooks/handbook-v1/Generics
[^3]: TSDoc.TS手册指南v1-接口\[EB/OL].(2023-04-28)\[2025-08-05]. https://fxzer.github.io/tsdoc-vitepress/zh/handbooks/handbook-v1/Interfaces
