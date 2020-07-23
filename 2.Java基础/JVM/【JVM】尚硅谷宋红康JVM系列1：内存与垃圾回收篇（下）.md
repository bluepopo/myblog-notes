【JVM】尚硅谷宋红康JVM系列1：内存与垃圾回收篇（下）





# 一、执行引擎

## 1.1 执行引擎概述

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624154616.png" alt="image-20200624153748813" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624154605.png" alt="image-20200624153817904" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624154105.png" alt="image-20200624154104488" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624154550.png" alt="image-20200624154548110" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628125831.png" alt="image-20200624154809102" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624155251.png" alt="image-20200624155250217" style="zoom:100%;" />







## 1.2 Java代码编译和执行过程

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628125832.png" alt="image-20200624155401559" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624155711.png" alt="image-20200624155710110" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624155759.png" alt="image-20200624155758065" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624155906.png" alt="image-20200624155905315" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624160229.png" alt="image-20200624160228571" style="zoom:100%;" />

## 1.3 机器码、指令、汇编语言

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628125833.png" alt="image-20200624160534739" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624160849.png" alt="image-20200624160848321" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624161037.png" alt="image-20200624161034930" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624161239.png" alt="image-20200624161238315" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624161441.png" alt="image-20200624161440100" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624161745.png" alt="image-20200624161554889" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624161734.png" alt="image-20200624161733062" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628125834.png" alt="image-20200624161836295" style="zoom:100%;" />





## 1.4 解释器

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624162156.png" alt="image-20200624162155029" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624162636.png" alt="image-20200624162635583" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628125835.png" alt="image-20200624162818852" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624163034.png" alt="image-20200624163033222" style="zoom:100%;" />



## 1.5 JIT编译器

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624163552.png" alt="image-20200624163550753" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624164052.png" alt="image-20200624164051084" style="zoom:100%;" />



总结：

- 解释器采用逐行解释，启动快，但是把总体的字节码解释成本地代码的周期较长，需要一定的时间。
- 即时编译器，启动慢，因为启动后要先编译大量字节码变成本地代码，然后一气呵成的执行。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624164654.png" alt="image-20200624164653385" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624165057.png" alt="image-20200624165056966" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624191602.png" alt="image-20200624191007255" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624191458.png" alt="image-20200624191139576" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624191449.png" alt="image-20200624191447897" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624191749.png" alt="image-20200624191747085" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628125836.png" alt="image-20200624191832255" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624191909.png" alt="image-20200624191908146" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624192337.png" alt="image-20200624192336066" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628125837.png" alt="image-20200624192420791" style="zoom:100%;" />





## 1.6 HotSpot设置模式：C1、C2编译器

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624192452.png" alt="image-20200624192451551" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624192809.png" alt="image-20200624192805485" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624193707.png" alt="image-20200624193334254" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624193716.png" alt="image-20200624193705494" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624193856.png" alt="image-20200624193855882" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624193941.png" alt="image-20200624193940070" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624193958.png" alt="image-20200624193957109" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624194305.png" alt="image-20200624194304806" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628125838.png" alt="image-20200624194405502" style="zoom:100%;" />



# 二、StringTable

## 2.1 String的基本特性

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624195347.png" alt="image-20200624195346231" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628125839.png" alt="image-20200624213859876" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624214217.png" alt="image-20200624214216228" style="zoom:100%;" />

```java
package com.atguigu.java;

import org.junit.Test;

/**
 * String的基本使用:体现String的不可变性
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  23:42
 */
public class StringTest1 {
    @Test
    public void test1() {
        String s1 = "abc";//字面量定义的方式，"abc"存储在字符串常量池中
        String s2 = "abc";
        s1 = "hello";

        System.out.println(s1 == s2);//判断地址：true  --> false

        System.out.println(s1);//
        System.out.println(s2);//abc

    }

    @Test
    public void test2() {
        String s1 = "abc";
        String s2 = "abc";
        s2 += "def";
        System.out.println(s2);//abcdef
        System.out.println(s1);//abc
    }

    @Test
    public void test3() {
        String s1 = "abc";
        String s2 = s1.replace('a', 'm');
        System.out.println(s1);//abc
        System.out.println(s2);//mbc
    }
}

```



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628125840.png" alt="image-20200624221911367" style="zoom:100%;" />

面试题：考察字符串的不可变性

```java
package com.atguigu.java;

/**
 * @author shkstart  shkstart@126.com
 * @create 2020  23:44
 */
public class StringExer {
    String str = new String("good");
    char[] ch = {'t', 'e', 's', 't'};

    public void change(String str, char ch[]) {
        str = "test ok";
        ch[0] = 'b';
    }

    public static void main(String[] args) {
        StringExer ex = new StringExer();
        ex.change(ex.str, ex.ch);
        System.out.println(ex.str);//good
        System.out.println(ex.ch);//best
    }

}

```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624222800.png" alt="image-20200624222759391" style="zoom:100%;" />











## 2.2 String的内存分配

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200624230442.png" alt="image-20200624230236440" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628125841.png" alt="image-20200624230247736" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628125842.png" alt="image-20200624231148738" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628125843.png" alt="image-20200624231221835" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628125844.png" alt="image-20200624231255185" style="zoom:100%;" />



```java
package com.atguigu.java;

import java.util.HashSet;
import java.util.Set;

/**
 * jdk6中：
 * -XX:PermSize=6m -XX:MaxPermSize=6m -Xms6m -Xmx6m
 *
 * jdk8中：
 * -XX:MetaspaceSize=6m -XX:MaxMetaspaceSize=6m -Xms6m -Xmx6m
 * @author shkstart  shkstart@126.com
 * @create 2020  0:36
 */
public class StringTest3 {
    public static void main(String[] args) {
        //使用Set保持着常量池引用，避免full gc回收常量池行为
        Set<String> set = new HashSet<String>();
        //在short可以取值的范围内足以让6MB的PermSize或heap产生OOM了。
        short i = 0;
        while(true){
            set.add(String.valueOf(i++).intern());
        }
    }
}

```





## 2.3 String的基本操作









<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200625125417.png" alt="image-20200625125408653" style="zoom:100%;" />







<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200625125933.png" alt="image-20200625125931896" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200625130039.png" alt="image-20200625130038059" style="zoom:100%;" />













## 2.4 字符串拼接

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200625130523.png" alt="image-20200625130522093" style="zoom:100%;" />

```java
package com.atguigu.java1;

import org.junit.Test;

/**
 * 字符串拼接操作
 * @author shkstart  shkstart@126.com
 * @create 2020  0:59
 */
public class StringTest5 {
    @Test
    public void test1(){
        String s1 = "a" + "b" + "c";//编译期优化：等同于"abc"
        String s2 = "abc"; //"abc"一定是放在字符串常量池中，将此地址赋给s2
        /*
         * 最终.java编译成.class,再执行.class
         * String s1 = "abc";
         * String s2 = "abc"
         */
        System.out.println(s1 == s2); //true
        System.out.println(s1.equals(s2)); //true
    }

    @Test
    public void test2(){
        String s1 = "javaEE";
        String s2 = "hadoop";

        String s3 = "javaEEhadoop";
        String s4 = "javaEE" + "hadoop";//编译期优化
        //如果拼接符号的前后出现了变量，则相当于在堆空间中new String()，具体的内容为拼接的结果：javaEEhadoop
        String s5 = s1 + "hadoop";
        String s6 = "javaEE" + s2;
        String s7 = s1 + s2;

        System.out.println(s3 == s4);//true
        System.out.println(s3 == s5);//false
        System.out.println(s3 == s6);//false
        System.out.println(s3 == s7);//false
        System.out.println(s5 == s6);//false
        System.out.println(s5 == s7);//false
        System.out.println(s6 == s7);//false
        //intern():判断字符串常量池中是否存在javaEEhadoop值，如果存在，则返回常量池中javaEEhadoop的地址；
        //如果字符串常量池中不存在javaEEhadoop，则在常量池中加载一份javaEEhadoop，并返回次对象的地址。
        String s8 = s6.intern();
        System.out.println(s3 == s8);//true
    }

    
    
      /*
        如下的 s4 = s1 + s2; 的执行细节：(变量s是我临时定义的）
        ① StringBuilder s = new StringBuilder();
        ② s.append("a")
        ③ s.append("b")
        ④ s.toString()  --> 约等于 new String("ab")

        补充：在jdk5.0之后使用的是StringBuilder,在jdk5.0之前使用的是StringBuffer
         */
    @Test
    public void test3(){
        String s1 = "a";
        String s2 = "b";
        String s3 = "ab";
        String s4 = s1 + s2;//左右两边是变量
        System.out.println(s3 == s4);//false
    }
    /*
    1. 字符串拼接操作不一定使用的是StringBuilder!
       如果拼接符号左右两边都是字符串常量或常量引用，则仍然使用编译期优化，即非StringBuilder的方式。
    2. 针对于final修饰类、方法、基本数据类型、引用数据类型的量的结构时，能使用上final的时候建议使用上。
    	编译期优化的意思是，在链接的prepare阶段变量就已经完成了默认初始化赋值操作，已经有等号右边的值了
     */
    @Test
    public void test4(){
        final String s1 = "a";
        final String s2 = "b";
        String s3 = "ab";
        String s4 = s1 + s2;//S1、S2 是使用final修饰的非变量
        System.out.println(s3 == s4);//true
    }
    //练习：
    @Test
    public void test5(){
        String s1 = "javaEEhadoop";
        String s2 = "javaEE";
        String s3 = s2 + "hadoop";
        System.out.println(s1 == s3);//false

        final String s4 = "javaEE";//s4:常量
        String s5 = s4 + "hadoop";
        System.out.println(s1 == s5);//true

    }

    /*
    体会执行效率：通过StringBuilder的append()的方式添加字符串的效率要远高于使用String的字符串+拼接方式！
    详情：① StringBuilder的append()的方式：自始至终中只创建过一个StringBuilder的对象
          使用String的字符串拼接方式：创建过多个StringBuilder和String的对象
         ② 使用String的字符串拼接方式：内存中由于创建了较多的StringBuilder和String的对象，内存占用更大；如果进行GC，需要花费额外的时间。

     改进的空间：在实际开发中，如果基本确定要前前后后添加的字符串长度不高于某个限定值highLevel的情况下,建议使用构造器实例化：
               StringBuilder s = new StringBuilder(highLevel);
               //底层是创建一个数组 new char[highLevel]
     */
    @Test
    public void test6(){

        long start = System.currentTimeMillis();

//        method1(100000);//4014
        method2(100000);//7

        long end = System.currentTimeMillis();

        System.out.println("花费的时间为：" + (end - start));
    }

    public void method1(int highLevel){
        String src = "";
        for(int i = 0;i < highLevel;i++){
            src = src + "a";//每次循环都会new StringBuilder、append、toString
        }
//        System.out.println(src);

    }

    public void method2(int highLevel){
        //只需要创建一个StringBuilder
        StringBuilder src = new StringBuilder();
        for (int i = 0; i < highLevel; i++) {
            src.append("a");
        }
//        System.out.println(src);
    }
}

```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200627193259.png" alt="image-20200627193249311" style="zoom:100%;" />





## ♥2.5 intern()的使用

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200627202012.png" alt="image-20200627202010936" style="zoom:100%;" />



## ♥2.6 intern()的面试题

- 首先先了解一个基础知识点：





<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200627202840.png" alt="image-20200627202839174" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200627204120.png" alt="image-20200627203904404" style="zoom:100%;" />





```java
package com.atguigu.java2;

/**
 * 题目：
 * new String("ab")会创建几个对象？看字节码，就知道是两个。
 *     一个对象是：new关键字在堆空间创建的
 *     另一个对象是：字符串常量池中的对象"ab"。 字节码指令：ldc
 *
 *
 * 思考：
 * new String("a") + new String("b")呢？答：6个
 *  对象1：new StringBuilder()
 *  对象2： new String("a")
 *  对象3： 常量池中的"a"
 *  对象4： new String("b")
 *  对象5： 常量池中的"b"
 *
 *  深入剖析： StringBuilder的toString():
 *      对象6 ：new String("ab")
 *       强调一下，通过字节码观察，StringBuilder的toString()的调用，在字符串常量池中，没有生成"ab"
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  20:38
 */
public class StringNewTest {
    public static void main(String[] args) {
//        String str = new String("ab");

        String str = new String("a") + new String("b");
    }
}

```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200627204505.png" alt="image-20200627204504556" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200627204721.png" alt="image-20200627204720403" style="zoom:100%;" />



- 我们再来看一个复杂的应用面试题

```java
package com.atguigu.java2;

import org.junit.Test;

/**
 * 如何保证变量s指向的是字符串常量池中的数据呢？
 * 有两种方式：
 * 方式一： String s = "shkstart";//字面量定义的方式
 * 方式二： 调用intern()
 *         String s = new String("shkstart").intern();
 *         String s = new StringBuilder("shkstart").toString().intern();
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  18:49
 */
public class StringIntern {
    public static void main(String[] args) {

        String s = new String("1");//创建了两个对象，这里返回堆空间中的对象
        s.intern();//调用此方法之前，字符串常量池中已经存在了"1"
        String s2 = "1";//返回常量池中的引用地址
        System.out.println(s == s2);//jdk6：false   jdk7/8：false


        String s3 = new String("1") + new String("1");//s3变量记录的地址为堆空间：new String("11")
        //执行完上一行代码以后，字符串常量池中，是否存在"11"呢？答案：不存在！！
        s3.intern();
        //在字符串常量池中生成"11"。
        //如何理解：jdk6:创建了一个新的对象在永久代的常量池中"11",也就有新的地址。
        //jdk7/8为了确保相同字符串在内存中只有一份拷贝，这里intern()会优化
     	// jdk7:此时常量中并没有在常量池中new 一个"11"对象,而是开辟一小块空间创建一个指向堆空间中new String("11")的地址，所以这样一来，常量池中的“11”和堆空间的“11”用的是相同地址。
        
        String s4 = "11";//s4变量记录的地址：使用的是上一行代码代码执行时，在常量池中生成的"11"的地址
        System.out.println(s3 == s4);//jdk6：false  jdk7/8：true
        
        //在jdk7/8，虽然s3引用堆空间，s4引用常量池，但是它们引用的地址都是相同得
    }


}

```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628125845.png" alt="image-20200628095846049" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628125846.png" alt="image-20200628095909311" style="zoom:100%;" />

```java
package com.atguigu.java2;

/**
 * @author shkstart  shkstart@126.com
 * @create 2020  22:10
 */
public class StringIntern1 {
    public static void main(String[] args) {
        //StringIntern.java中练习的拓展：
        String s3 = new String("1") + new String("1");//new String("11")
        //执行完上一行代码以后，字符串常量池中，是否存在"11"呢？答案：不存在！！
        String s4 = "11";	//在字符串常量池中生成新的对象"11"
        
        String s5 = s3.intern();
        //调用intern()方法，先去常量池中寻找“11”，没有的话就优化创建，但是这里已经有了，就直接引用常量池中已存在的“11”
        //所以推测 s4与s5是引用相同地址的，s3是引用堆空间地址的和他俩都不等价
        System.out.println(s3 == s4);//false
        System.out.println(s5 == s4);//true
        System.out.println(s3 == s5);//false
    }
}

```



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628105053.png" alt="image-20200628105051468" style="zoom:100%;" />





## ♥2.7 intern()的练习

练习一：

```java
package com.atguigu.java2;

/**
 * @author shkstart  shkstart@126.com
 * @create 2020  20:17
 */
public class StringExer1 {
    public static void main(String[] args) {
        String x = "ab";
        String s = new String("a") + new String("b");//new String("ab")
        //在上一行代码执行完以后，字符串常量池中并没有"ab"

        String s2 = s.intern();//jdk6中：在串池中创建一个字符串"ab"
                               //jdk8中：串池中没有创建字符串"ab",而是创建一个引用，指向new String("ab")，将此引用返回

        System.out.println(s2 == "ab");//jdk6:true  jdk8:true
        System.out.println(s == "ab");//jdk6:false  jdk8:true
    }
}

```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628105829.png" alt="image-20200628105826450" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628110223.png" alt="image-20200628110222920" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628110109.png" alt="image-20200628110108178" style="zoom:100%;" />





练习2：

```java
package com.atguigu.java2;

/**
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  20:26
 */
public class StringExer2 {
    public static void main(String[] args) {
        String s1 = new String("ab");//执行完以后，会在字符串常量池中会生成"ab"
//        String s1 = new String("a") + new String("b");////执行完以后，不会在字符串常量池中会生成"ab"//true
        s1.intern();
        String s2 = "ab";
        System.out.println(s1 == s2);//false  
    }
}

```



## 2.8 intern()的效率测试



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628122028.png" alt="image-20200628122027141" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628123111.png" alt="image-20200628123110553" style="zoom:100%;" />

## 2.6 StringTable的垃圾回收

```java
package com.atguigu.java3;

/**
 * String的垃圾回收:
 * -Xms15m -Xmx15m -XX:+PrintStringTableStatistics -XX:+PrintGCDetails
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  21:27
 */
public class StringGCTest {
    public static void main(String[] args) {
        for (int j = 0; j < 100000; j++) {
            String.valueOf(j).intern();
        }
    }
}

```



## 2.7 G1中字符串去重操作

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628124456.png" alt="image-20200628124455515" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628125847.png" alt="image-20200628124801094" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628125041.png" alt="image-20200628125040483" style="zoom:100%;" />



# 三、垃圾回收

## 3.1 垃圾回收的概述

### 3.1.1 什么是垃圾

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628130700.png" alt="image-20200628130659838" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628130909.png" alt="image-20200628130908285" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628131010.png" alt="image-20200628131009695" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628131418.png" alt="image-20200628131417809" style="zoom:100%;" />



### 3.1.2 为什么需要GC

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628161630.png" alt="image-20200628131751267" style="zoom:100%;" />

### 3.1.3 早期垃圾回收

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628161620.png" alt="image-20200628131833169" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628161614.png" alt="image-20200628132034213" style="zoom:100%;" />







### 3.1.4 Java垃圾回收的机制

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628132212.png" alt="image-20200628132211476" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628161606.png" alt="image-20200628132427080" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628133650.png" alt="image-20200628132541921" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628133606.png" alt="image-20200628133056218" style="zoom:100%;" />









## 3.2 垃圾回收的算法

### 3.2.1 标记阶段：引用计数算法

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628134346.png" alt="image-20200628134345611" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628161559.png" alt="image-20200628135303470" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628160434.png" alt="image-20200628135556000" style="zoom:100%;" />







代码实例：python得引用计数器实验方案

```java
package com.atguigu.java;

/**
 * -XX:+PrintGCDetails
 * 证明：java使用的不是引用计数算法
 * @author shkstart
 * @create 2020 下午 2:38
 */
public class RefCountGC {
    //这个成员属性唯一的作用就是占用一点内存
    private byte[] bigSize = new byte[5 * 1024 * 1024];//5MB

    Object reference = null;

    public static void main(String[] args) {
        RefCountGC obj1 = new RefCountGC();
        RefCountGC obj2 = new RefCountGC();

        obj1.reference = obj2;
        obj2.reference = obj1;

        obj1 = null;
        obj2 = null;
        //显式的执行垃圾回收行为
        //这里发生GC，obj1和obj2能否被回收？
        System.gc();

        try {
            Thread.sleep(1000000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

```





<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628160118.png" alt="image-20200628160117491" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628160843.png" alt="image-20200628160838748" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628161113.png" alt="image-20200628161112488" style="zoom:100%;" />

### 3.2.2 标记阶段：可达性分析算法

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628161400.png" alt="image-20200628161358800" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628161547.png" alt="image-20200628161546756" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628161838.png" alt="image-20200628161834209" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628170144.png" alt="image-20200628162114457" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628170150.png" alt="image-20200628162801918" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628170155.png" alt="image-20200628162822611" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628170137.png" alt="image-20200628163131459" style="zoom:100%;" />



### 3.2.3 对象得finalization机制

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628163435.png" alt="image-20200628163434736" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628163821.png" alt="image-20200628163819446" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628170129.png" alt="image-20200628164224086" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628165234.png" alt="image-20200628165233093" style="zoom:100%;" />



```java
package com.atguigu.java;

/**
 * 测试Object类中finalize()方法，即对象的finalization机制。
 *
 * @author shkstart
 * @create 2020 下午 2:57
 */
public class CanReliveObj {
    public static CanReliveObj obj;//类变量，属于 GC Root


    //此方法只能被调用一次
    @Override
    protected void finalize() throws Throwable {
        super.finalize();
        System.out.println("调用当前类重写的finalize()方法");
        obj = this;//当前待回收的对象在finalize()方法中与引用链上的一个对象obj建立了联系
    }


    public static void main(String[] args) {
        try {
            obj = new CanReliveObj();
            // 对象第一次成功拯救自己
            obj = null;
            System.gc();//调用垃圾回收器
            System.out.println("第1次 gc");
            // 因为Finalizer线程优先级很低，暂停2秒，以等待它
            Thread.sleep(2000);
            if (obj == null) {
                System.out.println("obj is dead");
            } else {
                System.out.println("obj is still alive");
            }
            System.out.println("第2次 gc");
            // 下面这段代码与上面的完全相同，但是这次自救却失败了
            obj = null;
            System.gc();
            // 因为Finalizer线程优先级很低，暂停2秒，以等待它
            Thread.sleep(2000);
            if (obj == null) {
                System.out.println("obj is dead");
            } else {
                System.out.println("obj is still alive");
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

```







### 3.2.4 MAT与JProfiler的GC Roots溯源



一、MAT的GC Roots溯源

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628170449.png" alt="image-20200628170448248" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628172641.png" alt="image-20200628170701025" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628172630.png" alt="image-20200628170755246" style="zoom:100%;" />

```java
package com.atguigu.java;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;
import java.util.Scanner;

/**
 * @author shkstart  shkstart@126.com
 * @create 2020  16:28
 */
public class GCRootsTest {
    public static void main(String[] args) {
        List<Object> numList = new ArrayList<>();
        Date birth = new Date();

        for (int i = 0; i < 100; i++) {
            numList.add(String.valueOf(i));
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        System.out.println("数据添加完毕，请操作：");
        new Scanner(System.in).next();
        numList = null;
        birth = null;

        System.out.println("numList、birth已置空，请操作：");
        new Scanner(System.in).next();

        System.out.println("结束");
    }
}

```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628171225.png" alt="image-20200628171223671" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628172618.png" alt="image-20200628171518739" style="zoom:100%;" />





二、JProfiler的GC Roots溯源



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628172602.png" alt="image-20200628172155879" style="zoom:100%;" />

```java
package com.atguigu.java;

import java.util.ArrayList;

/**
 * -Xms8m -Xmx8m -XX:+HeapDumpOnOutOfMemoryError
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  15:29
 */
public class HeapOOM {
    byte[] buffer = new byte[1 * 1024 * 1024];//1MB

    public static void main(String[] args) {
        ArrayList<HeapOOM> list = new ArrayList<>();

        int count = 0;
        try{
            while(true){
                list.add(new HeapOOM());
                count++;
            }
        }catch (Throwable e){
            System.out.println("count = " + count);
            e.printStackTrace();
        }
    }
}

```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628172552.png" alt="image-20200628172550883" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628172727.png" alt="image-20200628172726768" style="zoom:100%;" />





### 3.2.5 清楚阶段：标记-清楚算法

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628172926.png" alt="image-20200628172925242" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628173133.png" alt="image-20200628173131183" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628183603.png" alt="image-20200628175717818" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628183610.png" alt="image-20200628180437348" style="zoom:100%;" />

### 3.2.6 清除阶段：复制算法

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628183440.png" alt="image-20200628181504462" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628183432.png" alt="image-20200628181442913" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628183427.png" alt="image-20200628181919373" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628182113.png" alt="image-20200628182112198" style="zoom:100%;" />





### 3.2.7 清除阶段：标记-压缩算法

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628182226.png" alt="image-20200628182225645" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628182601.png" alt="image-20200628182600524" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628182917.png" alt="image-20200628182916307" style="zoom:100%;" />

![image-20200628183225188](G:\图片\blog\image-20200628183225188.png)

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628183414.png" alt="image-20200628183306372" style="zoom:100%;" />







### 3.2.8 小结



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200628183549.png" alt="image-20200628183548983" style="zoom:100%;" />

![image-20200628183942294](G:\图片\blog\image-20200628183942294.png)

### 3.2.9 分代收集算法

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200629151431.png" alt="image-20200629151422972" style="zoom:100%;" />

![image-20200629152313884](G:\图片\blog\image-20200629152313884.png)

![image-20200629175444041](G:\图片\blog\image-20200629175444041.png)





### 3.2.10 增量收集算法、分区算法

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630140403.png" alt="image-20200629180154525" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200629181159.png" alt="image-20200629181157790" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630140357.png" alt="image-20200629183923786" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200629184414.png" alt="image-20200629184413352" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630140412.png" alt="image-20200629184602997" style="zoom:100%;" />





---









## 3.3 垃圾回收的其他概念



### 3.3.1 System.gc()的理解

- 该方法可以主动的进行垃圾回收

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200629191004.png" alt="image-20200629191003101" style="zoom:100%;" />

```java
package com.atguigu.java;

/**
 * @author shkstart  shkstart@126.com
 * @create 2020  14:49
 */
public class SystemGCTest {
    public static void main(String[] args) {
        new SystemGCTest();
        System.gc();//提醒jvm的垃圾回收器执行gc,但是不确定是否马上执行gc
        //与Runtime.getRuntime().gc();的作用一样。

        System.runFinalization();//强制调用使用引用的对象的finalize()方法
    }

    @Override
    protected void finalize() throws Throwable {
        super.finalize();
        System.out.println("SystemGCTest 重写了finalize()");
    }
}

```

注意：

- 测试代码时发现如果只是调用 System.gc(); 方法了，但是最后结果确实不稳定的，有时候不会执行 finalize()  方法进行垃圾回收，有时候会执行 finalize() 方法完成一次垃圾回收。由此可见，主动调用 System.gc() 不一定就会立即进行垃圾回收。

- `System.gc();`加上`System.runFinalization();` 两个方法一起调用，发现确保会实现立即的垃圾回收。





练习：手动GC-理解不可达对象的垃圾回收行为

```java
package com.atguigu.java;

/**
 * @author shkstart  shkstart@126.com
 * @create 2020  14:57
 */
public class LocalVarGC {
    public void localvarGC1() {
        byte[] buffer = new byte[10 * 1024 * 1024];//10MB
        System.gc();
    }

    public void localvarGC2() {
        byte[] buffer = new byte[10 * 1024 * 1024];
        buffer = null;
        System.gc();
    }

    public void localvarGC3() {
        {
            byte[] buffer = new byte[10 * 1024 * 1024];
        }
        System.gc();
    }

    public void localvarGC4() {
        {
            byte[] buffer = new byte[10 * 1024 * 1024];
        }
        int value = 10;
        System.gc();
    }

    public void localvarGC5() {
        localvarGC1();
        System.gc();
    }

    public static void main(String[] args) {
        LocalVarGC local = new LocalVarGC();
        local.localvarGC5();
    }
}

```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200629194142.png" alt="image-20200629194141824" style="zoom:100%;" />

![image-20200629194351685](G:\图片\blog\image-20200629194351685.png)

![image-20200629194527079](G:\图片\blog\image-20200629194527079.png)

![image-20200629195022197](G:\图片\blog\image-20200629195022197.png)

![image-20200629195608397](G:\图片\blog\image-20200629195608397.png)



### 3.3.2 内存溢出与内存泄漏

![image-20200629201642701](G:\图片\blog\image-20200629201642701.png)

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200629232607.png" alt="image-20200629232606518" style="zoom:100%;" />

![image-20200630001240362](G:\图片\blog\image-20200630001240362.png)

![image-20200630001302132](G:\图片\blog\image-20200630001302132.png)

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630001741.png" alt="image-20200630001535323" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630003443.png" alt="image-20200630003442419" style="zoom:100%;" />



我们的Hotspot虚拟机没有使用引用-计数算法在标记阶段，那么什么情况会出现内存泄漏呢？



![image-20200630003740510](G:\图片\blog\image-20200630003740510.png)

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630003858.png" alt="image-20200630003857585" style="zoom:100%;" />



### 3.3.3 Stop The World 

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630084331.png" alt="image-20200630084323578" style="zoom:100%;" />

![image-20200630085208676](G:\图片\blog\image-20200630085208676.png)

```java
package com.atguigu.java;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;

/**
 * @author shkstart  shkstart@126.com
 * @create 2020  15:50
 */
public class StopTheWorldDemo {
    public static class WorkThread extends Thread {
        List<byte[]> list = new ArrayList<byte[]>();

        public void run() {
            try {
                while (true) {
                    for(int i = 0;i < 1000;i++){
                        byte[] buffer = new byte[1024];
                        list.add(buffer);
                    }

                    if(list.size() > 10000){
                        list.clear();
                        System.gc();//会触发full gc，进而会出现STW事件
                    }
                }
            } catch (Exception ex) {
                ex.printStackTrace();
            }
        }
    }

    public static class PrintThread extends Thread {
        public final long startTime = System.currentTimeMillis();

        public void run() {
            try {
                while (true) {
                    // 每秒打印时间信息
                    long t = System.currentTimeMillis() - startTime;
                    System.out.println(t / 1000 + "." + t % 1000);
                    Thread.sleep(1000);
                }
            } catch (Exception ex) {
                ex.printStackTrace();
            }
        }
    }

    public static void main(String[] args) {
        WorkThread w = new WorkThread();
        PrintThread p = new PrintThread();
        w.start();
        p.start();
    }
}

```





### 3.3.4 垃圾回收的并行与并发

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630093616.png" alt="image-20200630093615852" style="zoom:100%;" />

![image-20200630093805257](G:\图片\blog\image-20200630093805257.png)



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630093852.png" alt="image-20200630093851374" style="zoom:100%;" />

![image-20200630094032491](G:\图片\blog\image-20200630094032491.png)

![image-20200630094242876](G:\图片\blog\image-20200630094242876.png)







### 3.3.5 安全点与安全区域

![image-20200630094514654](G:\图片\blog\image-20200630094514654.png)

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630094856.png" alt="image-20200630094855402" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630095019.png" alt="image-20200630095018560" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630095154.png" alt="image-20200630095153277" style="zoom:100%;" />





### 3.3.6 再谈引用：强引用

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630135351.png" alt="image-20200630135350705" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630140335.png" alt="image-20200630140334450" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630140510.png" alt="image-20200630140509339" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630141148.png" alt="image-20200630141147784" style="zoom:100%;" />

```java
package com.atguigu.java1;

/**
 *  强引用的测试
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  16:05
 */
public class StrongReferenceTest {
    public static void main(String[] args) {
        StringBuffer str = new StringBuffer ("Hello,尚硅谷");
        StringBuffer str1 = str;

        str = null;
        System.gc();

        try {
            Thread.sleep(3000);//延迟一段时间保证GC能够实现
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        System.out.println(str1);//打印结果还是会打印出"Hello,尚硅谷"
    }
}

```

注意：上述代码，堆空间的StringBuffer并没有被回收，因为虽然 **str为null**消除了一份强引用，但是**str1也是一份强引用，**引用了堆空间中的StringBuffer ("Hello,尚硅谷");

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630141812.png" alt="image-20200630141811455" style="zoom:100%;" />

![image-20200630141856204](G:\图片\blog\image-20200630141856204.png)





### 3.3.7 再谈引用：软引用

- 软引用是不会导致OOM内存溢出的
- 软引用是迫不得已清理的
- 内存不够时，会清理软引用可达对象



![image-20200630142217243](G:\图片\blog\image-20200630142217243.png)

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630143857.png" alt="image-20200630142636171" style="zoom:100%;" />



```java
package com.atguigu.java1;

import java.lang.ref.SoftReference;

/**
 * 软引用的测试：内存不足即回收
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  16:06
 */
public class SoftReferenceTest {
    public static class User {
        public User(int id, String name) {
            this.id = id;
            this.name = name;
        }

        public int id;
        public String name;

        @Override
        public String toString() {
            return "[id=" + id + ", name=" + name + "] ";
        }
    }

    public static void main(String[] args) {
        //创建对象，建立软引用
//        SoftReference<User> userSoftRef = new SoftReference<User>(new User(1, "songhk"));
        //上面的一行代码，等价于如下的三行代码
        User u1 = new User(1,"songhk");
        SoftReference<User> userSoftRef = new SoftReference<User>(u1);
        u1 = null;//取消强引用


        //从软引用中重新获得强引用对象
        System.out.println(userSoftRef.get());

        System.gc();
        System.out.println("After GC:");
//        //垃圾回收之后获得软引用中的对象
        System.out.println(userSoftRef.get());//由于堆空间内存足够，所有不会回收软引用的可达对象。
//
        try {
            //让系统认为内存资源紧张、不够
//            byte[] b = new byte[1024 * 1024 * 7];
            byte[] b = new byte[1024 * 7168 - 635 * 1024];
        } catch (Throwable e) {
            e.printStackTrace();
        } finally {
            //再次从软引用中获取数据
            System.out.println(userSoftRef.get());//在报OOM之前，垃圾回收器会回收软引用的可达对象。
        }
    }
}



```

![image-20200630144821993](G:\图片\blog\image-20200630144821993.png)



### 3.3.8 再谈引用：弱引用

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630150253.png" alt="image-20200630150252625" style="zoom:100%;" />

![image-20200630151029168](G:\图片\blog\image-20200630151029168.png)



```java
package com.atguigu.java1;

import java.lang.ref.WeakReference;

/**
 * 弱引用的测试
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  16:06
 */
public class WeakReferenceTest {
    public static class User {
        public User(int id, String name) {
            this.id = id;
            this.name = name;
        }

        public int id;
        public String name;

        @Override
        public String toString() {
            return "[id=" + id + ", name=" + name + "] ";
        }
    }

    public static void main(String[] args) {
        //构造了弱引用
        WeakReference<User> userWeakRef = new WeakReference<User>(new User(1, "songhk"));
        //从弱引用中重新获取对象
        System.out.println(userWeakRef.get());//通过弱引用获取对象，打印[id=1,name=songhk]

        System.gc();
        // 不管当前内存空间足够与否，都会回收它的内存
        System.out.println("After GC:");
        //重新尝试从弱引用中获取对象
        System.out.println(userWeakRef.get());//null
    }
}

```



### 3.3.9 再谈引用：虚引用

![image-20200630151622285](G:\图片\blog\image-20200630151622285.png)

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630152354.png" alt="image-20200630152352832" style="zoom:100%;" />



```java
package com.atguigu.java1;

import java.lang.ref.PhantomReference;
import java.lang.ref.ReferenceQueue;

/**
 * 虚引用的测试
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  16:07
 */
public class PhantomReferenceTest {
    public static PhantomReferenceTest obj;//当前类对象的声明
    static ReferenceQueue<PhantomReferenceTest> phantomQueue = null;//引用队列

    public static class CheckRefQueue extends Thread {
        @Override
        public void run() {
            while (true) {
                if (phantomQueue != null) {
                    PhantomReference<PhantomReferenceTest> objt = null;
                    try {
                        objt = (PhantomReference<PhantomReferenceTest>) phantomQueue.remove();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    if (objt != null) {
                        System.out.println("追踪垃圾回收过程：PhantomReferenceTest实例被GC了");
                    }
                }
            }
        }
    }

    @Override
    protected void finalize() throws Throwable { //finalize()方法只能被调用一次！
        super.finalize();
        System.out.println("调用当前类的finalize()方法");
        obj = this;
    }

    public static void main(String[] args) {
        Thread t = new CheckRefQueue();
        t.setDaemon(true);//设置为守护线程：当程序中没有非守护线程时，守护线程也就执行结束。
        t.start();

        phantomQueue = new ReferenceQueue<PhantomReferenceTest>();
        obj = new PhantomReferenceTest();
        //构造了 PhantomReferenceTest 对象的虚引用，并指定了引用队列
        PhantomReference<PhantomReferenceTest> phantomRef = new PhantomReference<PhantomReferenceTest>(obj, phantomQueue);

        try {
            //不可获取虚引用中的对象
            System.out.println(phantomRef.get());

            //将强引用去除
            obj = null;
            //第一次进行GC,由于对象可复活，GC无法回收该对象
            System.gc();
            Thread.sleep(1000);
            if (obj == null) {
                System.out.println("obj 是 null");
            } else {
                System.out.println("obj 可用");
            }
            System.out.println("第 2 次 gc");
            obj = null;
            System.gc(); //一旦将obj对象回收，就会将此虚引用存放到引用队列中。
            Thread.sleep(1000);
            if (obj == null) {
                System.out.println("obj 是 null");
            } else {
                System.out.println("obj 可用");
            }
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

```



### 3.3.10  再谈引用：终结器引用

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630161252.png" alt="image-20200630161251688" style="zoom:100%;" />





## 四、  垃圾回收器

## 4.1 GC分类与性能指标

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630162323.png" alt="image-20200630162322174" style="zoom:100%;" />

![image-20200630162733748](G:\图片\blog\image-20200630162733748.png)

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701172532.png" alt="image-20200630162933033" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630163032.png" alt="image-20200630163031607" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630163110.png" alt="image-20200630163109990" style="zoom:100%;" />





<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630163307.png" alt="image-20200630163306975" style="zoom:100%;" />



## 4.2 评估GC性能的指标





<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630163343.png" alt="image-20200630163342695" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630163814.png" alt="image-20200630163813433" style="zoom:100%;" />

![image-20200630164012184](G:\图片\blog\image-20200630164012184.png)



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630164301.png" alt="image-20200630164300620" style="zoom:100%;" />

![image-20200630164451072](G:\图片\blog\image-20200630164451072.png)

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630164758.png" alt="image-20200630164757065" style="zoom:100%;" />

![image-20200630165936588](G:\图片\blog\image-20200630165936588.png)





## 4.2 不同的垃圾回收器概述

![image-20200630170128299](G:\图片\blog\image-20200630170128299.png)



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630170212.png" alt="image-20200630170211232" style="zoom:100%;" />

![image-20200630170631158](G:\图片\blog\image-20200630170631158.png)



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630174035.png" alt="image-20200630174034182" style="zoom:100%;" />





- **CMS是第一款 并发 的垃圾回收器。**

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630174314.png" alt="image-20200630174312967" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630174533.png" alt="image-20200630174532307" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630174653.png" alt="image-20200630174652165" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630175005.png" alt="image-20200630175004310" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630175310.png" alt="image-20200630175309299" style="zoom:100%;" />



![image-20200630175610159](G:\图片\blog\image-20200630175610159.png)

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630184235.png" alt="image-20200630184234798" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630184529.png" alt="image-20200630184528279" style="zoom:100%;" />









## 4.3 Serial回收器：串行回收

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630185058.png" alt="image-20200630185057152" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630190139.png" alt="image-20200630190137927" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630190223.png" alt="image-20200630190222334" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630190404.png" alt="image-20200630190403770" style="zoom:100%;" />





## 4.4 ParNew回收器：并行回收

![image-20200630190748655](G:\图片\blog\image-20200630190748655.png)

![image-20200630191014571](G:\图片\blog\image-20200630191014571.png)



![image-20200630191152608](G:\图片\blog\image-20200630191152608.png)

![image-20200630191333484](G:\图片\blog\image-20200630191333484.png)

![image-20200630191641667](G:\图片\blog\image-20200630191641667.png)







## 4.5 Parallel回收器：吞吐量优先

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630191731.png" alt="image-20200630191730997" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630192447.png" alt="image-20200630192446655" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630192501.png" alt="image-20200630192500226" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630192936.png" alt="image-20200630192935584" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630193207.png" alt="image-20200630193206721" style="zoom:100%;" />







<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630192625.png" alt="image-20200630192624403" style="zoom:100%;" />



## 4.6 CMS回收器：低延迟

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630222313.png" alt="image-20200630222312154" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630222720.png" alt="image-20200630222720056" style="zoom:100%;" />

![image-20200630222802455](G:\图片\blog\image-20200630222802455.png)

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630223001.png" alt="image-20200630223000868" style="zoom:100%;" />

![image-20200630223316085](G:\图片\blog\image-20200630223316085.png)

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630223413.png" alt="image-20200630223412103" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630224055.png" alt="image-20200630224054047" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630224524.png" alt="image-20200630224523449" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200630224713.png" alt="image-20200630224712327" style="zoom:100%;" />



浮动垃圾：

-  第一次标记时STW，设某个对象A这时还不是垃圾则不被标记

-  第二次重新标记，与用户线程并行，修正上次标记过的对象。若此时对象A变为垃圾，修正时判断不出。
- 由此，对象A就叫做浮动垃圾。它已经成为新造的垃圾，但是没有被标记。



![image-20200701092333957](G:\图片\blog\image-20200701092333957.png)

```java
package com.atguigu.java;

import java.util.ArrayList;

/**
 *  -XX:+PrintCommandLineFlags
 *
 *  -XX:+UseSerialGC:表明新生代使用Serial GC ，同时老年代使用Serial Old GC
 *
 *  -XX:+UseParNewGC：标明新生代使用ParNew GC
 *
 *  -XX:+UseParallelGC:表明新生代使用Parallel GC
 *  -XX:+UseParallelOldGC : 表明老年代使用 Parallel Old GC
 *  说明：二者可以相互激活
 *
 *  -XX:+UseConcMarkSweepGC：表明老年代使用CMS GC。同时，年轻代会触发对ParNew 的使用
 * @author shkstart  shkstart@126.com
 * @create 2020  0:10
 */
public class GCUseTest {
    public static void main(String[] args) {
        ArrayList<byte[]> list = new ArrayList<>();

        while(true){
            byte[] arr = new byte[100];
            list.add(arr);
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

```

![image-20200701142643467](G:\图片\blog\image-20200701142643467.png)

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701143217.png" alt="image-20200701143216069" style="zoom:100%;" />

![image-20200701143401173](G:\图片\blog\image-20200701143401173.png)



## 4.7 G1回收器：区域化分代式

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701143649.png" alt="image-20200701143648097" style="zoom:100%;" />

![image-20200701144044835](G:\图片\blog\image-20200701144044835.png)

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701144534.png" alt="image-20200701144534046" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701144637.png" alt="image-20200701144637016" style="zoom:100%;" />





<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701145019.png" alt="image-20200701145018980" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701145510.png" alt="image-20200701145157744" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701145500.png" alt="image-20200701145459124" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701145753.png" alt="image-20200701145725846" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701145744.png" alt="image-20200701145743481" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701150412.png" alt="image-20200701150411228" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701150633.png" alt="image-20200701150632410" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701151213.png" alt="image-20200701151212115" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701151642.png" alt="image-20200701151641453" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701151750.png" alt="image-20200701151749126" style="zoom:100%;" />

![image-20200701152050940](G:\图片\blog\image-20200701152050940.png)

![image-20200701152612439](G:\图片\blog\image-20200701152612439.png)

## 4.7 G1回收器：实现过程

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701152728.png" alt="image-20200701152726957" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701153034.png" alt="image-20200701153033379" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701153102.png" alt="image-20200701153101107" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701154750.png" alt="image-20200701154749008" style="zoom:100%;" />







<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701162149.png" alt="image-20200701162148478" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701154333.png" alt="image-20200701154332254" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701162346.png" alt="image-20200701162345658" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701162803.png" alt="image-20200701162802955" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701163037.png" alt="image-20200701163035947" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701163301.png" alt="image-20200701163300018" style="zoom:100%;" />

![image-20200701163927996](G:\图片\blog\image-20200701163927996.png)

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701164331.png" alt="image-20200701164330449" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701165010.png" alt="image-20200701164518497" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701164614.png" alt="image-20200701164613663" style="zoom:100%;" />



## 4.8 垃圾回收器总结

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701164714.png" alt="image-20200701164713114" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701164959.png" alt="image-20200701164855718" style="zoom:100%;" />



![image-20200701165120216](G:\图片\blog\image-20200701165120216.png)







<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701164949.png" alt="image-20200701164948340" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701165244.png" alt="image-20200701165143047" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701165235.png" alt="image-20200701165234961" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701165302.png" alt="image-20200701165258518" style="zoom:100%;" />

## 4.9 GC日志分析

![image-20200701165402662](G:\图片\blog\image-20200701165402662.png)



```java
package com.atguigu.java;

import java.util.ArrayList;

/**
 * -Xms60m -Xmx60m -XX:SurvivorRatio=8 -XX:+PrintGCDetails -Xloggc:./logs/gc.log
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  18:12
 */
public class GCLogTest {
    public static void main(String[] args) {
        ArrayList<byte[]> list = new ArrayList<>();

        for (int i = 0; i < 500; i++) {
            byte[] arr = new byte[1024 * 100];//100KB
            list.add(arr);
            try {
                Thread.sleep(50);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701165650.png" alt="image-20200701165648982" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701165720.png" alt="image-20200701165719534" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701165744.png" alt="image-20200701165743240" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701165757.png" alt="image-20200701165756658" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701165811.png" alt="image-20200701165810411" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701165935.png" alt="image-20200701165934543" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701165958.png" alt="image-20200701165957637" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701172452.png" alt="image-20200701170110716" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701172445.png" alt="image-20200701170133618" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701171336.png" alt="image-20200701170153274" style="zoom:100%;" />



举例：堆空间大小使用情况

```java
package com.atguigu.java;

/**
 * 在jdk7 和 jdk8中分别执行
 * -verbose:gc -Xms20M -Xmx20M -Xmn10M -XX:+PrintGCDetails -XX:SurvivorRatio=8 -XX:+UseSerialGC
 * @author shkstart  shkstart@126.com
 * @create 2020  0:12
 */
public class GCLogTest1 {
    private static final int _1MB = 1024 * 1024;

    public static void testAllocation() {
        byte[] allocation1, allocation2, allocation3, allocation4;
        allocation1 = new byte[2 * _1MB];
        allocation2 = new byte[2 * _1MB];
        allocation3 = new byte[2 * _1MB];
        allocation4 = new byte[4 * _1MB];
    }

    public static void main(String[] agrs) {
        testAllocation();
    }
}

```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701170417.png" alt="image-20200701170416399" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701171322.png" alt="image-20200701170730819" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701171242.png" alt="image-20200701171241621" style="zoom:100%;" />



----



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701172430.png" alt="image-20200701171308286" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701172421.png" alt="image-20200701171408751" style="zoom:100%;" />

在当前工程下创建好目录，然后使用参数设置将GC日志输出成文件，使用工具打开文件。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701172414.png" alt="image-20200701171557481" style="zoom:100%;" />



## 4.10 垃圾回收器的新发展

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701171652.png" alt="image-20200701171651088" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701171743.png" alt="image-20200701171742320" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701172408.png" alt="image-20200701171802147" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701172400.png" alt="image-20200701171828053" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701172349.png" alt="image-20200701171855750" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701172343.png" alt="image-20200701171920346" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701171951.png" alt="image-20200701171950734" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701172024.png" alt="image-20200701172023380" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701172055.png" alt="image-20200701172054364" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701172115.png" alt="image-20200701172114842" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701172204.png" alt="image-20200701172203439" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701172217.png" alt="image-20200701172216216" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701172245.png" alt="image-20200701172244398" style="zoom:100%;" />



# 五、最后寄语



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200701172525.png" alt="image-20200701172524552" style="zoom:100%;" />







