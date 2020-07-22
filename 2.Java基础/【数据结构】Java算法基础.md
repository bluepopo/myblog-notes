【数据结构】Java算法基础



一、前言

KMP算法

汉诺塔

八皇后（分治算法）

马踏棋盘算法（骑士周游问题）图的深度优先算法+贪心算法优化





# 第一章、稀疏数组和队列 

## 1.1 稀疏数组

**基本介绍**

当一个数组中大部分元素为０，或者为同一个值的数组时，可以使用稀疏数组来保存该数组。

**稀疏数组的处理方法是:**

1) 记录数组一共有几行几列，有多少个有效值

2) 把具有不同值的有效元素的行列及值记录在一个小规模的数组中，从而缩小程序的规模

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200722111438.png" alt="image-20200722111428966" style="zoom: 67%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200722112158.png" alt="image-20200722112156194" style="zoom: 67%;" />

![image-20200722113436031](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200722113438.png)

```java
package zy.code.sparsearray;

/**
 * 稀疏数组
 */
public class SparseArray {

    public static void main(String[] args) {
        //创建一个原始二维数组
        // 11 *11 的棋盘大小，1表示黑棋子，2表示蓝棋子
        int chessArr[][] = new int[11][11];
        chessArr[1][2] = 1;
        chessArr[2][3] = 2;
        chessArr[4][5] = 2;

        for (int[] row : chessArr){
            for (int data : row){
                System.out.printf("%d\t",data);
            }
            System.out.println();;
        }

        //计算有效值
        int val = 0;
        for (int i=0; i<11;i++){
            for (int j=0;j<11;j++){
                if (chessArr[i][j] != 0){
                    val++;
                }
            }
        }
        System.out.printf("原来二维有效值有 %d 个\n",val);
        
/**
 * 二维数组 转 稀疏数组的思路
 * 1. 遍历  原始的二维数组，得到有效数据的个数 sum
 * 2. 根据sum 就可以创建 稀疏数组 sparseArr   int[sum + 1] [3]。加一是因为第一列放的是原二维数组有几行几列、几个有效值。
 * 3. 将二维数组的有效数据数据存入到 稀疏数组
 */
        //创建稀疏数组
        int spareArr[][] = new int[val+1][3];
        spareArr[0][0] = 11;
        spareArr[0][1] = 11;
        spareArr[0][2] = val;

        //定义一个稀疏数组的行计数器
        int spare_row = 0;
        for (int i = 0; i<11; i++){
            for (int j = 0; j <11; j++){
                if (chessArr[i][j] != 0){
                    spare_row++;
                    spareArr[spare_row][0] = i;
                    spareArr[spare_row][1] = j;
                    spareArr[spare_row][2] = chessArr[i][j];
                }
            }
        }
        // 遍历稀疏数组
        System.out.println("转换后的稀疏数组为：");
        for (int i = 0; i < spareArr.length; i ++){
            System.out.printf("%d\t%d\t%d\t\n",spareArr[i][0],spareArr[i][1],spareArr[i][2]);
        }

        System.out.println();


        /**
         * 稀疏数组 转 原始的二维数组的思路
         *
         * 1. 先读取稀疏数组的第一行，根据第一行的数据，创建原始的二维数组大小，比如上面的  chessArr2 = int [11][11]
         * 2. 在读取稀疏数组后几行的数据，并赋给 原始的二维数组 即可.
         */
        int row_count = spareArr[0][0];
        int column_count = spareArr[0][1];
        int chessArr_val = spareArr[0][2];
        //创建原数组大小
        int chessArr2[][] = new int[row_count][column_count];
        //遍历稀疏数组并赋值
        for (int i = 1; i<= chessArr_val; i ++){
             int r = spareArr[i][0];
             int c = spareArr[i][1];
             int v = spareArr[i][2];
             chessArr2[r][c] = v;
        }

        //输出还原数组
        System.out.println("原来的二维数组为：");
        for (int[] row : chessArr2){
            for (int data : row){
                System.out.printf("%d\t",data);
            }
            System.out.println();;
        }
    }
}

```



作业：

- 抽取出普通二维数组与稀疏数组的转换，封装成工具类
- 使用 I/O 流基础，序列化对象保存到磁盘文件中



```java
/**
* 抽取普通二维数组与稀疏数组的转换。封装成工具方法
 */
public class SpareArryUtil {


    /**
     * 普通二维数组转换为稀疏数组
     */
    public static int[][] arrayTYoSpare(int[][] chessArr){
        //计算有效值
        int val = 0;
        for (int i=0; i<11;i++){
            for (int j=0;j<11;j++){
                if (chessArr[i][j] != 0){
                    val++;
                }
            }
        }

        //创建稀疏数组
        int spareArr[][] = new int[val+1][3];
        spareArr[0][0] = 11;
        spareArr[0][1] = 11;
        spareArr[0][2] = val;
        //定义一个稀疏数组的行计数器
        int spare_row = 0;
        for (int i = 0; i<11; i++){
            for (int j = 0; j <11; j++){
                if (chessArr[i][j] != 0){
                    spare_row++;
                    spareArr[spare_row][0] = i;
                    spareArr[spare_row][1] = j;
                    spareArr[spare_row][2] = chessArr[i][j];
                }
            }
        }


        return spareArr;
    }


    /**
     * 稀疏数组还原回普通二维数组
     */
    public static int[][] spareToArray(int[][] spareArr){

        int row_count = spareArr[0][0];
        int column_count = spareArr[0][1];
        int chessArr_val = spareArr[0][2];
        //创建原数组大小
        int chessArr2[][] = new int[row_count][column_count];
        //遍历稀疏数组并赋值
        for (int i = 1; i<= chessArr_val; i ++){
            int r = spareArr[i][0];
            int c = spareArr[i][1];
            int v = spareArr[i][2];
            chessArr2[r][c] = v;
        }

        return chessArr2;
    }
}

```



```java
/**
 * 将对象序列化到磁盘文件
 */
public class SerializeObjToFile {
    public static void main(String[] args) throws IOException {
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("G:\\test\\data\\spareArr.data"));

        int chessArr[][] = new int[11][11];
        chessArr[1][2] = 1;
        chessArr[2][3] = 2;
        chessArr[4][5] = 2;
        int[][] spareArr = SpareArryUtil.arrayTYoSpare(chessArr);
        int[][] chessArr2 = SpareArryUtil.spareToArray(spareArr);
        /**
         * 序列化对象到磁盘
         */
        oos.writeObject(spareArr);
        oos.flush();
        oos.close();

    }

}

```



## 1.2 数组模拟队列

（1）队列的数据结构：

 当我们将数据存入队列时称为 ”addQueue”，addQueue 的处理需要有两个步骤：

1. 尾指针后移：rear = rear+1 , 当 front == rear 【空】

2. 判断尾指针 rear 小于数组队列的最大下标 [ maxSize - 1 ]，则将数据存入 rear所指的数组元素中，否则无法存入数据。

   > 注意： 
   >
   > 1. rear == maxSize - 1;  [ 队列满 ]
   >
   > 2. rear 是队列最后  [含]
   > 3. front 是队列最前元素  [不含]

![image-20200722140953608](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200722141000.png)

```java
class ArrayQueue(arrMaxSize: Int) {
  val maxSize: Int = arrMaxSize //队列的最大容量，等于数组的最大容量
  val array = new Array[Int](arrMaxSize) //数组的最大容量
  var front: Int = -1 //队首初始值
  var rear: Int = -1 //队尾初始值
}

 //初始化队列
 val queue = new ArrayQueue(3)

```



代码实现：

```java
public class ArrayQueueDemo {

    public static void main(String[] args) {
        //测试一把
        //创建一个队列
        ArrayQueue queue = new ArrayQueue(3);
        char key = ' '; //接收用户输入
        Scanner scanner = new Scanner(System.in);//
        boolean loop = true;
        //输出一个菜单
        while(loop) {
            System.out.println("s(show): 显示队列");
            System.out.println("e(exit): 退出程序");
            System.out.println("a(add): 添加数据到队列");
            System.out.println("g(get): 从队列取出数据");
            System.out.println("h(head): 查看队列头的数据");
            key = scanner.next().charAt(0);//接收一个字符
            switch (key) {
                case 's':
                    queue.showQueue();
                    break;
                case 'a':
                    System.out.println("输出一个数");
                    int value = scanner.nextInt();
                    queue.addQueue(value);
                    break;
                case 'g': //取出数据
                    try {
                        int res = queue.getQueue();
                        System.out.printf("取出的数据是%d\n", res);
                    } catch (Exception e) {
                        // TODO: handle exception
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'h': //查看队列头的数据
                    try {
                        int res = queue.headQueue();
                        System.out.printf("队列头的数据是%d\n", res);
                    } catch (Exception e) {
                        // TODO: handle exception
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'e': //退出
                    scanner.close();
                    loop = false;
                    break;
                default:
                    break;
            }
        }

        System.out.println("程序退出~~");
    }

}

// 使用数组模拟队列-编写一个ArrayQueue类
class ArrayQueue {
    private int maxSize; // 表示数组的最大容量
    private int front; // 队列头
    private int rear; // 队列尾
    private int[] arr; // 该数据用于存放数据, 模拟队列

    // 创建队列的构造器
    public ArrayQueue(int arrMaxSize) {
        maxSize = arrMaxSize;
        arr = new int[maxSize];
        front = -1; // 指向队列头部，分析出front是指向队列头的前一个位置.
        rear = -1; // 指向队列尾，指向队列尾的数据(即就是队列最后一个数据)
    }

    // 判断队列是否满
    public boolean isFull() {
        return rear == maxSize - 1;
    }

    // 判断队列是否为空
    public boolean isEmpty() {
        return rear == front;
    }

    // 添加数据到队列
    public void addQueue(int n) {
        // 判断队列是否满
        if (isFull()) {
            System.out.println("队列满，不能加入数据~");
            return;
        }
        rear++; // 让rear 后移
        arr[rear] = n;
    }

    // 获取队列的数据, 出队列
    public int getQueue() {
        // 判断队列是否空
        if (isEmpty()) {
            // 通过抛出异常
            throw new RuntimeException("队列空，不能取数据");
        }
        front++; // front后移
        return arr[front];

    }

    // 显示队列的所有数据
    public void showQueue() {
        // 遍历
        if (isEmpty()) {
            System.out.println("队列空的，没有数据~~");
            return;
        }
        for (int i = 0; i < arr.length; i++) {
            System.out.printf("arr[%d]=%d\n", i, arr[i]);
        }
    }

    // 显示队列的头数据， 注意不是取出数据
    public int headQueue() {
        // 判断
        if (isEmpty()) {
            throw new RuntimeException("队列空的，没有数据~~");
        }
        return arr[front + 1];
    }
}
```



数组模拟队列的问题：

- 假溢出

![image-20200722150711261](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200722150712.png)



## 1.3 循环队列



优化思路如下:
1.  front 变量的含义做一个调整： front 就指向队列的第一个元素, 也就是说 arr[front] 就是队列的第一个元素 
- front 的初始值 = 0
2.  rear 变量的含义做一个调整：rear 指向队列的`最后一个元素的后一个位置`. 因为希望==空出一个空间做为约定==.
- rear 的初始值 = 0
3. 当队列满时，条件是  ==(rear  + 1) % maxSize == front 【满】==
4. 对队列为空的条件， ==rear == front 【空】==
5. 当我们这样分析， 队列中有效的数据的个数   count == ==(rear + maxSize - front) % maxSize==   // rear = 1 front = 0 
6. 我们就可以在原来的队列上修改得到，一个环形队列

![image-20200722161228463](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200722161253.png)

代码实现：



```java
package zy.code.queue;


/**
 * 数组实现循环队列
 */
public class CircleArrayQueue {

    private int maxSize;//最大容量为数组大小
    private int front;//队首指向第一个元素，初始值为0
    private int rear;//队尾指向最后一个元素的下一个位置，初始值为0
    private int[] arr;//存储队列元素

    /**
     * 队列初始化
     * @param maxSize
     */
    public CircleArrayQueue(int maxSize) {
        this.maxSize = maxSize;
        this.front = 0;
        this.rear = 0;
        this.arr = new int[maxSize];
    }

    /**
     * 判断是否已满
     */
    public boolean isFull(){
        if ((rear + 1) % maxSize == front) {
            return true;
        }
        return false;
    }

    /**
     * 判空
     */
    public boolean isEmpty(){
        if (this.front == this.rear){
            return true;
        }
        return false;
    }

    /**
     * 添加数据
     */
    public void addQueue(int n){
        if (isFull()){
            System.out.println("队列满，不能添加数据。");
            return;
        }

        arr[rear] = n;//添加元素
        rear = (rear + 1) % maxSize;//指针后移
    }

    /**
     * 取数据
     */
    public int getQueue(){
        if(isEmpty()){
            throw new RuntimeException("队列空，不能取数据。");
        }
        // 先把队头元素保存在一个临时变量
        // 然后对头指针后移
        // 返回临时变量
        int temp = arr[front];
        front = (front + 1 ) % maxSize;
        return temp;
    }

    /**
     * 显示队列数据
     */
    public void showQueue(){
        if(isEmpty()){
            System.out.println("队列空，没有数据可以显示。");
        }

        //怎么遍历数组？
        int count = size();
        for (int i = front; i < front + count; i++){
            //从队头开始遍历，真正的下标可能会超出数组大小，必须转换为循环后的真正下标
            int index = i % maxSize;
            int val = arr[index];
            System.out.printf("arr[%d] = %d\n",index,val);
        }

    }


    /**
     * 取出队列有效元素的个数
     */
    public int size(){
        return (rear + maxSize - front) % maxSize;
    }


    /**
     * 查看第一个元素
     */
    public int headQueue(){
        if(isEmpty()){
            throw new RuntimeException("队列空，不能取数据。");
        }
        return arr[front];
    }

}


```



```java
package zy.code.queue;

import java.util.Scanner;

/**
 * 测试循环队列
 */
public class CircleQueueTest {
    public static void main(String[] args) {
        //测试一把
        //创建一个队列
        CircleArrayQueue queue = new CircleArrayQueue(3);
        char key = ' '; //接收用户输入
        Scanner scanner = new Scanner(System.in);//
        boolean loop = true;
        //输出一个菜单
        while(loop) {
            System.out.println("s(show): 显示队列");
            System.out.println("e(exit): 退出程序");
            System.out.println("a(add): 添加数据到队列");
            System.out.println("g(get): 从队列取出数据");
            System.out.println("h(head): 查看队列头的数据");
            key = scanner.next().charAt(0);//接收一个字符
            switch (key) {
                case 's':
                    queue.showQueue();
                    break;
                case 'a':
                    System.out.println("输出一个数");
                    int value = scanner.nextInt();
                    queue.addQueue(value);
                    break;
                case 'g': //取出数据
                    try {
                        int res = queue.getQueue();
                        System.out.printf("取出的数据是%d\n", res);
                    } catch (Exception e) {
                        // TODO: handle exception
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'h': //查看队列头的数据
                    try {
                        int res = queue.headQueue();
                        System.out.printf("队列头的数据是%d\n", res);
                    } catch (Exception e) {
                        // TODO: handle exception
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'e': //退出
                    scanner.close();
                    loop = false;
                    break;
                default:
                    break;
            }
        }

        System.out.println("程序退出~~");
    }



}

```





# 第二章、链表

链表

单链表

双向链表

单项环形链表

josephu问题

单向环形链表解决约瑟夫问题

# 第三章、栈



栈的基础介绍



前缀中缀后缀表达式



逆波兰计算器



# 第四章、递归



迷宫问题

八皇后问题（回溯算法）





# 第五章、排序算法



冒泡排序

选择排序

插入排序

希尔排序

快速排序

归并排序

桶排序

# 第六章、算法时间、空间复杂度





# 第七章、查找算法



线性查找

二分查找

插值查找

斐波那契黄金分割查找



# 第八章、哈希表

哈希表原理

哈希表（散列）谷歌上机题





# 第九章、树与二叉树

二叉树



顺序二叉树

堆排序



赫夫曼树



赫夫曼编码



二叉排序树



平衡二叉树



# 第十章、多路查找树

二叉树与B树

2-3树

B树、B+树





# 第十一章、图的搜索







# 第十二章、经典常用算法

二分查找



分治算法



动态规划算法



KMP算法



贪心算法



普利姆算法



克鲁斯卡尔算法



迪杰斯特拉算法



弗洛伊德算法



马踏棋盘算法







