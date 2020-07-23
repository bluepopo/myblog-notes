@[toc]

---
b站视频地址：
https://www.bilibili.com/video/BV1PJ411n7xZ/?p=2
<iframe src="//player.bilibili.com/player.html?aid=83622425&bvid=BV1PJ411n7xZ&cid=143045190&page=2" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

评论区大佬笔记：
https://www.yuque.com/mo_ming/gl7b70/rfot9k

https://www.cnblogs.com/yanl55555/category/1686360.html

---



# <font color=#B71C0C>一、JVM与Java体系结构</font>

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607110149.png#pic_center" width="70%" />

## 1.前言

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607110208.png#pic_center" width="80%"/>



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607110221.png#pic_center" width="80%"/>

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607110234.png#pic_center" width="80%"/>


## 2.面向人群及参考书目

老师在这里提出了几个问题：
- “栈管运行、堆管存储 ”这句话一定对吗？
- Java中的堆一定是多线程共享的吗？
- Java中的对象一定要创建在堆上吗？
- 方法区中永久带、元空间到底是什么关系？
- Java为什么叫“半解释型、半编译型”语言？



- <img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607110250.png#pic_center" width="80%"/>

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607110258.png#pic_center" width="80%"/>

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607110310.png#pic_center" width="80%"/>



## 3.Java及JVM简介



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607110320.png#pic_center" width="80%"/>
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607110330.png#pic_center" width="80%"/>

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607110339.png#pic_center" width="80%"/>

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607110351.png#pic_center" width="80%"/>



## 4.Java发展重大事件
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607110404.png#pic_center" width="80%"/>

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607110415.png#pic_center" width="80%"/>

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607110526.png#pic_center" width="80%"/>


## 5.虚拟机与Java虚拟机
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607110536.png#pic_center" width="80%"/>

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607110544.png#pic_center" width="80%"/>

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607110552.png#pic_center" width="80%"/>



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607110846.png#pic_center" width="80%"/>

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111007.png#pic_center" width="80%"/>


## 6.JVM整体结构
如下图，其中方法区和堆是多线程共享的，Java栈、本地方法栈、程序计数器是每个线程独有一份的。
执行引擎相当于把字节码文件翻译成机器语言的引擎，使程序可以在操作系统上运行
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200616161427.png#pic_center" width="80%"/>

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111015.png#pic_center" width="80%"/>

## 7.Java代码的执行流程
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111146" alt="在这里插入图片描述" style="zoom:100%;" />
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111155" alt="在这里插入图片描述" style="zoom:100%;" />



## 8.JVM架构模型
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607110957" alt="在这里插入图片描述" style="zoom:100%;" />
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607110948" alt="在这里插入图片描述" style="zoom:100%;" />
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111207" alt="在这里插入图片描述" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111214" alt="在这里插入图片描述" style="zoom:100%;" />
## 9.JVM生命周期
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111224" alt="在这里插入图片描述" style="zoom:100%;" />
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111231" alt="在这里插入图片描述" style="zoom:100%;" />




## 10.JVM发展历程
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111239" alt="在这里插入图片描述" style="zoom:100%;" />

理解执行引擎
解释器的逐行解释特点使得它响应很快，
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111246" alt="在这里插入图片描述" style="zoom:100%;" />
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111255" alt="在这里插入图片描述" style="zoom:100%;" />
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111303" alt="在这里插入图片描述" style="zoom:100%;" />


<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111315" alt="在这里插入图片描述" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111326" alt="在这里插入图片描述" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111339" alt="在这里插入图片描述" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111346" alt="在这里插入图片描述" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111355" alt="在这里插入图片描述" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111437" alt="在这里插入图片描述" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111446" alt="在这里插入图片描述" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111454" alt="在这里插入图片描述" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111504" alt="在这里插入图片描述" style="zoom:100%;" />
<img src="https://img-blog.csdnimg.cn/20200606102238558.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxODY0NjQ4,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111515" alt="在这里插入图片描述" style="zoom:100%;" />

# <font color=#B71C0C>二、类加载子系统</font>


<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607112040" alt="在这里插入图片描述" style="zoom:100%;" />


## 1.概述类加载器及类加载过程
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607112044" alt="在这里插入图片描述" style="zoom:100%;" />
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607112059" alt="在这里插入图片描述" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607113647" alt="在这里插入图片描述" style="zoom:100%;" />
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607113659" alt="在这里插入图片描述" style="zoom:100%;" />
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607113710" alt="在这里插入图片描述" style="zoom:100%;" />

### 1.1 类加载过程一：Loading
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607113722" alt="在这里插入图片描述" style="zoom:100%;" />
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607113729" alt="在这里插入图片描述" style="zoom:100%;" />

### 1.2 类加载过程二：Linking

在准备阶段：
 变量是这个阶段分配值得，但是被final 修饰的static 算是常量了，在编译期就已经分配值了
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607113740" alt="在这里插入图片描述" style="zoom:100%;" />
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607113750" alt="在这里插入图片描述" style="zoom:100%;" />


### 1.3 类加载过程三：Initialization
ps。下图，解释，类变量就是有static修饰的成员变量，所以如果Java程序中没有类变量的显式赋值动作和静态代码块，也没有调用该类的实例的情况，clinit方法是不会出现的
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607113759" alt="在这里插入图片描述" style="zoom:100%;" />
老师的补充：
`clinit 方法`相当于类的构造器函数（与类中的静态变量赋值和静态代码块有关）

- clinit 方法只需要加载一次，加载完以后的类信息就放在**方法区**（在JDK8的时候叫元空间的一个区域，元空间使用的也就是本地内存，也就是说类加载到内存后给缓存起来了，所以后续使用调用这个类时，加载的都是缓存中的那个类本身，因此clinit也就只需要加载一次就OK了）

`init 方法`相当于构造器函数。任何一个类在声明以后，内部至少会存在一个类的构造器，（这个构造器可能是你显示声明的，也可能是我们系统默认提供的），它总是会存在的。


<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607113808" alt="在这里插入图片描述" style="zoom:100%;" />

如下图，可以这样把静态变量的声明写在静态代码块的后面，这是因为在`“Linking”`阶段的`“prepare”`阶段，默认初始化变量为零值，然后在`“Initialization”`中顺序执行`<clinit>`方法中的静态东东，先是执行静态代码块中给number赋值为20，之后再静态变量赋值时number又变成20
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607113816" alt="在这里插入图片描述" style="zoom:100%;" />
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607113826" alt="在这里插入图片描述" style="zoom:100%;" />


<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607113833" alt="在这里插入图片描述" style="zoom:100%;" />


**举个栗子：两个线程加载同一个类**
```java
package com.atguigu.java;

/**
 * @author shkstart
 * @create 2020 上午 11:23
 */
public class DeadThreadTest {
    public static void main(String[] args) {
        Runnable r = () -> {
            System.out.println(Thread.currentThread().getName() + "开始");
            DeadThread dead = new DeadThread();
            System.out.println(Thread.currentThread().getName() + "结束");
        };

        Thread t1 = new Thread(r,"线程1");
        Thread t2 = new Thread(r,"线程2");

        t1.start();
        t2.start();
    }
}

class DeadThread{
    static{
        if(true){
            System.out.println(Thread.currentThread().getName() + "初始化当前类");
            while(true){

            }
        }
    }
}
```
分析上面的代码：

- **验证了一个类只会被加载一次**
- DeadThread类一旦类初始化，执行 clinit 方法，就会进入死循环。并且这个类加载初始化只会加载一次的，所以一旦有一个线程去加载该DeadThread类，就出不来了，之后其他线程再也无法加载这个类了。**（会处于一种加锁的状态）**
- 上面的代码，线程一与线程二只会有一个加载到DeadThread类，打印出static中的语句

执行结果：
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607113846" alt="在这里插入图片描述" style="zoom:100%;" />


## 2. 类加载器的分类
前面讲解了，类加载的过程，这节讲述一下有哪几种类加载。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607113855" alt="在这里插入图片描述" style="zoom:100%;" />
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607113903" alt="在这里插入图片描述" style="zoom:100%;" />
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607113912" alt="在这里插入图片描述" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607113928" alt="在这里插入图片描述" style="zoom:100%;" />



```java
public class ClassLoaderTest {
    public static void main(String[] args) {

        //获取系统类加载器
        ClassLoader systemClassLoader = ClassLoader.getSystemClassLoader();
        System.out.println(systemClassLoader);//sun.misc.Launcher$AppClassLoader@18b4aac2

        //获取其上层：扩展类加载器
        ClassLoader extClassLoader = systemClassLoader.getParent();
        System.out.println(extClassLoader);//sun.misc.Launcher$ExtClassLoader@1540e19d

        //获取其上层：获取不到引导类加载器
        ClassLoader bootstrapClassLoader = extClassLoader.getParent();
        System.out.println(bootstrapClassLoader);//null

        //对于用户自定义类来说：默认使用系统类加载器进行加载
        ClassLoader classLoader = ClassLoaderTest.class.getClassLoader();
        System.out.println(classLoader);//sun.misc.Launcher$AppClassLoader@18b4aac2

        //String类使用引导类加载器进行加载的。---> Java的核心类库都是使用引导类加载器进行加载的。
        ClassLoader classLoader1 = String.class.getClassLoader();
        System.out.println(classLoader1);//null


    }
}
```



### 2.1 引导类加载器Bootstrap ClassLoader
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607113938" alt="在这里插入图片描述" style="zoom:100%;" />

### 2.2 扩展类加载器Extension ClassLoader
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607113947" alt="在这里插入图片描述" style="zoom:100%;" />

### 2.3 系统类加载器 AppClassLoader
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607113956" alt="在这里插入图片描述" style="zoom:100%;" />



```java
public class ClassLoaderTest1 {
    public static void main(String[] args) {
        System.out.println("**********启动类加载器**************");
        //获取BootstrapClassLoader能够加载的api的路径
        URL[] urLs = sun.misc.Launcher.getBootstrapClassPath().getURLs();
        for (URL element : urLs) {
            System.out.println(element.toExternalForm());
        }
        //从上面的路径中随意选择一个类,来看看他的类加载器是什么:引导类加载器
        ClassLoader classLoader = Provider.class.getClassLoader();
        System.out.println(classLoader);

        System.out.println("***********扩展类加载器*************");
        String extDirs = System.getProperty("java.ext.dirs");
        for (String path : extDirs.split(";")) {
            System.out.println(path);
        }

        //从上面的路径中随意选择一个类,来看看他的类加载器是什么:扩展类加载器
        ClassLoader classLoader1 = CurveDB.class.getClassLoader();
        System.out.println(classLoader1);//sun.misc.Launcher$ExtClassLoader@1540e19d

    }
}
```

### 2.4  例子：自定义一个类加载器
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607114006" alt="在这里插入图片描述" style="zoom:100%;" />


<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607114012" alt="在这里插入图片描述" style="zoom:100%;" />



```java
/**
 * 自定义用户类加载器
 * @author shkstart
 * @create 2019 下午 12:21
 */
public class CustomClassLoader extends ClassLoader {
    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {

        try {
            byte[] result = getClassFromCustomPath(name);
            if(result == null){
                throw new FileNotFoundException();
            }else{
                return defineClass(name,result,0,result.length);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        }

        throw new ClassNotFoundException(name);
    }

    private byte[] getClassFromCustomPath(String name){
        //从自定义路径中加载指定类:细节略
        //如果指定路径的字节码文件进行了加密，则需要在此方法中进行解密操作。
        return null;
    }

    public static void main(String[] args) {
        CustomClassLoader customClassLoader = new CustomClassLoader();
        try {
            Class<?> clazz = Class.forName("One",true,customClassLoader);
            Object obj = clazz.newInstance();
            System.out.println(obj.getClass().getClassLoader());
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```

### 2.5 ClassLoader自定义类加载器的使用及方法

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607114027" alt="在这里插入图片描述" style="zoom:100%;" />
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607114039" alt="在这里插入图片描述" style="zoom:100%;" />
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607114051" alt="在这里插入图片描述" style="zoom:100%;" />

由上图可知，扩展类加载器、系统类加载器都是间接的继承自ClassLoader的

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607114102" alt="在这里插入图片描述" style="zoom:100%;" />

```java
/**
 * 几种不同的方式获取类加载器
 */
public class ClassLoaderTest2 {
    public static void main(String[] args) {
        try {
            //1.
            ClassLoader classLoader = Class.forName("java.lang.String").getClassLoader();
            System.out.println(classLoader);
            //2.
            ClassLoader classLoader1 = Thread.currentThread().getContextClassLoader();
            System.out.println(classLoader1);

            //3.
            ClassLoader classLoader2 = ClassLoader.getSystemClassLoader().getParent();
            System.out.println(classLoader2);

        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}

```


### 2.6 双亲委派机制的工作原理
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607114111.png" alt="在这里插入图片描述" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607114418" alt="在这里插入图片描述" style="zoom:100%;" />

解释：双亲委派机制
若自定义了一个类、它所在的包也是自定义的比如com.atguigu.code.Test 加载该类时使用的是AppClassLoader ,但是系统类加载器不会立即加载，而是向上级委托给 Extension ClassLoader，扩展类加载之前讲过是加载 java.etc.dirs 目录下的类，所以不会接受这个委托，于是扩展类加载器继续向上级委托给Bootstrap ClassLoader，引导类加载器是只会加载 java javax sun开头的目录，也不会接受这个委托。

上级都没有一个接受委托的，不帮忙加载这个自定义类，那么这个委托只好向下走最后还回到本身的AppClassLoader，使用系统类加载器加载该类


<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607114449" alt="在这里插入图片描述" style="zoom:100%;" />

### 2.7 双亲委派机制举例

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607114458" alt="在这里插入图片描述" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607114508" alt="在这里插入图片描述" style="zoom:100%;" />

出于安全考虑，禁止自定义的类以java.lang包命名
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607114516" alt="在这里插入图片描述" style="zoom:100%;" />

### 2.8 沙箱安全机制
问题：
分析上面的代码和截图，为什么我们自定义shkStart类就会禁止访问java.lang包，而自定义String类就没有报错？
解释：
 加载的String类并非我们自定义的String类，而是引导类加载器加载的核心库中的String类。
 实质上我们还是不可以自己定义一个类放进java.lang包里面 企图用引导类加载器帮我们加载，这种操作是不安全的也是不允许的，这是出于对类加载器的一种保护机制-----------沙箱安全机制
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607114525" alt="在这里插入图片描述" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200608150819.png" alt="image-20200608150644386" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200608151137.png" alt="image-20200608151137808" style="zoom:100%;" />







### 2.9 类的主动使用与被动使用

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200608151641.png" alt="image-20200608151450450" style="zoom:100%;" />



-----



# 三、运行时数据区及线程概述 



运行时内存数据区如图：

红色的方法区、堆 是线程共享的

JDK 1.8 之后方法区换成了元空间，也就是本地缓存。它是 **堆外内存**  （又称永久带或元空间）

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200608152146.png" alt="image-20200608152146850" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200608152811.png" alt="image-20200608152811159" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200608153804.png" alt="image-20200608153348333" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200608153816.png" alt="image-20200608153740853" style="zoom:100%;" />





从虚拟机的角度看线程

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200608154924.png" alt="image-20200608154924810" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200608155422.png" alt="image-20200608155422598" style="zoom:100%;" />





# 四、 程序计数器

## 4.1 PC 概述

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200608170349.png" alt="image-20200608170349661" style="zoom:100%;" />

思考一个问题：Java虚拟机中存不存在寄存器结构呢？

- 存在。不过这里的寄存器只是一种对物理寄存器的抽象模拟。

## 4.2 PC作用

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200608171947.png" alt="image-20200608171946980" style="zoom:100%;" />



## 4.3 PC 详细介绍

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200608172251.png" alt="image-20200608172251264" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200608172839.png" alt="image-20200608172839124" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200608185141.png" alt="image-20200608185141567" style="zoom:100%;" />

如上图的反编译结构，圈出来的左边那列就是PC寄存器中存放的指令的偏移地址，右边就是对于的指令



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200608185948.png" alt="image-20200608185948649" style="zoom:100%;" />





## 4.4 PC计数器相关面试题



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200608190427.png" alt="image-20200608190218562" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200608190418.png" alt="image-20200608190418421" style="zoom:100%;" />





# 五、 虚拟机栈

### 5.1 虚拟机栈的概述

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200608192122.png" alt="image-20200608192122875" style="zoom:100%;" />

很多程序员都会粗略的将JVM中的内存区理解为栈和堆，（其实细分是有很多东西的）这是为什么？

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200609114920.png" alt="image-20200609114915662" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200609115428.png" alt="image-20200609115425471" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200609115931.png" alt="image-20200609115929551" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200609120448.png" alt="image-20200609120446108" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200609120753.png" alt="image-20200609120750968" style="zoom:100%;" />



手动设置栈的内存大小

![image-20200609121802564](G:\图片\blog\image-20200609121802564.png)

### 5.2 栈的存储单位

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200609122043.png" alt="image-20200609122042664" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200610110441.png" alt="image-20200610110432217" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200610111613.png" alt="image-20200610111611593" style="zoom:100%;" />





### 5.3 （栈帧）内部结构

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200610162421.png" alt="image-20200610162420112" style="zoom:100%;" />



### 5.4 （栈帧）局部变量表

每个线程都有各自的虚拟机栈、一个栈中有多个栈帧、每一个栈帧中都有各自的局部变量表

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200610164540.png" alt="image-20200610164538814" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200610171207.png" alt="image-20200610171206208" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200610174704.png" alt="image-20200610174702689" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200610174739.png" alt="image-20200610174738494" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200610180217.png" alt="image-20200610175319555" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200610175812.png" alt="image-20200610175810665" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200610180206.png" alt="image-20200610175958804" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200610184848.png" alt="image-20200610181340500" style="zoom:100%;" />



### 5.5 （栈帧）操作数栈

回顾面试题第一季中的 求和计算操作的详细步骤

在哪里存放变量，在哪里运算，执行赋值时把结果返回到局部变量表

```java
int i = 1 ;

i = i ++;

int j = i ++;

int k  = i + ++i +i++;
```



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200610185140.png" alt="image-20200610185035977" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200610185745.png" alt="image-20200610185744769" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200610185947.png" alt="image-20200610185945746" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200610190218.png" alt="image-20200610190217512" style="zoom:100%;" />





### 5.6 代码追踪

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200610190901.png" alt="image-20200610190859657" style="zoom:100%;" />



bipush  15 ：将常量值15压入操作数栈中

istore_1 ： 将操作数栈Pop数据，存储到局部变量表，成为变量1

iload_1：向操作数栈中压入变量1

iload_2：向操作数栈中压入变量

iadd ：执行引擎执行加操作

istore_3 ：将相加结果存储到局部变量表，成为变量3

return ：（如果方法带有返回值的话，其返回值结果会被压入当前栈帧的操作数栈当中，方法结束，并更新PC寄存器中下一条需要执行的的字节码指令）

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200610190924.png" alt="image-20200610190922969" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200610192220.png" alt="image-20200610192137933" style="zoom:100%;" />

### 5.7 栈顶缓存技术

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200610224902.png" alt="image-20200610224342977" style="zoom:100%;" />



### 5.8 （栈帧）动态链接

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200610224853.png" alt="image-20200610224851673" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200610231210.png" alt="image-20200610231209570" style="zoom:100%;" />

动态链接比喻：

当你需要调用一个方法时，使用一个常量池中已经存在的方法引用拿过来直接放在自己兜里会更加高效率。如上图，若多个栈帧都有调用同一个方法的话，那使用动态链接就很高效。



关于常量池，我们为什么需要运行时常量池呢？

常量池的作用，就是为了提供一些符号和常量，便于指令的识别。



### 5.9 方法的调用：虚方法与动态静态调用

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200611162830.png" alt="image-20200611162822278" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200611163256.png" alt="image-20200611163255344" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200611165018.png" alt="image-20200611164732014" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200611165837.png" alt="image-20200611165651720" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200616161449.png" alt="image-20200611180306980" style="zoom:100%;" />

注意：

调用`final`修饰的方法时使用的是`invokvirtual`，但是`final`修饰的方法是**非虚方法。**



```java
package com.atguigu.java2;

/**
 * 解析调用中非虚方法、虚方法的测试
 *
 * invokestatic指令和invokespecial指令调用的方法称为非虚方法
 * @author shkstart
 * @create 2020 下午 12:07
 */
class Father {
    public Father() {
        System.out.println("father的构造器");
    }

    public static void showStatic(String str) {
        System.out.println("father " + str);
    }

    public final void showFinal() {
        System.out.println("father show final");
    }

    public void showCommon() {
        System.out.println("father 普通方法");
    }
}

public class Son extends Father {
    public Son() {
        //invokespecial
        super();
    }
    public Son(int age) {
        //invokespecial
        this();
    }
    //不是重写的父类的静态方法，因为静态方法不能被重写！
    public static void showStatic(String str) {
        System.out.println("son " + str);
    }
    private void showPrivate(String str) {
        System.out.println("son private" + str);
    }

    public void show() {
        //invokestatic
        showStatic("atguigu.com");
        //invokestatic
        super.showStatic("good!");
        //invokespecial
        showPrivate("hello!");
        //invokespecial
        super.showCommon();//调用父类方法

        //invokevirtual
        showFinal();//因为此方法声明有final，不能被子类重写，所以也认为此方法是非虚方法。
        //虚方法如下：
        //invokevirtual
        showCommon();
        info();

        MethodInterface in = null;
        //invokeinterface
        in.methodA();
    }

    public void info(){

    }

    public void display(Father f){
        f.showCommon();
    }

    public static void main(String[] args) {
        Son so = new Son();
        so.show();
    }
}

interface MethodInterface{
    void methodA();
}

```



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200611182743.png" alt="image-20200611182741135" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200611183731.png" alt="image-20200611183730068" style="zoom:100%;" />





**方法重写的本质与虚方法表的使用**

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200611184039.png" alt="image-20200611184038187" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200611184842.png" alt="image-20200611184841007" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200611185440.png" alt="image-20200611185439452" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200611190356.png" alt="image-20200611190334970" style="zoom:100%;" />

### 5.10 （栈帧）方法返回地址

总结：方法返回地址 返回的就是 PC寄存器的值`(即下一条指令的地址)`

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200615230502.png" alt="image-20200615230150319" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200615230450.png" alt="image-20200615230445861" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200615230815.png" alt="image-20200615230814244" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200615231221.png" alt="image-20200615231219603" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200615231414.png" alt="image-20200615231413547" style="zoom:100%;" />





### 5.11 栈的相关面试题

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200616161103.png" alt="image-20200616154804038" style="zoom:100%;" />

```java
package com.atguigu.java3;

/**
 * 面试题：
 * 方法中定义的局部变量是否线程安全？具体情况具体分析
 *
 *   何为线程安全？
 *      如果只有一个线程才可以操作此数据，则必是线程安全的。
 *      如果有多个线程操作此数据，则此数据是共享数据。如果不考虑同步机制的话，会存在线程安全问题。
 * @author shkstart
 * @create 2020 下午 7:48
 */
public class StringBuilderTest {

    int num = 10;

    //s1的声明方式是线程安全的
    public static void method1(){
        //StringBuilder:线程不安全
        StringBuilder s1 = new StringBuilder();
        s1.append("a");
        s1.append("b");
        //...s1在方法内部消亡
    }
    //sBuilder的操作过程：是线程不安全的
    public static void method2(StringBuilder sBuilder){
        sBuilder.append("a");
        sBuilder.append("b");
        //...
    }
    //s1的操作：是线程不安全的
    public static StringBuilder method3(){
        StringBuilder s1 = new StringBuilder();
        s1.append("a");
        s1.append("b");
        return s1;//作为返回值返回出去，可能会有多个线程得到这个返回值
    }
    //s1的操作：是线程安全的
    public static String method4(){
        StringBuilder s1 = new StringBuilder();
        s1.append("a");
        s1.append("b");
        return s1.toString();//转换成立String对象，s1本身就消亡了
    }

    public static void main(String[] args) {
        StringBuilder s = new StringBuilder();


        new Thread(() -> {
            s.append("a");
            s.append("b");
        }).start();

        method2(s);

    }

}

```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200616165743.png" alt="image-20200616165741855" style="zoom:100%;" />

# 六、 本地方法接口

回顾一下JVM的整体架构图，找到本地方法接口的位置

注意：这里本地方法接口不在运行时数据区，我们讲完这里，再回归运行时数据区中的本地方法栈的讲解。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200607111015.png#pic_center" width="80%"/>

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200616162555.png" alt="image-20200616162554691" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200616162524.png" alt="image-20200616162521826" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200616162703.png" alt="image-20200616162700940" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200616163932.png" alt="image-20200616163607562" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200616163942.png" alt="image-20200616163929897" style="zoom:100%;" />





# 七、 本地方法栈

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200616164531.png" alt="image-20200616164224090" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200616165018.png" alt="image-20200616164652274" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200616201310.png" alt="image-20200616165209611" style="zoom:100%;" />



#  八、 堆

## 8.1 堆的概念及内部结构

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200616213121.png" alt="image-20200616213119806" style="zoom:100%;" />





<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617124606.png" alt="image-20200617124557560" style="zoom:100%;" />



`数组和对象可能永远不会存储在栈上，那么虚拟栈中的局部变量表存储的是一些基本类型的变量和对象的引用。`

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617130104.png" alt="image-20200617125202355" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617125359.png" alt="image-20200617125357733" style="zoom:100%;" />



如上图所示，当main方法结束后，栈帧从虚拟机栈出栈，对用的实例对象S1、s2在堆中就会被认为是垃圾了，等到垃圾回收阶段由GC处理。



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617130054.png" alt="image-20200617130053309" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617170413.png" alt="image-20200617130432786" style="zoom:100%;" />







<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617170403.png" alt="image-20200617130828522" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617170355.png" alt="image-20200617130923380" style="zoom:100%;" />



提问：JDK8中主要有哪些变化？

- 堆空间中的永久代----->元空间

- 字符串常量池、静态域也会发生了变化，这里的事后面会讲解

## 8.2 堆大小与OOM

一、堆的大小的参数设置



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617170347.png" alt="image-20200617170346399" style="zoom:100%;" />



```java
package com.atguigu.java;

/**
 * 1. 设置堆空间大小的参数
 * -Xms 用来设置堆空间（年轻代+老年代）的初始内存大小
 *      -X 是jvm的运行参数
 *      ms 是memory start
 * -Xmx 用来设置堆空间（年轻代+老年代）的最大内存大小
 *
 * 2. 默认堆空间的大小
 *    初始内存大小：物理电脑内存大小 / 64
 *             最大内存大小：物理电脑内存大小 / 4
 * 3. 手动设置：-Xms600m -Xmx600m
 *     开发中建议将初始堆内存和最大的堆内存设置成相同的值。
 *
 * 4. 查看设置的参数：方式一： jps   /  jstat -gc 进程id
 *                  方式二：-XX:+PrintGCDetails
 * @author shkstart  shkstart@126.com
 * @create 2020  20:15
 */
public class HeapSpaceInitial {
    public static void main(String[] args) {

        //返回Java虚拟机中的堆内存总量
        long initialMemory = Runtime.getRuntime().totalMemory() / 1024 / 1024;
        //返回Java虚拟机试图使用的最大堆内存量
        long maxMemory = Runtime.getRuntime().maxMemory() / 1024 / 1024;

        System.out.println("-Xms : " + initialMemory + "M");
        System.out.println("-Xmx : " + maxMemory + "M");

//        System.out.println("系统内存大小为：" + initialMemory * 64.0 / 1024 + "G");
//        System.out.println("系统内存大小为：" + maxMemory * 4.0 / 1024 + "G");

        try {
            Thread.sleep(1000000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

```



注意：堆内存的计算，在年轻代的survival0 survival1区，只会二选一，所以计算方式是 一个伊甸园区+一个survival区+一个老年区 

测试代码：设置 -Xms600m -Xmx600m

输出结果：

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617165753.png" alt="image-20200617165751642" style="zoom:100%;" />





<font color=red>**二、OOM：OutOfMemory错误举例**</font>font>❌



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617170504.png" alt="image-20200617170502653" style="zoom:100%;" />



```java
package com.atguigu.java;

import java.util.ArrayList;
import java.util.Random;

/**
 * -Xms600m -Xmx600m
 * @author shkstart  shkstart@126.com
 * @create 2020  21:12
 */
public class OOMTest {
    public static void main(String[] args) {
        ArrayList<Picture> list = new ArrayList<>();
        while(true){
            try {
                Thread.sleep(20);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            list.add(new Picture(new Random().nextInt(1024 * 1024)));
        }
    }
}

class Picture{
    private byte[] pixels;

    public Picture(int length) {
        this.pixels = new byte[length];
    }
}

```







## 8.3 年轻代与老年代

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617171439.png" alt="image-20200617171435101" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617171847.png" alt="image-20200617171846769" style="zoom:100%;" />

```java
/**

 * -Xms600m -Xmx600m
 *
 * -XX:NewRatio ： 设置新生代与老年代的比例。默认值是2.
 * -XX:SurvivorRatio ：设置新生代中Eden区与Survivor区的比例。默认值是8
 * -XX:-UseAdaptiveSizePolicy ：关闭自适应的内存分配策略  （暂时用不到）
 * -Xmn:设置新生代的空间的大小。 （一般不设置）
 *
 */
```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617172312.png" alt="image-20200617172311001" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617172341.png" alt="image-20200617172340726" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617175436.png" alt="image-20200617173047753" style="zoom:100%;" />

## 8.4 图解对象分配过程

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617224956.png" alt="image-20200617224955465" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617180432.png" alt="image-20200617180431420" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617224214.png" alt="image-20200617224213769" style="zoom:100%;" />

注意：

- 如图，当伊甸园区满了的时候会触发一次垃圾回收，这样的垃圾回收比如 **YGC/Minor GC**，它会回收伊甸园区和survival区的垃圾，即便survival区没满也会跟着一起回收。

**- 那么如果survival区满了咋办？**其实也可以跳级进入老年区，**也就是survival区的对象还没到达阈值15可以直接跳到老年区。**



- 这样一看，有的对象一出生就在老年区了噢。不一定全部都百分百在伊甸园出生。
- 垃圾回收最频繁的其实就是新生代，所以我们说大多数对象都是**朝生夕死的。**



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617225844.png" alt="image-20200617225843016" style="zoom:100%;" />



**对象分配的特殊情况**

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617231339.png" alt="image-20200617230214047" style="zoom:100%;" />

```java
package com.atguigu.java1;

import java.util.ArrayList;
import java.util.Random;

/**
 * -Xms600m -Xmx600m
 * @author shkstart  shkstart@126.com
 * @create 2020  17:51
 */
public class HeapInstanceTest {
    byte[] buffer = new byte[new Random().nextInt(1024 * 200)];

    public static void main(String[] args) {
        ArrayList<HeapInstanceTest> list = new ArrayList<HeapInstanceTest>();
        while (true) {
            list.add(new HeapInstanceTest());
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617231326.png" alt="image-20200617231325375" style="zoom:100%;" />







## 8.5 Minor GC、Major GC、Full GC

![image-20200617232358975](G:\图片\blog\image-20200617232358975.png)



![image-20200617233013208](G:\图片\blog\image-20200617233013208.png)

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617233359.png" alt="image-20200617233335240](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617234704.png)![image-20200617233358240" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617233532.png" alt="image-20200617233531492" style="zoom:100%;" />



测试代码：

编译完代码后设置参数 -Xms9m -Xms9m  -XX:+PrintGCDetails

```java
package com.atguigu.java1;

import java.util.ArrayList;
import java.util.List;

/**
 * 测试MinorGC 、 MajorGC、FullGC
 * -Xms9m -Xmx9m -XX:+PrintGCDetails
 * @author shkstart  shkstart@126.com
 * @create 2020  14:19
 */
public class GCTest {
    public static void main(String[] args) {
        int i = 0;
        try {
            List<String> list = new ArrayList<>();
            String a = "atguigu.com";
            while (true) {
                list.add(a);
                a = a + a;
                i++;
            }

        } catch (Throwable t) {
            t.printStackTrace();
            System.out.println("遍历次数为：" + i);
        }
    }
}

```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200617234656.png" alt="image-20200617234655007" style="zoom:100%;" />









## 8.6 堆空间分代思想

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200618143832.png" alt="image-20200618143831745" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200618143955.png" alt="image-20200618143954456" style="zoom:100%;" />





## 8.7 内存分配策略

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200618144322.png" alt="image-20200618144321482" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200619002315.png" alt="image-20200618215943169" style="zoom:100%;" />





## 8.8 为对象分配内存 TLAB



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200619002402.png" alt="image-20200618221140391" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200619002420.png" alt="image-20200618221155098" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200619002540.png" alt="image-20200618223845260" style="zoom:100%;" />





![image-20200618221511255](G:\图片\blog\image-20200618221511255.png)

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200619002612.png" alt="image-20200618223506414" style="zoom:100%;" />



## 8.9 （小结）堆空间的参数设置

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200619002602.png" alt="image-20200618234509665" style="zoom:100%;" />

![image-20200618234538372](G:\图片\blog\image-20200618234538372.png)



```java
package com.atguigu.java1;

/**
 * 测试堆空间常用的jvm参数：
 * -XX:+PrintFlagsInitial : 查看所有的参数的默认初始值
 * -XX:+PrintFlagsFinal  ：查看所有的参数的最终值（可能会存在修改，不再是初始值）
 *      具体查看某个参数的指令： jps：查看当前运行中的进程
 *                             jinfo -flag SurvivorRatio 进程id
 *
 * -Xms：初始堆空间内存 （默认为物理内存的1/64）
 * -Xmx：最大堆空间内存（默认为物理内存的1/4）
 * -Xmn：设置新生代的大小。(初始值及最大值)
 * -XX:NewRatio：配置新生代与老年代在堆结构的占比
 * -XX:SurvivorRatio：设置新生代中Eden和S0/S1空间的比例
 * -XX:MaxTenuringThreshold：设置新生代垃圾的最大年龄
 * -XX:+PrintGCDetails：输出详细的GC处理日志
 * 打印gc简要信息：① -XX:+PrintGC   ② -verbose:gc
 * -XX:HandlePromotionFailure：是否设置空间分配担保
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  17:18
 */
public class HeapArgsTest {
    public static void main(String[] args) {

    }
}

```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200619002620.png" alt="image-20200618235538774" style="zoom:100%;" />

## 8.10 堆是分配对象的唯一选择吗？

答案：并不是噢！



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200619002627.png" alt="image-20200619000257080" style="zoom:100%;" />

## 8.11 逃逸分析：概述

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200619002640.png" alt="image-20200619000645002" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200619002648.png" alt="image-20200619000717418" style="zoom:100%;" />

为啥是没有发生逃逸的对象，可以分配到栈上呢？

- 因为栈是每个线程一份，栈中的栈帧就是该线程的一个方法，如果这个new出的对象没有逃逸出这个方法，会随着方法的开始而创建，随着方法的结束而消亡，那这个对象是和这个栈帧同生共死的。可以优化到栈上。
- 方法结束后，栈帧自动出栈，释放空间，所以也就不用GC了



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200619002658.png" alt="image-20200619001256300" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200619173616.png" alt="image-20200619001314834" style="zoom:100%;" />



```java
package com.atguigu.java2;

/**
 * 逃逸分析
 *
 *  如何快速的判断是否发生了逃逸分析，大家就看new的对象实体是否有可能在方法外被调用。
 * @author shkstart
 * @create 2020 下午 4:00
 */
public class EscapeAnalysis {

    public EscapeAnalysis obj;

    /*
    方法返回EscapeAnalysis对象，发生逃逸
     */
    public EscapeAnalysis getInstance(){
        return obj == null? new EscapeAnalysis() : obj;
    }
    /*
    为成员属性赋值，发生逃逸
     */
    public void setObj(){
        this.obj = new EscapeAnalysis();
    }
    //思考：如果当前的obj引用声明为static的？仍然会发生逃逸。

    /*
    对象的作用域仅在当前方法中有效，没有发生逃逸
     */
    public void useEscapeAnalysis(){
        EscapeAnalysis e = new EscapeAnalysis();
    }
    /*
    引用成员变量的值，发生逃逸
     */
    public void useEscapeAnalysis1(){
        EscapeAnalysis e = getInstance();
        //getInstance().xxx()同样会发生逃逸
    }
}

```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120146.png" alt="image-20200619001753453" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120153.png" alt="image-20200619001852645" style="zoom:100%;" />



## 8.12 逃逸分析：栈上分配

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120200.png" alt="image-20200619001908148" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120208.png" alt="image-20200619165132980" style="zoom:100%;" />

 



代码测试：

```java
package com.atguigu.java2;

/**
 * 栈上分配测试
 * -Xmx1G -Xms1G -XX:-DoEscapeAnalysis -XX:+PrintGCDetails
 * @author shkstart  shkstart@126.com
 * @create 2020  10:31
 */
public class StackAllocation {
    public static void main(String[] args) {
        long start = System.currentTimeMillis();

        for (int i = 0; i < 10000000; i++) {
            alloc();
        }
        // 查看执行时间
        long end = System.currentTimeMillis();
        System.out.println("花费的时间为： " + (end - start) + " ms");
        // 为了方便查看堆内存中对象个数，线程sleep
        try {
            Thread.sleep(1000000);
        } catch (InterruptedException e1) {
            e1.printStackTrace();
        }
    }

    private static void alloc() {
        User user = new User();//未发生逃逸
    }

    static class User {

    }
}

```





<font color=blue>设置 -xms256m -xmx256m  -XX：-printescapeanalysis  -XX：+printGCdetails</font>>

解析参数设置：减少堆空间的大小（容易发生GC）、不使用逃逸分析

结果：如下图，执行时间较长，对象都在堆空间创建 且堆空间发生了垃圾回收，造成运行效率变低

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120215.png" alt="image-20200619172428318" style="zoom:100%;" />





<font color=blue>设置 -xms256m -xmx256m  -XX：+printescapeanalysis  -XX：+printGCdetails</font>>

解析参数设置：减少堆空间大小，使用了逃逸分析，打印GC细节

结果：如下图，执行时间明显变短，没有出现GC，程序方法中的某些变量采用了逃逸分析分配到了栈上

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120225.png" alt="image-20200619172853127" style="zoom:100%;" />



## 8.13 逃逸分析：同步省略

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120230.png" alt="image-20200619173542605" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120236.png" alt="image-20200619174254437" style="zoom:100%;" />















## 8.14 逃逸分析：标量替换

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120244.png" alt="image-20200619174643146" style="zoom:100%;" />



**标量替换的规则：**

**如果方法中的变量没有逃逸，也就是可以进行栈上分配，那么怎么给这个变量对象或者数组类型进行栈上分配呢？？毕竟我们栈上存放的都是基本数据类型，所以这里就需要把 “聚合量”拆解，拆解成标量，然后进行栈上分配。**







<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120251.png" alt="image-20200619212028461" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120259.png" alt="image-20200619212416595" style="zoom:100%;" />



```java
package com.atguigu.java2;

/**
 * 标量替换测试
 *  -Xmx100m -Xms100m -XX:+DoEscapeAnalysis -XX:+PrintGC -XX:-EliminateAllocations
 * @author shkstart  shkstart@126.com
 * @create 2020  12:01
 */
public class ScalarReplace {
    public static class User {
        public int id;
        public String name;
    }

    public static void alloc() {
        User u = new User();//未发生逃逸
        u.id = 5;
        u.name = "www.atguigu.com";
    }

    public static void main(String[] args) {
        long start = System.currentTimeMillis();
        for (int i = 0; i < 10000000; i++) {
            alloc();
        }
        long end = System.currentTimeMillis();
        System.out.println("花费的时间为： " + (end - start) + " ms");
    }
}

/*
class Customer{
    String name;
    int id;
    Account acct;

}

class Account{
    double balance;
}


 */

```









<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120304.png" alt="image-20200619213045721" style="zoom:100%;" />





![image-20200619213228805](G:\图片\blog\image-20200619213228805.png)



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120312.png" alt="image-20200619213634958" style="zoom:100%;" />





#  九、方法区

## 9.1 栈、堆、方法区的交互关系

上一节我们提到了堆中字符串常量和静态变量也是在堆中创建的，曾经是在堆中的永久代，后来叫做元空间，的地方创建。那么现在这个永久带（元空间）哪去了呢？

方法区又是什么，方法区是干啥的？能帮助我们什么？请带着疑问看下去。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120319.png" alt="image-20200619213800381" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120331.png" alt="image-20200619232127749" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120325.png" alt="image-20200619232359462" style="zoom:100%;" />



## 9.2 方法区的理解



内存上独立于堆，所以在设置堆空间大小的参数时，对方法区的空间大小是没有影响的。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120339.png" alt="image-20200619233334988" style="zoom:100%;" />



```java
package com.atguigu.java;

/**
 *  测试设置方法区大小参数的默认值
 *
 *  jdk7及以前：
 *  -XX:PermSize=100m -XX:MaxPermSize=100m
 *
 *  jdk8及以后：
 *  -XX:MetaspaceSize=100m  -XX:MaxMetaspaceSize=100m
 * @author shkstart  shkstart@126.com
 * @create 2020  12:16
 */
public class MethodAreaDemo {
    public static void main(String[] args) {
        System.out.println("start...");
//        try {
//            Thread.sleep(1000000);
//        } catch (InterruptedException e) {
//            e.printStackTrace();
//        }

        System.out.println("end...");
    }
}

```



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120345.png" alt="image-20200619234341429" style="zoom:100%;" />



## 9.3 设置方法区的大小与OOM

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120350.png" alt="image-20200621114659204" style="zoom:100%;" />





<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120357.png" alt="image-20200621115200643" style="zoom:100%;" />

```java
package com.atguigu.java;

/**
 *  测试设置方法区大小参数的默认值
 *
 *  jdk7及以前：
 *  -XX:PermSize=100m -XX:MaxPermSize=100m
 *
 *  jdk8及以后：
 *  -XX:MetaspaceSize=100m  -XX:MaxMetaspaceSize=100m
 * @author shkstart  shkstart@126.com
 * @create 2020  12:16
 */
public class MethodAreaDemo {
    public static void main(String[] args) {
        System.out.println("start...");
//        try {
//            Thread.sleep(1000000);
//        } catch (InterruptedException e) {
//            e.printStackTrace();
//        }

        System.out.println("end...");
    }
}

```



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120405.png" alt="image-20200621120110661" style="zoom:100%;" />



```java
package com.atguigu.java;

import com.sun.xml.internal.ws.org.objectweb.asm.ClassWriter;
import jdk.internal.org.objectweb.asm.Opcodes;

/**
 * jdk6/7中：
 * -XX:PermSize=10m -XX:MaxPermSize=10m
 *
 * jdk8中：
 * -XX:MetaspaceSize=10m -XX:MaxMetaspaceSize=10m
 *
 * @author shkstart  shkstart@126.com
 * @create 2020  22:24
 */
public class OOMTest extends ClassLoader {
    public static void main(String[] args) {
        int j = 0;
        try {
            OOMTest test = new OOMTest();
            for (int i = 0; i < 10000; i++) {
                //创建ClassWriter对象，用于生成类的二进制字节码
                ClassWriter classWriter = new ClassWriter(0);
                //指明版本号，修饰符，类名，包名，父类，接口
                classWriter.visit(Opcodes.V1_6, Opcodes.ACC_PUBLIC, "Class" + i, null, "java/lang/Object", null);
                //返回byte[]
                byte[] code = classWriter.toByteArray();
                //类的加载
                test.defineClass("Class" + i, code, 0, code.length);//Class对象
                j++;
            }
        } finally {
            System.out.println(j);
        }
    }
}

```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621121554.png" alt="image-20200621121552256" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621122035.png" alt="image-20200621121610961" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621122206.png" alt="image-20200621122205729" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621122449.png" alt="image-20200621122448091" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621122808.png" alt="image-20200621122806938" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621122828.png" alt="image-20200621122827325" style="zoom:100%;" />













## 9.4 方法区的内部结构

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621122026.png" alt="image-20200621122025247" style="zoom:100%;" />





<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621123816.png" alt="image-20200621123815613" style="zoom:100%;" />



如上图，要记住的是，在类加载阶段后生成的类型信息.class文件放到方法区，同时还存放加载该类需要的类加载器的信息。



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621124627.png" alt="image-20200621124624016" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621124902.png" alt="image-20200621124900149" style="zoom:100%;" />

声明了` final `的类变量，在编译的时候就初始化了。如下图所示 53行的`number`变量在**编译时**就初始化了值为2，而没有加 `final `修饰的` count `变量在编译阶段并没有任何初始化值。



还记得以前讲过的`编译--->链接--->初始化`的类加载阶段吗？复盘一下，普通的类变量在加载阶段是在 链接中的 `prepare （准备）`阶段初始化赋默认零值，在 初始化 `initialization `阶段才赋值程序员给的初始值。





![image-20200621125129085](G:\图片\blog\image-20200621125129085.png)





## 9.5 运行时常量池



<font color=#0067B0>**class字节码文件中的常量池**</font>





<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200622000227.png" alt="image-20200622000225770" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200622000511.png" alt="image-20200622000510859" style="zoom:100%;" />







<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200622002056.png" alt="image-20200622000903411" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200622002038.png" alt="image-20200622001014516" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200622002026.png" alt="image-20200622002025766" style="zoom:100%;" />



![image-20200622002225340](G:\图片\blog\image-20200622002225340.png)





<font color=#0067B0>**运行时常量池**</font>

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623143542.png" alt="image-20200623143534370" style="zoom:100%;" />



## 9.5 方法区使用举例

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623145237.png" alt="image-20200623145233677" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623145543.png" alt="image-20200623145542152" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623145613.png" alt="image-20200623145612156" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623145736.png" alt="image-20200623145735502" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623145831.png" alt="image-20200623145830787" style="zoom:100%;" />



![image-20200623145934445](G:\图片\blog\image-20200623145934445.png)

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623151344.png" alt="image-20200623151340745" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623151523.png" alt="image-20200623151522506" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623151644.png" alt="image-20200623151643338" style="zoom:100%;" />

![image-20200623151949825](G:\图片\blog\image-20200623151949825.png)



![image-20200623152258778](G:\图片\blog\image-20200623152258778.png)







## 9.6 方法区的演变过程

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120412.png" alt="image-20200619234414007" style="zoom:100%;" />



问题：为什么JDK7这样设置永久代的模式更容易OOM呢？

- hotspot在 JDK7之前习惯上叫永久代，而永久代使用的还是JVM运行时的内存空间，这样就给JVM造成了一定的开销嘛，东西存的一多就更容易OOM









<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120418.png" alt="image-20200619234915713" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200621120424.png" alt="image-20200619234943521" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623152630.png" alt="image-20200623152628800" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623153335.png" alt="image-20200623153334761" style="zoom:100%;" />



![image-20200723132641569](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200723132642.png)



![image-20200723132718826](G:\图片\blog\image-20200723132718826.png)



### <font color=blue>经典面试题：永久代为什么要被元空间替换呢？</font>



- 官方文档中声明将永久代去除改用元空间的动机是因为与 JRocket虚拟机融合，而JRocket虚拟机没有永久代，所以JDK8以后就去除了永久代。

- 官方文档这样的解释有点模糊，详细的原因我们下面剖析一下。



![image-20200723132622041](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200723132623.png)



### <font color=blue>经典面试题：StringTabl为什么要调整位置</font>



![image-20200723132527831](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200723132602.png)





```java
package com.atguigu.java1;

/**
 * 结论：
 * 静态引用对应的对象实体始终都存在堆空间
 *
 * jdk7：
 * -Xms200m -Xmx200m -XX:PermSize=300m -XX:MaxPermSize=300m -XX:+PrintGCDetails
 * jdk 8：
 * -Xms200m -Xmx200m -XX:MetaspaceSize=300m -XX:MaxMetaspaceSize=300m -XX:+PrintGCDetails
 * @author shkstart  shkstart@126.com
 * @create 2020  21:20
 */
public class StaticFieldTest {
    private static byte[] arr = new byte[1024 * 1024 * 100];//100MB

    public static void main(String[] args) {
        System.out.println(StaticFieldTest.arr);

//        try {
//            Thread.sleep(1000000);
//        } catch (InterruptedException e) {
//            e.printStackTrace();
//        }
    }
}
//测试结果 arr对象在jdk6-jdk8 都是存放在堆空间的老年代中的
```







```java
package com.atguigu.java1;

/**
 * 《深入理解Java虚拟机》中的案例：
 * staticObj、instanceObj、localObj存放在哪里？
 * @author shkstart  shkstart@126.com
 * @create 2020  11:39
 */
public class StaticObjTest {
    static class Test {
        static ObjectHolder staticObj = new ObjectHolder();
        ObjectHolder instanceObj = new ObjectHolder();

        void foo() {
            ObjectHolder localObj = new ObjectHolder();
            System.out.println("done");
        }
    }

    private static class ObjectHolder {
    }

    public static void main(String[] args) {
        Test test = new StaticObjTest.Test();
        test.foo();
    }
}

```

![image-20200723132431964](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200723132433.png)



![image-20200723132452205](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200723132453.png)



## 9.7 方法区的垃圾回收

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623204125.png" alt="image-20200623204124722" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623204810.png" alt="image-20200623204808855" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623205441.png" alt="image-20200623204907245" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623205432.png" alt="image-20200623205428164" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623205830.png" alt="image-20200623205829298" style="zoom:100%;" />











# 十、对象的实例化内存布局与访问定位

面试题中经常出现该关键点，主要考察对象实例化的过程和JVM内存结构的细节分配流程。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623210944.png" alt="image-20200623210901216" style="zoom:100%;" />









## 10.1 对象的实例化

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623211814.png" alt="image-20200623211812926"  />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623212349.png" alt="image-20200623212348232" style="zoom:100%;" />

![image-20200723132139110](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200723132140.png)



![image-20200723132201729](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200723132343.png)

![image-20200723132222727](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200723132329.png)

![image-20200723132245527](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200723132319.png)

![image-20200723132303256](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200723132308.png)





```java
package com.atguigu.java;

/**
 * 测试对象实例化的过程
 *  ① 加载类元信息 - ② 为对象分配内存 - ③ 处理并发问题  - ④ 属性的默认初始化（零值初始化）
 *  - ⑤ 设置对象头的信息 - ⑥ 属性的显式初始化、代码块中初始化、构造器中初始化
 *
 *
 *  给对象的属性赋值的操作：
 *  ① 属性的默认初始化 - ② 显式初始化 / ③ 代码块中初始化 - ④ 构造器中初始化
 * @author shkstart  shkstart@126.com
 * @create 2020  17:58
 */

public class Customer{
    int id = 1001;
    String name;
    Account acct;

    {
        name = "匿名客户";
    }
    public Customer(){
        acct = new Account();
    }

}
class Account{

}

```





## 10.2 对象的内存布局

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623215215.png" alt="image-20200623215213724" style="zoom:100%;" />



![image-20200723132057139](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200723132140.png)



## 10.3 对象的访问定位

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623220637.png" alt="image-20200623220635991" style="zoom:100%;" />



![image-20200723132035083](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200723132036.png)



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623220902.png" alt="image-20200623220900328" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623220928.png" alt="image-20200623220926716" style="zoom:100%;" />







# 十、 直接内存

![image-20200723131945582](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200723131947.png)

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623222522.png" alt="image-20200623222520692" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200623222617.png" alt="image-20200623222615870" style="zoom:100%;" />

![image-20200723131922753](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200723131947.png)

```java
package com.atguigu.java;

import sun.misc.Unsafe;

import java.lang.reflect.Field;

/**
 * -Xmx20m -XX:MaxDirectMemorySize=10m
 * @author shkstart  shkstart@126.com
 * @create 2020  0:36
 */
public class MaxDirectMemorySizeTest {
    private static final long _1MB = 1024 * 1024;

    public static void main(String[] args) throws IllegalAccessException {
        Field unsafeField = Unsafe.class.getDeclaredFields()[0];
        unsafeField.setAccessible(true);
        Unsafe unsafe = (Unsafe)unsafeField.get(null);
        while(true){
            unsafe.allocateMemory(_1MB);
        }

    }
}

```

![image-20200723131855981](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200723131904.png)



