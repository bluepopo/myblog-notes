【Spring】框架总结



# （ 一） spring概述简介入门

## spring是什么

Spring为简化企业级开发而生，使用Spring开发可以将Bean对象，Dao组件对象，Service组件对象等交给Spring容器来管理，这样使得很多复杂的代码在Spring中开发却变得非常的优雅和简洁，有效的降低代码的耦合度，极大的方便项目的后期维护、升级和扩展。

 Spring是一个**IOC(DI**)和**AOP**容器框架。

. **Spring的优良特性**

> **[1]**非侵入式：基于Spring开发的应用中的对象可以不依赖于Spring的API
>
> **[2] 控制反转：IOC——Inversion of Control，指的是将对象的创建权交给Spring去创建。**使用Spring之前，对象的创建都是由我们自己在代码中new创建。而使用Spring之后。对象的创建都是由给了Spring框架。
>
> **[3]** 依赖注入：DI——Dependency Injection，是指依赖的对象不需要手动调用setXX方法去设置，而是通过配置赋值。
>
> **[4]** **面向切面编程：Aspect Oriented Programming——AOP**
>
> **[5]** **容器：Spring是一个容器，因为它包含并且管理应用对象的生命周期**
>
> **[6]** 组件化：Spring实现了使用简单的组件配置组合成一个复杂的应用。在 Spring 中可以使用XML和Java注解组合这些对象。
>
> **[7]** 一站式：在IOC和AOP的基础上可以整合各种企业应用的开源框架和优秀的第三方类库（实际上Spring 自身也提供了表述层的SpringMVC和持久层的Spring JDBC）

> 
> 原文链接：https://blog.csdn.net/weixin_42405670/article/details/83048002



## spring的两大核心

控制反转 ioc 与面向切面编程 AOP 

## spring的发展的两大优势







## spring的体系结构



![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200704230508.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

# （二）程序的耦合和解耦
## 2.1 什么是程序的耦合
**耦合**: 程序间的依赖关系。我们在开发中，有些依赖关系是必须的，有些依赖关系可以通过优化代码来解除的。在开发中,应该做到解决编译期依赖,即编译期的不依赖，运行时的才依赖。
**解耦的思路:** 使用反射来创建对象,而避免使用`new 关键字`并通过读取配置文件来获取要创建的对象全限定类名.


> 请看下面的示例代码：
>
> ```java /**
> * 账户的业务层实现类
> */ public class AccountServiceImpl implements IAccountService {
> 
> 		 private IAccountDao accountDao = new AccountDaoImpl(); 
> 
>  } 
> ```
> 上面的代码表示：
> 业务层调用持久层，并且此时业务层在依赖持久层的接口和实现类。如果此时没有持久层实现类，编译将不能通过。
>
> 这种编译期依赖关系，应该在我们开发中杜绝。我们需要优化代码解决。

再比如：
早期我们的 JDBC 操作，注册驱动时，我们为什么不使用 DriverManager 的 register 方法，而是采
用 Class.forName 的方式？

> ```java 
> //1.注册驱动 
> /*  DriverManager.registerDriver(new com.mysql.jdbc.Driver());  */
>  Class.forName("com.mysql.jdbc.Driver");
> //2.获取连接
> //3.获取预处理sql语句对象
> //4.获取结果集
> //5.遍历结果集
> ```
> **原因就是：**
> 我们的类依赖了数据库的具体驱动类（MySQL），如果这时候更换了数据库品牌（比如 Oracle），           需要修改源码来重新数据库驱动。这显然不是我们想要的。


## 2.2 解决程序耦合的思路
如上代码，当是我们讲解 jdbc 时，是通过**反射**来注册驱动的，代码如下：

` ·Class.forName("com.mysql.jdbc.Driver");//此处只是一个字符串· `
此时的好处是，我们的类中不再依赖具体的驱动类，此时就算删除 mysql 的驱动 jar 包，依然可以编译（运行就不要想了，没有驱动不可能运行成功的）。

同时，也产生了一个新的问题，mysql 驱动的`全限定类名字符串`是在 java 类中写死的，一旦要改还是要修改源码。
== 解决这个问题也很简单，不使用字符串指定，而使用配置文件配置。 ==


## 2.3 工厂模式解耦
  在实际开发中我们可以把**三层的对象**都使用配置文件配置起来，当启动服务器应用加载的时候，让一个类中的方法通过读取配置文件，把这些对象创建出来 `并存起来`。在接下来的使用的时候，直接拿过来用就好了。
  那么，这个读取配置文件，创建和获取三层对象的类就是工厂。

  

##  2.4 解耦实例: UI层,Service层,Dao层 三层的调用

在Web项目中,UI层,Service层,Dao层之间有着前后调用的关系.层间存在着 使用` new 关键字` 的较强耦合.
解决方案：使用读取外部配置文件 bean.properties 在运行时加载所需类


> ###  （一） 外部配置文件
> 外部引入配置文件 ` bean.properties`
>
> ```java
> accountService=code.service.impl.AccountServiceImpl
> accountDao=code.dao.impl.AccountDaoImpl 
> ```
> 配置文件中写明 （key , value），名称和**实现类的全限定类名**。

---------

> ### （二）编写工厂类 BeanFactory 
> 创建一个Bean对象的工厂
> ` Bean：`可重用组件含义。  `JavaBean：`用Java语言编写的可重用组件。
>  `  JavaBean > 实体类 `   它就是创建我们的service和dao对象的。  
>  第一步：需要一个配置文件来配置我们的service和dao
>   <font color=#008000>  *配置的内容：唯一标志  = 权限定类名 (key = value)*</font> 
>  第二步：通过读取配置文件中配置的内容，反射创建对象。  我的配置文件可以是xml也可以是properties

```java

public class BeanFactory {
    //定义一个Properties对象
    private static Properties props;

    //使用静态代码块为 Properties对象赋值
    static {
        try {
            //实例化对象
            props =new Properties();
            //获取properties文件的流对象
            InputStream in = BeanFactory.class.getClassLoader().getResourceAsStream("bean.properties");
            props.load(in);

        }catch (Exception e){
            throw new ExceptionInInitializerError("初始化properties失败！");
        }

    }

/**
 * 获取 Bean 对象
 */
public static Object getBean(String beanName){
    Object bean=null;
    String beanPath = props.getProperty(beanName);
    try {
        bean = Class.forName(beanPath).newInstance();//创建一个新的实例
    } catch (Exception e) {
        e.printStackTrace();
    }
    return bean;
}

}

```
-------


>
>### （三）表现层 Client类
>```java 
>/**  * 模拟一个表现层  */ 
>public class Client {
>public static void main(String[] args) { 
>//        IAccountService accountService=new AccountServiceImpl();
>    IAccountService accountService = (IAccountService) BeanFactory.getBean("accountService");
>    accountService.saveAccount();
>    } 
>}
>```
>调用service要求服务

---------

>
> ### （四）业务实现层 AccountService
> ```Java
> /**
> * 账户的业务层实现类
> */
> public class AccountServiceImpl implements IAccountService {
> //    IAccountDao accountDao = new AccountDaoImpl();
>    IAccountDao accountDao = >(IAccountDao)BeanFactory.getBean("accountDao");
> 
>    public void saveAccount() {
>        accountDao.saveAccount();
>    }
> }
> ```
> 调用 dao层实现服务

------
> ### （五）持久层 AccountDao

> ```java 
> /**  * 持久层的实现类  */ 
> public class AccountDaoImpl implements
> IAccountDao {
> 
>  public void saveAccount() {
>      System.out.println("保存了账户");
> 
>    } 
>  } 
> ```
>
> JDBC 操作数据库

------

## 2.5 工厂模式解耦存在的问题与解决方案

**问题**：每次使用`BeanFactory`工厂生成出Bean对象都是不同的，即便是使用相同的全限定类名。

```java
  public static void main(String[] args) {
 for (int i =0;i<5;i++){
            IAccountService accountService = (IAccountService) BeanFactory.getBean("accountService");
            
            System.out.println(accountService);  //打印的对象地址各不相同 
            
            accountService.saveAccount();
        }
}
```
**解决方案**：工厂内置一个`容器`保存 生产的 `Obeject对象`,每次根据名称提供一个Bean对象，名称相同时，提供相同的对象。
` //定义一个Map。用于存放我们要创建的对象，我们把它称之为容器`
`    private static Map<String,Object>  beans;`

​         <font color=#006C11 >  //实例化容器</font>
​            beans = new HashMap<String, Object>();

​           <font color=#006C11 > //取出配置文件中所有的Key</font>
​            Enumeration  keys=props.keys();
​            <font color=#006C11 >//遍历枚举</font>
​            while (keys.hasMoreElements()){
​               <font color=#006C11 > //取出每个key</font>
​                String key=keys.nextElement().toString();
​              <font color=#006C11 >  //根据key获取value</font>
​                String beanPath = props.getProperty(key);
​               <font color=#006C11 > //反射创建对象</font>
​                Object value = Class.forName(beanPath).newInstance();
​               <font color=#006C11 > //把key和value存入容器中</font>
​                beans.put(key,value);

```java
public class BeanFactory {
    //定义一个Properties对象
    private static Properties props;

    //定义一个Map。用于存放我们要创建的对象，我们把它称之为容器
    private static Map<String,Object> beans;

    //使用静态代码块为 Properties对象赋值
    static {
        try {
            //实例化对象
            props =new Properties();
            //获取properties文件的流对象
            InputStream in = BeanFactory.class.getClassLoader().getResourceAsStream("bean.properties");
            props.load(in);
            //实例化容器
            beans = new HashMap<String, Object>();
            //取出配置文件中所有的Key
            Enumeration  keys=props.keys();
            //遍历枚举
            while (keys.hasMoreElements()){
                //取出每个key
                String key=keys.nextElement().toString();
                //根据key获取value
                String beanPath = props.getProperty(key);
                //反射创建对象
                Object value = Class.forName(beanPath).newInstance();
                //把key和value存入容器中
                beans.put(key,value);
           }
        }catch (Exception e){
            throw new ExceptionInInitializerError("初始化properties失败！");
        }
    }

/**
 * 获取 Bean 对象
 */
public static Object getBean(String beanName){

    return beans.get(beanName); //获取的是单例的对象
  }
}

```

运行结果：
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200704224531.png)





# （三）控制反转IOC和bean标签





## 3.1 控制反转-Inversion Of Control
工厂是什么？
工厂就是负责给我们从容器中获取指定对象的类。这时候我们获取对象的方式发生了改变。
原来：我们在获取对象时，都是采用 new 的方式。`是主动的。`
现在：我们获取对象时，同时跟工厂要，有工厂为我们查找或者创建对象。`是被动的。`

<font color=#006C11>**这种被动接收的方式获取对象的思想就是控制反转，它是 spring 框架的核心之一。**</font>

IOC简介：

`控制反转（Inversion of Control，缩写为IoC）`	，是面向对象编程中的一种设计原则，`可以用来减低计算机代码之间的耦合度`。其中最常见的方式叫做`依赖注入（Dependency Injection，简称DI）`，还有一种方式叫`“依赖查找”（Dependency Lookup）`。
	通过控制反转，对象在被创建的时候，由一个调控系统内所有对象的外界实体将其所依赖的对象的引用传递给它。也可以说，依赖被注入到对象中.
	
**明确 ioc 的作用：**
削减计算机程序的耦合(解除我们代码中的依赖关系)。



## 3.2 使用 spring 的  IOC 解决程序耦合
### Spring基于xml的ioc环境搭建


#### 一、pom.xml文件导入依赖

```xml
<dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
</dependencies>
```
####  二、Bean.xml文件配置
```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

<!--    把对象的创建交给Spring来管理-->
    <bean id="accountService" class="code.service.impl.AccountServiceImpl"></bean>
    <bean id="accountDao" class="code.dao.impl.AccountDaoImpl"></bean>
</beans>
```


####  三、dao、Service 的代码

> ***持久层的实现类：AccountDao*** 
>
> ```java 
> 
> public class AccountDaoImpl implements IAccountDao {
>  public void saveAccount() {
>      System.out.println("保存了账户");
>     } 
>  } 
> ```
>
> ***账户的业务层实现类 : AccountService***
>
> ```java 
> /**  * 账户的业务层实现类  */ 
> public class AccountServiceImpl implements IAccountService {
>  IAccountDao accountDao = new AccountDaoImpl();
>  public void saveAccount() {
>      accountDao.saveAccount();
>      } 
>  }
> ```


####  四、表现层通过Spring核心对象`ApplicationContext `,读取XML配置文件,创建对象


>***表现层UI：获取Spring的IOC核心容器，并根据id获取对象***
>
>```java 
>/**  * 获取Spring的IOC核心容器，并根据id获取对象  */ 
>public class Client {
>public static void main(String[] args) {
>   //1.获取核心容器对象
>    ApplicationContext ac =new ClassPathXmlApplicationContext("bean.xml");
>    
>   // 2.根据id获取bean对象
>   IAccountService as=(IAccountService) ac.getBean("accountService");
>    IAccountDao adao= (IAccountDao)ac.getBean("accountDao");
>
>    System.out.println(as);
>    System.out.println(adao);
>
>    as.saveAccount();
>
>   } 
>}
>```
>
>执行结果：
>![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200704230520.png)


## 3.3 spring  中工厂的类结构图
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200704230525.png)
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200704230534.png)

### 一、BeanFactory 和 ApplicationContext  的区别

BeanFactory 才是 Spring 容器中的顶层接口。
ApplicationContext 是它的子接口。
`BeanFactory 和 ApplicationContext 的区别：`

 - 创建对象的时间点不一样。
- `ApplicationContext`：它在构建核心容器时，创建对象采取的策略是采用 `立即加载 `的方式。也就是说，只要一读取完配置文件，马上就创建配置文件中配置的对象。`适用于单例对象。`
- 
-  `BeanFactory`：`延迟加载`，什么使用什么时候创建对象。`适用于多例对象。`
- 
示例代码：BeanFactory创建bean对象
```java
  //--------------BeanFactory-------------------------
        Resource resource =  new ClassPathResource("bean.xml");
        BeanFactory factory = new XmlBeanFactory(resource);
        IAccountService as = (IAccountService) factory.getBean("accountService");
        System.out.println(as);
```



### 二、 ApplicationContext  接口 的三个实现类
`ClassPathXmlApplicationContext ：`它是从类的根路径下加载配置文件 推荐使用这种
`FileSystemXmlApplicationContext ：`它是从磁盘路径上加载配置文件，配置文件可以在磁盘的任意位置。
`AnnotationConfigApplicationContext:`当我们使用注解配置容器对象时，需要使用此类来创建 spring 容器。它用来读取注解。

## 3.4 IOC 中bean标签
把对象的创建交给spring来管理-->
  `spring对bean的管理细节-->`


 - 1.创建bean的三种方式-->
 -  2.bean对象的作用范围-->
 -  3.bean对象的生命周期-->



`创建Bean的三种方式`

   <font color=red>第一种方式：使用默认构造函数创建。</font>
            在spring的配置文件中使用Bean标签，配以 id 和class属性之后，且没有其他属性和标签时，
            采用的就是默认构造函数的创建Bean对象，此时如果类中没有默认构造函数，则无法创建。
```java
    <bean id="accountService" class="code.service.impl.AccountServiceImpl"></bean>
    <bean id="accountDao" class="code.dao.impl.AccountDaoImpl"></bean>
```

----

<font color=red>  第二种方式：使用普通工厂中的方法创建对象（使用某个类的方法创建对象），并存入spring容器</font>

```java
    <bean id="instanceFactory" class="code.factory.InstanceFactory"></bean>
    <bean id="accountService" factory-bean="instanceFactory" factory-method="getAccountService"></bean>
```

---
<font color=red>第三种方式，使用工厂中的静态方法创建对象（使用某个类中的方法创建对象，并存入spring容器）</font>
```java

    <bean id="accountService"  class="code.factory.staticFactory" factory-method="getAccountService">
   </bean>

```









`bean标签的属性`:
属性：
* **id**：给对象在容器中提供一个唯一标识。用于获取对象。

* **class**：指定类的全限定类名。用于反射创建对象。默认情况下调用无参构造函数。

* scope：指定对象的作用范围。
  * **singleton** :默认值，单例的.
  * **prototype** :多例的.
  * request  : WEB 项目中,Spring 创建一个 Bean 的对象,将对象存入到 request 域中.
  * session  : WEB 项目中,Spring 创建一个 Bean 的对象,将对象存入到 session 域中.
  * global session  :WEB 项目中,<font color=blue>应用在 Portlet 环境.</font>如果没有 Portlet 环境     
     那么globalSession 相当于 session.

  

   init-method：指定类中的初始化方法名称。

  destroy-method：指定类中销毁方法名称。

<font color=blue>global session的含义又叫全局session，作用于集群环境的全局会话范围
global session-负载均衡</font>
![global session-负载均衡](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200704230541.png)

`bean对象的作用范围`


><font color=#009C41> **单例对象：scope="singleton"** </font>
><font color=#009C41>一个应用只有一个对象的实例。它的作用范围就是整个引用。  即使用相同的id获得的都是同一个对象引用。</font>
>**生命周期**： 
>对象出生：当应用加载，创建容器时，对象就被创建了。 
>对象活着：只要容器在，对象一直活着。 
>对象死亡：当应用卸载，销毁容器时，对象就被销毁了。

><font color=#009C41> **多例对象：scope="prototype"** </font>
><font color=#009C41>每次访问对象时，都会重新创建对象实例。使用相同的id 多次申请会得到新的对象引用。 </font>
>**生命周期：** 
>对象出生：当使用对象时，创建新的对象实例。 
>对象活着：只要对象在使用中，就一直活着。 
>对象死亡：当对象长时间不用时，被 java的垃圾回收器回收了。

**单例模式下的初始化和销毁，且创建bean对象时为立即加载**

```java
<!--   bean对象的作用范围：单例模式 -->
    <bean id="accountService" class="code.service.impl.AccountServiceImpl"
        scope="singleton" init-method="init" destroy-method="destroy"></bean>

```

```java
public class Client {
    public static void main(String[] args) {
        //1.获取核心容器对象
        ClassPathXmlApplicationContext ac =new ClassPathXmlApplicationContext("bean.xml");
        // 2.根据id获取bean对象
        IAccountService as=(IAccountService) ac.getBean("accountService");

        System.out.println(as);
        as.saveAccount();
        //手动关闭ac的容器
        ac.close();
  }
}
```

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200704230549.png)

**多例模式下的初始化和销毁，且创建bean对象时为延迟加载**

```java
<!--   bean对象的作用范围 ：多例模式 -->
    <bean id="accountService" class="code.service.impl.AccountServiceImpl"
        scope="prototype" init-method="init" destroy-method="destroy"></bean>

```
如下图，多例对象不会立即销毁，而是会有Java的垃圾回收机制销毁。
即使调用了 `close()方法`，也不会立即销毁。
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200704230713.png)







#  （四）spring的依赖注入

10 构造函数注入
11 set方法注入
12 注入集合数据
13 课程知识梳理









## 4.1 依赖注入的概念
`依赖注入`：Dependency Injection。它是 spring 框架核心 ioc 的具体实现。
我们的程序在编写时，通过控制反转，把对象的创建交给了 spring，但是代码中不可能出现没有依赖的情况。
`ioc 解耦`只是降低他们的依赖关系，但不会消除。
<font color=#005C01>例如：我们的业务层仍会调用持久层的方法。那这种业务层和持久层的依赖关系，在使用 spring 之后，就让spring来维护了。</font>
简单的说，就是坐等框架把持久层对象传入业务层，而不用我们自己去获取。

## 4.2 构造函数注入
顾名思义，就是使用类中的构造函数，给成员变量赋值。注意，赋值的操作不是我们自己做的，而是通过配置的方式，让 spring 框架来为我们注入。具体代码如下：

**对象的类提供带参的构造函数**

```java
public class AccountServiceImpl implements IAccountService {
    //如果是经常变化的数据，不适用于注入的方式
    private String name;
    private  Integer age;
    private Date birthday;
    public AccountServiceImpl(String name,Integer age,Date birthday){
        this.name=name;
        this.age=age;
        this.birthday=birthday;
    }
    public void saveAccount() {

        System.out.println("service种saveAccount方法执行了。。。");
        System.out.println(name+","+age+","+birthday);
    }
```

**bean.xml文件配置**
```java
<bean id="accountService" class="code.service.impl.AccountServiceImpl">
    <constructor-arg name="name" value="张三"></constructor-arg>
    <constructor-arg name="age" value="18"></constructor-arg>
    <constructor-arg name="birthday" ref="now"></constructor-arg>
</bean>
```

 **使用构造函数的方式，给 service 中的属性注入传值**
**要求**：
类中需要提供一个对应参数列表的构造函数。
涉及的标签：constructor-arg
标签出现的位置：bean标签的内部
**标签的属性**
     

 - type：用于指定要注入的数据类型，该数据类型也是构造函数中的某个或某些参数的类型
 - index：用于指定要注入的数据给构造函数中指定索引位置的参数赋值。索引的位置从0开始。
-   name：用于指定给构造函数中指定名称的参数赋值。
` 以上参数用于指定给构造函数中哪个参数赋值`     
-  value：用于提供基本类型和String类型的数据
-  ref：用于指定其他的bean类型数据，它指的就是在spring的IOC核心容器中出现过的bean对象。

`优势`：在获取bean对象时，注入数据是必须的操作，否则对象无法创建成功。
`弊端`：改变了bean对象的实例化方式，使我们在创建对象时，如果不注入这些所有的参数数据，是无法创建对象的。


## 4.3 set方法注入

```java
public class AccountServiceImpl implements IAccountService {
    //如果是经常变化的数据，不适用于注入的方式
    private String name;
    private  Integer age;
    private Date birthday;
    
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }
```



```java
 <!--    配置一个日期对象-->
    <bean id="now" class="java.util.Date"></bean>


    <bean id="accountService2" class="code.service.impl.AccountServiceImpl">
        <property name="name" value="李四"></property>
        <property name="age" value="20"></property>
        <property name="birthday" ref="now"></property>
    </bean>
```
 通过配置文件给 bean 中的属性传值：使用 set 方法的方式
`涉及的标签：property`

`属性`：

- name：找的是类中 set 方法后面的部分
- ref：给属性赋值是其他 bean 类型的
- value：给属性赋值是基本数据类型和 string 类型的
实际开发中，此种方式用的较多。
`优势`：创建对象时没有明确的限制，可以直接使用默认构造函数。
`弊端`：如果有某个成员必须有值，则使用set方法时不一定会注入数据。

## 4.4 注入集合数据
顾名思义，就是给类中的集合成员传值，它用的也是set方法注入的方式，只不过变量的数据类型都是集合。
我们这里介绍注入数组，List,Set,Map,Properties。具体代码如下：
` 在注入集合数据时，只要结构相同，标签可以互换 `
注入集合数据
`List 结构的`：array,list,set
`Map 结构的`:map,entry,props,prop

```java
<!--复杂类型的注入/集合类型-->
<bean id="accountService" class="code.service.impl.AccountServiceImpl">
<property name="myStrs" >
    <array>
        <value>AAA</value>
        <value>BBB</value>
        <value>CCC</value>
    </array>

</property>
<property name="myList" >
        <list>
            <value>AAA</value>
            <value>BBB</value>
            <value>CCC</value>
        </list>
</property>
    <property name="mySet" >
        <set>
            <value>AAA</value>
            <value>BBB</value>
            <value>CCC</value>
        </set>
    </property>
    <property name="myMap" >
        <map>
            <entry key="testA" value="AAA"></entry>
            <entry key="testB">
                <value>BBB</value>
            </entry>
        </map>
    </property>
    <property name="myProps">
        <props>
            <prop key="testC">CCC</prop>
            <prop key="testD">DDD</prop>
        </props>
    </property>

</bean>

```

```java
**
 * 账户的业务层实现类
 */
public class AccountServiceImpl implements IAccountService {

    private  String[] myStrs;
    private List<String> myList;
    private Set<String> mySet;
    private Map<String,String> myMap;
    private Properties myProps;

    public String[] getMyStrs() {
        return myStrs;
    }

    public void setMyStrs(String[] myStrs) {
        this.myStrs = myStrs;
    }

    public List<String> getMyList() {
        return myList;
    }

    public void setMyList(List<String> myList) {
        this.myList = myList;
    }

    public Set<String> getMySet() {
        return mySet;
    }

    public void setMySet(Set<String> mySet) {
        this.mySet = mySet;
    }

    public Map<String, String> getMyMap() {
        return myMap;
    }

    public void setMyMap(Map<String, String> myMap) {
        this.myMap = myMap;
    }

    public Properties getMyProps() {
        return myProps;
    }

    public void setMyProps(Properties myProps) {
        this.myProps = myProps;
    }

    public void saveAccount() {
        System.out.println(Arrays.toString(myStrs));
        System.out.println(myList);
        System.out.println(mySet);
        System.out.println(myMap);
        System.out.println(myProps);

    }
}
```





# （五）Spring常用注解

## 5.1 基于注解的IOC配置
**曾经XML的配置：**

```java
 <bean id="accountService" class="com.itheima.service.impl.Acc
       scope=""  init-method="" destroy-method="">
     <property name=""  value="" | ref=""></property>
 </bean>
```
**使用注解配置时的bean.xml配置**
注意：`告知spring在创建容器时要扫描的包，配置所需要的标签不是在beans的约束中`

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">
        
<!--告知spring在创建容器时要扫描的包，配置所需要的标签不是在beans的约束中，-->
<!--    而是在context名称空间和约束中。-->

    <context:component-scan base-package="code"></context:component-scan>

</beans>
```

<br>

---


**常用注解按作用分类：**

<br>

---

<font color=#003780> **用于创建对象的**</font>
相当于：
`< bean id=" " class=" ">`
     他们的作用就和在XML配置文件中编写一个`< bean >`标签实现的功能是一样的
    <br>
<br>


>  <font color=#DA3C78>  **Component:**</font>
>
>  **作用**：用于把当前类对象存入spring容器中
>        **属性**：
>            value：用于指定bean的id。当我们不写时，它的默认值是当前类名，且首字母改小写。

>  <font color=#DA3C78>  **Controller**：</font>一般用在表现层  
>  <font color=#DA3C78>   **Service**：</font>一般用在业务层   <font color=#DA3C78> 
>  **Repository**：</font>一般用在持久层


以上三个注解他们的作用和属性与Component是一模一样。
     他们三个是spring框架为我们提供明确的三层使用的注解，使我们的三层对象更加清晰


<br>

---

<font color=#003780>**用于注入数据的**</font>
相当于：
`< property name=" " ref=" ">`
`< property name=" " value=" ">`
     注解的作用就和在xml配置文件中的bean标签中写一个 `< property >` 标签的作用是一样的

   

>   <font color=#DA3C78>**Autowired:**</font>
>         **作用**：自动按照类型注入。只要容器中有唯一的一个bean对象类型和要注入的变量类型匹配，就可以注入成功
>        &ensp; &ensp; &ensp; 如果ioc容器中没有任何bean的类型和要注入的变量类型匹配，则报错。
>          &ensp; &ensp; &ensp;     如果Ioc容器中有多个类型匹配时：根据变量名称和bean的id名称匹配。
>         **出现位置：**
>            &ensp; &ensp; &ensp; &ensp; 可以是变量上，也可以是方法上
>         **细节**：
>           &ensp;  &ensp; &ensp; &ensp; 在使用注解注入时，set方法就不是必须的了。

 <font color=#DA3C78>Autowired:图解</font>
![自动按照类型注入](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705110452.png)


>   <font color=#DA3C78> **Qualifier:**</font>
>   **作用**：在自动按照类型注入的基础之上，再按照 Bean 的 id 注入。它在给字段注入时不能独立使用，必须和@Autowire 一起使用；但是给方法参数注入时，可以独立使用。
>   **属性**：
>    value：用于指定注入bean的id。

>    <font color=#DA3C78>**Resource**</font>
>    **作用**：直接按照 Bean 的 id 注入。它也只能注入其他 bean 类型。
>    **属性**：
>        &ensp;  &ensp; &ensp; &ensp; name：用于指定bean的id。

     以上三个注入都只能注入其他bean类型的数据，而基本类型和String类型无法使用上述注解实现。
     另外，集合类型的注入只能通过XML来实现。


> <font color=#DA3C78> **Value**</font>
>       **作用**：用于注入基本类型和String类型的数据
>       **属性**：
>           value：用于指定数据的值。它可以使用spring中SpEL(也就是spring的el表达式）
>      &ensp;  &ensp; &ensp; &ensp;  SpEL的写法：${表达式}


示例1：`Autowired`、`Qualifier`配合使用进行数据的注入
```java
/**
 * 账户的业务层实现类
 */
@Component(value = "accountService")
public class AccountServiceImpl implements IAccountService {

@Autowired
@("accountDao2")
private  IAccountDao accountDao;

/*
@Resource(name = "accountDao1")
private  IAccountDao accountDao;
*/

    public void saveAccount() {
        accountDao.saveAccount();
    }
}

```


<br>

---
<font color=#003780>**用于改变作用范围的**</font>
相当于：`< bean id=" " class="" scope=" ">`
  他们的作用就和在bean标签中使用scope属性实现的功能是一样的


>   <font color=#DA3C78>**Scope**</font>
>         **作用**：用于指定bean的作用范围
>         **属性**：
>              &ensp;  &ensp; &ensp; &ensp; value：指定范围的取值。
>   &ensp;  &ensp; &ensp; &ensp; 常用取值：**singleton 、prototype**、request、 session、 globalsession

<br>

---

<font color=#003780>**和生命周期相关 了解**</font>
相当于：
`< bean id= "  " class=" " init-method=" " destroy-method=" " />`
     他们的作用就和在bean标签中使用` init-method `和` destroy-methode `的作用是一样的


>    <font color=#DA3C78> **PreDestroy**</font>
>          作用：用于指定销毁方法
>    <font color=#DA3C78>  **PostConstruct**</font>
>          作用：用于指定初始化方法


```java
/**
 * 账户的业务层实现类
 */
@Component(value = "accountService")
public class AccountServiceImpl implements IAccountService {

@Resource(name = "accountDao1")
private  IAccountDao accountDao;

@PostConstruct
public void init(){
    System.out.println("AccountServiceImpl初始化方法执行了。。。");
}

@PreDestroy
    public void destroy(){
        System.out.println("AccountServiceImpl销毁方法执行了。。。");
    }


    public void saveAccount() {
        accountDao.saveAccount();
    }
}

```
## 5.2 关于 Spring  注解和 XML 比较
`注解的优势`：配置简单，维护方便（我们找到类，就相当于找到了对应的配置）。
`XML  的优势`：修改时，不用改源码。不涉及重新编译和部署。
`Spring  管理 Bean  方式的比较`：
![xml与注解配置的对比](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720131837.png)

## 5.3 待改造问题：纯注解配置
写到此处，基于注解的 IoC 配置已经完成，但是大家都发现了一个问题：我们依然离不开 spring 的 xml 配置文件，那么能不能不写这个 bean.xml，所有配置都用注解来实现呢？

我们发现，之所以我们现在离不开 xml 配置文件，是因为我们有一句很关键的配置：
<!-- 告知spring框架在，读取配置文件，创建容器时，扫描注解，依据注解创建对象，并存入容器中 -->

```java
<context:component-scan base-package="code"></context:component-scan>
```

如果它要也能用注解配置，那么我们就离`脱离 xml 文件`又进了一步。

`另外，数据源和 JdbcTemplate 的配置也需要靠注解来实现。`
```java

<!-- 配置 dbAssit -->
<bean id="dbAssit" class="com.itheima.dbassit.DBAssit">
<property name="dataSource" ref="dataSource"></property>
</bean>

<!-- 配置数据源 -->
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
<property name="driverClass" value="com.mysql.jdbc.Driver"></property>
<property name="jdbcUrl" value="jdbc:mysql:///spring_day02"></property>
<property name="user" value="root"></property>
<property name="password" value="1234"></property>
</bean>
```

<font color=blue>为解决这个问题，可以采用更多的 **纯注解配置** ，从而脱离 xml 文件配置，下一章讨论。</font>
[【Spring框架】（六）Spring 新注解](https://blog.csdn.net/qq_41864648/article/details/104





# （六）Spring 新注解



## 6.1 新注解说明
为了更多使用纯注解配置bean，脱离xml文件配置。我们使用更多的新注解。

## 6.2 @Configuration

> **作用**： 
> 用于指定当前类是一个 spring 配置类，当创建容器时会从该类上加载注解。
> 获取容器时需要使用 `AnnotationApplicationContext`  ( 有@Configuration 注解的 类. class )。 
> **属性**：
> <font color= #0287D0> value : 用于指定配置类的字节码</font>

示例代码：
```java
@Configuration
public class SpringConfiguration {
}

```
注意：我们已经把配置文件用类来代替了，但是如何配置创建容器时要扫描的包呢？
请看下一个注解。

<br>

----
<br>


## 6.3 @ComponentScan

> **作用**： 
> 用于指定 spring 在初始化容器时要扫描的包。作用和在 spring 的 xml 配置文件中的：
> `< context:component-scan base-package="com.itheima"/> `是一样的。
> **属性**：
> <font color= #0287D0> basePackages：用于指定要扫描的包。和该注解中的 value 属性作用一样。</font>


```java
@Configuration
@ComponentScan("com.itheima")
public class SpringConfiguration {
}
```
注意：我们已经配置好了要扫描的包，但是数据源和 JdbcTemplate 对象如何从配置文件中移除呢？
请看下一个注解。


<br>

----
<br>


## 6.4 @Bean

> **作用**：
> 该注解只能写在方法上，表明使用此方法创建一个对象，并且放入 spring 容器。 
> **属性**： 
> <font color= #0287D0> name：给当前@Bean 注解方法创建的对象指定一个名称 (即 bean 的 id）。</font>


```java
public class JdbcConfig {
/**
* 创建一个数据源，并存入 spring 容器中
* @return
*/
@Bean(name="dataSource")
public DataSource createDataSource() {
try {
		ComboPooledDataSource ds = new ComboPooledDataSource();
		ds.setUser("root");
		ds.setPassword("1234");
		ds.setDriverClass("com.mysql.jdbc.Driver");
		ds.setJdbcUrl("jdbc:mysql:///spring_day02");
		
		return ds;
		
} catch (Exception e) {
		throw new RuntimeException(e);
  }
} 
/**
* 创建一个 DBAssit，并且也存入 spring 容器中
* @param dataSource
* @return
*/
@Bean(name="dbAssit")
public DBAssit createDBAssit(DataSource dataSource) {
		return new DBAssit(dataSource);
  }
}
```
注意:
我们已经把数据源和 DBAssit 从配置文件中移除了，`此时可以删除 bean.xml 了`。
但是由于没有了配置文件，创建数据源的配置又都写死在类中了。如何把它们配置出来呢？
请看下一个注解。


<br>

----
<br>


## 6.5 @PropertySource

> **作用**：
> 用于加载.properties 文件中的配置。
> 例如我们配置数据源时，可以把连接数据库的信息写到properties
> 配置文件中，就可以` 使用此注解指定 properties 配置文件的位置` 。
> **属性**：
> <font color= #0287D0> value[]：用于指定 properties文件位置。如果是在类路径下，需要写上 classpath:</font>

示例代码：

```java
配置：
/**
* 连接数据库的配置类
*/
public class JdbcConfig {
@Value("${jdbc.driver}")
private String driver;
@Value("${jdbc.url}")
private String url;
@Value("${jdbc.username}")
private String username;
@Value("${jdbc.password}")
private String password;
/**
* 创建一个数据源，并存入 spring 容器中
* @return
*/
@Bean(name="dataSource")
public DataSource createDataSource() {
try {
ComboPooledDataSource ds = new ComboPooledDataSource();
ds.setDriverClass(driver);
ds.setJdbcUrl(url);
ds.setUser(username);
ds.setPassword(password);
return ds;
} catch (Exception e) {
throw new RuntimeException(e);
}
} 
}
```


jdbc.properties  文件：

```java
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/day44_ee247_spring
jdbc.username=root
jdbc.password=1234

```
**在主配置类中加入 `@PropertySource 注解`，引入外部的properties文件**
```java
//@Configuration
@ComponentScan(basePackages = {"code"})
@Import(JdbcConfig.class)
@PropertySource("classpath:jdbcConfig.properties")
public class SpringConfiguration {
}
```

注意：
此时我们已经有了两个配置类，但是他们还没有关系。如何建立他们的关系呢？
请看下一个注解。
<br>

----
<br>


## 6.6 @Import

> **作用**：
> 用于导入其他配置类，在引入其他配置类时，` 可以不用再写@Configuration 注解。`当然，写上也没问 题。 
> **属性**：
> <font color= #0287D0> value[]：用于指定其他配置类的字节码。</font>

示例代码：

```java

@Configuration
@ComponentScan(basePackages = "com.itheima.spring")
@Import({ JdbcConfig.class})
public class SpringConfiguration {
}
@Configuration
@PropertySource("classpath:jdbc.properties")
public class JdbcConfig{
}

```
注意：
我们已经把要配置的都配置好了，但是新的问题产生了，由于没有配置文件了，如何获取容器呢？
请看下一小节。
<br>

----
<br>


## 6.7 通过注解获取容器： 

```java
ApplicationContext ac =
new AnnotationConfigApplicationContext(SpringConfiguration.class);
```

<br>

----
<br>

## 6.8 案例 对bean.xml的改造

原始的bean.xml文件配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

<!--告知spring在创建容器时要扫描的包-->
<context:component-scan base-package="code"></context:component-scan>

<!--    面对多个dao使用QueryRunner时，这里runner的范围为多例对象，为每个申请的新建一个对象-->
    <!--配置QueryRunner-->
    <bean id="runner" class="org.apache.commons.dbutils.QueryRunner" scope="prototype">
        <!--注入数据源-->
        <constructor-arg name="ds" ref="dataSource"></constructor-arg>
    </bean>

    <!-- 配置数据源 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <!--连接数据库的必备信息-->
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/spring"></property>
        <property name="user" value="root"></property>
        <property name="password" value="root"></property>
    </bean>
</beans>
```

一、新建一个config包并添加一个配置类 `SpringConfiguration`
进行改造

```xml
<!--告知spring在创建容器时要扫描的包-->
<context:component-scan base-package="code"></context:component-scan>
```

```java
/**
 * 该类是一个配置类，它的作用和 bean.xml 是一样的
 * spring 中的新注解
 * configuration
 *          作用：指定当前类是一个配置类
 *  componentScan
 *          作用：用于通过注解指定要扫描的包
 *          属性：basePackages/value=" 包名 "
 */
 
@Configuration
@ComponentScan(basePackages = {"code"})
public class SpringConfiguration { 
}
```


二、改造bean对象

```xml
 <!--配置QueryRunner-->
    <bean id="runner" class="org.apache.commons.dbutils.QueryRunner" scope="prototype">
        <!--注入数据源-->
        <constructor-arg name="ds" ref="dataSource"></constructor-arg>
    </bean>


  <!-- 配置数据源 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <!--连接数据库的必备信息-->
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/spring"></property>
        <property name="user" value="root"></property>
        <property name="password" value="root"></property>
    </bean>
```


```java
@Configuration
@ComponentScan(basePackages = {"code"})
public class SpringConfiguration {

    /**
     * 用于创建一个 QueryRunner 对象，并存入spring的容器中
     * @param dataSource
     * @return
     */
    @Bean(name ="runner")
    public QueryRunner creatQueryRunner(DataSource dataSource){
        return new QueryRunner(dataSource);
      }
    }

	/**
     * 创建数据源对象
     * 实际中不应该在这里写“死”数据源，而是通过properties文件的配置方式，通过注解注入数据进来。
     * @return
     */

    @Bean(name ="dataSource")
    public DataSource creatDataSource(){
        try {
            ComboPooledDataSource ds =new ComboPooledDataSource();
            ds.setDriverClass("com.mysql.jdbc.Driver");
            ds.setJdbcUrl("jdbc:mysql://localhost:3306/spring");
            ds.setUser("root");
            ds.setPassword("root");
            return ds;
        }catch (Exception e){
            throw new RuntimeException(e);
        }

    }

```


三、配置数据源DataSource

```java
public class JdbcConfig {

    @Value("${jdbc.driver}")
    private String driver;

    @Value("${jdbc.url}")
    private  String url;

    @Value("${jdbc.username}")
    private  String username;

    @Value("${jdbc.password}")
    private  String password;
 /**
     * 创建数据源对象
     * @return
     */

    @Bean(name ="dataSource")
    public DataSource creatDataSource(){
        try {
            ComboPooledDataSource ds =
                    new ComboPooledDataSource();

            ds.setDriverClass(driver);
            ds.setJdbcUrl(url);
            ds.setUser(username);
            ds.setPassword(password);
            return ds;
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }

```
**注意：**
**在resources包下添加` jdbcConfig.properties `文件**
**主配置文件中必须加入注解，说明properties文件的位置（classpath表示根据类路径）**
` @PropertySource("classpath:jdbcConfig.properties") `

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720131850.png)

<br>
<br>

四、测试类，调用spring核心容器时，要使用注解方式



```java
//1.获取容器
        ApplicationContext ac= new  AnnotationConfigApplicationContext(SpringConfiguration.class);
```
<br>

----
<br>

<font color=#0EAC51>细节问题
可以在方法的参数中使用注解，如下图的 ` @Qualifier `，就是用来进行名称匹配的</font>
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705114517.png)





# （八）Account 单表操作 案例



## 8.1 基于xml的ioc案例
pom.xml 依赖

```xml
  <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>commons-dbutils</groupId>
            <artifactId>commons-dbutils</artifactId>
            <version>1.4</version>
        </dependency>


        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>

        <dependency>
            <groupId>c3p0</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.1.2</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.10</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

业务层 AccountService

```java
/**
 * 账户的业务层接口
 */
public interface IAccountService {
    
    List<Account> findAllAccount();
    
    Account findAccountById(Integer accountId);
    
    void saveAccount(Account account);

    void updateAccount(Account account);

    void  deleteAccount(Integer accountId);
}
```


```java
**
 * 账户的业务层实现类
 */
public class AccountServiceImpl implements IAccountService {
    private IAccountDao accountDao;

    public void setAccountDao(IAccountDao accountDao) {
        this.accountDao = accountDao;
    }

    public List<Account> findAllAccount() {
        List<Account> allAccount = accountDao.findAllAccount();
        return allAccount;
    }

    public Account findAccountById(Integer accountId) {
        return accountDao.findAccountById(accountId);
    }

    public void saveAccount(Account account) {
        accountDao.saveAccount(account);
    }

    public void updateAccount(Account account) {
        accountDao.updateAccount(account);
    }

    public void deleteAccount(Integer accountId) {
        accountDao.deleteAccount(accountId);
    }
}

```

持久层AccountDao

```java
public interface IAccountDao {
    List<Account> findAllAccount();
    
    Account findAccountById(Integer accountId);
    
    void saveAccount(Account account);

    void updateAccount(Account account);

    void  deleteAccount(Integer accountId);
}

```


```java
public class AccountDaoImpl implements IAccountDao {

    private QueryRunner runner;

    public void setRunner(QueryRunner runner) {
        this.runner = runner;
    }

    /**
     * 查询所有
     */
    public List<Account> findAllAccount() {
        try {
            return runner.query("select * from account",new BeanListHandler<Account>(Account.class));
        } catch (SQLException e) {
           throw new RuntimeException(e);
        }
    }

    /**
     * 查询一个
     */
    public Account findAccountById(Integer accountId) {
        try {
            return runner.query("select * from account where id =?",new BeanHandler<Account>(Account.class),accountId);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * 保存账户
     */
    public void saveAccount(Account account) {
        try {
           runner.update("insert into account (name,money) values(?,?)",account.getName(),account.getMoney());
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * 更新账户
     */
    public void updateAccount(Account account) {
        try {
            runner.update("update account set name=?,money=? where id=?",account.getName(),account.getMoney(),account.getId());
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * 删除
     */
    public void deleteAccount(Integer accountId) {
        try {
            runner.update("delete from account where id=?",accountId);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }
}

```


bean.xml 文件配置。添加 容器中的 bean
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 配置Service -->
    <bean id="accountService" class="code.service.impl.AccountServiceImpl">
        <!-- 注入dao -->
        <property name="accountDao" ref="accountDao"></property>
    </bean>

    <!--配置Dao对象-->
    <bean id="accountDao" class="code.dao.impl.AccountDaoImpl">
        <!-- 注入QueryRunner -->
        <property name="runner" ref="runner"></property>
    </bean>

<!--    面对多个dao使用QueryRunner时，这里runner的范围为多例对象，为每个申请的新建一个对象-->

    <!--配置QueryRunner-->
    <bean id="runner" class="org.apache.commons.dbutils.QueryRunner" scope="prototype">
        <!--注入数据源-->
        <constructor-arg name="ds" ref="dataSource"></constructor-arg>
    </bean>

    <!-- 配置数据源 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <!--连接数据库的必备信息-->
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/spring"></property>
        <property name="user" value="root"></property>
        <property name="password" value="root"></property>
    </bean>
</beans>
```

测试类AccountTest

```java
 @Test
    public void testFindAll() {
        //1.获取容器
        ApplicationContext ac=new ClassPathXmlApplicationContext("bean.xml");
        //2.得到业务层对象
        IAccountService accountService = (IAccountService)ac.getBean("accountService");
        //3.执行方法
        List<Account> accounts = accountService.findAllAccount();

        for (Account account:accounts){
            System.out.println(account);
        }
    }
```



## 8.2 基于注解的ioc案例
bean.xml 文件配置，先删除 accountService、accountDao的bean配置。
再添加如下标签，使用注解，必须告知要扫描哪个包呀( •̀ ω •́ )✧。
```java
<!--告知spring在创建容器时要扫描的包-->
<context:component-scan base-package="code"></context:component-scan>
```
业务层实现类 加注解（可不加 set方法注入）
```java
@Service("accountService")
public class AccountServiceImpl implements IAccountService {
    @Autowired
    private IAccountDao accountDao;
```

持久层实现类 加注解 （可不加 set方法注入）
```java
@Repository("accountDao")
public class AccountDaoImpl implements IAccountDao {

    @Autowired
    private QueryRunner runner;
```


## 8.3 纯注解配置
在第二个案例中，由于还有bean.xml文件的存在，称不上完全脱离了 xml 配置。
在学习完 [新注解 ](https://blog.csdn.net/qq_41864648/article/details/104380034) 一章后，继续改进代码，将 bean.xml 文件内的剩余内容 都用 注解 进行替换。最后的结果代码如下。

<font color=#0067B0>**一、添加config包，包内有 两个 配置类 SpringConfiguration、JdbcCongig 。** </font>
![工程目录结构](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720131902.png)
**SpringConfiguration 类**

```java
//@Configuration
@ComponentScan(basePackages = {"code"})
@Import(JdbcConfig.class)
@PropertySource("classpath:jdbcConfig.properties")
public class SpringConfiguration {
}
```
**JdbcConfig 类**

```java
public class JdbcConfig {

    @Value("${jdbc.driver}")
    private String driver;

    @Value("${jdbc.url}")
    private  String url;

    @Value("${jdbc.username}")
    private  String username;

    @Value("${jdbc.password}")
    private  String password;

    /**
     * 用于创建一个 QueryRunner 对象，并存入spring的容器中
     * @param dataSource
     * @return
     */
    @Bean(name ="runner")
    @Scope("prototype")
    public QueryRunner creatQueryRunner(@Qualifier("dataSource") DataSource dataSource){
        return new QueryRunner(dataSource);
    }

    /**
     * 创建数据源对象
     * @return
     */
    @Bean(name ="dataSource")
    public DataSource creatDataSource(){
        try {
            ComboPooledDataSource ds = new ComboPooledDataSource();

            ds.setDriverClass(driver);
            ds.setJdbcUrl(url);
            ds.setUser(username);
            ds.setPassword(password);
            return ds;
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }
}

```
<font color=#0067B0> **二、编写完上面两个配置类，就可以删除 xml 文件了。接下来进行测试**
（这里已经使用了JUnit和Spring整合，注意更改pom中的依赖）</font>


```java
/**
 * 测试配置
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfiguration.class)
public class AccountServiceTest {
    @Autowired
    private IAccountService accountService =null;

    @Test
    public void testFindAll() {
        //3.执行方法
        List<Account> accounts = accountService.findAllAccount();
        for (Account account:accounts){
            System.out.println(account);
        }
    }
```



# （九）JUnit 整合

## 9.1 问题
在测试类中，每个测试方法都有以下两行代码：
`ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");`
`IAccountService as = ac.getBean("accountService",IAccountService.class);`
这两行代码的作用是获取容器，如果不写的话，直接会提示空指针异常。所以又不能轻易删掉。

## 9.2 解决方案
针对上述问题，我们需要的是程序能自动帮我们创建容器。一旦程序能自动为我们创建 spring 容器，我们就无须手动创建了，问题也就解决了。
我们都知道，junit 单元测试的原理（在 web 阶段课程中讲过），但显然，junit 是无法实现的，因为它自己都无法知晓我们是否使用了 spring 框架，更不用说帮我们创建 spring 容器了。不过好在，junit 给我们暴露了一个注解，可以让我们替换掉它的运行器。

这时，我们需要依靠 spring 框架，因为它提供了一个运行器，可以读取配置文件（或注解）来创建容器。我们只需要告诉它配置文件在哪就行了。




<font color=#009C41>
 使用Junit单元测试：测试我们的配置

     Spring整合junit的配置
    
     1、导入spring整合junit的jar(坐标)
     2、使用Junit提供的一个注解把原有的main方法替换了，替换成spring提供的
            @Runwith
     3、告知spring的运行器，spring和ioc创建是基于xml还是注解的，并且说明位置
         @ContextConfiguration
                 locations：指定xml文件的位置，加上classpath关键字，表示在类路径下
                 classes：指定注解类所在地位置
     
    当我们使用spring 5.x版本的时候，要求junit的jar必须是4.12及以上
</font>

<br>

---

<br>


## 9.3 步骤
第一步：pom.xml文件，导入依赖。并且JUnit依赖的版本改为4.12

```java
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

```

**第二步：**
测试类不再 使用` new `的方式，创建spring 的ioc容器。

- 使用` @RunWith ` 注解替换原有运行器
` @RunWith(SpringJUnit4ClassRunner.class) `


- 使用` @ContextConfiguration ` 指定 spring  配置文件的位置
 ` @ContextConfiguration(locations= {"classpath:bean.xml"})`
 ` @ContextConfiguration(classes = SpringConfiguration.class) `

 - 使用@Autowired  给测试类中的变量注入数据
```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfiguration.class)
public class AccountServiceTest {
    @Autowired
    private IAccountService accountService =null;

    @Test
    public void testFindAll() {
        //3.执行方法
        List<Account> accounts = accountService.findAllAccount();

        for (Account account:accounts){
            System.out.println(account);
        }
    }
```
<br>

---

<br>


## 9.4 思考：为什么不把测试类配到 xml 中？
在解释这个问题之前，先解除大家的疑虑，配到 XML 中能不能用呢？
答案是肯定的，没问题，可以使用。
那么为什么不采用配置到 xml 中的方式呢？

这个原因是这样的：
- 第一：当我们在 xml 中配置了一个 bean，spring 加载配置文件创建容器时，就会创建对象。
- 第二：测试类只是我们在测试功能时使用，而在项目中它并不参与程序逻辑，也不会解决需求上的问题，所以创建完了，并没有使用。那么存在容器中就会造成资源的浪费。

 所以，基于以上两点，我们不应该把测试配置到 xml 文件中。





# （十）Account银行账户案例



## pom.xml

```xml
   <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </depende ncy>
        <dependency>
            <groupId>commons-dbutils</groupId>
            <artifactId>commons-dbutils</artifactId>
            <version>1.4</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>

        <dependency>
            <groupId>c3p0</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.1.2</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
    </dependencies>

```
## jdbcConfig.properties

```java
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/spring
jdbc.username=root
jdbc.password=root
```
## 配置类 ：SpringConfiguration类、JdbcConfig类

```java
@Configuration
@ComponentScan(basePackages = "code")
@PropertySource("classpath:jdbcConfig.properties")
@Import({JdbcConfig.class})

public class SpringConfiguration {


}

```

```java
public class JdbcConfig {
    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String username;
    @Value("${jdbc.password}")
    private String password;



    @Bean(name = "runner")
    public QueryRunner createQueryRunner(DataSource dataSource) {
        return new QueryRunner(dataSource);
    }

    @Bean(name = "dataSource")
    public DataSource createDataSource() {
        ComboPooledDataSource ds = new ComboPooledDataSource();
        try {
            ds.setDriverClass(driver);
            ds.setJdbcUrl(url);
            ds.setUser(username);
            ds.setPassword(password);
            return ds;
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

}

```


## 业务层Service层

```java
/**
 * 账户的业务层接口
 */
public interface IAccountService {

    List<Account> findAllAccount();

    Account findAccountById(Integer accountId);

    void saveAccount(Account account);

    void updateAccount(Account account);

    void  deleteAccount(Integer accountId);

    /**
     * 转账
     * @param sourceName 转出账户
     * @param targetName 转入账户
     * @param money      金额
     */
    void transfer(String sourceName,String targetName,Float money);


}

```

```java
/**
 * 账户的业务层实现类
 */
@Service(value = "accountService")
public class AccountServiceImpl implements IAccountService {
    @Autowired
    private IAccountDao accountDao;

    public void setAccountDao(IAccountDao accountDao) {
        this.accountDao = accountDao;
    }

    public List<Account> findAllAccount() {
        List<Account> allAccount = accountDao.findAllAccount();
        return allAccount;
    }

    public Account findAccountById(Integer accountId) {
        return accountDao.findAccountById(accountId);
    }

    public void saveAccount(Account account) {
        accountDao.saveAccount(account);
    }

    public void updateAccount(Account account) {
        accountDao.updateAccount(account);
    }

    public void deleteAccount(Integer accountId) {
        accountDao.deleteAccount(accountId);
    }

    public void transfer(String sourceName, String targetName, Float money) {
        //1.根据名称查询转出账户
        Account source = accountDao.findAccountByName(sourceName);
        //2.根据名称查询转入账户
        Account target = accountDao.findAccountByName(targetName);
        //3.转出账户减钱
        source.setMoney(source.getMoney()-money);
        //4.转入账户加钱
        target.setMoney(target.getMoney()+money);
        //5.更新转出账户
        accountDao.updateAccount(source);
        //6.更新转入账户
        accountDao.updateAccount(target);
    }
}

```


## 持久层Dao

```java
public interface IAccountDao {
    List<Account> findAllAccount();

    Account findAccountById(Integer accountId);

    void saveAccount(Account account);

    void updateAccount(Account account);

    void  deleteAccount(Integer accountId);

    /**
     * 根据名称查询账户
     * @param name
     * @return 有结果则返回，没有就返回null。如果结果集有多个，就抛出异常。
     */
    Account findAccountByName(String name);

}

```

```java
@Repository(value = "accountDao")
public class AccountDaoImpl implements IAccountDao {


    @Autowired
    private QueryRunner runner;

    public void setRunner(QueryRunner runner) {
        this.runner = runner;
    }

    /**
     * 查询所有
     */
    public List<Account> findAllAccount() {
        try {
            return runner.query("select * from account",new BeanListHandler<Account>(Account.class));
        } catch (SQLException e) {
           throw new RuntimeException(e);
        }
    }

    /**
     * 查询一个
     */
    public Account findAccountById(Integer accountId) {
        try {
            return runner.query("select * from account where id =?",new BeanHandler<Account>(Account.class),accountId);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * 保存账户
     */
    public void saveAccount(Account account) {
        try {
           runner.update("insert into account (name,money) values(?,?)",account.getName(),account.getMoney());
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * 更新账户
     */
    public void updateAccount(Account account) {
        try {
            runner.update("update account set name=?,money=? where id=?",account.getName(),account.getMoney(),account.getId());
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    /**
     * 删除
     */
    public void deleteAccount(Integer accountId) {
        try {
            runner.update("delete from account where id=?",accountId);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }

    public Account findAccountByName(String name) {

        try {
            List<Account> accounts= runner.query("select * from account where name=?",new BeanListHandler<Account>(Account.class),name);
            if (accounts==null || accounts.size()==0){
                return null;
            }
            if (accounts.size()>1){
                throw new RuntimeException("结果集不唯一，数据有问题");
            }
            return accounts.get(0);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }
}

```

## 测试

```java
**
 * 测试配置
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfiguration.class)
public class AccountServiceTest {

    private ApplicationContext ac;
    @Autowired
    private IAccountService accountService;


    @Test
    public void testFindAll() {
        //3.执行方法
        List<Account> accounts = accountService.findAllAccount();

        for (Account account:accounts){
            System.out.println(account);
        }
    }
 @Test
    public void testTransfer(){
        accountService.transfer("aaa","bbb",100f);
    }

```

## 分析事务的问题并编写ConnectionUtils类、TransactionManager类

<font color= #FF6CA8>**上述代码存在的问题：**</font>
-   ，如下图阐述，在“转账”的过程中，发生了异常导致程序终止。那么将会产生严重的信息错误。在持久层操作数据库时，我们必须要考虑到这种情况。` 即不符合事务的一致性。`

<font color= #FF6CA8>**解决思想：**</font>
 - 如下图，操作数据库的持久层每执行一个方法就会从池中获取一次连接，产生一个事务，共产生了四个独立的连接和事务。显然 ，“ 转账 ” 是一个完整的事件，不应被分隔开来。
 - 应该让业务层来控制事务的提交和回滚。`（这里指Service层的 transfer 方法）`



![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705122330.png)

<font color= #FF6CA8>**解决第一步：创建utils包，并添加：ConnectionUtils类**</font>

ConnectionUtils类：管理 `线程与jdbc连接` 的工具类，应使一个线程绑定一个连接，当线程结束时才可以解绑连接。





## ConnectionUtils类
```java
/**
 * 连接的工具类，
 * 它用于从数据源中获取一个连接，并且实现这些连接和线程的绑定
 */
public class ConnectionUtils {

    private  ThreadLocal<Connection> tl=new ThreadLocal<Connection>();
    private DataSource dataSource;

    /**
     * 获取当前线程上的连接
     */
    public Connection getThreadConnection(){
        try {
            //1.先从ThreadLocal上获取
            Connection connection = tl.get();

            //2.判断当前线程上是否有连接
            if (connection==null){
                //3.从数据源中获取一个连接，并存入ThreadLocal中
                connection=  dataSource.getConnection();
            }
            //4.返回当前线程上的连接
            return connection;
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }

    /**
     *把连接和线程解绑
     * @return
     */
    public void removeConnection(){
        tl.remove();
    }

}

```

> ThreadLocal该类用于指定线程内部的存储数据是什么，
> threadlocal 是一个线程内部的存储类，可以在指定线程内的 存储数据，数据存储以后，只有指定线程可以得到存储数据。
> 每个线程持有一个ThreadLocalMap对象。
>
> https://www.jianshu.com/p/98b68c97df9b


## TransactionManager 类
TransactionManager 类：事务管理的工具类，包括开启事务、提交事务、回滚事务、释放连接。

```java
/**
 * 和事务管理相关的工具类
 */
public class TransactionManager {

    private ConnectionUtils connectionUtils;

    public void setConnectionUtils(ConnectionUtils connectionUtils) {
        this.connectionUtils = connectionUtils;
    }

    /**
     * 获得当前线程的连接并为其开启事务
     */
    public void beginTransaction(){
        try {
            connectionUtils.getThreadConnection().setAutoCommit(false);
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }


    /**
     * 获得当前线程的连接并为其提交事务
     */
    public void commit(){
        try {
            connectionUtils.getThreadConnection().commit();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }


    /**
     * 获得当前线程的连接并为其回滚事务
     */
    public void rollback(){
        try {
            connectionUtils.getThreadConnection().rollback();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }


    /**
     * 获得当前线程的连接并为其释放连接
     * 注意这里的close()方法只是将但前线程的连接归还到了池中，该线程还是持有该连接资源的。
     * 必须调用 connectionUtils.removeConnection()方法才会将该线程与连接 彻底解绑释放资源。
     */
    public void release(){
        try {
            connectionUtils.getThreadConnection().close();//把连接还回了连接池中
            connectionUtils.removeConnection();//将线程与连接解绑
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

}

```
## 改造后AccountServiceImpl 

<font color= #FF6CA8> **第二步：改造后的业务层实现类** </font>
使得每个业务方法用事务的流程包裹起来，将每次业务操作视为一个完整的方法。
结构如下：

```java
 try {
            //1.开启事务
            txManager.beginTransaction();
            //2.执行操作
            //3.提交事务
            txManager.commit();
            //4.返回结果
        }catch (Exception e){
            //5.回滚操作
            txManager.rollback();
            throw new RuntimeException(e);
        }finally {
            //6.释放连接
            txManager.release();
        }
```

代码：

```java
/**
 * 账户的业务层实现类
 */
@Service(value = "accountService")
public class AccountServiceImpl implements IAccountService {
    @Autowired
    private IAccountDao accountDao;

    @Resource(name = "transactionManager")
    private TransactionManager txManager;


    public List<Account> findAllAccount() {
        try {
            //1.开启事务
            txManager.beginTransaction();
            //2.执行操作
            List<Account> allAccount = accountDao.findAllAccount();
            //3.提交事务
            txManager.commit();
            //4.返回结果
            return allAccount;

        }catch (Exception e){
            //5.回滚操作
            txManager.rollback();
            throw new RuntimeException(e);
        }finally {
            //6.释放连接
            txManager.release();
        }
    }

    public Account findAccountById(Integer accountId) {
        try {
            //1.开启事务
            txManager.beginTransaction();
            //2.执行操作
         Account account=  accountDao.findAccountById(accountId);
            //3.提交事务
            txManager.commit();
            //4.返回结果
            return account;

        }catch (Exception e){
            //5.回滚操作
            txManager.rollback();
            throw new RuntimeException(e);
        }finally {
            //6.释放连接
            txManager.release();
        }
    }

    public void saveAccount(Account account) {
        try {
            //1.开启事务
            txManager.beginTransaction();
            //2.执行操作
            accountDao.saveAccount(account);
            //3.提交事务
            txManager.commit();
            //4.返回结果


        }catch (Exception e){
            //5.回滚操作
            txManager.rollback();
        }finally {
            //6.释放连接
            txManager.release();
        }

    }

    public void updateAccount(Account account) {
        try {
            //1.开启事务
            txManager.beginTransaction();
            //2.执行操作
            accountDao.updateAccount(account);
            //3.提交事务
            txManager.commit();
            //4.返回结果
        }catch (Exception e){
            //5.回滚操作
            txManager.rollback();
        }finally {
            //6.释放连接
            txManager.release();
        }


    }

    public void deleteAccount(Integer accountId) {
        try {
            //1.开启事务
            txManager.beginTransaction();
            //2.执行操作
            accountDao.deleteAccount(accountId);
            //3.提交事务
            txManager.commit();
            //4.返回结果

        }catch (Exception e){
            //5.回滚操作
            txManager.rollback();
        }finally {
            //6.释放连接
            txManager.release();
        }

    }

    public void transfer(String sourceName, String targetName, Float money) {
        try {
            //1.开启事务
            txManager.beginTransaction();
            //2.执行操作

            //2.1.根据名称查询转出账户
            Account source = accountDao.findAccountByName(sourceName);
            //2.2.根据名称查询转入账户
            Account target = accountDao.findAccountByName(targetName);
            //2.3.转出账户减钱
            source.setMoney(source.getMoney()-money);
            //2.4.转入账户加钱
            target.setMoney(target.getMoney()+money);
            //2.5.更新转出账户
            accountDao.updateAccount(source);
        int a=1/0;
            //2.6.更新转入账户
            accountDao.updateAccount(target);


            //3.提交事务
            txManager.commit();
            //4.返回结果

        }catch (Exception e){
            //5.回滚操作
            txManager.rollback();
        }finally {
            //6.释放连接
            txManager.release();
        }

    }
}

```

## 持久层实体类：AccountDaoImpl 类
改造持久层Dao，和queryRunner 在创建runner时使用传参连接

```java
 @Bean(name = "runner")
    public QueryRunner createQueryRunner() {
        return new QueryRunner();
    }
```

```java
@Repository(value = "accountDao")
public class AccountDaoImpl implements IAccountDao {

    @Autowired
    private QueryRunner runner;
    @Resource(name = "connectionUtils")
    private ConnectionUtils connectionUtils;

    /**
     * 查询所有
     */
    public List<Account> findAllAccount() {
        try {
            return runner.query(connectionUtils.getThreadConnection(),"select * from account",new BeanListHandler<Account>(Account.class));
        } catch (SQLException e) {
           throw new RuntimeException(e);
        }
    }

```



# (十一 ) 面向切面编程AOP

## 1.AOP的概念
什么是AOP：
> <font color=#003780>**百度百科：**</font>
> &ensp; &ensp; 在软件业，AOP为 Aspect OrientedProgramming的缩写，意为：面向切面编程，`通过预编译方式和运行期间动态代理`实现程序功能的统一维护的一种技术。
> &ensp; &ensp;  AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而` 使得业务逻辑各部分之间的耦合度降低` ，提高程序的可重用性，同时提高了开发的效率。
> <br>
>
> &ensp; &ensp;我们常说：“这件事情要从几个方面来看待”，往往意思是：需要从不同的角度来看待同一个事物。
> 这里的“方面”，指的是事物的外在特性在不同观察角度下的体现。而在AOP中，Aspect的含义，可能更多的理解为“切面”比较合适。
> <br>
> &ensp; &ensp;可以通过预编译方式和运行期动态代理实现在不修改源代码的情况下给程序动态统一添加功能的一种技术。AOP实际是GoF设计模式的延续，设计模式孜孜不倦追求的是调用者和被调用者之间的解耦,提高代码的灵活性和可扩展性，AOP可以说也是这种目标的一种实现。
> <br>
> &ensp; &ensp; 在Spring中提供了面向切面编程的丰富支持，允许通过` 分离应用的业务逻辑与系统级服务`（例如审计（auditing）和事务（transaction）管理）进行`内聚性`的开发。
> &ensp; &ensp; 应用对象只实现它们应该做的——完成业务逻辑——仅此而已。它们并不负责（甚至是意识）其它的系统级关注点，例如日志或事务支持。
> <br>
> <font color=#003780> **主要意图**</font>
> &ensp; &ensp; 将日志记录，性能统计，安全控制，事务处理，异常处理等代码从业务逻辑代码中`划分`出来.
> 通过对这些行为的`分离`，我们希望可以将它们独立到非指导业务逻辑的方法中，进而改变这些行为的时候`不影响业务逻辑`的代码.
> <br>
> <font color=#003780> **主要功能**</font>
> <font color=#0057A0> &ensp; &ensp; 日志记录，性能统计，安全控制，事务处理，异常处理等等。</font>

总结：
- AOP是一种编程范式，从某一个角度（切面）来考虑程序结构。
- AOP为开发人员提供了一种描写叙述横切关注点的机制，并可以自己主动将横切关注点织入到面向对象的软件系统中。从而实现了横切关注点的模块化。
- AOP可以将那些与业务无关，却为业务模块所共同调用的逻辑或责任。比如事务处理、日志管理、权限控制等。封装起来，便于降低系统的反复代码，降低模块间的耦合度，并有利于未来的可操作性和可维护性。    
[参考文章](https://www.cnblogs.com/cynchanpin/p/6931707.html)

## 2.spring中的AOP相关术语
<font color=#003780>**Joinpoint( 连接点):**</font>

所谓连接点是指那些被拦截到的点。在 spring 中,这些点指的是方法,因为 spring 只支持方法类型的
连接点。

举个栗子: 平时我们写的业务层Service层接口中的方法。
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720133337.png)

<font color=#003780>**Pointcut( 切入点):**</font>

所谓切入点是指我们要对哪些 Join point 进行拦截的定义。即那些被切入的方法，就是我们进行了增强的方法。
并不是所有的连接点都会被增强，即不是所有连接点都是切入点。


<font color=#003780>**Advice( 通知/ 增强):**</font>

所谓通知是指拦截到 Join point 之后所要做的事情就是通知。
通知的类型：前置通知，后置通知，异常通知，最终通知，环绕通知。

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720133343.png)

<font color=#003780>**Introduction( 引介):**</font>
引介是一种特殊的通知在不修改类代码的前提下, Introduction 可以在运行期为类动态地添加一些方
法或 Field。

<font color=#003780>**Target( 目标对象):**</font>
代理的目标对象。

<font color=#003780>**Weaving( 织入):**</font>
是指把增强应用到目标对象来创建新的代理对象的过程。
spring 采用动态代理织入，而 AspectJ 采用编译期织入和类装载期织入。

<font color=#003780>**Proxy （代理）:**</font>
一个类被 AOP 织入增强后，就产生一个结果代理类。

<font color=#003780>**Aspect( 切面):**</font>
是切入点和通知（引介）的结合。

## 3. 学习 Spring 中的 AOP要明确的事
**a 、开发阶段（我们做的）**

- 编写核心业务代码（开发主线）：大部分程序员来做，要求熟悉业务需求。
- 把公用代码抽取出来，制作成通知。（开发阶段最后再做）：AOP 编程人员来做。
- 在配置文件中，声明切入点与通知间的关系，即切面。：AOP 编程人员来做。


**b 、运行阶段（Spring  框架完成的）**
- Spring 框架监控切入点方法的执行。一旦监控到切入点方法被运行，使用代理机制，动态创建目标对象的代理对象，根据通知类别，在代理对象的对应位置，将通知对应的功能织入，完成完整的代码逻辑运行。

**关于代理的选择**

- 在 spring 中，框架会根据目标类是否实现了接口来决定采用哪种动态代理的方式。

## 4. 基于XML 的AOP配置
第一步：环境搭建，加入pom依赖。和对应的bean.xml的配置

```xml
  <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.7</version>
        </dependency>
    </dependencies>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">
</beans>
```
第二步：编写模拟业务类、Logger类（通知类)

> <font color=#000>**业务类**</font>
> ```java 
> public interface IAccountService {
> 
>  void saveAccount();
> 
>  void updateAccount(int i);
> 
>  int deleteAccount();
>   }
> 
> ```
>
> ```java 
> public class AccountServiceImpl implements IAccountService {
>  public void saveAccount() {
>      System.out.println("执行了保存方法");
>    }
> 
>  public void updateAccount(int i) {
>      System.out.println("执行了更新方法："+i);
>    }
> 
>  public int deleteAccount() {
>      System.out.println("执行了删除方法");
>      return 0;
>      } 
>  }
> 
> ```
>
>
> <font color=#000>**通知类Logger**</font>
>
> ```java 
> /**  
> * 用于记录日志的工具类，它里面提供了公共的代码  
> */
>  public class Logger {
>     /**
>      * 用于打印日志，计划其在切入点方法执行之前执行
>      */
>     	public void printLog(){
>        		 System.out.println("Logger类中的printLog方法开始记录日志了。。。");
>   	  }
>      }
> ```

第三步：在bean.xml中，配置AOP

```java
    <!-- 配置service-->
    <bean id="accountService" class="code.service.impl.AccountServiceImpl"></bean>

    <!-- 配置Logger-->
    <bean id="logger" class="code.utils.Logger"></bean>
    
    <!-- 配置AOP-->
	<aop:config>
	    <aop:aspect id="logAdvice" ref="logger">
	<!--        配置通知的类型，并且建立通知方法和切入点方法的关联-->
	        <aop:before method="beforeprintLog" pointcut="execution(public void code.service.impl.AccountServiceImpl.saveAccount())">
            </aop:before>
	    </aop:aspect>
	</aop:config>

```

说明：
>  <font color=#000>**spring中基于XML的AOP配置步骤**</font>
>
>  &ensp;&ensp; &ensp;&ensp;1、把通知Bean也交给spring来管理
>  <br>
>  &ensp;&ensp; &ensp;&ensp;2、使用aop:config标签表明开始AOP的配置
>  <br>
>  &ensp;&ensp; &ensp;&ensp;3、使用aop:aspect标签表明配置切面
>  &ensp;&ensp; &ensp;&ensp;&ensp;&ensp; &ensp;&ensp;   id属性：是给切面提供一个唯一标识
>  &ensp;&ensp; &ensp;&ensp;&ensp;&ensp; &ensp;&ensp;  ref属性：是指定通知类bean的Id。
>  &ensp;&ensp; &ensp;&ensp;4、在aop:aspect标签的内部使用对应标签来配置通知的类型
>  &ensp;&ensp; &ensp;&ensp;&ensp;&ensp; &ensp;&ensp; 我们现在示例是让printLog方法在切入点方法执行之前之前：所以是前置通知
>  &ensp;&ensp; &ensp;&ensp;&ensp;&ensp; &ensp;&ensp;   aop:before：表示配置前置通知
>  &ensp;&ensp; &ensp;&ensp;&ensp;&ensp; &ensp;&ensp; method属性：用于指定Logger类中哪个方法是前置通知
>  &ensp;&ensp; &ensp;&ensp;&ensp;&ensp; &ensp;&ensp;  pointcut属性：用于指定切入点表达式，该表达式的含义指的是对业务层中哪些方法增强

<table><tr><td bgcolor=#DCC6E0>
  切入点表达式的写法：
  关键字：execution(表达式)
</td></tr></table>



>  <font color=#000>**表达式规则：**  访问修饰符  返回值  包名.包名.包名...类名.方法名(参数列表)</font>
>  <br>
>  标准的表达式写法：
>  &ensp;&ensp; &ensp;&ensp;public void com.itheima.service.impl.AccountServiceImpl.saveAccount()
>               访问修饰符可以省略
>  &ensp;&ensp; &ensp;&ensp; void com.itheima.service.impl.AccountServiceImpl.saveAccount()
>               返回值可以使用通配符，表示任意返回值
>  &ensp;&ensp; &ensp;&ensp;  * com.itheima.service.impl.AccountServiceImpl.saveAccount()
>               包名可以使用通配符，表示任意包。但是有几级包，就需要写几个*.
>
>  ```
>  * *.*.*.*.AccountServiceImpl.saveAccount())
>  ```
>
>  ​             包名可以使用..表示当前包及其子包
>  &ensp;&ensp; &ensp;&ensp; * *..AccountServiceImpl.saveAccount()
>  ​             类名和方法名都可以使用*来实现通配
>  &ensp;&ensp; &ensp;&ensp; * *..*.*()
>
>  <br>
>  <font color=#000>
>  -  实际开发中切入点表达式的通常写法：  </font>
>
>  &ensp;&ensp; &ensp;&ensp;  切到业务层实现类下所有方法：
>   &ensp;&ensp; &ensp;&ensp;  * code.service.impl.*.* (..) 
>       

## 5. XML中的四种常用通知类型
xml配置
```java
<!--    配置AOP-->
<aop:config>
    <aop:aspect id="logAdvice" ref="logger">
<!--        配置通知的类型，并且建立通知方法和切入点方法的关联-->

<!--        前置通知：在切入点方法执行之前执行-->
        <aop:before method="beforeprintLog" pointcut-ref="pt1" ></aop:before>
<!--        后置通知：在切入点方法正常执行之后-->
        <aop:after-returning method="afterprintLog" pointcut-ref="pt1"></aop:after-returning>
<!--        异常通知：在切入点方法执行产生异常之后执行-->
        <aop:after-throwing method="throwprintLog"  pointcut-ref="pt1"></aop:after-throwing>
<!--        最终通知：无论切入点方法是否正常执行，它都会在后面执行-->
        <aop:after method="finalprintLog"  pointcut-ref="pt1"></aop:after>
<!--        环绕通知-->
        <aop:around method="aroundprintLog" pointcut-ref="pt1"></aop:around>
        
<!--        单独定义切入点表达式-->
<!--    在切面标签内部：只在当前切面使用
        在切面标签外部：对所有切面使用
-->
        <aop:pointcut id="pt1" expression="execution( * code.service.impl.*.*(..))"/>
    </aop:aspect>
</aop:config>
```

Logger类
```java
/**
 * 用于记录日志的工具类，它里面提供了公共的代码
 */
public class Logger {
    /**
     *前置通知
     */
    public void beforeprintLog(){
        System.out.println("前置通知。。。");
    }

    /**
     *后置通知
     */
    public void afterprintLog(){
        System.out.println("后置通知。。。");
    }

    /**
     *异常通知
     */
    public void throwprintLog(){
        System.out.println("异常通知。。。");
    }

    /**
     *最终通知
     */
    public void finalprintLog(){
        System.out.println("最终通知。。。");
    }
    /**
     *环绕通知
     * 问题：
     *  当我们配置了环绕通知后，切入点方法没有执行，而通知方法执行了。
     *  分析：
     *  通过对比动态代理中的环绕通知代码，发现动态代理的环绕通知有明确的切入点方法调用。
     *  解决：
     *      spring框架为我们提供了一个接口，ProceedingJoinPoint。该接口有一个方法Proceed()
     *      此方法就相当于明确了调用切入点方法
     *      该接口可以作为环绕通知的方法参数，在程序执行时，spring框架会为我们提供该接口的实现类供我们使用。
     */
    public Object aroundprintLog(ProceedingJoinPoint pjp){
        Object returnValue=null;
        try {
            Object[] args=pjp.getArgs();//得到切入点方法的参数
            System.out.println("环绕中的前置通知");
            returnValue = pjp.proceed(args);//明确调用业务层方法（切入点方法）
            System.out.println("环绕中的后置通知");

        }catch (Throwable t){
            System.out.println("环绕中的异常通知");
        }finally {
            System.out.println("环绕中的最终通知");
        }
        return returnValue;
    }
}

```
## 6. 基于注解的AOP配置
改造bean.xml

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

<!--    配置spring创建容器时要扫描的包-->
<context:component-scan base-package="code"></context:component-scan>
    
<!--    配置spring开启注解AOP的支持-->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>

</beans>
```

改造Logger类，加上相应的注解
```java
/**
 * 用于记录日志的工具类，它里面提供了公共的代码
 */
@Component("logger")
@Aspect//表示当前类是切面类
public class Logger {

    @Pointcut("execution(* code.service.impl.*.*(..)))")
    private void pt1(){

    }

    /**
     *前置通知
     */
  //  @Before("pt1()")
    public void beforeprintLog(){
        System.out.println("前置通知。。。");
    }

    /**
     *后置通知
     */
  //  @AfterReturning("pt1()")
    public void afterprintLog(){
        System.out.println("后置通知。。。");
    }

    /**
     *异常通知
     */
 //   @AfterThrowing("pt1()")
    public void throwprintLog(){
        System.out.println("异常通知。。。");
    }

    /**
     *最终通知
     */
  //  @After("pt1()")
    public void finalprintLog(){
        System.out.println("最终通知。。。");
    }
   
    @Around("pt1()")
    public Object aroundprintLog(ProceedingJoinPoint pjp){
        Object returnValue=null;
        try {
            Object[] args=pjp.getArgs();//得到切入点方法的参数
            System.out.println("环绕中的前置通知");
            returnValue = pjp.proceed(args);//明确调用业务层方法（切入点方法）
            System.out.println("环绕中的后置通知");

        }catch (Throwable t){
            System.out.println("环绕中的异常通知");
        }finally {
            System.out.println("环绕中的最终通知");
        }
        return returnValue;
    }
}

```





# （十二 ）练习：XML实现AOP事务控制

## 1. 工程目录结构
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705201437.png)
## 2. pom.xml

```xml
 <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>commons-dbutils</groupId>
            <artifactId>commons-dbutils</artifactId>
            <version>1.4</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>

        <dependency>
            <groupId>c3p0</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.1.2</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.7</version>
        </dependency>
    </dependencies>
```

## 3. 账户实体类Account

```java
/**
 * 账户的实体类
 */
public class Account implements Serializable {

    private Integer id;
    private String name;
    private Float money;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Float getMoney() {
        return money;
    }

    public void setMoney(Float money) {
        this.money = money;
    }

    @Override
    public String toString() {
        return "Account{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", money=" + money +
                '}';
    }
}

```

## 4. 业务层AccountService
业务层接口
```java
/**
 * 账户的业务层接口
 */
public interface IAccountService {
    /**
     * 转账
     * @param sourceName        转出账户名称
     * @param targetName        转入账户名称
     * @param money             转账金额
     */
    void transfer(String sourceName, String targetName, Float money);
}

```

业务层实现类
```java
/**
 * 账户的业务层实现类
 *
 * 事务控制应该都是在业务层
 */
public class AccountServiceImpl implements IAccountService{

    private IAccountDao accountDao;
  //基于XML的set注入
    public void setAccountDao(IAccountDao accountDao) {
        this.accountDao = accountDao;
    }
 //这里省略其他方法

    public void transfer(String sourceName, String targetName, Float money) {
        System.out.println("transfer....");
            //2.1根据名称查询转出账户
            Account source = accountDao.findAccountByName(sourceName);
            //2.2根据名称查询转入账户
            Account target = accountDao.findAccountByName(targetName);
            //2.3转出账户减钱
            source.setMoney(source.getMoney()-money);
            //2.4转入账户加钱
            target.setMoney(target.getMoney()+money);
            //2.5更新转出账户
            accountDao.updateAccount(source);

            int i=1/0;

            //2.6更新转入账户
            accountDao.updateAccount(target);
    }
}

```

## 5. 持久层AccountDao
持久层接口IAccountDao
```java
/**
 * 账户的持久层接口
 */
public interface IAccountDao {

    /**
     * 更新
     * @param account
     */
    void updateAccount(Account account);

    /**
     * 根据名称查询账户
     * @param accountName
     * @return  如果有唯一的一个结果就返回，如果没有结果就返回null
     *          如果结果集超过一个就抛异常
     */
    Account findAccountByName(String accountName);
}

```

持久层实现类AccountDaoImpl
```java
/**
 * 账户的持久层实现类
 */
public class AccountDaoImpl implements IAccountDao {

    private QueryRunner runner;
    private ConnectionUtils connectionUtils;

    public void setRunner(QueryRunner runner) {
        this.runner = runner;
    }

    public void setConnectionUtils(ConnectionUtils connectionUtils) {
        this.connectionUtils = connectionUtils;
    }
    
   public void updateAccount(Account account) {
        try{
            runner.update(connectionUtils.getThreadConnection(),"update account set name=?,money=? where id=?",account.getName(),account.getMoney(),account.getId());
        }catch (Exception e) {
            throw new RuntimeException(e);
        }
    }


    public Account findAccountByName(String accountName) {
        try{
            List<Account> accounts = runner.query(connectionUtils.getThreadConnection(),"select * from account where name = ? ",new BeanListHandler<Account>(Account.class),accountName);
            if(accounts == null || accounts.size() == 0){
                return null;
            }
            if(accounts.size() > 1){
                throw new RuntimeException("结果集不唯一，数据有问题");
            }
            return accounts.get(0);
        }catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}

```

## 6. Utils工具类实现一个线程一个连接和事务：
### 6.1 ConnectionUtils连接工具类

```java
/**
 * 连接的工具类，它用于从数据源中获取一个连接，并且实现和线程的绑定
 */
public class ConnectionUtils {

    private ThreadLocal<Connection> tl = new ThreadLocal<Connection>();

    private DataSource dataSource;

    public void setDataSource(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    /**
     * 获取当前线程上的连接
     * @return
     */
    public Connection getThreadConnection() {
        try{
            //1.先从ThreadLocal上获取
            Connection conn = tl.get();
            //2.判断当前线程上是否有连接
            if (conn == null) {
                //3.从数据源中获取一个连接，并且存入ThreadLocal中
                conn = dataSource.getConnection();
                tl.set(conn);
            }
            //4.返回当前线程上的连接
            return conn;
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }

    /**
     * 把连接和线程解绑
     */
    public void removeConnection(){
        tl.remove();
    }
}

```

### 6.2 TransactionManager事务管理的工具类

```java
/**
 * 和事务管理相关的工具类，它包含了，开启事务，提交事务，回滚事务和释放连接
 */
public class TransactionManager {

    private ConnectionUtils connectionUtils;

    public void setConnectionUtils(ConnectionUtils connectionUtils) {
        this.connectionUtils = connectionUtils;
    }

    /**
     * 开启事务
     */
    public  void beginTransaction(){
        try {
            connectionUtils.getThreadConnection().setAutoCommit(false);
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    /**
     * 提交事务
     */
    public  void commit(){
        try {
            connectionUtils.getThreadConnection().commit();
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    /**
     * 回滚事务
     */
    public  void rollback(){
        try {
            connectionUtils.getThreadConnection().rollback();
        }catch (Exception e){
            e.printStackTrace();
        }
    }


    /**
     * 释放连接
     */
    public  void release(){
        try {
            connectionUtils.getThreadConnection().close();//还回连接池中
            connectionUtils.removeConnection();
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}

```


## 7. bean.xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">


     <!-- 配置Service -->
    <bean id="accountService" class="code.service.impl.AccountServiceImpl">
        <!-- 注入dao -->
        <property name="accountDao" ref="accountDao"></property>
    </bean>

    <!--配置Dao对象-->
    <bean id="accountDao" class="code.dao.impl.AccountDaoImpl">
        <!-- 注入QueryRunner -->
        <property name="runner" ref="runner"></property>
        <!-- 注入ConnectionUtils -->
        <property name="connectionUtils" ref="connectionUtils"></property>
    </bean>

    <!--配置QueryRunner-->
    <bean id="runner" class="org.apache.commons.dbutils.QueryRunner" scope="prototype"></bean>

    <!-- 配置数据源 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <!--连接数据库的必备信息-->
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/spring"></property>
        <property name="user" value="root"></property>
        <property name="password" value="root"></property>
    </bean>

    <!-- 配置Connection的工具类 ConnectionUtils -->
    <bean id="connectionUtils" class="code.utils.ConnectionUtils">
        <!-- 注入数据源-->
        <property name="dataSource" ref="dataSource"></property>
    </bean>

    <!-- 配置事务管理器-->
    <bean id="txManager" class="code.utils.TransactionManager">
        <!-- 注入ConnectionUtils -->
        <property name="connectionUtils" ref="connectionUtils"></property>
    </bean>


    <aop:config>
        <aop:pointcut id="pt1" expression="execution(* code.service.impl.*.*(..))"></aop:pointcut>
      <aop:aspect id="txAdvice" ref="txManager">
          <!--            配置前置通知：开启事务-->
          <aop:before method="beginTransaction" pointcut-ref="pt1"></aop:before>
          <!--            配置后置通知：提交事务-->
          <aop:after-returning method="commit" pointcut-ref="pt1"></aop:after-returning>
          <!--            配置异常通知：回滚事务-->
          <aop:after-throwing method="rollback" pointcut-ref="pt1"></aop:after-throwing>
          <!--            配置最终对象：释放连接-->
          <aop:after method="release" pointcut-ref="pt1"></aop:after>

      </aop:aspect>

    </aop:config>


</beans>
```


## 8. 测试类AccountServiceTest

```java
/**
 * 使用Junit单元测试：测试我们的配置
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:bean.xml")
public class AccountServiceTest {

    @Autowired
    private  IAccountService as;

    @Test
    public  void testTransfer(){

        as.transfer("aaa","bbb",100f);
    }

}

```

## 9. 执行结果
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705201619.png)

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705201626.png)

![在这里插入图片描述](G:\图片\blog\20200225223146285.png)



# （十三）注解实现AOP事务控制



## 1. pom.xml依赖

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>commons-dbutils</groupId>
            <artifactId>commons-dbutils</artifactId>
            <version>1.4</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>

        <dependency>
            <groupId>c3p0</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.1.2</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.7</version>
        </dependency>
    </dependencies>

```


## 2. bean.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

<!--配置spring创建容器时要扫描的包-->
    <context:component-scan base-package="code"></context:component-scan>
    
    <!--配置QueryRunner-->
    <bean id="runner" class="org.apache.commons.dbutils.QueryRunner" scope="prototype"></bean>

    <!-- 配置数据源 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <!--连接数据库的必备信息-->
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/spring"></property>
        <property name="user" value="root"></property>
        <property name="password" value="root"></property>
    </bean>
    
    <!--    告知spring开启注解的AOP配置-->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
    
</beans>
```

## 3. 账户实体类Account

```java
/**
 * 账户的实体类
 */
public class Account implements Serializable {

    private Integer id;
    private String name;
    private Float money;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Float getMoney() {
        return money;
    }

    public void setMoney(Float money) {
        this.money = money;
    }

    @Override
    public String toString() {
        return "Account{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", money=" + money +
                '}';
    }
}

```


## 4. 业务层AccountService
IAccountService 
```java

/**
 * 账户的业务层接口
 */
public interface IAccountService {
    /**
     * 查询所有
     * @return
     */
    List<Account> findAllAccount();

    /**
     * 查询一个
     * @return
     */
    Account findAccountById(Integer accountId);

    /**
     * 保存
     * @param account
     */
    void saveAccount(Account account);

    /**
     * 更新
     * @param account
     */
    void updateAccount(Account account);

    /**
     * 删除
     * @param acccountId
     */
    void deleteAccount(Integer acccountId);

    /**
     * 转账
     * @param sourceName        转出账户名称
     * @param targetName        转入账户名称
     * @param money             转账金额
     */
    void transfer(String sourceName, String targetName, Float money);

    //void test();//它只是连接点，但不是切入点，因为没有被增强
}

```
AccountServiceImpl 
```java
/**
 * 账户的业务层实现类
 *
 * 事务控制应该都是在业务层
 */
@Service("accountService")
public class AccountServiceImpl implements IAccountService{

    @Autowired
    private IAccountDao accountDao;

    public List<Account> findAllAccount() {
        return accountDao.findAllAccount();
    }

    public Account findAccountById(Integer accountId) {
        return accountDao.findAccountById(accountId);
   }

    public void saveAccount(Account account) {
        accountDao.saveAccount(account);
    }

    public void updateAccount(Account account) {
        accountDao.updateAccount(account);
    }
    
    public void deleteAccount(Integer acccountId) {
        accountDao.deleteAccount(acccountId);
    }

    public void transfer(String sourceName, String targetName, Float money) {
        System.out.println("transfer....");
        //2.1根据名称查询转出账户
        Account source = accountDao.findAccountByName(sourceName);
        //2.2根据名称查询转入账户
        Account target = accountDao.findAccountByName(targetName);
        //2.3转出账户减钱
        source.setMoney(source.getMoney()-money);
        //2.4转入账户加钱
        target.setMoney(target.getMoney()+money);
        //2.5更新转出账户
        accountDao.updateAccount(source);

        int i=1/0;

        //2.6更新转入账户
        accountDao.updateAccount(target);
    }
}

```
## 5. 持久层AccountDao
IAccountDao 
```java
/**
 * 账户的持久层接口
 */
public interface IAccountDao {

    /**
     * 查询所有
     * @return
     */
    List<Account> findAllAccount();

    /**
     * 查询一个
     * @return
     */
    Account findAccountById(Integer accountId);

    /**
     * 保存
     * @param account
     */
    void saveAccount(Account account);

    /**
     * 更新
     * @param account
     */
    void updateAccount(Account account);

    /**
     * 删除
     * @param acccountId
     */
    void deleteAccount(Integer acccountId);

    /**
     * 根据名称查询账户
     * @param accountName
     * @return  如果有唯一的一个结果就返回，如果没有结果就返回null
     *          如果结果集超过一个就抛异常
     */
    Account findAccountByName(String accountName);
}

```
AccountDaoImpl 
```java
/**
 * 账户的持久层实现类
 */
@Repository("accountDao")
public class AccountDaoImpl implements IAccountDao {

    @Autowired
    private QueryRunner runner;
    @Autowired
    private ConnectionUtils connectionUtils;



    public List<Account> findAllAccount() {
        try{
            return runner.query(connectionUtils.getThreadConnection(),"select * from account",new BeanListHandler<Account>(Account.class));
        }catch (Exception e) {
            throw new RuntimeException(e);
        }
    }


    public Account findAccountById(Integer accountId) {
        try{
            return runner.query(connectionUtils.getThreadConnection(),"select * from account where id = ? ",new BeanHandler<Account>(Account.class),accountId);
        }catch (Exception e) {
            throw new RuntimeException(e);
        }
    }


    public void saveAccount(Account account) {
        try{
            runner.update(connectionUtils.getThreadConnection(),"insert into account(name,money)values(?,?)",account.getName(),account.getMoney());
        }catch (Exception e) {
            throw new RuntimeException(e);
        }
    }


    public void updateAccount(Account account) {
        try{
            runner.update(connectionUtils.getThreadConnection(),"update account set name=?,money=? where id=?",account.getName(),account.getMoney(),account.getId());
        }catch (Exception e) {
            throw new RuntimeException(e);
        }
    }


    public void deleteAccount(Integer accountId) {
        try{
            runner.update(connectionUtils.getThreadConnection(),"delete from account where id=?",accountId);
        }catch (Exception e) {
            throw new RuntimeException(e);
        }
    }


    public Account findAccountByName(String accountName) {
        try{
            List<Account> accounts = runner.query(connectionUtils.getThreadConnection(),"select * from account where name = ? ",new BeanListHandler<Account>(Account.class),accountName);
            if(accounts == null || accounts.size() == 0){
                return null;
            }
            if(accounts.size() > 1){
                throw new RuntimeException("结果集不唯一，数据有问题");
            }
            return accounts.get(0);
        }catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}

```


## 6. Utils工具类实现一个线程一个连接和事务：

### 6.1 utils：connectionUtils

```java
/**
 * 连接的工具类，它用于从数据源中获取一个连接，并且实现和线程的绑定
 */
@Component("connectionUtils")
public class ConnectionUtils {

    private ThreadLocal<Connection> tl = new ThreadLocal<Connection>();

    @Autowired
    private DataSource dataSource;


    /**
     * 获取当前线程上的连接
     * @return
     */
    public Connection getThreadConnection() {
        try{
            //1.先从ThreadLocal上获取
            Connection conn = tl.get();
            //2.判断当前线程上是否有连接
            if (conn == null) {
                //3.从数据源中获取一个连接，并且存入ThreadLocal中
                conn = dataSource.getConnection();
                conn.setAutoCommit(false);
                tl.set(conn);
            }
            //4.返回当前线程上的连接
            return conn;
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }

    /**
     * 把连接和线程解绑
     */
    public void removeConnection(){
        tl.remove();
    }
}

```
### 6.2 utils：TransactionManager 

```java
/**
 * 和事务管理相关的工具类，它包含了，开启事务，提交事务，回滚事务和释放连接
 */
@Component("txManager")
@Aspect
public class TransactionManager {

    @Autowired
    private ConnectionUtils connectionUtils;

  @Pointcut("execution(* code.service.impl.*.*(..))")
    public void pt1(){

    }

    /**
     * 开启事务
     */
   // @Before("pt1()")
    public  void beginTransaction(){
        try {
            connectionUtils.getThreadConnection().setAutoCommit(false);
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    /**
     * 提交事务
     */
   // @AfterReturning("pt1()")
    public  void commit(){
        try {
            connectionUtils.getThreadConnection().commit();
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    /**
     * 回滚事务
     */
   // @AfterThrowing("pt1()")
    public  void rollback(){
        try {
            connectionUtils.getThreadConnection().rollback();
        }catch (Exception e){
            e.printStackTrace();
        }
    }


    /**
     * 释放连接
     */
   // @After("pt1()")
    public  void release(){
        try {
            connectionUtils.getThreadConnection().close();//还回连接池中
            connectionUtils.removeConnection();
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    @Around("pt1()")
    public Object  aroundAdvice(ProceedingJoinPoint pjp){
        Object rtValue=null;
        try {
            //1.得到切入点方法的参数
            Object[] args = pjp.getArgs();
            //2.前置通知：开启事务
            this.beginTransaction();
            //3.调用切入点方法
            rtValue = pjp.proceed(args);

            //4.后置通知：提交事务
            this.commit();
        }catch (Throwable e){
            //5.异常通知
            this.rollback();
            throw new RuntimeException(e);
        }finally {
            //6.最终通知
            this.release();
        }
        return rtValue;
    }
}

```
## 7. 测试类AccountServiceTest 

```java
/**
 * 使用Junit单元测试：测试我们的配置
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:bean.xml")
public class AccountServiceTest {

    @Autowired
    private IAccountService as;

    @Test
    public  void testTransfer(){
        as.transfer("aaa","bbb",100f);
    }

}

```

















# （十二 ）Spring中的JdbcTemplate

## 1. JdbcTemplate概述与入门
它是 spring 框架中提供的一个对象，是对原始 Jdbc API 对象的简单封装。spring 框架为我们提供很多的操作模板类。

<font color=#009B90>操作关系型数据的：</font>

- JdbcTemplate
- HibernateTemplate

<font color=#009B90>操作 nosql 数据库的：</font>

- RedisTemplate

<font color=#009B90>操作消息队列的：</font>

- JmsTemplate

我们今天的主角在 `spring-jdbc-5.0.2.RELEASE.jar `中，我们在导包的时候，除了要导入这个 jar 包
外，还需要导入一个 `spring-tx-5.0.2.RELEASE.jar`（它是和事务相关的）。

操作数据库的持久层图：
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705200536.png)

## 2. 使用JdbcTemplate导包

```xml
<dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>

    </dependencies>

```
## 3. XML方式让spring的ioc容器管理JdbcTemplate

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"          xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">
        
<!--    配置AccountDao-->
<bean id="accountDao2" class="code.dao.impl.AccountDaoImpl2">
        <property name="dataSource" ref="dataSource"></property>
</bean>

<!--配置JdbcTemplate-->
<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <property name="dataSource" ref="dataSource"></property>
</bean>

<!--配置数据源-->
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
    <property name="url" value="jdbc:mysql://localhost:3306/spring"></property>
    <property name="username" value="root"></property>
    <property name="password" value="root"></property>
</bean>

</beans>
```
## 4. 使用继承JdbcDaoSupport方式
<font color=#009B90>**冗余代码的问题**</font>
在以前的持久层AccountDaoImpl类中，我们经常创建JdbcTemplate成员变量，并用它帮我们操作数据库。

```java
public class AccountDaoImpl implements IAccountDao {

   private JdbcTemplate jdbcTemplate;

    public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }
	//查询所有
    public List<Account> findAllAccount(){
        List<Account> allacount = jdbcTemplate.query("select * from account", new BeanPropertyRowMapper<Account>(Account.class));
        return allacount;
    }
```

**思考：**
此种方式有什么问题吗?
  有个小问题。就是我们的 dao 有很多时，每个 dao 都有一些`重复性的代码`。下面就是`重复代码`：

```java
private JdbcTemplate jdbcTemplate;

// 基于XML的 set注入 方式
public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
this.jdbcTemplate = jdbcTemplate;
}
```
**答案：**
能不能把它抽取出来呢？能。即让到继承JdbcDaoSupport





<font color=#009B90> **JdbcDaoSupport 是spring 框架为我们提供的一个类，该类中定义了一个 JdbcTemplate 对象，我们可以直接获取使用，但是要想创建该对象，需要为其提供一个数据源：
JdbcDaoSupport 具体源码如下：**</font>

```java
public abstract class JdbcDaoSupport extends DaoSupport {
//定义对象
private JdbcTemplate jdbcTemplate;
//set 方法注入数据源，判断是否注入了，注入了就创建 JdbcTemplate
public final void setDataSource(DataSource dataSource) {
if (this.jdbcTemplate == null || dataSource != this.jdbcTemplate.getDataSource())
{  //如果提供了数据源就创建 JdbcTemplate
this.jdbcTemplate = createJdbcTemplate(dataSource);
initTemplateConfig();
}
}
//使用数据源创建 JdcbTemplate
protected JdbcTemplate createJdbcTemplate(DataSource dataSource) {
return new JdbcTemplate(dataSource);
}
//当然，我们也可以通过注入 JdbcTemplate 对象
public final void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
this.jdbcTemplate = jdbcTemplate;
initTemplateConfig();
}
//使用 getJdbcTmeplate 方法获取操作模板对象
public final JdbcTemplate getJdbcTemplate() {
return this.jdbcTemplate;
}
```

<font color=#009B90>**账户的持久层实现类，此版本 dao，只需要给它的父类注入一个数据源** </font>


```java
public class AccountDaoImpl2 extends JdbcDaoSupport implements IAccountDao {

//getJdbcTemplate()方法是从父类上继承下来的。
    public List<Account> findAllAccount() {
        List<Account> allacount = getJdbcTemplate().query("select * from account", new BeanPropertyRowMapper<Account>(Account.class));
        return allacount;
    }

    public Account findAccountById(Integer id) {
    //getJdbcTemplate()方法是从父类上继承下来的。
        List<Account> accounts = getJdbcTemplate().query("select * from account where id = ?", new BeanPropertyRowMapper<Account>(Account.class), id);

        return accounts.isEmpty()?null:accounts.get(0);

    }

```
<font color=#009B90> **bean.xml 配置accountDao时，必须要指明一个数据源** </font>
```java
<!-- 配置 dao2 -->
<bean id="accountDao2" class="com.itheima.dao.impl.AccountDaoImpl2">
<!-- 注入 dataSource -->
<property name="dataSource" ref="dataSource"></property>
</bean>
```



# （十四） Spring 中的声明式事务控制

## 1. 相关API介绍

第一：JavaEE 体系进行分层开发，事务处理位于业务层，Spring 提供了分层设计 `业务层`的事务处理解决方案。
第二：spring 框架为我们提供了一组事务控制的接口。具体在后面的第二小节介绍。这组接口是在
`spring-tx-5.0.2.RELEASE.jar `中。
第三：spring 的事务控制都是基于 AOP 的，它既可以使用`编程`的方式实现，也可以使用`配置`的方式实现。`我们学习的重点是使用配置的方式实现。`

### PlatformTransactionManager
此接口是 spring 的事务管理器，它里面提供了我们常用的操作事务的方法，如下图：
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705205449.png" alt="在这里插入图片描述" style="zoom:80%;" />
我们在开发中都是使用它的实现类，如下图：
真正管理事务的对象
org.springframework.jdbc.datasource.`DataSourceTransactionManager ` 用 使用 Spring
JDBC 或iBatis  进行持久化数据时使用
org.springframework.orm.hibernate5.`HibernateTransactionManager` 使用Hibernate 版本进行持久化数据时使用

### TransactionDefinition
它是事务的定义信息对象，里面有如下方法：
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705205819.png" alt="在这里插入图片描述" style="zoom:80%;" />

## 2. 事务的隔离级别

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705205931.png" alt="在这里插入图片描述" style="zoom:80%;" />

### 事务的传播行为
`REQUIRED`:如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务中。一般的选择（默认值）
`SUPPORTS`:支持当前事务，如果当前没有事务，就以非事务方式执行（没有事务）
MANDATORY：使用当前的事务，如果当前没有事务，就抛出异常
REQUERS_NEW:新建事务，如果当前在事务中，把当前事务挂起。
NOT_SUPPORTED:以非事务方式执行操作，如果当前存在事务，就把当前事务挂起
NEVER:以非事务方式运行，如果当前存在事务，抛出异常
NESTED:如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行 REQUIRED 类似的操作。

### 超时时间
默认值是-1，没有超时限制。如果有，以秒为单位进行设置。

### 是否是只读事务
建议查询时设置为只读。

### TransactionStatus
此接口提供的是事务具体的运行状态，方法介绍如下图：
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705210415.png" alt="在这里插入图片描述" style="zoom:80%;" />


## 3. spring基于XML的声明式事务控制
### pom依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>zy.code</groupId>
    <artifactId>day05_spring_02tx_xml</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>

        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.7</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.0.2.RELEASE</version>
        </dependency>

    </dependencies>

</project>
```


### bean.xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">
<!--    配置AccountService-->

   <bean id="accountService" class="code.service.impl.AccountServiceImpl">
       <property name="accountDao" ref="accountDao"></property>
   </bean>

<!--    配置AccountDao-->
   <bean id="accountDao" class="code.dao.impl.AccountDaoImpl">
       <property name="jdbcTemplate" ref="jdbcTemplate"></property>
   </bean>



<!--配置JdbcTemplate-->
<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <property name="dataSource" ref="dataSource"></property>
</bean>

<!--配置数据源-->
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
    <property name="url" value="jdbc:mysql://localhost:3306/spring"></property>
    <property name="username" value="root"></property>
    <property name="password" value="root"></property>
</bean>
<!--spring中基于XML的声明式事务控制配置步骤
    1.配置事务管理器
    2.配置事务的通知
        此时我们需要导入事务的约束 tx名称空间和约束，同时也需要AOP的
        使用tx:advice标签配置事务通知
        属性：
            id：给事务通知起一个唯一标识
            transaction-manager：给事务通知提供一个业务管理器的引用
    3.配置AOP中的通用切入点表达式
    4.建立事务通知和切入点表达式的对应关系
    5.配置事务的属性
            在事务的通知tx:advice标签内部

            isolation：用于指定事务的隔离级别。默认值是DEFAULT，表示使用数据库的默认隔离级别。
            propagation：用于指定事务的传播行为。默认值是REQUIRED，表示一定会有事务，增删改的选择。查询方法可以选择SUPPORTS。
            read-only：用于指定事务是否只读。只有查询方法才能设置为true。默认值是false，表示读写。
            timeout：用于指定事务的超时时间，默认值是-1，表示永不超时。如果指定了数值，以秒为单位。
            rollback-for：用于指定一个异常，当产生该异常时，事务回滚，产生其他异常时，事务不回滚。没有默认值。表示任何异常都回滚。
            no-rollback-for：用于指定一个异常，当产生该异常时，事务不回滚，产生其他异常时事务回滚。没有默认值。表示任何异常都回滚。

-->

<!--配置事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>

<!--  配置事务的通知  -->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="transfer" propagation="REQUIRED" read-only="false" ></tx:method>
            <tx:method name="find*" propagation="SUPPORTS" read-only="true"></tx:method>


        </tx:attributes>
    </tx:advice>

<!--    配置AOP-->
<aop:config>
<!--    配置切入点表达式-->
    <aop:pointcut id="pt1" expression="execution(* code.service.impl.*.*(..))"/>
<!--    建立切入点表达式和事务通知的对应关系-->
    <aop:advisor advice-ref="txAdvice" pointcut-ref="pt1"></aop:advisor>
</aop:config>

</beans>
```

## 4. spring基于注解的声明式事务控制
bean.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx" xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

<!--配置spring创建容器时要扫描的包-->
<context:component-scan base-package="code"></context:component-scan>

<!--配置JdbcTemplate-->
<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <property name="dataSource" ref="dataSource"></property>
</bean>

<!--配置数据源-->
<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
    <property name="url" value="jdbc:mysql://localhost:3306/spring"></property>
    <property name="username" value="root"></property>
    <property name="password" value="root"></property>
</bean>

<!--spring中基于XML的声明式事务控制配置步骤
    1.配置事务管理器
    2.开启spring对注解事务的支持
    3.在需要事务支持的地方使用 @Transactional 注解
-->
<!--    配置事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
<!--    开启spring对注解事务的支持-->
    <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>

</beans>
```

AccountServiceImpl

```java
/**
 * 账户的业务层实现类
 *
 * 事务控制应该都是在业务层
 */
@Service("accountService")
@Transactional(propagation = Propagation.SUPPORTS,readOnly = true)
public class AccountServiceImpl implements IAccountService {

    @Autowired
    private IAccountDao accountDao;
    
    public Account findAccountById(Integer accountId) {
        return accountDao.findAccountById(accountId);
    }

    @Transactional(propagation = Propagation.REQUIRED,readOnly = false)
    public void transfer(String sourceName, String targetName, Float money) {
        System.out.println("transfer....");
            //2.1根据名称查询转出账户
            Account source = accountDao.findAccountByName(sourceName);
            //2.2根据名称查询转入账户
            Account target = accountDao.findAccountByName(targetName);
            //2.3转出账户减钱
            source.setMoney(source.getMoney()-money);
            //2.4转入账户加钱
            target.setMoney(target.getMoney()+money);
            //2.5更新转出账户
            accountDao.updateAccount(source);

//            int i=1/0;

            //2.6更新转入账户
            accountDao.updateAccount(target);
    }
}

```
## 5. spring基于纯注解的声明式事务控制
tips：

```java
<!--    开启spring对注解事务的支持-->
    <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
```
替换为的注解：
<font color=#F1892D>  **@EnableTransactionManagement**  </font> //开启spring对注解事务的支持

剩余的XML与纯注解的替换关系如下图：
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705211023.png)

纯注解配置的代码 请看下一节
[【Spring框架】十五、Spring 基于纯注解配置的声明式事务控制]()





# （十五）Spring的面试题



1. IOC的过程有哪些？

首先我们在加载配置文件时都会有下面这样的代码

```java
//1.获取核心容器对象
ApplicationContext ac =new ClassPathXmlApplicationContext("bean.xml");
/*ApplicationContext ac =
new AnnotationConfigApplicationContext(SpringConfiguration.class);
*/
// 2.根据id获取bean对象
IAccountService as=(IAccountService) ac.getBean("accountService");
IAccountDao adao= (IAccountDao)ac.getBean("accountDao");

```

下面进入构造方法的源码：

```java
public ClassPathXmlApplicationContext(
		String[] configLocations, boolean refresh, @Nullable ApplicationContext parent)
		throws BeansException {

	super(parent);
    //根据提供的路径，处理成配置文件数组(以分号、逗号、空格、tab、换行符分割)
	setConfigLocations(configLocations);
	if (refresh) {
        //这个refresh();方法可以用来重新初始化ApplicationContext
		refresh();
	}
}

```





下面看refresh();方法的源码：

```java
@Override
	public void refresh() throws BeansException, IllegalStateException {
		synchronized (this.startupShutdownMonitor) {
			// Prepare this context for refreshing.
            //这个方法是做准备工作的，记录容器的启动时间、标记“已启动”状态、处理配置文件中的占位符
			prepareRefresh();

			// Tell the subclass to refresh the internal bean factory.
            //，这一步是把配置文件解析成一个个Bean，并且注册到BeanFactory中，注意这里只是注册进去，并没有初始化。
			ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();

			// Prepare the bean factory for use in this context.
            //设置 BeanFactory 的类加载器
			prepareBeanFactory(beanFactory);

			try {
				// Allows post-processing of the bean factory in context subclasses.
                //方法是提供给子类的扩展点，到这里的时候，所有的 Bean 都加载、注册完成了，但是都还没有初始化，具体的子类可以在这步的时候添加一些特殊的 BeanFactoryPostProcessor 的实现类，来完成一些其他的操作。
				postProcessBeanFactory(beanFactory);

				// Invoke factory processors registered as beans in the context.
                //这个方法是调用 BeanFactoryPostProcessor 各个实现类的 postProcessBeanFactory(factory) 方法；
				invokeBeanFactoryPostProcessors(beanFactory);

				// Register bean processors that intercept bean creation.
                //这个方法注册 BeanPostProcessor 的实现类，和上面的BeanFactoryPostProcessor 是有区别的，这个方法调用的其实是PostProcessorRegistrationDelegate类的registerBeanPostProcessors方法；这个类里面有个内部类BeanPostProcessorChecker，BeanPostProcessorChecker里面有两个方法postProcessBeforeInitialization和postProcessAfterInitialization，这两个方法分别在 Bean 初始化之前和初始化之后得到执行。然后回到refresh()方法中继续往下看
				registerBeanPostProcessors(beanFactory);

				// Initialize message source for this context.
                //法是初始化当前 ApplicationContext 的 MessageSource，国际化处理，继续往下
				initMessageSource();

				// Initialize event multicaster for this context.
                //方法初始化当前 ApplicationContext 的事件广播器继续往下
				initApplicationEventMulticaster();

				// Initialize other special beans in specific context subclasses.
                //方法初始化一些特殊的 Bean（在初始化 singleton beans 之前）；继续往下
				onRefresh();

				// Check for listener beans and register them.
                //方法注册事件监听器，监听器需要实现 ApplicationListener 接口；继续往下
				registerListeners();

				// Instantiate all remaining (non-lazy-init) singletons.
                //初始化所有的 singleton beans（单例bean），懒加载（non-lazy-init）的除外，这个方法也是等会细说
				finishBeanFactoryInitialization(beanFactory);

				// Last step: publish corresponding event.
                //法是最后一步，广播事件，ApplicationContext 初始化完成
				finishRefresh();
			}

			catch (BeansException ex) {
				if (logger.isWarnEnabled()) {
					logger.warn("Exception encountered during context initialization - " +
							"cancelling refresh attempt: " + ex);
				}

				// Destroy already created singletons to avoid dangling resources.
				destroyBeans();

				// Reset 'active' flag.
				cancelRefresh(ex);

				// Propagate exception to caller.
				throw ex;
			}

			finally {
				// Reset common introspection caches in Spring's core, since we
				// might not ever need metadata for singleton beans anymore...
				resetCommonCaches();
			}
		}
	}

```



原文链接：https://blog.csdn.net/qq_34203492/article/details/83865450