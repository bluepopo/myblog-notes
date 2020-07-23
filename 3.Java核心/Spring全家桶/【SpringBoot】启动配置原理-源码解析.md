【SpringBoot】启动配置原理-源码解析



# 一、启动原理



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200718233341.png" alt="image-20200718233338702" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200718233405.png" alt="image-20200718233359049" style="zoom: 67%;" />









## 1.1 创建SpringApplication对象



[SpringApplication](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-spring-application.html)类用于引导和启动一个Spring应用程序(即SpringBoot开发的应用)。

SpringApplication类会做如下事情启动应用：

- 为应用创建一个合适的ApplicationContext
- 注册一个CommandLinePropertySource，通过CommandLinePropertySource可以对外暴露命令行参数，并将命令行参数与spring应用中用到的properties关联起来
- 启动ApplicationContext
- 执行所有的CommandLineRunner类型bean



<font color=#1EBC61>下面是SpringApplication类静态run方法的源码。可以看到，当我们调用这个静态run方法时，实际上会构造一个SpringApplication实例，然后再调用实例的run方法完成spring应用的启动。</font>

下面是创建SpringApplication实例的过程

- 会保存参数中传递进来的配置类 class[]
- 会获取spring.factories 中的所有 初始化器initializer
- 会获取spring.factories 中的所有 监听者 Listeners
- 设置主配置类

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200718235205.png" alt="image-20200718235204396" style="zoom:100%;" />

```java
initialize(sources);
private void initialize(Object[] sources) {
    //保存主配置类
    if (sources != null && sources.length > 0) {
        this.sources.addAll(Arrays.asList(sources));
    }
    //判断当前是否一个web应用
    this.webEnvironment = deduceWebEnvironment();
    
    //注意！这一步很重要！！
    //从类路径下找到META‐INF/spring.factories配置的所有ApplicationContextInitializer；然后保存起来
    //Initializers
    setInitializers((Collection) getSpringFactoriesInstances(ApplicationContextInitializer.class));
    //Listeners
 	this.setListeners(this.getSpringFactoriesInstances(ApplicationListener.class));	
    //从多个配置类中找到有main方法的主配置类
    this.mainApplicationClass = deduceMainApplicationClass();
}
```



<font color=red>注意：初始化器 Initializers,    run方法时你还会需要它！</font>

```java
//  initializers 是SpringApplication的重要成员变量，在是SpringApplication实例调用run()方法时，在准备上下文环境时，就会使用加载的所有 initializers 初始化器，调用初始化方法，做一个上下文环境的准备。
private List<ApplicationContextInitializer<?>> initializers;

// listeners 也是创建实例阶段获取到应用中的重要成员，在run阶段，监听者也充当重要角色
private List<ApplicationListener<?>> listeners;
```





<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200719081757.png" alt="" style="zoom:100%;" />









## 1.2 run方法

```java
 public ConfigurableApplicationContext run(String... args) {
     //开始的一些准备
        StopWatch stopWatch = new StopWatch();
        stopWatch.start();
        ConfigurableApplicationContext context = null;
        Collection<SpringBootExceptionReporter> exceptionReporters = new ArrayList();
        this.configureHeadlessProperty();
     // 获取所有的SpringApplicationRunListeners，从类路径下MATE-INF/springfactories 下获取所有监听者
        SpringApplicationRunListeners listeners = this.getRunListeners(args);
     // 监听者回调start方法开启监听
        listeners.starting();

        Collection exceptionReporters;
        try {
            // 封装命令行参数
            ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
            // 准备环境
            //创建环境完成后回调SpringApplicationRunListener.environmentPrepared()；表示环境准备完成
            ConfigurableEnvironment environment = this.prepareEnvironment(listeners, applicationArguments);
            this.configureIgnoreBeanInfo(environment);
            //打印版本图标
            Banner printedBanner = this.printBanner(environment);
            // 创建ioc容器
            context = this.createApplicationContext();
            // 做异常处理分析使用
            exceptionReporters = this.getSpringFactoriesInstances(SpringBootExceptionReporter.class, new Class[]{ConfigurableApplicationContext.class}, context);
            //准备上下文环境
            // 该方法中会完成所有initializer初始化器的initialize()方法一起完成初始化
            // 初始化完成，同时该步骤就会回调listeners的prepareContext()方法，表示准备阶段
            // prepareContext运行完成以后回调所有的SpringApplicationRunListener的contextLoaded（）；表示完成阶段
            this.prepareContext(context, environment, listeners, applicationArguments, printedBanner);
            
            //刷新容器，表示ioc容器初始化完成，扫描所有的配置类、@Bean
            // 如果是web应用这步骤还会创建嵌入式servlet
            this.refreshContext(context);
            //从ioc容器中获取所有的ApplicationRunner和CommandLineRunner进行回调
  		     //有优先级：ApplicationRunner先回调，CommandLineRunner再回调
            this.afterRefresh(context, applicationArguments);
            
            //所有的SpringApplicationRunListener回调finished方法
            
            // 结束
            stopWatch.stop();
            //日志记录
            if (this.logStartupInfo) {
                (new StartupInfoLogger(this.mainApplicationClass)).logStarted(this.getApplicationLog(), stopWatch);
            }
			//开启阶段完成
            listeners.started(context);
            this.callRunners(context, applicationArguments);
        } catch (Throwable var10) {
            // 捕获异常处理分析
            this.handleRunFailure(context, var10, exceptionReporters, listeners);
            throw new IllegalStateException(var10);
        }

      
        try {
          //  listeners.running(context)通知所有的SpringApplicationRunListener：“注意啦、注意啦，SpringBoot应用已经开始运行啦”。
            listeners.running(context);
            //整个SpringBoot应用启动完成以后,返回启动的ioc容器：context；
            return context;
        } catch (Throwable var9) {
            this.handleRunFailure(context, var9, exceptionReporters, (SpringApplicationRunListeners)null);
            throw new IllegalStateException(var9);
        }
    }
```

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200719085646.png" alt="image-20200719085644631" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200719085948.png" alt="image-20200719085947349" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200719093641.png" alt="image-20200719093640520" style="zoom:100%;" />





## 1.3 总结

SpringApplication类将一个SpringBoot应用的启动流程模板化，并在启动过程中提供了一些扩展点让我们可以根据具体需求进行扩展(通过SpringApplicationRunListener机制)。
SpringBoot提供的就是这样一个标准化的平台，在这个平台上既可以运行普通java应用，也可以运行web应用。SpringBoot平台会根据classpath自动判断应用类型，创建对应类型的ApplicationContext和加载恰当的配置。
当然，除了平台的能力外，SpringBoot还提供了很多XXXAutoConfiguration支持自动装配。我想，正是平台的能力和自动装配的能力，才让SpringBoot变得强大吧。

> 该总结作者：poype
>
>    文章：[深入理解SpringApplication](https://segmentfault.com/a/1190000019560001)
>
> ​	来源：SegmentFault 思否
> 
>





# 二、源码定制-事件监听机制

几个重要的事件回调机制

- 配置在META-INF/spring.factories，每个自动配置依赖的初始化器都可以到外部依赖中的该路径去寻找这个包

- ApplicationContextInitializer，初始化器

- SpringApplicationRunListener，启动时的监听者

  

- 只需要放在ioc容器中，然后去加载得
  ApplicationRunner
  CommandLineRunner

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720125041.png" alt="image-20200719115625631" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200719113423.png" alt="image-20200719113421285" style="zoom:100%;" />

```java
public class HelloApplicationContextInitializer implements ApplicationContextInitializer<ConfigurableApplicationContext> {
    @Override
    public void initialize(ConfigurableApplicationContext applicationContext) {
        System.out.println("ApplicationContextInitializer...initialize..."+applicationContext);
    }
}
```

```java
public class HelloSpringApplicationRunListener implements SpringApplicationRunListener {

    public HelloSpringApplicationRunListener(SpringApplication application,String[] args) {
    }

    @Override
    public void starting() {
        System.out.println("HelloSpringApplicationRunListener...starting..开始启动中");
    }

    @Override
    public void environmentPrepared(ConfigurableEnvironment environment) {
        System.out.println("HelloSpringApplicationRunListener...environmentPrepared..准备环境");
    }

    @Override
    public void contextPrepared(ConfigurableApplicationContext context) {
        System.out.println("HelloSpringApplicationRunListener...contextPrepared..准备上下文ioc容器环境");
    }

    @Override
    public void contextLoaded(ConfigurableApplicationContext context) {
        System.out.println("HelloSpringApplicationRunListener...contextLoaded..上下文ios容器环境准备完成了");
    }

    @Override
    public void started(ConfigurableApplicationContext context) {
        System.out.println("HelloSpringApplicationRunListener...started..SpringApplication开启阶段完成");
    }

    @Override
    public void running(ConfigurableApplicationContext context) {
        System.out.println("HelloSpringApplicationRunListener...running..springBoot应用已经开始运行啦");
    }

    @Override
    public void failed(ConfigurableApplicationContext context, Throwable exception) {
        //在实验时加入了mysql、mybatis依赖，但是没有配置，会抛出没有数据源的异常导致
        System.out.println("HelloSpringApplicationRunListener...failed..启动阶段抛出异常了");
    }
}

```

```java
@Component
public class HelloApplicationRunner implements ApplicationRunner {
    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("ApplicationRunner...run....");
    }
}
```

```java
@Component
public class HelloCommandLineRunner implements CommandLineRunner {
    @Override
    public void run(String... args) throws Exception {
        System.out.println("CommandLineRunner...run..."+ Arrays.asList(args));
    }
}
```



# 三、源码定制-自定义Starters



SpringBoot的强大之处，是将原来的spring底层配置全都抽取成了，各个场景下的starters（场景启动器），如果为了简单使用，通过启动器就可以非常快速方便的进行开发。



官网starters ：https://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/htmlsingle/#using-boot-starter

想要自定义starter，必须明确自动配置的原理与过程

```properties
@Configuration  //指定这个类是一个配置类
@ConditionalOnXXX  //在指定条件成立的情况下自动配置类生效
@AutoConfigureAfter  //指定自动配置类的顺序
@Bean  //给容器中添加组件

@ConfigurationPropertie //结合相关xxxProperties类来绑定相关的配置
@EnableConfigurationProperties //让xxxProperties生效加入到容器中


自动配置类要能加载
将需要启动就加载的自动配置类，配置在META‐INF/spring.factories
使用自动配置类接口的全类名=路径1，路径2，....
# 可以实现同一个自动配置类接口，实现多种
# org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
# org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
# org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
```





<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200719121143.png" alt="image-20200719121142209" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720125054.png" alt="image-20200719122113002" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720125059.png" alt="image-20200719134727586" style="zoom: 50%;" />



第一步、创建启动器模块和自动配置模块

- myhello-spring-boot-autoconfigurer : springboot 模块，不加其他模块
- myhello-spring-boot-starter : 场景启动器模块，普通maven工程即可



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200719160913.png" alt="image-20200719160911047" style="zoom: 80%;" />

第二步：建立依赖关系

```xml
<!--      starter启动器模块中：  引入自动配置模块-->
        <dependency>
            <groupId>zy.code</groupId>
            <artifactId>myhello-spring-boot-autoconfigurer</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
```



```java

public class HelloService {

    private  HelloProperties helloProperties;

    public HelloProperties getHelloProperties() {
        return helloProperties;
    }

    public void setHelloProperties(HelloProperties helloProperties) {
        this.helloProperties = helloProperties;
    }

    public String sayHello(){
        return helloProperties.getPrefix() + "hello" + helloProperties.getSuffix();
    }
}
```



```java
public class HelloProperties {
    private String prefix;
    private  String suffix;

    public String getPrefix() {
        return prefix;
    }

    public void setPrefix(String prefix) {
        this.prefix = prefix;
    }

    public String getSuffix() {
        return suffix;
    }

    public void setSuffix(String suffix) {
        this.suffix = suffix;
    }
}

```

```java
@Configuration
@ConditionalOnWebApplication//只有在web环境下才配置
@EnableConfigurationProperties(HelloProperties.class)//让属性文件生效
public class HelloServiceAutoConfiguration {

    @Autowired
    HelloProperties helloProperties;

    @Bean
    public HelloService helloService(){
        // 将helloService注入到容器中
       HelloService helloService =  new HelloService();
       helloService.setHelloProperties(helloProperties);
        return helloService;
    }


}

```

META-INF/spring.factories

```properties

# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
zy.code.configurer.HelloServiceAutoConfiguration
```



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720125106.png" alt="image-20200719225615860" style="zoom:100%;" />







<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720125114.png" alt="image-20200719225104136" style="zoom:100%;" />







第三步、使用自定义starter

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200719230204.png" alt="image-20200719230201427" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200719230250.png" alt="image-20200719230249563" style="zoom:100%;" />



```properties
server.port=8082

zy.myhello.prefix=welcome
zy.myhello.suffix=to my world
```



```java
@Controller
public class HelloController {

    @Autowired
    HelloService helloService;

    @GetMapping("/hello")
    @ResponseBody
    public String sayHello(){
        return  helloService.sayHello();

    }
}

```



总结：自定义一个starter

- Service ：我可以提供的服务
- Properties：属性
- configuration：配置。使properties生效、什么条件下自动进行该配置
- META-INF/spring.factories : org.springframework.boot.autoconfigure.EnableAutoConfiguration=我们的配置类全类名

# 四、自动配置原理



上面的不管是全局默认配置文件，还是我们自己创建的yml/properties配置文件，都会被主配置类自动配置。

那么springBoot是怎么将配置文件中设置的属性扫描、配置到工程中的呢？使用了哪些必要的代码和参数？





<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200710100527.png" alt="image-20200708101557479" style="zoom:80%;" />





1）、SpringBoot启动的时候加载主配置类，开启了自动配置功能 ==@EnableAutoConfiguration==
2）、@EnableAutoConfiguration 作用：

- 利用EnableAutoConfigurationImportSelector给容器中导入一些组件？
- 可以查看selectImports()方法的内容；
- List configurations = getCandidateConfigurations(annotationMetadata, attributes);  //获取候选的配置



```
SpringFactoriesLoader.loadFactoryNames()
扫描所有已导入的jar包类路径下的  META‐INF/spring.factories
把扫描到的这些文件的内容包装成 properties对象
从 properties 中获取到 EnableAutoConfiguration.class类（类名）对应的值，然后把他们添加在容器中
```

将 类路径下 ==META-INF/spring.factories== 里面配置的所有EnableAutoConfiguration的值加入到了容器中；





<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720125225.png" alt="image-20200708103918305" style="zoom:100%;" />

每一个这样的 xxxAutoConfiguration类都是容器中的一个组件，都加入到容器中；用他们来做自动配置；
3）、每一个自动配置类进行自动配置功能；
4）、以HttpEncodingAutoConfiguration（Http编码自动配置）为例解释自动配置原理；

```java
//表示这是一个配置类，以前编写的配置文件一样，也可以给容器中添加组件
@Configuration   
//启动指定类的
//ConfigurationProperties功能；将配置文件中对应的值和HttpEncodingProperties绑定起来；并把
//HttpEncodingProperties加入到ioc容器中
@EnableConfigurationProperties(HttpEncodingProperties.class)  



//Spring底层@Conditional注解（Spring注解版），根据不同的条件，如果满足指定的条件，整个配置类里面的配置就会生效；    
// 判断当前应用是否是web应用，如果是，当前配置类生效
@ConditionalOnWebApplication 

//判断当前项目有没有这个类    CharacterEncodingFilter；SpringMVC中进行乱码解决的过滤器；
@ConditionalOnClass(CharacterEncodingFilter.class)  

//判断配置文件中是否存在某个配置  spring.http.encoding.enabled；如果不存在，判断也是成立的
//即使我们配置文件中不配置pring.http.encoding.enabled=true，也是默认生效的；
@ConditionalOnProperty(prefix = "spring.http.encoding", value = "enabled", matchIfMissing =true)  
public class HttpEncodingAutoConfiguration {
 
   //他已经和SpringBoot的配置文件映射了  
   private final HttpEncodingProperties properties;
   //只有一个有参构造器的情况下，参数的值就会从容器中拿
   public HttpEncodingAutoConfiguration(HttpEncodingProperties properties) {  
	this.properties = properties;        
}  
    
//给容器中添加一个组件，这个组件的某些值需要从properties中获取
@Bean   
//判断容器没有这个组件？没有这个组件就加入这个组件，已有，则不加    
@ConditionalOnMissingBean(CharacterEncodingFilter.class)
public CharacterEncodingFilter characterEncodingFilter() {    
	CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();        
	filter.setEncoding(this.properties.getCharset().name());        
	filter.setForceRequestEncoding(this.properties.shouldForce(Type.REQUEST));        
	filter.setForceResponseEncoding(this.properties.shouldForce(Type.RESPONSE));        
	return filter;        
} 
```

根据当前不同的条件判断，决定这个配置类是否生效？
一但这个配置类生效；这个配置类就会给容器中添加各种组件；这些组件的属性是从对应的properties类中获取
的，这些类里面的每一个属性又是和配置文件绑定的；

5）、所有在配置文件中能配置的属性都是在xxxxProperties类中；配置文件能配置什么就可以参照某个功
能对应的这个xxxxProperties类

```java
//从配置文件中获取指定的值和bean的属性进行绑定
@ConfigurationProperties(prefix = "spring.http.encoding")  

public class HttpEncodingProperties {
   public static final Charset DEFAULT_CHARSET = Charset.forName("UTF‐8");
```



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720125234.png" alt="image-20200708110120399" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720125243.png" alt="image-20200708110150420" style="zoom:100%;" />



**精髓：**
**1）、SpringBoot启动会加载大量的自动配置类**
**2）、我们看我们需要的功能有没有SpringBoot默认写好的自动配置类；**
**3）、我们再来看这个自动配置类中到底配置了哪些组件；（如果有我们要用的组件，我们就不需要再来手动配置了）**
**4）、给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们就可以在配置文件中指定这**
**些属性的值；**
xxxxAutoConfigurartion：自动配置类；给容器中添加组件

xxxxProperties:封装配置文件中相关属性；



### 细节：@Conditional派生注解

@Conditional派生注解（Spring注解版原生的@Conditional作用）
作用：必须是@Conditional指定的条件成立，才给容器中添加组件，配置配里面的所有内容才生效；

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720125251.png" alt="image-20200708110756522" style="zoom:100%;" />

例如上面的HttpEncodingAutoConfiguration自动配置类中有这样一段代码

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720125257.png" alt="image-20200708111113877" style="zoom:100%;" />





自动配置类必须在一定的条件下才能生效；
我们怎么知道哪些自动配置类生效；
**我们可以通过启用 debug=true属性；来让控制台打印自动配置报告，这样我们就可以很方便的知道哪些自动配置类是生效的；**

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720125303.png" alt="image-20200708111339915" style="zoom:100%;" />



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720125309.png" alt="image-20200708111458979" style="zoom:100%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720125316.png" alt="image-20200708111617062" style="zoom:100%;" />







