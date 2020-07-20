【SSM】整合案例（入门篇）





# （一）整合入门基础



## <font color=#009C41> 前言：整合的思路</font>

1. 整合说明：SSM整合可以使用多种方式，咱们会选择XML + 注解的方式
2. 整合的思路
1. 先搭建整合的环境
2. 先把Spring的配置搭建完成
3. 再使用Spring整合SpringMVC框架
4. 最后使用Spring整合MyBatis框架
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706104553.png)

## <font color=#009C41>一、搭建环境</font>
### 创建数据库和表

```sql
create database ssm;
use ssm;
create table account(
id int primary key auto_increment,
name varchar(20),
money double
);
```

### 创建maven的工程导入依赖

`（今天会使用到工程的聚合和拆分的概念，这个技术maven高级会讲）`
1. 创建ssm_parent父工程（打包方式选择pom，必须的）
2. 创建ssm_web子模块（打包方式是war包）
3. 创建ssm_service子模块（打包方式是jar包）
4. 创建ssm_dao子模块（打包方式是jar包）
5. 创建ssm_domain子模块（打包方式是jar包）
6. web依赖于service，service依赖于dao，dao依赖于domain
7. 在ssm_parent的pom.xml文件中引入坐标依赖

```xml
<properties>
    <spring.version>5.0.2.RELEASE</spring.version>
    <slf4j.version>1.6.6</slf4j.version>
    <log4j.version>1.2.12</log4j.version>
    <mysql.version>5.1.6</mysql.version>
    <mybatis.version>3.4.5</mybatis.version>
  </properties>

  <dependencies>
    <!-- spring -->
    <dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
      <version>1.6.8</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aop</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>${mysql.version}</version>
    </dependency>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>servlet-api</artifactId>
      <version>2.5</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>jsp-api</artifactId>
      <version>2.0</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>jstl</groupId>
      <artifactId>jstl</artifactId>
      <version>1.2</version>
    </dependency>
    <!-- log start -->
    <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>${log4j.version}</version>
    </dependency>
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-api</artifactId>
      <version>${slf4j.version}</version>
    </dependency>
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-log4j12</artifactId>
      <version>${slf4j.version}</version>
    </dependency>
    <!-- log end -->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>${mybatis.version}</version>
    </dependency>

    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>1.3.0</version>
    </dependency>
    <dependency>
      <groupId>c3p0</groupId>
      <artifactId>c3p0</artifactId>
      <version>0.9.1.2</version>
      <type>jar</type>
      <scope>compile</scope>
    </dependency>
  </dependencies>

```

### 工程目录
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706104559.png)



###  实体类domain

```java
public class Account implements Serializable {
    private Integer id;
    private  String name;
    private  Double money;

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

    public Double getMoney() {
        return money;
    }

    public void setMoney(Double money) {
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

### 持久层AccountDao
持久层只写接口
```java
/**
 * 账户dao接口
 */
public interface AccountDao {
    /**
     * 查询所有
     */
  List<Account> findAll();

    /**
     * 保存账户
     */
  void saveAccount(Account account);
  
}

```

###  业务层AccountService

```java
public interface AccountService {
    /**
     * 查询所有
     */
    List<Account> findAll();
    /**
     * 保存账户
     */
    void saveAccount(Account account);
}
```

```java
@Service("accountService")
public class AccountServiceImpl implements AccountService {

    public List<Account> findAll() {
        System.out.println("业务层,查询所有账户...");
        return null;
    }

    public void saveAccount(Account account) {
        System.out.println("业务层,保存账户...");
    }
}
```

###  表项层controller

在controller包下创建类AccountController，以后待用

Spring框架代码的编写

----

## <font color=#009C41>二、Spring框架测试 </font>
###   1. 搭建和测试Spring的开发环境
在`resources`资源目录下创建`ApplicationContext.xml`配置文件，用来进行`spring`的配置.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd">


    <!-- 开启注解扫描，要扫描的是service和dao层的注解，要忽略web层注解，因为web层让SpringMVC框架
去管理 -->
    <context:component-scan base-package="code">
        <!-- 配置要忽略的注解 -->
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>

</beans>
```
###   2. 编写测试方法，进行测试

```java
public class TestSpring {

    @Test
    public void run1(){
         // 1.加载 spring 的配置文件
        ApplicationContext ac =new ClassPathXmlApplicationContext("classpath:ApplicationContext.xml");

        // 2.获取对象
        AccountService as = (AccountService) ac.getBean("accountService");
        //3.调用方法
        as.findAll();
    }
}
```

---
## <font color=#009C41>二、SpringMVC框架测试</font>
###   1. 在web.xml中配置前端控制器

```xml
<web-app>
  <display-name>Archetype Created Web Application</display-name>

<!--  配置前端控制器-->
  <servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
<!--  加载 springmvc 配置文件-->
<init-param>
  <param-name>contextConfigLocation</param-name>
  <param-value>classpath:springmvc.xml</param-value>
</init-param>

<!--启动服务器，创建该servlet-->
<load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
  
<!--  解决中文乱码的过滤器-->
  <filter>
    <filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
  <init-param>
    <param-name>encoding</param-name>
    <param-value>UTF-8</param-value>
  </init-param>
  
  </filter>
  <filter-mapping>
    <filter-name>characterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>

</web-app>

```
###   2. springmvc.xml配置文件
在resources资源包下创建springmvc进行配置
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="
        http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

<!--开启注解扫描，只扫描 Controller 注解-->
<context:component-scan base-package="code">
<!--    只扫描Controller注解-->
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>

<!--    配置的视图解析器-->
<bean  id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/pages/"></property>
    <property name="suffix" value=".jsp"></property>
</bean>

<!--    过滤静态资源-->
    <!-- 设置静态资源不过滤 -->
    <mvc:resources mapping="/css/" location="/css/**"    ></mvc:resources>
    <mvc:resources mapping="/js/" location="/js/**"    ></mvc:resources>
    <mvc:resources mapping="/images/" location="/images/**"    ></mvc:resources>

    <!--    开启SpringMVC 注解的支持-->
    <mvc:annotation-driven />

</beans>
```
###   3. 测试springmvc单独的正确性
index.jsp

```html
<a href="account/findAll">查询所有</a>
```
controller控制器方法
```java
/**
 * 账户 web
 */
@Controller
@RequestMapping(path = "/account")
public class AccountController {

    @RequestMapping(path = "/findAll")
    public String findAll(){
        System.out.println("表项层,查询所有账户...");
        return "list";
    }
}
```

## <font color=#009C41> 三、Spring整合SpringMVC的框架</font>
###  1.  整合原理：
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706104606.png)
###   2.让web加载spring配置文件applicationContext.xml
1. 目的：在controller中能成功的调用service对象中的方法
2. 让springmvc在服务器启动时加载applicationContext.xml
在web.xml中配置监听器，让监听器在监听到服务器启动时就加载applicationContext.xml

`ContextLoaderListener监听器（该监听器只能加载WEB-INF目录下的applicationContext.xml的配文件）`
```xml
<!--  配置spring的监听器,默认只加载WEB-INF目录下的ApplicationContext文件-->
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:ApplicationContext.xml</param-value>
  </context-param>
```
###   3. 在controller中注入service对象，调用service的方法

```java
/**
 * 账户 web
 */
@Controller
@RequestMapping(path = "/account")
public class AccountController {

    @Autowired
    private AccountService accountService;

    @RequestMapping(path = "/findAll")
    public String findAll(){
        System.out.println("表项层,查询所有账户...");
        // 调用service方法
        accountService.findAll();
        return "list";
    }
}

```


<br>

---
<br>


##  <font color=#009C41> 四、MyBatis框架测试</font>
###   1. 使用注解方式编写sql语句

```java
/**
 * 账户dao接口
 */
public interface AccountDao {
    /**
     * 查询所有
     */
    @Select("select * from account")
  List<Account> findAll();

    /**
     * 保存账户
     */
    @Insert("insert into account (name,money) values(#{name},#{money})")
  void saveAccount(Account account);

}

```

###   2. 编写SqlMapConfig.xml的配置文件
在resources资源包下创建

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
<!--    配置环境-->
    <environments default="mysql">
        <environment id="mysql">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql:///ssm"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>

            </dataSource>
        </environment>
    </environments>

<!--    引入映射的配置文件-->
    <mappers>
<!--        <mapper class="code.dao.AccountDao"></mapper>-->
        <package name="code.dao"/>
    </mappers>
</configuration>
```
###   3. 测试能否操作数据库

```java
public class TestMyBatis {

    @Test
    public void run2() throws IOException {
        //1.加载配置文件
        InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
        //2.构建者模式创建工厂
        SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
        //3.工厂模式生成session
        SqlSession session = factory.openSession();
        //4.代理模式得到dao对象
        AccountDao dao = session.getMapper(AccountDao.class);
        //5.调用dao方法
        List<Account> accouts = dao.findAll();
        for (Account account:accouts){
            System.out.println(account);
        }
        //6.关闭资源
        session.close();
        in.close();

    }
    }
```

## <font color=#009C41> 五、Spring整合MyBatis框架 </font>
 <font color=#0077C0>**目的：
 把SqlMapConfig.xml配置文件中的内容配置到applicationContext.xml配置文件中。**</font>

 因为我们的AccounService已经让spring容器替我们管理了，需要使用时随时注入。

 因此我们也希望`AccountDao`也被`spring`容器管理，不需要再通过读取配置文件流使用工厂去手动创建。
 这样在AccountServiceImpl类中直接使用AccountDao为业务层服务。

###  1. 在ApplicationContex.xml中编写spring的MyBatis配置

```xml
<!--    spring整合MyBatis框架-->

<!--    配置连接池-->
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="driverClass" value="com.mysql.jdbc.Driver"/>
    <property name="jdbcUrl" value="jdbc:mysql:///ssm"/>
    <property name="user" value="root"/>
    <property name="password" value="root"/>
</bean>
<!--    配置SqlSessionFactory工厂-->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="dataSource"></property>
</bean>

<!--    配置AccountDao接口所在包-->
    <bean id="mapperScanner" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="code.dao"></property>
    </bean>
```
###   2. 在AccountDao接口中添加@Repository注解
###   3. 在service中注入dao对象，进行测试
**4. 代码如下**

1. AccountService层使用注解注入方式创建AccountDao
```java
@Service("accountService")
public class AccountServiceImpl implements AccountService {
    @Autowired
    private AccountDao accountDao;

    public List<Account> findAll() {
        System.out.println("业务层,查询所有账户...");
        List<Account> list = accountDao.findAll();
        return list;
    }

    public void saveAccount(Account account) {
        System.out.println("业务层,保存账户...");
        accountDao.saveAccount(account);
    }

}
```

2. AcoountController表现层发出查询命令，并将结果展示到跳转的页面上


```java
/**
 * 账户 web
 */
@Controller
@RequestMapping(path = "/account")
public class AccountController {
    @Autowired
    private AccountService accountService;

    @RequestMapping(path = "/findAll")
    public String findAll(Model model){
        System.out.println("表项层,查询所有账户...");
        // 调用service方法
        List<Account> list = accountService.findAll();
        model.addAttribute("list",list);
        return "list";
    }
}
```
3. 跳转目的页面 list.jsp

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" isELIgnored="false" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>查询所有</title>
</head>
<body>
<h3>已经查询了所有账户信息</h3>

<%--遍历结果集--%>
<c:forEach items="${list}" var="account">
    ${account.name}
</c:forEach>
</body>
</html>

```



## <font color=#009C41> 六、总结概览，整合的思路</font>
最后运行结果，在页面上点击连接首先传到 UserConteoller类中，执行里面的代码。
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720132620.png)



![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706104613.png)