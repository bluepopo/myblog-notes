【Mybatis 】基础总结

# （一）入门案例

## 1. Mybatis概述

mybatis是一个优秀的基于 java 的持久层框架，它内部封装了 jdbc，使开发者只需要关注 sql 语句本身，
而不需要花费精力去处理加载驱动、创建连接、创建 statement 等繁杂的过程。
mybatis通过 xml 或注解的方式将要执行的各种statement配置起来，并通过java对象和statement 中
sql 的动态参数进行映射生成最终执行的 sql 语句，最后由 mybatis 框架执行 sql 并将结果映射为 java 对象并
返回。

![MVC三层架构](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705212727.png) 

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

------

## 2. Mybatis环境搭建

一、 创建 maven  工程

二、在 pom.xml 文件中添加相关依赖

注意需要导入打印日志的配置文件需要配置log4j.properties，

细节：

​     第一步：创建maven工程并导入坐标
​     第二步：创建实体类和dao的接口
​     第三步：创建Mybatis的主配置文件
​         SqlMapConifg.xml
​     第四步：创建映射配置文件
​         IUserDao.xml
   环境搭建的注意事项：
​     第一个：创建IUserDao.xml 和 IUserDao.java时名称是为了和我们之前的知识保持一致。
​       在Mybatis中它把持久层的操作接口名称和映射文件也叫做：Mapper
​       所以：IUserDao 和 IUserMapper是一样的
​     第二个：在idea中创建目录的时候，它和包是不一样的
​       包在创建时：com.itheima.dao它是三级结构
​       目录在创建时：com.itheima.dao是一级目录
​     **第三个：mybatis的映射配置文件位置必须和dao接口的包结构相同**
​     第四个：映射配置文件的mapper标签namespace属性的取值必须是dao接口的全限定类名
​     第五个：映射配置文件的操作配置（select），id属性的取值必须是dao接口的方法名

​     当我们遵从了第三，四，五点之后，我们在开发中就无须再写dao的实现类。







```html
    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.4.5</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>

        <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13-beta-3</version>
            <scope>test</scope>
        </dependency>

    </dependencies>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

三、User  实体类：对应数据库中的字段类型

四、编写 持久层 接口 IUserDao：在接口中声明findAll 操作数据库的方法

五、编写两个重要配置文件 ：

configuration.xml (用于得到connection等与数据库做连接)



```html
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--mybaits的主配置文件-->
<configuration>
    <!--配置环境-->
    <environments default="mysql">
        <!--配置mysql环境-->
        <environment id="mysql">
            <!--配置事务类型-->
            <transactionManager type="JDBC"></transactionManager>
            <!--配置数据源（连接池）-->
            <dataSource type="POOLED">
                <!--配置数据源（连接池）的基本信息-->
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>

            </dataSource>
        </environment>
    </environments>

    <!--指定映射配置文件的位置，映射配置文件指的是每个dao配置的文件-->
    <mappers>
        <mapper resource="code/dao/IUserDao.xml"/> <!--   使用的是xml-->
<!--        <mapper class="code.dao.IUserDao"/>    &lt;!&ndash;使用的是注解 annotations&ndash;&gt;-->
    </mappers>

</configuration>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)





IUserDao.xml(映射相关的配置文件)

```html
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace指的是你的dao接口-->
<mapper namespace="code.dao.IUserDao">
    <!--配置查询所有  其中id不能乱写必须是dao接口中的方法  resultType写的是实体类的全路径-->
    <select id="findAll" resultType="code.domain.User">
        select * from user
    </select>
</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



六、编写测试类使用Mybatis

```java
public class MybatisTest {
/**
 * 测试查询所有操作
 * */
public static void main(String[] args) throws IOException {
    //1.读取配置文件
    InputStream in= Resources.getResourceAsStream("SqlMapConfig.xml");


    //2.创建SqlSessionFactory工厂
    SqlSessionFactoryBuilder builder=new SqlSessionFactoryBuilder();
    SqlSessionFactory factory = builder.build(in);//配置文件作为构建工厂的参数路由
    //3.使用工厂生产sqlSession对象
    SqlSession session = factory.openSession();
    //4.使用SqlSession创建Dao接口的代理对象
    //在这里userdao是代理对象，而不需要像以前一样再去创建impl对象
    IUserDao userDao = session.getMapper(IUserDao.class);

    //5.使用代理对象增强方法，在不改变接口方法的基础上实现方法
    List<User> users = userDao.findAll();
    for (User user:users){
        System.out.println(user);
    }

    //6.释放资源
    session.close();
    in.close();

}

}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**测试类执行Dao操作的步骤如下**

​    第一步：读取配置文件
​     第二步：创建SqlSessionFactory工厂
​     第三步：创建SqlSession
​     第四步：创建Dao接口的代理对象
​     第五步：执行dao中的方法
​     第六步：释放资源

![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705220455.png)

![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705220732.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH





##  3. 执行原理解析

### 3.1 几个对象的构建

创建SqlSessionFactory对象：使用了构建者模式，把创建对象的内部细节隐藏，使用者直接调用方法就可以得到想要的对象。

创建SqlSession对象：使用了工厂模式，解耦，降低了类之间的依赖性

创建IUserDao接口的实现类对象：使用了代理模式，即使不创建实现类也可以实现接口中的方法，并可以做方法的增强功能。



### 3.2 xml配置文件与sqlsession合作

在IUserDao.xml映射文件中，id就是接口中的方法名，resultType就是返回结果的封装实体类（在这里必须指定全限定类名）

如图，蓝色字体部分就是传统的我们使用JDBC时的步骤。

通过SQL语句查询到了resultSet结果集，将结果集封装成对象时，我们必须知道类名。

如下图，这就是XML配置文件的组装原理。



![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705220937.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



### 3.3 SqlSessionFactory、SqlSession

简书：https://www.jianshu.com/p/65b722e3f3a8 作者：[圣村的希望](https://www.jianshu.com/u/ebedc4011de2)

![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705220947.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)







## 4. 使用注解方式配置



![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705220956.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



# （二）CRUD操作



## 1. 基于代理 Dao  实现 CRUD  操作

使用要求：
1、持久层接口和持久层接口的映射配置必须在相同的包下
2、持久层映射配置中 mapper 标签的 namespace 属性取值必须是持久层接口的全限定类名
3、SQL 语句的配置标签<select>,<insert>,<delete>,<update>的 id 属性必须和持久层接口的方法名相同。

![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720131724.png)

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)





### 1.2  代码：User实体类

```java
package code.domain;

public class User {
private String username;
private  String password;

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    @Override
    public String toString() {
        return "User{" +
                "username='" + username + '\'' +
                ", password='" + password + '\'' +
                '}';
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 1.3  代码：IUserDao.xml中配置sql语句

```html
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace指的是你的dao接口-->
<mapper namespace="code.dao.IUserDao">
    <!--配置查询所有  其中id不能乱写必须是dao接口中的方法  resultType写的是实体类的全路径-->
    <select id="findAll" resultType="code.domain.User">
        select * from user
    </select>


    <insert id="saveUser" parameterType="code.domain.User">
        insert into user(username,password)values (#{username},#{password})
    </insert>


    <update id="updataUser"  parameterType="code.domain.User">
        update user set password=#{password} where username=#{username};
    </update>

    <delete id="deleteUser" parameterType="string">
       delete from  user where username=#{value};
    </delete>

    <!--模糊查询用户 使用${value} 表示取String参数的值-->
    <select id="findByName" parameterType="string" resultType="code.domain.User">
        select * from user where username like '%${value}%';
    </select>

    <select id="findTotal" resultType="int">
        select count(*) from user ;
    </select>


</mapper>
 
 
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 1.4 Test测试类执行sql语句

```java
/**
 *Mybatis初级测试
 * */
public class MybatisTest {
/**
 * 测试查询所有操作
 * */
    InputStream in;
    SqlSessionFactory factory;
    SqlSession sqlSession;
    IUserDao userDao;

    @Before
    public void init() throws IOException {
        //1.读取配置文件
         in= Resources.getResourceAsStream("SqlMapConfig.xml");
        //2.创建SqlSessionFactory工厂
        factory=new SqlSessionFactoryBuilder().build(in);
        //3.使用工厂生产sqlSession对象
        sqlSession = factory.openSession();
        //4.使用SqlSession创建Dao接口的代理对象
        //在这里userdao是代理对象，而不需要像以前一样再去创建impl对象
         userDao = sqlSession.getMapper(IUserDao.class);

    }

    @After
    public void destroy() throws IOException {
        //6.释放资源
        sqlSession.close();
        in.close();
    }
/**
 * 查询所有
 * */
    @Test
    public void testfindAll(){
        //5.使用代理对象增强方法，在不改变接口方法的基础上实现方法
        List<User> users = userDao.findAll();
        for (User user:users){
            System.out.println(user);
        }
    }

    /**
     * 添加用户
     * */
    @Test
    public  void  testsaveUser(){
        User user=new User();
        user.setUsername("王涵");
        user.setPassword("wanghan");
        userDao.saveUser(user);
        sqlSession.commit();

    }

/**
 * 更新一条用户的记录*/
    @Test
    public  void  testupdateUser(){
        User user=new User();
        user.setUsername("林青霞");
        user.setPassword("linqingxai");
        userDao.updataUser(user);
        sqlSession.commit();

    }

    /**
     * 删除一条记录
     * */
    @Test
    public  void  testdeleteUser(){
       userDao.deleteUser("李彤");
        sqlSession.commit();

    }

/**
 * 模糊查询
 * */
    @Test
    public  void  testfindByNamer(){
        List<User> users = userDao.findByName("霞");
//        sqlSession.commit();
        for (User user:users){
            System.out.println(user);
        }

    }

    /**
     * 使用聚合函数
     * */
    @Test
    public  void  testfindTotal(){
        int total = userDao.findTotal();

        System.out.println(total);
    }

/**
 * 参数的深入：使用POJO类作为参数
 * */
    @Test
    public  void  testfindByVo(){
        QueryVo vo=new QueryVo();
        User user=new User();
        user.setUsername("%王%");
        vo.setUser(user);

        List<User> users = userDao.findByVo(vo);
        for (User u:users){
            System.out.println(u);
        }
    }



    /**
     * 当User2中的成员变量名称与数据库中的不一致时，查询所有
     * */
    @Test
    public void testfindAll2(){
        //5.使用代理对象增强方法，在不改变接口方法的基础上实现方法
        List<User2> users2 = userDao.findAll2();
        for (User2 user2:users2){
            System.out.println(user2);
        }
    }


}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



## 2. Mybatis  的参数深入

### 2.1 parameterType注意事项

1. **基 本类型**和 String 我 们可 以直接 写类型 名称 ，也 可以 使用包 名 . 类名的 方式 ，例如 ：java.lang.String，也可以直接使用String。
2. **实体类类型**，目前我们只能使用全限定类名
3. 传递 **pojo  包装对象，参数深入**



如：

1.编写 QueryVo

```java
package code.domain;

import java.io.Serializable;

public class QueryVo implements Serializable {
    private User user;
    public User getUser() {
        return user;
    }
    public void setUser(User user) {
        this.user = user;
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

2.编写持久层接口

```java
/**
 * 根据 QueryVo 中的条件查询用户
 * @param vo
 * @return
 */
List<User> findByVo(QueryVo vo);
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

3.持久层接口的映射 文件

```xml
<select id="findByVo" resultType="code.domain.User" parameterType="code.domain.QueryVo">
    select * from user where username like #{user.username};
</select>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

4.测试包装类作为参数

```java
/**
 * 参数的深入：使用POJO类作为参数
 * */
    @Test
    public  void  testfindByVo(){
        QueryVo vo=new QueryVo();
        User user=new User();
        user.setUsername("%王%");
        vo.setUser(user);

        List<User> users = userDao.findByVo(vo);
        for (User u:users){
            System.out.println(u);
        }
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 2.2 resultType注意事项：

当domain实体类与数据库中的字段值不一致时怎么办？

1.采用取别名方法

2.resultMap  设置映射结果类型



![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705232126.png)

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



 **第一种方法：**

起别名方法  运行起来比较快

![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705232119.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```xml
使用别名查询
<!-- 配置查询所有操作 -->
<select id="findAll2" resultType="code.domain.User2">
      select username as name ,password as psw from user;
</select>
 
```



**第二种方法**

**resultMap** 采用配置的方式：配置查询结果的列名和类的属性名 映射对应关系



**resultMap** 标签可以建立查询的列名和实体类的属性名称不一致时建立对应关系。从而实现封装。
在 select 标签中使用 resultMap 属性指定引用即可。同时 resultMap 可以实现将查询结果映射为复杂类
型的 pojo，比如在查询结果映射对象中包括 pojo 和 list 实现一对一查询和一对多查询。



```java
public class User2 {
    private String name;
    private  String psw;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getPsw() {
        return psw;
    }

    public void setPsw(String psw) {
        this.psw = psw;
    }

    @Override
    public String toString() {
        return "User2{" +
                "name='" + name + '\'' +
                ", psw='" + psw + '\'' +
                '}';
    }
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

使用resultMap设置对应关系

```html
<!--    配置 查询结果的列名和实体类的属性名的对应关系-->
   <resultMap id="UserMap" type="code.domain.User2">
<!--        主键字段的对应-->
<!--       <id property="userId" column="uid"></id>-->
<!--       非主键字段的对应-->
       <result property="name" column="username"></result>
       <result property="psw" column="password"></result>


   </resultMap>
    <select id="findAll2" resultMap="UserMap">
          select * from user;
    </select>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705232112.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)





# （三）连接池与多表操作

## 1.连接池以及事务控制

### 1.1 mybatis中连接池使用及分析

**1、连接池：**
  我们在实际开发中都会使用连接池。
  因为它可以减少我们获取连接所消耗的时间。
**2、mybatis中的连接池**
  mybatis连接池提供了3种方式的配置：
  配置的位置：
     主配置文件SqlMapConfig.xml中的dataSource标签，type属性就是表示采用何种连接池方式。

​     type属性可以取： POOLED、UNPOOLED、JNDI  

![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705232107.png)

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



   type属性的取值意义：

> ​     POOLED  采用传统的 javax.sql.DataSource 规范中的连接池，mybatis中有针对规范的实现
> ​    UNPOOLED 采用传统的获取连接的方式，虽然也实现 Javax.sql.DataSource 接口，但是并没有使用池的思想。
> ​    JNDI  采用服务器提供的JNDI技术实现，来获取DataSource对象，不同的服务器所能拿到DataSource是不一样。
>
> ​       注意：如果不是web或者maven的war工程，是不能使用JNDI  的。
> ​       课程中使用的是tomcat服务器，采用连接池就是dbcp连接池。





MyBatis 在初始化时，根据<dataSource>的 type 属性来创建相应类型的的数据源 DataSource，即：

- type=”POOLED”：MyBatis 会创建 PooledDataSource 实例
- type=”UNPOOLED” ： MyBatis 会创建 UnpooledDataSource 实例
- type=”JNDI”：MyBatis 会从 JNDI 服务上查找 DataSource 实例，然后返回使用

 



### 1.2 mybatis事务控制的分析

![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705222901.png)

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705222912.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)





## 2.mybatis基于XML配置的动态SQL语句使用

### 2.1 mapper配置文件中的几个标签

#### if

```xml
<select id="findByCondition" resultType="user"> 
    select * from users where 1=1               
    <if test="username != null">                
    and username = #{username}                  
    </if>                                       
 	<if test="sex != null">                      
	and sex = #{sex}                             
	</if>                                        
</select>    
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

#### where

![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705223204.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



#### foreach



![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705223158.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)





## 3.mybatis中的多表操作

#### 一对一

方法一：通过建立**子类AccountUser**的方式，实现多表连接查询

![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705232056.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



方法二：常用方式：一对一的查询



IAccountDao.xml文件中的配置如下：从表中建立主表User的映射配置

```xml
<!--    定义封装account和user的resultMap-->
    <resultMap id="accountUserMap" type="code.domain.Account">
        <id property="id" column="aid"></id>
        <result property="uid" column="uid"></result>
        <result property="money" column="money"></result>
<!--   一对一的关系映射：配置封装user的内容-->
   <association property="user" column="uid" javaType="user">
       <id property="username" column="username"></id>
       <result property="address" column="address"></result>
       <result property="sex" column="sex"></result>
       <result property="birthday" column="birthday"></result>
   </association>                                         
    </resultMap>                                         
                                                              
    <!--配置查询所有  其中id不能乱写必须是dao接口中的方法  resultType写的是实体类的全路径-->                                        
    <select id="findAll" resultMap="accountUserMap">
        select u.*,a.id as aid,a.uid,a.money
        from account a,user u    
        where u.id =a.uid;                             
    </select>               
```



注意： **javaType=“User”** 指名的是主体表的实体类名 。**column**中指名从表的外键

从Account中去查询User。一个账户对应一个用户

![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705224239.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



#### 一对多

一个用户User对应一组Account，主表User中建立从表Account的集合应用，并在IUserDao.xml中配置映射关系



![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705224249.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



#### 多对多

用户与角色的关系。多个用户对应多种角色，必须**借助中间表**实现多对多的映射



![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705224437.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



当我们查询用户时，可以同时得到用户所包含的角色信息

注意：collection标签属性中oftype值为映射到的引用对象Role类型（这里已经使用了别名）

![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705224620.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



当我们查询角色时，可以同时得到角色的所赋予的用户信息

![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705224634.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)





## 4.JNDI

**什么是JNDI?**

没有JNDI的做法存在的问题：
1、数据库服务器名称MyDBServer 、用户名和口令都可能需要改变，由此引发JDBC URL需要修改；
2、数据库可能改用别的产品，如改用DB2或者Oracle，引发JDBC驱动程序包和类名需要修改；
3、随着实际使用终端的增加，原配置的连接池参数可能需要调整；
4、......
解决办法：
程序员应该不需要关心“具体的数据库后台是什么？JDBC驱动程序是什么？JDBC URL格式是什么？访问数据库的用户名和口令是什么？”等等这些问题，程序员编写的程序应该没有对 JDBC 驱动程序的引用，没有服务器名称，没有用户名称或口令 —— 甚至没有数据库池或连接管理。而是把这些问题交给J2EE容器来配置和管理，程序员只需要对这些配置和管理进行引用即可。

由此，就有了JNDI。

 用了JNDI之后的做法：
首先，在在J2EE容器中配置JNDI参数，定义一个数据源，也就是JDBC引用参数，给这个数据源设置一个名称；然后，在程序中，通过数据源名称 引用 数据源 从而访问后台数据库。



**JNDI的环境配置修改 以及需要的配置文件**



![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705232046.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

### 问题：操作数据库的代码要放到tomcat中运行



![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705232038.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)







# （四） 延迟加载、缓存、注解开发

## 1.Mybatis中的延迟加载

### 1.1 介绍

**问题**：在一对多中，当我们有一个用户，它有100个账户。
     在查询用户的时候，要不要把关联的100个账户查出来？
     在查询账户的时候，要不要把关联的一个用户查出来？

     在查询用户时，用户下的账户信息应该是，什么时候使用，什么时候查询的。
     在查询账户时，账户的所属用户信息应该是随着账户查询时一起查询出来。



**什么是延迟加载**

​     在真正使用数据时才发起查询，不用的时候不查询。按需加载（懒加载）



**什么是立即加载**

​     不管用不用，只要一调用方法，马上发起查询。

**在对应的四种表关系中：一对多，多对一，一对一，多对多**
   一对多，多对多：通常情况下我们都是采用延迟加载。
   多对一，一对一：通常情况下我们都是采用立即加载。

**SqlMapConfig.xml文件的配置修改**



```html
 <configuration>
<!--    配置参数-->
    <settings>
        <setting name="lazyLoadingEnabled" value="true"/>
        <setting name="aggressiveLazyLoading" value="false"/>
    </settings>
</configuration>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



### 1.2 一对一关系实现延迟加载

实例：一个Account账户对应一个user用户，查询所有account账户时通过uid查询对应的user
步骤一：IAccountDao.xml文件的映射配置如下

注意：

<!--     select属性指定的内容：查询用户的唯一标识-->
<!--     column属性指定的内容：用户根据id查询时，所需要的参数的值-->

```html
<mapper namespace="code.dao.IAccountDao">
<!--    定义封装account和user的resultMap-->
    <resultMap id="accountUserMap" type="code.domain.Account">
        <id property="id" column="id"></id>
        <result property="uid" column="uid"></result>
        <result property="money" column="money"></result>
<!--    一对一的关系映射：配置封装user的内容-->
<!--        select属性指定的内容：查询用户的唯一标识-->
<!--        column属性指定的内容：用户根据id查询时，所需要的参数的值-->
        <association property="user" column="uid" javaType="user" select="code.dao.IUserDao.findById">
        </association>
    </resultMap>

    <!--配置查询所有  其中id不能乱写必须是dao接口中的方法  resultType写的是实体类的全路径-->
    <select id="findAll" resultMap="accountUserMap">
        select * from account
    </select>


</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

步骤二：IUserDao.xml文件映射配置如下，并提供findById的映射

```html
<mapper namespace="code.dao.IUserDao">
<!--    定义User的resultMap-->
    <resultMap id="userAccountMap" type="user">
<!--        配置User的映射-->
        <id property="id" column="id"></id>
        <result property="username" column="username"></result>
        <result property="address" column="address"></result>
        <result property="sex" column="sex"></result>
        <result property="birthday" column="birthday"></result>
<!--        配置user对象中account集合的映射-->
        <collection property="accounts" ofType="account">
        <id property="id" column="aid"></id>
        <result property="uid" column="uid"></result>
        <result property="money" column="money"></result>
        </collection>
    </resultMap>

    <!--配置查询所有  其中id不能乱写必须是dao接口中的方法  resultType写的是实体类的全路径-->
    <select id="findAll" resultMap="userAccountMap">
        select * from user u left outer join account a on u.id=a.uid
    </select>
    <select id="findById" parameterType="int" resultType="user">
    select * from user where id=#{uid}
    </select>


</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

步骤三：通过AccountTest.java测试查询所有Account方法，实现查询Account的同时查到对应的用户信息
查询结果对比如下

![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705232032.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)





### 1.3 一对多关系实现延迟加载

示例：一个user用户 对应多个Account账户

第一步：首先改造 IUserDao.java  **IUserDao.xml 文件**，改造 findAll 方法查询user表。

注意：collection标签中表示延迟查询每个user对应 的account。

其中select表示通过uid对account表的查询。

column表示select查询的依据参数，即 **uid。**

```html
<mapper namespace="code.dao.IUserDao">
<!--    定义User的resultMap-->
    <resultMap id="userAccountMap" type="user">
<!--        配置User的映射-->
        <id property="id" column="id"></id>
        <result property="username" column="username"></result>
        <result property="address" column="address"></result>
        <result property="sex" column="sex"></result>
        <result property="birthday" column="birthday"></result>
<!--        配置user对象中account集合的映射-->
        <collection property="accounts" ofType="account" column="id" select="code.dao.IAccountDao.findAccountByUid">
        </collection>
    </resultMap>

    <!--配置查询所有  其中id不能乱写必须是dao接口中的方法  resultType写的是实体类的全路径-->
    <select id="findAll" resultMap="userAccountMap">
        select * from user
    </select>
    <select id="findById" parameterType="int" resultType="user">
    select * from user where id=#{uid}
    </select>
    
</mapper>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)





第二步：改造 IAccountDao.java 和 **IAccountDao.xml文件。**

**添加方法映射如下**

```html
  <select id="findAccountByUid" parameterType="int" resultType="account">
        select * from account where uid = #{uid}
    </select>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

第三步：测试IUserTest的 findAll方法

结果对比如下

![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705232024.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



------

## 2.Mybatis中的缓存

**2.1 什么是缓存**

 存在于内存中的临时数据。

**2.2 为什么使用缓存**

减少和数据库的交互次数，提高执行效率。

**2.3 什么样的数据能使用缓存，什么样的数据不能**

适用于缓存：
       经常查询并且不经常改变的。
       数据的正确与否对最终结果影响不大的。
     不适用于缓存：
       经常改变的数据
       数据的正确与否对最终结果影响很大的。
       例如：商品的库存，银行的汇率，股市的牌价。

**2.4 mybatis中的一级缓存、二级缓存**



### 2.1 一级缓存：

​       它指的是Mybatis中SqlSession对象的缓存。
​       当我们执行查询之后，查询的结果会同时存入到SqlSession为我们提供一块区域中。
​       该区域的结构是一个Map。当我们再次查询同样的数据，mybatis会先去sqlsession中
​       查询是否有，有的话直接拿出来用。
​       当SqlSession对象消失时，mybatis的一级缓存也就消失了。

```java
      sqlSession.clearCache();清楚了一级缓存
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```java
 @Test
    public void testfindById()throws IOException{
        User user1 = userDao.findById(6);
        System.out.println(user1);

         sqlSession.clearCache();
        User user2 = userDao.findById(6);
        System.out.println(user2);

        System.out.println(user1==user2);  //false
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705232019.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)


**一级缓存是sqlSession范围的缓存，当调用SqlSeesion的 修改、添加、删除、清空缓存、关闭close()、提交commit()方法时，都会清空一级缓存。**
示例：触发清空一级缓存的清空：当更新缓存的同步时。
注意：如下代码，数据库的持久层数据中 id=6 的数据并未发生更新。**第二次查询时，并不会从缓存中获取，而是从数据库中直接查询获取。**

```java
/**
 * 测试缓存的同步
 * @throws IOException
 */
@Test
public void testClearCache()throws IOException{
    //1.根据id查询用户信息
    User user1 = userDao.findById(6);
    System.out.println(user1);
    //2.更新用户信息
    user1.setUsername("更新后");
    user1.setAddress("北京");
    userDao.updateUser(user1);
    //3.再次查询id=6的用户信息
    User user2 = userDao.findById(6);
    System.out.println(user2);
    System.out.println(user1==user2);//false

}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)





###        2.2 **二级缓存:**


       它指的是**Mybatis中SqlSessionFactory对象的缓存**。由同一个SqlSessionFactory对象创建的SqlSession共享其缓存。
       二级缓存的使用步骤：
         第一步：让Mybatis框架支持二级缓存（在SqlMapConfig.xml中配置）

```html
 <settings>
        <setting name="cacheEnabled" value="true"/>
    </settings>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)


​         第二步：让当前的映射文件支持二级缓存（在IUserDao.xml中配置）

```html
<mapper namespace="code.dao.IUserDao">

<!--    开启User支持二级缓存-->
    <cache/>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)


​         第三步：让当前的操作支持二级缓存（在select标签中配置） **useCache="true"**

```html
<select id="findById" parameterType="int" resultType="user" useCache="true">
         select * from user where id = #{id}
    </select>
```



测试类UserTest

```java
 @Test
    public  void testSecondCache(){
        SqlSession sqlSession1 = factory.openSession();
        IUserDao userDao1 = sqlSession1.getMapper(IUserDao.class);
        User user1 = userDao1.findById(5);
        System.out.println(user1);
        sqlSession1.clearCache();//一级缓存消失

        SqlSession sqlSession2 = factory.openSession();
        IUserDao userDao2= sqlSession1.getMapper(IUserDao.class);
        User user2= userDao1.findById(5);
        System.out.println(user2);
        sqlSession2.clearCache();//一级缓存消失

        System.out.println(user1==user2);//false
    }
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705225958.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**(user1==user2);//false的原因：虽然第二次查询并没有发起新的数据库操作，而是从二级缓存中获得，但是却重新创建了一个user对象用来接收查询到的数据，所以用“==”判断返回了 false。**





![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705230015.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



### 2.3  缓存的配置

XML方式：修改配置文件和注解配置，开启二级缓存

```html
<configuration>
    <settings>
        <setting name="cacheEnabled" value="true"/>
    </settings>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

注解方式：IUserDao接口中注解配置改造

```java
@CacheNamespace(blocking = true)
public interface IUserDao {
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



练习：编写二级缓存测试类SecondCacheTest.java

通过factory开启两个不同的SqlSession，生成两个不同的代理对象userDao1、userDao2

**通过两次查询。第一次查询通过数据库查询，第二次查询到的数据是通过二级缓存（factory中的缓存数据）得到，不需要再去查询数据库操作。**

```java
@Test
    public void testfindById(){
        SqlSession sqlSession1=factory.openSession();
        IUserDao userDao1 = sqlSession1.getMapper(IUserDao.class);
        User user1 = userDao1.findById(9);
        System.out.println(user1);
        sqlSession1.close(); //释放一级缓存

        SqlSession sqlSession2=factory.openSession(); //再次打开 sqlSession1
        IUserDao userDao2 = sqlSession2.getMapper(IUserDao.class);
        User user2 = userDao2.findById(9);
        System.out.println(user2);
        sqlSession2.close(); //释放一级缓存

        System.out.println(user1==user2);


}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)





![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705231538.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)





------

# （五）Mybatis中的注解开发

## 3.1 环境搭建

编写SqlMapConfig.xml配置文件，使用外部数据源配置

注意事项：注解和xml映射 两种方式不可以同时使用，在使用注解的同时要确保resources下没有相应的xml文件存在，防止发生矛盾出错。

```xml
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/mybatis
jdbc.username=root
jdbc.password=root
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```html
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
<!--    引入外部配置文件-->
<properties resource="jdbcConfig.properties"></properties>
<!--    配置别名-->
<typeAliases>
    <package name="code.domain"></package>
</typeAliases>
<!--    配置环境-->
<enviroments default="mysql">
    <enviroment>
        <transactionManager type="JDBC"></transactionManager>
        <dataSource type="POOLED">
            <property name="driver" value="${jdbc.driver}"></property>
            <property name="url" value="${jdbc.url}"></property>
            <property name="username" value="${jdbc.username}"></property>
            <property name="password" value="${ jdbc.password}"></property>
        </dataSource>
    </enviroment>
</enviroments>

<!--    指定带有注解的dao接口所在位置-->
<mappers>
    <package name="code.dao"></package>
</mappers>
</configuration>
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

```java
package code.dao;
import code.domain.User;
import org.apache.ibatis.annotations.Select;
import java.util.List;

/**
 *
 *mybatis中针对，CRUD一共有四个注解
 * @Select @Insert @Update @Delete
 */
public interface IUserDao {
    /**
     * 查询所有
     * @return
     */
    @Select("select * from user")
    List<User> findAll();

    
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

test测试文件代码和以往相同





## 3.2 单表CRUD操作（代理Dao方式）

代理Dao方式 ，即不需要我们再手动去写实现类了，这里都用数据源配置文件和注解SQL帮我们实现了底层。

```java
public interface IUserDao {

    @Select("select * from user")
    List<User> findAll();

    @Insert("insert into user(id,username,birthday,sex,address) values(#{id},#{username},#{birthday},#{sex},#{address})")
    void saveUser(User user);

    @Update("update user set username=#{username},address=#{address} where id=#{id}")
    void updateUser(User user);

    @Delete("delete from user where id=#{id}")
    void deleteUser(Integer id);

}
/**
     * 根据 id 查询用户
     */
    @Select("select * from user where id=#{id}")
    User findById(Integer id);

    /**
     * 根据名称查询用户
     */
    @Select("select * from user where username like '%${value}%'")
    List<User> findByUsername(String username);


    /**
     *查询总记录条数
     */
    @Select("select count(*) from user")
    int findTotal();
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



## 3.3 列名和属性名映射

User类

```java
public class User implements Serializable {
    private  Integer userId;
    private  String userName;
    private Date userBirthday;
    private  String userSex;
    private String userAddress;
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



```java
/**
	* IUserDao接口：查询所有 方法
	* 类似于xml文件的resultMap映射关系，将数据库表中的 列名 column 和 对象属性 property 对应起来。
*/

@Select("select * from user")
   @Results(id = "userMap",value = {
           @Result(id = true,column = "id",property = "userId"),
           @Result(column = "username",property = "userName"),
           @Result(column = "birthday",property = "userBirthday"),
           @Result(column = "sex",property = "userSex"),
           @Result(column = "address",property = "userAddress")
   }
   )
    List<User> findAll();


 /**
     * 根据 id 查询用户
     */
    @Select("select * from user where id=#{id}")
    @ResultMap(value = "userMap")
    User findById(Integer id);

    /**
     * 根据名称查询用户
     */
    @Select("select * from user where username like #{username}")
    @ResultMap("userMap")
//    @Select("select * from user where username like '%${value}%'") 
    List<User> findByUsername(String username);
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)







## 3.4 多表查询操作

（1）一对一：一个账户对应一个用户

Account类

```java
public class Account implements Serializable {
    private Integer id;
    private Integer uid;
    private  Double money;

    //多对一，（Mybatis中称为一对一），一个账户只能对应一个用户
    private  User user;
```



IAccountDao接口

```java
public interface IAccountDao {

    /**
     * 查询所有账户，并且和所属用户信息
     */
    @Select("select * from account")
    @Results(value = {
            @Result(id = true,property = "id",column = "id"),
            @Result(property = "uid",column = "uid"),
            @Result(property = "money",column = "money"),
            @Result(property = "user" ,column = "uid",one = @One(select = "code.dao.IUserDao.findById",fetchType = FetchType.EAGER))//立即加载

    })
    List<Account> findAll();
}
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

注意： 

> ```java
> @Result(property = "user" ,column = "uid",one = @One(select = 
> "code.dao.IUserDao.findById",fetchType =FetchType.EAGER))
> ```

> 其中 column表示查询**User表的依据是uid，也是Account表的外键**
>
> ​     **@One** 注解，表示对应一个User结果**，@Many** 表示对应多个结果
>
> ​     select 中填写查找User表的依据
>
> ​     FetchType.EAGER 表示立即加载， LAZY表示延迟加载

多表查询结果：

![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705231547.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)





（2）一对多：

示例：一个用户User 可以对应多个 Account账户

User类

```java
public class User implements Serializable {
    private  Integer userId;
    private  String userName;
    private Date userBirthday;
    private  String userSex;
    private String userAddress;

    //一对多，一个用户对应多个账户
    private  List<Account> accounts;
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

IUserDao接口

```java
 @Select("select * from user")
   @Results(id = "userMap",value = {
           @Result(id = true,column = "id",property = "userId"),
           @Result(column = "username",property = "userName"),
           @Result(column = "birthday",property = "userBirthday"),
           @Result(column = "sex",property = "userSex"),
           @Result(column = "address",property = "userAddress"),
           @Result(property = "accounts",column = "id",many =
           @Many(select = "code.dao.IAccountDao.findAccountByUid",fetchType = FetchType.LAZY))}
   )
    List<User> findAll();
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

IAccountDao接口，select = "code.dao.IAccountDao.findAccountByUid",方法

```java
  @Select("select * from account where uid = #{userId}")
    Account findAccountByUid(Integer userId);
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



执行结果

![img](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705231554.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



（3）多对多





待补充



# （六）练习：使用注解进行SQL查询

## 6.1 环境

- 挂载于VMware虚拟机下的远程Oracle数据库
- product 产品表
- 简单查询 findAll 操作
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705231721.png)


## 6.2 创建表

```sql
CREATE TABLE product(
id varchar2(32) default SYS_GUID() PRIMARY KEY,
productNum VARCHAR2(50) NOT NULL,
productName VARCHAR2(50),
cityName VARCHAR2(50),
DepartureTime timestamp,
productPrice Number,
productDesc VARCHAR2(500),
productStatus INT,
CONSTRAINT product UNIQUE (id, productNum)
)
insert into PRODUCT (id, productnum, productname, cityname, departuretime, productprice,
productdesc, productstatus)
values ('676C5BD1D35E429A8C2E114939C5685A', 'itcast-002', '北京三日游', '北京', to_timestamp('10-
10-2018 10:10:00.000000', 'dd-mm-yyyy hh24:mi:ss.ff'), 1200, '不错的旅行', 1);
insert into PRODUCT (id, productnum, productname, cityname, departuretime, productprice,
productdesc, productstatus)
values ('12B7ABF2A4C544568B0A7C69F36BF8B7', 'itcast-003', '上海五日游', '上海', to_timestamp('25-
04-2018 14:30:00.000000', 'dd-mm-yyyy hh24:mi:ss.ff'), 1800, '魔都我来了', 0);
insert into PRODUCT (id, productnum, productname, cityname, departuretime, productprice,
productdesc, productstatus)
values ('9F71F01CB448476DAFB309AA6DF9497F', 'itcast-001', '北京三日游', '北京', to_timestamp('10-
10-2018 10:10:00.000000', 'dd-mm-yyyy hh24:mi:ss.ff'), 1200, '不错的旅行', 1);
```

## 6.3 pom.xml
参考以前的章节

[【Mybatis 】（一）入门案例](https://blog.csdn.net/qq_41864648/article/details/103870949)内有连接MySQL的依赖

[【SSM】三、maven环境整合ssm（上）](https://blog.csdn.net/qq_41864648/article/details/104988909)内有连接Oracle的依赖



```xml

	 <properties>
        <mybatis.version>3.4.5</mybatis.version>
          <oracle.version>10.2.0.4.0</oracle.version>
    </properties>
     <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>${mybatis.version}</version>
        </dependency>
		 <dependency>
            <groupId>com.oracle</groupId>
            <artifactId>ojdbc14</artifactId>
            <version>${oracle.version}</version>
        </dependency>
		 <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13-beta-3</version>
            <scope>test</scope>
        </dependency>

```


## 6.4 实体类

```java
/**
 * 产品信息
 */
public class Product {
    private String id; // 主键
    private String productNum; // 编号 唯一
    private String productName; // 名称
    private String cityName; // 出发城市
    private Date departureTime; // 出发时间
    private String departureTimeStr;
    private double productPrice; // 产品价格
    private String productDesc; // 产品描述
    private Integer productStatus; // 状态 0 关闭 1 开启
    private String productStatusStr;
 }

```


## 6.5 持久层接口

```java
public interface IProductDao {
    /**
     * 查询所有的产品信息
     */
    @Select("select * from product")
  public   List<Product> findAll() throws Exception;
}
```


## 6.6 SqlMapConfig.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--mybaits的主配置文件-->
<configuration>
    <!--配置环境-->
    <environments default="oracle">
        <!--配置mysql环境-->
        <environment id="oracle">
            <!--配置事务类型-->
            <transactionManager type="JDBC"></transactionManager>
            <!--配置数据源（连接池）-->
            <dataSource type="POOLED">
                <!--配置数据源（连接池）的基本信息-->
                <property name="driver" value="oracle.jdbc.driver.OracleDriver"/>
                <property name="url" value="jdbc:oracle:thin:@192.168.80.10:1521:orcl"/>
                <property name="username" value="ssm"/>
                <property name="password" value="itcast"/>

            </dataSource>
        </environment>
    </environments>

    <!--指定映射配置文件的位置，映射配置文件指的是每个dao配置的文件-->
    <mappers>
<!--        <mapper resource="code/dao/IUserDao.xml"/> &lt;!&ndash;   使用的是xml&ndash;&gt;-->
                <mapper class="code.dao.IProductDao"/>    <!--使用的是注解 annotations-->
    </mappers>

</configuration>
```


## 6.7 测试类

```java
public class TestDao {

    @Test
    public void test1() throws Exception {

        // 1.读取配置文件
        InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
        //2.创建者模式创建工厂
       SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
        //3. 工厂模式生成 SqlSession
        SqlSession sqlSession = factory.openSession();
        //4.代理模式生成dao
        IProductDao dao = sqlSession.getMapper(IProductDao.class);
        //5.调用方法
        List<Product> products = dao.findAll();

        //6.输出结果
        for (Product product : products){
            System.out.println(product);
        }
    }
}

```


![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705232019.png)



