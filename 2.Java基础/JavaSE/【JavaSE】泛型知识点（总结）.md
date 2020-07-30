# 【JavaSE】泛型知识点（总结）





# 一、简介

泛型的本质是为了参数化类型（在不创建新的类型的情况下，通过泛型指定的不同类型来控制形参具体限制的类型）。也就是说在泛型使用过程中，

操作的数据类型被指定成一个参数，这种参数类型可以用在类、接口和方法中，分别被称为泛型类、泛型接口、泛型方法。

**泛型**：把类型明确的工作推迟到创建对象或调用方法的时候才去明确的特殊的类型
**Java泛型设计原则**：只要在编译时期没有出现警告，那么运行时期就不会出现ClassCastException异常.


> **Java 的泛型**
> Java 泛型的参数只可以代表类，不能代表个别对象。由于 Java 泛型的类型参数之实际类型在编译时会被消除，所以无法在运行时得知其类型参数的类型。
> Java 编译器在编译泛型时会自动加入类型转换的编码，故运行速度不会因为使用泛型而加快。Java 允许对个别泛型的类型参数进行约束，包括以下两种形式（假设 T 是泛型的类型参数，C 是一般类、泛类，或是泛型的类型参数）：T 实现接口 I 。T 是 C ，或继承自 C 。一个泛型类不能实现Throwable接口。
>  ------------------------------------------------------------<百度百科> ---------------------------------------------------------




# 二、泛型的使用
参考原文：[https://blog.csdn.net/s10461/article/details/53941091]()
## 2.1 泛型类
泛型类：就是把泛型定义在类上，用户使用该类的时候，才把类型明确下来.
通过泛型可以完成对一组类的操作对外开放相同的接口。最典型的就是各种容器类，如：List、Set、Map。

```java
/**
 * 该类为一个泛型类
 * @param <T> ：在实例化泛型类时，必须指定 类型参数T 的具体类型
 */
public class Generic<T> {

    // 这个成员变量的类型为T,T的类型由外部指定
    private T value;

    // 泛型构造方法形参value的类型也为T，T的类型由外部指定
    public Generic(T value) {
        this.value = value;
    }
    //泛型方法getValue的返回值类型为T，T的类型由外部指定
    public T getValue() {
        return value;
    }
}

```
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200704173650.png)
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200704173942.png)
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200704173957.png)


**==注意==：**

- 泛型的类型参数只能是类类型，不能是简单类型。
- 不能对确切的泛型类型使用`instanceof`操作。如下面的操作是非法的，编译时会出错。
　`　if(ex_num instanceof Generic<Number>){ }`

- 泛型类派生出的子类
	- 子类将泛型类型确定

	- 子类泛型类型不确定：继续使用父类定义的泛型



## 2.2 泛型接口
泛型接口与泛型类的定义及使用基本相同。泛型接口常被用在各种类的生产器中，可以看一个例子：
```java
/**
 * 泛型接口
 * @param <T>
 */
public interface Generator<T> {

    public T next();
}
```
当实现泛型接口的类，未传入泛型实参时：
```java
/**
 * 实现泛型接口的类
 * 未传入泛型实参时，与泛型类的定义相同，在声明类的时候，需将泛型的声明 T 也一起加到类中
 * 即：class FruitGenerator<T> implements Generator<T>{
 * 如果不声明泛型，如：class FruitGenerator implements Generator<T>，编译器会报错：cannot resolve symbol T
 * @param <T>
 */
public class FruitGenerator<T> implements Generator<T>{
    @Override
    public T next() {
        return null;
    }
}

```


当实现泛型接口的类，传入泛型实参时：
```java

/**
 * 传入泛型实参时：
 * 定义一个生产器实现这个接口,虽然我们只创建了一个泛型接口 Generator<T>
 * 但是我们可以为T传入无数个实参，形成无数种类型的 Generator接口。
 * 在实现类实现泛型接口时，如已将泛型类型传入实参类型，则所有使用泛型的地方都要替换成传入的实参类型
 * 即：Generator<T>，public T next();中的的T都要替换成传入的String类型。
 */
public class FruitGenerator implements Generator<String>{
    private String[] fruits = new String[]{"apple","banana","peach"};

    @Override
    public String next() {
        Random r = new Random();
        return fruits[r.nextInt(3)];
    }
}

```

## 2.3 泛型方法
泛型类，是在实例化类的时候指明泛型的具体类型；
泛型方法，是在调用方法的时候指明泛型的具体类型 。
就只能调对象与类型无关的方法，不能调用对象与类型有关的方法

```java
/**
 * 泛型方法的基本介绍
 * @param tClass 传入的泛型实参
 * @return T 返回值为T类型
 * 说明：
 *     1）public 与 返回值中间<T>非常重要，可以理解为声明此方法为泛型方法。
 *     2）只有声明了<T>的方法才是泛型方法，泛型类中的使用了泛型的成员方法并不是泛型方法。
 *     3）<T>表明该方法将使用泛型类型T，此时才可以在方法中使用泛型类型T。
 *     4）与泛型类的定义一样，此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型。
 */
public <T> T genericMethod(Class<T> tClass)throws InstantiationException ,
  IllegalAccessException{
        T instance = tClass.newInstance();
        return instance;
}
```


```java
public class GenericTest {
   //这个类是个泛型类，在上面已经介绍过
   public class Generic<T>{     
        private T key;

        public Generic(T key) {
            this.key = key;
        }

        //我想说的其实是这个，虽然在方法中使用了泛型，但是这并不是一个泛型方法。
        //这只是类中一个普通的成员方法，只不过他的返回值是在声明泛型类已经声明过的泛型。
        //所以在这个方法中才可以继续使用 T 这个泛型。
        public T getKey(){
            return key;
        }

        /**
         * 这个方法显然是有问题的，在编译器会给我们提示这样的错误信息"cannot reslove symbol E"
         * 因为在类的声明中并未声明泛型E，所以在使用E做形参和返回值类型时，编译器会无法识别。
        public E setKey(E key){
             this.key = keu
        }
        */
    }

    /** 
     * 这才是一个真正的泛型方法。
     * 首先在public与返回值之间的<T>必不可少，这表明这是一个泛型方法，并且声明了一个泛型T
     * 这个T可以出现在这个泛型方法的任意位置.
     * 泛型的数量也可以为任意多个 
     *    如：public <T,K> K showKeyName(Generic<T> container){
     *        ...
     *        }
     */
    public <T> T showKeyName(Generic<T> container){
        System.out.println("container key :" + container.getKey());
        //当然这个例子举的不太合适，只是为了说明泛型方法的特性。
        T test = container.getKey();
        return test;
    }

    //这也不是一个泛型方法，这就是一个普通的方法，只是使用了Generic<Number>这个泛型类做形参而已。
    public void showKeyValue1(Generic<Number> obj){
        Log.d("泛型测试","key value is " + obj.getKey());
    }

    //这也不是一个泛型方法，这也是一个普通的方法，只不过使用了泛型通配符?
    //同时这也印证了泛型通配符章节所描述的，?是一种类型实参，可以看做为Number等所有类的父类
    public void showKeyValue2(Generic<?> obj){
        Log.d("泛型测试","key value is " + obj.getKey());
    }

     /**
     * 这个方法是有问题的，编译器会为我们提示错误信息："UnKnown class 'E' "
     * 虽然我们声明了<T>,也表明了这是一个可以处理泛型的类型的泛型方法。
     * 但是只声明了泛型类型T，并未声明泛型类型E，因此编译器并不知道该如何处理E这个类型。
    public <T> T showKeyName(Generic<E> container){
        ...
    }  
    */

    /**
     * 这个方法也是有问题的，编译器会为我们提示错误信息："UnKnown class 'T' "
     * 对于编译器来说T这个类型并未项目中声明过，因此编译也不知道该如何编译这个类。
     * 所以这也不是一个正确的泛型方法声明。
    public void showkey(T genericObj){

    }
    */

    public static void main(String[] args) {


    }
}
```

## 2.4 泛型通配符
在泛型中并没有像我们面向对象的继承结构，想要使用任意的泛型类型，我们可以使用通配符！
?号通配符表示可以匹配任意类型，任意的Java类都可以匹配
带有子类限定的可以从泛型读取`(? extend T)`
带有超类限定的可以从泛型写入`(? super T)`

如果参数之间的类型有依赖关系，或者返回值是与参数之间有依赖关系的。那么就使用泛型
如果没有依赖关系的，就使用通配符，通配符会灵活一些.



## 2.5 泛型与可变参数
什么是可变参数，了解可变参数 参考：[https://blog.csdn.net/w605283073/article/details/91902705]()

<font color=red>定义</font>

 **可变参数：适用于参数个数不确定，类型确定的情况，java把可变参数当做数组处理。**

  注意：可变参数必须位于最后一项。



<font color=red>特点</font>

1. 只能出现在参数列表的最后； 
2. ...位于变量类型和变量名之间，前后有无空格都可以；
3. 调用可变参数的方法时，编译器为该可变参数隐含创建一个数组，在方法体中以数组的形式访问可变参数。





再看一个泛型方法和可变参数的例子：

```java
public <T> void printMsg( T... args){
    for(T t : args){
        Log.d("泛型测试","t is " + t);
    }
}
```

```java
printMsg("111",222,"aaaa","2323.4",55.55);
```


## 2.6 静态方法与泛型

静态方法有一种情况需要注意一下，那就是在类中的静态方法使用泛型：静态方法无法访问类上定义的泛型；如果静态方法操作的引用数据类型不确定的时候，必须要将泛型定义在方法上。

**即：如果静态方法要使用泛型的话，必须将静态方法也定义成泛型方法 。**

```java
public class StaticGenerator<T> {
    ....
    ....
    /**
     * 如果在类中定义使用泛型的静态方法，需要添加额外的泛型声明（将这个方法定义成泛型方法）
     * 即使静态方法要使用泛型类中已经声明过的泛型也不可以。
     * 如：public static void show(T t){..},此时编译器会提示错误信息：
          "StaticGenerator cannot be refrenced from static context"
     */
    public static <T> void show(T t){

    }
}
```

# 三、泛型的优点
- 代码更加简洁【不用强制转换】

- 程序更加健壮【只要编译时期没有警告，那么运行时期就不会出现ClassCastException异常】

- 可读性和稳定性【在编写集合的时候，就限定了类型】

# 四、类型擦除
**什么是类型擦除？**

> 泛型是提供给javac编译器使用的，它用于限定集合的输入类型，让编译器在源代码级别上，即挡住向集合中插入非法数据
>
> 但编译器编译完带有泛型的java程序后，生成的class文件中将不再带有泛型信息，以此使程序运行效率不受到影响，这个过程称之为“**类型擦除**“。

# 案例应用