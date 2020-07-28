【Java源码分析】图解 ArrayList 底层原理

本人在校小菜鸟，目前对源码得分析更着重于逻辑得分解剖析，更深层的架构设计可能我并不能一一都能分析出来。本文会用源码图片的方式制作图解，一览代码的设计过程，思路一定要清晰。更适合我这个小菜鸟以后复盘。



下面是本文借鉴的原博文作者和文章，站在巨人肩膀上，在此感谢。

作者 [HelloWorld_EE](https://blog.csdn.net/u010412719) :文章:https://blog.csdn.net/u010412719/article/details/51108357

# 一、属性



```java
public class ArrayList<E> extends AbstractList<E>
        implements List<E>, RandomAccess, Cloneable, java.io.Serializable
```

ArrayList类主要是继承AbstractList类并实现了List接口，实现Cloneable和Serializable接口使得ArrayList具有克隆和序列化的功能。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200728141029.png" alt="image-20200728141022556" style="zoom:100%;" />

```java
private static final long serialVersionUID = 8683452581122892189L;

/**
 * 默认初始容量大小
 */
private static final int DEFAULT_CAPACITY = 10;

/**
 * Shared empty array instance used for empty instances.
 * 共享式的空数组实例，用于空实例
 */
private static final Object[] EMPTY_ELEMENTDATA = {};

/**
 * Shared empty array instance used for default sized empty instances.
 * 共享式的空数组实例，用于有默认大小的空实例（其实调用无参构造器就会使用到这个空数组实例）
 *  翻译：与 EMPTY_ELEMENTDATA 数组的区别在于当第一个元素被加入进来的时候它知道如何扩张；如何扩张这一点在 add 方法中会充分体现
 */
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

/**
 * 存储ArrayList元素的数组缓冲区。
 * ArrayList的容量是这个数组缓冲区的长度。
 * 添加第一个元素时，任何elementData==DEFAULTCAPACITY_empty_elementData的空ArrayList实例都将扩展为默认的DEFAULT_CAPACITY。
 */
transient Object[] elementData; // 非私有以简化嵌套类访问

/**
 * The size of the ArrayList (the number of elements it contains).
 * ArrayList 实例的元素个数
 * @serial
 */
private int size;
```



# 二、构造函数

## 2.1 无参构造函数

```java
/**
 * Constructs an empty list with an initial capacity of ten.
 * 构造一个初始容量为10的，常量空数组。
 */
public ArrayList() {
    this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
}
```

源码上介绍的功能为：构造一个初始容量为 10 的空列表。

即当我们不提供参数而new一个对象时，底层的数组就是直接用长度为10 的空常量数组`DEFAULTCAPACITY_EMPTY_ELEMENTDATA`进行实例化。

可能有的人会问，此时`DEFAULTCAPACITY_EMPTY_ELEMENTDATA`的长度我们还不知道呀，从哪里可以看到是构造了一个初始容量为10的空列表呢？这里我们先不解释，在下面的`add(E e)`函数源码的介绍中会给出答案。






## 2.2 带参构造函数

1. 参数中指定容量

```java
public ArrayList(int initialCapacity) {
    if (initialCapacity > 0) {
        this.elementData = new Object[initialCapacity];
    } else if (initialCapacity == 0) {
        this.elementData = EMPTY_ELEMENTDATA;
    } else {
        throw new IllegalArgumentException("Illegal Capacity: "+
                                           initialCapacity);
    }
}
```

当参数小于0时，抛异常。

当参数等于0时，用空的常量数组对象EMPTY_ELEMENTDATA来初始化底层数组elementData。

当参数大于0时，new 一个指定容量的数组。默认初始化容量就是参数中指定的。



2. 参数中指定Collection容器

```java
public ArrayList(Collection<? extends E> c) {
    elementData = c.toArray();
    if ((size = elementData.length) != 0) {
        // c.toArray might (incorrectly) not return Object[] (see 6260652)
        if (elementData.getClass() != Object[].class)
            elementData = Arrays.copyOf(elementData, size, Object[].class);
    } else {
        // replace with empty array.
        this.elementData = EMPTY_ELEMENTDATA;
    }
}
```

将容器Collection转化为数组赋给数组elementData，还对Collection转化是否转化为了Object[]进行了检查判断。如果Collection为空，则就将空的常量数组对象EMPTY_ELEMENTDATA赋给了elementData；

# 三、add()方法与数组扩容

首先我们看一下源码中的方法定义：

```java
public boolean add(E e) {
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    elementData[size++] = e;
    return true;
}

 public void add(int index, E element) {
        rangeCheckForAdd(index);

        ensureCapacityInternal(size + 1);  // Increments modCount!!
        System.arraycopy(elementData, index, elementData, index + 1, size - index);
        elementData[index] = element;
        size++;
    }
```

在上面的add方法中，需要注意的是

 ==ensureCapacityInternal(size + 1);==

这个方法的调用，每次都会在添加之前调用。那么这个方法是干什么的呢？

其实，这个方法的大致用意就是 “确定容量” ，因为我们正要添加一个元素嘛，为了保证底层数组不越界，那么在添加之前，必须保证目前数组的容量够用。



接下来就是数组自动扩容机制了。

其实我们知道，底层数组一旦创建就指定了初始大小，所谓的扩容，其实就是再开辟一个新的更大的数组，并拷贝原数组的数据。



那么，底层它是如何实现自动扩容的呢？扩容时应该扩大到多少才行？

带着这些问题，继续看下去吧。

```java
private void ensureCapacityInternal(int minCapacity) {
    ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
}
```

- ==第一步：确定容量的内部方法==

- 调用 calculateCapacity() ,计算容量。会根据当前 elementData是一个如何构造的数组，来返回一个合适的容量。（详情请看下一步）
- 调用 ensureExplicitCapacity( ) 方法。



```java
private static int calculateCapacity(Object[] elementData, int minCapacity) {
    if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
        return Math.max(DEFAULT_CAPACITY, minCapacity);
    }
    return minCapacity;
}
```

- ==第二步：计算容量。==
- 该方法的用意是：判断当前的 elementData数组实例，是如何创建的。来确定合适的扩容容量。
  - 如果是以，defaultCapacity_Empty_ElementData 创建的实例，那么比较新容量 minCapacity 和默认容量 10 的大小。谁大谁就是新容量。



> 从源码可以看出，如果`elementData==DEFAULTCAPACITY_EMPTY_ELEMENTDATA`，则在默认容量10的基础上开辟空间。
>
> 这里的源码就解释了`DEFAULTCAPACITY_EMPTY_ELEMENTDATA`数组和`EMPTY_ELEMENTDATA`数组的区别之所在。
>
> - 相似：这两个写法好像，都带有 EMPTY_ELEMENTDATA ,这点说明它俩在初始化时都是空数组，容量为0；
> - 不同：`DEFAULTCAPACITY_EMPTY_ELEMENTDATA`在添加第一个元素时使用的扩容机制会和 10 比较大小，然后选择一个合适的新容量。
>
> 

```java
private void ensureExplicitCapacity(int minCapacity) {
    modCount++;

    // overflow-conscious code
    if (minCapacity - elementData.length > 0)
        grow(minCapacity);
}
```

- ==第三步：确定一个明确的容量==
- 一是判断新容量是否超出了原来的范围
- 二是，超出的话，调用  `grow(minCapacity);`方法，进行容量的扩大。（该方法的详解请看下一步)





```java
private void grow(int minCapacity) {
    // overflow-conscious code
    int oldCapacity = elementData.length;
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    if (newCapacity - minCapacity < 0)
        newCapacity = minCapacity;
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    // minCapacity is usually close to size, so this is a win:
    elementData = Arrays.copyOf(elementData, newCapacity);
}
```

- ==第四步：容量增长机制（重要！！）==
- <font color=red>首先，第一个 if 语句：</font>将原数组扩大 1.5 倍，判断一下能不能满足需求
- 如果不能满足 我们需要的空间 minCapacity ，则就扩大到 minCapacity好了。
- <font color=red>第二个 if 语句：</font>判断你的需求容量 是否超出了可接受的最大值
- 如果超出，就是你想要的太大了，我不能无休止的满足你，调用 `hugeCapacity(minCapacity);` 返回  `Integer.MAX_VALUE`
- <font color=red>最后， Arrays.copyOf ( )</font>,可见扩容的本质是开辟新空间，并拷贝原数据

> 从源码中可以看到，此函数的功能就是一个数组的扩张。一般情况下是扩展到原来数组长度的1.5倍。
> 但是，由于扩张1.5倍可能和我们的需要不一致，即可能太小，也可能太大。
>
> 因此，就有了源码中的两个 if 条件 的处理。即如果扩张1.5倍太小，则就用我们需要的空间大小minCapacity来进行扩张。如果扩张1.5倍太大或者是我们所需的空间minCapacity也太大，大到超出了可接受范围，则只能给你一个我能接受的最大数 `Integer.MAX_VALUE`来进行扩张。
> 

```java
private static int hugeCapacity(int minCapacity) {
    if (minCapacity < 0) // overflow
        throw new OutOfMemoryError();
    return (minCapacity > MAX_ARRAY_SIZE) ?
        Integer.MAX_VALUE :
        MAX_ARRAY_SIZE;
}
```

> 该方法，用来返回一个巨大容量值，参数指定一个想要的容量，
>
> - 如果容量过头了，只能提供  `Integer.MAX_VALUE `,
> - 如果没有过头，则返回可接受的最大容量 ` MAX_ARRAY_SIZE`
>
> - MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;



如果干看代码逐句分析还是一团乱码，下面的一张图可以帮你快速回忆起整个逻辑过程

==抽象总结：==

- add() 调用之前先检查容量
- 需要扩容时，检查默认1.5倍扩容能不能满足你，不能满足就按着你的需求扩张，但是你的需求不能超出 MAX_ARRAY_SIZE 。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200728165651.png" alt="" style="zoom:100%;" />





-----

补充：上面grow函数中最后一条语句elementData = Arrays.copyOf(elementData, newCapacity);的功能就是将原来的数组中的元素复制扩展到大小为newCapacity的新数组中，并返回这个新数组。Arrays.copyOf(elementData, newCapacity)的源码如下：


<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200728180558.png" alt="" style="zoom:100%;" />



# 四、其他方法



## get() 方法

```java
public E get(int index) {
    rangeCheck(index); //检查下标

    return elementData(index);
}
```





##  set(int index, E element)

改变ArrayList对象中某个位置元素的值的大小

```java
public E set(int index, E element) {
    rangeCheck(index);

    E oldValue = elementData(index);//获取指定下标的原来数据
    elementData[index] = element;
    return oldValue;
}
```

```java
E elementData(int index) {
    return (E) elementData[index];
}
```



## remove 删除方法

删除元素的方法有两个重载方法

- public E remove(int index)  ： 删除指定索引的元素
- public boolean remove(Object o)  ： 删除 匹配指定值 o 的元素（只删除一个匹配元素）

```java
public E remove(int index) {
    rangeCheck(index);

    modCount++;
    E oldValue = elementData(index);

    int numMoved = size - index - 1;
    if (numMoved > 0)
        System.arraycopy(elementData, index+1, elementData, index,
                         numMoved);
    elementData[--size] = null; // clear to let GC do its work

    return oldValue;
}
```

可见，该删除方法，删除指定索引的元素。底层使用拷贝机制。(即将数组元素从index+1位置开始到末尾拷贝到从index开始处)





```java
public boolean remove(Object o) {
    if (o == null) {
        for (int index = 0; index < size; index++)
            if (elementData[index] == null) {
                fastRemove(index);
                return true;
            }
    } else {
        for (int index = 0; index < size; index++)
            if (o.equals(elementData[index])) {
                fastRemove(index);//这是什么？
                return true;
            }
    }
    return false;
}
```

可见，该删除方法是删除第一个值与参数 o 值相等的元素

```java
/*
 * Private remove method that skips bounds checking and does not
 * return the value removed.
 */
private void fastRemove(int index) {
    modCount++;
    int numMoved = size - index - 1;
    if (numMoved > 0)
        System.arraycopy(elementData, index+1, elementData, index,
                         numMoved);
    elementData[--size] = null; // clear to let GC do its work
}
```

> 原以为fastRemove(int index)这个函数与remove(int index)会有很大的不同，看到源码之后，发现其实区别真心不大，唯一的区别是fastRemove函数没有对index进行有效性检查，以及没有返回移除的旧值，为什么不返回呢？这是因为remove(Object o)给出的就是要删除的值，因此不返回旧值是非常正常的
> 



##  clear() ：清空方法

直接将数组中的所有元素设置为null即可，这样便于垃圾回收。

```java
public void clear() {
    modCount++;

    // clear to let GC do its work
    for (int i = 0; i < size; i++)
        elementData[i] = null;

    size = 0;
}
```