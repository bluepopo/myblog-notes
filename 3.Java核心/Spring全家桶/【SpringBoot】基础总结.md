【SpringBoot】基础总结

# 一、SpringBoot简介

## 1.1  原有Spring优缺点分析

**1.1.1 Spring的优点分析**

Spring是Java企业版（Java Enterprise Edition，JEE，也称J2EE）的轻量级代替品。无需开发重量级的Enterprise JavaBean（EJB），Spring为企业级Java开发提供了一种相对简单的方法，通过依赖注入和面向切面编程，用简单的Java对象（Plain Old Java Object，POJO）实现了EJB的功能。

**1.1.2 Spring的缺点分析**

虽然Spring的组件代码是轻量级的，但它的配置却是重量级的。一开始，Spring用XML配置，而且是很多XML配置。Spring 2.5引入了基于注解的组件扫描，这消除了大量针对应用程序自身组件的显式XML配置。Spring 3.0引入了基于Java的配置，这是一种类型安全的可重构配置方式，可以代替XML。

所有这些配置都代表了开发时的损耗。因为在思考Spring特性配置和解决业务问题之间需要进行思维切换，所以编写配置挤占了编写应用程序逻辑的时间。和所有框架一样，Spring实用，但与此同时它要求的回报也不少。

除此之外，项目的依赖管理也是一件耗时耗力的事情。在环境搭建时，需要分析要导入哪些库的坐标，而且还需要分析导入与之有依赖关系的其他库的坐标，一旦选错了依赖的版本，随之而来的不兼容问题就会严重阻碍项目的开发进度。

## 1.2 SpringBoot的概述

### 1.2.1 SpringBoot解决上述Spring的缺点

SpringBoot对上述Spring的缺点进行的改善和优化，基于约定优于配置的思想，可以让开发人员不必在配置与逻辑业务之间进行思维的切换，全身心的投入到逻辑业务的代码编写中，从而大大提高了开发的效率，一定程度上缩短了项目周期。

### 1.2.2 SpringBoot的特点

- 为基于Spring的开发提供更快的入门体验
- 开箱即用，没有代码生成，也无需XML配置。同时也可以修改默认值来满足特定的需求
- 提供了一些大型项目中常见的非功能性特性，如嵌入式服务器、安全、指标，健康检测、外部配置等
- SpringBoot不是对Spring功能上的增强，而是提供了一种快速使用Spring的方式

### 1.2.3 SpringBoot的核心功能

- 起步依赖

  起步依赖本质上是一个Maven项目对象模型（Project Object Model，POM），定义了对其他库的传递依赖，这些东西加在一起即支持某项功能。

  简单的说，起步依赖就是将具备某种功能的坐标打包到一起，并提供一些默认的功能。

- 自动配置

  Spring Boot的自动配置是一个运行时（更准确地说，是应用程序启动时）的过程，考虑了众多因素，才决定Spring配置应该用哪个，不该用哪个。该过程是Spring自动完成的。




	注意：起步依赖和自动配置的原理剖析会在第三章《SpringBoot原理分析》进行详细讲解



----
# 二、SpringBoot快速入门

## 2.1 代码实现

### 2.1.1 创建Maven工程
### 2.1.2 添加SpringBoot的起步依赖
SpringBoot要求，项目要继承SpringBoot的起步依赖spring-boot-starter-parent
SpringBoot要集成SpringMVC进行Controller的开发，所以项目要导入web的启动依赖
pom.xml
```xml
	<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.1.RELEASE</version>
    </parent>

    <dependencies>
<!--        web功能的起步依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!--热部署配置-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
        </dependency>
    </dependencies>
```
### 2.1.3 编写SpringBoot引导类

要通过SpringBoot提供的引导类起步SpringBoot才可以进行访问

```java
//声明该类是一个SpringBoot引导类
@SpringBootApplication
public class MySpringBootApplication {

    public static void main(String[] args) {
        //run方法：表示运行SpringBoot的引导类
        SpringApplication.run(MySpringBootApplication.class);
    }
}
```


### 2.1.4 编写Controller

在引导类MySpringBootApplication同级包或者子级包中创建QuickStartController

```java
@Controller
public class StartController {

    @RequestMapping("/start")
    @ResponseBody
    public String start(){
        return "hello springboot";
    }

}
```
## 2.2 使用idea快速创建SpringBoot项目

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200707214421.png" alt="在这里插入图片描述" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200707214427.png" alt="在这里插入图片描述" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200707214434.png" alt="在这里插入图片描述" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200707214440.png" alt="在这里插入图片描述" style="zoom:100%;" />





# 

# 四、SpringBoot的配置文件

## 4.1 SpringBoot配置文件类型


SpringBoot是基于约定的，所以很多配置都有默认值，但如果想使用自己的配置替换默认配置的话，就可以使用`application.properties`或者`application.yml（application.yaml）`进行配置。

> SpringBoot默认会从Resources目录下加载`application.properties`或`application.yml（application.yaml）`文件
>

其中，application.properties文件是键值对类型的文件，之前一直在使用，所以此处不在对properties文件的格式进行阐述。除了properties文件外，SpringBoot还可以使用yml文件进行配置，下面对yml文件进行讲解。

**YML文件格式**是YAML (YAML Aint Markup Language)编写的文件格式，YAML是一种直观的能够被电脑识别的的数据数据序列化格式，并且容易被人类阅读，容易和脚本语言交互的，可以被支持YAML库的不同的编程语言程序导入，比如： C/C++, Ruby, Python, Java, Perl, C#, PHP等。YML文件是以数据为核心的，比传统的xml方式更加简洁。

YML文件的扩展名可以使用.yml或者.yaml。

示例如下：

```yaml
server:
  port: 8083

#对象的配置
person:
  name: zhangsan
  age: 18
  addr: beijing

#行内对象配置
person2: {name: zhangsan,age: 18,addr: beijing}

#数组的配置
city:
  - beijing
  - tianjing
  - shanghai
  - guangzhou

citys: [beijing,tianjing,shanghai,guangzhou]

#数组内容是对象
students:
  - name: tom
    age: 18
    addr: beijing
  - name: lucy
    age: 17
    addr: shanghai

student: [{name: tome,age: 18,addr: beijing},{name: lucy,age: 17,addr: shanghai}]


```
- 注意：value1与之间的 - 之间存在一个空格

## 4.2 配置文件与配置类的属性映射方式
### 4.2.1 使用注解@Value映射
我们可以通过@Value注解将配置文件中的值映射到一个Spring管理的Bean的字段上

> 类似于 @autowired 与 XML文件中的Bean对象

```java
@Controller
public class StartController {
    @Value("${person.name}")
    private String name;
    @Value("${person.addr}")
    private String addr;
    
    @RequestMapping("/start3")
    @ResponseBody
    public String start3(){
        return "name:"+name+","+"addr:"+addr;
    }

}
```
### 4.2.2 使用注解@ConfigurationProperties映射

```java
@Controller
@ConfigurationProperties(prefix = "person")
public class Start2Controller {

    private String name;
    private String age;
    private String addr;

    @RequestMapping(value = "/start4")
    @ResponseBody
    public String start4(){
        return "name:"+name+","+"addr:"+addr+","+"age:"+age;
    }
    public void setName(String name) {
        this.name = name;
    }

    public void setAge(String age) {
        this.age = age;
    }

    public void setAddr(String addr) {
        this.addr = addr;
    }
}

```
**注意**：使用`@ConfigurationProperties`方式可以进行配置文件与实体字段的自动映射，但需要字段必须`提供set方法`才可以，而使用 @Value注解修饰的字段不需要提供set方法

**注意** 在使用` @ConfigurationProperties `注解之前要先引入maven依赖，引入之后程序就会自动扫描到 yml配置文件里配置过的值并给出单词提示。

```xml
<!--        @ConfigurationProperties注解的执行器-->
        <dependency> 
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
```





# 四、整合其他

## <font color=#F39C12>💛 5.1 SpringBoot整合Mybatis</font>

### 5.1.1 添加Mybatis的起步依赖

```xml
<!--mybatis起步依赖-->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.1.1</version>
</dependency>
```

### 5.1.2 添加数据库驱动坐标

```xml
<!-- MySQL连接驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```

### 5.1.3 添加数据库连接信息

在application.properties中添加数据量的连接信息

```properties
#数据库连接信息:
spring.datasource.driverClassName=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/ssh?serverTimezone=UTC&useSSL=false
spring.datasource.username=root
spring.datasource.password=root
```

注：解决数据库驱动报错问题
[`Loading class com.mysql.jdbc.Driver'. This is deprecated. The new driver class is com.mysql.cj.jdb`
https://blog.csdn.net/weixin_42323802/article/details/82500458](https://blog.csdn.net/weixin_42323802/article/details/82500458)
### 5.1.4 创建user表

在test数据库中创建user表

```sql
-- ----------------------------
-- Table structure for `user`
-- ----------------------------
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `username` varchar(50) DEFAULT NULL,
  `password` varchar(50) DEFAULT NULL,
  `name` varchar(50) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=10 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of user
-- ----------------------------
INSERT INTO `user` VALUES ('1', 'zhangsan', '123', '张三');
INSERT INTO `user` VALUES ('2', 'lisi', '123', '李四');
```

### 5.1.5 创建实体Bean

```java
public class User {
    // 主键
    private Long id;
    // 用户名
    private String username;
    // 密码
    private String password;
    // 姓名
    private String name;
  
    //此处省略getter和setter方法 .. ..
    
}
```

### 5.1.6 编写Mapper

```java
@Mapper
public interface UserMapper {
	public List<User> queryUserList();
}
```

注意：@Mapper标记该类是一个mybatis的mapper接口，可以被spring boot自动扫描到spring上下文中

### 5.1.7 配置Mapper映射文件

在src\main\resources\mapper路径下加入UserMapper.xml配置文件"

```xml
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.itheima.mapper.UserMapper">
    <select id="queryUserList" resultType="user">
        select * from user
    </select>
</mapper>
```

### 5.1.8 在application.properties中添加mybatis的信息

```properties
#spring集成Mybatis环境
#pojo别名扫描包
mybatis.type-aliases-package=com.itheima.domain
#加载Mybatis映射文件
mybatis.mapper-locations=classpath:mapper/*Mapper.xml
```

### 5.1.9 编写测试Controller

```java
@Controller
public class MapperController {

    @Autowired
    private UserMapper userMapper;

    @RequestMapping("/queryUser")
    @ResponseBody
    public List<User> queryUser(){
        List<User> users = userMapper.queryUserList();
        return users;
    }

}
```

### 5.1.10 测试

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200707214412.png" alt="在这里插入图片描述" style="zoom:100%;" />

## <font color=#F39C12>💛 5.2 SpringBoot整合Junit</font>

### 5.2.1 添加Junit的起步依赖

```xml
<!--测试的起步依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
</dependency>
```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200707214404.png" alt="在这里插入图片描述" style="zoom:100%;" />
### 5.2.2 编写测试类

```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = Springboot02MybatisApplication.class)//SpringBoot的引导类
public class MybatisTest {

    @Autowired
    private UserMapper userMapper;

    @Test
    public void test01(){
        List<User> users = userMapper.queryUserList();
        System.out.println(users);

    }
}
```
其中，

SpringRunner继承自`SpringJUnit4ClassRunner`，使用哪一个Spring提供的测试测试引擎都可以

```java
public final class SpringRunner extends SpringJUnit4ClassRunner 
```

@SpringBootTest的属性指定的是引导类的字节码对象


##  <font color=#F39C12>5.3 SpringBoot整合Spring Data JPA</font>

### 5.3.1 添加Spring Data JPA的起步依赖

```xml
<!-- springBoot JPA的起步依赖 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

### 5.3.2 添加数据库驱动依赖

```xml
<!-- MySQL连接驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```

### 5.3.3 在application.properties中配置数据库和jpa的相关属性

```properties
#DB Configuration:
spring.datasource.driverClassName=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/test?useUnicode=true&characterEncoding=utf8
spring.datasource.username=root
spring.datasource.password=root

#JPA Configuration:
spring.jpa.database=MySQL
spring.jpa.show-sql=true
spring.jpa.generate-ddl=true
spring.jpa.hibernate.ddl-auto=update
spring.jpa.hibernate.naming_strategy=org.hibernate.cfg.ImprovedNamingStrategy
```

### 5.3.4 创建实体配置实体

```java
@Entity
public class User {
    // 主键
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    // 用户名
    private String username;
    // 密码
    private String password;
    // 姓名
    private String name;
 
    //此处省略setter和getter方法... ...
}
```

### 5.3.5 编写UserRepository

```java
public interface UserRepository extends JpaRepository<User,Long>{
    public List<User> findAll();
}
```

### 5.3.6 编写测试类

```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes=MySpringBootApplication.class)//SpringBoot的引导类
public class JpaTest {

    @Autowired
    private UserRepository userRepository;

    @Test
    public void test(){
        List<User> users = userRepository.findAll();
        System.out.println(users);
    }

}
```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200707214358.png" alt="在这里插入图片描述" style="zoom:100%;" />


注：jdk9会出现报错，需要导入没有集成的包
```xml
<!--jdk9需要导入如下坐标-->
<dependency>
    <groupId>javax.xml.bind</groupId>
    <artifactId>jaxb-api</artifactId>
    <version>2.3.0</version>
</dependency>
```

## <font color=#F39C12>5.4 SpringBoot整合Redis</font>

### 5.4.1 添加redis的起步依赖

```xml
<!-- 配置使用redis启动器 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

### 5.4.2 配置redis的连接信息

```properties
#Redis
spring.redis.host=127.0.0.1
spring.redis.port=6379
```

### 5.4.3 注入RedisTemplate测试redis操作

```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = SpringbootJpaApplication.class)
public class RedisTest {

    @Autowired
    private UserRepository userRepository;

    @Autowired
    private RedisTemplate<String, String> redisTemplate;

    @Test
    public void test() throws JsonProcessingException {
        //从redis缓存中获得指定的数据
        String userListData = redisTemplate.boundValueOps("user.findAll").get();
        //如果redis中没有数据的话
        if(null==userListData){
            //查询数据库获得数据
            List<User> all = userRepository.findAll();
            //转换成json格式字符串
            ObjectMapper om = new ObjectMapper();
            userListData = om.writeValueAsString(all);
            //将数据存储到redis中，下次在查询直接从redis中获得数据，不用在查询数据库
            redisTemplate.boundValueOps("user.findAll").set(userListData);
            System.out.println("===============从数据库获得数据===============");
        }else{
            System.out.println("===============从redis缓存中获得数据===============");
        }

        System.out.println(userListData);

    }

}
```