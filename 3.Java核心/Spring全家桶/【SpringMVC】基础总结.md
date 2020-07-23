【SpringMVC】基础总结



# （一）框架介绍概述



## 1. 什么是 SpringMVC ?
SpringMVC框架是以请求为驱动，围绕Servlet设计，将请求发给控制器，然后通过模型对象，分派器来展示请求结果视图。其中核心类是DispatcherServlet，它是一个Servlet，顶层是实现的Servlet接口。
MVC 全名是 `Model View Controller`，是`模型(model)－视图(view)－控制器(controller)`的缩写，是一种用于设计创建 Web 应用程序表现层的模式。MVC 中每个部分各司其职：
`Model （模型） ：`

 - 通常指的就是我们的数据模型。作用一般情况下用于封装数据。


`View （视图） ：`

 - 通常指的就是我们的 jsp 或者 html。作用一般就是展示数据的。通常视图是依据模型数据创建的。


`Controller （控制器） ：`

 - 是应用程序中处理用户交互的部分。作用一般就是处理程序逻辑的。

它相对于前两个不是很好理解，这里举个例子：
 例如：
我们要保存一个用户的信息，该用户信息中包含了姓名，性别，年龄等等。
这时候表单输入要求年龄必须是 1~100 之间的整数。姓名和性别不能为空。并且把数据填充
到模型之中。
此时除了 js 的校验之外，服务器端也应该有数据准确性的校验，那么`校验就是控制器的该做的`。
当校验失败后，由控制器负责把错误页面展示给使用者。
如果校验成功，也是控制器负责`把数据填充到模型`，并且`调用业务层`实现完整的业务需求。


## 2. 三层架构模型

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705232523.png" alt="在这里插入图片描述" style="zoom:100%;" />


## 3. SpringMVC执行流程原理
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705232513.png" alt="在这里插入图片描述" style="zoom:100%;" />



## 4. SpringMVC的优势
1、清晰的角色划分：
- 前端控制器（DispatcherServlet）
- 请求到处理器映射（HandlerMapping）
- 处理器适配器（HandlerAdapter）
- 视图解析器（ViewResolver）
- 处理器或页面控制器（Controller）
- 验证器（ Validator）
- 命令对象（Command 请求参数绑定到的对象就叫命令对象）
- 表单对象（Form Object 提供给表单展示和提交到的对象就叫表单对象）。

2、分工明确，而且扩展点相当灵活，可以很容易扩展，虽然几乎不需要。
3、由于命令对象就是一个 POJO，无需继承框架特定 API，可以使用命令对象直接作为业务对象。
4、和 Spring 其他框架无缝集成，是其它 Web 框架所不具备的。
5、`可适配`，通过 `HandlerAdapter` 可以支持任意的类作为处理器。
6、可定制性，HandlerMapping、ViewResolver 等能够非常简单的定制。
7、功能强大的数据验证、格式化、绑定机制。
8、利用 Spring 提供的 `Mock `对象能够非常简单的进行 Web 层单元测试。
9、本地化、主题的解析的支持，使我们更容易进行国际化和主题的切换。
10、强大的 JSP 标签库，使 JSP 编写更容易。
………………还有比如RESTful风格的支持、简单的文件上传、约定大于配置的契约式编程支持、基于注解的零配置支持等等。

## 5. SpringMVC 和 和 Struts2  的优略分析
**共同点：**
它们都是表现层框架，都是基于 MVC 模型编写的。
它们的底层都离不开原始 ServletAPI。
它们处理请求的机制都是一个核心控制器。
**区别：**
`Spring MVC 的入口是 Servlet, 而 Struts2 是 Filter`

Spring MVC 是`基于方法`设计的，而 Struts2 是`基于类`，Struts2 每次执行都会创建一个动作类。所
以 Spring MVC 会稍微比 Struts2 快些。

Spring MVC 使用更加简洁,同时还支持 JSR303, 处理 ajax 的请求更方便

(JSR303 是一套 JavaBean 参数校验的标准，它定义了很多常用的校验注解，我们可以直接将这些注
解加在我们 JavaBean 的属性上面，就可以在需要校验的时候进行校验了。)

Struts2 的 OGNL 表达式使页面的开发效率相比 Spring MVC 更高些，但执行效率并没有比 JSTL 提
升，尤其是 struts2 的表单标签，远没有 html 执行效率高。





# （二）简单入门案例

## 1. 入门程序的需求

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705232635.png" alt="在这里插入图片描述" style="zoom:100%;" />
我的工程目录

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705232648.png" alt="在这里插入图片描述" style="zoom:100%;" />





## 2. 代码


### 2.1 创建web工程，修改pom.xml

```xml
 <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <!--  spring的版本锁定-->
    <spring.version>5.0.2.RELEASE</spring.version>
  </properties>


  <dependencies>
    
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

  </dependencies>
```


### 2.2 配置DispatcherServlet：核心的控制器
1. 在`web.xml`配置文件中核心控制器DispatcherServlet

```xml
<web-app>
  <display-name>Archetype Created Web Application</display-name>
  
  <servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:springmvc.xml</param-value>
    </init-param>

    <load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
  
  
</web-app>

```


###  2.3 springmvc.xml

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

<!--开启注解扫描-->
<context:component-scan base-package="code"></context:component-scan>

<!--    视图解析器对象-->
<bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/pages/"></property>
    <property name="suffix" value=".jsp"></property>

</bean>


<!--    开启注解SpringMVC框架注解的支持-->
    <mvc:annotation-driven />
</beans
```

### 4.index.jsp和HelloController

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>入门案例</title>
</head>
<body>

<h3>入门程序</h3>
<a href="hello">入门程序：hello</a>
</body>
</html>
```


```java
//控制器
@Controller
public class HelloController {

    //请求映射
    @RequestMapping(path = "/hello")
    public String sayHello(){
        System.out.println("hello springMVC!");
        return "success";
    }
}
```


### 5.在WEB-INF：success.jsp


```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>成功页面</title>
</head>
<body>
<h3>入门成功了！</h3>
</body>
</html>
```
###  6. 启动Tomcat服务器，进行测试






## 3. 过程分析

 1. 当启动Tomcat服务器的时候，因为配置了`load-on-startup`标签，所以会创建`DispatcherServle`t对象，
就会加载`springmvc.xml`配置文件


2. `开启了注解扫描`，那么HelloController对象就会被创建
3. 从index.jsp发送请求，请求会`先到达DispatcherServlet核心控制器`，根据配置@RequestMapping注解找到执行的具体方法
4. 根据执行方法的返回值，再根据配置的`视图解析器`，去指定的目录下查找指定名称的`JSP文件`
5. Tomcat服务器渲染页面，做出响应

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705233607.png" alt="在这里插入图片描述" style="zoom:100%;" />

执行流程：
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705233616.png" alt="在这里插入图片描述" style="zoom:100%;" />


### 3.1 入门案例中涉及的组件
- **DispatcherServlet：前端控制器**
用户请求到达前端控制器，它就相当于 mvc 模式中的 c，dispatcherServlet 是整个流程控制的中心，由它调用其它组件处理用户的请求，dispatcherServlet 的存在降低了组件之间的耦合性。

- **HandlerMapping：处理器映射器**
 HandlerMapping 负责根据用户请求找到 Handler 即处理器，SpringMVC 提供了不同的映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。

- **Handler ：处理器**
它就是我们开发中要编写的`具体业务控制器`。由 DispatcherServlet 把用户请求转发到 Handler。Handler 对具体的用户请求进行处理。

- **HandlAdapter ：处理器适配器**
通过 HandlerAdapter 对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行。

-  **View Resolver ：视图解析器**
View Resolver 负责将处理结果生成 View 视图，View Resolver 首先根据逻辑视图名解析成物理视图名即具体的页面地址，再生成 View 视图对象，最后对 View 进行渲染将处理结果通过页面展示给用户。

- **View ：视图**
SpringMVC 框架提供了很多的 View 视图类型的支持，包括：jstlView、freemarkerView、pdfView等。我们最常用的视图就是 jsp。
一般情况下需要通过页面标签或页面模版技术将模型数据通过页面展示给用户，需要由程序员根据业务需求开发具体的页面。

---


### 3.2 mvc:annotation-driven 注解说明
springmvc.xml文件中：
```xml
<!--    开启注解SpringMVC框架注解的支持-->
    <mvc:annotation-driven />
```

在 SpringMVC 的各个组件中，处理器映射器、处理器适配器、视图解析器称为 SpringMVC 的三大组件。使 用 <mvc:annotation-driven> `自动加载 `RequestMappingHandlerMapping （处理映射器） 和
RequestMappingHandlerAdapter （ 处 理 适 配 器 ） ， 可 用 在 SpringMVC.xml 配 置 文 件 中 使 用<mvc:annotation-driven>替代注解处理器和适配器的配置。

它就相当于在 xml 中配置了：
 上面的标签相当于 如下配置（了解即可）


```java
<!-- Begin -->
<!-- HandlerMapping -->
<bean
class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerM
apping"></bean>
<bean
class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"></bean>
<!-- HandlerAdapter -->
<bean
class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerA
dapter"></bean>
<bean
class="org.springframework.web.servlet.mvc.HttpRequestHandlerAdapter"></bean>
<bean
class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"></bean>
<!-- HadnlerExceptionResolvers -->
<bean
class="org.springframework.web.servlet.mvc.method.annotation.ExceptionHandlerExcept
ionResolver"></bean>
<bean
class="org.springframework.web.servlet.mvc.annotation.ResponseStatusExceptionResolv
er"></bean>
<bean
class="org.springframework.web.servlet.mvc.support.DefaultHandlerExceptionResolver"
></bean>
<!-- End -->

```


### 3.3 RequestMapping 注解说明

作用：
用于建立请求 URL 和处理请求方法之间的对应关系。
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705233625.png" alt="在这里插入图片描述" style="zoom:100%;" />

属性：
value：用于指定请求的 URL。它和 `path `属性的作用是一样的。

method：用于指定请求的方式。

params：用于指定限制请求参数的条件。它支持简单的表达式。要求请求参数的 key 和 value 必须和配置的一模一样。

例如：
params = {"accountName"}，表示请求参数必须有 accountName
params = {"moeny!100"}，表示请求参数中 money 不能是 100。
headers：用于指定限制请求消息头的条件。

```java
    @RequestMapping(value = "/testRequestMapping",method= RequestMethod.GET,params = {"username"},headers = {"Accept"})

```

注意：
以上四个属性只要出现 2 个或以上时，他们的关系是与&&的关系。





# （三）请求参数的绑定及自定义类型转换

## <font color=#009C41>1. 绑定的机制 </font>

**什么是请求参数的绑定？**

通过客户端发送的请求中都会携带一些参数 比如 username=zhangsan ，服务器端把这些参数拿到就是参数的绑定，类似于曾经的web工程里的` Request.getParameter 方法`。但是我们使用了 springMVC框架以后就不需要那么麻烦了~不用我们手动一个个 `g   e  t   (lll￢ω￢)`

**那么SpringMVC是如何进行参数绑定的呢？**

答：我们都知道，表单中请求参数都是基于` key=value `的。
SpringMVC 绑定请求参数的过程是通过把表单提交请求参数，作为控制器中`方法参数`进行绑定的。
例如：

```java
<a href="account/findAccount?username=hehe&&password=123">查询用户</a>
超链接中请求参数是：
username=hehe&&password=123

/**
* 查询账户
* @return
*/
@RequestMapping("/findAccount")
public String findAccount(String username，String password) {
System.out.println("查询了用户。。。。"+username);
return "success";
}
```

## <font color=#009C41>2. 支持的数据类型 </font>
<font color=#F1892D>基本类型参数 ：</font>

 - 包括基本类型和 String 类型

<font color=#F1892D>基本POJO  类型参数 :</font>

 - 包括实体类，以及关联的实体类

<font color=#F1892D>数组和集合类型参数 ：</font>

- 包括 List 结构和 Map 结构的集合（包括数组）

## <font color=#009C41>3. 参数绑定的使用要求 </font>
SpringMVC 绑定请求参数是自动实现的，但是要想使用，必须遵循使用要求。

1. `如果是基本类型或者 String 类型：`
要求我们的参数名称必须和控制器中方法的形参名称保持一致。(严格区分大小写)

2. ` 如果是 POJO  类型 ，或者它的关联对象 ：`
要求表单中参数名称和 POJO 类的属性名称保持一致。并且控制器方法的参数类型是 POJO 类型。

3.  `如果是集合类型, 有两种方式`
 `第一种：`
要求集合类型的请求参数必须在 POJO 中。在表单中请求参数名称要和 POJO 中集合属性名称相同。
给 List 集合中的元素赋值，使用下标。
给 Map 集合中的元素赋值，使用键值对。
` 第二种：`
接收的请求参数是 json 格式数据。需要借助一个注解实现。

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720132031.png" alt="在这里插入图片描述" style="zoom:100%;" />


## ▶ 4. Request中文乱码解决
web.xml中添加如下配置：

```xml
<!--  配置解决中文乱码的过滤器-->
  <filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
<!--    初始化参数-->
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
  </filter>
  <filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>
```


## 5. 自定义类型转换器
问题：
1. 表单提交的任何数据类型全部都是字符串类型，但是后台定义Integer类型，数据也可以封装上，说明Spring框架内部会默认进行数据类型转换。
2. 前台日期的输入格式为 "2020/3/3" 时同样可以实现 从String 类型到 Data类型的转换。但是如果前台文本框输入的是“2020-3-3” 时的格式就会和Data类型不匹配，造成出错。（Bad Request）
---



1. **第一步：如果想自定义数据类型转换，可以实现Converter的接口**


```java
public class StringToDataConverter implements Converter<String, Date> {

    @Override
    public Date convert(String source) {
        if(source == null){
            throw new RuntimeException("参数不能为空");
        }
        SimpleDateFormat df=new SimpleDateFormat("yyyy-mm-dd");
        try {
           return df.parse(source);
        } catch (ParseException e) {
            throw new RuntimeException("类型转换时出错");
        }
    }
}

```
2.  **第二步：注册自定义类型转换器，在springmvc.xml配置文件中编写配置**

```xml

<!--    自定义类型转换器-->
    <bean id="conversionService" class="org.springframework.context.support.ConversionServiceFactoryBean">
        <property name="converters">
            <set>
                <bean class="code.utils.StringToDataConverter"></bean>
            </set>
        </property>
    </bean>


<!--    开启注解SpringMVC框架注解的支持-->
    <mvc:annotation-driven conversion-service="conversionService"/>
```



## 6. 获取Servlet原生API

 在控制器中使用原生的ServletAPI对象

 只需要在控制器的方法参数定义HttpServletRequest和HttpServletResponse对象


```java
  @RequestMapping(path = "/testServlet",method = RequestMethod.GET)
    public String testServlet(HttpServletRequest request, HttpServletResponse response){
        System.out.println("testServlet。。。。");

        System.out.println("request "+request);
        HttpSession session = request.getSession();
        System.out.println("session "+session);

        System.out.println("response "+response);
        return "success";
    }
```

# （四）常用的注解

##  <font color=#E67E22>    1. RequestParam   </font>

作用：把请求中的指定名称的参数传递给控制器中的形参赋值
属性
- value：请求参数中的名称
- required：请求参数中是否必须提供此参数，默认值是true，必须提供
代码如下：
注意：required默认true，表示前台传入的参数的名称必须叫做`name`

```html
<a href="anno/testRequestParam?name=哈哈" >RequestParam </a>
```

```java
@RequestMapping(path = "/anno")
public class AnnoController {

    @RequestMapping(path = "/testRequestParam")
    public String testRequestParam(@RequestParam(value = "name") String username){
        System.out.println("testRequestParam 控制器方法执行了。。。");
        System.out.println(username);
        return "success";
    }
}

```



----
<br>


##  <font color=#E67E22>     2. RequestBody注解   </font>
**作用：**
用于获取请求体内容。直接使用得到是 key=value&key=value...结构的数据。
get 请求方式不适用。
**属性：**
required：是否必须有请求体。默认值是:true。当取值为 true 时,get 请求方式会报错。如果取值
为 false，get 请求得到是 null。

```html
<h5>RequestBody</h5>
<form action="anno/testRequestBody" method="post">
    <input type="text" name="username" /><br>
    <input type="text" name="age" /><br>
    <input type="submit" value="提交"/>
</form>

```

```java
 @RequestMapping(path = "/testRequestBody")
    public String testRequestBody(@RequestBody String body){
        System.out.println("testRequestBody 控制器方法执行了。。。");
        System.out.println(body);
        return "success";
    }
```



##  <font color=#E67E22>   3. PathVariable注解 </font>

**作用：**
用于绑定 url 中的占位符。例如：请求 url 中` /delete/{id}`，这个`{id}`就是` url 占位符`。
url 支持占位符是 spring3.0 之后加入的。是 springmvc 支持 rest 风格 URL 的一个重要标志。
**属性：**

- value：用于指定 url 中占位符名称。
- required：是否必须提供占位符。

```java
<a href="anno/testPathVariable/100" >PathVariable 注解 </a>
```

```java
 @RequestMapping(path = "/testPathVariable/{sid}")
    public String testPathVariable(@PathVariable(value = "sid" ) String id){
        System.out.println("testPathVariable 控制器方法执行了。。。");
        System.out.println(id);
        return "success";
    }
```

注：
[REST  风格 的URL请求](https://blog.csdn.net/a909301740/article/details/80587580)



----
<br>

##  <font color=#E67E22>  4. HiddentHttpMethodFilter过滤器 </font>
**作用：**
由于浏览器 form 表单只支持 GET 与 POST 请求，而 DELETE、PUT 等 method 并不支持，Spring3.0 添加了一个过滤器，可以将浏览器请求改为指定的请求方式，发送给我们的控制器方法，使得支持 GET、POST、PUT与 DELETE 请求。

**使用方法：**
1. 第一步：在 web.xml 中配置该过滤器。
2. 第二步：请求方式必须使用 post 请求。
3. 第三步：按照要求提供_method 请求参数，该参数的取值就是我们需要的请求方式。

源码分析：
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705234905.png" alt="在这里插入图片描述" style="zoom:100%;" />
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705234914.png" alt="在这里插入图片描述" style="zoom:100%;" />

----
<br>

##  <font color=#E67E22> 5. RequestHeader注解 </font>
**作用：**
用于获取请求消息头。
**属性：**
value：提供消息头名称
required：是否必须有此消息头
 注：在实际开发中一般不怎么用。

```html
<!-- RequestHeader 注解 -->
<a href="springmvc/useRequestHeader">获取请求消息头</a>
```

```java
@RequestMapping("/useRequestHeader")
public String  useRequestHeader(@RequestHeader(value="Accept-Language",required=false)String requestHeader){
System.out.println(requestHeader);
return "success";
}
```

----
<br>

##  <font color=#E67E22> 6. CookieValue注解 </font>
**作用：**
用于把指定 cookie 名称的值传入控制器方法参数。
**属性：**
value：指定 cookie 的名称。
required：是否必须有此 cookie。

```html
<a href="anno/testCookieValue" >CookieValue 注解 </a>

```

```java
 @RequestMapping(path = "/testCookieValue")
    public String testCookieValue(@CookieValue(value = "JSESSIONID") String cookieValue){
        System.out.println("testCookieValue 控制器方法执行了。。。");
        System.out.println(cookieValue);
        return "success";
    }

```



----
<br>


##  <font color=#E67E22> 7. ModelAttribute注解</font>
**作用：**
该注解是 SpringMVC4.3 版本以后新加入的。它可以用于`修饰方法和参数。`

1. 出现在方法上，表示当前方法会在控制器的方法执行`之前`，先执行。它可以修饰没有返回值的方法，也可以修饰有具体返回值的方法。

2. 出现在参数上，获取指定的数据给参数赋值。

**属性：**
- value：用于获取数据的 key。key 可以是 POJO 的属性名称，也可以是 map 结构的 key。

**应用场景：**
当表单提交数据不是完整的实体类数据时，保证没有提交数据的字段使用数据库对象原来的数据。
例如：
我们在编辑一个用户时，用户有一个创建信息字段，该字段的值是不允许被修改的。在提交表单数
据时肯定没有此字段的内容，一旦更新会把该字段内容置为 null，此时就可以使用此注解解决问题。

<font color=#CA2C68>作用在方法上，会先于控制器方法执行 </font>

```java
<form action="anno/testModelAttribute" method="post">
    <input type="text" name="uname" /><br>
    <input type="text" name="age" /><br>
    <input type="submit" value="提交"/>
</form>
```

```java
  @RequestMapping(path = "/testModelAttribute")
    public String testModelAttribute(User user){
        System.out.println("testModelAttribute 控制器方法执行了。。。");
        System.out.println(user);
        return "success";
    }

    @ModelAttribute
    public User showUser(String uname){
        System.out.println("showUser 方法执行了。。。");
        User user=new User();
        user.setUname(uname);
        user.setAge(30);
        user.setBirthday(new Date("1988/8/8"));

        return user;
    }

```
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705234946.png" alt="在这里插入图片描述" style="zoom:100%;" />
<font color=#CA2C68>作用在参数上 </font>


 修饰的方法没有返回值，将对象存入一个map键值对集合。详看代码示例。


```java
 @RequestMapping(path = "/testModelAttribute")
    public String testModelAttribute(@ModelAttribute(value = "abc") User user){
        System.out.println("testModelAttribute 控制器方法执行了。。。");
        System.out.println(user);
        return "success";
    }

    @ModelAttribute
    public void showUser(String uname, Map<String,User> map){
        System.out.println("showUser 方法执行了。。。");
        User user=new User();
        user.setUname(uname);
        user.setAge(30);
        user.setBirthday(new Date("1988/8/8"));

      map.put("abc",user);
    }
```

----
<br>


##  <font color=#E67E22>  8. SessionAttributes注解</font>
 **作用**：将数据存到request域或者Session域，用于多次执行控制器方法间的参数共享。
 **属性**：value：指定存入属性的名称。

` 注意：该注解作用位置只能作用于类上。`

<font color=#CA2C68>**示例代码1：setSessionAttributes 数据存入session**</font>

```java
 @RequestMapping(path = "/setSessionAttributes")
    public String setSessionAttributes(Model model){
        System.out.println("setSessionAttributes 控制器方法执行了。。。");
        
        //model底层会存储到request域对象中
        model.addAttribute("msg","呵呵");

        return "success";
    }
```

success.jsp页面使用EL表达式从域中获取数据展示

```html
request域: ${requestScope.msg}
<br>
<br>
<br>
session域：${sessionScope}
```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705235407.png" alt="在这里插入图片描述" style="zoom:100%;" />


<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200705235401.png" alt="在这里插入图片描述" style="zoom:100%;" />

<font color=#CA2C68>  **示例代码2：getSessionAttributes  从session获取** </font>

```java
  @RequestMapping(path = "/getSessionAttributes")
    public String getSessionAttributes(ModelMap map){
        System.out.println("getSessionAttributes 控制器方法执行了。。。");
        String msg = (String)map.get("msg");
        System.out.println(msg);
        return "success";
    }
```
注：ModelMap是Model接口的实现类


<font color=#CA2C68>  **示例代码3：deleteSessionAttributes 从session中清除**  </font>

```java
@RequestMapping(path = "/deleteSessionAttributes")
    public String deleteSessionAttributes(SessionStatus status){
        System.out.println("deleteSessionAttributes 控制器方法执行了。。。");

        status.setComplete(); //设置完成，等于本次操作完成，数据从session中清楚
        return "success";
    }
```

## 9. ResponseBody

待补充

`@ResponseBody`的作用其实是将java对象转为json格式的数据。

> `@responseBody`注解的作用是将`controller`的方法返回的对象通过适当的转换器转换为指定的格式之后，写入到response对象的body区，通常用来返回JSON数据或者是XML数据。
> 注意：在使用此注解之后不会再走视图处理器，而是直接将数据写入到输入流中，他的效果等同于通过response对象输出指定格式的数据。

@ResponseBody是作用在方法上的，@ResponseBody 表示该方法的返回结果直接写入 HTTP response body 中，一般在异步获取数据时使用【也就是AJAX】。

> 注意：在使用 @RequestMapping后，返回值通常解析为跳转路径，但是加上 @ResponseBody 后返回结果不会被解析为跳转路径，而是直接写入 HTTP response body 中。 比如异步获取 json 数据，加上 @ResponseBody 后，会直接返回 json 数据。@RequestBody 将 HTTP 请求正文插入方法中，使用适合的 HttpMessageConverter 将请求体写入某个对象。

https://www.cnblogs.com/tyadmin/p/11241007.html

后台 Controller类中对应的方法：

```java
@RequestMapping("/login.do")
@ResponseBody
public Object login(String name, String password, HttpSession session) {
	user = userService.checkLogin(name, password);
	session.setAttribute("user", user);
	return new JsonResult(user);
}
/**
* @RequestBody是作用在形参列表上，用于将前台发送过来固定格式的数据【xml格式 或者 json等】封装为对应的 JavaBean 对象，
* 封装时使用到的一个对象是系统默认配置的 HttpMessageConverter进行解析，然后封装到形参上。
* 如上面的登录后台代码可以改为：
*/
@RequestMapping("/login.do")
@ResponseBody
public Object login(@RequestBody User loginUuser, HttpSession session) {
	user = userService.checkLogin(loginUser);
	session.setAttribute("user", user);
	return new JsonResult(user);

}
```

# （五）响应数据和结果视图

## <font color=#005B50>1. 返回值分类 </font>
### 1.1 String类型

```java
  @RequestMapping(path = "/testString")
    public String testString(Model model){
        System.out.println("testString 方法执行了。。。");
        User user=new User();
        user.setUname("美美");
        user.setAge(2);
        user.setBirthday(new Date(2018/8/8));
        model.addAttribute("user",user);

        return "success";//使用视图解析器，跳转到success.jsp
    }

```

### 1.2 void类型
在昨天的学习中，我们知道 Servlet 原始 API 可以作为控制器中方法的参数：
1 、使用 request  转向页面
2 、也可以通过 response 
3 、也可以通过 response  指定响应结果，`例如响应 json 数据`

```html
<a href="user/testVoid">testVoid</a><br>
```


```java

    @RequestMapping(path = "/testVoid")
    public void testVoid(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("testVoid 方法执行了。。。");
        // 编写请求转发的程序
//        request.getRequestDispatcher("/WEB-INF/pages/success.jsp").forward(request,response);

        // 重定向
//        response.sendRedirect(request.getContextPath()+"/redir.jsp");

        //设置响应的中文乱码问题
        response.setCharacterEncoding("UTF-8");
        response.setContentType("text/html;charset=UTF-8");

        // 直接会进行响应
        response.getWriter().print("哈喽");

    }

```

### 1.3 ModelAndView类型
`ModelAndView` 是 SpringMVC 为我们提供的一个对象，该对象也可以用作控制器方法的返回值。
**注意：**
我们在 跳转页面上获取使用的是 requestScope.username 取的，所以`返回 ModelAndView 类型时`，浏览器跳转只能是`请求转发。`


```html
<a href="user/testModelAndView">testModelAndView</a><br>
```

```java
@RequestMapping(path = "/testModelAndView")
    public ModelAndView testModelAndView(){
        System.out.println("testModelAndView 方法执行了。。。");

        // 创建ModelAndView对象
        ModelAndView mv=new ModelAndView();
        // 模拟从数据库中查询User对象
        User user=new User();
        user.setUname("军军");
        user.setAge(5);
        user.setBirthday(new Date(2018/8/8));

        // 把user对象存到mv中，也会把user对象存入到request域对象中
        mv.addObject("user",user);

        // 跳转到哪个页面
        mv.setViewName("success");

        return mv;
    }

```

<br>

---

<br>



## <font color=#005B50> 2. 页面跳转 </font>
### 2.1 forward转发
### 2.2 redirect重定向

```java

    @RequestMapping(path = "/testForwardOrRedirect")
    public String testForwardOrRedirect(){
        System.out.println("testForwardOrRedirect 方法执行了。。。");

        // 请求的转发（从服务端出发）
//        return "forward:/WEB-INF/pages/success.jsp";

        //重定向（从客户端出发）
        return "redirect:/redir.jsp";
    }
    
```


<br>

---

<br>


## <font color=#005B50>3. ResponseBody响应 json数据</font>
###  3.1 json数据：不过滤静态资源
1. `DispatcherServlet`会拦截到所有的资源，导致一个问题就是静态资源（img、css、js）也会被拦截到，从而不能被使用。解决问题就是需要配置静态资源不进行拦截，在`springmvc.xml配置文件`添加如下配置:
-  1. mvc:resources标签配置不过滤
- 2. location元素表示webapp目录下的包下的所有文件
- 3. mapping元素表示以/static开头的所有请求路径，如/static/a 或者/static/a/b
```xml
<!--前端控制器，那些静态资源不拦截-->
    <!-- 设置静态资源不过滤 -->
    <mvc:resources location="/css/" mapping="/css/**"/> <!-- 样式 -->
    <mvc:resources location="/images/" mapping="/images/**"/> <!-- 图片 -->
    <mvc:resources location="/js/" mapping="/js/**"/> <!-- javascript -->

```





###  3.2 发送ajax的请求

```js
 <script>
    // 页面加载，绑定单击事件
        $(function () {
            $("#btn").click(function () {
                // alert("hello btn");
                // 发送Ajax请求
                $.ajax({
                    // 编写json格式，设置属性和值
                    url:"user/testAjax",
                    contentType:"application/json;charset=UTF-8",
                    data:'{"username":"hehe","password":"123","age":"20"}',
                    dataType:"json",
                    type:"post",
                    success:function (data) {
                        // data服务器响应的json的数据，进行解析
                            alert(data);
                            alert(data.uname);
                            alert(data.password);
                            alert(data.age);
                    }
                });
            });
        });

    </script>

```



###  3.3  响应json格式数据
1. 使用 `@RequestBody`获取请求体数据,
2. 使用 @RequestBody 注解把json的字符串转换成JavaBean的对象
3. 使用 `@ResponseBody `注解把JavaBean对象转换成json字符串，直接响应, 要求方法需要返回JavaBean的对象

```java
 @RequestMapping(path = "/testAjax")
    public @ResponseBody User testAjax(@RequestBody  User user){
        System.out.println("testAjax 方法执行了。。。");
        // 客户端发送Ajax的请求，传的是json字符串，后端把json字符串封装到user对象中
        System.out.println(user);
        // 做响应，模拟查询数据库
        user.setUname("haha");
        user.setAge(40);
        // 做响应
        return  user;

    }
```



`注意`： json字符串和JavaBean对象互相转换的过程中，需要使用jackson的jar包

```xml
<dependency>
<groupId>com.fasterxml.jackson.core</groupId>
<artifactId>jackson-databind</artifactId>
<version>2.9.0</version>
</dependency>
<dependency>
<groupId>com.fasterxml.jackson.core</groupId>
<artifactId>jackson-core</artifactId>
<version>2.9.0</version>
</dependency>
<dependency>
<groupId>com.fasterxml.jackson.core</groupId>
<artifactId>jackson-annotations</artifactId>
<version>2.9.0</version>
</dependency>
```





# （六）文件上传

## 1. 文件上传的回顾
1. 导入文件上传的jar包

```xml
<dependency>
<groupId>commons-fileupload</groupId>
<artifactId>commons-fileupload</artifactId>
<version>1.3.1</version>
</dependency>
<dependency>
<groupId>commons-io</groupId>
<artifactId>commons-io</artifactId>
<version>2.4</version>
</dependency>
```

2. 编写文件上传的JSP页面

```html
<form action="user/fileupload1" method="post" enctype="multipart/form-data">
    选择文件:<input type="file" name="upload"/><br>
    <input type="submit" value="上传"/>
</form>
```

3. 编写文件上传的Controller控制器

```java
@Controller
@RequestMapping(path = "/user")
public class UserController {

    @RequestMapping(path = "/fileupload1")
    public String fileupload1(HttpServletRequest request) throws Exception {
        System.out.println("文件上传1......");
        // 使用fileupload组件完成文件上传

        //上传的位置
        String path = request.getSession().getServletContext().getRealPath("/uploads/");
        //判断，该路径是否存在
        File file =new File(path);
        if (!file.exists()){
            // 创建文件夹
            file.mkdirs();
        }

        // 解析request对象，获取上传文件项
        DiskFileItemFactory factory = new DiskFileItemFactory();
        ServletFileUpload upload= new ServletFileUpload(factory);
        //解析request
        //返回文件项集合
        List<FileItem> fileItems = upload.parseRequest(request);
        //遍历
        for (FileItem item:fileItems ){
            // 进行判断，当前item对象是否是上传文件项
            if (item.isFormField()){
                //说明是普通表单项

            }else{
                //说明是上传文件项
                //获取上传文件的名称
                String filename = item.getName();

                // 把文件的名称设置唯一值，uuid
                String uuid = UUID.randomUUID().toString().replace("-","");
                filename = uuid+filename;
                //完成文件上传
                item.write(new File(path,filename));
                //删除临时文件
                item.delete();
            }
        }

        return "success";
    }


}

```
---
## 2. SpringMVC实现文件上传
springMVC框架文件上传的原理分析：

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706092107.png" alt="在这里插入图片描述" style="zoom:100%;" />

1. SpringMVC框架提供了`MultipartFile对象`，该对象表示上传的文件，要求变量名称必须和表单file标签的name属性名称相同。
2. 代码如下


```java
@RequestMapping(path = "/fileupload2")
    public String fileupload2(HttpServletRequest request, MultipartFile upload) throws Exception {
        System.out.println("文件上传2......");

        // 先获取到要上传的文件目录
        String path = request.getSession().getServletContext().getRealPath("/uploads/");
        // 创建File对象，一会向该路径下上传文件
        File file =new File(path);
        if (!file.exists()){
            // 创建文件夹
            file.mkdirs();
        }
         //获取到上传文件的名称
        String filename = upload.getOriginalFilename();

        // 把文件的名称设置唯一值，uuid
        String uuid = UUID.randomUUID().toString().replace("-","");
        filename = uuid+filename;
        // 完成文件上传
        upload.transferTo(new File(path,filename));
        return "success";
    }
```

3. 配置文件解析器对象

```xml
<!-- 配置文件解析器对象，要求id名称必须是multipartResolver -->
<bean id="multipartResolver"
class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
<property name="maxUploadSize" value="10485760"/>
</bean>
```
**注意：**
文件上传的解析器` id 是固定的`，不能起别的名称，否则无法实现请求参数的绑定。（不光是文件，其他字段也将无法绑定）

---
## 3. SpringMVC跨服务器方式文件上传
**分服务器的目的**
在实际开发中，我们会有很多处理不同功能的服务器。例如：

应用服务器：负责部署我们的应用
数据库服务器：运行我们的数据库
缓存和消息服务器：负责处理大并发访问的缓存和消息
文件服务器：负责存储用户上传文件的服务器。

`( 注意：此处说的不是服务器集群）`
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706094150.png" alt="" style="zoom:100%;" />



实现步骤
 1. 新建一个服务器fileupload，新建一个工程 fileuploadserver 。
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706095935.png" alt="在这里插入图片描述" style="zoom:100%;" />
2. 导入需要使用的jar包

```xml
<dependency>
<groupId>com.sun.jersey</groupId>
<artifactId>jersey-core</artifactId>
<version>1.18.1</version>
</dependency>
<dependency>
<groupId>com.sun.jersey</groupId>
<artifactId>jersey-client</artifactId>
<version>1.18.1</version>
</dependency>
```
3. 编写文件上传的JSP页面

```html
<form action="user/fileupload3" method="post" enctype="multipart/form-data">
    选择文件:<input type="file" name="upload"/><br>
    <input type="submit" value="跨服务器上传"/>
</form>
```

3. 编写控制器

```java
 @RequestMapping(path = "/fileupload3" ,method = RequestMethod.POST)
    public String fileupload3(HttpServletRequest request, MultipartFile upload) throws Exception {
        System.out.println("文件上传3:跨服务器上传......");
        // 定义文件上传服务器的路径
        String path = "http://localhost:9090/uploads/";


        //获取到上传文件的名称
        String filename = upload.getOriginalFilename();

        // 把文件的名称设置唯一值，uuid
        String uuid = UUID.randomUUID().toString().replace("-","");
        filename = uuid + filename;
        // 向图片服务器上传文件


        // 创建客户端对象
        Client client =new Client();
        // 连接图片服务器
        WebResource webResource = client.resource(path + filename);

        // 上传文件
        String result = webResource.put(String.class, upload.getBytes());

        System.out.println(result);
        return "success";
    }
```





# （七）异常处理

## 1. 异常处理器分析
**分析：**

**SpringMVC进行异常处理**

系统中异常包括两类：预期异常和运行时异常 RuntimeException，前者通过捕获异常从而获取异常信息，后者主要通过规范代码开发、测试通过手段减少运行时异常的发生。
系统的 dao、service、controller 出现都通过 throws Exception 向上抛出，最后由 springmvc `前端
控制器`交由`异常处理器`进行异常处理，如下图：
**异常处理思路**

- Controller调用service，service调用dao，异常都是向上抛出的，最终有 DispatcherServlet 找异常处理器进行异常的处理。
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706100411.png" alt="在这里插入图片描述" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706100417.png" alt="在这里插入图片描述" style="zoom:100%;" />
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200706100431.png" alt="在这里插入图片描述" style="zoom:100%;" />

## 2. SpringMVC的异常处理
1. 自定义异常类

```java

public class SysException extends Exception{
    // 存储异常信息
    private String message;

    public SysException(String message) {
        this.message = message;
    }

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }
}

```


2. 自定义异常处理器

```java

public class SysExceptionResolver implements HandlerExceptionResolver {
    /**
     * 处理异常的业务逻辑
     */

    @Override
    public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) {
        System.out.println("异常处理方法执行了...");

        // 获取到异常对象
        SysException exception =null;
        if (e instanceof SysException){
            exception = (SysException)e;
        }else{
            exception = new SysException("系统正在维护....");
        }

        // 创建ModelAndView对象
        ModelAndView mv = new ModelAndView();
        mv.addObject("errorMsg",exception.getMessage());
        // 跳转页面
        mv.setViewName("error");
        
        return mv;
    }
}

```

3.  配置异常处理器

```xml
<!--    配置异常处理器-->
  <bean id="sysExceptionResolver" class="code.exception.SysExceptionResolver">
  </bean>
```

4. 页面和控制器方法
```html
<a href="user/testException">异常处理...</a>
```

```java

@Controller
@RequestMapping(path = "/user")
public class UserController {

    @RequestMapping(path = "/testException")
    public  String testException() throws Exception{
        System.out.println("testException 执行了。。。");

        try {
            // 模拟异常
            int i=10/0;
        } catch (Exception e) {
            // 打印异常信息
            e.printStackTrace();
            // 抛出自定义异常信息
            throw new SysException("运行出现错误了...");
        }
        return "success";
    }

}

```



5. 页面跳转结果
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720132926.png" alt="在这里插入图片描述" style="zoom:100%;" />



# （八）拦截器



## 1. 拦截器的介绍
**拦截器的功能：**
&ensp; Spring MVC 的处理器拦截器类似于 **Servlet** 开发中的**过滤器 Filter**，用于对处理器进行`预处理`和`后处理`。

&ensp; 用户可以自己定义一些拦截器来实现特定的功能。

&ensp; 谈到拦截器，还要向大家提一个词——`拦截器链（Interceptor Chain）`。拦截器链就是将拦截器按一定的顺序联结成一条链。在访问被拦截的方法或字段时，拦截器链中的拦截器就会按其之前定义的顺序被调用。


&ensp; 说到这里，可能大家脑海中有了一个疑问，这不是我们之前学的过滤器吗？是的它和过滤器是有几分相似，但是也有区别，接下来我们就来说说他们的区别：

<font color=#009B90>**过滤器**</font>是 servlet 规范中的一部分，任何 java web 工程都可以使用。
<font color=#102770> **拦截器** </font>是 SpringMVC 框架自己的，只有使用了 SpringMVC 框架的工程才能用。
<font color=#009B90> **过滤器** </font>在 url-pattern 中配置了/*之后，可以对所有要访问的资源拦截。
<font color=#102770>  **拦截器** </font>它是只会拦截访问的控制器方法，如果访问的是 jsp，html,css,image 或者 js 是不会进行拦
截的。
&ensp; 它也是 AOP 思想的具体应用。
&ensp; 我们要想自定义拦截器， 要求必须实现：<font color=#C72C1C>  **HandlerInterceptor  接口。** </font>


<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720132932.png" alt="在这里插入图片描述" style="zoom:100%;" />

##  2. 环境搭建
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720133307.png" alt="在这里插入图片描述" style="zoom:100%;" />
### 2.1 pom.xml

```xml
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <!--  spring的版本锁定-->
    <spring.version>5.0.2.RELEASE</spring.version>
  </properties>

  <dependencies>
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

  </dependencies>
```

### 2.2 web.xml

```xml
<web-app>
  <display-name>Archetype Created Web Application</display-name>

  <servlet>
    <servlet-name>dispatcherServlet</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

    <init-param>
      <param-name>contextConfigLocation</param-name>
      <param-value>classpath:springmvc.xml</param-value>
    </init-param>
    <load-on-startup>1</load-on-startup>
  </servlet>

  <servlet-mapping>
    <servlet-name>dispatcherServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>


  <!--  配置解决中文乱码的过滤器-->
  <filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <!--    初始化参数-->
    <init-param>
      <param-name>encoding</param-name>
      <param-value>UTF-8</param-value>
    </init-param>
  </filter>

  <filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
  </filter-mapping>

</web-app>

```

### 2.3 springmvc.xml

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

<!--开启注解扫描-->
<context:component-scan base-package="code"></context:component-scan>

<!--    视图解析器对象-->
<bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/pages/"></property>
    <property name="suffix" value=".jsp"></property>

</bean>

<!--前端控制器，那些静态资源不拦截-->
    <!-- 设置静态资源不过滤 -->
    <mvc:resources location="/css/" mapping="/css/**"/> <!-- 样式 -->
    <mvc:resources location="/images/" mapping="/images/**"/> <!-- 图片 -->
    <mvc:resources location="/js/" mapping="/js/**"/> <!-- javascript -->

<!--配置拦截器-->
<mvc:interceptors>
    <mvc:interceptor>
<!--        要拦截的具体方法-->
        <mvc:mapping path="/user/**"/>
<!--        不要拦截的具体方法-->
<!--        <mvc:exclude-mapping path=""/>-->

<!--        配置拦截器对象-->
        <bean class="code.interceptor.MyInterceptor1"></bean>
    </mvc:interceptor>
</mvc:interceptors>
    

<!--    开启注解SpringMVC框架注解的支持-->
    <mvc:annotation-driven />
</beans>
```


## 3. 入门代码
###  1. UserController

```htnl
<a href="user/testInterceptor">拦截器....</a>
```

```java
@Controller
@RequestMapping(path = "/user")
public class UserController {

    @RequestMapping(path = "/testInterceptor")
    public  String testInterceptor() throws Exception{
        System.out.println("测试testInterceptor 执行了。。。");

        return "success";
    }
}
```


###   2. 自定义拦截器：MyInterceptor1

```java
/**
 * 自定义拦截器
 */
public class MyInterceptor1 implements HandlerInterceptor {

    /**
     * 预处理，controller方法执行前
     * 返回值：true，表示放行，执行下一个拦截器，若没有下一个拦截器，就执行 controller 中的方法
     * 返回值：false，可以使用request、response对象跳转到相关页面
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {

        System.out.println("拦截器1执行了....前");
//        request.getRequestDispatcher("/WEB-INF/pages/error.jsp").forward(request,response);
        return true;
    }

    /**
     * 后处理方法，controller 方法执行之后，success.jsp 执行之前
     */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("拦截器1执行了....后");
    }

    /**
     * success.jsp页面执行后，该方法会执行
     */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("拦截器1执行了....最后");
    }
}


```

###   3. 在springmvc.xml中配置拦截器（上面的代码中已经加入）

```xml
<!--配置拦截器-->
<mvc:interceptors>
    <mvc:interceptor>
<!--        要拦截的具体方法-->
        <mvc:mapping path="/user/**"/>
<!--        不要拦截的具体方法-->
<!--        <mvc:exclude-mapping path=""/>-->

<!--        配置拦截器对象-->
        <bean class="code.interceptor.MyInterceptor1"></bean>
    </mvc:interceptor>
</mvc:interceptors>
```



**结果**
<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720133316.png" alt="在这里插入图片描述" style="zoom:100%;" />





