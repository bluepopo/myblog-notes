《Java源码分析》LinkedList：双向链表

# 一、介绍

## 1.继承结构

先看下LinkedList的继承体系结构。

```java
public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable
```

ArrayList类底层是借助于数组来实现，LinkedList由于实现了Deque接口，==则LinkedList类也具有双端队列的特性==。

LinkedList 是一个继承于AbstractSequentialList的**双向链表**。
LinkedList 可以被当作**堆栈、队列或双端队列**进行操作。
LinkedList 实现 List 接口，所以能对它进行**队列操作**。
LinkedList 实现 Deque 接口，能将LinkedList当作**双端队列**使用。
LinkedList 实现了Cloneable接口，即覆盖了函数**clone()**，能克隆。
LinkedList 实现java.io.Serializable接口，所以LinkedList**支持序列化**，能通过序列化去传输。
LinkedList 是非同步的。





## 2. 内部类：Node

下面就是Node类的定义。

![image-20200801160224384](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200801160225.png)

从源码可以看到，Node类只定义了数据元素、前驱指向、后继指向。这就是双向链表的节点的定义。

还有一个有参的构造函数，从参数中可以看出，创建一个新的Node对象时要提供新Node对象的前指针、本节点的value和后指针。

```java
 private static class Node<E> {
        E item;
        Node<E> next;
        Node<E> prev;

        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }
```





# 二、属性



```java
/**
 * 链表的大小
 */
transient int size = 0;

/**
 * 指向前一个结点
 */
transient Node<E> first;

/**
* 指向后一个结点
 */
transient Node<E> last;
```



# 三、构造器



```java
//无参构造函数，即构造一个空链表。

public LinkedList() {
}

//有参构造函数，参数是一个Collection，此函数首先调用无参构造函数构造了一个空对象，然后调用了addAll(c)方法
public LinkedList(Collection<? extends E> c) {
    this();
    addAll(c);
}
```



# 四、常用方法



## 增加：

*add(E e)*：在链表后添加一个元素；  通用方法
*addFirst(E e)*：在链表头部插入一个元素； 特有方法
*addLast(E e)*：在链表尾部添加一个元素； 特有方法
*push(E e)*：与addFirst方法一致 
*offer(E e)*：在链表尾部插入一个元素 

 *add(int index, E element)*：在指定位置插入一个元素。   
*offerFirst(E e)*：JDK1.6版本之后，在头部添加； 特有方法                                                     

*offerLast(E e)*：JDK1.6版本之后，在尾部添加； 特有方法

## 删除：

*remove()* ：移除链表中第一个元素;  通用方法 
*remove(E e)*：移除指定元素；  通用方法
*removeFirst(E e)*：删除头，获取元素并删除； 特有方法
*removeLast(E e)*：删除尾； 特有方法
*pollFirst()*：删除头； 特有方法
*pollLast()*：删除尾； 特有方法
*pop()*：和removeFirst方法一致，删除头。 
*poll()*：查询并移除第一个元素   特有方法   

## 查：

*get(int index)*：按照下标获取元素； 通用方法
*getFirst()*：获取第一个元素； 特有方法
*getLast()*：获取最后一个元素； 特有方法
*peek()*：获取第一个元素，但是**不移除**； 特有方法
*peekFirst()*：获取第一个元素，但是不移除； 
*peekLast()*：获取最后一个元素，但是不移除；
*pollFirst()*：查询并删除头； 特有方法
*pollLast()*：删除尾； 特有方法
*poll()*：查询并移除第一个元素   特有方法





# 五、详细源码



```java
package java.util;
import java.util.function.Consumer;
public class LinkedList<E>
        extends AbstractSequentialList<E>
        implements List<E>, Deque<E>, Cloneable, java.io.Serializable {
    /**
	 * 元素数量
	 */
    transient int size = 0;
 
    /**
	 * 首结点引用
	 */
    transient Node<E> first;
 
    /**
	 * 尾节点引用
	 */
    transient Node<E> last;
 
    /**
	 * 无参构造方法
	 */
    public LinkedList() {
    }
 
    /**
	 * 通过一个集合初始化，元素顺序有这个集合的迭代器返回顺序决定
	 * 
	 * @param c
	 *            其元素将被放入此列表中的集合
	 * @throws NullPointerException
	 *             如果指定的集合是空的
	 */
    public LinkedList(Collection<? extends E> c) {
        // 调用无参构造
        this();
        // 添加所有的元素
        addAll(c);
    }
 
    /**
	 * 头插，内部使用
	 */
    private void linkFirst(E e) {
        // 当前首结点
        final Node<E> f = first;
        // 构建新节点
        final Node<E> newNode = new Node<>(null, e, f);
        // 将newNode作为首节点
        first = newNode;
        // 如果原首节点为null，即原链表为null，则链表尾节点也设置为newNode
        if (f == null)
            last = newNode;
        else                // 否则，原首节点的prev设置为newNode
            f.prev = newNode;
        size++;             // 长度+1
        modCount++; // 修改次数+1
    }
 
    /**
	 * 尾插
	 */
    void linkLast(E e) {
        // 获取尾结点
        final Node<E> l = last;
        // 构建新节点
        final Node<E> newNode = new Node<>(l, e, null);
        // newNode作为尾节点
        last = newNode;
        // 如果原尾节点为null，即原链表为null，则链表首节点也设置为newNode
        if (l == null)
            first = newNode;
        else    // 否则，原尾节点的next设置为newNode
            l.next = newNode;
        size++;
        modCount++;
    }
 
    /**
	 * 在非空节点succ前插入节点值e
	 */
    void linkBefore(E e, Node<E> succ) {
        final Node<E> pred = succ.prev;
        // 构建新节点
        final Node<E> newNode = new Node<>(pred, e, succ);
        // 设置newNode为succ的前节点
        succ.prev = newNode;
        // 如果succ.prev为null，即succ为首节点，则将newNode设置为首节点
        if (pred == null)
            first = newNode;
        else        // 如果succ不是首节点
            pred.next = newNode;
        size++;
        modCount++;
    }
 
    /**
	 * 删除首结点，返回存储的元素，内部使用
	 */
    private E unlinkFirst(Node<E> f) {
        // 获取首结点存储的元素
        final E element = f.item;
        // 获取首结点的后继结点
        final Node<E> next = f.next;
        // 删除首结点
        f.item = null;
        f.next = null; // 便于垃圾回收
        // 首结点后继结点设为首结点
        first = next;
        // 如果原来首结点的后继结点为空，则尾结点设为null
        // 否则，原来首结点的后继结点的前驱结点设为null
        if (next == null)
            last = null;
        else
            next.prev = null;
        size--;
        modCount++;
        // 返回原来首结点存储的元素
        return element;
    }
 
    /**
	 * 删除尾结点，返回存储的元素，内部使用
	 */
    private E unlinkLast(Node<E> l) {
        // 获取尾结点存储的元素
        final E element = l.item;
        // 获取尾结点的前驱结点
        final Node<E> prev = l.prev;
        // 删除尾结点
        l.item = null;
        l.prev = null; // help GC
        // 原来尾结点的前驱结点设为尾结点
        last = prev;
        // 如果原来尾结点的前驱结点为空，则首结点设为null
        // 否则，原来尾结点的前驱结点的后继结点设为null
        if (prev == null)
            first = null;
        else
            prev.next = null;
        size--;
        modCount++;
        // 返回原来尾结点存储的元素
        return element;
    }
 
    /**
	 * 删除指定非空结点，返回存储的元素
	 */
    E unlink(Node<E> x) {
        // 获取指定非空结点存储的元素
        final E element = x.item;
        // 获取指定非空结点的后继结点
        final Node<E> next = x.next;
        // 获取指定非空结点的前驱结点
        final Node<E> prev = x.prev;
        /**
		 * 如果指定非空结点的前驱结点为空，则指定非空结点的后继结点设为首结点 
		 * 否则，指定非空结点的后继结点设为指定非空结点的前驱结点的后继结点，
		 * 指定非空结点的前驱结点设为null
		 */
        if (prev == null) {
            first = next;
        } else {
            prev.next = next;
            x.prev = null;
        }
 
        /**
		 * 如果指定非空结点的后继结点为空，则指定非空结点的前驱结点设为尾结点 
		 * 否则，指定非空结点的前驱结点设为指定非空结点的后继结点的前驱结点，
		 * 指定非空结点的后继结点设为null
		 */
        if (next == null) {
            last = prev;
        } else {
            next.prev = prev;
            x.next = null;
        }
 
        // 指定非空结点存储的元素设为null
        x.item = null;
        size--;
        modCount++;
        // 返回指定非空结点存储的元素
        return element;
    }
 
    /**
	 * 获取首结点存储的元素
	 * 
	 * @return 首结点存储的元素
	 * @throws NoSuchElementException
	 *             如果链表为空
	 */
    public E getFirst() {
        // 获取首结点引用
        final Node<E> f = first;
        // 如果首结点为空，则抛出无该元素异常
        if (f == null)
            throw new NoSuchElementException();
        // 返回首结点存储的元素
        return f.item;
    }
 
    /**
	 * 获取尾结点存储的元素
	 * 
	 * @return 尾结点存储的元素
	 * @throws NoSuchElementException
	 *             如果链表为空
	 */
    public E getLast() {
        // 获取尾结点引用
        final Node<E> l = last;
        // 如果尾结点为空，则抛出无该元素异常
        if (l == null)
            throw new NoSuchElementException();
        // 返回尾结点存储的元素
        return l.item;
    }
 
    /**
	 * 删除首结点，返回存储的元素
	 * 
	 * @return 首结点存储的元素
	 * @throws NoSuchElementException
	 *             如果链表为空
	 */
    public E removeFirst() {
        // 获取首结点引用
        final Node<E> f = first;
        // 如果首结点为空，则抛出无该元素异常
        if (f == null)
            throw new NoSuchElementException();
        // 删除首结点，返回存储的元素
        return unlinkFirst(f);
    }
 
    /**
	 * 删除尾结点，返回存储的元素
	 * 
	 * @return 尾结点存储的元素
	 * @throws NoSuchElementException
	 *             如果链表为空
	 */
    public E removeLast() {
        // 获取尾结点引用
        final Node<E> l = last;
        // 如果尾结点为空，则抛出无该元素异常
        if (l == null)
            throw new NoSuchElementException();
        // 删除尾结点，返回存储的元素
        return unlinkLast(l);
    }
 
    /**
	 * 头部插入指定元素
	 * 
	 * @param e
	 *            要添加的元素
	 */
    public void addFirst(E e) {
        // 通过头插法来插入指定元素
        linkFirst(e);
    }
 
    /**
	 * 尾部插入指定元素，该方法等价于add()
	 * 
	 * @param e
	 *            the element to add
	 */
    public void addLast(E e) {
        linkLast(e);
    }
 
    /**
	 * 判断是否包含指定元素
	 * 
	 * @param o
	 *            判断链表是否包含的元素
	 * @return {@code true} 如果链表包含指定的元素
	 */
    public boolean contains(Object o) {
        // 返回指定元素的索引位置，不存在就返回-1，然后比较返回bool值
        return indexOf(o) != -1;
    }
 
    /**
	 * 获取元素数量
	 * 
	 * @return 元素数量
	 */
    public int size() {
        return size;
    }
 
    /**
	 * 插入指定元素，返回操作结果,默认添加到末尾作为最后一个元素
	 * 
	 * @param e
	 *            要添加到此链表中的元素
	 * @return {@code true} (as specified by {@link Collection#add})
	 */
    public boolean add(E e) {
        // 通过尾插法来插入指定元素
        linkLast(e);
        return true;
    }
 
    /**
	 * 删除指定元素，默认从first节点开始，删除第一次出现的那个元素
	 * 
	 * @param o
	 *            要从该列表中删除的元素（如果存在）
	 * @return {@code true} 如果这个列表包含指定的元素
	 */
    public boolean remove(Object o) {
        // 会根据是否为null分开处理。若值不是null，会用到对象的equals()方法
        if (o == null) {
            // 遍历链表，查找到指定元素后删除该结点，返回true
            for (Node<E> x = first; x != null; x = x.next) {
                if (x.item == null) {
                    unlink(x);
                    return true;
                }
            }
        } else {
            for (Node<E> x = first; x != null; x = x.next) {
                if (o.equals(x.item)) {
                    unlink(x);
                    return true;
                }
            }
        }
        // 查找失败
        return false;
    }
 
    /**
	 * 将集合插入到链表尾部
	 * 
	 * @param c
	 *            包含要添加到此链表中的元素的集合
	 * @return {@code true} 如果该链表因添加而改变
	 * @throws NullPointerException
	 *             如果指定的集合是空的
	 */
    public boolean addAll(Collection<? extends E> c) {
        return addAll(size, c);
    }
 
    /**
	 * 将集合从指定位置开始插入
	 * 
	 * @param index
	 *            在哪个索引处前插入指定集合中的第一个元素
	 * @param c
	 *            包含要添加到此链表中的元素的集合
	 * @return {@code true} 如果该链表因添加而改变
	 * @throws IndexOutOfBoundsException
	 *             {@inheritDoc}
	 * @throws NullPointerException
	 *             如果指定的集合是空的
	 */
    public boolean addAll(int index, Collection<? extends E> c) {
        // 检查索引是否正确（0<=index<=size）
        checkPositionIndex(index);
        // 得到元素数组
        Object[] a = c.toArray();
        // 得到元素个数
        int numNew = a.length;
        // 没有元素，返回false
        if (numNew == 0)
            return false;
        // succ指向当前需要插入节点的位置，pred指向其前一个节点
        Node<E> pred, succ;
        // 末尾开始添加，当前节点后一个节点为null，前一个节点为尾节点
        if (index == size) {
            succ = null;
            pred = last;
        } else {    
        	//当前位置的节点为指定位置的节点，前一个节点为要添加的节点的前一个节点
            succ = node(index);
            pred = succ.prev;
        }
        // 遍历数组并添加到列表中
        for (Object o : a) {
            @SuppressWarnings("unchecked") E e = (E) o;
            // “封装”一个新节点
            Node<E> newNode = new Node<>(pred, e, null);
            // 原链表为null，新插入的节点作为链表首节点
            if (pred == null)
                first = newNode;
            else
                pred.next = newNode;    // 存在前节点，前节点向后指向新加的节点
            pred = newNode; // pred指针向后移动
        }
        // 从最后开始添加，则新节点成为尾节点
        if (succ == null) {
            last = pred;
        } else {
            pred.next = succ;   // 新节点向后指向之前得到的后续第一个节点
            succ.prev = pred;   // 后续的第一个节点也应改为向前指向最后一个添加的节点
        }
 
        size += numNew;
        modCount++;
        return true;
    }
 
    /**
	 * 删除所有元素
	 */
    public void clear() {
        // 遍历链表，删除所有结点,方便gc回收垃圾
        for (Node<E> x = first; x != null; ) {
            Node<E> next = x.next;
            x.item = null;
            x.next = null;
            x.prev = null;
            x = next;
        }
        // 首尾结点置空
        first = last = null;
        // 元素数量置0
        size = 0;
        modCount++;
    }
 
 
    // 位置访问操作
 
    /**
	 * 获取指定位置的元素
	 * 
	 * @param index
	 *            要返回的元素的索引
	 * @return 该链表中指定位置的元素
	 * @throws IndexOutOfBoundsException
	 *             {@inheritDoc}
	 */
    public E get(int index) {
        // 判断指定位置是否合法
        checkElementIndex(index);
        // 返回指定位置的元素
        return node(index).item;
    }
 
    /**
	 * 修改指定位置的元素，返回之前元素
	 * 
	 * @param index
	 *            要替换的元素的索引
	 * @param element
	 *            要存储在指定位置的元素
	 * @return 之前在指定位置的元素
	 * @throws IndexOutOfBoundsException
	 *             {@inheritDoc}
	 */
    public E set(int index, E element) {
        // 判断指定位置是否合法
        checkElementIndex(index);
        // 获取指定位置的结点
        Node<E> x = node(index);
        // 获取该结点存储的元素
        E oldVal = x.item;
        // 修改该结点存储的元素
        x.item = element;
        // 返回该结点存储的之前的元素
        return oldVal;
    }
 
    /**
	 * 在指定位置前插入指定元素
	 * 头插法
	 * @param index
	 *            指定元素将被插入的索引
	 * @param element
	 *            要插入的元素
	 * @throws IndexOutOfBoundsException
	 *             {@inheritDoc}
	 */
    public void add(int index, E element) {
        // 判断指定位置是否合法
        checkPositionIndex(index);
        // 如果指定位置在尾部，则通过尾插法来插入指定元素
        if (index == size)
            linkLast(element);
        else        // 如果指定位置不是尾部，则添加到指定位置前
            linkBefore(element, node(index));
    }
 
    /**
	 * 删除指定位置的元素，返回之前元素
	 * 
	 * @param index
	 *            要删除的元素的索引
	 * @return 之前在指定位置的元素
	 * @throws IndexOutOfBoundsException
	 *             {@inheritDoc}
	 */
    public E remove(int index) {
        // 判断指定位置是否合法
        checkElementIndex(index);
        // 删除指定位置的结点，返回之前元素
        return unlink(node(index));
    }
 
    /**
	 * 判断指定位置是否合法
	 */
    private boolean isElementIndex(int index) {
        return index >= 0 && index < size;
    }
 
    /**
	 * 判断迭代器遍历时或插入元素时指定位置是否合法
	 */
    private boolean isPositionIndex(int index) {
        return index >= 0 && index <= size;
    }
 
    /**
	 * 获取越界异常信息
	 */
    private String outOfBoundsMsg(int index) {
        return "Index: " + index + ", Size: " + size;
    }
 
    /**
	 * 判断指定位置是否合法
	 * 
	 * @param index
	 */
    private void checkElementIndex(int index) {
        if (!isElementIndex(index))
            throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
    }
 
    /**
	 * 判断指定位置是否合法
	 * 
	 * @param index
	 */
    private void checkPositionIndex(int index) {
        if (!isPositionIndex(index))
            throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
    }
 
    /**
	 * 获取指定下标的结点
	 */
    Node<E> node(int index) {
        // 如果指定下标<一半元素数量，则从首结点开始遍历
        // 否则，从尾结点开始遍历
        if (index < (size >> 1)) {
            Node<E> x = first;
            for (int i = 0; i < index; i++)
                x = x.next;
            return x;
        } else {
            Node<E> x = last;
            for (int i = size - 1; i > index; i--)
                x = x.prev;
            return x;
        }
    }
 
    // 查询操作
 
    /**
	 * 获取首次出现指定元素的位置 -1表示不存在
	 * 同样是根据是否为null进行区分
	 * @param o
	 *            要查找的元素
	 * @return the index of the first occurrence of the specified element in
	 *         this list, or -1 if this list does not contain the element
	 */
    public int indexOf(Object o) {
        int index = 0;
        if (o == null) {
            // 遍历链表，顺序查找指定元素
            for (Node<E> x = first; x != null; x = x.next) {
                if (x.item == null)
                    return index;
                index++;
            }
        } else {
            for (Node<E> x = first; x != null; x = x.next) {
                if (o.equals(x.item))
                    return index;
                index++;
            }
        }
        return -1;
    }
 
    /**
	 * 获取逆序下首次出现指定元素的位置 -1表示不存在
	 * 
	 * @param o
	 *            要查找的元素
	 * @return the index of the last occurrence of the specified element in this
	 *         list, or -1 if this list does not contain the element
	 */
    public int lastIndexOf(Object o) {
        int index = size;
        if (o == null) {
            // 遍历链表，逆序查找指定元素
            for (Node<E> x = last; x != null; x = x.prev) {
                index--;
                if (x.item == null)
                    return index;
            }
        } else {
            for (Node<E> x = last; x != null; x = x.prev) {
                index--;
                if (o.equals(x.item))
                    return index;
            }
        }
        return -1;
    }
 
    // 队列操作
 
    /**
	 * 出队（从前端），获得第一个元素，不存在会返回null，不会删除元素（节点） 获取首元素
	 * 
	 * @return the head of this list, or {@code null} 如果链表为空
	 * @since 1.5
	 */
    public E peek() {
        final Node<E> f = first;
        // 如果首结点为空，则返回null
        // 否则，返回首结点存储的元素
        return (f == null) ? null : f.item;
    }
 
    /**
	 * 出队（从前端），不删除元素，若为null会抛出异常而不是返回null 获取首元素
	 * 
	 * @return the head of this list
	 * @throws NoSuchElementException
	 *             如果链表为空
	 * @since 1.5
	 */
    public E element() {
        // 返回首结点存储的元素
        return getFirst();
    }
 
    /**
	 * 出队（从前端），如果不存在会返回null，存在的话会返回值并移除这个元素（节点） 获取并删除首元素
	 * 
	 * @return the head of this list, or {@code null} 如果链表为空
	 * @since 1.5
	 */
    public E poll() {
        // 获取首结点引用
        final Node<E> f = first;
        // 如果首结点为空，则返回null
        // 否则，删除首结点，返回首结点存储的元素
        return (f == null) ? null : unlinkFirst(f);
    }
 
    /**
	 * 出队（从前端），如果不存在会抛出异常而不是返回null，存在的话会返回值并移除这个元素（节点） 获取并删除首元素
	 * 
	 * @return the head of this list
	 * @throws NoSuchElementException
	 *             如果链表为空
	 * @since 1.5
	 */
    public E remove() {
        // 删除首结点，返回首结点存储的元素
        return removeFirst();
    }
 
    /**
	 * 入队（从后端），始终返回true
	 * 
	 * 链表不会溢出
	 * @param e
	 *            the element to add
	 * @return {@code true} (as specified by {@link Queue#offer})
	 * @since 1.5
	 */
    public boolean offer(E e) {
        // 通过尾插法插入指定元素，返回操作结果
        return add(e);
    }
 
    // 双端队列操作
 
    /**
	 * 入队（从前端），始终返回true
	 * 
	 * @param e
	 *            要插入的元素
	 * @return {@code true} (as specified by {@link Deque#offerFirst})
	 * @since 1.6
	 */
    public boolean offerFirst(E e) {
        // 通过头插法来插入指定元素
        addFirst(e);
        return true;
    }
 
    /**
	 * 入队（从后端），始终返回true
	 * 
	 * @param e
	 *            要插入的元素
	 * @return {@code true} (as specified by {@link Deque#offerLast})
	 * @since 1.6
	 */
    public boolean offerLast(E e) {
        // 通过尾插法来插入指定元素
        addLast(e);
        return true;
    }
 
    /**
	 * 出队（从前端），获得第一个元素，不存在会返回null，不会删除元素（节点）
	 * 
	 * @return the first element of this list, or {@code null} 如果链表为空
	 * @since 1.6
	 */
    public E peekFirst() {
        // 获取首结点引用
        final Node<E> f = first;
        // 如果首结点为空，则返回null
        // 否则，返回首结点存储的元素
        return (f == null) ? null : f.item;
    }
 
    /**
	 * 出队（从后端），获得最后一个元素，不存在会返回null，不会删除元素（节点）
	 * 
	 * @return the last element of this list, or {@code null} 如果链表为空
	 * @since 1.6
	 */
    public E peekLast() {
        // 获取尾结点引用
        final Node<E> l = last;
        // 如果尾结点为空，则返回null
        // 否则，返回尾结点存储的元素
        return (l == null) ? null : l.item;
    }
 
    /**
	 * 出队（从前端），获得第一个元素，不存在会返回null，会删除元素（节点）
	 * 
	 * @return the first element of this list, or {@code null} if this list is
	 *         empty
	 * @since 1.6
	 */
    public E pollFirst() {
        // 获取首结点引用
        final Node<E> f = first;
        // 如果首结点为空，则返回null
        // 否则，删除首结点，返回首结点存储的元素
        return (f == null) ? null : unlinkFirst(f);
    }
 
    /**
	 * 出队（从后端），获得最后一个元素，不存在会返回null，会删除元素（节点）
	 * 
	 * @return the last element of this list, or {@code null} if this list is
	 *         empty
	 * @since 1.6
	 */
    public E pollLast() {
        // 获取尾结点引用
        final Node<E> l = last;
        // 如果尾结点为空，则返回null
        // 否则，删除尾结点，返回尾结点存储的元素
        return (l == null) ? null : unlinkLast(l);
    }
 
    /**
	 * 入栈，从前面添加
	 * 
	 * @param e
	 *            the element to push
	 * @since 1.6
	 */
    public void push(E e) {
        // 通过头插法来插入指定元素
        addFirst(e);
    }
 
    /**
	 * 出栈，返回栈顶元素，从前面移除（会删除）
	 * 
	 * @return the element at the front of this list (which is the top of the
	 *         stack represented by this list)
	 * @throws NoSuchElementException
	 *             如果链表为空
	 * @since 1.6
	 */
    public E pop() {
        // 删除首结点，返回首结点存储的元素
        return removeFirst();
    }
 
    /**
	 * 删除顺序下首次出现的指定元素，返回操作结果
	 * 
	 * @param o
	 *            要从该列表中删除的元素（如果存在）
	 * @return {@code true} 如果链表包含指定的元素
	 * @since 1.6
	 */
    public boolean removeFirstOccurrence(Object o) {
        // 删除顺序下首次出现的指定元素对应的结点，返回操作结果
        return remove(o);
    }
 
    /**
	 * 删除逆序下首次出现的指定元素，返回操作结果
	 * 
	 * @param o
	 *            要从该列表中删除的元素（如果存在）
	 * @return {@code true} 如果链表包含指定的元素
	 * @since 1.6
	 */
    public boolean removeLastOccurrence(Object o) {
        // 由于LinkedList中允许存放null，因此下面通过两种情况来分别处理
        if (o == null) {
            // 遍历链表，从尾结点开始查找指定元素
            // 如果查找成功，删除该结点，返回true
            for (Node<E> x = last; x != null; x = x.prev) {
                if (x.item == null) {
                    unlink(x);
                    return true;
                }
            }
        } else {
            for (Node<E> x = last; x != null; x = x.prev) {
                if (o.equals(x.item)) {
                    unlink(x);
                    return true;
                }
            }
        }
        // 查找失败
        return false;
    }
 
    /**
	 * Returns a list-iterator of the elements in this list (in proper
	 * sequence), starting at the specified position in the list. Obeys the
	 * general contract of {@code List.listIterator(int)}.
	 * <p>
	 * <p>
	 * The list-iterator is <i>fail-fast</i>: if the list is structurally
	 * modified at any time after the Iterator is created, in any way except
	 * through the list-iterator's own {@code remove} or {@code add} methods,
	 * the list-iterator will throw a {@code ConcurrentModificationException}.
	 * Thus, in the face of concurrent modification, the iterator fails quickly
	 * and cleanly, rather than risking arbitrary, non-deterministic behavior at
	 * an undetermined time in the future.
	 * 
	 * @param index
	 *            index of the first element to be returned from the
	 *            list-iterator (by a call to {@code next})
	 * @return a ListIterator of the elements in this list (in proper sequence),
	 *         starting at the specified position in the list
	 * @throws IndexOutOfBoundsException
	 *             {@inheritDoc}
	 * @see List#listIterator(int)
	 */
    public ListIterator<E> listIterator(int index) {
        checkPositionIndex(index);
        return new ListItr(index);
    }
 
    private class ListItr implements ListIterator<E> {
        private Node<E> lastReturned;
        private Node<E> next;
        private int nextIndex;
        private int expectedModCount = modCount;
 
        ListItr(int index) {
            // assert isPositionIndex(index);
            next = (index == size) ? null : node(index);
            nextIndex = index;
        }
 
        public boolean hasNext() {
            return nextIndex < size;
        }
 
        public E next() {
            checkForComodification();
            if (!hasNext())
                throw new NoSuchElementException();
 
            lastReturned = next;
            next = next.next;
            nextIndex++;
            return lastReturned.item;
        }
 
        public boolean hasPrevious() {
            return nextIndex > 0;
        }
 
        public E previous() {
            checkForComodification();
            if (!hasPrevious())
                throw new NoSuchElementException();
 
            lastReturned = next = (next == null) ? last : next.prev;
            nextIndex--;
            return lastReturned.item;
        }
 
        public int nextIndex() {
            return nextIndex;
        }
 
        public int previousIndex() {
            return nextIndex - 1;
        }
 
        public void remove() {
            checkForComodification();
            if (lastReturned == null)
                throw new IllegalStateException();
 
            Node<E> lastNext = lastReturned.next;
            unlink(lastReturned);
            if (next == lastReturned)
                next = lastNext;
            else
                nextIndex--;
            lastReturned = null;
            expectedModCount++;
        }
 
        public void set(E e) {
            if (lastReturned == null)
                throw new IllegalStateException();
            checkForComodification();
            lastReturned.item = e;
        }
 
        public void add(E e) {
            checkForComodification();
            lastReturned = null;
            if (next == null)
                linkLast(e);
            else
                linkBefore(e, next);
            nextIndex++;
            expectedModCount++;
        }
 
        public void forEachRemaining(Consumer<? super E> action) {
            Objects.requireNonNull(action);
            while (modCount == expectedModCount && nextIndex < size) {
                action.accept(next.item);
                lastReturned = next;
                next = next.next;
                nextIndex++;
            }
            checkForComodification();
        }
 
        final void checkForComodification() {
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
        }
    }
 
    /**
	 * 节点的数据结构，包含前后节点的引用和当前节点
	 * 
	 * @param <E>
	 */
    private static class Node<E> {
        // 存储的元素
        E item;
        // 后继结点
        Node<E> next;
        // 前驱结点
        Node<E> prev;
 
        // 前驱结点、存储的元素和后继结点作为参数的构造方法
        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }
 
    /**
	 * 返回迭代器
	 * 
	 * @since 1.6
	 */
    public Iterator<E> descendingIterator() {
        return new DescendingIterator();
    }
 
    /**
	 * 因为采用链表实现，所以迭代器很简单
	 */
    private class DescendingIterator implements Iterator<E> {
        private final ListItr itr = new ListItr(size());
 
        public boolean hasNext() {
            return itr.hasPrevious();
        }
 
        public E next() {
            return itr.previous();
        }
 
        public void remove() {
            itr.remove();
        }
    }
 
    /**
	 * 父类克隆方法
	 */
    @SuppressWarnings("unchecked")
    private LinkedList<E> superClone() {
        try {
            return (LinkedList<E>) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new InternalError(e);
        }
    }
 
    /**
	 * 克隆，浅拷贝
	 * 
	 * 浅拷贝时，若存储的是对象的引用，拷贝时，对象本身的改变将表现到副本中，而深拷贝不会。
	 * @return a shallow copy of this {@code LinkedList} instance
	 */
    public Object clone() {
        LinkedList<E> clone = superClone();
 
        // 链表初始化
        clone.first = clone.last = null;
        clone.size = 0;
        clone.modCount = 0;
 
        // 插入结点
        for (Node<E> x = first; x != null; x = x.next)
            clone.add(x.item);
        // 返回克隆后的对象引用
        return clone;
    }
 
    /**
     * 返回新的数组，数组含有列表中所有元素
	 */
    public Object[] toArray() {
        Object[] result = new Object[size];
        int i = 0;
        for (Node<E> x = first; x != null; x = x.next)
            result[i++] = x.item;
        return result;
    }
 
    /**
	 * Returns an array containing all of the elements in this list in proper
	 * sequence (from first to last element); the runtime type of the returned
	 * array is that of the specified array. If the list fits in the specified
	 * array, it is returned therein. Otherwise, a new array is allocated with
	 * the runtime type of the specified array and the size of this list.
	 */
    @SuppressWarnings("unchecked")
    public <T> T[] toArray(T[] a) {
        if (a.length < size)
            a = (T[]) java.lang.reflect.Array.newInstance(
                    a.getClass().getComponentType(), size);
        int i = 0;
        Object[] result = a;
        for (Node<E> x = first; x != null; x = x.next)
            result[i++] = x.item;
 
        if (a.length > size)
            a[size] = null;
 
        return a;
    }
 
    private static final long serialVersionUID = 876323262645176354L;
 
    /**
	 * 序列化
	 */
    private void writeObject(java.io.ObjectOutputStream s)
            throws java.io.IOException {
        // 默认序列化
        s.defaultWriteObject();
 
        // 写入元素数量
        s.writeInt(size);
 
        // 遍历链表，写入所有元素
        for (Node<E> x = first; x != null; x = x.next)
            s.writeObject(x.item);
    }
 
    /**
	 * 反序列化
	 */
    @SuppressWarnings("unchecked")
    private void readObject(java.io.ObjectInputStream s)
            throws java.io.IOException, ClassNotFoundException {
        // 默认反序列化
        s.defaultReadObject();
 
        // 读取元素数量
        int size = s.readInt();
 
        // 遍历链表，读取所有元素并尾部插入
        for (int i = 0; i < size; i++)
            linkLast((E) s.readObject());
    }
 
    /**
	 * Creates a <em><a href="Spliterator.html#binding">late-binding</a></em>
	 * and <em>fail-fast</em> {@link Spliterator} over the elements in this
	 * list.
	 */
    @Override
    public Spliterator<E> spliterator() {
        return new LLSpliterator<E>(this, -1, 0);
    }
 
    /**
	 * A customized variant of Spliterators.IteratorSpliterator
	 */
    static final class LLSpliterator<E> implements Spliterator<E> {
        static final int BATCH_UNIT = 1 << 10;  // batch array size increment
        static final int MAX_BATCH = 1 << 25;  // max batch array size;
        final LinkedList<E> list; // null OK unless traversed
        Node<E> current;      // current node; null until initialized
        int est;              // size estimate; -1 until first needed
        int expectedModCount; // initialized when est set
        int batch;            // batch size for splits
 
        LLSpliterator(LinkedList<E> list, int est, int expectedModCount) {
            this.list = list;
            this.est = est;
            this.expectedModCount = expectedModCount;
        }
 
        final int getEst() {
            int s; // force initialization
            final LinkedList<E> lst;
            if ((s = est) < 0) {
                if ((lst = list) == null)
                    s = est = 0;
                else {
                    expectedModCount = lst.modCount;
                    current = lst.first;
                    s = est = lst.size;
                }
            }
            return s;
        }
 
        public long estimateSize() {
            return (long) getEst();
        }
 
        public Spliterator<E> trySplit() {
            Node<E> p;
            int s = getEst();
            if (s > 1 && (p = current) != null) {
                int n = batch + BATCH_UNIT;
                if (n > s)
                    n = s;
                if (n > MAX_BATCH)
                    n = MAX_BATCH;
                Object[] a = new Object[n];
                int j = 0;
                do {
                    a[j++] = p.item;
                } while ((p = p.next) != null && j < n);
                current = p;
                batch = j;
                est = s - j;
                return Spliterators.spliterator(a, 0, j, Spliterator.ORDERED);
            }
            return null;
        }
 
        public void forEachRmaining(Consumer<? super E> action) {
            Node<E> p;
            int n;
            if (action == null) throw new NullPointerException();
            if ((n = getEst()) > 0 && (p = current) != null) {
                current = null;
                est = 0;
                do {
                    E e = p.item;
                    p = p.next;
                    action.accept(e);
                } while (p != null && --n > 0);
            }
            if (list.modCount != expectedModCount)
                throw new ConcurrentModificationException();
        }
 
        public boolean tryAdvance(Consumer<? super E> action) {
            Node<E> p;
            if (action == null) throw new NullPointerException();
            if (getEst() > 0 && (p = current) != null) {
                --est;
                E e = p.item;
                current = p.next;
                action.accept(e);
                if (list.modCount != expectedModCount)
                    throw new ConcurrentModificationException();
                return true;
            }
            return false;
        }
 
        public int characteristics() {
            return Spliterator.ORDERED | Spliterator.SIZED | Spliterator.SUBSIZED;
        }
    }
 
}
```





