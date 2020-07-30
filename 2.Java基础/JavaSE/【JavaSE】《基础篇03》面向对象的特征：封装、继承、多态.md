【JavaSE】面向对象的特征：封装、继承、多态



# 一、继承

## 抽象类与接口的区别

1. ==语法上==

① 抽象类

abstract关键字

```java
abstract class Demo ｛   
 private int id;
 private int name;
    
abstract void method1();     
abstract void method2();    
｝
```

 

② 接口

默认 public static final 关键字 

```java
interface Demo {    
void method1();     
void method2();    
}  
```

在abstract class方式中，Demo可以有自己的数据成员，也可以有非abstract的成员方法，

而在interface方式实现中，Demo只能有静态的不能被修改的数据成员（也就是默认都是`static final`的 )

不过在interface中一般不定义数据成员，**所有的成员方法**都是抽象的。从某种意义上说，interface时一种特殊的abstract class。



2. ==继承角度==

类是单继承、接口可以多继承

一个普通类继承一个接口后，必须实现这个接口中定义的所有方法，否则就只能被定义为抽象类。

但是在接口的定义中，方法却不能拥有默认行为。不过在jdk1.8中可以使用default关键字实现默认方法。

```java
interface InterfaceA {
    default void foo() {
        System.out.println("default，接口中的默认方法实现。。");
    }
}
```

在Java8之前，接口与其实现类之间的耦合度太高了，当需要添加一个方法时，所有的实现类都必须随之修改。

`默认方法`解决了这个问题，他可以为接口添加新的方法，而不会破坏已有的实现类。这在 lambda 表达式作为Java 8 语言的重要特性而出现之际，为升级旧接口且保持向后兼容（backward compatibility）提供了途径。

总结：

1. 抽象类和接口都不能直接实例化，必须借助具体实现类进行实例化。
2. 抽象类要被子类单继承，接口要被类可以多实现。
3. 接口里定义的变量只能是公共的静态的常量（static final），抽象类中的变量是普通变量。
4. 抽象类里可以没有抽象方法。
5. 接口中没有 `this` 指针，没有构造函数，不能拥有实例字段（实例变量）或实例方法。
6. 抽象类不能在Java 8 的 lambda 表达式中使用。

原文链接：https://blog.csdn.net/xiaoxu9522/article/details/90748914



## 为什么不能多继承？

答：（菱形继承）反推
假设存在多继承，A类派生B，C ， 则B包含自己的内容和继承过来的内容 a，b ，C同 a，c；如果多继承存在 D继承B和C，则D中会包含a，b a ，c 此时a被复制两份 同一个作用域中，不能出现两份相同的
所以只能单一继承。





# 二、多态



继承是多态得以实现的基础。从字面上理解，多态就是一种类型表现出多种状态。

多态有两种表现形式：

- 继承+重写（动多态）
- 重载（静多态）

> 将一个方法调用同这个方法所属的主体（也就是对象或类）关联起来叫做绑定，分前期绑定和后期绑定两种。下面解释一下它们的定义：
>
> 1. 前期绑定：在程序运行之前进行绑定，由编译器和连接程序实现，又叫做静态绑定。比如static方法和final方法，注意，这里也包括private方法，因为它是隐式final的。
> 2. 后期绑定：在运行时根据对象的类型进行绑定，由方法调用机制实现，因此又叫做动态绑定，或者运行时绑定。除了前期绑定外的所有方法都属于后期绑定。

下面以一个例子来看一下继承+重写形式实现多态的思想

```java
  //汽车接口
 interface Car {
 	// 汽车名称
 	String getName();
 	// 获得汽车售价
 	int getPrice();
 }
 
 // 宝马
 class BMW implements Car {
 	public String getName() {
 		return "BMW";
 	}
 	public int getPrice() {
 		return 300000;
 	}
 }
 
 // 奇瑞QQ
 class CheryQQ implements Car {
 	public String getName() {
 		return "CheryQQ";
 	}
 	public int getPrice() {
 		return 20000;
 	}
 }
 
 // 汽车出售店
 public class CarShop {
 	// 售车收入
 	private int money = 0;
 	// 卖出一部车
 	public void sellCar(Car car) {
 		System.out.println("车型：" + car.getName() + " 单价：" + car.getPrice());
 		// 增加卖出车售价的收入
 		money += car.getPrice();
 	}
 	// 售车总收入
 	public int getMoney() {
 		return money;
 	}
 
 	public static void main(String[] args) {
 		CarShop aShop = new CarShop();
 		// 卖出一辆宝马
 		aShop.sellCar(new BMW());
 		// 卖出一辆奇瑞QQ
 		aShop.sellCar(new CheryQQ());
 		System.out.println("总收入：" + aShop.getMoney());
 	}
 }
```

多态就是在后期绑定这种机制上实现的。多态给我们带来的好处是消除了类之间的耦合关系，使程序更容易扩展。比如在上例中，新增加一种类型汽车的销售，只需要让新定义的类继承Car类并实现它的所有方法，而无需对原有代码做任何修改，CarShop类的sellCar(Car car)方法就可以处理新的车型了。新增代码如下：

```java
// 桑塔纳汽车
 class Santana implements Car {
  public String getName() {
   return "Santana";
  }
 public int getPrice() {
    return 80000;
  }
 }
```



## 重载(overloading)和重写(overriding)

重载和重写都是针对方法的概念，在弄清楚这两个概念之前，我们先来了解一下什么叫`方法的型构`（英文名是signature，有的译作“签名”，）。

`型构`就是指方法的组成结构，具体包括方法的名称和参数，(参数的数量、类型以及出现的顺序，但是不包括方法的返回值类型，访问权限修饰符，以及abstract、static、final等修饰符)。

比如下面两个就是具有相同型构的方法：

```java
public void method(int i, String s) {

 }
 public String method(int i, String s) {

 }
//只有返回值不同，无法构成不同型构
```



了解完型构的概念后我们再来看看重载和重写，请看它们的定义：

==重载==

- 英文名是overloading，是指在同一个类中定义了一个以上具有相同名称，但是型构不同的方法。在同一个类中，是不允许定义多个的相同型构方法的。

==重写==

- 英文名是overriding，是指在继承情况下，子类中定义了与其基类中方法具有相同型构的新方法，就叫做子类把基类的方法重写了。这是实现多态必须的步骤。

- 重写方法一定不能抛出新的检查异常或者比被重写方法申明更加宽泛的检查型异常，譬如父类方法声明了一个检查异常 IOException，在重写这个方法时就不能抛出 Exception，只能抛出 IOException 的子类异常，可以抛出非检查异常。

==总结==

重载与重写是 Java 多态性的不同表现，重写是父类与子类之间多态性的表现，在运行时起作用（动态多态性，譬如实现动态绑定），而重载是一个类中多态性的表现，在编译时起作用（静态多态性，譬如实现静态绑定）。





### <font color=#009C41>**问：Java 构造方法能否被重写和重载？**</font>



**答：**重写是子类方法重写父类的方法，重写的方法名不变，而类的构造方法名必须与类名一致，假设父类的构造方法如果能够被子类重写则子类类名必须与父类类名一致才行，所以 Java 的构造方法是不能被重写的。而重载是针对同一个的，所以构造方法可以被重载。



### <font color=#009C41>**问：下面程序的运行结果是什么，为什么？**</font>

```java
public class Demo
{    
public boolea equals(Demo other)
{  
 System.out.println("use Demo equals.");       
 return true;   
}
 public static void main (string[] args){
     Obeject o1 = new Demo();
     Obeject o2 = new Demo();
     Demo o3 = new Demo();
     Demo o4 = new Demo();
     
     if(o1.equels(o2)){
         System.out.println("o1 is equal with o2");
     }
      if(o3.equels(o4)){
         System.out.println("o3 is equal with o4");
     }
     
 }
```

**答：**上面程序的运行结果如下。

```
use Demo equals.

o3 is equal with o4.
```



因为 Demo 类中的` public boolean equals(Demo other) `方法并没有重写 Object 类中的 `public boolean equals(Object obj) `方法，原因是其违背了参数规则，其中一个是 Demo 类型而另一个是 Object 类型，因此这两个方法是重载关系（发生在编译时）而不是重写关系；

  故当调用 `o1.equals(o2) `时，实际上调用了 Object 类中的 `public boolean equals(Object obj) `方法，而 Object 类的 equals 方法是通过比较内存地址才返回 false；

当调用 `o3.equals(o4) `时，实际上调用了 Demo 类中的 `equals(Demo other) `方法，，所以才有上面的打印。