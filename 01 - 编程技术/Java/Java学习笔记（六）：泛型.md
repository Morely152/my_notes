---

---
--- 
> 声明：本篇笔记部分摘自[《Java核心技术（卷Ⅰ） - 机械工业出版社》](https://detail.tmall.com/item.htm?ali_refid=a3_420434_1006%3A1151895243%3AN%3AoB1xLXSDdjSpCunkFwpZbCtvD%2B6YEaA9%3A39f8fcdda956d1ec63523e9a6e9e2355&id=708821240842&mi_id=0000mg2-P7Ustbzeym2_6DxuUMLCpndkVCAGc5EaA_l8QQ0&mm_sceneid=1_0_128421313_0&priceTId=2147831a17554253371677975e1dca&spm=a21n57.1.hoverItem.2&utparam=%7B%22aplus_abtest%22%3A%226b956865e0df43cd4a6620880d877f11%22%7D&xxc=ad_ztc)及[Java教程-廖雪峰-2025-06-16](https://liaoxuefeng.com/books/java/introduction/index.html)，遵循[CC BY 4.0协议](https://creativecommons.org/licenses/by/4.0/legalcode.zh-hans)。
> 存在由AI生成的小部分内容，仅供参考，请仔细甄别可能存在的错误。

# 一、引入

考虑这样一个需求：实现一个方法，原封不动地返回输入的参数，实现类似于`echo`的效果。

有同学说，这还不简单：

```java
public class Demo {  
    public static void main(String[] args) throws Exception {  
        System.out.println(  
			echo("Hello World!")  
        );  
    }  
  
    private static String echo(String s) {  
        return s;  
    }  
  
}

// 运行结果：Hello World!
```

这么看似乎没得问题，JVM也运行出了正确的结果，但是我们似乎忽略了一些问题：

```java
System.out.printf(  
	"%s + %s = %s", echo("1"), echo("1"), echo(1+1)  
);  
```

预期会得到`1 + 1 = 2`的输出，但理想很美满，现实却是输出了`java: 不兼容的类型: int无法转换为java.lang.String`。原因在与我们定义方法时固定了参数和返回值的类型均为`String`，而上例中`echo(1+1)`传入的参数类型不对，因此触发了异常。

又有同学说了，使用方法重载啊，再定义一个`int echo(int i) {}`不就行了吗？但是我们还有`double`、`float`、`boolean`这些数据类型呢，更极端地说，如果输入的是一个类的实例呢？

显然我们无法预测程序运行时，会传入哪些类型的数据。为每一种数据类型都重载一个方法显然是不理智的选择（上班摸鱼当我没说，前提是写出这样的代码不会被追着骂🤔），但是如果不写全又会找不到合适的方法来执行，会报告类型错误。那我们能否**将类型也看作是一种不定的“变量”**，随着参数一同传入方法中，然后这个方法再**根据参数的类型决定返回什么类型的数据**呢？

恭喜我们探索出了一个很有用的工具——**泛型**（Generics）：

```java
private static <Type> Type echo(Type parameter) {  
    return parameter;  
}
```

字如其名，泛型即**广泛的类型**，我们可以通过泛型来将参数的类型作为参数的一部分传入方法中，方法内根据类型来进行相应的处理，现在再调用`System.out.printf(  "%s + %s = %s", echo("1"), echo("1"), echo(1+1)  )`，就可以顺利输出`1 + 1 = 2`了。

（顺带一提，这个例子在我读一位大佬撰写的[TypeScript入门教程](https://fxzer.github.io/tsdoc-vitepress/zh/handbooks/handbook-v1/Generics)时让我非常顺利地理解了泛型的定义和作用，对我的启发很大，在我的Ts笔记中也有使用。这里也分享出来作为一个初识概念的引子，希望大家以后遇到类似的需求时能够想起泛型；毕竟重要的不是看懂而是会用。）

# 二、泛型类

上面我们已经演示了泛型方法的实现，我们来实现一下泛型类：

```java
public class Demo {  
    public static void main(String[] args) throws Exception {  
  
        CommonClass<String> t1 = new CommonClass<String>("Hello");  
        System.out.println(t1.getField());  
  
        CommonClass<Integer> t2 = new CommonClass<Integer>(123);  
        System.out.println(t2.getField());  
  
        CommonClass<Boolean> t3 = new CommonClass<Boolean>(true);  
        System.out.println(t3.getField());  
  
  
    }  
}  
  
class CommonClass<T> {  
    private T field;  
  
    public CommonClass(T field) {  
        this.field = field;  
    }  
  
    public T getField() {  
        return field;  
    }  
}

/* 运行结果：
Hello
123
true
*/
```

给`CommonClass`的构造方法传入不同类型的参数，都很好地实现了初始化与字段的读取，这就是泛型的优点：**在保持编译时类型安全的同时，获得了代码的极大复用性**。

# 三、擦拭法

Java语言的泛型实现方式是**擦拭法**（Type Erasure）。也就是说，JVM其实并不知道有泛型的存在，编译阶段会由编译器将泛型转换成实际的类型。

Java的泛型是由编译器在编译时实行的，编译器内部永远把所有类型`T`视为`Object`处理，但是，在需要转型的时候，编译器会根据`T`的类型自动为我们实行安全地强制转型。这样会使得Jabalpur中的泛型存在这些局限：

### ① 局限一：泛型不能是基本类型

`<T>`不能是基本类型，例如`int`、`double`，因为`Object`类型无法持有这些基本类型。

![](20250827032217880.png)

### ② 无法取得带泛型的`Class`。

因为`T`是`Object`，我们对`Pair<String>`和`Pair<Integer>`类型获取`Class`时，获取到的是同一个`Class`，也就是`Pair`类的`Class`。

换句话说，所有泛型实例，无论`T`的类型是什么，`getClass()`返回同一个`Class`实例，因为编译后它们全部都是`Pair<Object>`。

局限三：无法判断带泛型的类型：

```java
Pair<Integer> p = new Pair<>(123, 456);
// Compile error:
if (p instanceof Pair<String>) {
}
```

原因和前面一样，并不存在`Pair<String>.class`，而是只有唯一的`Pair.class`。

局限四：不能实例化`T`类型：

```java
public class Pair<T> {
    private T first;
    private T last;
    public Pair() {
        // Compile error:
        first = new T();
        last = new T();
    }
}
```

上述代码无法通过编译，因为构造方法的两行语句：

```java
first = new T();
last = new T();
```

擦拭后实际上变成了：

```java
first = new Object();
last = new Object();
```

这样一来，创建`new Pair<String>()`和创建`new Pair<Integer>()`就全部成了`Object`，显然编译器要阻止这种类型不对的代码。

要实例化`T`类型，我们必须借助额外的`Class<T>`参数：

```java
public class Pair<T> {
    private T first;
    private T last;
    public Pair(Class<T> clazz) {
        first = clazz.newInstance();
        last = clazz.newInstance();
    }
}
```

上述代码借助`Class<T>`参数并通过反射来实例化`T`类型，使用的时候，也必须传入`Class<T>`。例如：

```java
Pair<String> pair = new Pair<>(String.class);
```

因为传入了`Class<String>`的实例，所以我们借助`String.class`就可以实例化`String`类型。