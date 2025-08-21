---
categories:
  - 学习笔记
  - Java
tags:
  - Java
---

> 声明：本篇笔记部分摘自[《Java核心技术（卷Ⅰ） - 机械工业出版社》](https://detail.tmall.com/item.htm?ali_refid=a3_420434_1006%3A1151895243%3AN%3AoB1xLXSDdjSpCunkFwpZbCtvD%2B6YEaA9%3A39f8fcdda956d1ec63523e9a6e9e2355&id=708821240842&mi_id=0000mg2-P7Ustbzeym2_6DxuUMLCpndkVCAGc5EaA_l8QQ0&mm_sceneid=1_0_128421313_0&priceTId=2147831a17554253371677975e1dca&spm=a21n57.1.hoverItem.2&utparam=%7B%22aplus_abtest%22%3A%226b956865e0df43cd4a6620880d877f11%22%7D&xxc=ad_ztc)，遵循[CC BY 4.0协议](https://creativecommons.org/licenses/by/4.0/legalcode.zh-hans)。
> 存在由AI生成的小部分内容，仅供参考，请仔细甄别可能存在的错误。从本节开始，所有的代码片段都尽量保证完整性，可以直接复制粘贴到IDEA中运行（文件名需为Demo.java以便编译器能找到主类）。代码片段均经过实机运行检测，若运行结果与示例不同，欢迎在文末的评论区指出错误。
# 一、引入

## 1.面向对象概述

> 引言：面向对象编程，是一种通过对象的方式，把现实世界映射到计算机模型的一种编程方法。

拿洗衣服为例，涉及到以下流程：

```
人放衣服到洗衣机里 -> 洗衣机洗衣服 -> 洗衣机甩干 -> 人取出衣服晾晒
```

经典的面向过程编程，会实现这几个函数然后依次调用：

```
pushClothes(person, machine);
washClothes(machine, clothes);
dryingClothes(machine, clothes);
fetchClothes(person, machine);
```

面向对象编程则会将这一系列流程分为`对象 `、`动作` 和 `属性`。如上例中的人、洗衣机和衣服可以看做是三个独立的对象；人具有放衣服、拿衣服两个动作；洗衣机具有洗衣服、烘干两个动作；而衣服具有是否干净以及是否在洗衣机里的属性。

这样一来，我们就给这些对象定义一系列的行为（又称作方法
、函数）以及属性；程序运行的逻辑就从面向过程的自上而下变成了对象之间的交互；这样的程序设计思想抽象程度更高，也很好地降低了代码之间的耦合度，是一种更加接近现实的程序设计思路。

> 关于上文中提到的“降低了代码之间的耦合度”，这里给出我的理解，或许不一定正确：
> 对于一个“人吃饭”的这么一个事儿，面向过程编程定义的函数一般是 **吃(人,饭)**，即参数中包含了人和饭两个对象；而面向对象编程中，一般将吃饭看做是人的行为，将饭作为人这个吃的行为设计到的另一个对象，于是写成 **人.吃(饭)**， 这样一来，方法中只需要考虑对饭进行处理，而无需像 **吃(人,饭)** 一样，既要考虑饭，还要考虑人，把人和饭绑定在一起。面向对象的写法中，"吃"是人的普遍行为，只要考虑是吃什么饭，而不用想是什么人来吃，这样就解除了人和饭的绑定，于是就让不同模块的代码之间的耦合度降低了。

需要注意以下两点：

1. 面向对象中的“对象”不一定是具象化的物体，比如说小狗、桌子等，也可以是一个抽象的概念，如成绩（具有分数、绩点等属性；修改分数、计算绩点等方法）、字符串（具有长度、内容等属性；计算长度、替换内容等方法）。
2. 虽然面向对象看起来比面向过程看起来更加高深莫测，在实际应用时也各有各的优势，不能认为学习了面向对象就看不上/用不着面向过程了。
3. 类与对象的关系：类是概念，对象是概念衍生出的实例，即 `类 --实例化--> 对象`。如同所有的人统称为“人类”，每一个人都是“人类”这一概念下的实体对象。

## 2.重点学习方向

本篇笔记重点学习Java中面向对象的以下内容：

1. 基本概念
	- 类
	- 实例
	- 方法
2. 面向对象特性
	- 继承
	- 多态
3. Java提供的一些机制
	- package
	- classpath
	- jar
4. Java核心类
	- 字符串
	- 包装类型
	- JavaBean
	- 枚举
	- 常用工具类

最后同样需要提醒的是，即使学习了面向对象这个程序设计的思想，也不能保证能找到对象🤣。

![](20250821160749656.png)

# 二、面向对象基础

## 1.一个简单的demo

```java
// 蛋糕类
class Cake {
    // 属性：蛋糕的数量
    private int count;

    // 构造方法：创建蛋糕对象时初始化数量
    public Cake(int count) {
        this.count = count;
    }

    // 获取当前蛋糕数量
    public int getCount() {
        return count;
    }

    // 蛋糕被吃：数量减一
    public void beEaten() {
        if (count > 0) {
            count--;
            System.out.println("蛋糕被吃了一块，剩余数量: " + count);
        } else {
            System.out.println("已经没有蛋糕了！");
        }
    }
}

// 人类
class Person {
    // 属性：人的名字
    private String name;

    // 构造方法：创建人对象时设置名字
    public Person(String name) {
        this.name = name;
    }

    // 进食方法：人吃蛋糕
    public void eat(Cake cake) {
        System.out.println(name + "开始吃蛋糕...");
        // 调用蛋糕的被吃方法
        cake.beEaten();
    }

    // 获取名字
    public String getName() {
        return name;
    }
}

// 主类：程序入口
public class Demo {

    public static void main(String[] args) {

        // 创建一个有3块蛋糕的对象
        Cake cake = new Cake(3);
        // 创建一个名叫"小明"的人
        Person xiaoming = new Person("小明");

        System.out.println("初始蛋糕数量: " + cake.getCount());

        // 小明吃蛋糕
        xiaoming.eat(cake); // 第一次吃
        xiaoming.eat(cake); // 第二次吃
        xiaoming.eat(cake); // 第三次吃
        xiaoming.eat(cake); // 第四次吃（尝试吃已经吃完的蛋糕）
        
        System.out.println("最终蛋糕数量: " + cake.getCount());
    }
}
```

Java中类的声明、属性和方法的定义、实例化以及方法的调用都和C++几乎相同，这里不再赘述；有C++编程经验的同学应该都能看懂上面这个简单的例子。运行程序，输出了小明吃蛋糕的过程：

```java
初始蛋糕数量: 3
小明开始吃蛋糕...
蛋糕被吃了一块，剩余数量: 2
小明开始吃蛋糕...
蛋糕被吃了一块，剩余数量: 1
小明开始吃蛋糕...
蛋糕被吃了一块，剩余数量: 0
小明开始吃蛋糕...
已经没有蛋糕了！
最终蛋糕数量: 0
```

## 2.数据保护

在面向对象程序设计时，为了防止外部的程序读写对象的属性，从而引起意料之外的错误，通常把属性设置为 `private` 或 `protected`（需要继承时）,然后通过定义读写的方法 `getXXX()` 或 `setXXX()` 来向外暴露接口，其中包含数据验证和读写等逻辑，通过调用这些方法来读取和修改对象中的属性。如下例：

```java
class Student {  
    private String name = "";  
  
    public Student(String name) {  
        this.name = name;  
    }  
  
	// 读取name的值
    public String getName() {  
        return name;  
    }  
  
	// 写入name的值，并进行数据验证
	public void setName(String name) {  
	    if (!name.isEmpty() && name != null) {  
	        this.name = name;  
	    }  
	}
}  
  
public class Demo {  
    public static void main(String[] args) {
      
        Student s1 = new Student("张三 ");  
    
	    System.out.println(s1.getName());  
        s1.setName("李四");  
        System.out.println(s1.getName());  
    
    }  
}
```

运行程序，先输出了对象 `s1` 的 `name` 初始值“张三”，然后输出了修改后的值“李四”。

## 3.this变量

在方法内部，可以使用一个隐含的变量 `this` ，它始终指向当前实例。因此，通过 `this` 就可以访问当前实例的字段。

```java
class Number {  
    private int num;  
  
    public Number(int num) {  
        this.num = num;  
    }  
  
    // 两数相加
    public int addTo(Number other) {  
        this.num = this.num + other.num;  
        return this.num;  
    }  
}  
  
public class Demo {  
    public static void main(String[] args) {  
  
        Number n1 = new Number(1);  
        Number n2 = new Number(2);  
  
        System.out.println(n1.addTo(n2));  
  
    }  
}
```

> 小技巧：两个同类的对象进行操作时（如累加、比较），可以在对应的方法形参中将另一个对象命名为 `other`，这样使用`this` 和 `other` 就可以很清晰地弄清楚是在对哪个对象进行操作。

## 4.可变参数

```java
import java.util.Arrays;  
  
class Numbers {  
    private int[] nums;  
  
    // 这里参数其实可以直接写 int[] nums,这里是为了演示可变参数
    public Numbers(int... nums) {  
        this.nums = nums;  
    }  
  
    public void printOut() {  
        System.out.println(Arrays.toString(this.nums));  
    }  
}  
  
public class Demo {  
    public static void main(String[] args) {  
  
        Numbers numbers = new Numbers(1, 3, 4, 5, 6);  
        numbers.printOut();  
  
    }  
}
```

"可变参数"主要"变"的是参数的个数，实现类似于数组的效果。通过指定形参为 `数据类型... 形参名` 的格式,可以指定这个参数是可变的。可变参数须位于参数列表的末尾，以免混淆前面参数的一一对应关系。

（这里的三个点是英文输入法下的句号 `...` 哈，不要打成中文省略号 `…` 【Shift + 6】了。编程语言中字符串之外的符号应该都是英文符号。）

# 三、构造方法

与C++类似，Java的构造方法须与类同名，没有返回值，且被声明为 `public`（不然外部都无法调用构造函数来进行实例化）。一个类可以有多个参数不同的构造方法（即方法重载，其实一般的方法也支持重载），编译器会根据参数自动选择执行相应的构造方法。

一个构造函数还可以调用其他的构造函数以提高代码复用率：

```java
// 注意：此代码片段不完整
class Person {
	private String name;
	private int age;
	
	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
	
	public Person(String name) {
		this(name, 18); // 调用另一个构造方法Person(String, int)
	}
	
	public Person() {
		this("Unnamed"); // 调用另一个构造方法Person(String)
	}
}
```

需要注意的是，Java**不支持**C++中通过成员初始化列表定义构造函数，成员变量的赋值必须在函数体中进行：

```java
public Person(String n) : name(n); // ×，不支持这样的写法

public Person(String n) {          // √，这样写是正确的
	this.name = name;
}
```

# 四、继承

## 1.概述

使用关键字 `extend` 继承一个现有的类，会自动拥有它的属性和方法。

```java
class Person {  
  ...
}  

class Student extends Person {  
  ...
}
```

被继承的类被称作父类、基类；相对而言继承父类的类被称作子类、派生类。注意到这里的 `Person` 类没有 `extend` ,这种情况下编译器自动添加 `extend Object`,即上例的继承树是这样的：

```
┌───────────┐
│   Object  │
└───────────┘
	  ▲
	  │
┌───────────┐
│  Person   │
└───────────┘
      ▲
      │
┌───────────┐
│  Student  │
└───────────┘
```

Java只允许一个class继承自一个类，因此一个类有且仅有一个父类。`Object` 比较特殊，它没有父类。

与C++相同，继承类也无法访问父类的 `private` 属性或方法，只能访问 `public` 和 `protected`。想要在子类中访问父类的属性，需要像 `this` 一样使用 `super` 关键字。

## 2.继承的构造过程

```java
class Person {  
    protected String name;  
    protected int age;  
    public Person(String name, int age) {  
        this.name = name;  
        this.age = age;  
    }  
}  
  
class Student extends Person {  
    protected int score;  
    public Student(String name, int age, int score) {  
        this.score = score;  
    }  
}  
  
public class Demo {  
    public static void main(String[] args) {  
        Student s = new Student("Xiao Ming", 12, 89);  
    }  
}
```

上面这个程序看起来没啥问题，实际上将它粘贴到编辑器中，就会发现子类的构造函数报错了。提示 `'Person'中没有可用的无形参构造函数`。这是因为Java在构造子类的对象之前，必须先调用父类的构造方法（很理所当然，先有父再有子么），然后一看发现父类
