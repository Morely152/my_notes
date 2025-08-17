---

---
--- 
> 声明：本篇笔记部分摘自[《Java核心技术（卷Ⅰ） - 机械工业出版社》](https://detail.tmall.com/item.htm?ali_refid=a3_420434_1006%3A1151895243%3AN%3AoB1xLXSDdjSpCunkFwpZbCtvD%2B6YEaA9%3A39f8fcdda956d1ec63523e9a6e9e2355&id=708821240842&mi_id=0000mg2-P7Ustbzeym2_6DxuUMLCpndkVCAGc5EaA_l8QQ0&mm_sceneid=1_0_128421313_0&priceTId=2147831a17554253371677975e1dca&spm=a21n57.1.hoverItem.2&utparam=%7B%22aplus_abtest%22%3A%226b956865e0df43cd4a6620880d877f11%22%7D&xxc=ad_ztc)，遵循[CC BY 4.0协议](https://creativecommons.org/licenses/by/4.0/legalcode.zh-hans)。
> 存在由AI生成的小部分内容，仅供参考，请仔细甄别可能存在的错误。

# 一、Java的发展史

## **1. 起源（1991–1995）**

- **项目名称**：最初由**James Gosling**（詹姆斯·高斯林）在Sun Microsystems（太阳微系统公司）领导开发，命名为**Oak**（橡树），目标是为嵌入式设备（如电视机顶盒）设计一种便携语言。
- **改名Java**：因商标冲突，1995年更名为**Java**（灵感来自印尼爪哇岛的咖啡，因此Logo是一杯咖啡）。
- **互联网机遇**：随着互联网兴起，Java的**跨平台特性**（通过JVM实现"一次编写，到处运行"）使其迅速成为Web开发的热门选择。

## **2. 正式发布（1995–2000）**

- **1995年**：Sun发布**Java 1.0**，强调**Applet**（在浏览器中运行的小程序），推动早期Web动态交互。
- **1996年**：JDK 1.0（Java Development Kit）发布，包含核心库和JVM。
- **企业级扩展**：1999年推出**J2EE**（Java 2 Platform Enterprise Edition），支持企业应用开发（如Servlet、EJB）。

## **3. 成熟与开源（2000–2010）**

- **版本演进**：
    - J2SE 1.4（2002）：引入**NIO**、正则表达式等。
    - Java 5（2004）：重大更新，支持**泛型**、**注解**、**枚举**等。
- **开源化**：2006年Sun将Java部分开源，2007年完成全部开源（OpenJDK）。
- **2009年**：Oracle收购Sun Microsystems，成为Java的主要维护者。

## **4. 现代Java（2010至今）**

- **版本加速**：
    - **Java 8**（2014）：里程碑版本，引入**Lambda表达式**、**Stream API**、**新的日期时间API**。
    - **Java 11**（2018）：首个长期支持（LTS）版本，Oracle调整发布周期（每半年一版，每3年一个LTS）。
    - **Java 17**（2021）：最新LTS版本，增强模式匹配、密封类等。

- **生态繁荣**：Spring框架、Android开发（早期基于Java）、大数据（Hadoop）等广泛应用。

## [Java白皮书](https://www.oracle.com/java/technologies/javase/javase-whitepapers.html)上提到的11个关键术语

- 简单性
- 面向对象
- 分布式
- 健壮性
- 安全性
- 体系结构和中立
- 可移植性
- 解释性
- 高性能
- 多线程
- 动态性

# 二、Hello World!

像学习其他的编程语言一样，从最简单的输出"Hello World！"开始：

```java
// HelloWorld.java

public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

仔细分析一下这段程序：通过访问修饰符`publlic`定义了一个公有的类`HelloWorld`，其中有一个`main`方法，通过`system.out.println()`打印了`"Hello World!"`这么一个字符串。需要注意以下几点：

- 必须存在一个公共类与源代码的文件名相同，作为程序的入口；类似于C/C++中的main函数；并且该类中必须包含一个main方法，且该方法也必须声明为`public`。
- 类的标准命名方式为**驼峰命名法**，即所有单词的首字母均大写；Java区分大小写，`main`和`Main`不同。
- **类**是Java应用的构建模块，所有的Java程序都必须放在类中。
- 语句结束的标志不是回车而是`;`，有必要的情况下可以使用回车编写一个多行的语句。
- `system.out.println()`方法会在打印之后自动换行，相比之下`system.out.print()`方法会把新的内容打印在同一行中。

作为初学者，或许会对`main`方法的参数`String[] args`感到疑惑，这里做出解释：

> 当我们运行Java程序时，可以在命令行（或终端）后面附加参数，这些参数会被传递给`main`方法，如`java HelloWorld arg1 arg2 arg3`，这里的`agr1 arg2 arg3`会作为字符串数组`String[]`传进`main`方法。Java规范要求`main`方法的签名必须如此，作为程序的统一入口点。即使我们不使用它，也需要声明，否则JVM无法识别`main`方法。

# 三、注释

与C/C++类似，Java中的注释也有这两种方式：

```java
// 单行注释

/*
	多行注释
	多行注释
	多行注释
*/
```

# 四、数据类型

Java是一种**强类型语言**，即必须为每一个变量声明一个类型。在Java中，一共有8种数据类型（4整型+2浮点型+1字符型+1布尔型）。

## 1.整型

表示没有小数的数字，可以为负。

| 类型    | 大小    | 取值范围                                                   |
| ----- | ----- | ------------------------------------------------------ |
| int   | 4 bit | -2 147 483 648 ~ 2 147 483 647                         |
| short | 2 bit | -32 768 ~ 32 767                                       |
| long  | 8 bit | -9 223 372 036 854 775 808 ~ 9 223 372 036 854 775 807 |
| byte  | 1 bit | -128 ~ 127                                             |

在Java中，整型的取值范围与运行平台无关，这使得Java有较好的可移植性；一定程度上避免了C/C++在不同的机器上可能存在的溢出问题。

长整型的数据末尾有一个L或者l，十六进制前面有0x或0x，八进制的前缀是0（如8 -> 010），这种写法容易混淆所以使用得比较少。二进制数的前缀是0b或0B。数字很大时，可以加上下划线使其更易读，如2_147_483_647。

## 2.浮点型

表示有小数部分的数值，与C/C++类似有两种类型：

| 类型     | 大小    | 取值范围                                              |
| ------ | ----- | ------------------------------------------------- |
| long   | 4 bit | 约 $\pm3.402 823 47\times10^{38}$（6~7位有效数字）        |
| double | 8 bit | 约 $\pm1.79769313486231570\times10^{308}$（15位有效数字） |

double表示的数值精度是float的两倍，因此也有“双精度浮点数”的说法。大多数情况下，都会使用double类型存储浮点数。在一个数后面加上d、D或者f、F，可以分别标识为单精度或双精度。

在浮点数中，有三个特殊数值表示溢出或错误：

- POSITIVE_INFINITY：正无穷大
- NEGATIVE_INFINITY：负无穷大
- NaN：非数字

例如，一个非零数除以零会得到无穷大的结果，而0除以0或者负数开偶次方根会得到NaN。需要注意的是，NaN不等于任何值，包括它自己，因此无法通过与NaN比较来判断结果不为NaN，而是应该使用`Double.isNaN(x)`。

```java
public class Demo {
    public static void main(String[] args) {
        // 打印3 / 0的结果
        System.out.println(3 / 0);
		// 输出Exception in thread "main" java.lang.ArithmeticException: / by zero
		
        // 运算 -2 的平方根
        System.out.println(Math.sqrt(-2));
        // 输出NaN
    }
}
```
## 3.