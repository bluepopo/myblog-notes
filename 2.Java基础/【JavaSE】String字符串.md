【JavaSE】String字符串







# （一）、String类



@[toc]

## 一、概述
String类型是引用类型，可以通过 `String s1 = "abc"; `直接赋值进行实例化，也可以通过` new` 关键字实例化，它也有自己的构造函数。

字符串：就是由多个字符组成的一串数据。也可以看成是一个字符数组。

 通过API，我们可以知道

- 字符串字面值“abc”也可以看成一个字符串对象。

- <font color=#E74C3C>字符串是常量，一旦被赋值，就不能改变。</font>

阅读String类定义的源码，你会发现：

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char value[];

    /** Cache the hash code for the string */
    private int hash; // Default to 0
    ...
}

```

1）String类被final关键字修饰，意味着String类不能被继承，并且它的成员方法都默认为final方法；字符串一旦创建就不能再修改。
2）String类实现了Serializable、CharSequence、 Comparable接口。
3）String实例的值是通过字符数组实现字符串存储的。

<font color=#95A5A6>此处借鉴原文链接：https://blog.csdn.net/ifwinds/article/details/80849184</font>


## 二、构造方法
1. 空构造：public String()
2. 字节数组转字符串:

```java
public String(byte[] bytes)（注意字节数组的格式，数值范围-128~127）
```

```java
public String(byte[] bytes, int index, int length)
```
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702100939.png)
3. 字符数组转字符串

```java
public String(char[] value)
public String(char[] value, int index, int count)
```



4. 字符串常量值转成字符串：

```java
public String(String original)
```

## 三、常用方法

### 3.1 判断方法
比较字符串内容是否相同，区分大小写：`boolean equals(Object obj)`

比较字符串的内容是否相同，忽略大小写：`boolean equalsIgnoreCase(String str)`

判断大字符串是否包含小字符串：`boolean contains(String str)`

判断字符串是都以某个指定的字符串开头：`boolean startsWith(String str)`

判断字符串是都以某个指定的字符串结尾：`boolean endsWith(String str)`

判断字符串是否为空：`boolean isEmpty()`

```java
package cn.itcast_03;

/*
 * 
 * 注意：
 * 		字符串内容为空和字符串对象为空。
 * 		String s = "";
 * 		String s = null;
 */
public class StringDemo {
	public static void main(String[] args) {
		// 创建字符串对象
		String s1 = "helloworld";
		String s2 = "helloworld";
		String s3 = "HelloWorld";

		// boolean equals(Object obj):比较字符串的内容是否相同,区分大小写
		System.out.println("equals:" + s1.equals(s2));
		System.out.println("equals:" + s1.equals(s3));
		System.out.println("-----------------------");

		// boolean equalsIgnoreCase(String str):比较字符串的内容是否相同,忽略大小写
		System.out.println("equals:" + s1.equalsIgnoreCase(s2));
		System.out.println("equals:" + s1.equalsIgnoreCase(s3));
		System.out.println("-----------------------");

		// boolean contains(String str):判断大字符串中是否包含小字符串
		System.out.println("contains:" + s1.contains("hello"));
		System.out.println("contains:" + s1.contains("hw"));
		System.out.println("-----------------------");

		// boolean startsWith(String str):判断字符串是否以某个指定的字符串开头
		System.out.println("startsWith:" + s1.startsWith("h"));
		System.out.println("startsWith:" + s1.startsWith("hello"));
		System.out.println("startsWith:" + s1.startsWith("world"));
		System.out.println("-----------------------");

		// 练习：boolean endsWith(String str):判断字符串是否以某个指定的字符串结尾这个自己玩

		// boolean isEmpty():判断字符串是否为空。
		System.out.println("isEmpty:" + s1.isEmpty());

		String s4 = "";
		String s5 = null;
		System.out.println("isEmpty:" + s4.isEmpty());
		// NullPointerException
		// s5对象都不存在，所以不能调用方法，空指针异常
		System.out.println("isEmpty:" + s5.isEmpty());
	}
}

```


### 3.2 获取方法
**获取字符串的长度**：public int length()
**获取指定索引位置的字符** ： public char charAt(int index)

**获取指定字符在此字符串中第一次出现处的索引。**
- public int indexOf(int ch)

**返回指定字符在此字符串中从指定位置后第一次出现处的索引**
- public int indexOf(int ch, int fromIndex)


**返回指定字符串在此字符串中第一次出现处的索引。**
- public int indexOf(String str)

**返回指定字符串在此字符串中从指定位置后第一次出现处的索引。**
- public int indexOf(String str, int fromIndex)

**获取子串**
- public String substring(int start)
- public String substring(int start, int end)


**替换**
- String replace(char old, char new)
- String replace(String old, String new)



### 3.3 其他常用方法
**去除两端空格**
- String trim()

**按字典顺序比较**
- int compareTo(String str)
- int compareToIgnoreCase(String str)

```java
// 按字典顺序比较两个字符串
		String s6 = "hello";
		String s7 = "hello";
		String s8 = "abc";
		String s9 = "xyz";
		System.out.println(s6.compareTo(s7));// 0 
		System.out.println(s6.compareTo(s8));// 7    s6比s8大7
		System.out.println(s6.compareTo(s9));// -16  s6比s9小16
```
String类的compareTo方法的源码解析
```java
  private final char value[];
  
    字符串会自动转换为一个字符数组。
  
  public int compareTo(String anotherString) {
  		//this -- s1 -- "hello"
  		//anotherString -- s2 -- "hel"
  
        int len1 = value.length; //this.value.length--s1.toCharArray().length--5
        int len2 = anotherString.value.length;//s2.value.length -- s2.toCharArray().length--3
        int lim = Math.min(len1, len2); //Math.min(5,3); -- lim=3;
        char v1[] = value; //s1.toCharArray()
        char v2[] = anotherString.value;
        
        //char v1[] = {'h','e','l','l','o'};
        //char v2[] = {'h','e','l'};

        int k = 0;
        while (k < lim) {
            char c1 = v1[k]; //c1='h','e','l'
            char c2 = v2[k]; //c2='h','e','l'
            if (c1 != c2) {
                return c1 - c2;
            }
            k++;
        }
        return len1 - len2; //5-3=2;
   }
   
   String s1 = "hello";
   String s2 = "hel";
   System.out.println(s1.compareTo(s2)); // 2
```



**转大小写**
- public String toUpperCase()
- public String toLowerCase()

**把字符串拼接**
- public String concat(String str)



---

## 四、String的特点
### 4.1 一旦被赋值就不能改变


![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702100957.png)







### 4.2 String s = "abc"和new String("abc")的区别

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702100958.png)


### 4.3  String之间的加法规则
变量相加先开空间再拼接，常量先拼接再找，没有就创建

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702100959.png)


## 五、String与其他类型的转换
### 5.1 字节数组转化
1. 字节数组转字符串
- public String(byte[] bytes)（注意字节数组的格式，数值范围-128~127）
- public String(byte[] bytes, int index, int length)

2. 字符串转字节数组
- public byte[] getBytes()

### 5.2 字符数组转化
1. 字符数组转字符串
- public String(char[] value)
- public String(char[] value, int index, int count)
- public static String valueOf(char[] chs)


2. 字符串转字符数组
- public char[] toCharArray()


### 5.3 基本类型转字符串
- public static String valueOf(int i)

可以把任意类型的数据转换成字符串的表示形式
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702101000.png)
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702101001.png)




## 六、面试题中遇到的

### 6.1 ”=“的直接赋值问题

String str1 = “Hello” ;
String str2 = new String(“Hello”) ;
String str3 = str2 ; // 引用传递


str1 == str2; //false
str2 == str3; //true

这里涉及到 栈内存和堆内存 的区别。
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702101002.png)
通过以上分析可以发现，`==`  比较的不是字符串对象包含的内容，而是两个对象所在的的内存对象的数值。所以  `==` 属于数值比较，比较的是内存地址。

### 6.2 "+"连接符的原理

Java语言为“+”连接符以及对象转换为字符串提供了特殊的支持，字符串对象可以使用“+”连接其他对象。其中**字符串连接是通过 StringBuilder（或 StringBuffer）类及其append 方法实现的**，对象转换为字符串是通过 toString 方法实现的，该方法由 Object 类定义，并可被 Java 中的所有类继承。有关字符连接和转换的更多信息，可以参阅 Gosling、Joy 和 Steele 合著的 《The Java Language Specification》
<font color=#95A5A6>此处借鉴原文链接：[https://blog.csdn.net/ifwinds/article/details/80849184]()</font>


我们可以通过反编译验证一下

```java
/**
 * 测试代码
 */
public class Test {
    public static void main(String[] args) {
        int i = 10;
        String s = "abc";
        System.out.println(s + i);
    }
}

/**
 * 反编译后
 */
public class Test {
    public static void main(String args[]) {    //删除了默认构造函数和字节码
        byte byte0 = 10;      
        String s = "abc";      
        System.out.println((new StringBuilder()).append(s).append(byte0).toString());
    }
}

```


由上可以看出，<font color=#E74C3C>Java中使用"+"连接字符串对象时，会创建一个StringBuilder()对象，并调用append()方法将数据拼接，</font>最后调用toString()方法返回拼接好的字符串。由于append()方法的各种重载形式会，<font color=#E74C3C>调用String.valueOf方法</font>，所以我们可以认为：

```java
//以下两者是等价的
s = i + ""
s = String.valueOf(i);
 
//以下两者也是等价的
s = "abc" + i;
s = new StringBuilder("abc").append(i).toString();

```

调用形式的图解：
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702101003.png)

注：由此可见，+ 号连接符在大量使用时由于创建多个StringBuilder实例会降低效率









# （二）StringBuilder



## 一、概述

**StringBuilder定义**
StringBuilder是一个可变的字符序列。此类提供一个与 StringBuffer 兼容的 API，**但不保证同步**。该类被设计用作 StringBuffer 的一个简易替换，用在**字符串缓冲区**被单个线程使用的时候（这种情况很普遍）。

**+符号的弊端**
在程序开发过程中，我们常常碰到字符串连接的情况，方便和直接的方式是通过`"+"符号`来实现，但是这种方式达到目的的效率比较低，且`每执行一次都会创建一个String对象`，即耗时，又浪费空间。使用StringBuilder类就可以避免这种问题的发生

**append 方法的原理**
在 StringBuilder 上的主要操作是 **append 和 insert 方法**。每个方法都能有效地将给定的数据转换成字符串，然后将该字符串的字符添加或插入到**字符串生成器**中。
 **每个字符串生成器都有一定的容量**。只要字符串生成器所包含的字符序列的长度没有超出此容量，就无需分配新的内部缓冲区。如果内部缓冲区溢出，则此容量自动增大。 将StringBuilder的实例用于多个线程是不安全的。如果需要这样的同步，则建议使用StringBuffer。 [1]  StringBuilder类可以用于在无需创建一个新的字符串对象情况下修改字符串。StringBuilder不是线程安全的，而StringBuffer是线程安全的。但StringBuilder在单线程中的性能比StringBuffer高。



# （三）、StringBuffer





## 一、概述

String类是`字符串常量`，是不可更改的常量。而StringBuffer是`字符串变量`，它的对象是可以扩充和修改的。

**StringBuffer是使用缓冲区的**，本身也是操作字符串的，但与String类不同，String类中的内容一旦声明之后不可改变，改变的只是其内存地址的指向，而StringBuffer中的内容是可以改变的 。

对于StringBuffer而言，本身是一个具体的操作类，所以不能像String那样采用直接赋值的方式进行对象实例化，必须通过**构造方法**完成。

StringBuffer连接字符操作
当一个字符串的内容需要被经常改变时就要使用StringBuffer。
在StringBuffer中使用**append()方法**，完成字符串的连接操作。





## 二、构造方法

- StringBuffer()
- StringBuffer(String str)
- StringBuffer(int capacity)
- StringBuffer(CharSequence seq)

> CharSequence 是个接口，它有几个实现类：CharBuffer, Segment, String, StringBuffer,
> StringBuilder




## 三、常用方法
获取长度：
- 获取理想容量：capacity()
- 获取实际长度：length()

StringBuffer的capacity()和length()的区别如下图：
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702102230.png)


**添加**

- 在末尾追加：public StringBuffer append(String str)
- 在指定位置插入：public StringBuffer insert(int offset, String str)

注意：append方法可以追加的数据类型包含很多种，并且支持链式的写法
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702102231.png)

**删除**

- 删除指定位置的元素：public StringBuffer deleteCharAt(int index)

- 删除某个区间的元素（包左不包右）：public StringBuffer delete(int start, int end)



**替换**

- public StringBuffer replace(int start, int end, String str)

**反转**
`注意：String类是没有这个功能的`
- public StringBuffer reverse()

**截取**
`注意这里返回值不再是StringBuffer了`
public String subString(int start)

public String subString(int start, int end)



![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702102232.png)

## 四、案例

### 4.1 把数组拼接成指定格式的字符串
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702102233.png)

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702102234.png)
### 4.2 字符串反转
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702102235.png)
### 4.3 判断一个字符串是否对称（回文）
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702102236.png)

```java
 public static boolean isTenet(String s){
        boolean flag = true;

        char[] chars = s.toCharArray();
        for (int start=0,end = chars.length - 1;start <= end;start++,end--){
            if (chars[start] != chars[end]){
                flag = false;
                break;
            }
        }
        return flag;
    }
```

```java
 public static boolean isTenet2(String s){
        return new StringBuffer(s).reverse().toString().equals(s);
    }
```


## 五、常见面试题
### 5.1 String、StringBuffer、StringBuilder的区别

- String 是内容不可变的，StringBuffer、StringBuilder内容是可以修改的
- StringBuffer是线程安全的，效率低，StringBuilder是线程不安全的，单线程使用效率高。

### 5.2 StringBuffer和数组的区别

- StringBuffer 的数据最终是一个字符串数据
- 数组可以存放多种数据类型，但必须是同一种数据类型的。



### 5.3 常见对象 String、StringBuffer分别作为参数传递

抓住拼接的本质即可，String会另外开辟空间，StringBuffer操作本身！
```java
/**
 * 常见对象 String、StringBuffer分别作为参数传递
 */
public class Test01 {

    public static void main(String[] args) {
        String s1 = "hello";
        String s2 = "world";
        change(s1,s2);
        System.out.println(s1 + "-----" + s2);//hello-----world

        StringBuffer sb1 = new StringBuffer("hello");
        StringBuffer sb2 = new StringBuffer("world");
        change(sb1,sb2);
        System.out.println(sb1 + "-----" + sb2);//hello-----worldworld

    }

    public static void change(String s1,String s2){
        s1 = s2;
        s2 = s1 + s2;
    }
    public static void change(StringBuffer sb1,StringBuffer sb2){
      sb1 = sb2;
      sb2.append(sb1);
    }
}

```
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702102237.png)

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702102238.png)