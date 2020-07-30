【JavaSE】IO流总结

@[toc]

![image-20200703122636963](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720131226.png)







![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702184017.png)
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702184129.png)

# <font color=#FF8C00>一、File类</font>

## 1. File类概述和方法
- 它是文件和目录路径名的抽象表示
- 文件和目录是可以通过 File封装成对象的
- 对于 File而言，其封装的并不是一个真正存在的文件，仅仅是一个路径名而已。它可以是存在的，也可以是不存在的。
<font color=#FF8C00>.将来是要通过具体的操作把这个路径的内容转换为具体存在的</font>

### 构造方法
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702185024.png)

```java
File file = new File("E:\\demo\\a.txt")
File file = new File("E\\demo", "a.txt")


File file1 = new File("e\\demo")
File file2 = new File(file1, "a.txt")
```

### 创建文件夹或文件

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702210124.png)








个人认为在某个文件夹下创建文件最好的办法是

```java
File file = new File("e:\\demo\\test\\aaa\\bbb");
file.mkdirs();

File file2 = new File("e:\\demo\\test\\aaa\\bbb\\ccc.txt");
file2.createNewFile();

```


### 判断功能
- ` public boolean isDirectory()`  测试此抽象路径名表示的File是否为目录
- `public boolean isFile() `测试此抽象路径名表示的File是否为文件
- `public boolean exists()`  测试此抽象路径名表示的File是否存在

- `public boolean canWrite()`
- `public boolean isHidden()`
### 获取功能

- `public String getAbsolutePath() `  ： 绝对路径名字符串
- `public String getPath() ` ： 抽象路径名转换为路径名字符串
- `public String getName() ` ： 返回由此抽象路径名表示的文件或目录的名称
- `public String[] list() ` ： 返回此抽象路径名表示的目录中的`文件和目录的名称字符串数组`
- `public File[] listFiles() ` ： 返回此抽象路径名表示的目录中的`文件和目录的File对象数组`

- `length()`
- `lastModified()`

### 高级获取
- ` File[] fileArray = file.listFiles()` ： 全部获取再遍历判断输出
- `String[] strArray = file.list(FilenameFilter filter)` ： 通过文件过滤器
- `File[] fileArray = file.listFiles(FilenameFilter filter)` ： 通过文件过滤器


### 删除功能
-  `public boolean delete() ` ： 删除由此抽象路径名表示的文件或目录

> 如果忘了创建文件或文件夹忘了写盘符，那么默认在项目路径下
> Java中的删除不走回收站
> 要删除一个文件夹，请注意该文件夹不能包含文件或文件夹（要为空）
>


### 重命名
- `renameTo(File dest)`：如果路径名相同，就是改名；如果不同，改名并剪切，目标文件不需要先创建，只要File对象即可


### 练习：输出指定目录下指定后缀名的文件名
```java
/**
 * 高级获取
 * 获取某个文件目录下的所有文件集合
 */
public class FileTest01 {
    public static void main(String[] args) {
        // 方式一
        System.out.println("方式一: files----------------------");
        File file1 = new File("G:\\data");
        File[] files = file1.listFiles();
        for (File file : files){
            if (file.isFile()){
                if (file.getName().endsWith(".txt")){
                    System.out.println(file.getName());
                }
            }
        }

        // 方式二 ：过滤器 String[] strArray = file.list(FilenameFilter filter);
        System.out.println("方式二: filename----------------------");
        File file2 = new File("G:\\data");
        String[] list = file2.list(new FilenameFilter() {
            @Override
            public boolean accept(File dir, String name) {
                File file = new File(dir, name);
                return file.isFile() && file.getName().endsWith(".txt");
            }
        });

        for (String s : list){
            System.out.println(s);
        }

        //方式3：过滤器 File[] fileArray = file.listFiles(FilenameFilter filter);
        System.out.println("方式三: files----------------------");
        File file3 = new File("G:\\data");
        File[] files3 = file3.listFiles(new FilenameFilter() {
            @Override
            public boolean accept(File dir, String name) {
                File file = new File(dir, name);

                return file.isFile() && file.getName().endsWith(".txt");
            }
        });
        for (File f: files3){
            System.out.println(f.getName());
        }

    }
}

```
### 练习：批量修改文件名案例
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702185015.png)

```java
public class FileTest02 {
    public static void main(String[] args) {
        File dir = new File("G:\\data");
        File[] files = dir.listFiles();
        for (File f: files){
            if (f.isFile()){
                String name = f.getName();
                // 获取前面的数字编号
                String numbername = name.substring(0, 3);//[0,3)
                // 获取后面的内容
                int index = name.lastIndexOf("_");
                String contentname = name.substring(index);//startindex

                String newname = numbername + contentname;
                File newFile = new File(dir,newname);
                //重命名
                f.renameTo(newFile);
            }
        }
    }
}

```
### 练习：递归遍历目录下所有文件
```java
File srcFile = new File("E:\\itheima");
//调用方法
    getAllFilePath(srcFile);

//定义一个方法，用于获取给定目录下的所有内容，参数为第1步创建的File对象
  public static void getAllFilePath(File srcFile) {
    //获取给定的File目录下所有的文件或者目录的File数组
    File[] fileArray = srcFile.listFiles();
    //遍历该File数组，得到每一个File对象
    if(fileArray != null) {
      for(File file : fileArray) {
        //判断该File对象是否是目录
        if(file.isDirectory()) {
          //是：递归调用
          getAllFilePath(file);
       } else {
          //不是：获取绝对路径输出在控制台
          System.out.println(file.getAbsolutePath());
          }
         }
       }

```

# <font color=FF8C00>二、字节流</font>
## 字节流 FileInputStream && FileOutputStream
**字节流抽象基类**

- InputStream ：这个抽象类是表示字节输入流的所有类的超类
- OutputStream ：这个抽象类是表示字节输出流的所有类的超类



子类名特点：子类名称都是以其父类名作为子类名的后缀



### FileOutputStream 字节输出流
**构造方法**
- `new FileOutputStream(File file) `
- `FileOutputStream(String name) `：创建文件输出流以指定的名称写入文件
- `new FileOutputStream(File file, boolean append)` : 支持追加写入，每次运行程序，同个代码实现追加写入，而不是覆盖

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702185026.png)
**write方法**

-  `void  write(byte[] b)` :指定的字节数组写入此文件输出流 一次写一个数组
- `void  write(byte[] b, int off, int len)` : 从偏移量off开始写入此文件输出流， 指定长度len  一次写**一个数组的部分数据**,从
- `void  write(int b)` :  将指定的字节写入此文件输出流 一次写一个字节数据

```java
等价于

//     new File(name)
//    FileOutputStream fos = 
```
**使用字节输出流写数据的步骤**

- <font color=#8B008B>创建字节输出流对象 (调用系统功能创建了文件，创建字节输出流对象，让字节输出流对象指向文件)</font>

- <font color=#8B008B>调用字节输出流对象的写数据方法</font>

- <font color=#8B008B>释放资源 (关闭此文件输出流并释放与此流相关联的任何系统资源)</font>

 /*
    <font color=#FF8C00>  *做了三件事情：
        A:调用系统功能自动帮我们创建了文件
        B:创建了字节输出流对象
        C:让字节输出流对象指向创建好的文件*</font>
    */

```java
public class FileOutputStreamDemo01 {
  public static void main(String[] args) throws IOException {
  
    //创建字节输出流对象
    //FileOutputStream(String name)：创建文件输出流以指定的名称写入文件
    FileOutputStream fos = new FileOutputStream("myByteStream\\fos.txt");

    //void write(int b)：将指定的字节写入此文件输出流
    fos.write(97);
//    fos.write(57);
//    fos.write(55);
    //最后都要释放资源
    //void close()：关闭此文件输出流并释放与此流相关联的任何系统资源。
    fos.close();
 }
}
```



### FileInputStream字节输入流
**构造方法**
- `new FileIntputStream(File file)`
- `new FileInputStream(String name)`

**read方法**
- read()

> 读取一个后自动指向下一个，返回值是int类型
> 读到末尾返回-1
> 原本的汉字可能是两个字节，现在每读取一个字节就转换为char输出，会出现乱码，好比汉字被拆成两半。


- read(byte[] b)

> 返回读取字节长度，如果返回-1，说明已经读到文件末尾
> 输出打印时，String构造方法：把字节数组转字符串



**练习：按字节读取文件内容**
```java
//读数据
//一次读取一个字节
FileInputStream fis=new FileInputStream("day04_IO\\fos.txt");
int by;
while ((by=fis.read())!=-1){
    System.out.print((char)by);
}
fos.close();


//一次读取一个数组
FileInputStream fis2=new FileInputStream("day04_IO\\test.txt");
byte[] bys=new byte[1024];
int len;//实际读取到的字节长度
while ((len=fis2.read(bys))!=-1){
    String s=new String(bys,0,len);//字节数组转字符串
    System.out.println(s);
}


//复制图片
FileInputStream fis3=new FileInputStream("G:\\图片\\头像\\16.jpeg");
FileOutputStream fos3=new FileOutputStream("day04_IO\\pic.png");

byte[] bys3=new byte[2048];
int len3;
while ((len3=fis3.read(bys3))!=-1){
    fos3.write(bys3,0,len3);
}
fis3.close();
fos3.close();

```

## 字节缓冲流BufferedInputStream &&  BufferedOutputStream

**字节缓冲流介绍**
- lBufferOutputStream ：该类实现缓冲输出流。 通过设置这样的输出流，应用程序可以向底层输出流写入字节，而不必为写入的每个字节导致底层系统的调用

- lBufferedInputStream ：创建BufferedInputStream将创建一个内部缓冲区数组。 当从流中读取或跳过字节时，内部缓冲区将根据需要从所包含的输入流中重新填充，一次很多字节
**构造方法：**

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702214148.png)

> 注意：为什么构造方法参数需要的是文件字节流，而不是具体的文件或者路径呢？
> 字节缓冲流**仅仅提供缓冲区**，而真正的读写数据还得依靠基本的字节流对象（FileInputStream）**进行底层操作**

实例：创建一个对象：


```java
//        FileInputStream fis=new FileInputStream("day04_IO\\test.txt");
//        BufferedInputStream bis=new BufferedInputStream(fis);
//等价于
        BufferedInputStream bi=new BufferedInputStream(new FileInputStream("day04_IO\\test.txt"));

```
**IO流(字节流四种方式复制MP4并测试效率).PNG**

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702213554.png)









<br>

---
<br>


#  <font color=FF8C00>三、字符流</font>
**String的编码与解码**

方法名	----------------------------------------			       说明
- ` byte[] getBytes() 	`				使用平台的默认字符集将该 String编码为一系列字节
- ` byte[] getBytes(String charsetName) `	使用指定的字符集将该 String编码为一系列字节
- `String(byte[] bytes) 	`				使用平台的默认字符集解码指定的字节数组来创建字符串
- `String(byte[] bytes, String charsetName) `通过指定的字符集解码指定的字节数组来创建字符串

应用：字符串中的编码与解码问题
```java

String s="中国";
//编码
byte[] by=s.getBytes();
String s1 = Arrays.toString(by);
System.out.println(s1);//默认UTF-8 [-28, -72, -83, -27, -101, -67]
//解码
String ss=new String(by,"UTF-8");
System.out.println(ss);//中国


//编码
byte[] bys=s.getBytes("GBK");
String s2=Arrays.toString(bys);//[-42, -48, -71, -6]
System.out.println(s2);
//解码
String sss=new String(bys,"GBK");
System.out.println(sss);
```

## 字符流InputStreamReader && OutputStreamWriter
字节流的弊端：
字节流玩中文不是特别方便，读一个打印一个：中文必定乱码,一次读一个数组，打印一个字符串：中文有可能乱码，具体看末尾字节是不是半个.
于是，引入转换流：字节流+编码表

**InputStreamReader** ：是从字节流到字符流的桥梁
**OutputStreamWriter** ：是从字符流到字节流的桥梁
实际进行读写的还是基本字节流对象

```java
OutputStreamWriter osw = new OutputStreamWriter(new 
                     FileOutputStream("myCharStream\\osw.txt"));
```


### OutputStreamWriter
构造方法：

```java
OutputStreamWriter(OutputStream out) 
OutputStreamWriter(OutputStream out, String charsetName) //指定字符编码
```

写数据



![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702210348.png)

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702185029.png)



```java
OutputStreamWriter osw = new OutputStreamWriter(new  FileOutputStream("myCharStream\\osw.txt"));
                     
                     
OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("myCharStream\\osw.txt"),"GBK");
    osw.write("中国");
    osw.close();  
    
```


```java
char[] chs = {'a', 'b', 'c', 'd', 'e'};
  osw.write(chs);//写入一个字符数组
  osw.write(chs, 0, chs.length);
//    osw.write(chs, 1, 3);//写入字符数组的一部分

osw.write("abcde", 1, 3);//写一个字符串的一部分
```

### InputStreamReader 
构造方法

```java
InputStreamReader(InputStream in)
InputStreamReader(InputStream in, String charsetName) //指定字符编码
```
读方法

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702211340.png)







```java
InputStreamReader isr = new InputStreamReader(new  FileInputStream("myCharStream\\osw.txt"),"GBK");
    //read() ：一次读取一个字符数据
    int ch;
    while ((ch=isr.read())!=-1) {
      System.out.print((char)ch);
   }
  

    //int read(char[] chs)：一次读一个字符数组数据
    char[] chs = new char[1024];
    int len;
    while ((len = isr.read(chs)) != -1) {
      System.out.print(new String(chs, 0, len));
   }  
      
  isr.close();              
           
```



### 文件缓冲流 FileWriter &FileReader

InputStreamReader && OutputStreamWriter的子类 



## 字符缓冲流  BufferedWriter &BufferedReader 
<font color=red>注意：一般二者都是一起包裹使用</font>

```java
 BufferedWriter bw=new BufferedWriter(new FileWriter("day05_net\\server.txt"));
```

如果文件不存在，它会自动创建出这个文件，因为它的底层还是调用的<font color=red>字节流对象FileOutPutStream,实际上是它会检查文件是否存在然后创建</font>



![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702211144.png)

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702211411.png)








```java
    BufferedWriter bw = new BufferedWriter(new FileWriter("myCharStream\\bw.txt"));
      bw.write("hello\r\n");
      bw.write("world\r\n");
      bw.close();
    
    BufferedReader br = new BufferedReader(new  FileReader("myCharStream\\bw.txt"));
    //一次读取一个字符数据
     int ch;
     while ((ch=br.read())!=-1) {
         System.out.print((char)ch);
     }

    //一次读取一个字符数组数据
    char[] chs = new char[1024];
    int len;
    while ((len=br.read(chs))!=-1) {
        System.out.print(new String(chs,0,len));
   }
    br.close();

   // 一次读取一行字符数组的数据
    String line;
    while ((line=br.readLine())!=null) {
      System.out.println(line);
   }
    br.close();


```

<br>

---
<br>


#  <font color=FF8C00>四、转换流</font>

## 💜 标准输入输出流


**System 类**中有两个静态的成员变量
- public static final InputStream in ：标准输入流。通常该流对应于**键盘输入**或由主机环境或用户指定的另一个输入源
- public static final PrintStream out ：标准输出流。通常该流对应于**显示输出**或由主机环境或用户指定的另一个输出目标



```java
//public static final InputStream in：标准输入流
//    InputStream is = System.in;
//    int by;
//    while ((by=is.read())!=-1) {
//      System.out.print((char)by);
//    }
    //如何把字节流转换为字符流？用转换流
//    InputStreamReader isr = new InputStreamReader(is);
//    //使用字符流能不能够实现一次读取一行数据呢？可以
//    //但是，一次读取一行数据的方法是字符缓冲输入流的特有方法
//    BufferedReader br = new BufferedReader(isr);

BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
    System.out.println("请输入一个字符串：");
    String line = br.readLine();
    
    System.out.println("请输入一个整数：");
    int i = Integer.parseInt(br.readLine());
    
    //自己实现键盘录入数据太麻烦了，所以Java就提供了一个类供我们使用
    Scanner sc = new Scanner(System.in);
```




## 💜字节打印流PrintStream


**打印流分类**
- 字节打印流： PrintStream
- 字符打印流： PrintWriter

**打印流的特点**
- 只负责输出数据，不负责读取数据
- 永远不会抛出 IOException
- 有自己的特有方法
- 使用自己的特有方法写数据，查看的数据原样输出
- 可以改变输出语句的目的地

public static void setOut(PrintStream out)：重新分配“标准”输出流

```java
PrintStream ps = new PrintStream("myOtherStream\\ps.txt");

//写数据
    //字节输出流有的方法
//    ps.write(97);
    //使用特有方法写数据
//    ps.print(97);
//    ps.println();
//    ps.print(98);
    ps.println(97);
    //释放资源
    ps.close();
```



## 💜字符打印流PrintWriter





![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702211013.png)



```java
PrintWriter pw = new PrintWriter("myOtherStream\\pw.txt");
PrintWriter pw = new PrintWriter(new FileWriter("myOtherStream\\pw.txt"),true);
pw.println("hello");

```

## 对象序列化流ObjectOutputStream 、反序列化流ObjectInputStream
概念：
- 对象序列化：就是将对象保存到磁盘中，或者在网络中传输对象
- 这种机制就是使用一个<font color=red>字节序列</font>表示一个对象，
- 该字节序列包含：对象的类型、对象的数据和对象中存储的属性等信息
- 字节序列写到文件之后，相当于文件中持久保存了一个对象的信息
- 反之，该字节序列还可以从文件中读取回来，<font color=red>重构对象，对它进行反序列化</font>

注意事项
- 一个对象要想被序列化，该对象所属的类必须必须实现 Serializable 接口
- <font color=red>Serializable 是一个标记接口</font>，实现该接口，不需要重写任何方法

对象序列化流： ObjectOutputStream



![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702214219.png)





```java
    //对象序列化
ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("myOtherStream\\oos.txt"));
    //创建对象
    Student s = new Student("林青霞",30);
    //void writeObject(Object obj)：将指定的对象写入ObjectOutputStream
    oos.writeObject(s);
    //释放资源
    oos.close();
    
    //反序列化
    ObjectInputStream ois = new ObjectInputStream(new FileInputStream("myOtherStream\\oos.txt"));
    //Object readObject()：从ObjectInputStream读取一个对象
    Object obj = ois.readObject();
    Student s = (Student) obj;
    System.out.println(s.getName() + "," + s.getAge());
    ois.close();
```


## serialVersionUID&transient
序列化时存在的一些不稳定性问题
-   <font color=blue>**serialVersionUID**</font>

用对象序列化流序列化了一个对象后，假如我们修改了对象所属的类文件，读取数据会不会出问题呢？会出问题，会抛出 <font color=red>InvalidClassException异常</font>

如果出问题了，如何解决呢？
- <font color=blue>重新序列化
给对象所属的类加一个 serialVersionUID
private static final long serialVersionUID = 42L;</font>



![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702214224.png)

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720131300.png)






![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702185037.png)
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702185038.png)





## 💜Properties 集合
参考：[https://blog.csdn.net/yjltx1234csdn/article/details/93769032]()
Properties 介绍
- 属性配置流
- 是一个 Map体系的集合类
- Properties 可以保存到流中或从流中加载
- 属性列表中的每个键及其对应的值都是一个<font color=red>字符串</font>

**作为Map集合的基本用法**

```java

public class PropertiesDemo01 {
  public static void main(String[] args) {
    //创建集合对象
//    Properties<String,String> prop = new Properties<String,String>(); //错
误
    Properties prop = new Properties();
    //存储元素
    prop.put("itheima001", "林青霞");
    prop.put("itheima002", "张曼玉");
    prop.put("itheima003", "王祖贤");
    //遍历集合
    Set<Object> keySet = prop.keySet();
    for (Object key : keySet) {
      Object value = prop.get(key);
      System.out.println(key + "," + value);
   }
 }
}
```
**特殊用处**
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200702185039.png)

```java
 public static void main(String[] args) {
    //创建集合对象
    Properties prop = new Properties();
    //Object setProperty(String key, String value)：设置集合的键和值，都是String类型，底层调用Hashtable方法put
    prop.setProperty("itheima001", "林青霞");
    /*
      Object setProperty(String key, String value) {
        return put(key, value);
      }
      Object put(Object key, Object value) {
        return map.put(key, value);
      }
    */
    prop.setProperty("itheima002", "张曼玉");
    prop.setProperty("itheima003", "王祖贤");
    //String getProperty(String key)：使用此属性列表中指定的键搜索属性
//    System.out.println(prop.getProperty("itheima001"));
//    System.out.println(prop.getProperty("itheima0011"));
//    System.out.println(prop);
    //Set<String> stringPropertyNames()：从该属性列表中返回一个不可修改的键集，其中键及其对应的值是字符串
    Set<String> names = prop.stringPropertyNames();//键集合
    for (String key : names) {
//      System.out.println(key);
      String value = prop.getProperty(key);
      System.out.println(key + "," + value);
   }
 }
}

```

**Properties 和IO流相结合**

```java
public class PropertiesDemo03 {
  public static void main(String[] args) throws IOException {
    //把集合中的数据保存到文件
//    myStore();
    //把文件中的数据加载到集合
    myLoad();
 }
  private static void myLoad() throws IOException {
    Properties prop = new Properties();
    //void load(Reader reader)：
    FileReader fr = new FileReader("myOtherStream\\fw.txt");
    prop.load(fr);
    fr.close();
    System.out.println(prop);
 }
  private static void myStore() throws IOException {
    Properties prop = new Properties();
    prop.setProperty("itheima001","林青霞");
    prop.setProperty("itheima002","张曼玉");
    prop.setProperty("itheima003","王祖贤");
    //void store(Writer writer, String comments)：
    FileWriter fw = new FileWriter("myOtherStream\\fw.txt");
    prop.store(fw,null);
    fw.close();
 }
 }

```






<br>

---
<br>



#  <font color=FF8C00>五、练习</font>
### 练习1：集合到文件
把ArrayList集合中的学生数据写入到文本文件。要求：每一个学生对象的数据作为文件中的一行数据 格式：
学号,姓名,年龄,居住地 举例：itheima001,林青霞,30,西安
```java

public class ArrayListToFileDemo {
    public static void main(String[] args) throws IOException {
        //创建ArrayList集合存储学生对象
        ArrayList<Student> array=new ArrayList<Student>();

        Student s1=new Student("20171303011","林青霞",18,"武汉");
        Student s2=new Student("20171345658","张曼玉",25,"海南");
        Student s3=new Student("20171389655","王祖贤",36,"北京");

        array.add(s1);
        array.add(s2);
        array.add(s3);

//创建字符缓冲输出流对象
        BufferedWriter bw=new BufferedWriter(new FileWriter("day04_IO\\namelist.txt",true));

////遍历集合,把学生对象的数据拼接成指定格式的字符串
        for (Student student:array){
            StringBuilder sb=new StringBuilder();
            sb.append(student.getSid()+","+student.getName()+","+
                    student.getAge()+","+student.getAddress());

            bw.write(sb.toString());
            bw.newLine();
            bw.flush();
        }
        bw.close();
    }
}

```
### 练习2：文件到集合
把文本文件中的数据读取到集合中，并遍历集合。要求：文件中每一行数据是一个学生对象的成员变量值
 举例：itheima001,林青霞,30,西安
把读取到的字符串数据用 split()进行分割，得到一个字符串数组
把字符串数组中的每一个元素取出来对应的赋值给学生对象的成员变量值


```java

public class FileToArrayListDemo {
    public static void main(String[] args) throws IOException {
        //创建字符缓冲输入流对象
        BufferedReader br=new BufferedReader(new FileReader("day04_IO\\namelist.txt"));

        ArrayList<Student> array=new ArrayList<Student>();
        //调用字符缓冲输入流对象的方法读数据
        String line;
        while ((line=br.readLine())!=null){
            String[] straray=line.split(",");

            Student student=new Student();
        //把字符串数组中的每一个元素取出来对应的赋值给学生对象的成员变量值
        //itheima001,林青霞,30,西安
            student.setSid(straray[0]);
            student.setName(straray[1]);
            student.setAge(Integer.parseInt(straray[2]));
            student.setAddress(straray[3]);

            array.add(student);
        }

        br.close();

        //遍历集合
        for (Student student:array){
            System.out.println(student.getSid() + "," + student.getName() + "," +student.getAge() + "," + student.getAddress());
        }
    }
}

```


### 练习3：从文本中随机读取人名
文件到集合：**点名器**
```java

public class CallNameDemo {
    public static void main(String[] args) throws IOException {
        //创建字符缓冲输入流对象
        BufferedReader br=new BufferedReader(new FileReader("day04_IO\\namelist.txt"));
        //创建ArrayList集合对象
        ArrayList<String> array=new ArrayList<String>();
        //调用字符缓冲输入流对象的方法读数据
        String line;
        while ((line=br.readLine())!=null){
            array.add(line);
        }
        br.close();

        //随机数读取集合
        Random r=new Random();
        int index=r.nextInt(array.size());
        String name = array.get(index);
        System.out.println(name);


    }
}

```


### 练习4：复制单级文件夹
```java
/**
 * 复制单级目录
 *  要求：
 *         1. 封装目录 sourceDir
 *         2. 获取该目录下的所有文件 file
 *         3. 创建新的目的地 destDir
 *         4. 复制源路径的每一个文件
 *         5. 把原文件读取并复制到新目录下
 */
public class CopyFolderDemo {
    public static void main(String[] args) throws IOException {
        // 源目录
        File sourceDir = new File("G:\\data\\d");
        // 新目录
        File destDir = new File("G:\\test");
        // 如果目标目录不存在，就创建
        if (!destDir.exists()){
            destDir.mkdir();
        }

        //遍历源目录下的文件
        File[] sourceFiles = sourceDir.listFiles();
        for (File sourceFile : sourceFiles){
            // 创建目标文件
            String fileName = sourceFile.getName();
            File destFile = new File(destDir,fileName);
            // 复制源文件到新文件
            copyFile(sourceFile,destFile);

        }

        // 遍历目录下文件
        travelFile(destDir);

    }

    /**
     * 使用字节流读取源文件
     * 使用字节流写出目标文件
     */
  private static void copyFile(File sourceFile,File destFile) throws IOException {
      BufferedInputStream bis = new BufferedInputStream(new FileInputStream(sourceFile));
      BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(destFile));

      byte[] bys = new byte[1024];
      int len = 0;
      while ((len = bis.read(bys)) != -1){
          bos.write(bys,0,len);
      }

      bis.close();
      bos.close();

    }

    /**
     * 遍历
     */
    private static void travelFile(File srcfile){
        File[] fileList = srcfile.listFiles();

        if (fileList != null){
            for (File file : fileList){
                if (file.isDirectory()){
                    travelFile(file);
                }else {
                    System.out.println(file.getName());
                }
            }
        }else{
            System.out.println("该目录下没有任何内容");
        }
    }

}

```




### 练习5：复制指定目录下指定后缀名的文件并修改名称
```java
/**
 * 复制单级目录下指定后缀名的文件
 *  要求：
 *        1. 封装源目录
 *        2. 获取源目录下的所有以 .java后缀文件集合
 *        3. 将源文件复制到目的地mulu
 *        4. 在目的地改名
 */
public class CopyFolderFilter {
    public static void main(String[] args) throws IOException {
        File sourceDir = new File("G:\\data\\d");
        File destDir = new File("G:\\test");

        File[] sourceFiles = sourceDir.listFiles(new FilenameFilter() {
            @Override
            public boolean accept(File dir, String name) {
                return new File(dir, name).isFile() && name.endsWith(".java");
            }
        });

        //复制文件
        for (File sourceFile : sourceFiles){
            String fileName = sourceFile.getName();
            File destFile = new File(destDir,fileName);

            copyFile(sourceFile,destFile);
        }
        // 在目的地改名
        File[] destFiles = destDir.listFiles();
        for (File destFile : destFiles){
            String fileName = destFile.getName();
            String newName = fileName.replace(".java", ".jad");

            File newFile = new File(destDir,newName);

            destFile.renameTo(newFile);

        }


        //遍历
        travelFile(destDir);
    }

    /**
     * 使用字节流读取源文件
     * 使用字节流写出目标文件
     */
    private static void copyFile(File sourceFile, File destFile) throws IOException {
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream(sourceFile));
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(destFile));

        byte[] bys = new byte[1024];
        int len = 0;
        while ((len = bis.read(bys)) != -1){
            bos.write(bys,0,len);
        }

        bis.close();
        bos.close();

    }

    /**
     * 遍历
     */
    private static void travelFile(File srcfile){
        File[] fileList = srcfile.listFiles();

        if (fileList != null){
            for (File file : fileList){
                if (file.isDirectory()){
                    travelFile(file);
                }else {
                    System.out.println(file.getName());
                }
            }
        }else{
            System.out.println("该目录下没有任何内容");
        }
    }
    
    
}

```



### 练习6：复制多级文件夹

```java
/**
 * 复制多级目录
 * 要求：
 *      1. 如果源文件是一个目录
 *              以改文件名在目的地创建一个目录
 *              遍历源文件下的所有文件
 *              重复 1
 *      2. 如果源文件是一个文件
 *             复制文件
 */
public class CopyFoldersDemo2 {
    public static void main(String[] args) throws IOException {
        File sourceDir = new File("G:\\data");
        File destDir = new File("G:\\test");
        // 复制多级目录
        copyFolders(sourceDir,destDir);
        // 遍历多级目录
        travelFile(destDir);


    }
    public static void copyFolders(File sourceFile,File destFile) throws IOException {

        //1. 判断根目录是否为目录
        if (sourceFile.isDirectory()){
            // 1.1 以该名称创建一个目录
            File newFolder = new File(destFile,sourceFile.getName());
            newFolder.mkdir();

            // 1.2 遍历该目录下的所有文件
            File[] SourceFiles = sourceFile.listFiles();
            for (File file : SourceFiles){

                   copyFolders(file,newFolder);

            }

        }else{
            File newFile = new File(destFile,sourceFile.getName());
            copyFile(sourceFile,newFile);
        }



    }
    /**
     * 使用字节流读取源文件
     * 使用字节流写出目标文件
     */
    private static void copyFile(File sourceFile, File destFile) throws IOException {
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream(sourceFile));
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream(destFile));

        byte[] bys = new byte[1024];
        int len = 0;
        while ((len = bis.read(bys)) != -1){
            bos.write(bys,0,len);
        }

        bis.close();
        bos.close();

    }


    /**
     * 遍历
     */
    private static void travelFile(File srcfile){
        File[] fileList = srcfile.listFiles();

        if (fileList != null){
            for (File file : fileList){
                if (file.isDirectory()){
                    travelFile(file);
                }else {
                    System.out.println(file.getName());
                }
            }
        }else{
            System.out.println("该目录下没有任何内容");
        }
    }

}

```




### 练习7：键盘录入学生信息按照总分排序并写入文本文件
```java
/**
 * 键盘录入5个学生信息(姓名,语文成绩,数学成绩,英语成绩),按照总分从高到低存入文本文件
 *
 * 分析：
 * 		A:创建学生类
 * 		B:创建集合对象
 * 			TreeSet<Student>
 * 		C:键盘录入学生信息存储到集合
 * 		D:遍历集合，把数据写到文本文件
 */
public class StudentDemo {
    public static void main(String[] args) throws IOException {
        // 创建集合对象
        TreeSet<Student> ts = new TreeSet<>(new Comparator<Student>() {
            @Override
            public int compare(Student s1, Student s2) {
                int num = s2.getSum() - s1.getSum();
                int num2 = num == 0 ? s2.getChinese() - s1.getChinese():num;
                int num3 = num2 == 0 ? s2.getMath() - s1.getMath():num2;
                int num4 = num3 == 0 ? s2.getEnglish() - s1.getEnglish():num3;
                int num5 = num4 == 0 ? s2.getName().compareTo(s1.getName()):num4;

                return num5;
            }
        });

        // 键盘录入学生信息存储到集合
        for (int i = 0; i < 3; i++){
            Scanner sc = new Scanner(System.in);
            System.out.println("请输入学生姓名：");
            String name = sc.nextLine();
            System.out.println("请输入语文成绩：");
            int chinese = sc.nextInt();
            System.out.println("请输入数学成绩：");
            int math = sc.nextInt();
            System.out.println("请输入英语成绩：");
            int english = sc.nextInt();

            Student student = new Student(name,chinese,math,english);
            ts.add(student);

        }

        // 遍历集合，把数据写到文本当中
        BufferedWriter bw = new BufferedWriter(new FileWriter("G:\\data\\d\\hhh.txt"));
        bw.flush();
        for (Student s : ts){
            StringBuilder sb = new StringBuilder();
            sb.append(s.getName() + "," + s.getChinese() + "," + s.getMath() + "," + s.getEnglish());
            bw.write(sb.toString());
            bw.newLine();
            bw.flush();
        }
        bw.close();
        System.out.println("信息存储完毕~");


    }
}

```

### 练习8：把一个文件中的字符串排序后再写入另一个文件

```java
/**
 * 已知s.txt文件中有这样的一个字符串：“hcexfgijkamdnoqrzstuvwybpl”
 * 请编写程序读取数据内容，把数据排序后写入ss.txt中。
 *
 * 分析：
 * 		A:把 a.txt这个文件给做出来
 * 		B:读取该文件的内容，存储到一个字符串中
 * 		C:把字符串转换为字符数组
 * 		D:对字符数组进行排序
 * 		E:把排序后的字符数组转换为字符串
 * 		F:把字符串再次写入b.txt中
 *
**/
public class StringSortDemo {
    public static void main(String[] args) throws IOException {
        // 读取该文件的内容，存储到一个字符串中
        BufferedReader br = new BufferedReader(new FileReader("G:\\data\\a.txt"));
        String line = br.readLine();
        char[] chars = line.toCharArray();

        // 对字符数组进行排序
        Arrays.sort(chars);
        // 把排序后的字符数组转换为字符串
        String s = new String(chars);
        
        // 把字符串再次写入b.txt中
        BufferedWriter bw = new BufferedWriter(new FileWriter("G:\\data\\b.txt"));
        bw.write(s);
        bw.newLine();
        bw.flush();

        br.close();
        bw.close();

    }
}

```
---

### 练习9：自定义类模拟BufferedReader的readLine()功能

```java
/*
 * 用Reader模拟BufferedReader的readLine()功能
 * 
 * readLine():一次读取一行，根据换行符判断是否结束，只返回内容，不返回换行符
 */
public class MyBufferedReader {
	private Reader r;

	public MyBufferedReader(Reader r) {
		this.r = r;
	}

	/*
	 * 思考：写一个方法，返回值是一个字符串。
	 */
	public String readLine() throws IOException {
		/*
		 * 我要返回一个字符串，我该怎么办呢? 我们必须去看看r对象能够读取什么东西呢? 两个读取方法，一次读取一个字符或者一次读取一个字符数组
		 * 那么，我们要返回一个字符串，用哪个方法比较好呢? 我们很容易想到字符数组比较好，但是问题来了，就是这个数组的长度是多长呢?
		 * 根本就没有办法定义数组的长度，你定义多长都不合适。 所以，只能选择一次读取一个字符。
		 * 但是呢，这种方式的时候，我们再读取下一个字符的时候，上一个字符就丢失了 所以，我们又应该定义一个临时存储空间把读取过的字符给存储起来。
		 * 这个用谁比较和是呢?数组，集合，字符串缓冲区三个可供选择。
		 * 经过简单的分析，最终选择使用字符串缓冲区对象。并且使用的是StringBuilder
		 */
		StringBuilder sb = new StringBuilder();

		// 做这个读取最麻烦的是判断结束，但是在结束之前应该是一直读取，直到-1
		
		
		/*
		hello
		world
		java	
		
		104101108108111
		119111114108100
		1069711897
		 */
		
		int ch = 0;
		while ((ch = r.read()) != -1) { //104,101,108,108,111
			if (ch == '\r') {
				continue;
			}

			if (ch == '\n') {
				return sb.toString(); //hello
			} else {
				sb.append((char)ch); //hello
			}
		}

		// 为了防止数据丢失，判断sb的长度不能大于0
		if (sb.length() > 0) {
			return sb.toString();
		}

		return null;
	}

	/*
	 * 先写一个关闭方法
	 */
	public void close() throws IOException {
		this.r.close();
	}
}

```

```java
/*
 * 测试MyBufferedReader的时候，你就把它当作BufferedReader一样的使用
 */
public class MyBufferedReaderDemo {
	public static void main(String[] args) throws IOException {
		MyBufferedReader mbr = new MyBufferedReader(new FileReader("my.txt"));

		String line = null;
		while ((line = mbr.readLine()) != null) {
			System.out.println(line);
		}

		mbr.close();

		// System.out.println('\r' + 0); // 13
		// System.out.println('\n' + 0);// 10
	}
}

```



### 练习10：自定义类模拟LineNumberReader的获取行号功能

```java
/*
 * BufferedReader
 * 		|--LineNumberReader
 * 			public int getLineNumber()获得当前行号。 
 * 			public void setLineNumber(int lineNumber)
 */
public class LineNumberReaderDemo {
	public static void main(String[] args) throws IOException {
		LineNumberReader lnr = new LineNumberReader(new FileReader("my.txt"));

		// 从10开始才比较好
		// lnr.setLineNumber(10);

		// System.out.println(lnr.getLineNumber());
		// System.out.println(lnr.getLineNumber());
		// System.out.println(lnr.getLineNumber());

		String line = null;
		while ((line = lnr.readLine()) != null) {
			System.out.println(lnr.getLineNumber() + ":" + line);
		}

		lnr.close();
	}
}
```


**==自定义获取行号的工具类==**
```java
public class MyLineNumberReader {
	private Reader r;
	private int lineNumber = 0;

	public MyLineNumberReader(Reader r) {
		this.r = r;
	}

	public int getLineNumber() {
		// lineNumber++;
		return lineNumber;
	}

	public void setLineNumber(int lineNumber) {
		this.lineNumber = lineNumber;
	}

	public String readLine() throws IOException {
		lineNumber++;

		StringBuilder sb = new StringBuilder();

		int ch = 0;
		while ((ch = r.read()) != -1) {
			if (ch == '\r') {
				continue;
			}

			if (ch == '\n') {
				return sb.toString();
			} else {
				sb.append((char) ch);
			}
		}

		if (sb.length() > 0) {
			return sb.toString();
		}

		return null;
	}

	public void close() throws IOException {
		this.r.close();
	}
}

```

```java
public class MyLineNumberReader2 extends MyBufferedReader {
	private Reader r;

	private int lineNumber = 0;

	public MyLineNumberReader2(Reader r) {
		super(r);
	}

	public int getLineNumber() {
		return lineNumber;
	}

	public void setLineNumber(int lineNumber) {
		this.lineNumber = lineNumber;
	}

	@Override
	public String readLine() throws IOException {
		lineNumber++;
		return super.readLine();
	}
}

```


```java
public class MyLineNumberReaderTest {
	public static void main(String[] args) throws IOException {
		// MyLineNumberReader mlnr = new MyLineNumberReader(new FileReader(
		// "my.txt"));

		MyLineNumberReader2 mlnr = new MyLineNumberReader2(new FileReader(
				"my.txt"));

		// mlnr.setLineNumber(10);

		// System.out.println(mlnr.getLineNumber());
		// System.out.println(mlnr.getLineNumber());
		// System.out.println(mlnr.getLineNumber());

		String line = null;
		while ((line = mlnr.readLine()) != null) {
			System.out.println(mlnr.getLineNumber() + ":" + line);
		}

		mlnr.close();
	}
}

```







# 六、IO流面试题



### 6.1 什么是Java序列化，如何实现Java序列化？

序列化就是一种用来处理对象流的机制，将对象的内容进行流化。可以对流化后的对象进行读写操作，可以将流化后的对象传输于网络之间。序列化是为了解决在对象流读写操作时所引发的问题
序列化的实现：将需要被序列化的类实现Serialize接口，没有需要实现的方法，此接口只是为了标注对象可被序列化的，然后使用一个输出流（如：FileOutputStream）来构造一个ObjectOutputStream(对象流)对象，再使用ObjectOutputStream对象的write(Object obj)方法就可以将参数obj的对象写出




### 6.2 PrintStream、BufferedWriter、PrintWriter的比较?

> PrintStream类的输出功能非常强大，通常如果需要输出文本内容，都应该将输出流包装成PrintStream后进行输出。
>
> 与其他输出流不同，PrintStream 永远不会抛出 IOException；而是，异常情况仅设置可通过 checkError 方法测试的内部标志。另外，为了自动刷新，可以创建一个 PrintStream.**我们常用的System.out.println()方法其中System的静态成员out就是一个PrintStream类型。**
>
> PrintWriter的println方法自动添加换行，不会抛异常，若关心异常，需要调用checkError方法看是否有异常发生，PrintWriter构造方法可指定参数，实现自动刷新缓存（autoflush）
>
> BufferedWriter:将文本写入字符输出流，缓冲各个字符从而提供单个字符，数组和字符串的高效写入。通过write()方法可以将获取到的字符输出，然后通过newLine()进行换行操作。BufferedWriter中的字符流必须通过调用flush方法才能将其刷出去。并且BufferedWriter只能对字符流进行操作。如果要对字节流操作，则使用BufferedInputStream
> 





### 6.3 . 什么是节点流,什么是处理流,它们各有什么用处,处理流的创建有什么特征？

1. 节点流 直接与数据源相连，用于输入或者输出
2. 处理流：在节点流的基础上对之进行加工，进行一些功能的扩展
3. 处理流的构造器必须要 传入节点流的子类







### 6.4 流一般需要不需要关闭, 如果有多个流互相调用传入是怎么关闭的？

1. 流一旦打开就必须关闭，使用close方法
2. 放入finally语句块中（finally 语句一定会执行）
3. 调用的处理流就关闭处理流
4. 多个流互相调用只关闭最外层的流



### 6.5  InputStream里的read()返回的是什么,read(byte[] data)返回的是什么值？

1. 返回的是所读取的字节的int型（范围0-255）
2. read（byte [ ] data）将读取的字节储存在这个数组。返回的就是传入数组参数个数





### 6.6 NIO与IO的区别

原文链接：https://blog.csdn.net/zengxiantao1994/article/details/88094910





### 6.7 字节流与字符流有什么区别？



答：计算机中的一切最终都是以二进制字节形式存在的，对于我们经常操作的字符串，在写入时其实都是先得到了其对应的字节，然后将字节写入到输出流，在读取时其实都是先读到的是字节，然后将字节直接使用或者转换为字符给我们使用。由于对于字节和字符两种操作的需求比较广泛，所以 Java 专门提供了字符流与字节流相关IO类。

对于程序运行的底层设备来说永远都只接受字节数据，所以当我们往设备写数据时无论是字节还是字符最终都是写的字节流。字符流是字节流的包装类，所以当我们将字符流向字节流转换时要注意编码问题（因为字符串转成字节数组的实质是转成该字符串的某种字节编码）。

字符流和字节流的使用非常相似，但是实际上字节流的操作不会经过缓冲区（内存）而是直接操作文本本身的，而字符流的操作会先经过缓冲区（内存）然后通过缓冲区再操作文件。
