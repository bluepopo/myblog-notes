

@[toc]

总体框架图：
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702111109.png" alt="在这里插入图片描述" style="zoom:100%;" />
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702111110.png" alt="在这里插入图片描述" style="zoom:100%;" />

# 一、Collection接口
## 1. Collection接口源码

```java
public interface Collection<E> extends Iterable<E> {
   //返回大小
    int size();
    
    //是否为空
    boolean isEmpty();
    
    //是否包含
    boolean contains(Object o)
        
    //返回迭代器    
    Iterator<E> iterator();
    
    //集合转换为数组
    Object[] toArray();

    //转换为对应泛型的数组
    <T> T[] toArray(T[] a);

    //添加
    boolean add(E e);

    //移除指定元素
    boolean remove(Object o);

     //将 other 集合中的所有元素添加到这个集合，如果由于这个调用改变了集合 ， 返回 true
    boolean addAll(Collection<? extends E> c);
   
    

    //从这个集合删除 filter 返回 true 的所有元素
    default boolean removeIf(Predicate<? super E> filter) {
        Objects.requireNonNull(filter);
        boolean removed = false;
        final Iterator<E> each = iterator();
        while (each.hasNext()) {
            if (filter.test(each.next())) {
                each.remove();
                removed = true;
            }
        }
        return removed;
    }

   //是否存在交集。从这个集合中删除所有与 other 集合中的元素不同的元素 。
    boolean retainAll(Collection<?> c);
    
    //清空
    void clear();  
    
    boolean equals(Object o); 
    
    int hashCode();
    
    @Override
    default Spliterator<E> spliterator() {
        return Spliterators.spliterator(this, 0);
    }
  
    default Stream<E> stream() {
        return StreamSupport.stream(spliterator(), false);
    }
 
    default Stream<E> parallelStream() {
        return StreamSupport.stream(spliterator(), true);
    }
}
```



## 2. 迭代器介绍

[详情请看另一篇博客，Iterator迭代器介绍](https://blog.csdn.net/qq_41864648/article/details/107843606)



## 3. 遍历集合的方式

方式一：集合转数组。使用普通for循环

方式二：使用Collection接口定义的迭代器

方式三：使用实现集合类独有的遍历方式，

- 如 Arraylist可以使用下标进行普通for循环，增强for、迭代器
- LinkedList支持普通for（ list.get(index);）、增强for、迭代器
- Set只能使用 iterator 迭代器遍历、
- Map遍历则有5种方式（键值对EntrySet、迭代器、keySet+values、通过键找值的传统方式、正则表达式）
- Map的四种遍历方式：https://www.cnblogs.com/damoblog/p/9124937.html



## 4. 集合与数组之间的转换

List与数组的转换：https://www.cnblogs.com/quanqi-yhz/p/11023955.html



> 



## 5.接口、实现类架构图

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200801130105.png" alt="image-20200801130103417" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200806224342.png" alt="image-20200801122722011" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200806231432.png" alt="image-20200803230514416" style="zoom:100%;" />

# 二. List接口

**List 接口概述**

- 有序集合，通过整数索引随机访问元素
- 与 Set集合不同，List允许有重复元素

**实现类的特点**

- **ArrayList**:：底层是数组结构实现，查询快、增删慢
- **LinkedList**： 底层是双向链表结构实现，查询慢、增删快
- **Vector**：底层是数组。synchonszied 修饰，线程安全
- **CopyOnWriteArrayList**： 底层用vailette修饰了内部数组，线程安全、读写分离锁。




## 1. ArrayList

> [List集合之ArrayList深度解析](https://blog.csdn.net/weixin_40304387/article/details/80790177)
>
> [《Java基础系列-ArrayList》](https://www.jianshu.com/p/b85cf23fef07)
>
> 我的另一篇博客：[【Java源码分析】ArrayList底层原理（带图解）](https://blog.csdn.net/qq_41864648/article/details/107643243)



**案例：元素去重**

```java
/**
 * 字符串去重：与自身比较
 */
public class ArrayListDemo01 {
    public static void main(String[] args) {
        ArrayList arrayList = new ArrayList();
        arrayList.add("hello");
        arrayList.add("java");
        arrayList.add("hello");
        arrayList.add("world");

        for (int i = 0; i < arrayList.size() - 1; i++){
            for (int j = i + 1; j<arrayList.size();j++){
                if (arrayList.get(i).equals(arrayList.get(j))){
                    arrayList.remove(j);//删除完之后底层数组会发生移动，索引j会被后面的元素覆盖
                    j--;
                }
            }
        }
        //遍历集合
        for (int i = 0; i < arrayList.size(); i++){
            System.out.println(arrayList.get(i));
        }
    }
}

```

```java
/**
 * 自定义对象的集合，去重
 * 自定义对象一定要重写 equals、hashcode方法
 */

public class ArrayListDemo02 {
    public static void main(String[] args) {

        Student stu1 = new Student("张三",18);
        Student stu2 = new Student("李四",20);
        Student stu3 = new Student("张三",18);
        Student stu4 = new Student("张三",18);
        Student stu5 = new Student("张三",18);

        ArrayList list = new ArrayList();
        list.add(stu1);
        list.add(stu2);
        list.add(stu3);
        list.add(stu4);
        list.add(stu5);
        //开辟一个新的数组
        ArrayList newList = new ArrayList();
        // 元素去重
        for (int i = 0; i < list.size(); i++ ){
            Student  s = (Student) list.get(i);//没有使用泛型，这里需要强转一下
            if (!newList.contains(s)){
                //如果新数组不包含该元素，则添加
                newList.add(s);
            }
        }

        //遍历newList
        for (int i = 0; i < newList.size(); i++){
            Student s = (Student) newList.get(i);
            System.out.println(s);
        }

    }
}

```

`contains() `方法的源码分析：

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200801171559.png" alt="在这里插入图片描述" style="zoom:100%;" />



## 2. LinkedList

参考文章：

[Java集合 LinkedList的原理及使用](https://www.cnblogs.com/LiaHon/p/11107245.html)

[HelloWorld_EE：《JAVA源码分析》：LinkedList ](https://blog.csdn.net/u010412719/article/details/51124350)



## 3. Vector
要了解Vector源码大体有什么的基本了解：[《Java源码分析》：Vector](https://blog.csdn.net/u010412719/article/details/51992819)

这篇文章从源码基本分析了两个集合的区别，非常细致：[深入解析 Java集合类ArrayList与Vector的区别](https://blog.csdn.net/qq_37113604/article/details/80836025)



简单总结：

1、Vector是线程安全的，ArrayList是线程非安全的。从上面的构造方法还有增删改查的操作其实我们都发现了，都有这么一个synchronized关键字，就是这个关键字为Vector容器提供了一个安全机制，保证了线程安全。

2、Vector可以指定增长因子，如果该增长因子指定了，那么扩容的时候会每次新的数组大小会在原数组的大小基础上加上增长因子；如果不指定增长因子，那么就给原数组大小*2，



Vecor内部有一个 elements()方法，返回一个可用来迭代的 Enumeration 对象。其实 Vecor内部也是有Itr、ListItr两个内部类的。但是为啥又支持 Enumeration  捏？应该是Vecor太老了吧。。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702111122.png" alt="在这里插入图片描述" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200806221027.png" alt="image-20200806221015874" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200806221043.png" alt="image-20200806220944028" style="zoom:100%;" />



Vector的两种遍历方式



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200806221229.png" alt="在这里插入图片描述" style="zoom:100%;" />







## 4. CopyOnWriteArrayList
我的另一篇博客笔记：[【JavaSE】CopyOnWriteArrayList](https://blog.csdn.net/qq_41864648/article/details/107851120)





## 5. LIst各子类的区别与对比



## 说一下aray与ArrayLIst的区别

1. 动态性。ArrayList是容量可以动态增长的Java集合，底层也是使用的数组，每次自动扩容都会拷贝数组。但是 array就是纯静态的数组，一旦初始化容量就不可以改变。
2. 类型安全上，ArrayList不支持基本数据类型，但支持泛型，能够进行泛型检查，所以在编译期可以保证数据类型的安全。
3. 数组维度上，由于ArrayList只是封装了一个一维数组，不支持多维的。但是array可以支持多维。
4. 方法上，ArrayList作为Java集合类封装了很多的方法，但是数组类型基本没有什么扩展方法供我们调用，只有一个array.length()方法可以获取数组长度。

> 相似之处
>
> 它们都支持NULL值，都支持随机存取

## 说一下ArrayList 与LinkedList的区别

1. 底层数据结构不同。ArrayLIs底层是数组，LinkedLst底层是双向链表，Node内部类。
2. 应用场景不同。ArrayList随机访问，但掺入、删除要移动大量元素，效率低。LinkedList插入、删除快速，但是检索访问时必须从first或last去一个一个检索。





## 说一下ArrayList与Vector的区别



> 相同点
>
> 继承结构一模一样，都是LIst接口的子类，底层都是操作数组，elementData

1. 线程安全。Vector类中的方法都是用synchronized修饰的，所以线程安全，ArrayList则没有做到这一点，线程不安全，

2. 扩容因子不同。ArrayList自动扩容机制更加复杂，（如果手动设置的minCapacity小于自动增长的情况才会采取自动增长），并且默认自动增长是以1.5倍扩容。然而，Vector就比较简单粗暴，可以通过构造器传入一个自定义的扩容因子，每次扩容时新数组大小=原数组大小+扩容因子。如果构建Vector实例时不指定扩容因子，Vector默认以两倍大小自动扩容。

3. 初始化的容量。ArrayList还是更加复杂，内部这是了两个常量空数组Empty_elementData、defaultCapacity_Empty_ElementDara,其实初始容量在不指定容量参数时都是空（也就是0），但是再添加add第一个元素时，才会根据具体条件决定扩容至多少或者10。

   但是Vector还是简单粗暴，在不指定初始化容量参数时，都默认给你创建一个10大小的数组。



> 注意：
>
> 第一：Vector出现在JDK 1.0,而 ArrayList出现在JDK 1.2，所以ArrayList的某些机制也更为复杂，
>
> 第二：注意别频繁扩容。扩容的原理其实就是将原数组拷贝到一个容量更大的新数组，拷贝时也会有一定的开销。所以尽量预估自己的数据量有多少，避免频繁的扩容。
>
> 记住扩容的两个重要方法名、 EnsureCapacity、grow















<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702111124.png" alt="在这里插入图片描述" style="zoom:100%;" />







## 2. Set
**Set接口介绍**

- 无序：元素存储和取出没有顺序
- 遍历：没有索引、只能通过迭代器 iterator 或增强 for循环遍历
- 不重复：不能存储重复元素

**实现类特点**

- HashSet
- LinkedHashSet
- TreeSet



### HashSet
**HashSet 集合的特点**

- 底层数据结构是哈希表
- 对集合的迭代顺序不作任何保证，也就是说不保证存储和取出的元素顺序一致
- 没有带索引的方法，所以不能使用普通 for循环遍历
- 由于是 Set集合，所以是不包含重复元素的集合
- HashSet底层数据结构是哈希表：是**数组与链表结合，数组的每一个元素就是一个链表**，综合了数组查找快和链表增删方便的好处。

**HashSet 集合的基本使用**

```java
HashSet<String> hs = new HashSet<String>();
```

**HashSet集合保证元素唯一性源码分析**

<font color= 3CB371>1.根据对象的哈希值计算存储位置</font>

- 如果当前位置没有元素则直接存入
- 如果当前位置有元素存在，则进入第二步

<font color= 3CB371> .2.当前元素的元素和已经存在的元素比较哈希值</font>
- 如果哈希值不同，则将当前元素进行存储
- 如果哈希值相同，则进入第三步


<font color= 3CB371>3.通过equals()方法比较两个元素的内容</font>
- 如果内容不相同，则将当前元素进行存储
- 如果内容相同，则不存储当前元素



### LinkedHashSet

**LinkedHashSet 集合特点**
- 哈希表和链表实现的 Set接口，具有可预测的迭代次序
- 由链表保证元素有序，也就是说元素的存储和取出顺序是一致的
- 由哈希表保证元素唯一，也就是说没有重复的元素


### TreeSet

**TreeSet 集合特点**
- 元素有序，可以按照一定的规则进行排序，具体排序方式取决于构造方法
- TreeSet() ：根据其元素的自然排序进行排序	
- TreeSet(Comparator comparator)  ：根据指定的比较器进行排序

- **没有带索引的方法**，所以不能使用普通 for循环遍历
- 由于是 Set集合，所以**不包含重复元素**的集合


**TreeSet的add()源码解析**

底层是二叉树结构

 - <font color=#FF4500>.第一个元素作为根节点，和后面的元素比较，小的放左边，大的放右边，相同的不放</font>
- 在二叉树结构下，排序又依赖于comparable接口的`compareTo`方法，按照指定规则强行给对象比大小，然后存入树中。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702111126.png" alt="在这里插入图片描述" style="zoom:100%;" />
**Integer自然排序演示**

Integer已经实现comparable接口并重写compareTo()方法
另外，String也重写了compareTo()方法，根据字典顺序排序字符串

```java
public class TreeSetDemo01 {
  public static void main(String[] args) {
    //创建集合对象
    TreeSet<Integer> ts = new TreeSet<Integer>();//默认无参构造方法，自然排序
    //添加元素
    ts.add(10);
    ts.add(40);
    ts.add(30);
    ts.add(50);
    ts.add(20);
    ts.add(30);
    //遍历集合
    for(Integer i : ts) {
      System.out.println(i);//10 20 30 40 50 
   }
 }
}
```
**存储自定义对象并遍历练习**
Student类实现comparable接口并重写compareTo()方法

```java
@Override
    public int compareTo(Student s) {

        int num = this.age - s.age;
        int num2 = num == 0 ? this.name.compareTo(s.name) : num;
        return num2;
    }
```
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702111127.png" alt="在这里插入图片描述" style="zoom:100%;" />

**自定义排序比较器**
`TreeSet(Comparator<? super E> comparator)`：比较器排序（集合具备比较性）

方式一：创建Mycomparator类实现comparator接口，重写compare()方法：比较条件和自然排序一样写

方式二：如果只用一次，不另外创建比较器，直接用`匿名对象`传参（开发常用）

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702111128.png" alt="在这里插入图片描述" style="zoom:100%;" />
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702111129.png" alt="在这里插入图片描述" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702143325.png" alt="在这里插入图片描述" style="zoom:100%;" />







**总结 TreeSet集合**
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702111130.png" alt="在这里插入图片描述" style="zoom:100%;" />

# 二、Map接口

## 1. 介绍

**Map 接口**
MAP的键要保证唯一性，所以在用自定义的类做键时，一定要重写两个方法：`hashmap`  `equals`方法


- 键值对映射关系
- 一个键对应一个值
- 键不能重复，值可以重复
- 元素存取无序

**实现类特点：**

- HashMap 
- LinkedHashMap
- TreeMap
- ConcurrentHashMap



## 2. Map的四种遍历方式

map的另一种遍历方式：foreach+正则表达式

```java
map.forEach((k,v) -> {
    System.out.println("key=" + k + ",value=" + v);
});
```



**(方式1)：通过key找value**

-  map.keySet();
- map.get(key);

```java

    Set<String> keySet = map.keySet();

    for (String key : keySet) {
      String value = map.get(key);
      System.out.println(key + "," + value);
   }
 }
```




**(方式2)：获取所有EntrySet实体集合**

- Set<Map.Entry<K,V>> entrySet() ：获取所有键值对实体的集合
- 用增强 for实现，得到每一个Map.Entry	
- 根据Map.Entry获取键和值
	-  getKey()
	-  getValue()




```java
	//创建集合对象
    Map<String, String> map = new HashMap<String, String>();
	//获取所有键值对对象的集合
    Set<Map.Entry<String, String>> entrySet = map.entrySet();
    //遍历键值对对象的集合，得到每一个键值对对象
    for (Map.Entry<String, String> me : entrySet) {
      //根据键值对对象获取键和值
      String key = me.getKey();
      String value = me.getValue();
      System.out.println(key + "," + value);
   }
```

**(方式3)：实体的迭代器**

```java
//得到键值对实体的set集合
Set<Map.Entry<String, String>> entries = map.entrySet();
//得到set集合的迭代器
Iterator<Map.Entry<String, String>> iterator = entries.iterator();
//使用迭代器遍历每个实体
while (iterator.hasNext()){
    Map.Entry<String, String> entry = iterator.next();
    String key = entry.getKey();
    String value = entry.getValue();
    System.out.println("key=" + key + ",value=" + value);

}
```





**(方式4)：分别获得keys、values集合**

- map.keySet();   //获得所有keys
- map.values();   //获得所有values

```java
 Map<String, String> map = new HashMap<String, String>();
 map.put("key1","value1");
 map.put("key2","value2");
 map.put("key3","value3");
 map.put("key4","value4");

 Set<String> keySet = map.keySet();//获得所有keys
 Collection<String> values = map.values();//获得所有values
 System.out.println("=======keys=========");
 for (String key : keySet){
     System.out.println(key);
 }
 System.out.println("=======values=========");
for (String value : values){
    System.out.println(value);
}
```






## HashMap
HashMap是数组+链表or红黑树的结构

HashMap的几个案例（存储不同的键和值类型）

- HashMap<String, String>

- HashMap<Integer, String>

- HashMap<String, Student>

- HashMap<Student, String>

  

> 由于字符串、包装类、简单类型的不变特性，非常适合于做map的键，防止发生地址的改变而引起Map集合的混乱。
>
> `键为student对象，值为String`。所谓==键相同，值覆盖==的特性（区别于Set添加重复元素时是不会覆盖的）
>
> 底层必须借助哈希表保证key的唯一性，也就是student对象的唯一性，student类必须重写hashCode()和equals()。
>
> 引申一点：为什么重写 equals() 方法时，必须重写hashCode()方法？
>
> 当我们向一个Hash结构的集合中添加某个元素，集合会首先调用hashCode方法，这样就可以直接定位它所存储的位置，若该处没有其他元素，则直接保存。若该处已经有元素存在，就调用equals方法来匹配这两个元素是否相同，相同则不存，不同则链到后面（如果是链地址法）。
>
> 如果只重写equals()方法，我们可以new 两个数据一样的对象进行测试。



**练习：集合嵌套之ArrayList嵌套HashMap**

```java
//创建ArrayList集合
    ArrayList<HashMap<String, String>> array = new
ArrayList<HashMap<String, String>>();

 //创建HashMap集合，并添加键值对元素
    HashMap<String, String> hm1 = new HashMap<String, String>();
    hm1.put("孙策", "大乔");
    hm1.put("周瑜", "小乔");
    
 HashMap<String, String> hm2 = new HashMap<String, String>();
    hm2.put("郭靖", "黄蓉");
    hm2.put("杨过", "小龙女"); 
      
  HashMap<String, String> hm3 = new HashMap<String, String>();
    hm3.put("令狐冲", "任盈盈");
    hm3.put("林平之", "岳灵珊");  
    
    /把HashMap作为元素添加到ArrayList集合
    array.add(hm1);
    array.add(hm2);
    array.add(hm3);
    
   //遍历ArrayList集合
    for (HashMap<String, String> hm : array) {
      Set<String> keySet = hm.keySet();
      for (String key : keySet) {
        String value = hm.get(key);
        System.out.println(key + "," + value);
     } 
    
```


**练习：集合嵌套之HashMap嵌套ArrayList**

```java
//创建HashMap集合
    HashMap<String, ArrayList<String>> hm = new HashMap<String,
ArrayList<String>>();
//创建ArrayList集合，并添加元素
    ArrayList<String> sgyy = new ArrayList<String>();
    sgyy.add("诸葛亮");
    sgyy.add("赵云");

ArrayList<String> xyj = new ArrayList<String>();
    xyj.add("唐僧");
    xyj.add("孙悟空");
    
     ArrayList<String> shz = new ArrayList<String>();
    shz.add("武松");
    shz.add("鲁智深");
    
    
    //把ArrayList作为元素添加到HashMap集合
    hm.put("三国演义",sgyy);
     hm.put("西游记",xyj);
     hm.put("水浒传",shz);
     
     
      //遍历HashMap集合
    Set<String> keySet = hm.keySet();
    for(String key : keySet) {
      System.out.println(key);
      ArrayList<String> value = hm.get(key);
      for(String s : value) {
        System.out.println("\t" + s);
     }
   }
```


**题型：统计字符串中每个字符出现的次数**
案例需求
- 键盘录入一个字符串，要求统计字符串中每个字符串出现的次数。
- 举例：键盘录入 “aababcabcdabcde” 在控制台输出：“a(5)b(4)c(3)d(2)e(1)”
代码实现

```java
public class CharCountDemo {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入字符串: ");
        String line = sc.nextLine();

        //创建一个map集合，键是Character，值是Integer(HashMap,TreeMap类型都可)
        HashMap<Character,Integer> map = new HashMap<Character,Integer>();
       // TreeMap<Character,Integer> map = new TreeMap<>();
        
        //遍历字符串的每一个字符，判断该字符在map中是否存在该键
        for (int i = 0; i < line.length(); i++){
            char key = line.charAt(i);
            Integer value = map.get(key);
            //如果返回值是null：说明该字符在Map集合中不存在，就把该字符作为键， 1 作为值存储
            if (value == null){
                map.put(key,1);
            }else{
                //如果返回值不是null：说明该字符在Map集合中存在，把该值加1，然后重新存储该字符和对应的值
                value++;
                map.put(key,value);
            }
        }

        //遍历map集合
        StringBuilder sb = new StringBuilder();
        Set<Character> keys = map.keySet();
        for (Character key : keys){
            Integer value = map.get(key);
            sb.append(key).append("(").append(value).append(")");
        }
        System.out.println(sb.toString());
    }
}

```

## LinkedHashMap
Map接口的哈希表和链表列表实现，具有可预知的迭代顺序
- 哈希表保证唯一性
- 链表保证有序性

## Hashtable

Hashtable类的源码分析，博客在这里：http://blog.csdn.net/u010412719/article/details/51972602

（和HashMap几乎一样，被HashMap替代了）

**Hashtable与HashMap的区别**

- HashMap：线程不安全，效率高，允许null键和null值
- Hashtable：线程安全，效率低，不允许null键和null值


## TreeMap
[https://www.jianshu.com/p/e11fe1760a3d]()

[《Java源码分析》：TreeMap](_https://blog.csdn.net/u010412719/article/details/52425512)

- TreeMap存储K-V键值对，通过红黑树（R-B tree）实现；
- TreeMap继承了NavigableMap接口，NavigableMap接口继承了SortedMap接口，可支持一系列的导航定位以及导航操作的方法，当然只是提供了接口，需要TreeMap自己去实现；
- TreeMap实现了Cloneable接口，可被克隆，实现了Serializable接口，可序列化；
- TreeMap因为是通过红黑树实现，红黑树结构天然支持排序，默认情况下通过Key值的自然顺序进行排序；



## 三种Map子类集合的对比

- `HashMap`可实现快速存储和检索，但其缺点是其包含的元素是`无序的`，这导致它在存在大量迭代的情况下表现不佳。
- `LinkedHashMap`保留了HashMap的优势，且其包含的元素是`有序的`。它在有大量迭代的情况下表现更好。
- `TreeMap`能便捷的实现对其内部元素的`各种排序`，但其一般性能比前两种map差。

**LinkedHashMap映射减少了HashMap排序中的混乱，且不会导致TreeMap的性能损失。**


<br>

<br>

## ConcurrentHashMap
链接：[https://www.jianshu.com/p/d0b37b927c48]()
**HashMap线程不安全**
因为多线程环境下，使用HashMap进行put操作可能会引起死循环，导致CPU利用率接近100%，所以在并发情况下不能使用HashMap。

**Hashtable线程安全但效率低下**
Hashtable容器使用synchronized来保证线程安全，但在线程竞争激烈的情况下Hashtable的效率非常低下。因为当一个线程访问Hashtable的同步方法时，其他线程访问Hashtable的同步方法时，可能会进入阻塞或轮询状态。如线程1使用put进行添加元素，线程2不但不能使用put方法添加元素，并且也不能使用get方法来获取元素，所以竞争越激烈效率越低。


> Hashtable
> 的任何操作都会把`整个表锁住`，是阻塞的。好处是总能获取最实时的更新，比如说线程A调用putAll写入大量数据，期间线程B调用get，线程B就会被阻塞，直到线程A完成putAll，因此线程B肯定能获取到线程A写入的完整数据。坏处是所有调用都要排队，效率较低。
>
> ---
>
> ConcurrentHashMap`分段锁`
> 是设计为非阻塞的。在更新时会局部锁住某部分数据，但不会把整个表都锁住。同步读取操作则是完全非阻塞的。好处是在保证合理的同步前提下，效率很高。坏处是严格来说读取操作不能保证反映最近的更新。例如线程A调用putAll写入大量数据，期间线程B调用get，则只能get到目前为止已经顺利插入的部分数据。
> 应该根据具体的应用场景选择合适的HashMap。





---
# 三、Collections 集合工具类


方法名 说明
- 排序：`public static <T> void sort(List<T> list)` 
默认情况下是自然顺序，所以自定义对象要实现Comparable接口
- 反转 : `public static void reverse(List<?> list)` 反转指定列表中元素的顺序
- 随机置换 : `public static void shuffle(List<?> list)` 使用默认的随机源随机排列指定的列表
- 二分查找：`public static <T> int binarySearch(List<?> list, T key)`
- 最大值：`public static <T> max(Collection<?> coll)`



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200703181352.png" alt="image-20200703181351194" style="zoom:100%;" />

**案例：使用工具类对arraylist进行排序**
```java

{

    ArrayList<Student> array = new ArrayList<Student>();

    //创建学生对象
    Student s1 = new Student("lin", 30);
    Student s2 = new Student("wang", 32);
    Student s3 = new Student("liu", 33);
    Student s4 = new Student("zhang", 33);

    //把学生对象添加到集合
    array.add(s1);
    array.add(s2);
    array.add(s3);
    array.add(s4);

    //使用Collections 对ArrayList集合进行排序
    Collections.sort(array, new Comparator<Student>() {
        @Override
        public int compare(Student s1, Student s2) {
            //先按年龄排序，后按名字排序
            int num = s1.getAge() - s2.getAge();
            int num2 = num == 0 ? s1.getName().compareTo(s2.getName()) : num;
            return num2;
        }
    });
    //遍历集合
    for (Student s : array) {
        System.out.println(s.getName() + "," + s.getAge());
    }
}
```


**案例：斗地主**
```java

public class PokerDemo {
    public static void main(String[] args) {
        //创建HashMap，键是编号，值是牌
        HashMap<Integer,String> hm =new HashMap<Integer, String>();

        //创建ArrayList，存储编号
        ArrayList<Integer> array =new ArrayList<Integer>();

        //创建花色数组和点数数组
        String[] colors={"♦", "♣", "♥", "♠"};
        String[] numbers={"3", "4", "5", "6", "7", "8", "9", "10", "J", "Q",
                "K", "A", "2"};
        //从0开始往HashMap里面存储编号，并存储对应的牌。同时往ArrayList里面存储编号
        int index=0;
        for(String number:numbers){
            for (String color:colors){
                hm.put(index,color+number);
                array.add(index);
                index++;
            }
        }
        hm.put(index,"小王");
        array.add(index);
        index++;
        hm.put(index,"大王");
        array.add(index);
        
        //洗牌(洗的是编号)，用Collections的shuffle()方法实现
        Collections.shuffle(array);

        //发牌(发的也是编号，为了保证编号是排序的，创建TreeSet集合接收)
        TreeSet<Integer> lqxSet=new TreeSet<Integer>();
        TreeSet<Integer> lySet = new TreeSet<Integer>();
        TreeSet<Integer> fqySet = new TreeSet<Integer>();
        TreeSet<Integer> dpSet = new TreeSet<Integer>();//底牌

        for(int i=0;i<array.size();i++){
            int x=array.get(i);
            if(i>=array.size()-3){
                dpSet.add(x);
            }else if (i%3==0){
                lqxSet.add(x);
            }else if(i%3==1){
                lySet.add(x);
            }else if(i%3==2){
                fqySet.add(x);
            }
        }
        //调用看牌方法
        lookPoker("林青霞", lqxSet, hm);
        lookPoker("柳岩", lySet, hm);
        lookPoker("风清扬", fqySet, hm);
        lookPoker("底牌", dpSet, hm);




    }

    //定义方法看牌(遍历TreeSet集合，获取编号，到HashMap集合找对应的牌)
    public static void lookPoker(String name,TreeSet<Integer> ts,HashMap<Integer,String> hm){

        System.out.println(name+"的牌是：");

        for (Integer key:ts){
            String poker=hm.get(key);
            System.out.print(poker+" ");
        }
        System.out.println();

    }
}

```







# 四、Java集合相关面试题
以下内容是我在网上搜集的，仅供参考

**集合：**
1: HashMap和Hashtable的区别。

2:Collection 和 Collections的区别。

3: List, Set, Map是否继承自Collection接口?

4:说出ArrayList,Vector, LinkedList的存储性能和特性？

5:你所知道的集合类都有哪些？主要方法？







- <font color=red>Java 集合框架的基础接口有哪些？</font>

> Collection ，为集合层级的根接口。一个集合代表一组对象，这些对象即为它的元素。Java 平台不提供这个接口任何直接的实现。 
> - Set ，是一个不能包含重复元素的集合。这个接口对数学集合抽象进行建模，被用来代表集合，就如一副牌。
> -  List ，是一个有序集合，可以包含重复元素。你可以通过它的索引来访问任何元素。List 更像长度动态变换的数组。
>
>   Map ，是一个将 key 映射到value 的对象。一个 Map 不能包含重复的 key，每个 key 最多只能映射一个 value 。 
>   一些其它的接口有 Queue、Dequeue、SortedSet、SortedMap 和 ListIterator 。








-  <font color=red>Collection 和 Collections 的区别？</font>

> Collection ，是集合类的上级接口，继承与他的接口主要有 Set 和List 。 
> Collections ，是针对集合类的一个工具类，它提供一系列静态方法实现对各种集合的搜索、排序、线程安全化等操作。
>
> 






- <font color=red>集合框架底层数据结构总结</font>



> 1）List
>
> ArrayList ：Object 数组。 
> Vector ：Object 数组。 
>  LinkedList ：双向链表(JDK6 之前为循环链表，JDK7 取消了循环)。
>
> 
>
> 2）Map
>
> HashMap ：
> JDK8 之前，HashMap 由数组+链表组成的，数组是HashMap的主体，链表则是主要为了解决哈希冲突而存在的（“拉链法”解决冲突）。
> JDK8 以后，在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为 8 ）时，将链表转化为红黑树，以减少搜索时间。
>
> LinkedHashMap ：LinkedHashMap 继承自 HashMap，所以它的底层仍然是基于拉链式散列结构即由数组和链表或红黑树组成。另外，LinkedHashMap 在上面结构的基础上，增加了一条双向链表，使得上面的结构可以保持键值对的插入顺序。同时通过对链表进行相应的操作，实现了访问顺序相关逻辑。详细可以查看：《LinkedHashMap 源码详细分析（JDK1.8）》 。
>
> Hashtable ：数组+链表组成的，数组是 HashMap 的主体，链表则是主要为了解决哈希冲突而存在的。
>
> TreeMap ：红黑树（自平衡的排序二叉树）。
>
> 
>
> 3）Set
>
> HashSet ：无序，唯一，基于 HashMap 实现的，底层采用 HashMap 来保存元素。
> LinkedHashSet ：LinkedHashSet 继承自 HashSet，并且其内部是通过 LinkedHashMap 来实现的。有点类似于我们之前说的LinkedHashMap 其内部是基于 HashMap 实现一样，不过还是有一点点区别的。
> TreeSet ：有序，唯一，红黑树(自平衡的排序二叉树)。





- <font color=red>什么是迭代器(Iterator)？</font>

Iterator 接口，提供了很多对集合元素进行迭代的方法。每一个集合类都包含了可以返回迭代器实例的迭代方法。迭代器可以在迭代的过程中删除底层集合的元素，但是不可以直接调用集合的 `#remove(Object Obj)` 方法删除，可以通过迭代器的 `#remove()` 方法删除。

? **Iterator 和 ListIterator 的区别是什么？**

- Iterator 可用来遍历 Set 和 List 集合，但是 ListIterator 只能用来遍历 List。
- Iterator 对集合只能是前向遍历，ListIterator 既可以前向也可以后向。
- ListIterator 实现了 Iterator 接口，并包含其他的功能。比如：增加元素，替换元素，获取前一个和后一个元素的索引等等。





-  <font color=red>快速失败（fail-fast）和安全失败（fail-safe）的区别是什么？</font>

差别在于 ConcurrentModification 异常：

- 快速失败：当你在迭代一个集合的时候，如果有另一个线程正在修改你正在访问的那个集合时，就会抛出一个 ConcurrentModification 异常。 在 `java.util` 包下的都是快速失败。
- 安全失败：你在迭代的时候会去底层集合做一个拷贝，所以你在修改上层集合的时候是不会受影响的，不会抛出 ConcurrentModification 异常。在 `java.util.concurrent` 包下的全是安全失败的。





- <font color=red> 如何删除 List 中的某个元素？ </font>

有两种方式，分别如下：

- 方式一，使用 Iterator ，顺序向下，如果找到元素，则使用迭代器的 remove 方法进行移除。
- 方式二，倒序遍历 List ，如果找到元素，则使用 remove 方法进行移除。（防止索引乱序）







- <font color=red>Enumeration 和 Iterator 接口有什么不同？</font>

- Enumeration 跟 Iterator 相比较快两倍，而且占用更少的内存。
- 但是，Iterator 相对于 Enumeration 更安全，因为其他线程不能修改当前迭代器遍历的集合对象。同时，Iterators 允许调用者从底层集合中移除元素，这些 Enumerations 都没法完成。

对于很多胖友，可能并未使用过 Enumeration 类，所以可以看看 [《Java Enumeration 接口》](http://www.runoob.com/java/java-enumeration-interface.html) 文章。





- <font color=red>为何 Iterator 接口没有具体的实现？</font>

Iterator 接口，定义了遍历集合的方法，但它的实现则是集合实现类的责任。每个能够返回用于遍历的 Iterator 的集合类都有它自己的 Iterator 实现内部类。

这就允许集合类去选择迭代器是 fail-fast 还是 fail-safe 的。比如，ArrayList 迭代器是 fail-fast 的，而 CopyOnWriteArrayList 迭代器是 fail-safe 的。(CopyOnWriteArrayList 就是在底层将数组拷贝一份)





- <font color=red>Comparable 和 Comparator 的区别?</font>

- Comparable 接口，在 `java.lang` 包下，用于当前对象和其它对象的比较，所以它有一个 `#compareTo(Object obj)` 方法用来排序，该方法只有一个参数。
- Comparator 接口，在 `java.util` 包下，用于传入的两个对象的比较，所以它有一个 `#compare(Object obj1, Object obj2)` 方法用来排序，该方法有两个参数。

详细的，可以看看 [《Java 自定义比较器》](https://blog.csdn.net/whing123/article/details/77851737) 文章，重点是如何自己实现 Comparable 和 Comparator 的方法。

> ? **compareTo 方法的返回值表示的意思？**
>
> - 大于 0 ，表示对象大于参数对象。
> - 小于 0 ，表示对象小于参数对象
> - 等于 0 ，表示两者相等。





- <font color=red>如何对 Object 的 List 排序？</font>

- 对 `Object[]` 数组进行排序时，我们可以用 `Arrays#sort(...)` 方法。
- 对 `List<Object>` 数组进行排序时，我们可以用 `Collections#sort(...)` 方法。





- <font color=red>List 和 Set 区别？</font>

List，Set 都是继承自 Collection 接口。

- List 特点：元素有放入顺序，元素可重复。
- Set 特点：元素无放入顺序，元素不可重复，重复元素会覆盖掉。

> 注意：元素虽然无放入顺序，但是元素在 Set 中的位置是有该元素的 hashcode 决定的，其位置其实是固定的。
>
> 另外 List 支持 `for` 循环，也就是通过下标来遍历，也可以用迭代器，但是 Set 只能用迭代，因为他无序，无法用下标来取得想要的值。

Set 和 List 对比：

- Set：检索元素效率高，删除和插入效率低，插入和删除不会引起元素位置改变。
- List：和数组类似，List 可以动态增长，查找元素效率低，插入删除元素效率，因为可能会引起其他元素位置改变。





- <font color=red> ArrayList 与 LinkedList 区别？</font>

? **ArrayList**

- 优点：ArrayList 是实现了基于动态数组的数据结构，因为地址连续，一旦数据存储好了，查询操作效率会比较高（在内存里是连着放的）。
- 缺点：因为地址连续，ArrayList 要移动数据，所以插入和删除操作效率比较低。

? **LinkedList**

- 优点：LinkedList 基于链表的数据结构，地址是任意的，所以在开辟内存空间的时候不需要等一个连续的地址。对于新增和删除操作 add 和 remove ，LinedList 比较占优势。LinkedList 适用于要头尾操作或插入指定位置的场景。
- 缺点：因为 LinkedList 要移动指针，所以查询操作性能比较低。





- <font color=red> ArrayList 是如何扩容的？</font>

直接看 [《ArrayList 动态扩容详解》](https://www.cnblogs.com/kuoAT/p/6771653.html) 文章，很详细。主要结论如下：

- 如果通过无参构造的话，初始数组容量为 0 ，当真正对数组进行添加时，才真正分配容量。每次按照 **1.5** 倍（位运算）的比率通过 copeOf 的方式扩容。
- 在 JKD6 中实现是，如果通过无参构造的话，初始数组容量为10，每次通过 copeOf 的方式扩容后容量为原来的 **1.5** 倍。

> 重点是 1.5 倍扩容，这是和 HashMap 2 倍扩容不同的地方。





<font color=red>ArrayList 与 Vector 区别？</font>

ArrayList 和 Vector 都是用数组实现的，主要有这么三个区别：

- 1、Vector 是多线程安全的，线程安全就是说多线程访问同一代码，不会产生不确定的结果，而 ArrayList 不是。这个可以从源码中看出，Vector 类中的方法很多有 `synchronized` 进行修饰，这样就导致了 Vector 在效率上无法与 ArrayList 相比。

  > Vector 是一种老的动态数组，是线程同步的，效率很低，一般不赞成使用。

- 2、两个都是采用的线性连续空间存储元素，但是当空间不足的时候，两个类的增加方式是不同。

- 3、Vector 可以设置增长因子，而 ArrayList 不可以。

适用场景分析：

- 1、Vector 是线程同步的，所以它也是线程安全的，而 ArrayList 是线程无需同步的，是不安全的。如果不考虑到线程的安全因素，一般用 ArrayList 效率比较高。

  > 实际场景下，如果需要多线程访问安全的数组，使用 CopyOnWriteArrayList 。

- 2、如果集合中的元素的数目大于目前集合数组的长度时，在集合中使用数据量比较大的数据，用 Vector 有一定的优势。

  > 这种情况下，使用 LinkedList 更合适。





<font color=red>HashMap 和 Hashtable 的区别？</font>

> Hashtable 是在 Java 1.0 的时候创建的，而集合的统一规范命名是在后来的 Java2.0 开始约定的，而当时其他一部分集合类的发布构成了新的集合框架。

- Hashtable 继承 Dictionary ，HashMap 继承的是 Java2 出现的 Map 接口。
- 2、HashMap 去掉了 Hashtable 的 contains 方法，但是加上了 containsValue 和 containsKey 方法。
- 3、HashMap 允许空键值，而 Hashtable 不允许。
- 【重点】4、HashTable 是同步的，而 HashMap 是非同步的，效率上比 HashTable 要高。也因此，HashMap 更适合于单线程环境，而 HashTable 适合于多线程环境。
- 5、HashMap 的迭代器（Iterator）是 fail-fast 迭代器，HashTable的 enumerator 迭代器不是 fail-fast 的。
- 6、HashTable 中数组默认大小是 11 ，扩容方法是 `old * 2 + 1` ，HashMap 默认大小是 16 ，扩容每次为 2 的指数大小。

一般现在不建议用 HashTable 。主要原因是两点：

- 一是，HashTable 是遗留类，内部实现很多没优化和冗余。
- 二是，即使在多线程环境下，现在也有同步的 ConcurrentHashMap 替代，没有必要因为是多线程而用 Hashtable 。

? **Hashtable 的 #size() 方法中明明只有一条语句 “return count;” ，为什么还要做同步？**

同一时间只能有一条线程执行固定类的同步方法，但是对于类的非同步方法，可以多条线程同时访问。所以，这样就有问题了，可能线程 A 在执行 Hashtable 的 put 方法添加数据，线程 B 则可以正常调用 `#size()` 方法读取 Hashtable 中当前元素的个数，那读取到的值可能不是最新的，可能线程 A 添加了完了数据，但是没有对 `count++` ，线程 B 就已经读取 `count` 了，那么对于线程 B 来说读取到的 `count` 一定是不准确的。

**而给 #size() 方法加了同步之后，意味着线程 B 调用 #size() 方法只有在线程 A 调用 put 方法完毕之后才可以调用，这样就保证了线程安全性**。



<font color=red>HashSet 和 HashMap 的区别？</font>

- Set 是线性结构，值不能重复。HashSet 是 Set 的 hash 实现，HashSet 中值不能重复是用 HashMap 的 key 来实现的。

- Map 是键值对映射，可以空键空值。HashMap 是 Map 的 hash 实现，key 的唯一性是通过 key 值 hashcode 的唯一来确定，value 值是则是链表结构。

  > 因为不同的 key 值，可能有相同的 hashcode ，所以 value 值需要是链表结构。

他们的共同点都是 hash 算法实现的唯一性，他们都不能持有基本类型，只能持有对象。

> 为了更好的性能，Netty 自己实现了 key 为基本类型的 HashMap ，例如 [IntObjectHashMap](https://netty.io/4.1/api/io/netty/util/collection/IntObjectHashMap.html) 。

> **如何决定选用 HashMap 还是 TreeMap？**
>
> - 对于在 Map 中插入、删除和定位元素这类操作，HashMap 是最好的选择。
> - 然而，假如你需要对一个有序的 key 集合进行遍历， TreeMap 是更好的选择。
>
> 基于你的 collection 的大小，也许向 HashMap 中添加元素会更快，再将 HashMap 换为 TreeMap 进行有序 key 的遍历。







<font color=red>HashMap 的工作原理是什么？</font>

我们知道在 Java 中最常用的两种结构是数组和模拟指针（引用），几乎所有的数据结构都可以利用这两种来组合实现，HashMap 也是如此。实际上 HashMap 是一个**“链表散列”**。

HashMap 是基于 hashing 的原理。

[HashMap 图解](http://dl.iteye.com/upload/attachment/177479/3f05dd61-955e-3eb2-bf8e-31da8a361148.jpg)

- 我们使用 `#put(key, value)` 方法来存储对象到 HashMap 中，使用 `get(key)` 方法从 HashMap 中获取对象。
- 当我们给 `#put(key, value)` 方法传递键和值时，我们先对键调用 `#hashCode()` 方法，返回的 hashCode 用于找到 bucket 位置来储存 Entry 对象。

? **当两个对象的 hashCode 相同会发生什么？**

因为 hashcode 相同，所以它们的 bucket 位置相同，“碰撞”会发生。

因为 HashMap 使用链表存储对象，这个 Entry（包含有键值对的 Map.Entry 对象）会存储在链表中。

? **hashCode 和 equals 方法有何重要性？**

HashMap 使用 key 对象的 `#hashCode()` 和 `#equals(Object obj)` 方法去决定 key-value 对的索引。当我们试着从 HashMap 中获取值的时候，这些方法也会被用到。

- 如果这两个方法没有被正确地实现，在这种情况下，两个不同 Key 也许会产生相同的 `#hashCode()` 和 `#equals(Object obj)` 输出，HashMap 将会认为它们是相同的，然后覆盖它们，而非把它们存储到不同的地方。

同样的，所有不允许存储重复数据的集合类都使用 `#hashCode()` 和 `#equals(Object obj)` 去查找重复，所以正确实现它们非常重要。`#hashCode()` 和 `#equals(Object obj)` 方法的实现，应该遵循以下规则：

- 如果 `o1.equals(o2)` ，那么 `o1.hashCode() == o2.hashCode()` 总是为 `true` 的。
- 如果 `o1.hashCode() == o2.hashCode()` ，并不意味 `o1.equals(o2)` 会为 `true` 。



<font color=red> 我们能否使用任何类作为 Map 的 key？</font>

我们可以使用任何类作为 Map 的 key ，然而在使用它们之前，需要考虑以下几点：

- 1、如果类重写了 equals 方法，它也应该重写 hashcode 方法。

- 2、类的所有实例需要遵循与 equals 和 hashcode 相关的规则。

- 3、如果一个类没有使用 equals ，你不应该在 hashcode 中使用它。

- 4、用户自定义 key 类的最佳实践是使之为不可变的，这样，hashcode 值可以被缓存起来，拥有更好的性能。不可变的类也可以确保hashcode 和 equals 在未来不会改变，这样就会解决与可变相关的问题了。

  > 比如，我有一个 类MyKey ，在 HashMap 中使用它。代码如下：

  ```
  //传递给MyKey的name参数被用于equals()和hashCode()中
  MyKey key = new MyKey('Pankaj'); //assume hashCode=1234
  myHashMap.put(key, 'Value');
  // 以下的代码会改变key的hashCode()和equals()值
  key.setName('Amit'); //assume new hashCode=7890
  //下面会返回null，因为HashMap会尝试查找存储同样索引的key，而key已被改变了，匹配失败，返回null
  myHashMap.get(new MyKey('Pankaj'));
  
  12345678
  ```

  - 这就是为何 String 和 Integer 被作为 HashMap 的 key 大量使用。





<font color=red>HashSet 的工作原理是什么？</font>

HashSet 是构建在 HashMap 之上的 Set hashing 实现类。让我们直接撸下源码，代码如下：

**HashSet 如何检查重复？**

> 艿艿：正如我们上面看到 HashSet 的实现原理，我们自然可以推导出，HashMap 也是如何检查重复滴。

如下摘取自 《Head First Java》 第二版：

当你把对象加入 HashSet 时，HashSet会先计算对象的hashcode值来判断对象加入的位置，同时也会与其他加入的对象的hashcode值作比较。

- 如果没有相符的 hashcode ，HashSet会假设对象没有重复出现。
- 但是如果发现有相同 hashcode 值的对象，这时会调用 equals 方法来检查 hashcode 相等的对象是否真的相同。
  - 如果两者相同，HashSet 就不会让加入操作成功。
  - **如果两者不同，HashSet 就会让加入操作成功**。











