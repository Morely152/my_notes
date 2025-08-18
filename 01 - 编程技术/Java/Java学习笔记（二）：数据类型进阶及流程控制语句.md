---

---
---
> 声明：本篇笔记部分摘自[《Java核心技术（卷Ⅰ） - 机械工业出版社》](https://detail.tmall.com/item.htm?ali_refid=a3_420434_1006%3A1151895243%3AN%3AoB1xLXSDdjSpCunkFwpZbCtvD%2B6YEaA9%3A39f8fcdda956d1ec63523e9a6e9e2355&id=708821240842&mi_id=0000mg2-P7Ustbzeym2_6DxuUMLCpndkVCAGc5EaA_l8QQ0&mm_sceneid=1_0_128421313_0&priceTId=2147831a17554253371677975e1dca&spm=a21n57.1.hoverItem.2&utparam=%7B%22aplus_abtest%22%3A%226b956865e0df43cd4a6620880d877f11%22%7D&xxc=ad_ztc)，遵循[CC BY 4.0协议](https://creativecommons.org/licenses/by/4.0/legalcode.zh-hans)。
> 存在由AI生成的小部分内容，仅供参考，请仔细甄别可能存在的错误。
---
# 一、数据类型进阶

之前我们探索了整型、浮点型、布尔型以及字符型这几种相对简单的数据类型，现在我们可以在此基础上拓展出更复杂的类型了，虽然初看起来比较复杂繁琐，但是掌握熟练后会有很大的帮助。

## 1.字符串

Java字符串就是Unicode字符序列。如字符串"Java\u2122"由“J、a、v、a、$^{TM}$”这五个字符（TM商标算作一个字符）组成。String标准类库中提供的这个预定义类被很自然地称作了String。每个用双引号括起来的字符串都是String类的一个实例。

## ① 子串

类似于Python中对列表的“切片”操作，Java也定义了对字符串截取一部分子串的方法 `substring()`：

```java
String greeting = "Hello";
String sub_s = greeting.substring(0, 3);    // 得到子串“Hel”
```

该方法接收两个参数，范围为`[a, b)`；下标从`0`开始。

## ② 拼接

Java中可以很方便地使用加号 `+` 来直接拼接两个字符串：

```java
String s1 = "Hel";
String s2 = "lo!";

System.out.println(s1 + s2);

// 输出： Hello!
```

不同于Python在试图直接拼接字符与其他类型时会报告 `TypeError`，Java在拼接字符串时会自动将它们转换成`String`类型。

如果希望用一个间隔符将很多个字符串（特别是字符串数组）连接起来，可以使用join方法：

```java
System.out.println("常用的尺码有：" + String.join("/", "S", "M", "L", "XL"));

// 输出： 常见的尺码有：S/M/L/XL
```

在Python中可以将乘号 `*` 应用于字符串实现重复输出，如`print("\o/" * 3)`会打印出`\o/\o/\o/`。



# 二、流程控制语句

## 1.Swith表达式

Switch语句在Java 14被引入。熟悉C语系的编程语言都应该很熟悉了。直接上语法：

```java
类型 变量名 = switch (表达式) {
	case 值1 -> 操作1；
	case 值2 -> 操作2；
	case 值3 -> 操作3；
	default -> 操作；
}
```

略有不同的是，Java中需要为枚举定义一个变量，且Java中不需要使用 `break;` 来标记一个分支的结束。若要为多个分支定义相同的操作，可以使用这样的语法：

```java
boolean is_weekend = switch (week) {
	case 1, 2, 3, 4, 5 -> false;
	case 6, 7 -> true;
	default -> false;
}
```

Switch语句可以很好地与枚举类型搭配使用：

```java
public class Demo {
    public static void main(String[] args) {
        enum Size {
            small, medium, large, extra_large
        };
		
        Size s = Size.small;
		
        String size_label = switch (s) {
            case small -> "S";
            case medium -> "M";
            case large -> "L";
            case extra_large -> "XL";
        };
		
        System.out.println(s + "对应的尺寸为：" + size_label);
    }
}

// 输出结果：small对应的尺寸为：S
```

如果 `switch ()` 中的“条件”（专业术语为“操作数”）是 `null`，则会触发一个 `NullPointerException`。














