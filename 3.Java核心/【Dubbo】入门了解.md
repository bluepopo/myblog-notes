【Dubbo】入门了解

# 一、分布式RPC框架概述： Apache Dubbo

## 1. 软件架构的演变

### <font color=#0077C0>1. 单体架构 </font>

当网站流量很小时，`只需一个应用`，将`所有功能`都部署在一起，以减少部署节点和成本。
此时，用于简化增删改查工作量的数据访问框架(ORM)是关键。
适用于小型网站，小型管理系统，将所有功能都部署到一个功能里，简单易用。

**缺点：**
- 性能扩展比较难 
- 协同开发问题
- 不利于升级维护

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706155900.png)
### <font color=#0077C0>2. 垂直架构 </font>
当访问量逐渐增大，单一应用增加机器带来的加速度越来越小，将应用`拆成互不相干的几个应用`，以提升效率。此时，用于加速前端页面开发的Web框架(MVC)是关键。

 通过`切分业务`来实现各个`模块独立部署`，降低了维护和部署的难度，团队各司其职更易管理，性能扩展也更方便，更有针对性。

**缺点：** 
- 公用模块无法重复利用，开发性的浪费


![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706155940.png)
### <font color=#0077C0>3. SOA架构 </font>

SOA全称为	`Service-Oriented Architecture	`,即面向服务的架构。它可以根据需求通过网络对松散耦合的`粗粒度`应用组件(服务)进行分布式部署、组合和使用。一个服务通常以独立的形式存在于操作系统进程中。

站在`功能的角度`，把业务逻辑抽象成可复用的服务，通过服务的编排实现业务的快速再生，目的:把原先固有的业务功能转变为通用的业务服务，实现业务逻辑的快速复用。

**架构说明:**
将重复功能或模块抽取成组件的形式，对外提供服务,在项目与服务之间使用ESB (企业服务总线)的形式作为通信的桥梁。

**架构优点:** 
- 重复功能或模块抽取为服务，提高开发效率。
-	可重用性高。
-	可维护性高。

**架构缺点:**
-	各系统之间业务不同，很难确认功能或模块是重复的。
-	抽取服务的粒度大。
-	系统和服务之间耦合度高。

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706162808.png)

###  <font color=#0077C0>4. 微服务架构 </font>
将系统的服务层完全独立处理，抽取为一个个的微服务。
抽取的`粒度更细`，遵循单一原则。
采用轻量级框架协议传输。

**优点：**
 - 服务拆分粒度更细，提高开发效率
 - 可以针对不同服务定制对应的方案
 - 适用于互联网，产品的迭代周期更短

**缺点：**
- 粒度太细导致服务太多，维护成本高
- 分布式技术开发成本高

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706163042.png)

## 2. Dubbo简介
###  <font color=#0077C0>1. 什么是RPC </font>

`RPC【Remote Procedure Call】`是指`远程过程调用`，是一种进程间通信方式，他是一种技术的思想，而不是规范。它允许程序调用另一个地址空间（通常是共享网络的另一台机器上）的过程或函数，而不用程序员显式编码这个远程调用的细节。即程序员无论是调用本地的还是远程的函数，本质上编写的调用代码基本相同。

###  <font color=#0077C0>2. 什么是Dubbo</font>
Apache Dubbo (incubating) |ˈdʌbəʊ| 是一款高性能、轻量级的开源Java RPC框架，它提供了三大核心能力：面向接口的远程方法调用，智能容错和负载均衡，以及服务自动注册和发现。
官网：
[http://dubbo.apache.org/](http://dubbo.apache.org/)



###  <font color=#0077C0> 3. Dubbo架构</font>
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706160647.png)


**服务提供者（Provider）**：暴露服务的服务提供方，服务提供者在启动时，向注册中心`注册自己`提供的服务。

**服务消费者（Consumer）**: 调用远程服务的服务消费方，服务消费者在启动时，向注册中心`订阅`自己所需的服务，服务消费者，从提供者地址列表中，基于`软负载均衡算法`，选一台提供者进行调用，如果调用失败，再选另一台调用。

**注册中心（Registry）**：注册中心返回服务提供者地址列表给消费者，如果有`变更`，注册中心将基于`长连接推送`变更数据给消费者

**监控中心（Monitor）**：服务消费者和提供者，在内存中累计`调用次数`和`调用时间`，定时每分钟发送一次`统计数据`到监控中心


▶调用关系说明


	1.服务容器负责启动，加载，运行服务提供者。
	2.服务提供者在启动时，向注册中心注册自己提供的服务。
	3.服务消费者在启动时，向注册中心订阅自己所需的服务。
	4.注册中心返回服务提供者地址列表给消费者，如果有变更，注册中心将基于长连接推送变更数据给消费者。
	5.服务消费者，从提供者地址列表中，基于软负载均衡算法，选一台提供者进行调用，如果调用失败，再选另一台调用。
	6.服务消费者和提供者，在内存中累计调用次数和调用时间，定时每分钟发送一次统计数据到监控中心。



## 3. Zookeeper简介
Zookeeper是Apache Hadoop的子项目，是一个树型的目录服务 , 支持变更推送，适合作为Dubbo服务的`注册中心`，工业强度较高  ,可用于生产环境，并推荐使用。
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706161000.png)


**流程说明:**
- 服务提供者(Provider) 启动时:向 `/ dubbo/ com. foo. BarServi ce/providers `目录下写入自己的URL地址
- 服务消费者(Consumer) 启动时: 订阅` /dubbo/ com. foo. Bar Service/providers  `目录下的提供者URL地址。并向`  /dubbo/com. foo. BarService/ consumers  `目录下写入自己的URL地址
- 监控中心   (Monitor)启动时: 订阅  `/dubbo/com. foo. BarService ` 目录下的所有提供者和消费者URL地址





# 二、Dubbo入门案例

## 1. 环境搭建
1. 搭建服务器
   `java -jar dubbo-admin-0.0.1-SNAPSHOT.jar`命令打开服务，后访问  `http://localhost:7001/ ` 即可在dubbo admin 控制台看到服务者和消费者的信息

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706161312.png)

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706161304.png)

2. **准备两个mvn普通java工程**

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706181819.png)
**3.  对服务者、消费者工程pom.xml引入依赖**
注：版本问题这里两个依赖搭配必须同时引入

```xml
<!-- 引入Dubbo -->
<!-- https://mvnrepository.com/artifact/com.alibaba/dubbo -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>dubbo</artifactId>
    <version>2.6.2</version>
</dependency>
<!-- 注册中心使用的是zookeeper，引入操作zookeeper的客户端端 -->
<dependency>
	<groupId>org.apache.curator</groupId>
	<artifactId>curator-framework</artifactId>
	<version>2.12.0</version>
</dependency>

```

---

## 2. 公共接口模块
### 2.1 项目名称：gmall_interface
```xml
  <modelVersion>4.0.0</modelVersion>
    <groupId>zy.code</groupId>
    <artifactId>gmall_interface</artifactId>
    <version>1.0-SNAPSHOT</version>
```
### 2.2 用户接口 UserService 

```java
/**
 * 用户服务
 * @author lfy
 *
 */
public interface UserService {
	
	/**
	 * 按照用户id返回所有的收货地址
	 * @param userId
	 * @return
	 */
	public List<UserAddress> getUserAddressList(String userId);

}

```

### 2.3 订单接口 OrderService 

```java
public interface OrderService {
	
	/**
	 * 初始化订单
	 * @param userId
	 */
	public List<UserAddress> initOrder(String userId);

}

```

### 2.4 JavaBean UserAddress 

```java
/**
 * 用户地址
 * @author lfy
 *
 */
public class UserAddress implements Serializable {
	
	private Integer id;
    private String userAddress; //用户地址
    private String userId; //用户id
    private String consignee; //收货人
    private String phoneNum; //电话号码
    private String isDefault; //是否为默认地址    Y-是     N-否
    
    public UserAddress() {
		super();
		// TODO Auto-generated constructor stub
	}
    
	public UserAddress(Integer id, String userAddress, String userId, String consignee, String phoneNum,
                       String isDefault) {
		super();
		this.id = id;
		this.userAddress = userAddress;
		this.userId = userId;
		this.consignee = consignee;
		this.phoneNum = phoneNum;
		this.isDefault = isDefault;
	}
	
	public Integer getId() {
		return id;
	}
	public void setId(Integer id) {
		this.id = id;
	}
	public String getUserAddress() {
		return userAddress;
	}
	public void setUserAddress(String userAddress) {
		this.userAddress = userAddress;
	}
	public String getUserId() {
		return userId;
	}
	public void setUserId(String userId) {
		this.userId = userId;
	}
	public String getConsignee() {
		return consignee;
	}
	public void setConsignee(String consignee) {
		this.consignee = consignee;
	}
	public String getPhoneNum() {
		return phoneNum;
	}
	public void setPhoneNum(String phoneNum) {
		this.phoneNum = phoneNum;
	}
	public String getIsDefault() {
		return isDefault;
	}
	public void setIsDefault(String isDefault) {
		this.isDefault = isDefault;
	}
    
    


}
```



## 3. 服务提供者
###  3.1  在服务提供方实现接口
UserServiceImpl 
```java
public class UserServiceImpl implements UserService {
	
	public List<UserAddress> getUserAddressList(String userId) {

		UserAddress address1 = new UserAddress(1, "北京市昌平区宏福科技园综合楼3层", "1", "李老师", "010-56253825", "Y");
		UserAddress address2 = new UserAddress(2, "深圳市宝安区西部硅谷大厦B座3层（深圳分校）", "1", "王老师", "010-56253825", "N");

		return Arrays.asList(address1,address2);
	}

}
```


###  3.2 spring配置暴露服务
provider.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd
		http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd">
    
    <!-- 1、指定当前服务/应用的名字（同样的服务名字相同，不要和别的服务同名） -->
    <dubbo:application name="user-service-provider"></dubbo:application>
    <!-- 2、指定注册中心的位置 -->
    <dubbo:registry protocol="zookeeper" address="127.0.0.1:2181" />
    <!-- 3、指定通信规则（通信协议？通信端口） -->
    <dubbo:protocol name="dubbo" port="20882"></dubbo:protocol>
    <!-- 4、暴露服务   ref：指向服务的真正的实现对象 -->
    <dubbo:service interface="com.atguigu.gmall.service.UserService" ref="userServiceImpl">
    </dubbo:service>
    <!-- 服务的实现 -->
    <bean id="userServiceImpl" class="com.atguigu.gmall.service.impl.UserServiceImpl"></bean>

</beans>
```

###  3.3 测试服务者

```java
public class providerTest {

    public static void main(String[] args) throws IOException {
        ClassPathXmlApplicationContext ioc =new ClassPathXmlApplicationContext("provider.xml");
        ioc.start();

        System.in.read();
    }
}
```

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706162413.png)




## 4. 服务消费者
###   4.1 消费方远程调用服务方
```java
@Service
public class OrderServiceImpl implements OrderService {

    //自动注入依赖：远程的UserService业务
    @Autowired
    private UserService userService;

    public void initOrder(String userId) {
        // 查询用户的收货地址
        List<UserAddress> userAddressList = userService.getUserAddressList(userId);
        for (UserAddress userAddress:userAddressList){
            System.out.println(userAddress);
        }
    }
}

```

###   2.  Spring 配置消费方
consumer.xml
```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://dubbo.apache.org/schema/dubbo http://dubbo.apache.org/schema/dubbo/dubbo.xsd
		http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="com.atguigu.gmall.service.impl"></context:component-scan>

    <!-- 1、指定当前服务/应用的名字（同样的服务名字相同，不要和别的服务同名） -->
    <dubbo:application name="order-service-consumer"></dubbo:application>
    <!-- 2、指定注册中心的位置 -->
    <dubbo:registry protocol="zookeeper" address="127.0.0.1:2181" />
    <!--    声明需要远程调用的服务的接口，生成远程服务代理-->
    <dubbo:reference id="userService" interface="com.atguigu.gmall.service.UserService" ></dubbo:reference>

</beans>
```
###  3.测试消费方
consumerTest.java

```java
public class ConsumerTest {
    @SuppressWarnings("resource")
    public static void main(String[] args) throws IOException {
        ClassPathXmlApplicationContext ioc =new ClassPathXmlApplicationContext("consumer.xml");

        OrderService orderService = ioc.getBean(OrderService.class);
        orderService.initOrder("1");

        System.in.read();
    }
}
```