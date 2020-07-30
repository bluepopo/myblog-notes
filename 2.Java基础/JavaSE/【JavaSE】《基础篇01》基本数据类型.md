【JavaSE】基本数据类型与类型转换问题



# 一、基本数据类型

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200729212722.png" alt="image-20200729210902124" style="zoom:100%;" />

注意：

> float、double两种类型的最小值与Float.MIN_VALUE、 Double.MIN_VALUE的值并不相同，实际上Float.MIN_VALUE和Double.MIN_VALUE分别指的是 float和double类型所能表示的最小正数。也就是说存在
>
> 
>
> float ： 3.4e-45~1.4e38
>
> double ：4.9e-324~1.8e308
>
> 这样一种情况，0到±Float.MIN_VALUE之间的值float类型无法表示，
>
> 0 到±Double.MIN_VALUE之间的值double类型无法表示。这并没有什么好奇怪的，因为这些范围内的数值超出了它们的精度范围。
>
> 默认浮点类型为double，所以double类型可以不加d，而float类型必须加f







基本类型的优势：数据存储相对简单，运算效率比较高

包装类的优势：有的容易，比如集合的元素必须是对象类型，满足了java一切皆是对象的思想





# 二、基本数据类型之间的转换

简单类型数据间的转换,有两种方式:自动转换和强制转换,通常发生在表达式中或方法的参数传递时。

### 自动转换

小数据可以自动转换为大数据。具体地讲,当一个较"小"数据与一个较"大"的数据一起运算时,系统将自动将"小"数据转换成"大"数据,再进行运算。

这些类型由"小"到"大"分别为 ==(byte，short，char) ------> int ------> long ------>float ------>double。==这里我们所说的"大"与"小",并不是指占用字节的多少,而是指表示值的范围的大小。



①下面的语句可以在Java中直接通过：

```java
byte b;

int i=b; 
long l=b; 
float f=b; 
double d=b;


```





②如果低级类型为char型，向高级类型（整型）转换时，会转换为对应ASCII码值，例如

```java
char c='c'; 
int i=c;
System.out.println("output:"+i);输出：output:99;
```

③对于byte,short,char三种类型而言，他们是平级的，因此不能相互自动转换，可以使用下述的强制类型转换。

```java
short i=99;
char c=(char)i;
System.out.println("output:"+c);//输出：output:c;
```

### 强制转换

将"大"数据转换为"小"数据时，你可以使用强制类型转换。即你必须采用下面这种语句格式： int n=(int)3.14159/2;可以想象，这种转换肯定可能会导致溢出或精度的下降。

### 类型自动提升

表达式的数据类型自动提升, 关于类型的自动提升，注意下面的规则。

①所有的`byte,short,char`型的值将被提升为 `int `型；

②如果有一个操作数是long型，计算结果是long型；

③如果有一个操作数是float型，计算结果是float型；

④如果有一个操作数是double型，计算结果是double型；

```java
 byte b; 
 b=3; 
 b=(byte)(b*3);//必须声明(byte)
```







### 包装类过渡类型转换

①当希望把float型转换为double型时：

```java
float f1=100.00f;

Float F1=new Float(f1);

double d1=F1.doubleValue();//F1.doubleValue()为Float类的返回double值型的方法
```

②当希望把double型转换为int型时：

```java
double d1=100.00;

Double D1=new Double(d1);

int i1=D1.intValue();
```

简单类型的变量转换为相应的包装类，可以利用包装类的构造函数。即：

```java
new Boolean(boolean value)、//构造器参数都可以接收对应的基本类型
    Character(char value)、
    Integer(int value)、
    Long(long value)、
    Float(float value)、
    Double(double value)
```

而在各个包装类中，总有形为 ==`xxValue()`的方法，转换为对应的简单类型数据==。利用这种方法，也可以实现不同数值型变量间的转换，例如，对于一个双精度实型类，intValue()可以得到其对应的整型变量，而doubleValue()可以得到其对应的双精度实型变量。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200729234824.png" alt="image-20200729225959197" style="zoom:100%;" />

补充：

字符串与其它类型间的转换

> - ==其它类型向字符串的转换==
>
> ①调用类的串转换方法:X.toString();
>
> ②自动转换:X+"";
>
> ③使用String的方法:String.volueOf(X);
>
> - ==字符串作为值,向其它类型的转换==
>
> ①先转换成相应的封装器实例,再调用对应的方法转换成其它类型
>
> 例如，字符中"32.1"转换double型的值的格式为:new Float("32.1").doubleValue()。也可以用:Double.valueOf("32.1").doubleValue()
>
> ②静态parseXXX方法
>
> ```java
> String s = "1";
> 
> byte b = Byte.parseByte( s );
> 
> short t = Short.parseShort( s );
> 
> int i = Integer.parseInt( s );
> 
> long l = Long.parseLong( s );
> 
> Float f = Float.parseFloat( s );
> 
> Double d = Double.parseDouble( s );
> ```
>
> ③Character的 getNumericValue(char ch) 方法
>
> ```java
> char ch ='A';
> int value = Character.getNumericValue(ch);//10
> /**
> * getNumericValue()将实际表示数字的字符(如普通数字0-9字符)转换为其int型数字值
>  */
> ```
>
> 



###  原始数据类型和 Java 泛型并不能配合使用

Java 的泛型某种程度上可以算作伪泛型，它完全是一种编译期的技巧，Java 编译期会自动将类型转换为对应的特定类型，这就决定了使用泛型，必须保证相应类型可以转换为Object。

# 三、引用类型

###   对象的实例化过程

Java有 5种引用类型（对象类型）：类 、接口、 数组 、枚举 、标注

创建一个对象的过程：

eg：Student stu = new Student（）；

上例实现步骤：

- JVM加载Student.class 到方法区

- new Student()在堆空间分配空间并创建一个Student实例

- 将此实例的地址赋值给占空间的变量引用 stu



# 四、面试题



### 问题1

```java
byte b1=3,b2=4,b;
b=b1+b2;
b=3+4;
//哪句是编译失败的呢？为什么呢？
```



1. b=b1+b2;  编译失败: byte 运算先变成int,再计算，再赋值给byte类型，很明显 int类型不能自动转成 byte类型。 **byte,char,short编译时都是需要转成int，才可运算。**
2. ​    **变量相加**,必须考虑基本数据类型的转换能不能转。
3. ​    b=3+4; 编译正确:  因为他们都是常量 具有常量类型优化机制 可以直接识别为byte，两个常量相加，先计算常量数值，然后判断是否满足类型范围，再赋值。】
4. **常量相加**，优化机制，直接看计算结果在不在byte类型所允许的范围内。





### 问题2

```java
short s=1;
s = s + 1; //（错误）默认类型提升,提升为int类型,,不能用short去接收int类型的数据 
// 解决方案:强制类型转换

对于扩展的赋值运算符
    s+=1    
                不等价于 :s = s +1 ;
                等价于: s = (s)(s+1) ;
                扩展的赋值运算符隐藏了强制类型转换!
 
//上面两个代码有没有问题，如果有，哪里有问题
```



### 问题3

byte可以用在switch语句的表达式?
long可以作为switch语句的表达式?
String可以作为switch语句的表达式?

答：


switch语句后面的表达式:能跟的类型:byte,short,int,char

byte的存储范围小于int，可以向int类型进行隐式转换，所以switch可以作用在byte上

long的存储范围大于int，不能向int进行隐式转换，只能强制转换，所以switch不可以作用在long上

string在1.7版本之前不可以，1.7版本之后switch就可以作用在string上了。





### 问题4

<font color=#009C41>**java 中 char 类型变量能不能储存一个中文的汉字，为什么？**</font>



**答：**java 的 char 类型变量是用来储存 Unicode 编码字符的，Unicode 字符集包含了汉字，所以 char 类型自然就能存储汉字，但是在某些特殊情况下某个生僻汉字可能没有包含在 Unicode 编码字符集中，这种情况下 char 类型就不能存储该生僻汉字了。



<font color=#009C41>**问：java 中 3\*0.1 == 0.3 将会返回什么？true 还是 false？**</font>



**答：**false，因为浮点数运算不能完全精确的表示出来，一般都会损失精度。