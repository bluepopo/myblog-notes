【SSM】后台权限管理项目



# 一、AdminLTE 的基本使用

## 1. 概述与基本使用
**1.AdminLTE介绍**
AdminLTE 是一款建立在 **bootstrap** 和 **jquery** 之上的开源的模板主题工具，它提供了一系列响应的、可重复使用的组件，并内置了多个模板页面；同时自适应多种屏幕分辨率，兼容PC和移动端。通过AdminLTE，我们可以快速的创建一个响应式的Html5网站。
获取地址:**github**:[https://github.com/almasaeed2010/AdminLTE](https://github.com/almasaeed2010/AdminLTE)

github上下载[ 黑马定制版 AdminLTE2 ]( https://github.com/itheima2017/adminlte2-itheima)

**2.AdminLTE布局与皮肤**
1. **布局**
- .wrapper包住了body下的所有代码
-  .main-header里是网站的logo和导航栏的代码
-  .main-sidebar里是用户面板和侧边栏菜单的代码
-  .content-wrapper里是页面的页面和内容区域的代码
-  .main-footer里是页脚的代码
-  .control-sidebar里是页面右侧侧边栏区域的代码

2. **布局选项**
-  fixed：固定
-  layout-boxed：盒子布局
- layout-top-nav：顶部隐藏
- sidebar-collapse：侧边栏隐藏
-  sidebar-mini：侧边栏隐藏时有小图标

**3.  皮肤**
-  skin-blue：蓝色
-  skin-black：黑色
-  skin-purple：紫色
-  skin-yellow：黄色
-  skin-red：红色
-  skin-green：绿色

以上项我们可以查看start.html页面中查看。

**3. AdminLTE2-IT黑马目录结构**
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720132634.png)

由于AdminLTE2-IT黑马-定制版是基于FIS3进行开发，在目录结构中assets、modules、pages、
plugins都是前端开发时所使用到的，最终发布的就是release。所以对于我们使用AdminLTE2-IT黑
马-定制版来说，我们只需要关注release目录下的结构就可以。

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720132639.png)







# 二、VMware搭建Oracle数据库

## 1.VMware挂载windows server 2003

打开“资料/Oracle相关资料”文件夹中的` windows2003 `包解压成文件夹，双击扩展名为vmx的文件即可将windows2003系统挂载到VMware中
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706105304.png)

## 2.  网络配置
1.创建虚拟网卡
VMware 中选择菜单“编辑”--“虚拟网络编辑器”
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706105312.png)

弹出的窗口中，点击“添加网络”按钮，名称为VMnet2 ，确定
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706105318.png)

设置为仅主机方式，并设定子网IP为192.168.80.0 
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706105500.png)

**2.设定虚拟操作系统的网络网卡**

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706105509.png)

右键点击虚拟操作系统，选择“设置”菜单项，弹出以下窗口
点击网络适配器，选择自定义，VMnet2 
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706105523.png)

**3.设定虚拟操作系统的IP地址**
开启此虚拟机的操作系统，并在其中的“本地连接”中设定IP地址为 `192.168.80.10  `


![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706105537.png)

设置本地的操作系统的虚拟网卡VMnet的IP为 `192.168.80.6`
(与虚拟机中的操作系统的IP地址处于同一IP网段)
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706105547.png)

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706105553.png)

在本地操作系统打开命令行，用ping命令测试网络是否连接成功
`ping 192.168.80.10`
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706105606.png)

## 3. 在虚拟机系统安装Oracle数据库
**3.1.** 将**“资源”**文件夹ORACLE  安装包: `database ` 解压拷贝到虚拟机的系统中
并双击解压目录下的` setup.exe `，出现安装界面，如下：

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706105721.png)
3.2 输入口令和确认口令，如：itcast，点击下一步，出现如下进度条，
**注：此口令即是管理员密码。**

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706105727.png)


![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706105736.png)

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706105744.png)

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706105936.png)

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706105945.png)

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706105951.png)

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706105917.png)

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706105958.png)

**3.3 测试安装是否成功**

此时可以命令提示符下进行测试安装结果
输入: `sqlplus system/itcast`
itcast为你安装时输入的密码
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706110003.png)

注：在以后的测试中这里我出现过`“shared memory realm does not exist”`错误，原因是关闭数据库时未正常关闭导致。
解决方案为使用如下命令开启数据库服务
**1.进入cmd**
　　 输入以下命令重启数据库
**2、sqlplus /nolog**
　　 命令意思：登陆数据库的操作

**3、conn 用户名/密码 as sysdba**
　　 命令意思：以管理员权限登陆

**4、startup**
　　 命令意思：启动数据库
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706110009.png)


## 4. SQLPlus远程连接ORACLE数据库
将“资源”文件夹中的`instantclient_12_1 `拷贝到D盘根目录
进入命令提示符，进入该目录，输入如下命令连接远程的ORACLE
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706110015.png)

## 5. PLSQL Developer安装与配置
1. 在本地系统安装资源文件夹`PLSQL+Developer10.0.3.1701`,直接双击` .exe `文件即可
注意事项：安装目录不能有中文和空格，否则连接不上

2. 安装过程根据向导做“下一步”即可

3. 第一次登录先不使用用户名身份，直接以no log 
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706110057.png)

4. 在弹出的登陆窗口中，点取消，然后在软件界面上方工具条找到 `Tools->>Preferences`
将oracle支持的远程连接的`文件instantclient_12_1`所在路径和` oci.dll `路径，复制到如下两栏
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706110142.png)

5. 重新打开 PLSQL Developer,以用户身份填写用户名、密码、远程数据库服务器的ip地址，并点击ok登录
**注意事项**：在进行本项的登录之前，确保虚拟机的Oracle服务器正常开启，否则连接不上
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706110156.png)

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706110203.png)

## 6. 编辑ORA文件，设置环境变量
1. 在虚拟机的Oracle安装目录下 找到`C:\oracle\product\10.2.0\db_1\NETWORK\ADMIN`

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706110226.png)

2. 打开`tnsnames.ora`配置文件
ip 地址在虚拟机的` ipconfig `中可以查看
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706110242.png)

3. 将修改后虚拟机内的`tnsnames.ora` 文件复制到本机系统一个目录下
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706110258.png)

4. 设置环境变量 `TNS_ADMIN`  ，路径就是上面的`tnsnames.ora` 配置文件所在的路径
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706110308.png)

5. 重新启动 PLSQL Developer,这样在每次打开都要写一遍 Oracle远程服务器ip地址的时候，就可以用我们上面创建好的环境变量的配置，来帮我们使用快捷方式了

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720132720.png)

点击OK，登录成功后的界面如下
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706110331.png)

---


## 7. 补充：中文编码设置


在 SQLDevloper 的 SQL Window 下编写 sql 语句，查看服务器端编码 SQL :

```sql
select userenv('language') from dual;
```

我实际查到的结果为: **AMERICAN_AMERICA.ZHS16GBK** 

在本机设置环境变量：计算机->属性->高级系统设置->环境变量->新建

设置变量名:**NLS_LANG**
变量值:第1步查到的值， 我的是		**AMERICAN_AMERICA.ZHS16GBK**



![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706110453.png)

再次执行查询，如果中文显示还是???，那就**重启Oracle服务器或者删除表再重新创建表**，
然后再查询。
```sql
select * from product;
```
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706110444.png)





# 三、后台权限管理：环境搭建



## <font color=#E67E22>1.maven工程搭建</font>
### 1.1 创建maven工程

使用子模块的形式。首先创建一个maven project ：heima_ssm，接着创建几个子模块

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706112304.png)

### 1.2 pom.xml

在父工程的pom.xml配置下添加如下

```xml
	<properties>
        <spring.version>5.0.2.RELEASE</spring.version>
        <slf4j.version>1.6.6</slf4j.version>
        <log4j.version>1.2.12</log4j.version>
        <oracle.version>11.2.0.1.0</oracle.version>
        <mybatis.version>3.4.5</mybatis.version>
        <spring.security.version>5.0.1.RELEASE</spring.security.version>
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
            <artifactId>spring-context-support</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>

        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
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

       <!-- mybatis start -->
        
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
        
        <!-- mybatis end -->
        
        
          <!-- 分页查询 -->
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper</artifactId>
            <version>5.1.2</version>
        </dependency>
        
        
         <!-- security 安全登录 -->
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-web</artifactId>
            <version>${spring.security.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-config</artifactId>
            <version>${spring.security.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-core</artifactId>
            <version>${spring.security.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-taglibs</artifactId>
            <version>${spring.security.version}</version>
        </dependency>
        
        
         <!-- security 安全登录 end -->

        <dependency>
            <groupId>com.oracle</groupId>
            <artifactId>ojdbc14</artifactId>
            <version>${oracle.version}</version>
        </dependency>

        <dependency>
            <groupId>javax.annotation</groupId>
            <artifactId>jsr250-api</artifactId>
            <version>1.0</version>
        </dependency>
    </dependencies>
    <build>
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.2</version>
                    <configuration>
                        <source>1.8</source>
                        <target>1.8</target>
                        <encoding>UTF-8</encoding>
                        <showWarnings>true</showWarnings>
                    </configuration>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>



```


### 1.3 编写实体类
在`heima_domain`子模块下创建包路径`main/java/code/Product.java`

```java
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
    
  //。。。省略get 、set 方法
}
```


### 1.4 编写持久层接口

```java
public interface IProductDao {
    /**
     * 查询所有的产品信息
     */
    @Select("select * from product")
  public   List<Product> findAll() throws Exception;
}

```


### 1.5 编写业务层接口

```java
public interface IProductService {
    /**
     * 查询所有的产品信息
     */
    public List<Product> findAll() throws Exception;
}

```

**1.6 业务层实现类先创建上**

```java
@Service("productService")
@Transactional
public class ProductServiceImpl implements IProductService {

    @Autowired
    private IProductDao productDao;

    @Override
    public List<Product> findAll() throws Exception {

        return productDao.findAll();
    }
}

```

---

## <font color=#E67E22>2.spring环境搭建</font>
ssm整合（图）
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706112314.png)




### 2.1 applicationContext.xml

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


    <!-- 开启注解扫描，管理service和dao -->
    <context:component-scan base-package="code.service">
    </context:component-scan>
   <context:component-scan base-package="code.dao">
    </context:component-scan>


	<!--    spring整合mybatis-->
    <context:property-placeholder location="classpath:db.properties"/>
    <!-- 配置连接池 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}" />
        <property name="jdbcUrl" value="${jdbc.url}" />
        <property name="user" value="${jdbc.username}" />
        <property name="password" value="${jdbc.password}" />
    </bean>
    <!-- 把交给IOC管理 SqlSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
        <!-- 传入PageHelper的插件 -->
        <property name="plugins">
            <array>
                <!-- 传入插件的对象 -->
                <bean class="com.github.pagehelper.PageInterceptor">
                    <property name="properties">
                        <props>
                            <prop key="helperDialect">mysql</prop>
                            <prop key="reasonable">true</prop>
                        </props>
                    </property>
                </bean>
            </array>
        </property>
    </bean>

    <!-- 扫描dao接口 -->
    <bean id="mapperScanner" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="code.dao"/>
    </bean>

    <!-- 配置Spring的声明式事务管理 -->
    <!-- 配置事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <tx:annotation-driven transaction-manager="transactionManager"/>

</beans>

```

### 2.2 业务层加上注解

```java
@Service("productService")
//......

   @Autowired
    private IProductDao productDao;
```

 







## <font color=#E67E22>3.springMVC环境搭建</font>
### 3.1 web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">

  <!-- 配置加载类路径的配置文件 -->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
  </context-param>

  <!-- 配置监听器 -->
  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>
  <!-- 配置监听器，监听request域对象的创建和销毁的 -->
  <listener>
    <listener-class>org.springframework.web.context.request.RequestContextListener</listener-class>
  </listener>
  

  <!-- 前端控制器（加载classpath:springmvc.xml 服务器启动创建servlet） -->
  <servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!-- 配置初始化参数，创建完DispatcherServlet对象，加载springmvc.xml配置文件 -->
    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:springmvc.xml</param-value>
    </init-param>
    <!-- 服务器启动的时候，让DispatcherServlet对象创建 -->
    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>*.do</url-pattern>
  </servlet-mapping>

  <!-- 解决中文乱码过滤器 -->
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


  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
</web-app>

```

### 3.2 springmvc.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="
           http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/mvc
           http://www.springframework.org/schema/mvc/spring-mvc.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context.xsd
           http://www.springframework.org/schema/aop
		http://www.springframework.org/schema/aop/spring-aop.xsd
           ">

    <!-- 扫描controller的注解，别的不扫描 -->
    <context:component-scan base-package="code.controller">
    </context:component-scan>

    <!-- 配置视图解析器 -->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!-- JSP文件所在的目录 -->
        <property name="prefix" value="/pages/" />
        <!-- 文件的后缀名 -->
        <property name="suffix" value=".jsp" />
    </bean>

    <!-- 设置静态资源不过滤 -->
    <mvc:resources location="/css/" mapping="/css/**" />
    <mvc:resources location="/img/" mapping="/img/**" />
    <mvc:resources location="/js/" mapping="/js/**" />
    <mvc:resources location="/plugins/" mapping="/plugins/**" />

    <!-- 开启对SpringMVC注解的支持 -->
    <mvc:annotation-driven />

    <!--
        支持AOP的注解支持，AOP底层使用代理技术
        JDK动态代理，要求必须有接口
        cglib代理，生成子类对象，proxy-target-class="true" 默认使用cglib的方式
    -->
    <aop:aspectj-autoproxy proxy-target-class="true"/>

</beans>

```


### 3.3 编写Controller

```java
@Controller
@RequestMapping(path = "/product")
public class ProductController {

    @Autowired
    private IProductService productService;

    @RequestMapping(path = "/findAll.do")
    public ModelAndView findAll() throws Exception {

        System.out.println("web....");

        ModelAndView mv = new ModelAndView();
        List<Product> products = productService.findAll();
        mv.addObject("productList",products);
        mv.setViewName("product-list"); //指定视图
        return mv;
    }

}

```


## <font color=#E67E22>4.spring整合springMVC</font>
配置 context 的监听器，在上面的 web.xml 文件中已经添加了。（也就是下面的代码）
> ### 4.1 web.xml
>

> ```xml
> <!-- 配置加载类路径的配置文件 -->
> <context-param>
> <param-name>contextConfigLocation</param-name>
> <param-value>classpath:applicationContext.xml</param-value>
> </context-param>
> <!-- 配置监听器 -->
> <listener>
> <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
> </listener>
> ```
>




## <font color=#E67E22>5.spring整合Mybatis</font>
单独测试Mybatis的连通性，请先看 
[【Mybatis】五、使用注解的简单查询操作](https://blog.csdn.net/qq_41864648/article/details/105043359)

**整合思路**：把 mybatis 配置文件（mybatis.xml）中内容配置到 spring 配置文件中同时，把 mybatis 配置内容清掉。


### 5.1 db.properties

```java
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssm?useUnicode=true&characterEncoding=utf8
jdbc.username=root
jdbc.password=root
```

### 5.2 applicationContext.xml
在applicationContext.xml 文件中添加
注意下面的代码添加了 `分页管理 的插件`。(下面的代码在 2.2 节Spring环境搭建中已经添加了)

```xml
<!--    spring整合mybatis-->
    <context:property-placeholder location="classpath:db.properties"/>
    <!-- 配置连接池 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}" />
        <property name="jdbcUrl" value="${jdbc.url}" />
        <property name="user" value="${jdbc.username}" />
        <property name="password" value="${jdbc.password}" />
    </bean>
    <!-- 把交给IOC管理 SqlSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
        <!-- 传入PageHelper的插件 -->
        <property name="plugins">
            <array>
                <!-- 传入插件的对象 -->
                <bean class="com.github.pagehelper.PageInterceptor">
                    <property name="properties">
                        <props>
                            <prop key="helperDialect">mysql</prop>
                            <prop key="reasonable">true</prop>
                        </props>
                    </property>
                </bean>
            </array>
        </property>
    </bean>

    <!--自动扫描所有Mapper接口和文件 -->
    <!-- 扫描dao接口 -->
    <bean id="mapperScanner" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="code.dao"/>
    </bean>

    <!-- 配置Spring的声明式事务管理 -->
    <!-- 配置事务管理器 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <tx:annotation-driven transaction-manager="transactionManager"/>

```


## <font color=#E67E22>6. 测试运行</font>

### 6.1 请求发起页面 index.jsp

```html

<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>index</title>
</head>
<body>
<a href="${pageContext.request.ContextPath}/product/findAll.do">查询所有产品信息</a>
</body>
</html>

```

跳转页面如下：


![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706112322.png)


### 6.2 解决数据类型显示问题

如上结果界面，虽然查询到了正确数据，但是日期类型和 状态类型没有正确显示
原因是在结果页面上的文本框中应该是 string 类型数据
**解决方法**：在` get `数据库中的类型时，将对应类型转换为` String `类型
![数据库中](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706112252.png)

1. 修改状态类型，返回字符串

```java
 public String getProductStatusStr() {
        if(productStatus != null){
            if (productStatus==0){
                productStatusStr = "关闭";
            }
            if (productStatus == 1){
                productStatusStr = "开启";
            }
        }
        return productStatusStr;
    }
```




2. 在 utils 子模块下，创建一个工具类

```java
/**日期类型于字符串转换的工具类
 *
 */
public class DateUtils {

    /**
     *  日期转换成字符串
     */
    public static  String date2String(Date date,String patt){
        SimpleDateFormat sdf = new SimpleDateFormat(patt);
        String format = sdf.format(date);

        return format;
    }

    /**
     *  字符串转换成日期
     */
    public static  Date string2Date(String str,String patt) throws ParseException {
        SimpleDateFormat sdf = new SimpleDateFormat(patt);
        Date parse = sdf.parse(str);
        return parse;
    }

}

```

3.  修改实体类返回值` Product.java`
 查询到的 `DepartureTime `数据库值会赋值到 DepartureTime 类属性，在这里借助 DepartureTimeStr 类成员变量做一个转换，页面显示时调用此属性值，就可以得到想要的结果效应。

```java
   public String getDepartureTimeStr() {
        if (departureTime!=null){
            departureTimeStr = DateUtils.date2String(departureTime,"yy-mm-dd HH:mm:ss");
        }
        return departureTimeStr;
    }
```

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706112238.png)



# 四、后台权限管理：添加产品

## 1. 思路流程

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706112439.png)

## 2. 产品添加操作


1. 在 product-list 页面的` “创建” `按钮添加点击事件，跳转到` product-add.jsp`
```html
 <button type="button" class="btn btn-default" title="新建" onclick="location.href='${pageContext.request.contextPath}/pages/product-add.jsp'"><i class="fa fa-file-o"></i> 新建</button>

```

2. ` product-add.jsp` 页面的表单提交调用 save 方法
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720132745.png)

3. ProductController类

```java
  /**
     * 保存产品
     */
    @RequestMapping(path = "/save.do")
    public String save(Product product) throws Exception {
        productService.save(product);
        return "redirect:findAll.do";
    }

```


4. 业务层添加save 方法

```java
public interface IProductService {
 /**
     * 保存产品
     * @param product
     */
    void save(Product product)throws Exception;
}


```


业务层实现类

```java
 @Override
    public void save(Product product) throws Exception {

        productDao.save(product);

    }
```


5. 持久层接口

```java
 @Insert("insert into product (productNum,productName,cityName,departureTime,productPrice,productDesc,productStatus)" +
            "values(#{productNum},#{productName},#{cityName},#{departureTime},#{productPrice},#{productDesc},#{productStatus})")
    void save(Product product);
```

**6. 实体类与页面的数据类型转换问题**
页面选择的`日期类型转换`成数据库中的日期类型

```java
 @DateTimeFormat(pattern = "yyyy-mm-dd HH:mm")
    private Date departureTime; // 出发时间
```


7. 启动服务器测试





# 五、后台权限管理：订单操作

注：本节是针对 **订单表** 的操作

## 1. 表结构分析

下图是多表之间的关系，可以联系到现实中购买火车票的例子。一个会员一个订单，但是可以多位乘客。

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706112946.png)




## 2. 订单-查询所有
**2.1 思路分析**
查询所有订单时，每一条订单内都有一个产品 Product ，一个产品可以对应多个订单。
所以 **订单与产品是 多对一 关系**。

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706113146.png)


### 2.2 OrdersController

```java
@Controller
@RequestMapping(path = "/orders")
public class OrdersController {

    @Autowired
    private IOrdersService ordersService;

    //未分页
    @RequestMapping(path = "/findAll.do")
    public ModelAndView findAll() throws Exception {
        ModelAndView mv = new ModelAndView();

        List<Orders> ordersList = ordersService.findAll();

        mv.addObject("ordersList",ordersList);
        mv.setViewName("orders-list");

        for (Orders orders:ordersList){
            System.out.println(orders);
        }
        return mv;
    }
}

```

### 2.3 业务层 IOrdersService

```java
public interface IOrdersService {

    List<Orders> findAll() throws Exception;
}

```

```java
@Service("orderService")
@Transactional
public class OrdersServiceImpl implements IOrdersService {

    @Autowired
   private IOrderDao orderDao;

    @Override
    public List<Orders> findAll() throws Exception {
        return orderDao.findAll();
    }
}
```


### 2.4 持久层 IOrderDao
订单表---产品表
多对一的多表查询，这里使用了 `@One `注解

```java
public interface IOrderDao {

    @Select("select * from orders")
    @Results({
            @Result(id = true,property ="id",column = "id"),
            @Result(property = "orderNum",column = "orderNum"),
            @Result(property = "orderTime",column = "orderTime"),
            @Result(property = "orderStatus",column = "orderStatus"),
            @Result(property = "peopleCount",column = "peopleCount"),
            @Result(property = "payType",column = "payType"),
            @Result(property = "orderDesc",column = "orderDesc"),
            @Result(property = "product",column = "productId",javaType = Product.class,one = @One(select = "code.dao.IProductDao.findById")),
    })
    List<Orders> findAll() throws  Exception;
}
```
### 2.5 持久层 IProductDao

```java
 /**
     * 根据id查询产品
     */
    @Select("select * from product where id = #{id}")
    Product findById(String id) throws Exception;
```


### 2.6 测试结果


![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706113835.png)






## 3. 订单-分页查询

### 3.1 PageHelper介绍
PageHelper是国内非常优秀的一款开源的mybatis分页插件，它支持基本主流与常用的数据库，例如mysql、
oracle、mariaDB、DB2、SQLite、Hsqldb等。
本项目在 github 的项目地址：https://github.com/pagehelper/Mybatis-PageHelper

### 3.2 PageHelper使用

引入分页插件有下面2种方式，推荐使用 Maven 方式。
1. 引入 Jar 包
你可以从下面的地址中下载最新版本的 jar 包
https://oss.sonatype.org/content/repositories/releases/com/github/pagehelper/pagehelper/
http://repo1.maven.org/maven2/com/github/pagehelper/pagehelper/
由于使用了sql 解析工具，你还需要下载 jsqlparser.jar：
http://repo1.maven.org/maven2/com/github/jsqlparser/jsqlparser/0.9.5/

2.  使用 Maven
在 pom.xml 中添加如下依赖：

```xml
<dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper</artifactId>
            <version>5.1.2</version>
 </dependency>
```
### 3.3 配置
特别注意，新版拦截器是 ` com.github.pagehelper.PageInterceptor `。  
`com.github.pagehelper.PageHelper `现在是一个特殊的`  dialect `实现类，是分页插件的默认**实现类**，提供了和以前相同的用法。

- 单独在 MyBatis 中配置 xml 中配置拦截器插件

```xml
<!--
plugins在配置文件中的位置必须符合要求，否则会报错，顺序如下:
properties?, settings?,
typeAliases?, typeHandlers?,
objectFactory?,objectWrapperFactory?,
plugins?,
environments?, databaseIdProvider?, mappers?
-->
<plugins>
<!-- com.github.pagehelper为PageHelper类所在包名 -->
<plugin interceptor="com.github.pagehelper.PageInterceptor">
<!-- 使用下面的方式配置参数，后面会有所有的参数介绍 -->
<property name="param1" value="value1"/>
</plugin>
</plugins>
```
 - 在 Spring 配置文件中配置拦截器插件

```xml
 <!-- 把交给IOC管理 SqlSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
        <!-- 传入PageHelper的插件 -->
        <property name="plugins">
            <array>
                <!-- 传入插件的对象 -->
                <bean class="com.github.pagehelper.PageInterceptor">
                    <property name="properties">
                        <props>
                            <prop key="helperDialect">oracle</prop>
                            <prop key="reasonable">true</prop>
                        </props>
                    </property>
                </bean>
                
            </array>
        </property>
    </bean>
```


###  3.4 分页插件参数介绍
1.  `helperDialect `：分页插件会自动检测当前的数据库链接，自动选择合适的分页方式。 你可以配置helperDialect 属性来指定分页插件使用哪种方言。配置时，可以使用下面的缩写值：
oracle , mysql , mariadb , sqlite , hsqldb , postgresql , db2 , sqlserver , informix , h2 , sqlserver2012derby



> 特别注意：使用 SqlServer2012 数据库时，需要手动指定为  sqlserver2012 ，否则会使用 SqlServer2005 的方式进行分页。
> 你也可以实现  AbstractHelperDialect ，然后配置该属性为实现类的全限定名称即可使用自定义的实现方法。



1.  `offsetAsPageNum `：默认值为  false ，该参数对使用  RowBounds 作为分页参数时有效。 当该参数设置为true 时，会将  RowBounds 中的  offset 参数当成  pageNum 使用，可以用页码和页面大小两个参数进行分页。
2.  ` rowBoundsWithCount` ：默认值为 false ，该参数对使用  RowBounds 作为分页参数时有效。 当该参数设置为 true 时，使用  RowBounds 分页会进行 count 查询。
3.  ` pageSizeZero `：默认值为  false ，当该参数设置为  true 时，如果  pageSize=0 或者  RowBounds.limit =0 就会查询出全部的结果（相当于没有执行分页查询，但是返回结果仍然是  Page 类型）。
4.  `reasonable` ：分页合理化参数，默认值为 false 。当该参数设置为  true 时， pageNum<=0 时会查询第一页， pageNum>pages （超过总数时），会查询最后一页。默认 false 时，直接根据参数进行查询。
5.  ` params `：为了支持 startPage(Object params) 方法，增加了该参数来配置参数映射，用于从对象中根据属性名取值， 可以配置  pageNum,pageSize,count,pageSizeZero,reasonable ，不配置映射的用默认值， 默认值为

> pageNum=pageNum; 
> pageSize=pageSize; 
> count=countSql;
> reasonable=reasonable; 
> pageSizeZero=pageSizeZero

7. ` supportMethodsArguments` ：支持通过 Mapper 接口参数来传递分页参数，默认值 false ，分页插件会从查询方法的参数值中，自动根据上面  params 配置的字段中取值，查找到合适的值时就会自动分页。 使用方法可以参考测试代码中的  com.github.pagehelper.test.basic 包下的  ArgumentsMapTest 和ArgumentsObjTest 。

8.  `autoRuntimeDialect `：默认值为  false 。设置为  true 时，允许在运行时根据多数据源自动识别对应方言的分页 （不支持自动选择 sqlserver2012 ，只能使用 sqlserver ），用法和注意事项参考下面的场景五。

9.  `closeConn `：默认值为  true 。当使用运行时动态数据源或没有设置  helperDialect 属性自动获取数据库类型时，会自动获取一个数据库连接， 通过该属性来设置是否关闭获取的这个连接，默认 true 关闭，设置为 false 后，不会关闭获取的连接，这个参数的设置要根据自己选择的数据源来决定。

###  3.5 基本使用

**OrdersController**

```java
  //分页
    @RequestMapping(path = "/findAll.do")
    public ModelAndView findAllByPage(@RequestParam(name = "page",required = true,defaultValue = "1") int page,
                                      @RequestParam(name = "size",required = true,defaultValue = "4") int size) throws Exception {

        ModelAndView mv = new ModelAndView();

        List<Orders> ordersList = ordersService.findAll(page,size);

        //PageInfo 就是一个分页bean
        PageInfo pageInfo = new PageInfo(ordersList);

        mv.addObject("pageInfo",pageInfo);
        mv.setViewName("orders-page-list");

        return mv;
    }
```



**orderService**
```java
@Service("orderService")
@Transactional
public class OrdersServiceImpl implements IOrdersService {

    @Autowired
   private IOrderDao orderDao;

    @Override
    public List<Orders> findAll(int page,int size) throws Exception {
        //分页查询 pageNum是页码值，pageSize是每页大小
        PageHelper.startPage(page,size);
        return orderDao.findAll();
    }
}
```

**订单管理 请求页面**

```html
	<li id="system-setting">
        <a href="${pageContext.request.contextPath}/orders/findAll.do?page=1&size=4"> 
            <iclass="fa fa-circle-o"></i> 订单管理
	   </a>
   </li>
```


响应页面 orders-page-list.jsp

```html
<c:forEach items="${pageInfo.list}" var="orders">

		<tr>
			<td><input name="ids" type="checkbox"></td>
			<td>${orders.id }</td>
			<td>${orders.orderNum }</td>
			<td>${orders.product.productName }</td>
			<td>${orders.product.productPrice }</td>
			<td>${orders.orderTimeStr }</td>
			<td class="text-center">${orders.orderStatusStr }</td>
			<td class="text-center">
			<button type="button" class="btn bg-olive btn-xs">订单</button>
			<button type="button" class="btn bg-olive btn-xs" onclick="location.href='${pageContext.request.contextPath}/orders/findById.do?id=${orders.id}'">详情</button>
			<button type="button" class="btn bg-olive btn-xs">编辑</button>
			</td>
		</tr>
</c:forEach>
```


## 4.  订单-详情查询

### 4.1 思路分析：
查询所有订单后，点击订单展示页的 “**详情**” ，展示该订单的订单信息、产品信息、会员、游客。
这是一个多表的联合查询，有多对一、一对一、多对多的关系

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706114808.png)

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706114843.png)

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706114849.png)


### 4.2 OrdersController

```java
 @RequestMapping(path = "/findById.do")
    public ModelAndView findById(@RequestParam(name = "id",required = true) String id)throws Exception{
        ModelAndView mv = new ModelAndView();
        Orders orders =  ordersService.findById(id);
        mv.addObject("orders",orders);
        mv.setViewName("orders-show");

        return mv;
    }
```

### 4.3 orderService

```java
 @Override
    public Orders findById(String id) {
        return orderDao.findById(id);
    }
```


### 4.4 IOrderDao



```java
 @Select("select * from orders where id = #{id}")
    @Results({
            @Result(id = true,property ="id",column = "id"),
            @Result(property = "orderNum",column = "orderNum"),
            @Result(property = "orderTime",column = "orderTime"),
            @Result(property = "orderStatus",column = "orderStatus"),
            @Result(property = "peopleCount",column = "peopleCount"),
            @Result(property = "payType",column = "payType"),
            @Result(property = "orderDesc",column = "orderDesc"),
            @Result(property = "product",column = "productId",javaType = Product.class,one = @One(select = "code.dao.IProductDao.findById")),
            @Result(property = "member",column = "memberId",javaType = Member.class,one = @One(select = "code.dao.IMemberDao.findById")),
            @Result(property = "travellers",column = "id",javaType = java.util.List.class,many = @Many(select = "code.dao.ITravellerDao.findByOrdersId"))


    })
    Orders findById(String id);
```

### 4.5 IMemberDao 

```java
public interface IMemberDao {

    @Select("select * from member where id = #{id}")
    Member findById(String id) throws Exception;
}
```

### 4.6 ITravellerDao
order_traveller 是多对多关系中的中间表


```java
public interface ITravellerDao {
    
    @Select("select * from traveller " +
            "where id " +
            "in " +
            "(select travellerId from order_traveller where orderId = #{ordersId})")
    List<Traveller> findByOrdersId(String ordersId) throws Exception;
    
}
```


![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706115022.png)







# 六、后台权限管理：用户角色权限

## <font color=#007C21>1. 用户管理</font>

### 1.1 表结构分析
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706115213.png)
### 1.1 用户表
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706115218.png)
**sql语句**

```sql
CREATE TABLE users(
id varchar2(32) default SYS_GUID() PRIMARY KEY,
email VARCHAR2(50) UNIQUE NOT NULL,
username VARCHAR2(50),
PASSWORD VARCHAR2(50),
phoneNum VARCHAR2(20),
STATUS INT
)
```
**实体类**
```java
public class UserInfo {
private String id;
private String username;
private String email;
private String password;
private String phoneNum;
private int status;
private String statusStr;
private List<Role> roles;
    //省略了get、set方法
}
```

### 1.2 角色表
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720132809.png)
**sql语句**

```sql
CREATE TABLE role(
id varchar2(32) default SYS_GUID() PRIMARY KEY,
roleName VARCHAR2(50) ,
roleDesc VARCHAR2(50)
)
```
**实体类**

```java
public class Role {
private String id;
private String roleName;
private String roleDesc;
private List<Permission> permissions;
private List<User> users;
}
```

### 1.3 用户与角色关联表
用户与角色之间是`多对多关系`，我们通过`user_role表`来描述其关联，
在实体类中User中存在List，在Role中有List.

而角色与权限之间也存在关系，我们会在后面介绍

```sql
CREATE TABLE users_role(
userId varchar2(32),
roleId varchar2(32),
PRIMARY KEY(userId,roleId),
FOREIGN KEY (userId) REFERENCES users(id),
FOREIGN KEY (roleId) REFERENCES role(id)
)
```

### 1.4 权限表
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720132815.png)
**sql语句**

```sql
CREATE TABLE permission(
id varchar2(32) default SYS_GUID() PRIMARY KEY,
permissionName VARCHAR2(50) ,
url VARCHAR2(50)
)
```

**实体类**

```java
public class Permission {
private String id;
private String permissionName;
private String url;
private List<Role> roles;
}
```

### 1.5 权限资源与角色关联表
权限资源与角色是多对多关系，我们使用role_permission表来描述。在实体类Permission中存在List,在Role类中有List

```sql
CREATE TABLE role_permission(
permissionId varchar2(32),
roleId varchar2(32),
PRIMARY KEY(permissionId,roleId),
FOREIGN KEY (permissionId) REFERENCES permission(id),
FOREIGN KEY (roleId) REFERENCES role(id)
)
```

## 2. Spring Security概述
---

Spring Security 的前身是 Acegi Security ，是 Spring 项目组中用来提供安全认证服务的框架。
[https://projects.spring.io/spring-security/](https://projects.spring.io/spring-security/) Spring Security 为基于J2EE企业应用软件提供了全面安全服务。

人们使用Spring Security有很多种原因，不过通常吸引他们的是在J2EE Servlet规范或EJB规范中找不到典型企业应用场景的解决方案。 特别要指出的是他们不能再 WAR 或 EAR 级别进行移植。这样，如果你更换服务器环境，就要在新的目标环境进行大量的工作，对你的应用系统进行重新配置安全。

使用Spring Security 解决了这些问题，也为你提供很多有用的，完全可以指定的其他安全特性。

 安全包括两个主要操作。
“认证”，是为用户建立一个他所声明的主体。主题一般式指用户，设备或可以在你系 统中执行动作的其他系统。
“授权”指的是一个用户能否在你的应用中执行某个操作，在到达授权判断之前，身份的主题已经由身份验证过程建立了。
这些概念是通用的，不是Spring Security特有的。






### 2.1 快速使用
如图，快速使用 Spring Security ，只需要 `三步配置 `
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706115429.png)

**Maven依赖**
```xml
<dependencies>
	<dependency>
		<groupId>org.springframework.security</groupId>
		<artifactId>spring-security-web</artifactId>
		<version>5.0.1.RELEASE</version>
	</dependency>
	<dependency>
		<groupId>org.springframework.security</groupId>
		<artifactId>spring-security-config</artifactId>
		<version>5.0.1.RELEASE</version>
	</dependency>
</dependencies>
```


**web.xml**

```xml

<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:spring-security.xml</param-value>
  </context-param>


  <listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>

  <filter>
    <filter-name>springSecurityFilterChain</filter-name>
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
  </filter>
  <filter-mapping>
    <filter-name>springSecurityFilterChain</filter-name>
    <url-pattern> /* </url-pattern>
  </filter-mapping>
```

**spring-security.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:security="http://www.springframework.org/schema/security"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/security
        http://www.springframework.org/schema/security/spring-security.xsd">
    <security:http auto-config="true" use-expressions="false">
        <!-- intercept-url定义一个过滤规则 pattern表示对哪些url进行权限控制，ccess属性表示在请求对应
        的URL时需要什么权限，
        默认配置时它应该是一个以逗号分隔的角色列表，请求的用户只需拥有其中的一个角色就能成功访问对应
        的URL -->
        <security:intercept-url pattern="/**" access="ROLE_USER" />
        <!-- auto-config配置后，不需要在配置下面信息 <security:form-login /> 定义登录表单信息
        <security:http-basic
        /> <security:logout /> -->
    </security:http>
    <security:authentication-manager>
        <security:authentication-provider>
            <security:user-service>
                <security:user name="user" password="{noop}user"
                               authorities="ROLE_USER" />
                <security:user name="admin" password="{noop}admin"
                               authorities="ROLE_ADMIN" />
            </security:user-service>
        </security:authentication-provider>
    </security:authentication-manager>
</beans>
```


**测试**

启动服务器，出现如下登录页面，此 ` login `页面是一个 `Spring Security `的默认页面

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706115554.png)

若使用admin管理员角色登录，会出现以下403错误，提示是没有权限
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706115642.png)

### 2.2 自定义页面使用  Spring Security 


**spring-security.xml**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:security="http://www.springframework.org/schema/security"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/security
        http://www.springframework.org/schema/security/spring-security.xsd">


    <!-- 配置不过滤的资源（静态资源及登录相关） -->
    <security:http security="none" pattern="/login.html" />
    <security:http security="none" pattern="/failer.html" />
    <security:http auto-config="true" use-expressions="false" >
        <!-- 配置资料连接，表示任意路径都需要ROLE_USER权限 -->
        <security:intercept-url pattern="/**" access="ROLE_USER" />
        <!-- 自定义登陆页面，login-page 自定义登陆页面 authentication-failure-url 用户权限校验失败之后才会跳转到这个页面，如果数据库中没有这个用户则不会跳转到这个页面。
            default-target-url 登陆成功后跳转的页面。 注：登陆页面用户名固定 username，密码 password，action:login -->
        <security:form-login login-page="/login.html"
                             login-processing-url="/login" username-parameter="username"
                             password-parameter="password" authentication-failure-url="/failer.html"
                             default-target-url="/success.html"
        />
        <!-- 登出， invalidate-session 是否删除session logout-url：登出处理链接 logout-success-url：登出成功页面
            注：登出操作 只需要链接到 logout即可登出当前用户 -->
        <security:logout invalidate-session="true" logout-url="/logout"
                         logout-success-url="/login.jsp" />
        <!-- 关闭CSRF,默认是开启的 -->
        <security:csrf disabled="true" />
    </security:http>


    <security:authentication-manager>
        <security:authentication-provider>
            <security:user-service>
                <security:user name="user" password="{noop}user"
                               authorities="ROLE_USER" />
                <security:user name="admin" password="{noop}admin"
                               authorities="ROLE_ADMIN" />
            </security:user-service>
        </security:authentication-provider>
    </security:authentication-manager>
</beans>
```


**login.html**
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Insert title here</title>
</head>
<body>
<form action="login" method="post">
    <table>
        <tr>
            <td>用户名：</td>
            <td><input type="text" name="username" /></td>
        </tr>
        <tr>
            <td>密码：</td>
            <td><input type="password" name="password" /></td>
        </tr>
        <tr>
            <td colspan="2" align="center"><input type="submit" value="登录" />
                <input type="reset" value="重置" /></td>
        </tr>
    </table>
</form>
</body>
</html>
```

**自定义页面时需要在 spring-security.xml 中添加如下配置**
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706115905.png)

### 2.3 Spring Security使用数据库认证<font color=#009C41>（用户登录）</font>

之前我们采用了配置文件的方式从数据库中读取用户进行登录。虽然该方式的灵活性相较于静态账号密码的方式灵活了许多，但是将数据库的结构暴露在明显的位置上，绝对不是一个明智的做法。通过Java代码实现UserDetailsService接口来实现身份认证。





如下图，在这里 spring-security 框架取代了原来的 Controller 

- 使用配置文件的方式指明登录页面、成功、失败页面

- 在配置文件中指明了 ` “认证器”  `service , service与dao联系操作到数据库
- Iserivice 继承 `UserServiceDetails`
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706115931.png)

**web.xml 中添加 security 框架的配置代码**

```xml
<!-- 配置加载类路径的配置文件 -->
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml,classpath:spring-security.xml</param-value>
  </context-param>
```

```xml
<!-- 委派过滤器 -->
  <filter>
    <filter-name>springSecurityFilterChain</filter-name>
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
  </filter>
  <filter-mapping>
    <filter-name>springSecurityFilterChain</filter-name>
    <url-pattern> /* </url-pattern>
  </filter-mapping>
```




 **spring-security.xml 使用数据库中的用户进行认证**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:security="http://www.springframework.org/schema/security"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/security
    http://www.springframework.org/schema/security/spring-security.xsd">

    <!-- 配置不拦截的资源 -->
    <security:http pattern="/login.jsp" security="none"/>
    <security:http pattern="/failer.jsp" security="none"/>
    <security:http pattern="/css/**" security="none"/>
    <security:http pattern="/img/**" security="none"/>
    <security:http pattern="/plugins/**" security="none"/>

    <!--
    	配置具体的规则
    	auto-config="true"	不用自己编写登录的页面，框架提供默认登录页面
    	use-expressions="false"	是否使用SPEL表达式（没学习过）
    -->
    <security:http auto-config="true" use-expressions="false">
        <!-- 配置具体的拦截的规则 pattern="请求路径的规则" access="访问系统的人，必须有ROLE_USER的角色" -->
        <security:intercept-url pattern="/**" access="ROLE_USER,ROLE_ADMIN"/>

        <!-- 定义跳转的具体的页面 -->
        <security:form-login
                login-page="/login.jsp"
                login-processing-url="/login.do"
                default-target-url="/index.jsp"
                authentication-failure-url="/failer.jsp"
                authentication-success-forward-url="/pages/main.jsp"
        />

        <!-- 关闭跨域请求 -->
        <security:csrf disabled="true"/>

        <!-- 退出 -->
        <security:logout invalidate-session="true" logout-url="/logout.do" logout-success-url="/login.jsp" />

    </security:http>

    <!-- 认证管理交由业务层：切换成数据库中的用户名和密码 -->
    <security:authentication-manager>
        <security:authentication-provider user-service-ref="userService">
            <!-- 配置加密的方式 -->
            <security:password-encoder ref="passwordEncoder"/>
        </security:authentication-provider>
    </security:authentication-manager>

    <!-- 配置加密类 -->
    <bean id="passwordEncoder" class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder"/>

    <!-- 提供了入门的方式，在内存中存入用户名和密码
    <security:authentication-manager>
    	<security:authentication-provider>
    		<security:user-service>
    			<security:user name="admin" password="{noop}admin" authorities="ROLE_USER"/>
    		</security:user-service>
    	</security:authentication-provider>
    </security:authentication-manager>
    -->

</beans>

```
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706153127.png)

**创建IUserService继承UserDetailsService**

```java
import org.springframework.security.core.userdetails.UserDetailsService;

public interface IUserService extends UserDetailsService {

}

/**

为什么继承 UserDetailsService？
  验证身份就是加载响应的UserDetails，看看是否和用户输入的账号、密码、权限等信息匹配。此步骤由实现AuthenticationProvider的DaoAuthenticationProvider（它利用UserDetailsService验证用户名、密码和授权）处理。包含 GrantedAuthority 的 UserDetails对象在构建 Authentication对象时填入数据。

*/
```


**UserServiceImpl  重写方法进行与数据库匹配认证操作**
```java
/**
 * 认证器
 */
@Service("userService")
@Transactional
public class UserServiceImpl implements IUserService {

    @Autowired
    private IUserDao userDao;

    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        UserInfo userInfo = null;

        try {
            userInfo = userDao.findByUserName(username);
            System.out.println(userInfo);

        } catch (Exception e) {
            e.printStackTrace();
        }

        //处理自己的对象封装成UserDetails
        //将userInfo的Roles数据封装成权限集合对象，返回符合 UserDetails 要求的User对象
        User user = new User(userInfo.getUsername(),"{noop}"+ userInfo.getPassword(),userInfo.getStatus()==0?false:true,true,true,true,getAuthority(userInfo.getRoles()));//用户状态分激活/未激活
        return user;
    }


    /**
     * 返回list集合，集合中装入的是 角色描述
     我们知道UserDeitails接口里面有一个getAuthorities()方法。这个方法将返回此用户的所拥有的权限。这个集合将用于用户的访       问控制，也就是Authorization。

		所谓权限，就是一个字符串。一般不会重复。
		所谓权限检查，就是查看用户权限列表中是否含有匹配的字符串。
		在Security中，角色和权限共用GrantedAuthority接口，唯一的不同角色就是多了个前缀"ROLE_"
     * @return
     */
    public List<SimpleGrantedAuthority> getAuthority(List<Role> roles){
        List<SimpleGrantedAuthority> list = new ArrayList<>();

        for (Role role:roles){
            list.add(new SimpleGrantedAuthority("ROLE_"+role.getRoleName()));
        }

        return  list;
    }
}

```

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706120453.png)




**IUserDao**

```java
public interface IUserDao {

    @Select("select * from users where username = #{username}")
    @Results({
            @Result(id = true,property = "id",column = "id"),
            @Result(property = "username",column = "username"),
            @Result(property = "email",column = "email"),
            @Result(property = "password",column = "password"),
            @Result(property = "phoneNum",column = "phoneNum"),
            @Result(property = "status",column = "status"),
            @Result(property = "roles",column = "id",javaType = java.util.List.class,many = @Many(select = "code.dao.IRoleDao.findRoleByUserID"))
    })
    UserInfo findByUserName(String username) throws Exception;
}
```
**IRoleDao多表查询**

```java
public interface IRoleDao {

    @Select("select * from role where id in (select roleId from users_role where userId=#{userId})")
    List<Role> findRoleByUserID (String userId) throws Exception;
}
```



**logout 退出**

`logout-url="/logout.do"` ：只要页面发出 logout.do 的请求就可以触发spring-security 登出操作。
`logout-success-url="/login.jsp" ` ：登出成功后自动跳转到login.jsp
`invalidate-session="true" `：登出时自动杀死session

```xml
<!-- 退出 -->
        <security:logout invalidate-session="true" logout-url="/logout.do" logout-success-url="/login.jsp" />

```

## <font color=#009C41> 3.查询所有用户</font>
### 3.1 Controller
```java

@Controller
@RequestMapping("/user")
public class UserController {

    @Autowired
    private IUserService userService;


    /**
     * 查询所有用户
     */
    @RequestMapping("/findAll.do")
    public ModelAndView findAll() throws Exception{
        List<UserInfo> users = userService.findAll();
        ModelAndView mv = new ModelAndView();
        mv.addObject("userList",users);
        mv.setViewName("user-list");
        return mv;

	}

}
```


### 3.2 UserServiceImpl

```java
 /**
     * 查询所有
     */
    @Override
    public List<UserInfo> findAll() throws Exception {

        return userDao.findAll();
    }

```

### 3.3 IUserDao

```java
 @Select("select * from users")
    List<UserInfo> findAll();
```

## <font color=#009C41>4. 用户添加</font>
### 4.1 Controller
```java
 /**
     * 添加用户
     */
    @RequestMapping("/save.do")
    public  String  save(UserInfo userInfo) throws Exception{
        userService.save(userInfo);
        return  "redirect:findAll.do";
}
```


### 4.2 UserServiceImpl

在这里使用了 spring-security框架的密码加密类 `BCryptPasswordEncoder `
```java
 @Autowired
    private BCryptPasswordEncoder bCryptPasswordEncoder;

/**
     * 添加用户
     */
    @Override
    public void save(UserInfo userInfo) throws Exception {
        //对密码加密
        String password_encode = bCryptPasswordEncoder.encode(userInfo.getPassword());
        userInfo.setPassword(password_encode);
        //调用dao
        userDao.save(userInfo);
    }
```



### 4.3 IUserDao
```java
    @Insert("insert into users(email,username,password,phoneNum,status) values(#{email},#{username},#{password},#{phoneNum},#{status})")
    void save(UserInfo userInfo);
```

**4.4 测试密码加密方法**

```java
/**
 * 密码加密工具类
 */
public class BCryptPasswordEncoderUtils {
    private static BCryptPasswordEncoder bCryptPasswordEncoder = new BCryptPasswordEncoder();

    public static  String encodePassword(String password){
         String password_encode =  bCryptPasswordEncoder.encode(password);
         return  password_encode;
    }

    public static void main(String[] args) {
        String password = "hhh";
        String password_encode = encodePassword(password);
        System.out.println(password_encode);
    }

}

```

数据库中保存的是加密后的密码，而不是原来的密码
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706121148.png)

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706121155.png)


## <font color=#009C41>5. 用户详情</font>
### 5.1 思路分析

用户的详情包括角色信息、权限信息
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706121201.png)

### 5.2 UserController

```java
/**
 * 用户详情查询
 *
 */
    @RequestMapping("/findById.do")
    public ModelAndView findById(String id)throws Exception{
        ModelAndView mv = new ModelAndView();
        UserInfo userInfo = userService.findById(id);
        mv.addObject("user",userInfo);
        mv.setViewName("user-show1");

        return mv;
    }
```

### 5.3 IUserDao

```java
   @Select("select * from users where id = #{id}")
    @Results({
            @Result(id = true,property = "id",column = "id"),
            @Result(property = "username",column = "username"),
            @Result(property = "email",column = "email"),
            @Result(property = "password",column = "password"),
            @Result(property = "phoneNum",column = "phoneNum"),
            @Result(property = "status",column = "status"),
            @Result(property = "roles",column = "id",javaType = java.util.List.class,many = @Many(select = "code.dao.IRoleDao.findRoleByUserId")),

    })
    UserInfo findById(String id);
```

### 5.4 IRoleDao


```java
    @Select("select * from role where id in (select roleId from users_role where userId=#{userId})")
    @Results({
            @Result(id = true,property = "id",column = "id"),
            @Result(property = "roleName",column = "roleName"),
            @Result(property = "roleDesc",column = "roleDesc"),
            @Result(property = "permissions",column = "id",javaType = java.util.List.class,many = @Many(select = "code.dao.IPermissionDao.FindPermissionByRoleId"))
    })
    List<Role> findRoleByUserId (String userId) throws Exception;
```

### 5.5 IPermissionDao

```java

    @Select("select * from permission where id in (select permissionId from  role_permission where roleId =#{roleId})")
    List<Permission> FindPermissionByRoleId(String roleId) throws Exception;
```


### 5.6 user-show.jsp页面展示

```html
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```

```html
 <!--树表格-->
                        <div class="tab-pane" id="tab-treetable">
                            <table id="collapse-table" class="table table-bordered table-hover dataTable">
                                <thead>
                                <tr>
                                    <th>用户</th>
                                    <th>描述</th>
                                </tr>
                                </thead>

                                <tr data-tt-id="0">
                                    <td colspan="2">${user.username}</td>
                                </tr>
                                <tbody>
                                <c:forEach items="${user.roles}" var="role" varStatus="vs">

                                <tr data-tt-id="${vs.index+1}" data-tt-parent-id="0">
                                    <td>${role.roleName}</td>
                                    <td>${role.roleDesc}</td>
                                </tr>

                                    <c:forEach items="${role.permissions}" var="permission">
                                <tr data-tt-id="1-1" data-tt-parent-id="${vs.index+1}">
                                    <td>${permission.permissionName}</td>
                                    <td>${permission.url}</td>
                                </tr>
                                    </c:forEach>
                                </c:forEach>
                                </tbody>
                            </table>
                        </div>
   <!--树表格/-->
```


### 5.6 测试结果

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706121454.png)



---
<br>
<br>

## <font color=#F39C12> 6. 角色管理</font>
---



##  <font color=#F39C12> 1. 角色查询</font>
### 1.1 思路分析
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706121728.png)

### 1.2 RoleController

```java
@Controller
@RequestMapping(path = "/role")
public class RoleController {

    @Autowired
    private IRoleService roleService;

    @RequestMapping(path = "/findAll.do")
    public ModelAndView findAll()throws Exception{
        ModelAndView mv = new ModelAndView();
       List<Role> roles = roleService.findAll();
        mv.addObject("roleList",roles);
        mv.setViewName("role-list");
        return mv;

    }
}

```

###  1.3 IRoleDao

```java
    @Select("select * from role")
    List<Role> findAll() throws Exception;
```



![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706121945.png)

## <font color=#F39C12> 2. 角色添加</font>
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706122151.png)

### 2.1 RoleControlller

```java
   @RequestMapping(path = "/save.do")
    public String save(Role role)throws Exception{

        roleService.save(role);
        return "redirect:findAll.do";
    }

```


### 2.2 IRoleDao

```java
 @Insert("insert into role(roleName,roleDesc) values(#{roleName},#{roleDesc})")
    void save(Role role);
```
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706122156.png)



---
<br>
<br>

## <font color=#CA2C68>7. 资源权限管理</font>
---

## <font color=#CA2C68>1. 权限查询</font>
### 1.1 PermissionController 
```java
@Controller
@RequestMapping(path = "/permission")
public class PermissionController {
    @Autowired
    private IPermissionService permissionService;

    @RequestMapping("/findAll.do")
    public ModelAndView findAll() throws  Exception{
        ModelAndView mv = new ModelAndView();
        List<Permission> permissions = permissionService.findAll();
        mv.addObject("permissionList",permissions);
        mv.setViewName("permission-list");
        return mv;
    }

```


### 1.3 permission-list.jsp页面

```html
	<c:forEach items="${permissionList}" var="permission">
										<tr>
											<td><input name="ids" type="checkbox"></td>
											<td>${permission.id }</td>
											<td>${permission.permissionName }</td>
											<td>${permission.url }</td>
											<td class="text-center">
												<a href="${pageContext.request.contextPath}/permission/findById.do?id=${permission.id}" class="btn bg-olive btn-xs">详情</a>
												<a href="${pageContext.request.contextPath}/user/findUserByIdAndAllRole.do?id=${permission.id}" class="btn bg-olive btn-xs">添加角色</a>
											</td>
										</tr>
									</c:forEach>
```


### 1.2 IPermissionDao


```java
 @Select("select * from permission")
    List<Permission> findAll()throws  Exception;
```

## <font color=#CA2C68>2. 权限添加</font>
### 2.1 PermissionController 
```java
 @RequestMapping("/save.do")
    public String save(Permission permission) throws Exception{

        permissionService.save(permission);
        return "redirect:findAll.do";
    }
```

### 2.2 IPermissionDao
```java
   @Insert("insert into permission(permissionName,url) values(#{permissionName},#{url})")
    void save(Permission permission)throws  Exception;
```


### 2.3 permission-add.jsp页面

表单的提交
```html
<form action="${pageContext.request.contextPath}/permission/save.do"
				method="post">
				<!-- 正文区域 -->
				<section class="content"> <!--产品信息-->

				<div class="panel panel-default">
					<div class="panel-heading">资源权限信息</div>
					<div class="row data-type">

						<div class="col-md-2 title">资源权限名称</div>
						<div class="col-md-4 data">
							<input type="text" class="form-control" name="permissionName"
								placeholder="资源权限名称" value="">
						</div>
						<div class="col-md-2 title">URL</div>
						<div class="col-md-4 data">
							<input type="text" class="form-control" name="url"
								placeholder="url" value="">
						</div>
										

					</div>
				</div>
				<!--订单信息/--> <!--工具栏-->
				<div class="box-tools text-center">
					<button type="submit" class="btn bg-maroon">保存</button>
					<button type="button" class="btn bg-default"
						onclick="history.back(-1);">返回</button>
				</div>
				<!--工具栏/--> </section>
				<!-- 正文区域 /-->
			</form>
```
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706122223.png)







# 七、后台权限管理器：权限关联与控制

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706122345.png)

## 1. 用户关联角色
### 1.1 流程分析
**UserController**
- **findUserByIdAndAllRole**
  - 调用 IUserService 的 `findById` 方法获取要操作的User
  - 调用 IRoleService 的` findOtherRole `方法用于获取可以添加的角色信息

- **addRoleToUser(Long userId,Long[] ids)**
  - 该方法用于在用户与角色之间建立关系，参数userId代表要操作的用户id,参数ids代表的是角色id数组.主要操作的是 `insert into user_role`
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706152358.png)

### 1.2 UserController

```java
 /**
     * 查询用户可以添加的角色
     */
    @Override
    public List<Role> findOtherRoles(String userId) {
        return userDao.findOtherRoles(userId);
    }
```

```java
  /**
     * 给用户添加角色
     */
    @Override
    public void addRoleToUser(String userId, String[] roleIds) {
      for (String roleId:roleIds){
          userDao.addRoleToUser(userId,roleId);
      }
    }
```



### 1.3 UerService

```java
 /**
     * 查询用户可以添加的角色
     */
    @Override
    public List<Role> findOtherRoles(String userId) {
        return userDao.findOtherRoles(userId);
    }

    /**
     * 给用户添加角色
     */
    @Override
    public void addRoleToUser(String userId, String[] roleIds) {
      for (String roleId:roleIds){
          userDao.addRoleToUser(userId,roleId);
      }
    }
```

### 1.4 UserDao

```java
   @Select("select * from role where id not in (select roleId from users_role where userId = #{userId})")
    List<Role> findOtherRoles(String userId);


    @Insert("insert into users_role(userId,roleId) values(#{userId},#{roleId})")
    void addRoleToUser(@Param("userId") String userId, @Param("roleId") String roleId);
```

## 2. 角色关联权限

### 2.1 RoleController
```java
/**
     * 查询当前角色可添加的权限
     */
    @RequestMapping(path = "findRoleByIdAndAllPermission.do")
    public ModelAndView findRoleByIdAndAllPermission(@RequestParam(name = "id",required = true) String roleId) throws Exception{
        ModelAndView mv = new ModelAndView();
        Role role = roleService.findById(roleId);
         List<Permission>  permissionList = roleService.findOtherPermissions(roleId);
         mv.addObject("role",role);
         mv.addObject("permissionList",permissionList);
         mv.setViewName("role-permission-add");

        return mv;
    }
```

```java
 /**
     * 添加权限
     */
    @RequestMapping(path = "addPermissionToRole.do")
    public String addPermissionToRole(@RequestParam(name = "roleId",required = true) String roleId,@RequestParam(name = "ids",required = true) String[] permissionIds) throws Exception{
        System.out.println(roleId);
        roleService.addPermissionToRole(permissionIds,roleId);
        return "redirect:findAll.do";
    }
```
### 2.2 RoleService

```java
  /**
     *查询角色可以添加的权限
     */
    @Override
    public List<Permission> findOtherPermissions(String roleId) throws  Exception{
        return roleDao.findOtherPermissions(roleId);
    }

    /**
     * 添加权限
     */
    @Override
    public void addPermissionToRole(String[] permissionIds,String roleId) {
        for (String permissionId:permissionIds){
            roleDao.addPermissionToRole(permissionId,roleId);
        }

```

### 2.3 RoleDao

```java
   @Select("select * from permission where id not in (select permissionId from role_permission where roleId=#{roleId})")
    List<Permission> findOtherPermissions(String roleId)throws  Exception;

    @Insert("insert into role_permission(permissionId,roleId) values(#{permissionId},#{roleId})")
    void addPermissionToRole(@Param("permissionId") String permissionId,@Param("roleId") String roleId);
```


<br>
<br>

---

##  <font color=#CA2C68>3. 服务器端方法级权限控制</font>


### 3.1.开启注解使用
- 配置文件

```xml
<security:global-method-security jsr250-annotations="enabled"/>
<security:global-method-security secured-annotations="enabled"/>
<security:global-method-security pre-post-annotations="disabled"/>
```

- 注解开启
`@EnableGlobalMethodSecurity` ：Spring Security默认是禁用注解的，要想开启注解，需要在继承
`WebSecurityConfigurerAdapter`的类上加 @EnableGlobalMethodSecurity注解，并在该类中将
`AuthenticationManager`定义为Bean。

### 3.2 JSR-250注解
使用步骤一：导入pom依赖

```xml
   <dependency>
            <groupId>javax.annotation</groupId>
            <artifactId>jsr250-api</artifactId>
            <version>1.0</version>
   </dependency>
```

使用步骤二：spring=security.xml 配置

```xml
<!--    开启就是js250方法级别的权限控制-->
    <security:global-method-security jsr250-annotations="enabled"></security:global-method-security>
```

使用步骤三：在方法上添加权限限制的注解

```java
 /**
     * 查询所有产品
     */
    @RolesAllowed("ADMIN")
    @RequestMapping(path = "/findAll.do")
    public ModelAndView findAll() throws Exception {
        System.out.println("web....");

        ModelAndView mv = new ModelAndView();
        List<Product> products = productService.findAll();
        mv.addObject("productList",products);
        mv.setViewName("product_list"); //指定视图
        for (Product product :products){
            System.out.println(product);
        }

        return mv;
    }
```
注意：
 这里的 ` @RolesAllowed("ADMIN")`表示只用ADMIN角色的用户才可以访问该方法
- **@RolesAllowed** 表示访问对应方法时所应该具有的角色

>  示例：  @RolesAllowed({"USER", "ADMIN"}) 该方法只要具有"USER", "ADMIN"任意一种权限就可以访问。
>  这里可以省略前缀ROLE_，实际的权限可能是ROLE_ADMIN

- **@PermitAll** 表示允许所有的角色进行访问，也就是说不进行权限控制
- **@DenyAll** 是和PermitAll相反的，表示无论什么角色都不能访问

### 3.3 @Secured注解
- @Secured注解标注的方法进行权限控制的支持，其值默认为disabled。

> 示例： 
> @Secured("IS_AUTHENTICATED_ANONYMOUSLY") 
> public Account readAccount(Long id); 
> <br>
> @Secured("ROLE_TELLER")

使用步骤同上：只是不用额外导入依赖，springframework.sercurty 依赖中已经包含
1. spring-security.xml 开启@Secured注解支持
```xml
 <security:global-method-security secured-annotations="enabled"></security:global-method-security>
```


2. 使用@Secured 作用在方法上

```java
  @Secured("ROLE_ADMIN")
   @RequestMapping(path = "/findAll.do")
    public ModelAndView findAll() throws Exception {
    }
```


### 3.4 支持表达式的注解

第一步：开启

```xml
<security:global-method-security pre-post-annotations="enabled" ></security:global-method-security>
```

第二步：使用

```java
 /**
     * 查询所有产品
     */
  @PreAuthorize("hasRole('ROLE_USER')")
    @RequestMapping(path = "/findAll.do")
    public ModelAndView findAll() throws Exception {
    }

 /**
     * 保存产品
     */
    @PreAuthorize("authentication.principal.username == 'tom'")
    @RequestMapping(path = "/save.do")
    public String save(Product product) throws Exception {
}
```
上述注解表达式表示，所有 `ROLE_USER` 都可以查询产品，但只有 用户名为 `tom`的用户可以添加产品。

---
## <font color=#CA2C68>4. 页面端标签控制权限 </font>
### 4.1 security:authentication
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706122405.png)
**第一步： 导入pom依赖**

```xml
 <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-taglibs</artifactId>
            <version>5.0.1.RELEASE</version>
        </dependency>
```

**第二步：在jsp页面上使用 security:authentication**

```java

<%@ taglib prefix="security" uri="http://www.springframework.org/security/tags" %>
```

显示当前在线的用户名
```java
<security:authentication property="principal.username"></security:authentication>
```

设置只有ADMIN角色的用户，页面上的部分内容才可见。其他角色不可见。
```html
<security:authorize access="hasRole('ADMIN')">

			<li id="system-setting">
				<a href="${pageContext.request.contextPath}/user/findAll.do"> 
					<i class="fa fa-circle-o"></i> 用户管理
				</a>
			</li>

</security:authorize>

```


![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706122410.png)

**第三步：关于页面上的前提配置**
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706122416.png)

```xml
 <!--
    	配置具体的规则
    	auto-config="true"	不用自己编写登录的页面，框架提供默认登录页面
    	use-expressions="false"	是否使用SPEL表达式（没学习过）
    -->
    <security:http auto-config="true" use-expressions="true">
        <!-- 配置具体的拦截的规则 pattern="请求路径的规则" access="访问系统的人，必须有ROLE_USER的角色" -->
        <security:intercept-url pattern="/**" access="hasAnyRole('ROLE_USER','ROLE_ADMIN')"/>
```





# 八、后台权限管理：AOP记录日志

## 1. AOP日志
---
### 1.1 日志表信息描述sysLog

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706123004.png)

### 1.2 sql语句

```sql
CREATE TABLE sysLog(
id VARCHAR2(32) default SYS_GUID() PRIMARY KEY,
visitTime timestamp,
username VARCHAR2(50),
ip VARCHAR2(30),
url VARCHAR2(50),
executionTime int,
method VARCHAR2(200)
)
```
### 1.3.实体类

```java
public class SysLog {
private String id;
private Date visitTime;
private String visitTimeStr;
private String username;
private String ip;
private String url;
private Long executionTime;
private String method;
    //省略get、set方法
｝
```

## 2.基于AOP日志处理
---

### 2.1 切面类LogAop
**注意**：详细可以自行复习 反射、AOP面向切面编程

**思考：**
在切面类中我们需要获取登录用户的username，还需要获取ip地址，我们怎么处理？

- username获取
- SecurityContextHolder获取
- ip地址获取

> ip地址的获取我们可以通过request.getRemoteAddr()方法获取到。
> 在Spring中可以通过RequestContextListener来获取request或session对象。

**思路分析：**
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706143754.png)



```java
@Component
@Aspect
public class LogAop {

    @Autowired
    private HttpServletRequest httpServletRequest;

    @Autowired
    private ISysLogService sysLogService;

    private Date startTime; // 访问时间
    private Class executionClass;// 访问的类
    private Method executionMethod; // 访问的方法


    /**
     * 前置通知
     * 获取开始时间、执行的类是哪一个、访问的方法是哪一个
     */
    @Before("execution(* code.controller.*.*(..))")
    public void doBefore(JoinPoint jp) throws NoSuchMethodException {
        startTime = new Date();//当前时间就是开始访问时间
        executionClass = jp.getTarget().getClass();//具体要访问的类
       String methodName = jp.getSignature().getName();//获取访问的方法名
        Object[] args = jp.getArgs();//获取访问方法的参数

        //获取method对象
        if (args == null|| args.length == 0){
            executionMethod = executionClass.getMethod(methodName);
        }else {
            //有参数----解析参数的类，就将args中所有元素遍历，获取对应的Class，装入到一个Class[]
            Class[] classArgs = new Class[args.length];
            for (int i = 0; i < args.length; i++){
                classArgs[i] = args[i].getClass();
            }
            //获取有参数的方法对象
            executionMethod = executionClass.getMethod(methodName,classArgs);
        }

    }

    /**
     * 后置通知
     */
    @After("execution(* code.controller.*.*(..))")
    public void doAfter(JoinPoint jp) throws Exception {
        //1. 获取访问时长
        Long executionTime = new Date().getTime() - startTime.getTime();

        //2.获取url
        String url ="";
        if (executionClass != null && executionMethod !=null && executionClass != LogAop.class){
            RequestMapping classAnnotation = (RequestMapping)executionClass.getAnnotation(RequestMapping.class);
            if (classAnnotation != null){
                String[] classValue = classAnnotation.value();
                RequestMapping methodAnnotation = executionMethod.getAnnotation(RequestMapping.class);
                if (methodAnnotation != null){
                    String[] methodValue = methodAnnotation.value();
                    url = classValue[0] + methodValue[0];
                }
            }
        }
        //3.获取访问的ip地址(request对象)
        String ip = httpServletRequest.getRemoteAddr();

        //4.获取当前操作的用户

        //4.1 可以通过securityContext获取
        //4.2 也可以从request.getSession中获取,request.getSession().getAttribute("SPRING_SECURITY_CONTEXT")
        SecurityContext context = SecurityContextHolder.getContext();
        User user = (User) context.getAuthentication().getPrincipal();
        String username = user.getUsername();


        //5.将信息封装到sysLog对象
        SysLog sysLog = new SysLog();
        sysLog.setExecutionTime(executionTime);
        sysLog.setIp(ip);
        sysLog.setMethod("[类名] "+executionClass.getName()+"[方法名] "+executionMethod.getName());
        sysLog.setUrl(url);
        sysLog.setUsername(username);
        sysLog.setVisitTime(startTime);
        // 调用Service，调用dao将sysLog insert数据库
        sysLogService.save(sysLog);

    }

}

```

```java
public interface ISysLogDao {

    @Insert("insert into syslog(visitTime,username,ip,url,executionTime,method) values(#{visitTime},#{username},#{ip},#{url},#{executionTime},#{method})")
    void save(SysLog sysLog);
}
```

**注意：**
- 请求controller时如果访问的方法有参数时，必须是包装类型的参数比如 `Integer`,不可以是 `int`,因为`通过反射获取`方法对象时会报错。
- Controller 的 @RequestMapping(value = "/findAll.do") ，必须 指明 `value属性`,因为在上面的代码上，解析url时  String[] classValue = classAnnotation.value(); 、 String[] methodValue = methodAnnotation.value(); 都获取的是注解的 value 属性


**测试结果：**
用户登录后每访问一次服务器的方法，都会在数据库中增加一条记录，记录了用户的访问时间、访问时长、访问的url、ip地址、用户名等日志信息。

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706152203.png)

### 2.2  通过页面查看日志
**思路分析**：
![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706152209.png)

**SysLogController**

```java
@Controller
@RequestMapping(value = "/sysLog")
public class SysLogController {

    @Autowired
    private ISysLogService sysLogService;

    @RequestMapping(value = "/findAll.do")
    public ModelAndView findAll()throws Exception{
        ModelAndView mv = new ModelAndView();
        List<SysLog> sysLogs = sysLogService.findAll();
        mv.addObject("sysLogs",sysLogs);
        mv.setViewName("syslog-list");
        return mv;
    }

}
```
**ISysLogDao**
```java
    @Select("select * from syslog")
    @Results({
            @Result(id = true,property = "id",column = "id"),
            @Result(property = "visitTime",column = "visitTime"),
            @Result(property = "username",column = "username"),
            @Result(property = "ip",column = "ip"),
            @Result(property = "url",column = "url"),
            @Result(property = "executionTime",column = "executionTime"),
            @Result(property = "method",column = "method")

    })
    List<SysLog> findAll();
```

![在这里插入图片描述](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706152221.png)







