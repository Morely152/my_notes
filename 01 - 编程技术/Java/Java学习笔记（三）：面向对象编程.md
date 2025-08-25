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

最后同样需要提醒的是，即使学习了面向对象的程序设计思想，也不能保证能找到对象🤣。

![](20250821160749656.png#sc)

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

## 3.接口的静态字段

接口作为纯抽象类，无法定义普通的属性，但是可以定义静态字段。静态字段在接口中必须声明为 `final` 类型：

```java
public interface Person {
	puiblic static final String GENDER = "female";
	// 也可以简写成 String GENDER = "FEMALE";
}
```

# 九、包

## 1.概述

有时两个开发者定义了相同的类名，就会产生冲突。为了解决这种冲突，可以考虑使用一种方法来限定类的作用范围，在C++中称之为**命名空间**，Java中则称之为**包**。

Java推荐使用包将不同模块的类放到不同的文件中，根据文件夹的组织形式定义包的名称，在文件的第一行使用 `package` 关键字定义包名，然后在包中将类定义为 `public`。

例如，小明和小军一起进行开发，他们都定义了Person这个类，文件夹的组织形式如下：

```
Demo_project
└─ src
	├─ Demo.java
	├─ jun
	│    └─ Person.Java  
	├─ components
	│    └─ ming
			└─ Person.java
```

那么，小军的Person.Java应该这样写：

```java
package jun;

public class Person {...}
```

```java
package conponents.ming;

public class Person {...}
```

从这个例子中可以看出包可以是多层结构，用 `.` 隔开。例如： `java.util`。但是需要注意包没有父子的关系，如 `java.util` 和 `java.util.zip` 是不同的包，两者之间没有继承关系。

一个类总是属于某个包的。如果没有声明，这个类就属于默认包。类的完整名称是 `包.类` ,JVM根据这个完整的名称来辨识不同包中的类。即只要包不同，即使类名相同也，不是同一个类。

## 2.包作用域

位于同一个包的类，可以访问包作用域的字段和方法。不用 `public` 、 `protected` 、 `private` 修饰的字段和方法就是包作用域。

## 3.包的导入

有时我们需要在一个包中使用另一个包中的类，这时就需要使用 `import` 导入另外的包了。有三种导入方法：

### ① 直接写完整类名

```java
conponents.ming person = new conponents.ming.Person();
```

### ② 使用 import 语句

```java
package jun;
import conponents.ming.Person;

Person person = new Person();
```

这里可以使用通配符 `*` 来导入包中的所有类,如 `import conponents.ming.*;`

### ③ 导入另一个包的静态方法和静态字段

```java
package jun;
import static conponents.ming.Person;
```

这种方式使用得较少。

## 4.避免包的重名

通常为了防止出现包的重名，需要确定包名唯一，一般使用域名反写：

```
org.apache
come.xiaomi.bluetooth
top.morely.blog
```

也要注意不要与Java中已有的类和重名：

- String
- System
- Runtime
- java.util.List
- java.text.Format
- java.math.BigInteger
- ...

# 十、作用域

与C++类似，类中的字段和方法有 `public` 、 `private` 以及 `protected` 三种，`public` 可以在类外访问，`private` 只能在类内访问，而 `protected` 多用于在继承中将方法或字段暴露给子类。

另外，包作用域是指一个类允许访问同一个包的没有 `public`、`private` 修饰的 `class` ，以及没有 `public`、`protected`、`private` 修饰的字段和方法。

在方法内部定义的变量称为**局部变量**，局部变量作用域从变量声明处开始到对应的块结束。方法参数也是局部变量。Java中不建议使用全局变量，这样做不利于模块化以及数据保护。

我们来回顾一下 `final` 这个修饰符，它在数据保护中有“终止”的意味，包括终止类的继承、禁止子类复写、禁止重新赋值（定义常量）等。
# 十一、内部类

在Java程序中，通常情况下，我们把不同的类组织在不同的包下面，对于一个包下面的类来说，它们是在同一层次，没有父子关系。有些情况下，我们也会将一个类放在另一个类的内部进行定义，这个类就称为**内部类**（Inner Class）。

## 1.内部类的声明与实例化

内部类无法单独存在，必须依附于一个Outer Class（外部类），类似于这个外部类的一个属性。也就是说，实例化内部类之前必须先实例化内部类：

```java
class Person {  
    String name;  
  
    Person(String name) {  
        this.name = name;  
    }  
  
    class Student {  
        void showName() {  
            System.out.println("学生的姓名为：" + name);  
        }  
    }  
  
}  
public class Demo {  
    public static void main(String[] args) {  
  
        Person p = new Person("张三");  
        // 注意看内部类是如何初始化的：
        Person.Student s = p.new Student();  
  
        s.showName();  
  
    }  
}

// 输出结果： 学生的姓名为：张三
```

注意内部类的实例化写法是：`外部类.内部类 内部对象名 = 外部对象.new 内部类构造方法();`。内部类作为外部类的一个字段，可以访问外部类的 `private` 属性和方法。

## 2.匿名类

在类的方法内部，定义一个**匿名类**（Anonymous Class），也会定义一个内部类。

```java
import java.util.HashMap;

public class Demo {
	public static void main(String[] args) {
		HashMap<String, String> map1 = new HashMap<>();
		HashMap<String, String> map2 = new HashMap<>() {}; // 这里是匿名类
		HashMap<String, String> map3 = new HashMap<>() {
			{
				put("A", "1");
				put("B", "2");
			}
		};
		System.out.println(map3.get("A"));
	}
}

```

`map1` 是一个普通的 `HashMap` 实例，但 `map2` 是一个匿名类实例，只是该匿名类继承自 `HashMap`。`map3` 也是一个继承自 `HashMap` 的匿名类实例，并且添加了 `static` 代码块来初始化数据。观察编译输出可发现 `Main$1.class` 和 `Main$2.class` 两个匿名类文件。

## 3.静态内部类

使用 `static` 定义的内部类称为静态内部类（Static Nested Class）。用 `static` 修饰的内部类和普通内部类有很大的不同，它不再依附于外部类的实例对象，而是一个完全独立的类，因此无法引用 `Outer.this` ，但它可以访问外部类的 `private` 静态字段和静态方法。如果把静态内部类移到外部类之外，就失去了访问 `private` 的权限。

# 十二、classpath和jar

## 1.Classpath

`classpath` 是JVM用到的一个环境变量，它用来指示JVM如何搜索定义的类。现代使用的IDE如Eclipse、Intellij IDEA会自动配置这个变量，这里就不深入学习了，简单概括一下廖老师的文章；大家感兴趣的话可以研究一下，[点击访问原文地址](https://liaoxuefeng.com/books/java/oop/basic/classpath-jar/)：

### ① classpath的作用

- classpath是JVM用于搜索**编译后的.class文件**的**环境变量**（一组目录集合）。
- JVM根据 `classpath` 中的路径来查找需要加载的类（例如`abc.xyz.Hello`对应`abc/xyz/Hello.class`）。
- 搜索顺序是**从左到右**，一旦找到就停止搜索；如果所有路径都未找到，则报错。

### ② classpath的格式

- **Windows**：用分号`;`分隔，含空格的路径需用双引号括起（示例：`.;C:\work\bin;"D:\My Documents\bin"`）。
- **Linux/Mac**：用冒号`:`分隔（示例：`.:/usr/shared:/home/user/bin`）。

### ③ 设置classpath的两种方式

- **不推荐**：在系统环境变量中设置classpath（会污染系统环境）。
- **推荐**：启动JVM时通过`-classpath`（或`-cp`）参数指定（仅对当前进程有效）。  

```bash
java -cp .;C:\work\bin;C:\shared abc.xyz.Hello
```

- **默认行为**：如果不设置任何classpath，JVM默认使用当前目录（`.`）作为classpath。

### ④ 重要注意事项

- **无需添加Java核心库**（如`rt.jar`）：JVM会自动加载核心库（例如`String`、`ArrayList`等），手动添加反而可能导致问题。
- **IDE的处理**：IDE（如Eclipse、IntelliJ IDEA）会自动设置classpath（通常包括项目的`bin`目录和依赖的jar包）。
- **目录结构必须匹配包名**：  
    例如，类`com.example.Hello`必须位于`com/example/Hello.class`。  
    如果当前目录是`C:\work`，则完整路径应为`C:\work\com\example\Hello.class`，并使用命令：

```bash
java -cp . com.example.Hello
``` 

### ⑤ 实操建议

- **避免设置系统级classpath**，始终通过`-cp`参数传递。
- **默认使用当前目录（`.`）** 通常足够满足大部分场景。
- **确保目录结构与包名一致**，否则JVM无法找到类。

## 2.jar包

此部分忽略，实际项目中通常使用比较成熟的构建工具（如maven）来打包。

# 十三、class版本

通常我们提到的Java 8，Java11，Java 21；指的是Java的**JDK版本**

在cmd中执行命令 `java -version`，返回的是JDK的版本,也是JVM的版本，即 `Java.exe` 的版本。

![](20250823165958211.png#bc)

每个版本的JVM执行的class文件（字节码文件）版本也不同。例如，Java 11对应的class文件版本是55，而Java 17对应的class文件版本是61。Java是向下兼容的，即使用旧版本JDK编写的程序和字节码能在高版本的JVM上执行，而高版本的Java通常定义了新的方法和语句，新版本JDK编写的程序和字节码可能难以在旧版本的JVM上运行。

# 十四、模块

为了实现Java的模块化，从Java 9开始，原有的Java标准库已经由一个单一巨大的 `rt.jar` 分拆成了几十个模块，这些模块以 `.jmod` 扩展名标识，可以在 `$JAVA_HOME/jmods` 目录下找到它们：

- java.base.jmod
- java.compiler.jmod
- java.datatransfer.jmod
- java.desktop.jmod
- ...

把一堆class封装为jar仅仅是一个打包的过程，而把一堆class封装为模块则不但需要打包，还需要写 入依赖关系，并且还可以包含二进制代码（通常是JNI扩展）。此外，模块支持多版本，即在同一个 模块中可以为不同的JVM提供不同的版本。

这里**模块的编写、运行以及JRE打包**暂时忽略，以后需要使用时再来学习。


<center><h1>第二节 Java核心类</h1></center>

# 一、字符串和编码

## 1.String类

在Java中， `String` 是一个引用类型，它本身也是一个类，可以由构造方法定义一个字符串实例。但是Java编译器对 `String` 有特殊处理，可以直接用双引号 `""` 来定义一个字符串。

```Java
String s1 = "Hello";
String s2 = new String(new char[] {'H', 'e', 'l', 'l', 'o', '!'});
```

## 2.String类的常用方法

String类型已在前文探讨过，此处只补充一些实用的方法。

### ① trim():去除首尾的空白字符

```java
" \tHello\r\n ".trim(); // 得到"Hello"
```

这里的空白字符不仅包含空格，还包含`\t`、`\r`以及`\n`等转义字符。

另外 `strip()` 方法也用于去除空白符，但也会移除中文空格"\u3000"等字符。

### ② 字符串与char\[]的转换

```java
char[] cs = "Hello".toCharArray(); // String -> char[] 
String s = new String(cs); // char[] -> String
```

这里需要注意的是，使用 `toCharArray()` 将字符串转换成数组后对数组进行处理，原来的字符串不会变化。这样做其实也好理解，字符串作为不可变的类型，没有实现变化内容的方法。这里的转换更多是复制拷贝，而不是将两个变量关联起来。

StringBuilder、StringJoiner这两个对象已在前文提过，这里不再赘述。

# 四、包装类型

Java中的数据分为两种：

- 基本类型：`byte`、`byte`，`short`，`int`，`long`，`boolean`，`float`，`double`，`char`；
- 引用类型：基于类和接口的数据类型，如 `String`。

引用类型可以赋值为`null`，表示空，但基本类型不能赋值为`null`

想要实现以引用类型的方式来处理基本类型，我们可以将其包装成类。如定义 `Int` 类，让它只包含一个字段 `private int value = 0;` 。这样一来，`Int` 类就可以视为 `int` 的包装类。实际上无需我们来进行包装，Java的核心库已经定义好了这些基本类型对顶的包装类型：

|基本类型|对应的引用类型|
|---|---|
|boolean|java.lang.Boolean|
|byte|java.lang.Byte|
|short|java.lang.Short|
|int|java.lang.Integer|
|long|java.lang.Long|
|float|java.lang.Float|
|double|java.lang.Double|
|char|java.lang.Character|

这些包装对象都是不可变的，对他们进行比较也不能使用 `a == b`，应该使用 `a.equals(b)`。 

## 静态工厂方法

`Integer` 类有一个方法 `ValueOf()`，会将输入转换成一个 `Integer` 对象。使用此方法也可以同来创建 `Integer` 类的对象：

```java
Integer n = new Integer(100);
Integer n = Integer.valueOf(100);
```

这两种写法几乎等效，但是下面的更好。当我们使用 `Integer.valueOf(int i)` 时，如果传入的 `i` 在 **-128 到 127** 之间，方法会直接从内部的缓存数组中返回一个已经创建好的、相同的 `Integer` 对象。如果传入的值超出了这个范围，则会 `new` 一个新的 `Integer` 对象。

使用这样的方法有利于提升性能和节省内存，是一种比 `new` 更好的处理方式。

# 五、JavaBean

很多情况下，我们将类的属性设置为`private`，并且使用 `public` 方法来暴露读取或修改属性的“接口”，如 `getXxx()` 和 `setXxx()`。

## 1.JavaBean概述

如果一个类满足这些特点，我们可以说它是一个 `JavaBean`（看到这个词我的第一印象是咖啡豆？）：

- 拥有无参的公共构造函数
- 所有的属性均为 `private`
- 提供了 `public` 的属性读写方法，并且命名成 `getXxx()` 及 `SetXxx()` 的形式

对于 `boolean` 类型的字段，读写方法应该是这种格式：

```java
// 读方法：
public boolean isDone() {...}
// 写方法：
public void setDone() {...}
```

我们通常把一组对应的读方法（`getter`）和写方法（`setter`）称为属性（`property`）。只有`getter`的属性称为只读属性（read-only），只有`setter`的属性称为只写属性（write-only）。

只读属性比较常见，而只写属性就相对很少使用了。

这里之所以把这两个读写方法称之为属性而不是方法，是因为只需要定义 `getter` 和 `setter`，即可，不一定要有对应的字段，如：

```java
public class Person {
    private String name;
    private int age;

    public String getName() { return this.name; }
    public void setName(String name) { this.name = name; }

    public int getAge() { return this.age; }
    public void setAge(int age) { this.age = age; }

    public boolean isAdult() {
        return age >= 18;
    }
}
```

这里可以直接通过 `isAdult()`，根据类中的 `age` 字段判断是否为成年人，而无需再添加一个 `boolean` 类型的字段，这样看来 `isAdult()` 更像是标记了类的一种**属性**。正是由于对应的字段可能是虚拟的（或者说是间接的），这样的读写（主要是读取）更像是在操作类的一种属性，所以这里更倾向于认为它是一种属性而不是方法。

## 2.JavaBean的作用

JavaBean主要用来传递数据，即把一组数据组合成一个JavaBean便于传输。此外，JavaBean可以方便地被IDE工具分析，生成读写属性的代码，主要用在图形界面的可视化设计中。在IDE中也可以快速生成 `getter` 和 `setter`。

# 六、枚举类

枚举的基础知识已经在前文介绍过了，这里结合面向对象进行一些拓展：

## 1.枚举也是一种类型

通过 `enum` 定义的枚举也是一个 `class`，并且与其他的类没有很大的差异，主要具有以下几个特点：

- 定义的 `enum` 类型总是继承自 `java.lang.Enum`，且无法被继承；
- 只能定义出 `enum` 的实例，而无法通过 `new` 操作符创建`enum` 的实例；
- 定义的每个实例都是引用类型的唯一实例；
- 可以将 `enum` 类型用于 `switch` 语句。

```java
// 我们定义的enum:
public enum Color {
    RED, GREEN, BLUE;
}

// 编译器编译出的class:
public final class Color extends Enum { // 继承自Enum，标记为final class
    // 每个实例均为全局唯一:
    public static final Color RED = new Color();
    public static final Color GREEN = new Color();
    public static final Color BLUE = new Color();
    // private构造方法，确保外部无法调用new操作符:
    private Color() {}
}
```

我们可以使用这些方法对枚举进行操作，或者获取枚举的信息：

- name():返回枚举项的名称
- ordinal():返回枚举项的编号
- toString():返回枚举项的名称，但是可以进行覆写（不建议用于判断枚举项名称，可以覆写后用于优化输出格式）

```java
enum Color {  
    RED("红色"), BLUE("蓝色"), GREEN("绿色");  
  
    private final String color_name;  
  
    private Color(String color_name) {  
        this.color_name = color_name;  
    }  
  
    @Override  // 重写 toString 方法
    public String toString() {  
        return color_name;  
    }  
}  
  
  
public class Demo {  
    public static void main(String[] args) {  
  
        System.out.println(Color.RED.ordinal());  
        System.out.println(Color.BLUE.name());  
        System.out.println(Color.GREEN.name() + "是" + Color.GREEN.toString());  
    }  
}

/* 输出： 
0
BLUE
GREEN是绿色
*/
```

# 七、记录类

使用`String`、`Integer`等类型的时候，这些类型都是不变类，一个不变类具有以下特点：

1. 定义class时使用`final`，无法派生子类；
2. 每个字段使用`final`，保证创建实例后无法修改任何字段。

## 1.record类

Java 14之后，我们可以使用 `record` 类定义一个记录类：

```java
// 定义记录类：
record Point(int x, int y) {}

// 相当于这样写：
final class Point extends Record {
    private final int x;
    private final int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int x() {
        return this.x;
    }

    public int y() {
        return this.y;
    }

    public String toString() {
        return String.format("Point[x=%s, y=%s]", x, y);
    }

    public boolean equals(Object o) {
        ...
    }
    public int hashCode() {
        ...
    }
}
```

可以看到，除了用 `final` 修饰class以及每个字段外，编译器还自动为我们创建了构造方法，和字段名同名的方法，以及覆写 `toString()`、`equals()` 和 `hashCode()` 方法（良心大大的好啊）。

使用`record`关键字，可以一行写出一个不变类。和`enum`类似，我们自己不能直接从`Record`派生，只能通过`record`关键字由编译器实现继承。

## 2.记录类的构造

我们也可以手动修改构造类，比如说加上发现参数非法就抛出异常的处理：

```java
record Point(int x, int y) {  
    Point {  
        if (x < 0 || y < 0) {  
            throw new IllegalArgumentException("坐标参数不能为负");  
        }  
    }  
}  
  
public class Demo {  
    public static void main(String[] args) {  
        Point p = new Point(-1, -1);  
    }  
}

// 输出：Exception in thread "main" java.lang.IllegalArgumentException: 坐标参数不能为负
```

## 3.添加静态方法

通常我们定义一个 `of()` 静态方法，用于创建类对应的实例：

```java
public record Point(int x, int y) {
    public static Point of() {
        return new Point(0, 0);
    }
    public static Point of(int x, int y) {
        return new Point(x, y);
    }
}
```

这样我们可以通过 `var p = Point.of(1,2);` 来实例化一个类的对象，大家看出来这属于前文提到的静态工厂方法了吗？这样做有利于节省内存和提升性能，是一种推荐的做法。

# 八、BigInteger

如果我们在开发数据量很大的项目（人口统计系统、银行储蓄管理系统等）时，或许会超过 `long` 类型的表示范围。

在Java中，可以通过 `Java.math.BigInteger` 表示任意大小的整数。其内部通过 `int[]` 数组来模拟一个很大很大的整数。

```java
BigInteger bi = new BigInteger("1234567890");
System.out.println(bi.pow(5)); // 2867971860299718107233761438093672048294900000
```

## 1.BigInteger的计算

对`BigInteger`做运算的时候，只能使用实例方法。下表列出了 `BigInteger` 的常用运算方法；无需记忆，用时查表即可。

| 类别       | 方法签名                                                   | 描述                                                            |
| -------- | ------------------------------------------------------ | ------------------------------------------------------------- |
| **算术运算** | `BigInteger add(BigInteger val)`                       | 返回 `this + val` 的和                                            |
|          | `BigInteger subtract(BigInteger val)`                  | 返回 `this - val` 的差                                            |
|          | `BigInteger multiply(BigInteger val)`                  | 返回 `this * val` 的积                                            |
|          | `BigInteger divide(BigInteger val)`                    | 返回 `this / val` 的商（整数除法）                                      |
|          | `BigInteger[] divideAndRemainder(BigInteger val)`      | 返回一个数组，包含 `[商, 余数]`                                           |
|          | `BigInteger remainder(BigInteger val)`                 | 返回 `this % val` 的余数                                           |
| **模运算**  | `BigInteger mod(BigInteger m)`                         | 返回 `this mod m`（模数必须为正数）                                      |
|          | `BigInteger modPow(BigInteger exponent, BigInteger m)` | 返回 `(this^exponent) mod m`                                    |
|          | `BigInteger modInverse(BigInteger m)`                  | 返回 `this^(-1) mod m`（乘法逆元）                                    |
| **位运算**  | `BigInteger and(BigInteger val)`                       | 返回 `this & val`（按位与）                                          |
|          | `BigInteger or(BigInteger val)`                        | 返回 `this \| val`（按位或）                                         |
|          | `BigInteger xor(BigInteger val)`                       | 返回 `this ^ val`（按位异或）                                         |
|          | `BigInteger not()`                                     | 返回 `~this`（按位取反）                                              |
|          | `BigInteger shiftLeft(int n)`                          | 返回 `this << n`（左移n位）                                          |
|          | `BigInteger shiftRight(int n)`                         | 返回 `this >> n`（算术右移n位）                                        |
| **比较运算** | `int compareTo(BigInteger val)`                        | 比较大小。返回负数、零或正数，分别表示 `this < val`, `this == val`, `this > val` |
|          | `boolean equals(Object x)`                             | 判断值是否相等（与`compareTo`一致，不同于`==`）                               |
| **其他运算** | `BigInteger abs()`                                     | 返回绝对值                                                         |
|          | `BigInteger negate()`                                  | 返回相反数 (`-this`)                                               |
|          | `BigInteger pow(int exponent)`                         | 返回 `this^exponent`（指数）                                        |
|          | `BigInteger gcd(BigInteger val)`                       | 返回 `this` 和 `val` 的最大公约数 (GCD)                                |
|          | `BigInteger sqrt()`                                    | 返回 `this` 的整数平方根                                              |
|          | `BigInteger nextProbablePrime()`                       | 返回第一个大于 `this` 的素数（概率性）                                       |
|          | `boolean isProbablePrime(int certainty)`               | 判断此 BigInteger 是否为素数（概率性测试）                                   |
### 重要说明：

1. **不可变性 (Immutability)**：`BigInteger` 和 `BigDecimal` 对象是**不可变的**。所有上述方法执行运算后都会**返回一个全新的对象**，原来的对象值不会被修改。
2. **静态常量**：`BigInteger` 类提供了常用的静态常量，方便使用：

    - `BigInteger.ZERO`：表示数字0；
    - `BigInteger.ONE`：表示数字1；
    - `BigInteger.TWO`：表示数字2；
    - `BigInteger.TEN`：表示数字10。        

3. **性能考量**：由于不可变性和任意精度，`BigInteger` 的运算开销远大于基本数据类型（如 `int`, `long`）。应在确实需要处理大整数时才使用它。
4. **素数测试**：`nextProbablePrime()` 和 `isProbablePrime()` 方法中使用的是概率性测试（米勒-拉宾算法）。参数 `certainty` 表示对确定度的衡量，值越大，结果是素数的概率越高，但计算时间也更长。

## 2.类型转换

```java
BigInteger i = new BigInteger("123456789000");
System.out.println(i.longValue()); // 123456789000
System.out.println(i.multiply(i).longValueExact()); // java.lang.ArithmeticException: BigInteger out of long range
```

使用`longValueExact()`方法时，如果超出了`long`型的范围，会抛出`ArithmeticException`。

`BigInteger`和`Integer`、`Long`一样，也是不可变类，并且也继承自`Number`类。因为`Number`定义了转换为基本类型的几个方法：

- 转换为`byte`：`byteValue()`
- 转换为`short`：`shortValue()`
- 转换为`int`：`intValue()`
- 转换为`long`：`longValue()`
- 转换为`float`：`floatValue()`
- 转换为`double`：`doubleValue()`

如果`BigInteger`表示的范围超过了基本类型的范围，转换时将丢失高位信息，即结果不一定是准确的。如果需要准确地转换成基本类型，可以使用`intValueExact()`、`longValueExact()`等方法，在转换时如果超出范围，将直接抛出`ArithmeticException`异常。

如果`BigInteger`的值甚至超过了`float`的最大范围（3.4x1038），会返回无限值 `infinity`

# 九、BigDecimal

和`BigInteger`类似，`BigDecimal`可以表示一个任意大小且精度完全准确的浮点数。

``` java
BigDecimal bd = new BigDecimal("123.4567");
System.out.println(bd.multiply(bd)); // 15241.55677489
```

可以使用 `scale()` 方法获取小数的位数。如果一个`BigDecimal`的`scale()`返回负数，例如，`-2`，表示这个数是个整数，并且末尾有2个0。

对`BigDecimal`做加、减、乘时，精度不会丢失，但是做除法时，存在无法除尽的情况，这时，就必须指定精度以及如何进行截断：

```java
BigDecimal d1 = new BigDecimal("123.456");
BigDecimal d2 = new BigDecimal("23.456789");
BigDecimal d3 = d1.divide(d2, 10, RoundingMode.HALF_UP); // 保留10位小数并四舍五入
BigDecimal d4 = d1.divide(d2); // 报错：ArithmeticException，因为除不尽
```

还可以对`BigDecimal`做除法的同时求余数：

```java
import java.math.BigDecimal;

public class Demo {
    public static void main(String[] args) {
        BigDecimal n = new BigDecimal("12.345");
        BigDecimal m = new BigDecimal("0.12");
        BigDecimal[] dr = n.divideAndRemainder(m);
        System.out.println(dr[0]); // 102
        System.out.println(dr[1]); // 0.105
    }
}
```

调用`divideAndRemainder()`方法时，返回的数组包含两个`BigDecimal`，分别是商和余数，其中商总是整数，余数不会大于除数。

需要注意的是，在比较两个`BigDecimal`的值是否相等时，要特别注意，使用`equals()`方法不但要求两个`BigDecimal`的值相等，还要求它们的`scale()`，即小数点后的位数相等。

# 十、常用工具类

## 1.Math类

多用于数学计算，提供了很多静态方法：

求绝对值：

```java
Math.abs(-100); // 100
Math.abs(-7.8); // 7.8
```

取最大或最小值：

```java
Math.max(100, 99); // 100
Math.min(1.2, 2.3); // 1.2
```

计算 $x^{y}$：

```java
Math.pow(2, 10); // 2的10次方=1024
```

计算 $\sqrt x$​：

```java
Math.sqrt(2); // 1.414...
```

计算 $e^{x}$：

```java
Math.exp(2); // 7.389...
```

计算 $ln(x)$（底为e的对数）：

```java
Math.log(4); // 1.386...
```

计算 $lg(x)$（底为10的对数）：

```java
Math.log10(100); // 2
```

三角函数：

```java
Math.sin(3.14); // 0.00159...
Math.cos(3.14); // -0.9999...
Math.tan(3.14); // -0.0015...
Math.asin(1.0); // 1.57079...
Math.acos(1.0); // 0.0
```

Math还提供了几个数学常量：

```java
double pi = Math.PI; // 3.14159...
double e = Math.E; // 2.7182818...
Math.sin(Math.PI / 6); // sin(π/6) = 0.5
```

生成一个随机数x，x的范围是`0 <= x < 1`：

```java
Math.random(); // 0.53907... 每次都不一样
```

如果我们要生成一个区间在`[MIN, MAX)`的随机数，可以借助`Math.random()`实现，计算如下：

```java
// 区间在[MIN, MAX)的随机数
public class Main {
    public static void main(String[] args) {
        double x = Math.random(); // x的范围是[0,1)
        double min = 10;
        double max = 50;
        double y = x * (max - min) + min; // y的范围是[10,50)
        long n = (long) y; // n的范围是[10,50)的整数
        System.out.println(y);
        System.out.println(n);
    }
}
```

## 2.Random类

`Random`用来创建伪随机数。所谓伪随机数，是指只要给定一个初始的种子，产生的随机数序列是完全一样的。

要生成一个随机数，可以使用`nextInt()`、`nextLong()`、`nextFloat()`、`nextDouble()`：

```java
Random r = new Random();
r.nextInt(); // 2071575453,每次都不一样
r.nextInt(10); // 5,生成一个[0,10)之间的int
r.nextLong(); // 8811649292570369305,每次都不一样
r.nextFloat(); // 0.54335...生成一个[0,1)之间的float
r.nextDouble(); // 0.3716...生成一个[0,1)之间的double
```

有同学问，每次运行程序，生成的随机数都是不同的，没看出**伪随机数**的特性来。

这是因为我们创建`Random`实例时，如果不给定种子，就使用系统当前时间戳作为种子，因此每次运行时，种子不同，得到的伪随机数序列就不同。

如果我们在创建`Random`实例时指定一个种子，就会得到完全确定的随机数序列：

```java
import java.util.Random;

public class Demo {
    public static void main(String[] args) {
        Random r = new Random(12345);
        for (int i = 0; i < 10; i++) {
            System.out.println(r.nextInt(100));
        }
        // 51, 80, 41, 28, 55...
    }
}
```

前面我们使用的`Math.random()`实际上内部调用了`Random`类，所以它也是伪随机数，只是我们无法指定种子。

## 3.SecureRandom

真正的真随机数只能通过量子力学原理来获取，我们想要获取一个不可预测的安全的随机数时，可以使用`SecureRandom`这个类。

```java
SecureRandom sr = new SecureRandom();
System.out.println(sr.nextInt(100));
```

`SecureRandom`无法指定种子，它使用RNG（random number generator）算法。JDK的`SecureRandom`实际上有多种不同的底层实现，有的使用安全随机种子加上伪随机数算法来产生安全的随机数，有的使用真正的随机数生成器。实际使用的时候，可以优先获取高强度的安全随机数生成器，如果没有提供，再使用普通等级的安全随机数生成器：

```java
import java.util.Arrays;
import java.security.SecureRandom;
import java.security.NoSuchAlgorithmException;

public class Demo {
    public static void main(String[] args) {
        SecureRandom sr = null;
        try {
            sr = SecureRandom.getInstanceStrong(); // 获取高强度安全随机数生成器
        } catch (NoSuchAlgorithmException e) {
            sr = new SecureRandom(); // 获取普通的安全随机数生成器
        }
        byte[] buffer = new byte[16];
        sr.nextBytes(buffer); // 用安全随机数填充buffer
        System.out.println(Arrays.toString(buffer));
    }
}
```

`SecureRandom`的安全性是通过操作系统提供的安全的随机种子来生成随机数。这个种子是通过CPU的热噪声、读写磁盘的字节、网络流量等各种随机事件产生的“熵”。

在密码学中，安全的随机数非常重要。如果使用不安全的伪随机数，所有加密体系都将被攻破。因此，时刻牢记必须使用`SecureRandom`来产生安全的随机数。

--- 
# 参考资料

[^1]: 廖雪峰的官方网站.Java教程\[EB/OL].(2025-06-07)\[2025-08-21]. https://www.cnblogs.com/echolun/p/12709761.html