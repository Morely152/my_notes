> 声明：本篇笔记部分摘自[《Java核心技术（卷Ⅰ） - 机械工业出版社》](https://detail.tmall.com/item.htm?ali_refid=a3_420434_1006%3A1151895243%3AN%3AoB1xLXSDdjSpCunkFwpZbCtvD%2B6YEaA9%3A39f8fcdda956d1ec63523e9a6e9e2355&id=708821240842&mi_id=0000mg2-P7Ustbzeym2_6DxuUMLCpndkVCAGc5EaA_l8QQ0&mm_sceneid=1_0_128421313_0&priceTId=2147831a17554253371677975e1dca&spm=a21n57.1.hoverItem.2&utparam=%7B%22aplus_abtest%22%3A%226b956865e0df43cd4a6620880d877f11%22%7D&xxc=ad_ztc)，遵循[CC BY 4.0协议](https://creativecommons.org/licenses/by/4.0/legalcode.zh-hans)。
> 存在由AI生成的小部分内容，仅供参考，请仔细甄别可能存在的错误。从本节开始，所有的代码片段都尽量保证完整性，可以直接复制粘贴到IDEA中运行（文件名需为Demo.java以便编译器能找到主类）。代码片段均经过实机运行检测，若运行结果与示例不同，欢迎在文末的评论区指出错误。

<center><h1>第一节 面向对象基础</h1></center>

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

面向对象编程则会将这一系列流程分为`对象`、`动作`（方法） 和 `字段`（属性）。如上例中的人、洗衣机和衣服可以看做是三个独立的对象；人具有放衣服、拿衣服两个动作；洗衣机具有洗衣服、烘干两个动作；而衣服具有是否干净以及是否在洗衣机里的属性。

这样一来，我们就给这些对象定义一系列的行为（又称作方法
、函数）以及属性；程序运行的逻辑就从面向过程的自上而下变成了对象之间的交互；这样的程序设计思想抽象程度更高，也很好地降低了代码之间的耦合度，是一种更加接近现实的程序设计思路。

> 关于上文中提到的“降低了代码之间的耦合度”，这里给出我的理解，或许不一定正确：
> 对于一个“人吃饭”的这么一个事儿，面向过程编程定义的函数一般是 **吃(人,饭)**，即参数中包含了人和饭两个对象；而面向对象编程中，一般将吃饭看做是人的行为，将饭作为人这个吃的行为设计到的另一个对象，于是写成 **人.吃(饭)**， 这样一来，无需像 **吃(人,饭)** 一样，既要考虑饭，还要考虑人，把人和饭绑定在一起。面向对象的写法中，"吃"是人的普遍行为，只要考虑是吃什么饭，而不用想是什么人来吃，这样就解除了人和饭的绑定，于是就让不同模块的代码之间的耦合度降低了。

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

子类应该是以下三种之一：

- `final`（不允许继续继承）
- `sealed`（进一步限制继承）
- `non-sealed`（开放继承）

与C++不同的是，Java中的继承只有 `public` 一种，没有 `protected` 继承和 `private` 继承。

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

上面这个程序看起来没啥问题，实际上将它粘贴到编辑器中，就会发现子类的构造函数报错了。提示 `'Person'中没有可用的无形参构造函数`。这是因为Java在构造子类的对象之前，必须先调用父类的构造方法，如果没有明确调用父类的构造方法，就会默认在子类的构造方法前面加上`super()`（很理所当然，先有父再有子么），然后一看发现父类没有这么一个无参数的构造函数，于是就无了…

想要解决这个问题也很简单，直接调用 `Person` 类存在的某个构造方法:

```java
class Student extends Person {
	protected int score;
	
	public Student(String name, int age, int score) {
	super(name, age); // 调用父类的构造方法Person(String, int)
	this.score = score;
 }
}
```

如果父类没有默认的构造方法，子类就必须显式调用 `super()` 并给出参数，以便让编译器定位到父类的一个合适的构造方法。同时也要注意，子类**不会**继承任何父类的构造方法。子类默认的构造方法是编译器自动生成的，不是继承自父类的。

## 3.继承的限制

不允许某个类被其他的类所继承，可以使用 `final` 修饰符打断继承。从Java 15开始，允许使用 `sealed` 修饰class，并通过 `permits` 明确写出能够从该class继承的子类名称（子类白名单）。

以下是一个演示Shape父类允许继承三种类型，以及子类的继承方式的写法：

```java
sealed class Shape permits Rect, Circle, Triangle { }   // 明确指定只能继承给这三个类
  
final class Rect extends Shape { }                      // √，这个类在允许继承的列表中
  
final class Circle extends Shape { }                    // √，这个类在允许继承的列表中
  
final class Triangle extends Shape { }                  // √，这个类在允许继承的列表中
  
final class Line extends Shape { }                      // ×，这个类不允许继承Shape类
  
public class Demo {  
    public static void main(String[] args) { }  
}
```

运行该程序，由于 `Shape` 类中没有允许 `Line` 类继承，该程序会报告如下的错误：

``` bash
java: 类不得扩展密封类：Shape（因为它未列在其 'permits' 子句中）
```

> 为了获得较好的编码体验，建议使用 Intellij IDEA 来进行Java的代码编写工作。IDEA有较好的错误反馈、完整的工具链支持（Git、数据库等），甚至能进行一些简单的代码补全。

## 4.向上转型

想想一个情景：我们定义了父类 `Person` 及子类 `Student`，当然可以使用这两句来分别将其实例化：

```java
Person p = new Person();
Student s = new Student();
```

诶，有的同学就要问了，能不能写成这样咧？
![](20250822100201538.png#sc)

```java
Person p = new Student();
```

蛙趣，竟然没报错！这是是因为 `Student` 继承自 `Person` ，因此，它拥有 `Person` 的全部功能，具备父类的所有属性，也能够执行父类所有的方法。这种把一个子类类型安全地变为父类类型的赋值，被称为**向上转型（upcasting）**，相当于使用子类的构造方法来构造一个父类的对象~~（怎么感觉有点倒反天罡…）~~

很容易就能猜到，向上转型是可以跨越多层的，比如说继承关系是 `a -> b -> c -> d`，d可以直接向上转型成a、b以及c这三类。

## 5.向下转型

和向上转型相反，如果把一个父类类型强制转型为子类类型，就是**向下转型（downcasting）**。向下转型通常使用强制类型转换实现：

```java
Person p =  new Person();
Person s = new Student();

Student s1 = (Student) s; // √,是相同的类型
Student s = (Stuident) p; // 将父类转型为子类呢？
```

由于子类通常比父类有更多的属性或方法，这样向下转型大多数情况下是不允许的，因为父类无法实现一些子类特有的方法。向下转型失败时，编译器会报告 `ClassCastException`。

为了避免向下转型出错，Java提供了 `instanceof` 操作符，可以先判断一个实例究竟是不是某种类型,写法为 `(a instanceof b)`，返回一个布尔值表示a是否与b类型相同，或者是否为b的子类。

```java
Person p = new Person();
System.out.println(p instanceof Person);  // true
System.out.println(p instanceof Student); // false

Student s = new Student();
System.out.println(s instanceof Person);  // true
System.out.println(s instanceof Student); // true

Student n = null;
System.out.println(n instanceof Student); // false
```

需要向下转型时，可以先判断是否可以转型：

```java
Person p = new Student();

if (p instanceof Student) {
	// 只有判断成功才会向下转型:
	Student s = (Student) p; // 一定可以成功
}
```

从Java 14开始，判断 `instanceof` 后，可以直接转型为指定变量，避免再次强制转型:

```java
public class Demo {  
    public static void main(String[] args) {  
        Object obj = "hello";  
        if (obj instanceof String s) {  
            // String s = (String) obj;这一句不用再写了
            System.out.println(s.toUpperCase());  
        }  
    }  
}
```

## 6.组合

```java
class Book {
	protected String name;
	...
}
```

对于这一个 `Book` 类，也具有 `name` 字段。但是如果想表示一个 `Student` 拥有一本书，无论是让 `Student` 继承 `Book` 还是让 `Book` 继承 `Student` 都显得不太对劲。这里将 `Book` 作为 `Student` 的属性就合理了：

```java
class Student extends Person {
	protected Book book;
	protected int score;
}
```

`Student` 作为 `Person` 的子类，是归属（is）的关系，即 `Student is Person`，这种关系适合使用继承。而对于 `Book`，应该是 `Student has Book` 的持有（has）关系，这种关系适合用组合。

# 五、多态

## 1.方法覆写

在继承关系中，子类如果定义了一个与父类方法签名（方法名称、参数类型、顺序及数量）完全相同的方法，被称为覆写（Override）。例如：

```java
	class Person {
		public void run() {
			System.out.println("人类跑步");
		}
	}
	
	
	class Student extends Person {
		public void run() {
			@Override
			System.out.println("学生跑步");
		}
	}
```

`Override`（方法覆写）和 `Overload`（方法重载）不同之处在于方法签名相同。如果不同就是`Overload`了，`Overload`定义的是一个新方法；如果方法签名相同，并且返回值也相同，就是 `Override`。如果方法签名相同，返回值不同，Java编译器会报告错误。

加上 `@Override` 可以明确告诉编译器这里是覆写方法，让它帮助我们检查是否进行了正确的覆写。这个符号不是必须添加的。即使对子类进行了父类方法的覆写，我们仍然可以通过 `super` 关键字调用父类中被覆盖掉的方法。

## 2.实现多态

现在考虑这样一个情况，子类 `Student` 继承了父类 `Person` ，并且覆写了 `run` 方法，那当我们通过 `Person p = new Student()` 创建一个实际类型为 `Person`，引用类型为 `Student`的变量，再调用`run`方法时，实际调用的是哪一个方法呢？

```java
class Person {  
    public void run() {  
        System.out.println("人类跑步");  
    }  
}  
  
  
class Student extends Person {  
    @Override  
    public void run() {  
        System.out.println("学生跑步");  
    }  
}  
  
public class Demo {  
    public static void main(String[] args) {  
  
        Person p = new Student();  
        p.run();  
  
    }  
}
```

大家可以自己粘贴到IDEA运行一下，这个例子很好地展现了**多态**这一面向对象的重要特性。结果会输出 `"学生跑步"`，即调用的是子类的覆写方法。

从而我们得出了重要结论：**Java的实例方法调用是基于运行时的实际类型的动态调用，而非变量的声明类型。**

这样做是有很大好处而且很方便的。例如在税务计算程序中，通过定义一个统一的 `Income` 父类和 `getTax()` 方法，然后为不同类型的收入（普通收入、工资收入、国务院津贴）创建子类并覆写各自的税率计算方法。在计算总税费时，只需处理 `Income` 父类类型，程序会根据实际对象类型自动调用相应的税率计算方法。
这样设计使得系统具有良好的扩展性 —— 新增收入类型时只需添加新的子类，无需修改现有的税务计算逻辑，实现了"对扩展开放，对修改关闭"的设计原则。

```java  
// 普通收入父类（10%收税）  
class Income {  
    protected double income;  
      
    public Income(double income) {  
        this.income = income;  
    }  
      
    public double getTax() {  
        return income * 0.1; // 税率10%  
    }  
}  
  
// 工资收入子类（阶梯收税）  
class Salary extends Income {  
      
    public Salary(double income) {  
        super(income);  
    }   
      
    @Override  
    public double getTax() {  
        if (income <= 5000) {  
            return 0;  
        }  
        return (income - 5000) * 0.2;  
    }  
}  
  
// 津贴收入子类（免税）  
class StateCouncilSpecialAllowance extends Income {  
    public StateCouncilSpecialAllowance(double income) {  
        super(income);  
    }  
      
    @Override  
    public double getTax() {  
        return 0;  
    }  
}

// 主类：程序入口
public class Demo {  
    public static void main(String[] args) {  
        // 给一个有普通收入、工资收入和享受国务院特殊津贴的小伙伴算税:  
        Income[] incomes = new Income[] {  
                new Income(3000),  
                new Salary(7500),  
                new StateCouncilSpecialAllowance(15000)  
        };  
        System.out.println(totalTax(incomes));  
    }  
      
    public static double totalTax(Income... incomes) {  
        double total = 0;  
        // 特别注意这里，只需要逐个调用各个对象的getTax方法，编译器会自动按照对象调用各自的getTax方法
        for (Income income: incomes) {  
            total = total + income.getTax();  
        }  
        return total;  
    }  
} 
```

## 3.覆写Object方法

Java中所有的类都继承自 `Object`，这个类定义了一些通用方法：

- `toString()`：转换为字符串格式
- `equals()`：判断两个对象是否相同
- `hashCode()`：计算对象的哈希值

我们也可以在自己的类中覆写这些方法（是不是有点像运算符重载）。

```java
class Person {  
	...
	
	@Override
    public boolean equals(Person other) {  
        if (this.name == other.name) {  
            return true;  
        } else {  
            return false;  
        }  
    }  
}
```

（这段代码有多处错误，你发现了吗？）

前面提到过，方法覆写需要定义完全相同的方法签名，其中包括了参数类型必须相同。所以这里需要改成 `Object` 类型。还要注意`Object` 类型不一定有 `name` 这么一个属性，需要向下转型成 `Person` 类才能确保可以比较。最后一个问题是 `String` 作为引用类型，需要调用 `equals` 方法来比较，不能使用 `==`。

最终修改好之后的程序是这样的：

```java
class Person {  
    String name;  
  
    public Person(String name) {  
        this.name = name;  
    }  
  
    @Override  
    public boolean equals(Object other) {  
        if (other instanceof Person) {  
            if (this.name.equals(((Person) other).name)) {  
                return true;  
            }  
        }  
        return false;  
    }  
}  
  
  
public class Demo {  
    public static void main(String[] args) {  
  
        Person p1 = new Person("John");  
        Person p2 = new Person("Jane");  
  
        System.out.println(p1.equals(p2));  
    }  
}

// 输出结果：false
```

# 六、抽象类

既然我们实现了多态，有的情况下子类各有各的实现方式，父类或许难以实现具体的方法来实现大一统，例如说描述各种动物的叫声。这个时候我们去掉父类方法的方法体肯定不行，去掉整个方法也不行，那咋办咧？

办不成也得办，跟编译器玩抽象↓

## 1. 抽象方法

如果父类的方法本身不需要实现任何功能，仅仅是为了定义统一的方法签名，目的是让子类去覆写它，那么可以使用关键字 `abstract` 把父类的方法声明为**抽象方法**（类似于C++中的纯虚函数）：

```java
class Person {
	public abstract void run();
}
```

把一个方法声明为 `abstract` ，表示它是一个抽象方法，本身没有实现任何方法语句。

## 2.抽象类

实际上就算定义了抽象方法，还是有没解决的问题：抽象方法本身是无法执行的，所以 `Person` 类也无法被实例化。编译器会告诉我们，无法编译 `Person` 类，因为它包含抽象方法。

那就干脆把 `Person` 类定义成抽象的，就引出了**抽象类**（类似于C++中的纯虚基类）：

```java
abstract class Person {
	public abstract void run();
}
```

抽象类可以强迫子类实现其定义的抽象方法，否则编译会报错。因此，抽象方法实际上相当于定义了“规范”。

# 七、接口

## 1.概述

这里的“接口”并不是指用于接收网络请求的接口，而是在Java中特指定义类的一种范式。在抽象类中，抽象方法本质上是定义子类的规范，自身没有具体的方法和含义。这样的情况下，我们可以使用 `interface` 将其改写为**接口**：

```java
interface Person {
	void run();
	String getName();
}
```

然后我们使用 `implements` 关键字，通过 `Person` 接口实现一个 `Student` 类：

```java
class Student implements Person {  
    String name;  
  
    Student(String name) {  
        this.name = name;  
    }  
  
    public void run() {  
        System.out.println("Student run");  
    }  
  
    public String getName() {  
        return name;  
    }  
}
```

需要注意的是，接口中不需要定义属性和构造函数，这些须在实现类中定义。一个类无法继承多个父类，但是可以实现多个接口。（接口是一种比抽象类更加抽象的存在。）

## 2. 接口继承

一个接口也可以继承自另一个接口。接口的继承同样使用 `extends` 关键字。若A继承B，A自动拥有B中定义的抽象方法。

``` java
interface Person {
	String getName();
}

interface Studen extends Person {
	String getClass();	
	// 通过继承，该接口自动拥有getName()抽象方法
}
```

## 3. default方法

在接口中，可以定义 `default` 方法。例如，把 `Person` 接口的 `run()` 方法改为 `default` 方法：

```java
interface Person {  
    String getName();  
    default void run() {  
        System.out.println(getName() + " is running.");  
    }  
}  
  
class Student implements Person {  
    private String name;  
    public Student(String name) {  
        this.name = name;  
    }  
    public String getName() {  
        return this.name;  
    }  
}  
  
public class Demo {  
    public static void main(String[] args) {  
  
        Student s = new Student("John");  
        s.run();  
  
    }  
}

// 输出：John is running.
```

实现类可以不必覆写 `default` 方法。 `default` 方法的目的是，当我们需要给接口新增一个方法时，需要给所有的子类也分别添加这个方法的具体实现。但如果新增的是 `default` 方法，相当是给全部子类都添加了这个方法；那么子类就不必全部修改，只需要在需要覆写的地方去覆写新增方法。 

`default` 方法和抽象类的普通方法是有所不同的。因为接口中没有类的属性， `default`方法无法访问属性，而抽象类的普通方法可以访问实例的属性。
# 八、静态字段和静态方法

（Java中习惯将类的“属性”和“函数”分别称为“字段”和“方法”，考虑到专业术语这里沿用这种称呼，大家看多了也会习惯的。实际上这里）

## 1.静态字段

来考虑这样一个情景：

一个班级中有很多位同学，都有自己的姓名等信息~~（这不废话么）~~，此时需要统计全班同学的人数。常见的思路是定义一个班级类 `Class`,将全班人数作为该类的一个属性进行处理。但是这样一来就会多定义一个不必要的类，同时还要在学生增减时关联 `Class` 类中属性的变化，既让代码变长了，还让不同类之间的耦合度变高了，不是一个很优雅的做法。

当然会有同学想到使用全局的公共变量来存储这个字段，但是在实际项目中，非必要情况下不建议使用全局变量。全局变量暴露在整个项目或文件中可以访问，无法确保不会被其他的逻辑意外修改，不能实现数据保护。

来看这样的写法：

```java
class Student {  
    String name;  
    static int number_of_stu;  
  
    Student(String name) {  
        this.name = name;  
        number_of_stu++;  
    }  
  
    public void graduate() {  
        number_of_stu--;  
    }  
}  
  
public class Demo {  
    public static void main(String[] args) {  
  
        Student s1 = new Student("小明");  
        System.out.println(s1.name + "同学进入班级后，学生人数为" + s1.number_of_stu);  
  
        Student s2 = new Student("小李");  
        System.out.println(s2.name + "同学进入班级后，学生人数为" + s2.number_of_stu);  
  
        Student s3 = new Student("小张");  
        System.out.println(s3.name + "同学进入班级后，学生人数为" + s3.number_of_stu);  
  
        s2.graduate();  
        System.out.println(s3.name + "同学毕业后，学生人数为" + s3.number_of_stu);  
    }  
}

/* 输出结果：
小明同学进入班级后，学生人数为1
小李同学进入班级后，学生人数为2
小张同学进入班级后，学生人数为3
小张同学毕业后，学生人数为2

*/
```

通过 `static` 关键字声明人数为**静态字段**，实现了 `Student` 对象对于人数这一属性的**共享**，通过在类被构造和回收（Java中没有析构方法，而且不建议在回收时进行处理，因为这样做不稳定，于是在这个例子中应用了毕业的情景来实现人数的减少）时对静态字段进行处理，这样无论访问的是哪一个对象的静态字段，得到的都是它们共享的值。

> 说句题外话，这个例子让我印象很深刻，因为它不仅展示了静态字段的特性和写法，还很好地展示了静态字段的使用场景。我们学习编程语言的一些特性时，**不仅要明白这些技术是什么，怎么写；更应该思考为什么这样做，什么情况下需要这样做。** 后者带来的是程序设计思想的提升和进阶，能够让我们在面对一些复杂的情境下仍然能写出优雅精简的程序，快准狠地解决实际问题。

## 2.静态方法

有静态字段，就有静态方法。用 `static` 修饰的方法称为静态方法。调用实例方法必须通过一个实例变量，而调用静态方法则不需要实例变量，通过类名就可以调用。

因为静态方法属于 `class` 而不属于实例，因此，静态方法内部，无法访问 `this` 变量，也无法访 问实例字段，它只能访问静态字段。 当然，通过实例变量也可以调用静态方法。

静态方法经常用于工具类。例如：

- Arrays.sort()
- Math.random()

静态方法也经常用于辅助方法。注意到Java程序的入口 `main()` 也是静态方法。

# 九、包



# 十、作用域



# 十一、内部类



# 十二、classpath和jar



# 十三、class版本



# 十四、模块



<center><h1>第二节 Java核心类</h1></center>

# 一、字符串和编码



# 二、StringBuilder



# 三、StringJoiner



# 四、包装类型



# 五、JavaBean



# 六、枚举类



# 七、记录类



# 八、BigInteger



# 九、BigDecimal



# 十、常用工具类



