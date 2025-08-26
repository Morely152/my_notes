---

---
--- 
> 声明：本篇笔记部分摘自[《Java核心技术（卷Ⅰ） - 机械工业出版社》](https://detail.tmall.com/item.htm?ali_refid=a3_420434_1006%3A1151895243%3AN%3AoB1xLXSDdjSpCunkFwpZbCtvD%2B6YEaA9%3A39f8fcdda956d1ec63523e9a6e9e2355&id=708821240842&mi_id=0000mg2-P7Ustbzeym2_6DxuUMLCpndkVCAGc5EaA_l8QQ0&mm_sceneid=1_0_128421313_0&priceTId=2147831a17554253371677975e1dca&spm=a21n57.1.hoverItem.2&utparam=%7B%22aplus_abtest%22%3A%226b956865e0df43cd4a6620880d877f11%22%7D&xxc=ad_ztc)及[Java教程-廖雪峰-2025-06-16](https://liaoxuefeng.com/books/java/introduction/index.html)，遵循[CC BY 4.0协议](https://creativecommons.org/licenses/by/4.0/legalcode.zh-hans)。
> 存在由AI生成的小部分内容，仅供参考，请仔细甄别可能存在的错误。

# 一、反射

## 1.引入

考虑这样一个情况，用户成功登录后，后端需要返回用户的昵称、头像、个性签名等数据；常用的方式是将用户对象 `User` 中的信息的序列化为JSON文件进行传输。

我们可以在这个类中定义一个 `ToJson()` 方法，将类的一些字段打包整理成规范的格式；但是这样做有很多缺点：

- 如果有很多个类，每个类都需要写一套这样的格式化方法，工作量很大而且重复度很高；
- 如果类中的字段有改动，方法也需要跟着修改，否则会出错误；
- 格式化方法与每一个类深度绑定（相当于是“写死”的），无法预知和处理将来出现的新类。

再来考虑这样一个情况：统计字符串中各个字符出现的次数，我们只需读取整个字符串中的内容，逐个统计其中的每个字符。

回到之前的场景，能否也像这样接收一个类的信息，动态地分析每一个字段并且将他们拼接成JSON字符串呢？类都是定义好的结构和内容，而在程序运行时“查看”类的结构，就是在查看程序本身的一部分结构了。

**Java中，在程序运行时“反向”地查看和操作它自身的结构和行为，就是“反射”。**

## 2.Class类

### ① 什么是Class类

除了`int`等基本类型外，Java的其他类型全部都是`class`（包括`interface`）。例如：

- `String`
- `Object`
- `Runnable`
- `Exception`

我们可以认为类的本质是一种数据类型。JVM在第一次读取到一个类时，将其加载进内存，同时就为其创建一个`Class`类型的实例（这个实例只能由JVM创建），并关联起来：

```java
public final class Class {
    private Class() {}
}
```

所以，JVM持有的每个`Class`实例都指向一个数据类型（或者说是一个类）。`Class`实例中包含了这个类的所有完整信息：

```
┌───────────────────────────┐
│      Class Instance       │────▶ String
├───────────────────────────┤
│name = "java.lang.String"  │
├───────────────────────────┤
│package = "java.lang"      │
├───────────────────────────┤
│super = "java.lang.Object" │
├───────────────────────────┤
│interface = CharSequence...│
├───────────────────────────┤
│field = value[],hash,...   │
├───────────────────────────┤
│method = indexOf()...      │
└───────────────────────────┘
```

由于JVM为每个加载的类都创建了对应的`Class`实例，并在实例中保存了这个类的所有信息，包括类名、包名、父类、实现的接口、所有方法、字段等。因此，如果获取了某个`Class`实例，我们就可以通过这个`Class`实例获取到该实例对应的类的所有信息。

这种通过`Class`实例获取类的信息的方法称为**反射（Reflection）。**

### ② 获取一个类的Class实例

方法一：直接通过一个`class`的静态变量`class`获取：

```java
Class cls = String.class;
```

方法二：如果我们有一个实例变量，可以通过该实例变量提供的`getClass()`方法获取：

```java
String s = "Hello";
Class cls = s.getClass();
```

方法三：如果知道一个`class`的完整类名，可以通过静态方法`Class.forName()`获取：

```java
Class cls = Class.forName("java.lang.String");
```

因为`Class`实例在JVM中是唯一的，所以，上述方法获取的`Class`实例是同一个实例。

要从`Class`实例获取类的基本信息，参考下面的代码：

```java
public class Demo {  
    public static void main(String[] args) {  
        System.out.println("----- String类型的信息 -----");  
        printClassInfo("".getClass());  
        System.out.println("----- Runnable类型的信息 -----");  
        printClassInfo(Runnable.class);  
        System.out.println("----- Month类型的信息 -----");  
        printClassInfo(java.time.Month.class);  
        System.out.println("----- String[]类型的信息 -----");  
        printClassInfo(String[].class);  
        System.out.println("----- int类型的信息 -----");  
        printClassInfo(int.class);  
    }  
  
    static void printClassInfo(Class cls) {  
        System.out.println("类名: " + cls.getName());  
        System.out.println("简称: " + cls.getSimpleName());  
        if (cls.getPackage() != null) {  
            System.out.println("包名: " + cls.getPackage().getName());  
        }  
        System.out.println("接口: " + cls.isInterface());  
        System.out.println("枚举: " + cls.isEnum());  
        System.out.println("数组: " + cls.isArray());  
        System.out.println("基础数据类型: " + cls.isPrimitive());  
    }  
}
```

运行上述程序，输出以下内容：

```
----- String类型的信息 -----
类名: java.lang.String
简称: String
包名: java.lang
接口: false
枚举: false
数组: false
基础数据类型: false
----- Runnable类型的信息 -----
类名: java.lang.Runnable
简称: Runnable
包名: java.lang
接口: true
枚举: false
数组: false
基础数据类型: false
----- Month类型的信息 -----
类名: java.time.Month
简称: Month
包名: java.time
接口: false
枚举: true
数组: false
基础数据类型: false
----- String[]类型的信息 -----
类名: [Ljava.lang.String;
简称: String[]
接口: false
枚举: false
数组: true
基础数据类型: false
----- int类型的信息 -----
类名: int
简称: int
接口: false
枚举: false
数组: false
基础数据类型: true
```

注意到数组（例如`String[]`）也是一种类，而且不同于`String.class`，它的类名是`[Ljava.lang.String;`。此外，JVM为每一种基本类型如`int`也创建了`Class`实例，通过`int.class`访问。

### ③ 动态加载

JVM在执行Java程序的时候，并不是一次性把所有用到的class全部加载到内存，而是第一次需要用到class时才加载。即程序运行时，发现需要使用哪一个类，再将其动态添加到内存。

## 2.访问字段

### ① 获取所有的字段

`Class`类提供了以下几个方法来获取字段：

| 方法                             | 返回值               |
| ------------------------------ | ----------------- |
| `Field getField(name)`         | 指定的public字段（包括父类） |
| `Field getDeclaredField(name)` | 当前类的某个指定字段（不包括父类） |
| `Field[] getFields()`          | 所有public的字段（包括父类） |
| `Field[] getDeclaredFields()`  | 当前类的所有字段（不包括父类）   |
这些方法会获取到类似于 `private String Person.name` 这样的字段信息，包含了可见性、类型、类名.字段名这些信息。

获取到字段信息后，可以使用这些方截取取字段的部分信息：

| 方法               | 返回值                  |
| ---------------- | -------------------- |
| `getName()`      | 字段名称，如"name"         |
| `getType()`      | 字段类型，也是一个`Class`实例   |
| `getModifiers()` | 字段修饰符，是一个`int`，含义见下表 |

| 修饰符          | 对应的int类型 |
| ------------ | -------- |
| public       | 1        |
| private      | 2        |
| protected    | 4        |
| static       | 8        |
| final        | 16       |
| synchronized | 32       |
| volatile     | 64       |
| transient    | 128      |
| native       | 256      |
| interface    | 512      |
| abstract     | 1024     |
| strict       | 2048     |

```java
import java.lang.reflect.Field;  
import java.lang.reflect.Modifier;  
  
public class Demo {  
  
    public static void main(String[] args) {  
        try {  
            // 获取String类的value字段（存储字符串内容的byte数组）  
            Field f = String.class.getDeclaredField("value");  
            // 获取字段名称  
            System.out.println("字段名: " + f.getName());
            // 获取字段类型  
            System.out.println("字段类型: " + f.getType()); 
  
            // 获取修饰符  
            int m = f.getModifiers();  
            // 检查各种修饰符  
            System.out.println("是否是final: " + Modifier.isFinal(m));
            System.out.println("是否是public: " + Modifier.isPublic(m));
            System.out.println("是否是protected: " + Modifier.isProtected(m));
            System.out.println("是否是private: " + Modifier.isPrivate(m));
            System.out.println("是否是static: " + Modifier.isStatic(m));
  
            // 获取所有修饰符的字符串表示  
            System.out.println("所有修饰符: " + Modifier.toString(m));  
  
        } catch (NoSuchFieldException e) {  
            System.out.println("找不到指定的字段: " + e.getMessage());  
        } catch (SecurityException e) {  
            System.out.println("安全异常: " + e.getMessage());  
        }  
    }  
}

/* 运行结果：
字段名: value
字段类型: class [B       <-- 表示byte[]类型
是否是final: true
是否是public: false
是否是protected: false
是否是private: true
是否是static: false
所有修饰符: private final
*/
```

### ② 获取字段的值

获取到字段后，我们还需要一个方法来获取这些的值：

```java
import java.lang.reflect.Field;  
import java.lang.reflect.Modifier;  
  
public class Demo {  
  
    public static void main(String[] args) throws NoSuchFieldException, IllegalAccessException {  
  
        Person person = new Person("小明");  
  
        Class c = person.getClass();  
        System.out.println(c.getDeclaredField("name").get(person));  
  
    }  
}  
  
class Person {  
    public String name;  
  
    public Person(String name) {  
        this.name = name;  
    }  
}

// 运行结果：小明
```

如果有需要的话，我们也可以添加以下语句来访问`private`字段：

```java
f.setAccessible(true);
```

反射是一种非常规的用法，反射的代码非常繁琐，其次它更多地是给工具或者底层框架来使用，目的是在不知道目标实例任何信息的情况下，获取特定字段的值。

此外，`setAccessible(true)`可能会失败。如果JVM运行期存在`SecurityManager`，那么它会根据规则进行检查，有可能阻止`setAccessible(true)`。例如，某个`SecurityManager`可能不允许对`java`和`javax`开头的`package`的类调用`setAccessible(true)`，这样可以保证JVM核心库的安全。

### ③ 设置字段的值

```java
Person person = new Person("小明");

f.setAccessible(true);
f.set(person, "张三");

System.out.println(p.getName());  // 输出“张三”。

class Person() {
	private String name;
	public Person() {...};
	public getName() {...};
}
```

## 3.调用方法

### ① 获取所有的方法

既然能获取，甚至是设置对象的字段，那么我们也可以获取`Class`的所有方法信息。以下是几个实现的方法：

| 方法                                         | 返回值               |
| ------------------------------------------ | ----------------- |
| `Method getMethod(name, Class...)`         | 指定的public方法（包括父类） |
| `Method getDeclaredMethod(name, Class...)` | 指定的方法（不包括父类）      |
| `Method[] getMethods()`                    | 所有public的方法（包括父类） |
| `Method[] getDeclaredMethods()`            | 所有Method（不包括父类）   |

每个方法对象都包含了这个方法的所有信息：

- `getName()`：返回方法名称，例如：`"getScore"`；
- `getReturnType()`：返回方法返回值类型，也是一个Class实例，例如：`String.class`；
- `getParameterTypes()`：返回方法的参数类型，是一个Class数组，例如：`{String.class, int.class}`；
- `getModifiers()`：返回方法的修饰符，它是一个`int`，不同的bit表示不同的含义。

### ② 调用获取到的方法

```java
import java.lang.reflect.InvocationTargetException;  
import java.lang.reflect.Method;  
  
public class Demo {  
  
    public static void main(String[] args) throws NoSuchFieldException, IllegalAccessException, NoSuchMethodException, InvocationTargetException {  
  
        // String对象:  
        String s = "Hello world";  
        // 获取String substring(int)方法，参数为int:  
        Method m = String.class.getMethod("substring", int.class);  
        // 在s对象上调用该方法并获取结果:  
        String r = (String) m.invoke(s, 6);  
        // 打印调用结果:  
        System.out.println(r);
    }  
}

// 运行结果：world
```

同样地，我们可以通过 `Method.setAccessible(true)` 允许调用对象中的非public方法。

## 4.调用构造方法

如果通过反射来创建新的实例，可以调用Class提供的newInstance()方法：

```java
Person p = Person.class.newInstance();
```

调用`Class.newInstance()`的局限是，它只能调用该类的public无参数构造方法。如果构造方法带有参数，或者不是public，就无法直接通过`Class.newInstance()`来调用。

为了调用任意的构造方法，Java的反射API提供了`Constructor`对象，它包含一个构造方法的所有信息，可以创建一个实例。处理`Constructor`对象和获取对象的方法很类似，不同之处仅在于它是一个构造方法，并且，调用结果总是返回实例：

```java
import java.lang.reflect.Constructor;

public class Demo {
    public static void main(String[] args) throws Exception {
        // 获取构造方法Integer(int):
        Constructor cons1 = Integer.class.getConstructor(int.class);
        // 调用构造方法:
        Integer n1 = (Integer) cons1.newInstance(123);
        System.out.println(n1);

        // 获取构造方法Integer(String)
        Constructor cons2 = Integer.class.getConstructor(String.class);
        Integer n2 = (Integer) cons2.newInstance("456");
        System.out.println(n2);
    }
}
```

通过Class实例获取`Constructor`的方法如下：

| 方法                                 | 返回值                        |
| ---------------------------------- | -------------------------- |
| `getConstructor(Class...)`         | 获取某个`public`的`Constructor` |
| `getDeclaredConstructor(Class...)` | 获取某个`Constructor`          |
| `getConstructors()`                | 获取所有`public`的`Constructor` |
| `getDeclaredConstructors()`        | 获取所有`Constructor`          |

注意`Constructor`总是当前类定义的构造方法，和父类无关，因此不存在多态的问题。

调用非`public`的`Constructor`时，必须首先通过`setAccessible(true)`设置允许访问。



## 5.获取继承关系

### ① 获取父类的Class

获取到`Class`实例后，我们还可以调用`Class`实例的方法`getSuperclass()`获取父类的`Class`：

```java
public class Demo {
    public static void main(String[] args) throws Exception {
        Class cls = Integer.class;
        
        Class sup_cls = cls.getSuperclass();          // 第一级父类
        System.out.println(sup_cls);
        
        Class sup2_cls = sup_cls.getSuperclass();     // 第二级父类
        System.out.println(sup2_cls);
        
        System.out.println(sup2_cls.getSuperclass()); // 第三集父类
    }
}
```

运行上述代码，可以看到，`Integer`的父类类型是`Number`，`Number`的父类是`Object`，`Object`的父类是`null`。除`Object`外，其他任何非`interface`的`Class`都必定存在一个父类类型。

### ② 获取接口

由于一个类可能实现一个或多个接口，通过`Class`的`getInterfaces()`方法，我们就可以查询到实现的接口类型。例如，查询`Integer`实现的接口：

```java
import java.lang.reflect.Method;

public class Demo {
    public static void main(String[] args) throws Exception {
        Class s = Integer.class;
        Class[] is = s.getInterfaces();
        for (Class i : is) {
            System.out.println(i);
        }
    }
}
```

运行上述代码可知，`Integer`实现的接口有：

- java.lang.Comparable
- java.lang.constant.Constable
- java.lang.constant.ConstantDesc

要特别注意：`getInterfaces()`只返回当前类直接实现的接口类型，并不包括其父类实现的接口类型。如果一个类没有实现任何`interface`，那么`getInterfaces()`返回空数组。

### ③ 继承关系

像在普通对象中调用`instanceof()`查看对象能否向上转型，我们对`Class`实例使用`isAssignableForm()`来查看它们之间的继承关系：

```java
Object n = Integer.valueOf(123);
boolean isDouble = n instanceof Double; // false
```

## 6.动态代理

Java的`class`和`interface`有这样的区别：

- 可以实例化非抽象的`class`；
- 不能实例化`interface`。

所有`interface`类型的变量总是通过某个实例向上转型并赋值给接口类型变量的：

```java
CharSequence cs = new StringBuilder();
```

我们也可以不编写类，直接在运行期间创建某个`interface`的实例，这就需要使用**动态代理**（dynamic proxy）的机制。

这一部分先省略吧，毕竟我前面的都不是很懂…实际上反射在应用开发中使用得不多（常用于底层组件开发），我认为应该先掌握更加基础的知识再逐渐深入。

# 二、注解

注解是放在Java源码的类、方法、字段、参数前的一种特殊“注释”：

```java
@Resource("hello")
public class Hello {
    @Inject
    int n;

    @PostConstruct
    public void hello(@Param String name) {
        System.out.println(name);
    }

    @Override
    public String toString() {
        return "Hello";
    }
}
```

与注释不同的是，注解可以被编译器打包进入`.class`文件而不是直接被忽略，因此注解是一种用作标注的“元数据”。

## 1.注解的作用

注解本身对代码逻辑没有任何影响，如何使用注解完全由工具决定。Java注解有以下三种类型：

1. 由编译器使用的注解，它们不会被编译进入`.class`文件，它们在编译后就被编译器扔掉了。，如：
	- `@Override`：让编译器检查该方法是否正确地实现了覆写；
	- `@SuppressWarnings`：告诉编译器忽略此处代码产生的警告。
2. 






--- 
# 参考资料

[^1]: 廖雪峰的官方网站.Java教程\[EB/OL].(2025-06-07)\[2025-08-21]. https://www.cnblogs.com/echolun/p/12709761.html