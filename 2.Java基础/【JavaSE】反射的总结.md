【JavaSE】反射的总结





# 一、类的加载概述

![在这里插入图片描述](G:\图片\blog\20200525234855352.png)



加载
- 就是指将class文件读入内存，并为之创建一个Class对象
- 任何类被使用时系统都会建立一个Class对象

连接
- 验证：是否有正确的内部结构，并和其他类协调一致
- 准备：负责为类的静态成员分配内存，并设置默认初始化值
- 解析：将类的二进制数据中的符号引用替换为直接引用

初始化
 - 就是我们之前讲过的初始化步骤


 **类的初始化时机（什么时候被加载）**

> - 创建类的实例
>  - 用到类的静态变量
>   - 用到类的静态方法
>   -  使用反射方式强制创建某个类或接口对应的` java.lang.Class `对象 
>    - 初始化某个类的子类 直接使用` java.exe `命令来运行某个主类

 **类加载器**

>**引导类加载器（Bootstrap ClassLoader）**
>
>- 引导类加载器是jvm在运行时，内嵌在jvm中的一段特殊的用来加载java核心类库的C++代码。String.class 对象就是由引导类加载器加载的，引导类加载器具体加载哪些核心代码可以通过获取值为 "sun.boot.class.path" 的系统属性获得。引导类加载器不是java原生代码编写的，所以其也不是java.lang.ClassLoader类的实例，其没有getParent方法。

>
>**拓展类加载器（Extension ClassLoader）**
>
>- 拓展类加载器用来加载jvm实现的一个拓展目录，该目录下的所有java类都由此类加载器加载。此路径可以通过获取"java.ext.dirs"的系统属性获得。拓展类加载器就是java.lang.ClassLoader类的一个实例，其getParent方法返回的是引导类加载器（在 HotSpot虚拟机中用null表示引导类加载)。


>
>**应用类加载器（Application ClassLoader)**
>
>- 应用类加载器又称为系统类加载器，开发者可用通过 java.lang.ClassLoader.getSystemClassLoader()方法获得此类加载器的实例，系统类加载器也因此得名。其主要负责加载程序开发者自己编写的java类。一般来说，java应用都是用此类加载器完成加载的，可以通过获取"java.class.path"的系统属性（也就是我们常说的classpath）来获取应用类加载器加载的类路径。应用类加载器是java.lang.ClassLoader类的一个实例，其getParent方法返回的是拓展类加载器。


# 二、反射的基本使用
### 2.1 获取Class字节码对象
getClass()
.class
Class.forName("文件全路径")


### 2.2 获取构造方法

获取所有
```java
public Constructor[] getConstructors()
public Constructor[] getDeclaredConstructors()
```

获取单个
	
```java
public Constructor<T> getConstructor(Class<?>...parameterTypes);//带有参数
getDeclaredConstructor();//获取私有构造方法对象
con.setAccessible(true;//指反射的对象在使用时应取消java语言访问检查，暴力访问
```

```java
con.newInstance();//使用构造方法对象表示的构造方法来创建该构造方法的声明类的新实例
```

代码：获取无参构造函数
```java
public class ReflexDemo01 {

    public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        // 获取字节码对象
        Class person = Class.forName("Person");
        // 获取构造方法
        Constructor constructor = person.getConstructor();
        // 实例化对象
        Object obj = constructor.newInstance();

        System.out.println(obj);//Person@1b6d3586
    }
}

```

代码：获取带参构造函数
```java
 public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        Class person = Class.forName("Person");
        Constructor constructor = person.getConstructor(String.class, String.class, int.class);
        Object obj = constructor.newInstance("张三","男",20);
        System.out.println(obj);
    }
```

代码：获取私有构造函数


```java
 public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        Class person = Class.forName("Person");
        
        Constructor constructor = person.getDeclaredConstructor(String.class, String.class, int.class);
      
        constructor.setAccessible(true);
        
        Object obj = constructor.newInstance("张三","男",20);
       
        System.out.println(obj);
    }
```
> <font color=red> 注意：如果不设置  constructor.setAccessible(true);
> 会报错：java.lang.IllegalAccessException: Class ReflexDemo01 can not access a
> member of class Person with modifiers "private"</font>



### 2.3 获取成员变量
获取所有
- Field[] getFields()
- Field[] getDeclaredFields()

获取单个
- Filed getField("成员变量名")
- Field getDeclaredField()
- field.setAccessible(true)




### 2.4 获取成员方法并使用
- getMethods
- getDeclaredMethods
- getMethod
- getDeclaredMethod
- method.setAccessible(true)


```java
public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {
        Class person = Class.forName("Person");

        Constructor constructor = person.getDeclaredConstructor(String.class, String.class, int.class);

        constructor.setAccessible(true);

        Object obj = constructor.newInstance("张三","男",20);

        // Method[] methods = c.getMethods(); // 获取自己的包括父亲的公共方法
        // Method[] methods = c.getDeclaredMethods(); // 获取自己的所有的方法
        //Method[] methods = c.getMethod(String name,Class<?>... parameterTypes)
        // 第一个参数表示的方法名，第二个参数表示的是方法的参数的class类型
        Method method01 = person.getMethod("method01");
        Method method02 = person.getMethod("method02", String.class);

        Method method03 = person.getDeclaredMethod("method03", String.class, int.class);
        method03.setAccessible(true);

        method01.invoke(obj);
        method02.invoke(obj,"hello");
        Object resultStr = method03.invoke(obj, "hello", 123);
        System.out.println(resultStr);

    }
```

# 三、案例

### 3.1 通过反射运行配置文件

```java
/**
 * 通过反射运行配置文件
 */
public class ReflexDemo02 {
    public static void main(String[] args) throws IOException, ClassNotFoundException, NoSuchMethodException, IllegalAccessException, InvocationTargetException, InstantiationException {

        // 读取键值对数据
        Properties prop = new Properties();
        FileReader fr = new FileReader("F:\\Projects\\idea_projects\\javaseTest\\09reflex\\src\\class.txt");
        prop.load(fr);
        fr.close();
        // 获取文件数据
        String className = prop.getProperty("className");
        String methodName = prop.getProperty("methodName");

        // 反射
        Class c = Class.forName(className);

        Constructor constructor = c.getConstructor();

        Object obj = constructor.newInstance();

        Method method = c.getMethod(methodName);

        method.invoke(obj);


    }
}

```


### 3.2 通过反射越过泛型检查

```java
/**
 * 通过反射越过泛型检查
 */
public class ReflexDemo03 {
    public static void main(String[] args) throws NoSuchMethodException, InvocationTargetException, IllegalAccessException {

        // 创建集合
        ArrayList<Integer> arrayList = new ArrayList<Integer>();

        // arrayList.add(123);
       //  arrayList.add("hello");//直接调用编译期就会报错

        Class c = arrayList.getClass();
        Method m = c.getMethod("add", Object.class);

        m.invoke(arrayList,"hello");
        m.invoke(arrayList,"world");
        m.invoke(arrayList,"java");

        System.out.println(arrayList);//[hello, world, java]


    }
}

```

### 3.3 通过反射写一个Tool工具类，设置任意对象的任意属性



```java
public class PropertyTools {
    /**
     * 给传入对象设置属性
     */
    public void setProperty(Object obj,String propertyName,Object value) throws NoSuchFieldException, IllegalAccessException {

        // 根据对象获取字节码对象
        Class c = obj.getClass();
        // 根据名称获取成员变量
        Field field = c.getDeclaredField(propertyName);
        // 取消访问检查
        field.setAccessible(true);
        // 给对象的成员变量赋值为指定的值
        field.set(obj,value);

    }

    /**
     * 测试该工具类
     */
    public static void main(String[] args) throws NoSuchFieldException, IllegalAccessException {
        Person person = new Person();
        PropertyTools tools = new PropertyTools();
        tools.setProperty(person,"name","林青霞");
        tools.setProperty(person,"sex","女");
        tools.setProperty(person,"age",20);

        System.out.println(person);//Person{name='林青霞', sex='女', age=20}
    }


}

```

> <font color=red> 感觉这个案例很能说明：反射破坏了封装性</font>







# 四、面试题





### 4.1 说说你对 Java 中反射的理解

 Java中的反射首先是能够获取到Java中要反射类的字节码，获取字节码有三种方法，

- Class.forName(className)。

- 类名.class。

- this.getClass()。然后将字节码中的方法，变量，构造函数等映射成相应的Method、Filed、Constructor等类，这些类提供了丰富的方法可以被我们所使用。

 