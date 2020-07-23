【JVM】简单总结面试题





# 一、运行时数据区分类

JVM 运行时内存数据区分为： PC程序计数器、虚拟机栈、本地方法栈、堆空间、方法区。



![image-20200721224554074](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200721224555.png)

 ==PC程序计数器==：用来存储指向下一条指令的地址，也即执行引擎要执行的指令代码。由执行引擎读取下一条指令。

==虚拟机栈==：栈是每个线程独有的，与线程的生命周期一致。内部保存着栈帧，代表一次次的方法调用。栈帧中存储着局部变量表、操作数栈、动态链接、方法返回地址、一些附加信息等。

> 动态链接：指向该方法在常量池里的引用

==本地方法栈==：本地方法是非Java的C语言方法，通过栈去访问本地方法库

==堆空间==：几乎所有的对象实例都存储在堆中。JDK 7 中将静态变量和字符串常量池优化到了堆中。堆又分年轻代、老年代。年轻代又分 Eden、survivor0、survival1区。

==方法区==：类型信息、域信息、方法信息、运行时常量池

> 字节码中的常量池：源代码编译后的class字节码指向常量池中的引用





# 二、类加载过程

## 1. 类加载过程

1. Loading 加载

- 获取类的二进制字节流
- 将这个字节流存储结构转化为方法区的运行时数据结构
- 在内存中生成代表这个类的字节码对象

2. Linking 链接

- Verify验证（确保Class字节流中的信息是否符合当前虚拟机要求）
- Prepare准备（为类变量分配内存并且为类变量初始值、不包含 static final 修饰的该修饰在编译器就分配了）

- Resovler解析（将常量池内的符号引用转换为直接引用的过程）

3. initialization初始化

- clinit（初始化类中的静态变量显式赋值、静态代码块）
- init



## 2. 类加载器有哪几种

1. 引导类加载器 Bootstrap ClassLoader

- 用来加载Java的核心库

2. 扩展类加载器 Extension ClassLoader

- 从JDK安装目录的jre/lib/ext子目录下加载类库

3. 系统类加载器 AppClassLoader

- 负责加载环境变量classpath或系统属性 java.class.path 指定下的类库
- 程序默认的类加载器
- 通过ClassLoader#getSystemClassLoader() 方法可以获取该类加载器





## 3. 双亲委派机制

![image-20200706224314538](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706224528.png)







# 三、垃圾回收



## 1. 垃圾回收的算法

1. 标记阶段：引用计数算法
2. 标记阶段：可达性分析算法
3. 清除阶段：标记-清除算法
4. 清除阶段：复制算法
5. 清除阶段：标记-压缩算法
6. 分代收集算法
7. 增量收集算法
8. 分区算法



## 2. 垃圾回收器

1. serial回收器：串行回收
2. ParNew回收器：并行回收
3. parallel回收器：并行回收
4. CMS：低延迟
5. G1：区域化分代式



简述G1

- 设计原则：在满足GC暂停时间的同时，提高吞吐量

- JDK 9中的默认垃圾回收器
- 特点
  - 并行与并发。并行指多个垃圾收集线程可以同时刻工作。并发指在整个垃圾回收过程，用户线程不会完全阻塞。
  - 分代收集。
  - 空间整合。使用复制算法和标记-压缩算法，减少碎片。
  - 优先回收列表。G1会通过算法列出一个回收价值列表，在清除的时候会回收价值最大的 `Region`



![image-20200706225131916](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706231319.png)

![image-20200706225152180](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706231304.png)





## 3. Minor GC和Full GC触发条件

一、Minor GC触发条件
   1、eden区满时，触发MinorGC。即申请一个对象时，发现eden区不够用，则触发一次MinorGC。

   注：新生代分为三个区域，eden space, from space, to space。默认比例是8:1:1。在MinorGC时，会把存活的对象复制到to space区域，如果to space区域不够，则利用担保机制进入老年代区域。

二、Full GC触发条件

1. 老年代空间、永久代空间（JDK 1.7 ）不够分配新的内存





# 四、杂项

## 1. 方法区的存储内容与演变过程

https://www.cnblogs.com/cosmos-wong/p/12925299.html