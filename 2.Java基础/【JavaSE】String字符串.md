【JavaSE】String字符串







# ==（一）String类==



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
3. 字符数组转字符串

```java
public String(char[] value)
public String(char[] value, int index, int count)
```

4. 字符串常量值转成字符串对象：

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

| 1    | char charAt(int index)                                       | 返回指定索引处的 char 值。                                   |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 2    | int compareTo(Object o)                                      | 把这个字符串和另一个对象比较。                               |
| 3    | int compareTo(String anotherString)                          | 按字典顺序比较两个字符串。                                   |
| 4    | int compareToIgnoreCase(String str)                          | 按字典顺序比较两个字符串，不考虑大小写。                     |
| 5    | String concat(String str)                                    | 将指定字符串连接到此字符串的结尾。                           |
| 6    | boolean contentEquals(StringBuffer sb)                       | 当且仅当字符串与指定的StringButter有相同顺序的字符时候返回真。 |
| 7    | static String copyValueOf(char[] data)                       | 返回指定数组中表示该字符序列的 String。                      |
| 8    | static String copyValueOf(char[] data, int offset,  int count) | 返回指定数组中表示该字符序列的 String。                      |
| 9    | boolean endsWith(String suffix)                              | 测试此字符串是否以指定的后缀结束。                           |
| 10   | boolean equals(Object anObject)                              | 将此字符串与指定的对象比较。                                 |



| 11   | boolean equalsIgnoreCase(String anotherString)               | 将此 String 与另一个 String 比较，不考虑大小写。             |
| ---- | :----------------------------------------------------------- | ------------------------------------------------------------ |
| 12   | byte[] getBytes()                                            | 使用平台的默认字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中。 |
| 13   | byte[] getBytes(String charsetName)                          | 使用指定的字符集将此 String 编码为 byte 序列，并将结果存储到一个新的 byte 数组中。 |
| 14   | void getChars(int srcBegin, int srcEnd, char[] dst,  int dstBegin) | 将字符从此字符串复制到目标字符数组。                         |
| 15   | int hashCode()                                               | 返回此字符串的哈希码。                                       |
| 16   | int indexOf(int ch)                                          | 返回指定字符在此字符串中第一次出现处的索引。                 |
| 17   | int indexOf(int ch, int fromIndex)                           | 返回在此字符串中第一次出现指定字符处的索引，从指定的索引开始搜索。 |
| 18   | int indexOf(String str)                                      | 返回指定子字符串在此字符串中第一次出现处的索引。             |
| 19   | int indexOf(String str, int fromIndex)                       | 返回指定子字符串在此字符串中第一次出现处的索引，从指定的索引开始。 |
| 20   | String intern()                                              | 返回字符串对象的规范化表示形式。                             |



| 21   | int lastIndexOf(int ch)                                      | 返回指定字符在此字符串中最后一次出现处的索引。               |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 22   | int lastIndexOf(int ch, int fromIndex)                       | 返回指定字符在此字符串中最后一次出现处的索引，从指定的索引处开始进行反向搜索。 |
| 23   | int lastIndexOf(String str)                                  | 返回指定子字符串在此字符串中最右边出现处的索引。             |
| 24   | int lastIndexOf(String str, int fromIndex)                   | 返回指定子字符串在此字符串中最后一次出现处的索引，从指定的索引开始反向搜索。 |
| 25   | int length()                                                 | 返回此字符串的长度。                                         |
| 26   | boolean matches(String regex)                                | 告知此字符串是否匹配给定的正则表达式。                       |
| 27   | boolean regionMatches(boolean ignoreCase, int  toffset, String other, int ooffset, int len) | 测试两个字符串区域是否相等。                                 |
| 28   | boolean regionMatches(int toffset, String other, int  ooffset, int len) | 测试两个字符串区域是否相等。                                 |
| 29   | String replace(char oldChar, char newChar)                   | 返回一个新的字符串，它是通过用 newChar 替换此字符串中出现的所有 oldChar 得到的。 |
| 30   | String replaceAll(String regex, String replacement )         | 使用给定的 replacement 替换此字符串所有匹配给定的正则表达式的子字符串。 |



| 31   | String replaceFirst(String regex, String replacement)   | 使用给定的 replacement 替换此字符串匹配给定的正则表达式的第一个子字符串。 |
| ---- | ------------------------------------------------------- | ------------------------------------------------------------ |
| 32   | String[] split(String regex)                            | 根据给定正则表达式的匹配拆分此字符串。                       |
| 33   | String[] split(String regex, int limit)                 | 根据匹配给定的正则表达式来拆分此字符串。                     |
| 34   | boolean startsWith(String prefix)                       | 测试此字符串是否以指定的前缀开始。                           |
| 35   | boolean startsWith(String prefix, int toffset)          | 测试此字符串从指定索引开始的子字符串是否以指定前缀开始。     |
| 36   | CharSequence subSequence(int beginIndex, int  endIndex) | 返回一个新的字符序列，它是此序列的一个子序列。               |
| 37   | String substring(int beginIndex)                        | 返回一个新的字符串，它是此字符串的一个子字符串。             |
| 38   | String substring(int beginIndex, int endIndex)          | 返回一个新字符串，它是此字符串的一个子字符串。               |
| 39   | char[] toCharArray()                                    | 将此字符串转换为一个新的字符数组。                           |
| 40   | String toLowerCase()                                    | 使用默认语言环境的规则将此 String 中的所有字符都转换为小写。 |
| 41   | String toLowerCase(Locale locale)                       | 使用给定 Locale 的规则将此 String 中的所有字符都转换为小写。 |
| 42   | String toString()                                       | 返回此对象本身（它已经是一个字符串！）。                     |
| 43   | String toUpperCase()                                    | 使用默认语言环境的规则将此 String 中的所有字符都转换为大写。 |
| 44   | String toUpperCase(Locale locale)                       | 使用给定 Locale 的规则将此 String 中的所有字符都转换为大写。 |
| 45   | String trim()                                           | 返回字符串的副本，忽略前导空白和尾部空白。                   |
| 46   | static String valueOf(primitive data type x)            | 返回给定data type类型x参数的字符串表示形式。                 |



### 3.3 其他常用方法
- 

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









# ==（二）StringBuilder==



## 一、概述

**StringBuilder定义**
StringBuilder是一个可变的字符序列。此类提供一个与 StringBuffer 兼容的 API，**但不保证同步**。该类被设计用作 StringBuffer 的一个简易替换，用在**字符串缓冲区**被单个线程使用的时候（这种情况很普遍）。

**+符号的弊端**
在程序开发过程中，我们常常碰到字符串连接的情况，方便和直接的方式是通过`"+"符号`来实现，但是这种方式达到目的的效率比较低，且`每执行一次都会创建一个String对象`，即耗时，又浪费空间。使用StringBuilder类就可以避免这种问题的发生

**append 方法的原理**
在 StringBuilder 上的主要操作是 **append 和 insert 方法**。每个方法都能有效地将给定的数据转换成字符串，然后将该字符串的字符添加或插入到**字符串生成器**中。
 **每个字符串生成器都有一定的容量**。只要字符串生成器所包含的字符序列的长度没有超出此容量，就无需分配新的内部缓冲区。如果内部缓冲区溢出，则此容量自动增大。 将StringBuilder的实例用于多个线程是不安全的。如果需要这样的同步，则建议使用StringBuffer。 [1]  StringBuilder类可以用于在无需创建一个新的字符串对象情况下修改字符串。StringBuilder不是线程安全的，而StringBuffer是线程安全的。但StringBuilder在单线程中的性能比StringBuffer高。



# ==（三）StringBuffer==





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


# ==（四）面试题==
## 1. String、StringBuffer、StringBuilder的区别

- String 是内容不可变的，StringBuffer、StringBuilder内容是可以修改的
- StringBuffer是线程安全的，效率低，StringBuilder是线程不安全的，单线程使用效率高。

## 2. StringBuffer和数组的区别

- StringBuffer 的数据最终是一个字符串数据
- 数组可以存放多种数据类型，但必须是同一种数据类型的。



## 3. 常见对象 String、StringBuffer分别作为参数传递

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



## 4. String为什么设计为不可变的

https://www.jianshu.com/p/458283e5ce81

