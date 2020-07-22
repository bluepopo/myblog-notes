【高并发秒杀商城】毕业设计项目过程记录



# 一、项目概述



项目核心技术点：

为什么说秒杀项目有特点？因为它更多再实际场景中应用，是一瞬间得大量访问量是一个关键技术点、二是数据库得缓存技术，







<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717230828.png" alt="image-20200717230821427" style="zoom: 50%;" />





<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720130459.png" alt="image-20200717230941072" style="zoom:50%;" />







<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717231923.png" alt="image-20200717231921345" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717232003.png" alt="image-20200717232002386" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200717232226.png" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200720130453.png" alt="image-20200717232337778" style="zoom:50%;" />





# 第一章 项目框架搭建



<img src="https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200718092826.png" alt="image-20200717232633327" style="zoom:50%;" />



## 1.1 集成Mybatis

http://mybatis.org/spring-boot-starter/mybatis-spring-boot-autoconfigure/

```xml
  <dependency>
      <groupId>org.mybatis.spring.boot</groupId>
       <artifactId>mybatis-spring-boot-starter</artifactId>
       <version>2.1.3</version>
 </dependency>

 <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>

 <dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
     <version>1.0.5</version>
 </dependency>
```



```properties
# mybatis
mybatis.type-aliases-package=zy.code.project_highconcurrencymall.domain
mybatis.configuration.map-underscore-to-camel-case=true
mybatis.configuration.default-fetch-size=100
mybatis.configuration.default-statement-timeout=3000
# 将到层接口类和xml配置文件放在同一个包
mybatis.mapperLocations = classpath:zy/code/project_highconcurrencymall/dao/*.xml

# druid 数据源
spring.datasource.url=jdbc:mysql://192.168.40.132:3306/highmall?useUnicode=true&characterEncoding=utf-8&allowMultiQueries=true&useSSL=false
spring.datasource.username=root
spring.datasource.password=123456
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
spring.datasource.filters=stat
spring.datasource.maxActive=2
spring.datasource.initialSize=1
spring.datasource.maxWait=60000
spring.datasource.minIdle=1
spring.datasource.timeBetweenEvictionRunsMillis=60000
spring.datasource.minEvictableIdleTimeMillis=300000
spring.datasource.validationQuery=select 'x'
spring.datasource.testWhileIdle=true
spring.datasource.testOnBorrow=false
spring.datasource.testOnReturn=false
spring.datasource.poolPreparedStatements=true
spring.datasource.maxOpenPreparedStatements=20
```







## 1.2 整合Redis



### 1.2.1 环境搭建



Fastjson 是一个 Java 库，可以将 Java 对象转换为 JSON 格式，当然它也可以将 JSON 字符串转换为 Java 对象。

Fastjson 可以操作任何 Java 对象，即使是一些预先存在的没有源码的对象。

提供了 `toJSONString()` 和 `parseObject()` 方法来将 Java 对象与 JSON 相互转换

```xml
<!--        缓存 jedis客户端-->
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
        </dependency>

<!--        fastjson 格式转换-->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.38</version>
        </dependency>
```



```properties
# redis 配置
redis.host=192.168.40.132
redis.port=6379
redis.timeout=3
# redis.password=123456
redis.poolMaxTotal=10
redis.poolMaxIdle=10
redis.poolMaxWait=3
```



```java
/**
 * Redis 配置类
 * 读取配置文件中的属性设置，为 redis设置连接、连接池大小、超时时间等属性。
 */

@Component
@ConfigurationProperties(prefix="redis")
public class RedisConfig {
	private String host;
	private int port;
	private int timeout;//秒
	private String password;
	private int poolMaxTotal;
	private int poolMaxIdle;
	private int poolMaxWait;//秒
	public String getHost() {
		return host;
	}
	public void setHost(String host) {
		this.host = host;
	}
	public int getPort() {
		return port;
	}
	public void setPort(int port) {
		this.port = port;
	}
	public int getTimeout() {
		return timeout;
	}
	public void setTimeout(int timeout) {
		this.timeout = timeout;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
	public int getPoolMaxTotal() {
		return poolMaxTotal;
	}
	public void setPoolMaxTotal(int poolMaxTotal) {
		this.poolMaxTotal = poolMaxTotal;
	}
	public int getPoolMaxIdle() {
		return poolMaxIdle;
	}
	public void setPoolMaxIdle(int poolMaxIdle) {
		this.poolMaxIdle = poolMaxIdle;
	}
	public int getPoolMaxWait() {
		return poolMaxWait;
	}
	public void setPoolMaxWait(int poolMaxWait) {
		this.poolMaxWait = poolMaxWait;
	}
}
```



### 1.2.2 数据存储（Json转换与KeyPrefix）

思路：

- RedisConfig : Redis 配置 host、username、password等属性，扫描配置文件中的“redis”前缀

- RedisPoolFactory ： new JedisPool 创建连接池并通过 @Bean 注入到容器
- RedisService ： 提供get、set等方法，操作缓存数据，包括JSON格式转换。
- KeyPrefix、BasePrefix ： key生成前缀的规范，抽象层，可以使用 “类名：[prefix]xxxx”
- UserKey、OrderKey ：Key 具体生成实现类。可以使用“类名:idxxxx” 、“类名:namexxxx”等形式





**RedisService**

```java
@Service
public class RedisService {


    @Autowired
    JedisPool jedisPool;

    /**
     * 获取当个对象
     * */
    public <T> T get(KeyPrefix prefix, String key,  Class<T> clazz) {
        Jedis jedis = null;
        try {
            jedis =  jedisPool.getResource();
            //根据参数传入的KeyPrefix，生成对应的key前缀。传入 UserKey，就获取UserKey的前缀
            String realKey  = prefix.getPrefix() + key;
            String  str = jedis.get(realKey);
            T t =  stringToBean(str, clazz);
            return t;
        }finally {
            returnToPool(jedis);
        }
    }

    /**
     * 设置对象
     * */
    public <T> boolean set(KeyPrefix prefix, String key,  T value) {
        Jedis jedis = null;
        try {
            jedis =  jedisPool.getResource();
            String str = beanToString(value);
            if(str == null || str.length() <= 0) {
                return false;
            }
            //生成真正的key
            String realKey  = prefix.getPrefix() + key;
            int seconds =  prefix.expireSeconds();
            if(seconds <= 0) {
                jedis.set(realKey, str);
            }else {
                jedis.setex(realKey, seconds, str);
            }
            return true;
        }finally {
            returnToPool(jedis);
        }
    }

    /**
     * 判断key是否存在
     * */
    public <T> boolean exists(KeyPrefix prefix, String key) {
        Jedis jedis = null;
        try {
            jedis =  jedisPool.getResource();
            //生成真正的key
            String realKey  = prefix.getPrefix() + key;
            return  jedis.exists(realKey);
        }finally {
            returnToPool(jedis);
        }
    }

    /**
     * 增加值
     * */
    public <T> Long incr(KeyPrefix prefix, String key) {
        Jedis jedis = null;
        try {
            jedis =  jedisPool.getResource();
            //生成真正的key
            String realKey  = prefix.getPrefix() + key;
            return  jedis.incr(realKey);
        }finally {
            returnToPool(jedis);
        }
    }


    /**
     * 减少值
     * */
    public <T> Long decr(KeyPrefix prefix, String key) {
        Jedis jedis = null;
        try {
            jedis =  jedisPool.getResource();
            //生成真正的key
            String realKey  = prefix.getPrefix() + key;
            return  jedis.decr(realKey);
        }finally {
            returnToPool(jedis);
        }
    }


    /**
         * 类型转换：存入缓存数据时
         * 将Bean对象转换为JSON字符串格式
         */
    private <T> String beanToString(T value) {
        if(value == null) {
            return null;
        }
        Class<?> clazz = value.getClass();
        if(clazz == int.class || clazz == Integer.class) {
            return ""+value;
        }else if(clazz == String.class) {
            return (String)value;
        }else if(clazz == long.class || clazz == Long.class) {
            return ""+value;
        }else {
            return JSON.toJSONString(value);
        }
    }

    /**
     * 类型转换：获取缓存数据时
     * 将字符串对象转换为Bean对象
     */
    @SuppressWarnings("unchecked")
    private <T> T stringToBean(String str, Class<T> clazz) {
        if(str == null || str.length() <= 0 || clazz == null) {
            return null;
        }
        if(clazz == int.class || clazz == Integer.class) {
            return (T)Integer.valueOf(str);
        }else if(clazz == String.class) {
            return (T)str;
        }else if(clazz == long.class || clazz == Long.class) {
            return  (T)Long.valueOf(str);
        }else {
            return JSON.toJavaObject(JSON.parseObject(str), clazz);
        }
    }
    /**
     * 将连接返回到连接池
     * @param jedis
     */
    private void returnToPool(Jedis jedis) {
        if(jedis != null) {
            jedis.close();
        }
    }
}

```





![](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200721124423.png)





**KeyPrefix**

```java
/**

 * KeyPrefix key前缀的生成与获取
   */
   public interface KeyPrefix {
   	
   public int expireSeconds();

   public String getPrefix();

}
```



**BasePrefix**

```java
/**
 * 抽象类
 */
public abstract class BasePrefix implements KeyPrefix{
	
	private int expireSeconds;//过期时间
	
	private String prefix;//前缀
	
	public BasePrefix(String prefix) {
		//默认：0代表永不过期
		this(0, prefix);
	}
	
	public BasePrefix( int expireSeconds, String prefix) {
		this.expireSeconds = expireSeconds;
		this.prefix = prefix;
	}
	
	public int expireSeconds() {
		//默认0代表永不过期
		return expireSeconds;
	}

	public String getPrefix() {
		String className = getClass().getSimpleName();
		return className+":" + prefix + ":";
	}

}
```

**UserKey**

```java
/**
 * UserKey 创建前缀，可以根据id、name、email等待都可以自己定制
 */
public class UserKey extends BasePrefix{

	private UserKey(String prefix) {
		//使用父类默认构造方法，expireSeconds=0，prefix=参数
		super(prefix);
	}
	public static UserKey getById = new UserKey("id");//UserKey:id 为前缀
	public static UserKey getByName = new UserKey("name");//UserKey:name 为前缀
}
```



**MiaoshaUserKey**

在实现类中继承抽象类 BasePrefix，创建实例时可以指定 期限、前缀。

如下图，可以创建专门用于秒杀的缓存数据的 key

![image-20200721124909118](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200721124910.png)



测试生成的前缀是什么样子

```java
@GetMapping("/jedis/setbyname")
public Result<User> redisSetByName(User user){

    userService.insert(user);

    User user2 = userService.getByName(user.getName());

    redisService.set(UserKey.getByName,""+user2.getName(),user2);
    User redis_value = redisService.get(UserKey.getByName, "" + user2.getName(), User.class);
    return Result.success(redis_value);
}
```



![image-20200721125118363](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200721125119.png)







# 第二章、安全登录校验



```mysql
Create table `highmall`.`miaosha_user`(  
  `id` bigint(20) NOT NULL COMMENT '用户ID，手机号码',
  `nickname` varchar(255) DEFAULT 'null',
  `PASSWORD` varchar(32) DEFAULT 'NULL' COMMENT 'MDS(MDS(pass明文+固定salt) + salt)',
  `salt` varchar(10) DEFAULT 'NULL',
  `head` varchar(128) DEFAULT 'NULL' COMMENT '头像，云存储的ID',
  `register_data` datetime DEFAULT 'CURRENT_TIMESTAMP ' COMMENT '注册时间',
  `last_login_data` datetime DEFAULT 'CURRENT_TIMESTAMP ' COMMENT '上次登录时间',
  `login_count` int(11) DEFAULT 0 COMMENT '登录次数',
  primary key (`id`)
) ENGINE=InnoDB charset=utf8
```

注意：MySQL 大于5.6的版本，时间类型的默认值不能使用Null

## 2.1 两次MD5：明文密码加密



用户端第一次MD5: 为了防止用户前台输入的明文密码在网络上传输，如果谁抓包到这个用户名密码就存在信息安全泄漏。

- 明文密码+固定 Salt

服务端第二次MD5：防止数据库表中存的密码被盗，再进行一层加密。

- 用户输入+随机Salt

```xml
<!--        MD5 引入-->
        <dependency>
            <groupId>commons-codec</groupId>
            <artifactId>commons-codec</artifactId>
        </dependency>

        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>3.6</version>
        </dependency>
```

```java
/*
* 工具类
*/
public class MD5Util {
	
	public static String md5(String src) {
		return DigestUtils.md5Hex(src);
	}

	// 固定 sqlt
	private static final String salt = "1a2b3c4d";

	/**
	 * 用户输入的password，MD5加密，网络传输表单password
	 */
	public static String inputPassToFormPass(String inputPass) {
		String str = ""+salt.charAt(0)+salt.charAt(2) + inputPass +salt.charAt(5) + salt.charAt(4);
		System.out.println(str);
		return md5(str);
	}

	/**
	 * 网络表单password，MD5加密，数据库db的password
	 */
	public static String formPassToDBPass(String formPass, String salt) {
		String str = ""+salt.charAt(0)+salt.charAt(2) + formPass +salt.charAt(5) + salt.charAt(4);
		return md5(str);
	}

	/**
	 * 用户输入password，两次MD5加密，数据库db的password
	 */
	public static String inputPassToDbPass(String inputPass, String saltDB) {
		String formPass = inputPassToFormPass(inputPass);
		String dbPass = formPassToDBPass(formPass, saltDB);
		return dbPass;
	}
	
	public static void main(String[] args) {
		System.out.println(inputPassToFormPass("123456"));//d3b1294a61a07da9b49b6e22b2cbd7f9
//		System.out.println(formPassToDBPass(inputPassToFormPass("123456"), "1a2b3c4d"));
//		System.out.println(inputPassToDbPass("123456", "1a2b3c4d"));//b7797cce01b4b131b433b6acf4add449
	}
	
}

```

实现：

第一层加密，使用在发送Ajax之前。使用jQuery的MD5加密工具

![image-20200721193816552](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200721193819.png)

![image-20200721193904664](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200721193906.png)



**LoginController**

```java
  @RequestMapping("/do_login")
    @ResponseBody
    public Result<Boolean> doLogin(HttpServletResponse response,LoginVo loginVo) {
        //打印日志
    	log.info("loginController [dologin Method]"+loginVo.toString());
    	//初步参数校验
        String mobile = loginVo.getMobile();
        String password_input = loginVo.getPassword();
        if (StringUtils.isEmpty(password_input)){
            return Result.error(CodeMsg.PASSWORD_EMPTY);
        }
        if (StringUtils.isEmpty(mobile)){
            return Result.error(CodeMsg.MOBILE_EMPTY);
        }
        if (!ValidatorUtil.isMobile(mobile)){
            return Result.error(CodeMsg.MOBILE_ERROR);//手机号格式有误
        }

        //登录
        CodeMsg loginMsg = miaoshaUserService.login(response, loginVo);
        if (loginMsg.getCode() == 0){
            return Result.success(true);
        }else {
            return Result.error(loginMsg);
        }

    }
```

**MiaoshaUserService**

```java
/**
 * 登录
 */
public CodeMsg login(HttpServletResponse response, LoginVo loginVo) {
    if (loginVo == null){
        return CodeMsg.MOBILE_EMPTY;
    }

    String mobile = loginVo.getMobile();
    String password = loginVo.getPassword();
    MiaoshaUser userDB = miaoshaUserDao.findById(Long.parseLong(mobile));
    //用户名未注册
    if (userDB == null){
        return CodeMsg.MOBILE_NOT_EXIST;
    }
    //密码校验
    String dbPassword = userDB.getPassword();
    String dbSalt = userDB.getSalt();

    String  calcPassword = MD5Util.formPassToDBPass(password,dbSalt);

    if (!calcPassword.equals(dbPassword)) {
        return CodeMsg.PASSWORD_ERROR;

    }

    log.info("MiaoshaUserService [login Method] LoginVo calcPassword :" + calcPassword);
    return CodeMsg.SUCCESS;

}
```





## 2.2 JSR303 校验

在上面的代码中我们发现，前台页面传过来的参数，我们要进行判空、格式判断、比如手机号格式、Email格式、身份证号格式等待校验，难道每个都手写一个工具类吗？

no no no....现在流行的 JSR303 校验解放你的双手！

```xml
<!--        引入JSR303校验-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-validation</artifactId>
        </dependency>
```

![image-20200721210112052](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200721224452.png)



![image-20200721210208893](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200721224445.png)

![](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200721211430.png)



## 2.3 全局异常处理

![](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200722223022.png)



![image-20200722223124423](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200722223125.png)



还有一个小问题：

我们应该自定义一个运行时的全局异常类，而不是在逻辑代码发生异常时就return 一个 CodeMsg.Error 的错误码对象。

全局异常就是获取CodeMsg，然后在业务代码中，如果发生异常，我们要做的就是 throw 这个自定义全局异常。



抛出的这个异常就会被我们定义的 exceptionHandler 所捕获。

![image-20200722224213109](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200722224214.png)









## 2.4 分布式session

什么是分布式session？

实际的秒杀场景是需要很多服务器同时工作的，怎么解决session跨域问题？也就是用户的session信息在不同时间访问的的是多台不同的服务器。那么这些服务器怎么同步获得用户的session域信息？

本项目解决方案：

- 使用用户的token
- 用户登录成功后，服务器返回一个token给客户端，客户端把该值放到自己的cookie中
- 用户下次访问服务时，使用这个cookie

![](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200722235540.png)

![image-20200722233128247](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200722233129.png)

![image-20200722233452587](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200722235559.png)

![image-20200722233311054](https://raw.githubusercontent.com/bluepopo/myblog/master/img/20200722233315.png)