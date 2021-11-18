# 	SpringBoot

​	spring虽然将将项目中对象的创建以及对数据库处理等方面进行了很大程度上的优化，但是仍然避免不了写大量的配置来让业务逻辑与数据库等其他模块连接起来。SpringBoot的作用就是简化这些配置，将这些不同模块的整合过程更加方便。

**SpringBoot的优点**

- 创建独立Spring应用
- 内嵌web服务器
- 自动starter依赖，简化构建配置
- 自动配置Spring以及第三方功能
- 提供生产级别的监控、健康检查及外部化配置
- 无代码生成、无需编写XML

**SpringBoot的缺点**

- 人称版本帝，迭代快，需要时刻关注变化
- 封装太深，内部原理复杂，不容易精通

## 时代背景

### 微服务

- 微服务是一种架构风格
- 一个应用拆分为一组小型服务
- 每个服务运行在自己的进程内，也就是可独立部署和升级
- 服务之间使用轻量级HTTP交互
- 服务围绕业务功能拆分
- 可以由全自动部署机制独立部署
- 去中心化，服务自治。服务可以使用不同的语言、不同的存储技术

### 分布式

#### 分布式的困难

- 远程调用
- 服务发现
- 负载均衡
- 服务容错
- 配置管理
- 服务监控
- 链路追踪
- 日志管理
- 任务调度
- ......

#### 分布式的解决

- SpringBoot + SpringCloud

### 云原生

原生应用如何上云。 Cloud Native

#### 上云的困难

- 服务自愈
- 弹性伸缩
- 服务隔离
- 自动化部署
- 灰度发布
- 流量治理
- ......

#### 上云的解决

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211010151909475.png" alt="image-20211010151909475" style="zoom:80%;" /> 

# SpringBoot入门

- [Java 8](https://www.java.com/) & 兼容java14（14好像不行） .
- Maven 3.3+
- idea 2019.1.2

## maven的配置

spring-boot-starter-*：表示某种场景，其实就是一组场景的依赖描述
只要引入starter，这个场景所有需要的常规依赖都会自动引入

SpringBoot提供的所有场景：https://docs.spring.io/spring-boot/docs/2.5.5/reference/html/using.html#using.build-systems.starters

类似*-boot-starter开头的，一般都是第三方提供的简化场景依赖包

所有场景最底层的依赖就是

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
    <version>2.5.5</version>
    <scope>compile</scope>
</dependency>
```

maven实例：

```xml
<!--依赖管理，里面包含了几乎所有常用的一些依赖版本号，例如javax，aspectj等等，所以再这个之后添加的依赖大多都不用写版本号 --> 
<!-- 他的父依赖是
	<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-dependencies</artifactId>
        <version>2.5.5</version>
 	</parent> 

-->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.5.5</version>
</parent>

<!--SpringBoot中做web项目的核心依赖，有内置的tomcat等，打包出来的jar包可以在cmd中直接运行 --> 
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
  <!--若是对于父依赖提供的依赖包版本不满意可以如下修改，修改之后，依赖版本就会发生变化
    <properties>
        <mysql.version>5.1.43</mysql.version>
    </properties>
  -->
<!-- 打包插件，没有这个插件，打包下来只有jar文件没有orginal文件，没有orginal文件，项目无法运行 --> 
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

## Controller（web）

```java
/**
*@RestController包含了@Controller和@ResponseBody注释，是个父类注释
*/
@RestController
public class helloController {
    @ResponseBody
    @RequestMapping("/hello")
    public String handle01(){
        return "hello , world!";
    }
}
```

## 方法入口

这个方法所在的包及其子包都会被扫描，但是不在同一个包内的程序或者再这个方法所在目录的父目录下的程序都不会被扫描

​	但若是非要将这个主程序放在内层的话，其实也行。在@SpringBootApplication的属性下有一个scanBasePackage属性，将这个属性设置为需要扫描的包就行了：@SpringBootApplication(scanBasePackage = "com.test")

```java
//固定写法，就是告诉程序这是一个springboot应用，是个主程序类
//不能直接卸载java包下，需要有子包
//也可以直接当作是测试方法
@SpringBootApplication
public class mainApplication {

    public static void main(String[] args) {
        //ConfigurableApplicationContext run = SpringApplication.run(mainApplication.class,args);
        SpringApplication.run(mainApplication.class,args);
    }
}
```

​	ConfigurableApplicationContext run = SpringApplication.run(mainApplication.class,args);这个run其实就相当于spring中的容器，即原生spring中的applicationContext，可以在这个run对象中找到所有的对象，包括web.xml中的核心类DispatcherServlet以及其他的filter过滤器，文件上传filemultiply对象等等。

## 配置

所有的配置信息都可以在application.properties中修改

例：

```properties
server.port = 8888  #修改tomcat端口为8888
```

# 自动配置

无需手动加入

## 自动配好Tomcat

- 引入Tomcat依赖。
- 配置Tomcat

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
    <version>2.3.4.RELEASE</version>
    <scope>compile</scope>
</dependency>
```

## 自动配好SpringMVC

- 引入SpringMVC全套组件
- 自动配好SpringMVC常用组件（功能）

## 自动配好Web常见功能，如：字符编码问题

- SpringBoot帮我们配置好了所有web开发的常见场景

## 默认的包结构

- 主程序所在包及其下面的所有子包里面的组件都会被默认扫描进来
- 无需以前的包扫描配置
- 想要改变扫描路径，@SpringBootApplication(scanBasePackages=**"com.test"**)
  - 或者@ComponentScan 指定扫描路径

```java
@SpringBootApplication
等同于
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan("com.test")
```

## 各种配置拥有默认值

- 默认配置最终都是映射到某个类上，如：MultipartProperties
- 配置文件的值最终会绑定每个类上，这个类会在容器中创建对象

## 按需加载所有自动配置项

- 非常多的starter
- 引入了哪些场景这个场景的自动配置才会开启
- SpringBoot所有的自动配置功能都在 spring-boot-autoconfigure 包里面

## ......

# SpringBoot的底层注解

## @Configuration

```java
/**
 * 1.配置类里面使用@Bean标注的方法上给容器注册组件，默认是单实例的
 * 2.配置类本身也是一个组件
 * 3.SpringBoot2的新特性：@Configuration的新属性：proxyBeanMethods，表示代理bean的方法
 *      所谓full（全）和lite（轻量级）就是指的属性为true和false
 *      full：proxyBeanMethods = true
 *      lite：proxyBeanMethods = false
 */
//@Configuration的作用就是告诉SpringBoot这是一个配置类 就相当于 之前写的spring-config.xml配置实体类
@Configuration(proxyBeanMethods = true)
public class MyConfig {
    /**
     *当proxyBeanMethods属性为默认或者为true时，这个配置类就是一个代理对象，外部无论创建多少个组件实体，SpringBoot都会扫描容器，判断是否有这个实例，若是有的话都只会保持单实例。
     *当proxyBeanMethods属性为false时，这个配置类就不是以代理对象身份保存在容器中，他就是一个普通对象，外部创建新的组件实体时，就会产生多个实例
     */
    //给容器中添加组件，方法名就是组件对象的ID，返回类型是组件类型，返回的值就是组件在容器中的实例
    @Bean
    public User user(){
        return new User("zhangsan",18);
    }
    @Bean
    public Pet pet(){
        return new Pet("dog");
    }
}
```

举个例子：user中有一个pet属性

​	当proxyBeanMethods = true 时，那么当user对象中的pet属性创建时，这个pet就是容器中已经创建好的pet对象。

​	当proxyBeanMethods = false 时，那么当user对象中的pet属性创建时，这个属性就是一个新的对象（不在容器中），和spring容器中的对象不相同。

​	所以若是组件之间没有依赖关系，那么一般都是用false，这样相应的速度就会很快。若是组件之间有依赖关系，且组件在之后还需要使用，则用true来处理

## 模块注解

Controller、Service、Repository其本质就是Component。

它存在的本质只是给开发者看的，对Spring而言它们就都是Component。

- @Controller 控制层类

- @Service 业务层类

- @Repository 持久层类

- @Component 无法归类到前3种时就称为组件

## @Import

例：@Import({User.class,Pet.class})

给容器中自动创建出这两个类型的组件，默认调用无参构造，默认组件名就是全类名（com.bean.User，com.bean.Pet），不只是自己写的实体类，包括一些内置类，例如DBHelper这种springboot内部的类(ch.qos.logback.core.db.DBHelper)也可以在这里导入。

```java
@Import({User.class, DBHelper.class,Pet.class})
@Configuration
public class MyConfig {...}
```



## @Conditional

条件装配：满足Conditional指定的条件，则进行组件注入

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1354552/1602835786727-28b6f936-62f5-4fd6-a6c5-ae690bd1e31d.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_17%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10) 

以@ConditionalOnBean为例，这个注释表示存在某个bean时生效

```java
//@ConditionalOnBean(name="user")当这个注释出现在类上时，则只有当容器中存在名为user的组件时，这个类的所有组件才会生效
@Configuration
public class MyConfig {
//    @Bean
    public User user(){
        return new User("zhangsan",18);
    }
    //这个ConditionalOnBean在方法上表示，当容器中存在名为user的组件时，就会执行以下程序（创建pet组件）
    @ConditionalOnBean(name="user")
    @Bean("pet")
    public Pet pet(){
        return new Pet("dog");
    }
}
```

```java
@SpringBootApplication
public class mainApplication {
    public static void main(String[] args) {
    ConfigurableApplicationContext run = SpringApplication.run(mainApplication.class,args);
        String[] names = run.getBeanDefinitionNames();
        for(String name:names){
            System.out.println(name);
        }
        System.out.println("user : "+run.containsBean("user"));
    }
}
```

```java
//当user被@Bean注释时
pet : true
//当user没有被@Bean注释时
pet : false
```

## @ImportResource

​	当进行数据迁移时，对于一些以前写的spring配置文件，例如springmvc的配置文件，若是一个一个的迁移到配置类中，非常麻烦，所以可以使用@ImportResource来直接进行迁移

​	例：beans.xml中包含了所有的单例实体类组件，要是一个一个迁移会很麻烦，在类上面添加@ImportResource("classpath:beans.xml")即可。

## 配置绑定（3个注释）

- **@ConfigurationProperties+@Component**

  这个注解用于解决，当组件的值大量存在于某个properties文件中，在调取这个值的时候会非常麻烦（需要先绑定文件，再在文件中找到对应的属性值），而这个@ConfigurationProperties注解可以实现自动找到配置文件中对应的值。他有几个属性：

  - prefix：前缀，在配置文件中寻找前缀为指定值的属性，然后用前缀对应的属性名匹配值。

    ```java
    //要写@Conponent是因为只有在spring容器中的组件才可以实现springboot的赋值功能
    @Component
    //简而言之，用了这个注释就可以在yml文件中，以自定义的前缀设置此类属性的值
    @ConfigurationProperties(prefix = "mycar")
    public class Car {
        private String brand;
        private Integer price;
    	...//封装
    }
    ```
    
    ```properties
    mycar.brand = BWM
    mycar.price = 2000000
    ```
    
    ```java
    //Controller 
    //组件已经存在于容器中，直接自动匹配即可
     	@Autowired
        Car car;
        @RequestMapping("/car")
        public Car handle02(){
            return car;
        }
    ```

- ### @EnableConfigurationProperties + @ConfigurationProperties

  @EnableConfigurationProperties表示开启属性配置功能，并将指定的类自动注入到容器中

  例：

  ```java
  /*
  @EnableConfigurationProperties这个注释是放在配置类中的，后面的?.class表示开启这个类的属性配置功能，并将这个类实例化为组件注入容器中
  */
  @Configuration
  @EnableConfigurationProperties(Car.class)
  public class MyConfig {
      @Bean("pet")
      public Pet pet(){
          return new Pet("dog");
      }
  }
  ```

  ```java
  //这个类依旧需要加上@ConfigurationProperties注释，但不用再写@Component
  //@Component
  @ConfigurationProperties(prefix = "mycar")
  public class Car {
      private String brand;
      private Integer price;
      ...
      }
  ```

  ```properties
  mycar.brand = BWM
  mycar.price = 2000000
  ```

# 自动配置原理入门

## 引导加载自动配置类

```
@SpringBootApplication
就是以下三个的结合
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan("com.test")
```

```java
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
```

### @SpringBootConfiguration

​	他继承了@Configuration这个注释，代表当前是一个配置类

### @ComponentScan

​	指定扫描包和子包，就是扫描路径

### @EnableAutoConfiguration

```java
//合成注解
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {}
```

- #### @AutoConfigurationPackage

  自动配置包

  ```java
  //合成注解
  @Import({Registrar.class})//给容器中导入(Registrar.class)组件
  //源代码比较复杂，用了一个register方法，找到mainApplication主程序所在的包，并将包下的所有组件全部扫描注入
  //源代码：AutoConfigurationPackages.register(registry, (String[])(new AutoConfigurationPackages.PackageImports(metadata)).getPackageNames().toArray(new String[0]));
  public @interface AutoConfigurationPackage {}
  ```

- **@Import({AutoConfigurationImportSelector.class})**

  ```java
  1.getAutoConfigurationEntry(annotationMetadata)//给容器中批量导入一些组件
  2.再调用List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes)获取到所有需要导入到容器中的配置类
  3.利用工厂加载 Map<String, List<String>> loadSpringFactories(@Nullable ClassLoader classLoader)；得到所有的组件
  4.从META-INF/spring.factories位置来加载一个文件。
    默认扫描我们当前系统里面所有META-INF/spring.factories位置的文件
      spring-boot-autoconfigure-2.5.5.jar包里面也有META-INF/spring.factories
  ```

  spring.factories文件中写死了132个自动加载类（版本不同可能会有数量上的差别）

  ```properties
  # Auto Configure
  org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
  org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
  org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
  org.springframework.boot.autoconfigure.amqp.RabbitAutoConfiguration,\
  org.springframework.boot.autoconfigure.batch.BatchAutoConfiguration,\
  org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration,\
  org.springframework.boot.autoconfigure.cassandra.CassandraAutoConfiguration,\
  org.springframework.boot.autoconfigure.context.ConfigurationPropertiesAutoConfiguration,\
  org.springframework.boot.autoconfigure.context.LifecycleAutoConfiguration,\
  org.springframework.boot.autoconfigure.context.MessageSourceAutoConfiguration,\
  org.springframework.boot.autoconfigure.context.PropertyPlaceholderAutoConfiguration,\
  org.springframework.boot.autoconfigure.couchbase.CouchbaseAutoConfiguration,\
  org.springframework.boot.autoconfigure.dao.PersistenceExceptionTranslationAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.cassandra.CassandraDataAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.cassandra.CassandraReactiveDataAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.cassandra.CassandraReactiveRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.cassandra.CassandraRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.couchbase.CouchbaseDataAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.couchbase.CouchbaseReactiveDataAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.couchbase.CouchbaseReactiveRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.couchbase.CouchbaseRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchDataAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.elasticsearch.ReactiveElasticsearchRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.elasticsearch.ReactiveElasticsearchRestClientAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.jdbc.JdbcRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.jpa.JpaRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.ldap.LdapRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.mongo.MongoDataAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.mongo.MongoReactiveDataAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.mongo.MongoReactiveRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.mongo.MongoRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.neo4j.Neo4jDataAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.neo4j.Neo4jReactiveDataAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.neo4j.Neo4jReactiveRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.neo4j.Neo4jRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.r2dbc.R2dbcDataAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.r2dbc.R2dbcRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.redis.RedisAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.redis.RedisReactiveAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.redis.RedisRepositoriesAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.rest.RepositoryRestMvcAutoConfiguration,\
  org.springframework.boot.autoconfigure.data.web.SpringDataWebAutoConfiguration,\
  org.springframework.boot.autoconfigure.elasticsearch.ElasticsearchRestClientAutoConfiguration,\
  org.springframework.boot.autoconfigure.flyway.FlywayAutoConfiguration,\
  org.springframework.boot.autoconfigure.freemarker.FreeMarkerAutoConfiguration,\
  org.springframework.boot.autoconfigure.groovy.template.GroovyTemplateAutoConfiguration,\
  org.springframework.boot.autoconfigure.gson.GsonAutoConfiguration,\
  org.springframework.boot.autoconfigure.h2.H2ConsoleAutoConfiguration,\
  org.springframework.boot.autoconfigure.hateoas.HypermediaAutoConfiguration,\
  org.springframework.boot.autoconfigure.hazelcast.HazelcastAutoConfiguration,\
  org.springframework.boot.autoconfigure.hazelcast.HazelcastJpaDependencyAutoConfiguration,\
  org.springframework.boot.autoconfigure.http.HttpMessageConvertersAutoConfiguration,\
  org.springframework.boot.autoconfigure.http.codec.CodecsAutoConfiguration,\
  org.springframework.boot.autoconfigure.influx.InfluxDbAutoConfiguration,\
  org.springframework.boot.autoconfigure.info.ProjectInfoAutoConfiguration,\
  org.springframework.boot.autoconfigure.integration.IntegrationAutoConfiguration,\
  org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration,\
  org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration,\
  org.springframework.boot.autoconfigure.jdbc.JdbcTemplateAutoConfiguration,\
  org.springframework.boot.autoconfigure.jdbc.JndiDataSourceAutoConfiguration,\
  org.springframework.boot.autoconfigure.jdbc.XADataSourceAutoConfiguration,\
  org.springframework.boot.autoconfigure.jdbc.DataSourceTransactionManagerAutoConfiguration,\
  org.springframework.boot.autoconfigure.jms.JmsAutoConfiguration,\
  org.springframework.boot.autoconfigure.jmx.JmxAutoConfiguration,\
  org.springframework.boot.autoconfigure.jms.JndiConnectionFactoryAutoConfiguration,\
  org.springframework.boot.autoconfigure.jms.activemq.ActiveMQAutoConfiguration,\
  org.springframework.boot.autoconfigure.jms.artemis.ArtemisAutoConfiguration,\
  org.springframework.boot.autoconfigure.jersey.JerseyAutoConfiguration,\
  org.springframework.boot.autoconfigure.jooq.JooqAutoConfiguration,\
  org.springframework.boot.autoconfigure.jsonb.JsonbAutoConfiguration,\
  org.springframework.boot.autoconfigure.kafka.KafkaAutoConfiguration,\
  org.springframework.boot.autoconfigure.availability.ApplicationAvailabilityAutoConfiguration,\
  org.springframework.boot.autoconfigure.ldap.embedded.EmbeddedLdapAutoConfiguration,\
  org.springframework.boot.autoconfigure.ldap.LdapAutoConfiguration,\
  org.springframework.boot.autoconfigure.liquibase.LiquibaseAutoConfiguration,\
  org.springframework.boot.autoconfigure.mail.MailSenderAutoConfiguration,\
  org.springframework.boot.autoconfigure.mail.MailSenderValidatorAutoConfiguration,\
  org.springframework.boot.autoconfigure.mongo.embedded.EmbeddedMongoAutoConfiguration,\
  org.springframework.boot.autoconfigure.mongo.MongoAutoConfiguration,\
  org.springframework.boot.autoconfigure.mongo.MongoReactiveAutoConfiguration,\
  org.springframework.boot.autoconfigure.mustache.MustacheAutoConfiguration,\
  org.springframework.boot.autoconfigure.neo4j.Neo4jAutoConfiguration,\
  org.springframework.boot.autoconfigure.netty.NettyAutoConfiguration,\
  org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration,\
  org.springframework.boot.autoconfigure.quartz.QuartzAutoConfiguration,\
  org.springframework.boot.autoconfigure.r2dbc.R2dbcAutoConfiguration,\
  org.springframework.boot.autoconfigure.r2dbc.R2dbcTransactionManagerAutoConfiguration,\
  org.springframework.boot.autoconfigure.rsocket.RSocketMessagingAutoConfiguration,\
  org.springframework.boot.autoconfigure.rsocket.RSocketRequesterAutoConfiguration,\
  org.springframework.boot.autoconfigure.rsocket.RSocketServerAutoConfiguration,\
  org.springframework.boot.autoconfigure.rsocket.RSocketStrategiesAutoConfiguration,\
  org.springframework.boot.autoconfigure.security.servlet.SecurityAutoConfiguration,\
  org.springframework.boot.autoconfigure.security.servlet.UserDetailsServiceAutoConfiguration,\
  org.springframework.boot.autoconfigure.security.servlet.SecurityFilterAutoConfiguration,\
  org.springframework.boot.autoconfigure.security.reactive.ReactiveSecurityAutoConfiguration,\
  org.springframework.boot.autoconfigure.security.reactive.ReactiveUserDetailsServiceAutoConfiguration,\
  org.springframework.boot.autoconfigure.security.rsocket.RSocketSecurityAutoConfiguration,\
  org.springframework.boot.autoconfigure.security.saml2.Saml2RelyingPartyAutoConfiguration,\
  org.springframework.boot.autoconfigure.sendgrid.SendGridAutoConfiguration,\
  org.springframework.boot.autoconfigure.session.SessionAutoConfiguration,\
  org.springframework.boot.autoconfigure.security.oauth2.client.servlet.OAuth2ClientAutoConfiguration,\
  org.springframework.boot.autoconfigure.security.oauth2.client.reactive.ReactiveOAuth2ClientAutoConfiguration,\
  org.springframework.boot.autoconfigure.security.oauth2.resource.servlet.OAuth2ResourceServerAutoConfiguration,\
  org.springframework.boot.autoconfigure.security.oauth2.resource.reactive.ReactiveOAuth2ResourceServerAutoConfiguration,\
  org.springframework.boot.autoconfigure.solr.SolrAutoConfiguration,\
  org.springframework.boot.autoconfigure.sql.init.SqlInitializationAutoConfiguration,\
  org.springframework.boot.autoconfigure.task.TaskExecutionAutoConfiguration,\
  org.springframework.boot.autoconfigure.task.TaskSchedulingAutoConfiguration,\
  org.springframework.boot.autoconfigure.thymeleaf.ThymeleafAutoConfiguration,\
  org.springframework.boot.autoconfigure.transaction.TransactionAutoConfiguration,\
  org.springframework.boot.autoconfigure.transaction.jta.JtaAutoConfiguration,\
  org.springframework.boot.autoconfigure.validation.ValidationAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.client.RestTemplateAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.embedded.EmbeddedWebServerFactoryCustomizerAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.reactive.HttpHandlerAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.reactive.ReactiveWebServerFactoryAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.reactive.WebFluxAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.reactive.error.ErrorWebFluxAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.reactive.function.client.ClientHttpConnectorAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.reactive.function.client.WebClientAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.servlet.DispatcherServletAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.servlet.ServletWebServerFactoryAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.servlet.error.ErrorMvcAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.servlet.HttpEncodingAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.servlet.MultipartAutoConfiguration,\
  org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration,\
  org.springframework.boot.autoconfigure.websocket.reactive.WebSocketReactiveAutoConfiguration,\
  org.springframework.boot.autoconfigure.websocket.servlet.WebSocketServletAutoConfiguration,\
  org.springframework.boot.autoconfigure.websocket.servlet.WebSocketMessagingAutoConfiguration,\
  org.springframework.boot.autoconfigure.webservices.WebServicesAutoConfiguration,\
  org.springframework.boot.autoconfigure.webservices.client.WebServiceTemplateAutoConfiguration
  ```

## 按需开启自动配置

​	虽然默认加载会加载很多场景配置，但是最终通过条件注释（@Conditional）会实现按需配置。

​	对于某个场景配置，springboot会通过@ConditionalOnClass、@ConditionalOnBean等注释选择性的加载一些类，从而达到按需加载的目的。例：AOP场景

```java
@Configuration(
    proxyBeanMethods = false
)
@ConditionalOnProperty(
    prefix = "spring.aop",
    name = {"auto"},
    havingValue = "true",
    matchIfMissing = true
)
//首先设置非代理模式
//然后判断类属性中是否有前缀为spring.aop的场景，是否Value为true，是否没写Value等属性，判断要不要往后进行配置
```

```java
//包括他的类中
@Configuration(
        proxyBeanMethods = false
    )
    @ConditionalOnMissingClass({"org.aspectj.weaver.Advice"})
    @ConditionalOnProperty(
        prefix = "spring.aop",
        name = {"proxy-target-class"},
        havingValue = "true",
        matchIfMissing = true
    )
//会继续判断是否没有"org.aspectj.weaver.Advice"这个类，以及是否有满足条件的属性
```

类似Cache等没有用到的场景都是这样进行筛选加载的。

## 修改默认配置

SpringBoot用servlet的方式进行的核心组件就是DispatcherServlet，在springboot的autoconfigure中也会自动配置

以下是SpringBoot中MultipartResolver（文件上传下载）的源码

```java
@Bean
@ConditionalOnBean({MultipartResolver.class})
@ConditionalOnMissingBean(
    name = {"multipartResolver"}
)
public MultipartResolver multipartResolver(MultipartResolver resolver) {
    return resolver;
}
//这段代码的作用就是规范化文件上传下载的解析器类名
//步骤是：现在容器中寻找MultipartResolver类型的组件，在判断是否有命名为multipartResolver的解析器（防止有人起名不规范），他就会将这个不规范的解析器重新赋值给resolver，然后通过方法名重新命名
```

SpringBoot会在底层默认加载好所有的组件，但若是用户自己配置了，则以用户为优先

方式：@Conditional的各种形式

**总结**

- SpringBoot会先加载所有的自动配置类  xxxxAutoConfiguration

- 每个自动配置类按照条件去生效，默认都会绑定配置文件指定的值  xxxProperties中拿值，xxxProperties和配置文件进行绑定

- 生效的配置类就会给容器装配很多组件

- 若是容器中有这些组件，就默认对应的功能已经有了

- 定制化配置

  - 用户直接用@Bean替换底层组件（例如：CharacterEncodingFilter，直接可以在主配置类中修改替换）
  - 用户也可以去看这个组件获取的配置文件是什么值就去properties文件中改

  xxxAutoConfiguration --->组件 ---> xxxProperties里面拿值 ----> application.properties

## SpringBoot布置步骤

1. 引入场景依赖
   https://docs.spring.io/spring-boot/docs/current/reference/html/using.html#using.build-systems.starters
2. 查看配置了哪些（可选）
   - 自己分析，引入的场景都是生效的场景
   - 配置文件中写debug = true开启自动配置报告。Negative（不生效的）、Positive（生效的）
3. 是否需要修改
   - 参考文档修改配置项
     - https://www.yuque.com/r/goto?url=https%3A%2F%2Fdocs.spring.io%2Fspring-boot%2Fdocs%2Fcurrent%2Freference%2Fhtml%2Fappendix-application-properties.html%23common-application-properties
     - 自己分析，xxxProperties绑定了配置文件的那些内容
   - 自定义加入或者替换组件
     - @Bean、@Component
   - 自定义器 **xxxCustomizer** 可以修改组件中的方法
   - ......

# 插件和Spring的使用技巧

## lombok

​	是一个插件，这个插件可以省去很多代码，例如封装代码，toString，HashCode等，使实体类更加简洁

​	在springboot的父依赖中有包含他的版本号，所以不用写版本

​	使用步骤

```xml
<!--1.加入依赖 -->
 <dependency>
     <groupId>org.projectlombok</groupId>
     <artifactId>lombok</artifactId>
 </dependency>
<!--2.下载插件 -->
	在IDEA的Plugins中下载lombok即可
```

- @ToString   //toString方法
- @Data   //属性封装，getter和setter
- @AllArgsConstructor //全参构造
- @NoArgsConstructor  //无参构造
- @EqualsAndHashCode  //重写HashCode

```java
@ToString   //toString方法
@Data   //属性封装，getter和setter
@AllArgsConstructor //全参构造
@NoArgsConstructor  //无参构造
@EqualsAndHashCode  //重写HashCode
@ConfigurationProperties(prefix = "mycar")  //属性赋值
public class Car {
    private String brand;
    private Integer price;
}
```

- Slf4j	//日志输出，可以替代sout

```java
@Slf4j
@RestControllerpublic 
class helloController {
    @Autowired 
    Car car;
    @RequestMapping("/car")
    public Car handle02(){        
        log.info("请求进入！");        
        return car;    
    }}
```

```shell
2021-10-11 22:54:56.791  INFO 2740 --- [nio-8080-exec-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring DispatcherServlet 'dispatcherServlet'
2021-10-11 22:54:56.791  INFO 2740 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Initializing Servlet 'dispatcherServlet'
2021-10-11 22:54:56.792  INFO 2740 --- [nio-8080-exec-1] o.s.web.servlet.DispatcherServlet        : Completed initialization in 1 ms
2021-10-11 22:54:56.806  INFO 2740 --- [nio-8080-exec-1] com.Controller.helloController           : 请求进入！
```

## dev-tools

​	也是一个插件，功能就是实现“热部署”，但并不纯正，对类修改本质上就是重启，对静态页面修改可以快一点而已，真正的“热部署”还得是JRebel。

​	在springboot的父依赖中有包含他的版本号，所以不用写版本

​	使用步骤

```xml
<!--1.加入依赖 -->
<dependency>    
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency><!--以后修改之后可以按 Ctrl+F9 即可重新编译部署-->
```

## Spring Initailizr（项目初始化向导）

IDEA自带插件，快速构建springboot项目，使项目人员专注于业务

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211011232108217.png" alt="image-20211011232108217" style="zoom:70%;" /> 

------

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211011232403588.png" alt="image-20211011232403588" style="zoom:60%;" /> 

根据向导设置好之后就会直接产生对应的工程

**依赖：打包插件也会包含在内**

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211011232615950.png" alt="image-20211011232615950" style="zoom:50%;" /> 

**项目结构：主程序以及需要的注释还有运行代码都会写好**

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211011232803952.png" alt="image-20211011232803952" style="zoom:70%;" /> 

# SpringBoot2的核心功能

## 配置文件

### Properties

同上面properties文件的编写

### YAML

#### 简介

YAML 是 "YAML Ain't Markup Language"（YAML 不是一种标记语言）的递归缩写。在开发的这种语言时，YAML 的意思其实是："Yet Another Markup Language"（仍是一种标记语言）。 

非常适合用来做以数据为中心的配置文件

#### 基本语法

- key: value；kv之间有空格

- 区分大小写

- 使用空格表示层级关系（说是不支持tab，但在idea中tab也行）

- 缩进的空格数不重要，只要相同层级的元素左对齐即可

- ' # '表示注释

- 字符串无需添加引号，如果要加，' ' 和 " " 表示字符内容，会被转义/不转义

  ```yml
  #单引号中的转义字符不会生效，会直接输出
  userName: 'zhangsan \n lisi'
  输出: zhangsan \n lisi
  #双引号中的转义字符\n会生效
  userName: "zhangsan \n lisi"
  输出: zhangsan 
   	 lisi
  ```

#### 数据类型

- 字面量：单个的不可再分的值，date，boolean，string，number，null

  ```yaml
  k: v
  ```

- 对象：键值对集合。map，hash，set，object

  ```yaml
  行内写法：k: {k1: v1,k2: v2,k3: v3}
  #或
  k:
    k1: v1
    k2: v2
    k3: v3
  ```

- 数组：一组按次序排列的值。array，list，queue

  ```
  行内写法：k: [v1,v2,v3]#或k:  - v1  - v2  - v3
  ```

#### 例子

```java
@ConfigurationProperties(prefix = "person")
@Component
@ToString
@Datapublic 
class Person {    
    private String userName;    
    private Boolean boss;    
    private Date birth;    
    private Integer age;    
    private Pet pet;    
    private String[] interests;    
    private List<String> animal;    
    private Map<String, Object> score;    
    private Set<Double> salarys;    
    private Map<String, List<Pet>> allPets;
}
```

```java
@ToString
@Data
public class Pet {
    private String name;
    private Double weight;
}
```

application.yml

```yml
person:
  userName: zhangsan
  boss: true
  birth: 2019/12/9
  age: 18
#  interests: [篮球,足球]
  interests:
    - 篮球
    - 足球
    - 其他
  animal: [猫,狗]
  score:
    english: 80
    math: 90
#  score: {english:80,math:90}
  salarys:
    - 99999
    - 9999.12
  pet:
    name: baby
    weight: 24.12
  allPets:
    sick:
      - {name: d1,weight: 23}
      - name: c1
        weight: 21.12
      - name: in1
        weight: 12
    health:
      - {name: f1,weight: 12}
      - {name: s1,weight: 15}
#在yml中也可以配置组件
spring:
  datasource:
    password: 1213
    username: root
```

结果：![image-20211012141814612](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211012141814612.png)

### 配置提示

对于自己创建的实例，在写yml配置文件时没有提示（缺少配置解析器），可以加入依赖

```xml
<dependency>   
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-configuration-processor</artifactId>  
    <optional>true</optional>
</dependency>
<!-- 这个依赖加入之后，自建的组件属性也会有提示了 -->
```

但是这个依赖在打包时没有用处，所以可以加入一个插件，让spring在打包时忽略配置解析器

```xml
 	<build>        
        <plugins>      
            <plugin>          
                <groupId>org.springframework.boot</groupId>     
                <artifactId>spring-boot-maven-plugin</artifactId>     
                <configuration>              
                    <excludes>              
                        <!--忽略lombok打包插件-->    
                        <exclude>                  
                            <groupId>org.projectlombok</groupId>    
                            <artifactId>lombok</artifactId>       
                        </exclude>                    
                        <!--忽略类属性提示解析器打包插件-->            
                        <exclude>                    
                            <groupId>org.springframework.boot</groupId>  
                            <artifactId>spring-boot-configuration-processor</artifactId>    
                        </exclude>              
                    </excludes>             
                </configuration>         
            </plugin>      
        </plugins>   
</build>
```

## Web开发

### SpringMVC的自动配置

Spring Boot provides auto-configuration for Spring MVC that **works well with most applications.(大多场景我们都无需自定义配置)**

The auto-configuration adds the following features on top of Spring’s defaults:

- 内容协商视图解析器和BeanName视图解析器

- 静态资源（包括webjars）

- 自动注册 `Converter，GenericConverter，Formatter `

- 支持 `HttpMessageConverters` （后来我们配合内容协商理解原理）

- 自动注册 `MessageCodesResolver` （国际化用）

- 静态index.html 页支持

- 自定义 `Favicon`  

- 自动使用 `ConfigurableWebBindingInitializer` ，（DataBinder负责将请求数据绑定到JavaBean上）

> If you want to keep those Spring Boot MVC customizations and make more [MVC customizations](https://docs.spring.io/spring/docs/5.2.9.RELEASE/spring-framework-reference/web.html#mvc) (interceptors, formatters, view controllers, and other features), you can add your own `@Configuration` class of type `WebMvcConfigurer` but **without** `@EnableWebMvc`.
> **不用@EnableWebMvc注解。使用** **@Configuration** **+** **WebMvcConfigurer自定义规则**

> If you want to provide custom instances of `RequestMappingHandlerMapping`, `RequestMappingHandlerAdapter`, or `ExceptionHandlerExceptionResolver`, and still keep the Spring Boot MVC customizations, you can declare a bean of type `WebMvcRegistrations` and use it to provide custom instances of those components.
> **声明 WebMvcRegistrations 改变默认底层组件**

> If you want to take complete control of Spring MVC, you can add your own `@Configuration` annotated with `@EnableWebMvc`, or alternatively add your own `@Configuration`-annotated `DelegatingWebMvcConfiguration` as described in the Javadoc of `@EnableWebMvc`.
> **使用 @EnableWebMvc+@Configuration+DelegatingWebMvcConfiguration 全面接管SpringMVC**

#### 静态资源访问

##### 静态资源目录

只要静态资源放在类路径下：/static` (or `/public` or `/resources` or `/META-INF/resources`

访问 ： 当前项目根路径/ + 静态资源名 

![image-20211012161805401](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211012161805401.png) 

放在以上任意一个文件夹都是可以直接访问的

**原理：**静态映射 /**，即接受所有请求

​	请求进入，先去Controller中找，看能不能处理，若是处理则为动态资源处理。不能处理的所有请求都交给静态资源处理器，若是静态资源也找不到就会返回404.

##### 静态资源访问前缀

默认无前缀（即所有Controller无法处理的请求，静态资源处理器都会处理）

若要修改，只需要在yml文件中修改配置

```yml
spring:
  mvc:
    static-path-pattern: /res/**
    #之后静态资源若是没有res前缀就访问不到（不会给静态资源处理器处理）
```

当前项目 + static-path-pattern + 静态资源名 = 静态资源文件夹下找

##### 更改默认静态资源访问文件夹

默认是在resource下的指定文件夹名中寻找

若是需要修改，只要在yml文件中修改配置

```yml
spring:
  mvc:
    static-path-pattern: /res/**
    #访问静态资源需要加上res前缀
  web:
    resources:
      static-locations: classpath:/abc/ #也可以是个数组[classpath:/abc/,classpath:/abcd/]
      #表示静态资源在目录为/abc/之下寻找
    #这样配置之后，静态资源目录就不是之前系统指定的几个了，而是修改成的resources下的abc目录内寻找
```

##### webjar

静态网络工具（例：jQuery），自动映射	/webjars/**（默认前缀）

网站：https://www.webjars.org/

例子：加入jQuery资源

就是加入了这个以来之后，可以通过网址直接访问这个静态资源

```xml
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>jquery</artifactId>
    <version>3.6.0</version>
</dependency>
```

访问地址：http://localhost:8080/webjars/jquery/3.6.0/jquery.js	前缀后面地址按照依赖包路径写

#### 欢迎页支持

- 静态资源路径下 index.html（将index.html放在静态资源目录下，启动服务时会自动访问这个网页）

  - 可以配置静态资源目录，让其在指定文件夹在寻找这个html

  - 不可以配置静态资源的访问前缀，否则导致不能默认访问index.html

    ```yml
    spring:
      #mvc:	
        #static-path-pattern: /res/**   配置了这个就不能默认访问了
      web:
        resources:
          static-locations: classpath:/abc/
    #    username: root
    ```

#### Favicon

这个是访问网页时的小图标

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211012172019235.png" alt="image-20211012172019235" style="zoom:50%;" /> 

只要把名为favicon.ico的图片文件放入静态资源文件夹中，就可以自动实现网站小图标的显示

```yml
spring:
  mvc:	
    static-path-pattern: /res/**   
    #与上面一样，配置了这个就不默认设置小图标了
```

#### 静态资源配置原理（源代码解析）

- SpringBoot启动默认加载 xxxAutoConfiguration类（自动配置类）

- springMVC功能的自动配置类WebMvcAutoConfiguration，生效

  ```java
  @Configuration(proxyBeanMethods = false)
  @ConditionalOnWebApplication(type = Type.SERVLET)
  @ConditionalOnClass({Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class})
  @ConditionalOnMissingBean({WebMvcConfigurationSupport.class})
  @AutoConfigureOrder(-2147483638)
  @AutoConfigureAfter({DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class, ValidationAutoConfiguration.class})
  public class WebMvcAutoConfiguration {}
  ```

- 给容器中配了什么

  ```java
  @Configuration(proxyBeanMethods = false)
  @Import({WebMvcAutoConfiguration.EnableWebMvcConfiguration.class})
  @EnableConfigurationProperties({WebMvcProperties.class, ResourceProperties.class, WebProperties.class})
  @Order(0) 
  public static class WebMvcAutoConfigurationAdapter implements WebMvcConfigurer, ServletContextAware {}
  ```

- 配置文件的相关属性和什么进行了绑定（源代码）

  - ResourceProperties 绑定了 spring.resources 的属性
  - WebProperties 绑定了 spring.web 的属性
  - WebMvcProperties 绑定了 spring.mvc的属性

```java
//有参构造器中所有的参数值都会从容器中确定
public WebMvcAutoConfigurationAdapter(ResourceProperties resourceProperties, // 绑定了 spring.resources
                                       WebProperties webProperties, // 绑定了 spring.web 的属性
                                       WebMvcProperties mvcProperties, // 绑定了 spring.mvc的属性
                                       ListableBeanFactory beanFactory, //找到组件工厂
                                      // 找到所有的HttpMessageConvert（网页消息转换器）
                                       ObjectProvider<HttpMessageConverters> messageConvertersProvider,
                                      // 找到资源处理器的自定义器
                                      ObjectProvider<WebMvcAutoConfiguration.ResourceHandlerRegistrationCustomizer> resourceHandlerRegistrationCustomizerProvider, 
                                      //找到中心调度器
                                       ObjectProvider<DispatcherServletPath> dispatcherServletPath,
                                      // 给应用注册Servlet、Filter等
                                       ObjectProvider<ServletRegistrationBean<?>> servletRegistrations)
{
            this.resourceProperties = (Resources)(resourceProperties.hasBeenCustomized() ? resourceProperties : webProperties.getResources());
            this.mvcProperties = mvcProperties;
            this.beanFactory = beanFactory;
            this.messageConvertersProvider = messageConvertersProvider;
            this.resourceHandlerRegistrationCustomizer = (WebMvcAutoConfiguration.ResourceHandlerRegistrationCustomizer)resourceHandlerRegistrationCustomizerProvider.getIfAvailable();
            this.dispatcherServletPath = dispatcherServletPath;
            this.servletRegistrations = servletRegistrations;
            this.mvcProperties.checkConfiguration();
        }
```

- 资源处理的默认规则	

```java
 public void addResourceHandlers(ResourceHandlerRegistry registry) {
            if (!this.resourceProperties.isAddMappings()) {
                logger.debug("Default resource handling disabled");
            } else {
                //当isAddMappings属性为true，就添加前缀并访问（这就是为什么之前访问JQ只需要从webjars开始写起的原因）
                //这两个addResourceHandler方法有各自的重载
                this.addResourceHandler(registry, "/webjars/**", "classpath:/META-INF/resources/webjars/");
				    //（访问默认静态资源路径）getStaticPathPattern()
                this.addResourceHandler(registry, this.mvcProperties.getStaticPathPattern(), (registration) -> {				//（访问自定义静态资源路径）getStaticLocations()
                    /*
                    不设置自定义路径就是这4个默认路径
                    private static final String[] CLASSPATH_RESOURCE_LOCATIONS = new String[]{"classpath:/META-INF/resources/", "classpath:/resources/", "classpath:/static/", "classpath:/public/"};
                    */
                    //addResourceLocations方法中获取到了自定义静态资源路径的值，就改变了静态资源的默认访问路径
                    registration.addResourceLocations(this.resourceProperties.getStaticLocations());
                    if (this.servletContext != null) {
                        ServletContextResource resource = new ServletContextResource(this.servletContext, "/");
                        registration.addResourceLocations(new Resource[]{resource});
                    }
                });
            }
        }
```

```yaml
spring:
  web:
    resources:
      add-mappings: false #默认为true
      #通过源码（isAddMappings()）分析，有个 addMappings 属性，类型为boolean，且默认为true
      #于是在yml文件中设置addMapping属性为false时，静态资源全部关闭无法访问，推测这个方法就是开启默认静态资源映射的
```

```java
//Resources的构造方法将默认路径的值放入staticLocation中 
public static class Resources {
        private static final String[] CLASSPATH_RESOURCE_LOCATIONS = new String[]{"classpath:/META-INF/resources/", "classpath:/resources/", "classpath:/static/", "classpath:/public/"};
        private String[] staticLocations;
        ...
        }
public Resources() {
        this.staticLocations = CLASSPATH_RESOURCE_LOCATIONS;
    	...
        }
```

- 欢迎页的处理规则

  ```java
  //HandlerMapping：处理器映射。保存了每个Handler能处理那些请求
  @Bean
  public WelcomePageHandlerMapping welcomePageHandlerMapping(ApplicationContext applicationContext,
                                                             FormattingConversionService mvcConversionService,
                                                             ResourceUrlProvider mvcResourceUrlProvider) 
  {
        WelcomePageHandlerMapping welcomePageHandlerMapping = new WelcomePageHandlerMapping(new TemplateAvailabilityProviders(applicationContext), applicationContext, this.getWelcomePage(), this.mvcProperties.getStaticPathPattern());
        welcomePageHandlerMapping.setInterceptors(this.getInterceptors(mvcConversionService, mvcResourceUrlProvider));
        welcomePageHandlerMapping.setCorsConfigurations(this.getCorsConfigurations());
        return welcomePageHandlerMapping;
          }
  
  
  WelcomePageHandlerMapping(TemplateAvailabilityProviders templateAvailabilityProviders, ApplicationContext applicationContext, Resource welcomePage, String staticPathPattern) {
      //以下代码判断静态资源目录（默认或者自定义）下，是否存在index页面，有就当作默认欢迎页面
      //这也是为什么在修改了静态资源访问前缀之后，很多自动设置（欢迎页、favicon等）就不能设置的原因，源码中已经写死了不能添加前缀
          if (welcomePage != null && "/**".equals(staticPathPattern)) {
              logger.info("Adding welcome page: " + welcomePage);
              this.setRootViewName("forward:index.html");
          } else if (this.welcomeTemplateExists(templateAvailabilityProviders, applicationContext)) {
              logger.info("Adding welcome page template: index");
              this.setRootViewName("index");
          }
      }
  ```

- favicon

  根据静态资源访问路径和前缀访问 /favicon.ico ，之后处理就能完成页面标志的设置



### 请求参数处理

#### rest使用与原理

**根据需要配置HiddenHttpMethodFilter，可以自定义隐藏域的name**

- @xxxMapping；

- Rest风格支持（*使用**HTTP**请求方式动词来表示对资源的操作*）

  - 以前：*/getUser*  *获取用户*    */deleteUser* *删除用户*   */editUser*  *修改用户*      */saveUser* *保存用户*

  - 现在： */user*    *GET-获取用户*    *DELETE-删除用户*     *PUT-修改用户*      *POST-保存用户*

- 核心Filter：**HiddenHttpMethodFilter**

  - **用法：** 表单method=post，隐藏域 _method=put

  Controller中的映射不同请求方法也可以简化

  ```java
  //Controller请求 	
  //@RequestMapping(value = "/user",method = RequestMethod.GET)
  @GetMapping("/user") 	
  //@GetMapping就等同于@RequestMapping(value = "/user",method = RequestMethod.GET)
  public String getUser(){ 
      return "GET-张三";
  }	
  //@RequestMapping(value = "/user",method = RequestMethod.POST)
  @PostMapping("/user")	
  //同上    
  public String saveUser(){    
      return "POST-张三";   
  }    
  //@RequestMapping(value = "/user",method = RequestMethod.PUT)	
  @PutMapping("/user")	
  //同上    
  public String putUser(){     
      return "PUT-张三";   
  }    //@RequestMapping(value = "/user",method = RequestMethod.DELETE)	
  @DeleteMapping("/user")	
  //同上    
  public String deleteUser(){        return "DELETE-张三";    }
  ```

  ```java
  //WebMvcAutoConfiguration中对REST请求的源码	
  @Bean    
  @ConditionalOnMissingBean({HiddenHttpMethodFilter.class})//判断是否有自定义的Filter   
  @ConditionalOnProperty(prefix = "spring.mvc.hiddenmethod.filter",//没有的话就用默认的Filter
                         name = {"enabled"}//默认情况下这个filter的属性为false的，所以需要手动开启
                        )    
  public OrderedHiddenHttpMethodFilter hiddenHttpMethodFilter() {       
      return new OrderedHiddenHttpMethodFilter(); 
  }
  ```

  ```html
  <form action="/user" method="get">    
      <input value="Rest-GET 提交" type="submit">
  </form>
  <form action="/user" method="post"> 
      <input value="Rest-POST 提交" type="submit">
  </form>
  <form action="/user" method="post">
      <!--    隐藏域，向网页发起DELETE请求-->   
      <input name="_method" type="hidden" value="DELETE">  
      
      <input value="Rest-DELETE 提交" type="submit">
  </form>
  <form action="/user" method="post">    
      <!--    隐藏域，向网页发起PUT请求-->   
      <input name="_method" type="hidden" value="PUT">  
      
      <input value="Rest-PUT 提交" type="submit">
  </form>
  ```

  - SpringBoot中手动开启
  
    ```yml
    spring:  
    	mvc:    
    		hiddenmethod:
            	filter:     
                    enabled: true
    ```

  - REST原理（表单提交使用REST的时候）

    - 表单提交带上 **_method=PUT**

    - 请求过来会被HiddenHttpMethodFilter拦截

      - 方法会先判断请求是否正常，并且请求方式是POST

        - 获取 _method 的值
  
        - 兼容（允许进入）以下请求：**PUT , DELETE , PATCH**
  
        - 然后将要使用的请求转化为HttpMethodRequestWrapper类型
  
          ```java
          requestToUse = new HiddenHttpMethodFilter.HttpMethodRequestWrapper(request, method);//这个warpper的父类实现了HttpServletRequest接口，本质上仍然是servlet请求//wrapper没有改变request（POST）请求，但是重写了method的值，这个method值就是 _method 的value值
          private static class HttpMethodRequestWrapper extends HttpServletRequestWrapper {    
              private final String method;    
              public HttpMethodRequestWrapper(HttpServletRequest request, String method) {    
                  super(request);            
                  this.method = method;
              }       
              public String getMethod() {   
                  return this.method;     
              }   
          }
          ```
        
        - 过滤器放行的是wrapper，然后方法调用getMethod判断请求类型时，时调用wrapper的getMethod值，即新的请求类型
        
          ```tex
          捋一捋原理
          
          1.首先要知道，网页请求时都会有一个Filter过滤器来获取form表单的请求方式，默认只有GET和POST两种（不是只能处理GET和POST，而是表单只有GET和POST两种请求方式），也就是说，只有当请求类型与GET或者POST类型相同时，才会处理请求。
          
          2.然后springboot在获取这个请求方式时，先用POST将第一层Filter过去（相当于做了个包装，偷渡过去）
          
          3.然后再在后面用servlet获取value的方式获取到Hidden控件的value值（应该是为了好写，所以写死了那个隐藏的控件name必须是 _method）,判断是否为PUT,DELETE或者PATCH
          
          4.之后就把这个请求的方式放入另一个Filter（HiddenHttpMethodFilter），这个Filter专门处理PUT,DELETE或者PATCH类型的请求
          ```
    
    - Rest使用客户端
    
      客户端与上面的表单请求不一样，他在进入Filter时就已经是对应的请求了（不局限于GET,POST,例如PUT,DELETE都直接进入）,所以他不会进入第二层filter，而是直接进入对应的请求
    
      例如：Postman。
    
      所以就可以理解为什么之前springboot需要手动开启HiddenHttpMethodFilter，因为如是不做网页请求处理，没必要开启这个过滤器

  - 扩展：如何把_method 这个名字换成我们自己喜欢的。
  
    由于这个默认的HiddenHttpMethodFilter是在没有这个组件的情况下默认执行的，所以我们可以自己创建一个HiddenHttpMethodFilter组件，来对其中的methodParam进行修改
  
    这个新的组件仍然使用默认的处理类，但是只是修改了以下他的一个属性值而已
  
    ```java
    //源码 
    public static final String DEFAULT_METHOD_PARAM = "_method";
     private String methodParam = "_method";
    //HiddenHttpMethodFilter中虽然DEFAULT_METHOD_PARAM 是final类型的无法修改，但是他的methodParam提供了一个set方法来修改这个值，也就是说可以在自定义组件中修改这个值，就可以实现自定义 _method 
    ```
    

```java
@Configuration
public class WebConfig{
    @Bean
    public HiddenHttpMethodFilter hiddenHttpMethodFilter(){
        HiddenHttpMethodFilter methodFilter = new HiddenHttpMethodFilter();
        methodFilter.setMethodParam("_m");
        return methodFilter;
    }
}
```

#### 请求映射

Rest源码及原理

![image-20211014151255731](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211014151255731.png) 

```
	以doGet为例，结构如上图，HttpServlet提供了doGet方法，然后子类HttpServletBean继承HttpServlet但没有重写doGet方法，在子类FrameworkServlet中重写了doGet方法，他们都调用了一个processRequest方法，而这个方法的核心就是doService方法，但在本类中他是个抽象接口，所以继承了FrameworkServlet的子类DispatcherServlet实现了这个doServlce方法，对请求进行操作，而doService方法的核心就是doDipatch方法，这个方法中调用了很多处理请求的方法.
```

这个是自定义Controller可以处理的请求

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211014164042249.png" style="zoom:80%;" /> 

Sprintboot自动装配的Mapping

![](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211014164310239.png) 

所有的请求映射都在HandlerMapping中。

- SpringBoot自动配置欢迎页的 WelcomePageHandlerMapping 。访问 /能访问到index.html；
- SpringBoot自动配置了默认 的 RequestMappingHandlerMapping
- 请求进来，挨个尝试所有的HandlerMapping看是否有请求信息。

  - 如果有就找到这个请求对应的handler

  - 如果没有就是下一个 HandlerMapping

- 我们需要一些自定义的映射处理，我们也可以自己给容器中放**HandlerMapping**。自定义 **HandlerMapping**

#### 普通参数与基本注解

##### 注解类型

- @PathVariable（获取路径变量）

  <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211014213513369.png" alt="image-20211014213513369" style="zoom:80%;" /> 

- @RequestHeader（获取网页头部信息，根据value）

  <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211014213617726.png" alt="image-20211014213617726" style="zoom:80%;" /> 

- @RequestParam（获取请求参数，根据value）

  <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211014213755494.png" alt="image-20211014213755494" style="zoom:80%;" /> 

- @CookieValue（获取cookie的值，根据value）

  <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211014213826454.png" alt="image-20211014213826454" style="zoom:80%;" /> 

  <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211014213844471.png" alt="image-20211014213844471" style="zoom:80%;" /> 

- @RequestBody（获取请求体参数post，例如表单信息）

  <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211014214035422.png" alt="image-20211014214035422" style="zoom:80%;" /> 

- @RequestAttribute（获取request域属性，例如某个请求通过rq.setAttribute放入的值，在转发网站中可通过value获取，当然也可以用传统的HttpServletRequest获取）

  <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211014214118486.png" alt="image-20211014214118486" style="zoom:80%;" /> 

- @MatrixVariable（矩阵变量，通过key获取属性值，通过pathVar锁定路径变量）

  <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211014214151685.png" alt="image-20211014214151685" style="zoom:80%;" /> 

  ![image-20211014214219784](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211014214219784.png) 

```html
<!-- @PathVariable和@RequestHeader -->
<a href="/car/4/owner/zhangsan"><span>【@PathVariable】(获取路径变量)||【@RequestHeader】(获取浏览器头部信息)</span></a><br/>

<!-- @RequestParam -->
<a href="/car/4/owner/zhangsan/?age=18&ins=basketbann&ins=ganmes"><span>【@RequestParam】</span></a><br/>
<br/>

<!-- @RequestBody -->
<form method="post" action="/save">
    <h2>测试@RequestBody获取的数据</h2><br/>
    用户名：<input type="text" name="userName"><br/>
    email：<input type="text" name="email"><br/>
    <input type="submit" value="提交">
</form>

<!-- @MatrixVariable -->
<hr />
<a href="/user/sell;id=1001;name=zhangsan,lisi,wangwu"><span>{/user/sell;id=1001;name=zhangsan,lisi,wangwu}</span></a><br/>
<a href="/boss;id=00001;name=zhangsan,wangwu/empe;id=20001;name=lisi"><span>{/boss;id=00001;name=zhangsan,wangwu/empe;id=20001;name=lisi}</span></a><br/>
```

```java
@RestController
public class ParameterTestController {
    @GetMapping("/car/{id}/owner/{username}")
    public Map<String,Object> getCar(@PathVariable("id") Integer id,
                                     @PathVariable("username") String name,
                                     @PathVariable Map<String,String> pv,
                                     @RequestHeader("sec-ch-ua") String sec,
                                     @RequestHeader Map<String,String> head,
                                     @RequestParam("age") Integer age,
                                     @RequestParam("ins")List<String> ins,
                                     @RequestParam Map<String,String> params,
                                     @CookieValue("Idea-8296e771") String Idea,
                                     @CookieValue("Idea-8296e771") Cookie cookie
                                     ){

        Map<String,Object> map = new HashMap<>();
        /*路径信息 @PathVariable*/
        map.put("id",id);
        map.put("name",name);
        map.put("pv",pv);
        /*头部信息 @RequestHeader*/
        map.put("sec",sec);
        map.put("head",head);
        /*参数信息 @RequestParam*/
        map.put("age", age);
        map.put("ins", ins);
        map.put("params", params);
        /*Cookie信息 @CookieValue*/
        map.put("Idea-8296e771",Idea);
        System.out.println(cookie.getName()+" : "+cookie.getValue(
        ));
        return map;
    }
    /*表单信息 @RequestBody*/
     @PostMapping("/save")
    public Map<String,Object> postMethod(@RequestBody String content){
        Map<String,Object> map = new HashMap<>();
        map.put("content",content);
        return map;
    }
    /*矩阵变量 @MatrixVariable*/
    //矩阵变量的用法：/user/sell;id=1001;name=zhangsan
    //在springboot中默认是关闭矩阵变量的，要用的话需要手动开启 因为底层的这句代码：removeSemicolonContent = true，移除（；）封号内容
    //手动开启原理：Springboot中对于路径的处理都是用UrlPathHelper进行解析的
    //  而他的默认属性removeSemicolonContent（是否移除封号内容）这个属性是默认开启的，所以我们需要自定义底层规则
    @GetMapping("/user/{path}")
    //矩阵变量需要注意一点就是，他封号后面的内容，必须是绑定在“路径变量”后面的，直接写路径名是没有用的,属性内容可以是数组
    public Map<String,Object> user(@MatrixVariable("id") Integer id,
                                   @MatrixVariable("name")List<String> name){

        HashMap<String, Object> map = new HashMap<>();
        map.put("userId",id);
        map.put("username",name);
        return map;
    }

    /**
     * 对于属性相同但所属路径不同的可以用pathVar的值来区别开来
     * @param bid
     * @param eid
     * @param bname
     * @param ename
     * @return
     */
    @GetMapping("/{boss}/{emp}")
    public Map<String,Object> boss(@MatrixVariable(value = "id",pathVar = "boss") Integer bid,
                                   @MatrixVariable(value = "id",pathVar = "emp") Integer eid,
                                   @MatrixVariable(value = "name",pathVar = "boss") String bname,
                                   @MatrixVariable(value = "name",pathVar = "emp")String ename){
        HashMap<String, Object> map = new HashMap<>();
        map.put("bossId",bid);
        map.put("empId",eid);
        map.put("bname",bname);
        map.put("ename",ename);
        return map;
    }

}
```

```java
/*Request域信息 @RequestAttribute*/
@Controller
public class RequestController {

    @GetMapping("/goto")
    public String gotoPage(HttpServletRequest request){
        request.setAttribute("msg","成功跳转..");
        request.setAttribute("code","java");
        return "forward:/success";
    }

    @ResponseBody
    @GetMapping("/success")
    public Map<String,Object> success(@RequestAttribute("msg") String msg,
                                      @RequestAttribute("code") String code,
                                      HttpServletRequest request){
        String msg1 = (String) request.getAttribute("msg");
        HashMap<String, Object> map = new HashMap<>();
        map.put("request_msg1",msg1);
        map.put("annotation_msg",msg);
        map.put("code",code);
        return map;
    }
}
```

##### @MatrixVariable配置

**矩阵变量的用法：**
	web端

```html
<a href="localhost:8080/user/sell;id=1001;name=zhangsan"></a>
<!-- web端没有路径变量，要访问指定url，路径变量的值后面跟上封号(;)，后边都是传入的参数值 -->
```

服务器端

```java
@GetMapping("/user/{path}")
//处理映射方法
```

**开启矩阵变量**

​	在springboot中默认是关闭矩阵变量的，要用的话需要手动开启 因为底层的这句代码：removeSemicolonContent = true，移除（；）封号内容
​	手动开启原理：Springboot中对于路径的处理都是用 **UrlPathHelper** 进行解析的，而他的默认属性removeSemicolonContent（是否移除封号内容）这个属性是默认开启的，所以我们需要自定义底层规则

**组件配置方法（两种）**

- 第一种，直接加入自定义组件

  ```java
  @Configuration
  public class WebConfig {
  /**
       * 这个方法的过程解释一下：
       * 通过WebMvcAutoConfigurer（自动配置类）可以知道，其实他管理路径的方法是configurePathMatch
       * 而这个方法中，关于 矩阵变量 的属性修改的方法其实是UrlPathHelper这个类中的方法
       * 这个类中有一个属性 removeSemicolonContent 默认为true，意思是默认移除封号后面的内容
       * 换而言之就是不支持矩阵变量的书写方式，因此我们需要修改这个值，这个值有set方法，
       * 根据之前修改spring底层规则的方法，只需要创建一个webConfiturer类，并找到对应的类PathMatchConfigurer
       * 这个类中对应的方法 getUrlPathHelper() 获取路径帮助器修改RemoveSemicolonContent的值为false
       * @return
       */
      @Bean
      //由于矩阵变量是一个组件，所以要改他的值
      public WebMvcConfigurer webMvcConfigurer(){
          return new WebMvcConfigurer() {
              @Override
              public void configurePathMatch(PathMatchConfigurer configurer) {
                  configurer.getUrlPathHelper().setRemoveSemicolonContent(false);
              }
          };
      }
  }
  ```

- 第二种，实现方法

  ```java
  @Configuration
  //由于WebMvcAutoConfiguration是WebMvcConfigurer接口的子类，所以实现它并不影响其他组件
  public class WebConfig implements WebMvcConfigurer{
      @Override
      public void configurePathMatch(PathMatchConfigurer configurer) {
          configurer.getUrlPathHelper().setRemoveSemicolonContent(false);
      }
  }
  ```

### Servlet  API

包含：WebRequest，ServletRequest，MultipartRequest，HttpSession，pushBuilder，Principal，InputStream，Reader，Locale，TimeZone，ZoneId这些

一些可以直接放入参数的例如HttpSession这类servlet API参数，就是通过以下代码判断解析的

​	在ServletRequestMethodArgumentResolver类方法中判断处理

```java
//判断 
public boolean supportsParameter(MethodParameter parameter) {
        Class<?> paramType = parameter.getParameterType();
        return WebRequest.class.isAssignableFrom(paramType) || 
               ServletRequest.class.isAssignableFrom(paramType) || 
               MultipartRequest.class.isAssignableFrom(paramType) || 
               HttpSession.class.isAssignableFrom(paramType) || 
               pushBuilder != null && pushBuilder.isAssignableFrom(paramType) ||
               Principal.class.isAssignableFrom(paramType) && !parameter.hasParameterAnnotations() ||
               InputStream.class.isAssignableFrom(paramType) ||
               Reader.class.isAssignableFrom(paramType) || HttpMethod.class == paramType ||
               Locale.class == paramType ||
               TimeZone.class == paramType ||
               ZoneId.class == paramType;
    }
```

```java
//处理
@Nullable
private Object resolveArgument(Class<?> paramType, HttpServletRequest request) throws IOException {
    if (HttpSession.class.isAssignableFrom(paramType)) {
        HttpSession session = request.getSession();
        if (session != null && !paramType.isInstance(session)) {
            throw new IllegalStateException("Current session is not of type [" + paramType.getName() + "]: " + session);
        } else {
            return session;
        }
    } else if (pushBuilder != null && pushBuilder.isAssignableFrom(paramType)) {
        return ServletRequestMethodArgumentResolver.PushBuilderDelegate.resolvePushBuilder(request, paramType);
    } else if (InputStream.class.isAssignableFrom(paramType)) {
        InputStream inputStream = request.getInputStream();
        if (inputStream != null && !paramType.isInstance(inputStream)) {
            throw new IllegalStateException("Request input stream is not of type [" + paramType.getName() + "]: " + inputStream);
        } else {
            return inputStream;
        }
    } else if (Reader.class.isAssignableFrom(paramType)) {
        Reader reader = request.getReader();
        if (reader != null && !paramType.isInstance(reader)) {
            throw new IllegalStateException("Request body reader is not of type [" + paramType.getName() + "]: " + reader);
        } else {
            return reader;
        }
    } else if (Principal.class.isAssignableFrom(paramType)) {
        Principal userPrincipal = request.getUserPrincipal();
        if (userPrincipal != null && !paramType.isInstance(userPrincipal)) {
            throw new IllegalStateException("Current user principal is not of type [" + paramType.getName() + "]: " + userPrincipal);
        } else {
            return userPrincipal;
        }
    } else if (HttpMethod.class == paramType) {
        return HttpMethod.resolve(request.getMethod());
    } else if (Locale.class == paramType) {
        return RequestContextUtils.getLocale(request);
    } else {
        TimeZone timeZone;
        if (TimeZone.class == paramType) {
            timeZone = RequestContextUtils.getTimeZone(request);
            return timeZone != null ? timeZone : TimeZone.getDefault();
        } else if (ZoneId.class == paramType) {
            timeZone = RequestContextUtils.getTimeZone(request);
            return timeZone != null ? timeZone.toZoneId() : ZoneId.systemDefault();
        } else {
            throw new UnsupportedOperationException("Unknown parameter type: " + paramType.getName());
        }
    }
}
```

### 复杂参数

- **Map**
- **Model（map,model里面的数据会放在request的请求域 调用request.setAttribute）**
- **Errors/BindingResult**
- **RedirectAttributes（重定向携带数据）**
- **ServletResponse（response）**
- **SessionStatus**
- **UriComponentsBuilder**
- **ServletUriComponentsBuilder**

```java
Map<String,Object> map,  Model model, HttpServletRequest request 这些参数都是可以给request域中放数据
在处理这些数据时，后端可以直接request.getAttribute()取出，前端也可以用EL表达式取出
Cookie也可以通过response放入，取出可以通过参数配置，也可以直接request取出
```

```java
 @GetMapping("/params")
    public String testParams(Map<String ,Object> map,
                             Model model,
                             HttpServletRequest request,
                             HttpServletResponse response
    ){
        map.put("map","map's message");
        model.addAttribute("model","model's message");
        request.setAttribute("req","request's message");
	//"cookie's message"无效，不知道为什么
	// 懂了Cookie值不能带空格
        Cookie cookie = new Cookie("cookie","testCookie");
        response.addCookie(cookie);
        return "forward:/success";
    }
    @ResponseBody
    @GetMapping("/success")
    public Map<String,Object> success( HttpServletRequest request,@CookieValue("cookie")String cookie) {
        HashMap<String, Object> map = new HashMap<>();
        String map1 = (String) request.getAttribute("map");
        String model = (String) request.getAttribute("model");
        String req = (String) request.getAttribute("req");
        String c = cookie;
        map.put("map",map1);
        map.put("model",model);
        map.put("req",req);
        map.put("cookie",c);
        return map;
    }
```

**原理**

​	**Map、Model类型的参数**，会返回 mavContainer.getModel（）---> BindingAwareModelMap 是Model 也是Map

​	**mavContainer**.getModel(); 获取到值

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211015215048782.png" alt="image-20211015215048782" style="zoom:80%;" /> 

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211015215836349.png" alt="image-20211015215836349" style="zoom:80%;" /> 

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211015220955717.png" alt="image-20211015220955717" style="zoom:80%;" /> 

### 自定义对象参数（封装POJO）

可以自动类型转换与格式化，可以级联封装。

```html
<form action="/person" method="post">
    <!--
        private String userName;
        private Boolean boss;
        private String birth;
        private Integer age;
        private Pet pet;
        -->
    name:<label><input type="text" name="userName"/></label><br/>
    boss:<label><input type="text" name="boss"/></label><br/>
    birth:<label><input type="date" name="birth"/></label><br/>
    age:<label><input type="text" name="age"/></label><br/>
    pet.name:<label><input type="text" name="pet.name"/></label><br/>
    pet.weight:<label><input type="text" name="pet.weight"></label><br/>
    <input type="submit" value="提交">
</form>
```

```java
    @PostMapping("/person")
    public Person persion(Person person){
        return person;
}
```

**原理**

```dtd
	Spring内部在接收了 web 的请求之后，获取到这个请求并且创建一个对象参数，属性赋空，然后通过 WebDataBinder binder（ Web 数据绑定器）将请求的参数绑定到指定的 JavaBean 里面，之后这个 web 数组绑定器再利用他的 Converters 将请求数据的类型转换成指定数据类型，最后再次封装进 JavaBean中
	WebDataBinder binder = binderFactory.createBinder ( webRequest, attribute, name );
WebDataBinder :web 数据绑定器，将请求参数的值绑定到指定的 JavaBean 里面
WebDataBinder 利用它里面的 Converters 将请求数据转成指定的数据类型。再次封装到 JavaBean 中
	GenericConversionService ：在设置每一个值的时候，找它里面的所有 converter 那个可以将这个数据类型（ request 带来参数的字符串）转换到指定的类型（ JavaBean -- Integer ）
byte -- > file
@FunctionalInterfacepublic interface Converter<S,T> 这是 Coverter 的总接口
```

**自定义Converters**

自定义转换器，对于某些特殊需求，例如在自定义类属性中，以某些符号为分隔符，分别获取到对应的数据，并赋值给容器中的组件

方法：

```html
pet.weight:<input type="text" name="pet" value="dog,182.2"><br/>
```

```java
    @Bean
    //由于矩阵变量是一个组件，所以要改他的值
    public WebMvcConfigurer webMvcConfigurer(){
        return new WebMvcConfigurer() {
            //自定义转换器
            @Override
            public void addFormatters(FormatterRegistry registry) {
                registry.addConverter(new Converter<String, Pet>() {
                    @Override
                    public Pet convert(String source) {
                        if (StringUtils.hasText(source)) {
                            String[] strings = source.split(",");
                            Pet pet = new Pet();
                            pet.setName(strings[0]);
                            pet.setWeight(Double.valueOf(strings[1]));
                            return pet;
                        }
                        return null;
                    }
                });
            }
    	};
   	}
```



### 参数处理原理

- HandlerMapping中找到能处理请求的Handler（Controller.method()）

- 为当前Handler 找一个适配器 HandlerAdapter； **RequestMappingHandlerAdapter**

- 适配器执行目标方法并确定方法参数的每一个值

  

#### 1. 	HandlerAdapter

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211015145957093.png" alt="image-20211015145957093" style="zoom:80%;" /> 

​	0 - 支持方法上标注@RequestMapping 

​	1 - 支持函数式编程的

​	2/3 - 支持其他

#### 2.	执行目标方法

```jAVA
// Actually invoke the handler.真正执行处理的方法
//在DispatcherServlet 中的 doDispatch
mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
```

```java
mav = invokeHandlerMethod(request, response, handlerMethod); //执行目标方法

//在ServletInvocableHandlerMethod方法中
Object returnValue = invokeForRequest(webRequest, mavContainer, providedArgs);
//获取方法的参数值
Object[] args = getMethodArgumentValues(request, mavContainer, providedArgs);
```

#### 3.	参数解析器

HandlerMethodArgumentResolver

- 确定将要执行的目标方法的每一个参数的值是什么;
- SpringMVC目标方法能写多少种参数类型。取决于参数解析器。

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211015150827758.png" alt="image-20211015150827758" style="zoom:80%;" /> 

- 当前解析器是否支持解析这种参数
- 支持就调用 resolveArgument

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211015154013975.png" alt="image-20211015154013975" style="zoom:80%;" /> 

#### 4.	返回值处理器

确定映射返回值

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211015163809345.png" alt="image-20211015163809345" style="zoom:70%;" /> 



#### 5.	确定目标方法参数的值

```java
//在InvocableHandlerMethod方法中有以下代码，就是获取参数值的
protected Object[] getMethodArgumentValues(NativeWebRequest request, @Nullable ModelAndViewContainer mavContainer,
			Object... providedArgs) throws Exception {

		MethodParameter[] parameters = getMethodParameters();
		if (ObjectUtils.isEmpty(parameters)) {
			return EMPTY_ARGS;
		}

		Object[] args = new Object[parameters.length];
		for (int i = 0; i < parameters.length; i++) {
			MethodParameter parameter = parameters[i];
			parameter.initParameterNameDiscovery(this.parameterNameDiscoverer);
			args[i] = findProvidedArgument(parameter, providedArgs);
			if (args[i] != null) {
				continue;
			}
			if (!this.resolvers.supportsParameter(parameter)) {
				throw new IllegalStateException(formatArgumentError(parameter, "No suitable resolver"));
			}
			try {
				args[i] = this.resolvers.resolveArgument(parameter, mavContainer, request, this.dataBinderFactory);
			}
			catch (Exception ex) {
				// Leave stack trace for later, exception may actually be resolved and handled...
				if (logger.isDebugEnabled()) {
					String exMsg = ex.getMessage();
					if (exMsg != null && !exMsg.contains(parameter.getExecutable().toGenericString())) {
						logger.debug(formatArgumentError(parameter, exMsg));
					}
				}
				throw ex;
			}
		}
		return args;
	}
```

##### 5.1	挨个判断所有参数解析器那个支持解析这个参数

```java
  @Nullable
  private HandlerMethodArgumentResolver getArgumentResolver(MethodParameter parameter) {
    HandlerMethodArgumentResolver result = this.argumentResolverCache.get(parameter);
    if (result == null) {
      for (HandlerMethodArgumentResolver resolver : this.argumentResolvers) {
        if (resolver.supportsParameter(parameter)) {
          result = resolver;
          this.argumentResolverCache.put(parameter, result);
          break;
        }
      }
    }
    return result;
  }
```

##### 5.2	解析这个参数的值

```
调用各自 HandlerMethodArgumentResolver 的 resolveArgument 方法即可
```

#### 6.	目标方法执行完成

将所有的数据都放在 **ModelAndViewContainer**；包含要去的页面地址View。还包含Model数据。

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211015221445975.png" alt="image-20211015221445975" style="zoom:80%;" /> 

#### 7.	处理派发结果

```java
processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);//派发结果处理

renderMergedOutputModel(mergedModel, getRequestToExpose(request), response);//合并model和参数
```

```java
InternalResourceView：
@Override
	protected void renderMergedOutputModel(
			Map<String, Object> model, HttpServletRequest request, HttpServletResponse response) throws Exception {
		// Expose the model object as request attributes.
		exposeModelAsRequestAttributes(model, request);
		// Expose helpers as request attributes, if any.
		exposeHelpers(request);
		// Determine the path for the request dispatcher.
		String dispatcherPath = prepareForRendering(request, response);
		// Obtain a RequestDispatcher for the target resource (typically a JSP).
		RequestDispatcher rd = getRequestDispatcher(request, dispatcherPath);
		if (rd == null) {
			throw new ServletException("Could not get RequestDispatcher for [" + getUrl() +
					"]: Check that the corresponding file exists within your web application archive!");
		}
		// If already included or response already committed, perform include, else forward.
		if (useInclude(request, response)) {
			response.setContentType(getContentType());
			if (logger.isDebugEnabled()) {
				logger.debug("Including [" + getUrl() + "]");
			}
			rd.include(request, response);
		}
		else {
			// Note: The forwarded resource is supposed to determine the content type itself.
			if (logger.isDebugEnabled()) {
				logger.debug("Forwarding to [" + getUrl() + "]");
			}
			rd.forward(request, response);
		}
	}
```

```java
暴露模型作为请求域属性
// Expose the model object as request attributes.
    exposeModelAsRequestAttributes(model, request);
```

```java
protected void exposeModelAsRequestAttributes(Map<String, Object> model,
			HttpServletRequest request) throws Exception {
    //model中的所有数据遍历挨个放在请求域中
		model.forEach((name, value) -> {
			if (value != null) {
				request.setAttribute(name, value);
			}
			else {
				request.removeAttribute(name);
			}
		});
	}
```

#### 总结流程

```
	1.先找到处理映射请求的解析器（handlerAdapter），用于判断是否为RequestMapping标记
	2.然后对请求进行处理，先获取参数的值（key，value），并放入数组，然后挨个将数组中的值与解析器进行匹配，看用什么注释，匹配上了就交给他处理
	3.最后对映射返回值进行处理（ModelAndView、View、Model...）
```

### 数据响应与内容协商

#### 响应JSON

方式：@ResponseBody + jackson.jar

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<!-- 在springboot的web开发场景中，已经自动加入了json依赖，所以只要有这个场景依赖就可以了 -->
```

```java
//@RestController其实就已经包含了ResponseBody，对于json数据可以直接响应
//为了演示，暂时先不用@RestController,以@Controller为例
@Controller
public class ResponseTestController {
    @ResponseBody
    @RequestMapping("/test/pet")
    public Pet testPet(){
        Pet pet = new Pet();
        pet.setName("王超");
        pet.setWeight(100.0);
        return pet;
    }
}
//给前端自动返回json数据
```



##### 返回值解析器

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1605151359370-01cd1fbe-628a-4eea-9430-d79a78f59125.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_25%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:50%;" /> 

```java
try {//处理返回值
    this.returnValueHandlers.handleReturnValue(
    returnValue, getReturnValueType(returnValue), mavContainer, webRequest);
	}
```

```java
  @Override//处理的具体方法
  public void handleReturnValue(@Nullable Object returnValue, 
                                MethodParameter returnType,
                                ModelAndViewContainer mavContainer, 
                                NativeWebRequest webRequest) throws Exception {
      HandlerMethodReturnValueHandler handler = selectHandler(returnValue, returnType);
      if (handler == null) {
          throw new IllegalArgumentException("Unknown return value type: " +
                                             returnType.getParameterType().getName());
      }
      handler.handleReturnValue(returnValue, returnType, mavContainer, webRequest);
  }
```

```java
//RequestResponseBodyMethodProcessor    
@Override
  public void handleReturnValue(@Nullable Object returnValue, 
                                MethodParameter returnType,
                                ModelAndViewContainer mavContainer, 
                                NativeWebRequest webRequest)
      throws IOException, HttpMediaTypeNotAcceptableException, HttpMessageNotWritableException {
    mavContainer.setRequestHandled(true);
    ServletServerHttpRequest inputMessage = createInputMessage(webRequest);
    ServletServerHttpResponse outputMessage = createOutputMessage(webRequest);
    // Try even with null return value. ResponseBodyAdvice could get involved.
        // 使用消息转换器进行写出操作
    writeWithMessageConverters(returnValue, returnType, inputMessage, outputMessage);
  }
```

##### 返回值解析器原理

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1605151728659-68c8ce8a-1b2b-4ab0-b86d-c3a875184672.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_23%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" /> 

- 返回值处理器判断是否支持这种类型返回值 supportsReturnType
- 返回值处理器调用 handleReturnValue 进行处理
- RequestResponseBodyMethodProcessor 可以处理返回值标了@ResponseBody 注解的。
  - 利用 MessageConverters 进行处理 将数据写为json
    - 内容协商（浏览器默认会以请求头的方式告诉服务器他能接受什么样的内容类型）
    - 服务器最终根据自己自身的能力，决定服务器能生产出什么样内容类型的数据，
    - SpringMVC会挨个遍历所有容器底层的 HttpMessageConverter ，看谁能处理？
      - 得到MappingJackson2HttpMessageConverter可以将对象写为json
      - 利用MappingJackson2HttpMessageConverter将对象转为json再写出去。

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1605163005521-a20d1d8e-0494-43d0-8135-308e7a22e896.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_32%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" /> 

#### SpringMVC到底支持哪些返回值

```java
ModelAndView
Model
View
ResponseEntity 
ResponseBodyEmitter
StreamingResponseBody
HttpEntity
HttpHeaders
Callable
DeferredResult
ListenableFuture
CompletionStage
WebAsyncTask
//有 @ModelAttribute 且为对象类型的
@ResponseBody 注解 ---> RequestResponseBodyMethodProcessor；
```

#### HTTPMessageConverter原理

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1605163447900-e2748217-0f31-4abb-9cce-546b4d790d0b.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_19%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" /> 

HttpMessageConverter: 看是否支持将 此 Class类型的对象，转为MediaType类型的数据。

例子：Person对象转为JSON。或者 JSON转为Person

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1605163584708-e19770d6-6b35-4caa-bf21-266b73cb1ef1.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_17%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" /> 

0 - 只支持Byte类型的1 - String

2 - String

3 - Resource

4 - ResourceRegion

5 - DOMSource.**class \** SAXSource.**class**) \ StAXSource.**class \**StreamSource.**class \**Source.**class**

**6 -** MultiValueMap

7 - **true** 

**8 - true**

**9 - 支持注解方式xml处理的。**

最终 MappingJackson2HttpMessageConverter  把对象转为JSON（利用底层的jackson的objectMapper转换的）

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1605164243168-1a31e9af-54a4-463e-b65a-c28ca7a8a2fa.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_34%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" /> 

#### 内容协商

其实本质上就是解析

 以xml格式为例（包含但不仅限于此类型）

加入这个依赖之后，网页会优先将自定义类的数据转换为xml类型

```xml
<!-- 也是springboot-web包下的，不需要加版本号 -->
<dependency>
    <groupId>com.fasterxml.jackson.dataformat</groupId>
    <artifactId>jackson-dataformat-xml</artifactId>
</dependency>
```

```java
@ResponseBody
@RequestMapping("/test/pet")
public Pet testPet(Map<String,String> map){
    Pet pet = new Pet();
    pet.setName("王超");
    pet.setWeight(100.0);
    map.put("message","fail");
    return pet;
}
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211017101823177.png" alt="image-20211017101823177" style="zoom:80%;" /> 

##### postman分别测试返回json和xml

只需要改变请求头中Accept字段。Http协议中规定的，告诉服务器本客户端可以接收的数据类型。

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1605173127653-8a06cd0f-b8e1-4e22-9728-069b942eba3f.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_33%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" /> 

##### 开启浏览器参数方式内容协商功能

​	为了方便内容协商，开启基于请求参数的内容协商功能。这个方式会让内容协商管理器会多一种策略ParameterContent，专门用于处理format后面设置的参数类型

```yml
spring:
    contentnegotiation:
      favor-parameter: true  #开启请求参数内容协商模式
```

例子：设置页面能接受的格式（在后面加上一个参数：format=xxx ）

- http://localhost:8080/test/pet?format=json

- http://localhost:8080/test/pet?format=xml

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1605230907471-b0ed34bc-6782-40e7-84b7-615726312f01.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_22%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" /> 

确定客户端接收什么样的内容类型:

1、Parameter策略优先确定是要返回json数据（获取请求头中的format的值）

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605231074299-25f5b062-2de1-4a09-91bf-11e018d6ec0e.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_18%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10) 

2、最终进行内容协商返回给客户端json即可

##### 内容协商原理

- 1、判断当前响应头中是否已经有确定的媒体类型。MediaType
- **2、获取客户端（PostMan、浏览器）支持接收的内容类型。（获取客户端Accept请求头字段）【application/xml】**

- - **contentNegotiationManager 内容协商管理器 默认使用基于请求头的策略**

- - <img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1605230462280-ef98de47-6717-4e27-b4ec-3eb0690b55d0.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_15%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom: 80%;" /> 

- - **HeaderContentNegotiationStrategy  确定客户端可以接收的内容类型** 

    <img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1605230546376-65dcf657-7653-4a58-837a-f5657778201a.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_28%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" /> 

- 3、遍历循环所有当前系统的 **MessageConverter**，看谁支持操作这个对象（Person）
- 4、找到支持操作Person的converter，把converter支持的媒体类型统计出来。
- 5、客户端需要【application/xml】。服务端能力【10种、json、xml】

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1605173876646-f63575e2-50c8-44d5-9603-c2d11a78adae.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_20%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" /> 

- 6、进行内容协商的最佳匹配媒体类型
- 7、用 支持 将对象转为 最佳匹配媒体类型 的converter。调用它进行转化 。

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605173657818-73331882-6086-490c-973b-af46ccf07b32.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_18%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10) 

导入了jackson处理xml的包，xml的converter就会自动进来

```java
//WebMvcConfigurationSupport源码
jackson2XmlPresent = ClassUtils.isPresent("com.fasterxml.jackson.dataformat.xml.XmlMapper", classLoader);
if (jackson2XmlPresent) {
      Jackson2ObjectMapperBuilder builder = Jackson2ObjectMapperBuilder.xml();
      if (this.applicationContext != null) {
        builder.applicationContext(this.applicationContext);
      }
      messageConverters.add(new MappingJackson2XmlHttpMessageConverter(builder.build()));
    }
```



##### 自定义 MessageConverter

**实现多协议数据兼容。json、xml、x-guigu**

**0、**@ResponseBody 响应数据出去 调用 **RequestResponseBodyMethodProcessor** 处理

1、Processor 处理方法返回值。通过 **MessageConverter** 处理

2、所有 **MessageConverter** 合起来可以支持各种媒体类型数据的操作（读、写）

3、内容协商找到最终的 **messageConverter**；

SpringMVC的什么功能。一个入口给容器中添加一个  WebMvcConfigurer

```java
//放在配置类里面的配置器，之前有讲过 
public WebMvcConfigurer webMvcConfigurer(){
//        mvcUrlPathHelper.setRemoveSemicolonContent(false);
        return new WebMvcConfigurer() {
            //自定义MessageConfigure
            //拓展，还有一个configureMessageConverters是覆盖默认的converters从，注意区分
            @Override
            public void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
                //新建自定义的MessageConverter类并实现HttpMessageConverter，然后将组件放入converters中
                converters.add(new MyMessageConverter());
            }
        }
 }
/**
* 实现步骤
* 1.添加自定义的MessageConverter进系统层
* 2.系统底层会统计出所有MessageConverter能操作的类型
* 3.客户端内容协商 [王超--->100.0]
*
*/
```

MessageConverter.java的配置

```java
//需要继承HttpMessageConverter<Pet>，并设置接收数据的类型
public class MyMessageConverter implements HttpMessageConverter<Pet> {
    //自定义协议是否可读
    @Override
    public boolean canRead(Class<?> clazz, MediaType mediaType) {
        return false;
    }
    //自定义协议对pet类是否可写、修改
    @Override
    public boolean canWrite(Class<?> clazz, MediaType mediaType) {
        return clazz.isAssignableFrom(Pet.class);
    }
	//这里是设置对应请求响应的格式
    @Override
    public List<MediaType> getSupportedMediaTypes() {
        return MediaType.parseMediaTypes("application/x-guigu");
    }
    //自定义协议的读取
    @Override
    public Pet read(Class<? extends Pet> clazz, HttpInputMessage inputMessage) throws IOException, HttpMessageNotReadableException {
        return null;
    }

    //自定义协议的写出
    @Override
    public void write(Pet pet, MediaType contentType, HttpOutputMessage outputMessage) throws IOException, HttpMessageNotWritableException {
        String data = pet.getName()+"----->"+pet.getWeight();
        OutputStream body =  outputMessage.getBody();
        body.write(data.getBytes());
    }
}
```

-------

对于参数类型支持默认有两种（JSON和xml(xml还是依赖加入的)）

第一个 extendMessageConverters 方法是自定义一个媒体类型的converter协商处理办法

第二个 configureContentNegotiation 方法是自定义参数（format=？？）处理时，选择对应的converter媒体类型

​	

```java
 public WebMvcConfigurer webMvcConfigurer(){
//        mvcUrlPathHelper.setRemoveSemicolonContent(false);
        return new WebMvcConfigurer() {
            //自定义MessageConfigure
            //拓展，还有一个configureMessageConverters是覆盖默认的converters从，注意区分
            @Override
            public void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
                //新建自定义的MessageConverter类并实现HttpMessageConverter，然后将组件放入converters中
                converters.add(new MyMessageConverter());
            }
            
            //自定义网页协商策略，参数处理
            @Override
            public void configureContentNegotiation(ContentNegotiationConfigurer configurer) {
                //这个方法会覆盖默认的几个参数类型协商(json和xml),所以需要手动加上
                Map<String, MediaType> mediaTypeMap = new HashMap<>();
                mediaTypeMap.put("json",MediaType.APPLICATION_JSON);
                mediaTypeMap.put("xml",MediaType.APPLICATION_ATOM_XML);
                //这个是规定format的访问形式，format=gg，则对应的converter就是处理"application/x-guigu"数据的
                mediaTypeMap.put("gg",MediaType.parseMediaType("application/x-guigu"));
                ParameterContentNegotiationStrategy parameterContent = new ParameterContentNegotiationStrategy(mediaTypeMap);
                configurer.strategies((Arrays.asList(parameterContent)));
            }
        }
 
```

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1605260623995-8b1f7cec-9713-4f94-9cf1-8dbc496bd245.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_18%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" /> 

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1605261062877-0a27cc41-51cb-4018-a9af-4e0338a247cd.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_27%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" /> 
Postman中设置网页处理格式之后，显示的数据

![image-20211017164251479](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211017164251479.png) 

**注意**	

​	configureContentNegotiation方法需要注意，他会默认覆盖掉所有的网页端解析器，即用postman发送数据，不管规定的accept格式是什么类型，他都解析不了，服务器端只会默认格式为 * / * ，也就是说服务器的所有解析器都可以匹配上，而json是第一个解析器，所以不管设置accept是什么格式（甚至是未知格式），他都以json数据返回

​	**解决方法：**

将基于请求头策略的解析器也放入到解析策略中

```java
@Override
public void configureContentNegotiation(ContentNegotiationConfigurer configurer) {
	...
        //请求头策略的解析器
    ParameterContentNegotiationStrategy parameterContent = new ParameterContentNegotiationStrategy(mediaTypeMap);
    	//放入请求解析策略中
    configurer.strategies((Arrays.asList(parameterContent,headerStrategy)));
            }
```

**有可能我们添加的自定义的功能会覆盖默认很多功能，导致一些默认的功能失效。**

**大家考虑，上述功能除了我们完全自定义外？SpringBoot有没有为我们提供基于配置文件的快速修改媒体类型功能？怎么配置呢？【提示：参照SpringBoot官方文档web开发内容协商章节】**

### 视图解析与模板引擎

视图解析：**SpringBoot默认不支持JSP，需要引入第三方模板引擎技术实现页面渲染**

#### 视图解析

**视图解析原理流程**

1、目标方法处理的过程中，所有数据都会被放在 **ModelAndViewContainer 里面。包括数据和视图地址**

**2、方法的参数是一个自定义类型对象（从请求参数中确定的），把他重新放在** **ModelAndViewContainer** 

**3、任何目标方法执行完成以后都会返回 ModelAndView（数据和视图地址）。**

**4、processDispatchResult  处理派发结果（页面改如何响应）**

- **render**(**mv**, request, response); 进行页面渲染逻辑
  - 根据方法的String返回值得到 **View** 对象【定义了页面的渲染逻辑】
    - 所有的视图解析器尝试是否能根据当前返回值得到**View**对象
    - 得到了  **redirect:/main.html** --> Thymeleaf new **RedirectView**()
    - ContentNegotiationViewResolver 里面包含了下面所有的视图解析器，内部还是利用下面所有视图解析器得到视图对象。
    - view.render(mv.getModelInternal(), request, response);   视图对象调用自定义的render进行页面渲染工作
      - **RedirectView 如何渲染【重定向到一个页面】**
      - **获取目标url地址**
      - **response.sendRedirect(encodedURL);**
- **视图解析：**
- **返回值以 forward: 开始： new InternalResourceView(forwardUrl); -->  转发request.getRequestDispatcher(path).forward(request, response);** 
- **返回值以** **redirect: 开始：** **new RedirectView() --》 render就是重定向** 
- **返回值是普通字符串： new ThymeleafView（）--->** 

自定义视图解析器+自定义视图: **大厂学院。**

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1605679959020-54b96fe7-f2fc-4b4d-a392-426e1d5413de.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_23%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" />  

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1605679471537-7db702dc-b165-4dc6-b64a-26459ee5fd6c.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_17%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" /> 

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1605679913592-151a616a-c754-4da3-a2c1-91dc0230a48d.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_22%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" />  

#### 模板引擎-Thymeleaf 

##### thymeleaf简介 

现代化、服务端Java模板引擎,需要加入xmlns

```xml
<html lang="en" xmlns:th="http://www.thymeleaf.org">
```

##### 基本语法 

**表达式** 

| 表达式名字 | 语法   | 用途                               |
| ---------- | ------ | ---------------------------------- |
| 变量取值   | ${...} | 获取请求域、session域、对象等值    |
| 选择变量   | *{...} | 获取上下文对象值                   |
| 消息       | #{...} | 获取国际化等值                     |
| 链接       | @{...} | 生成链接                           |
| 片段表达式 | ~{...} | jsp:include 作用，引入公共页面片段 |

**字面量** 

- 文本值: 'one text' , 'Another one!' ,…数字: 0 , 34 , 3.0 , 12.3 ,…布尔值: true , false

- 空值: null

- 变量： one，two，.... 变量不能有空格

**文本操作** 

- 字符串拼接: +

- 变量替换: |The name is ${name}| 

**数学运算** 

- 运算符: + , - , * , / , %

**布尔运算** 

- 运算符:  and , or

- 一元运算: ! , not 

**比较运算** 

- 比较: > , < , >= , <= ( gt , lt , ge , le )等式: == , != ( eq , ne ) 

**条件运算** 

- If-then: (if) ? (then)
- If-then-else: (if) ? (then) : (else)
- Default: (value) ?: (defaultvalue) 

**特殊操作** 

- 无操作： _

**设置属性值-th:attr** 

- 设置单个值

```html
<form action="subscribe.html" th:attr="action=@{/subscribe}">
  <fieldset>
    <input type="text" name="email" />
    <input type="submit" value="Subscribe!" th:attr="value=#{subscribe.submit}"/>
  </fieldset>
</form>
```

- 设置多个值

```html
<img src="../../images/gtvglogo.png"  th:attr="src=@{/images/gtvglogo.png},title=#{logo},alt=#{logo}" />
```

- 以上两个的代替写法 th:xxxx

```html
<input type="submit" value="Subscribe!" th:value="#{subscribe.submit}"/>
<form action="subscribe.html" th:action="@{/subscribe}">
```

- 所有h5兼容的标签写法

https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#setting-value-to-specific-attributes

**迭代** 

```html
<tr th:each="prod : ${prods}">
        <td th:text="${prod.name}">Onions</td>
        <td th:text="${prod.price}">2.41</td>
        <td th:text="${prod.inStock}? #{true} : #{false}">yes</td>
</tr>
```

```html
<tr th:each="prod,iterStat : ${prods}" th:class="${iterStat.odd}? 'odd'">
  <td th:text="${prod.name}">Onions</td>
  <td th:text="${prod.price}">2.41</td>
  <td th:text="${prod.inStock}? #{true} : #{false}">yes</td>
</tr>
```

**条件运算** 

```html
<a href="comments.html" th:href="@{/product/comments(prodId=${prod.id})}"
th:if="${not #lists.isEmpty(prod.comments)}">view</a>
```

```html
<div th:switch="${user.role}">
  <p th:case="'admin'">User is an administrator</p>
  <p th:case="#{roles.manager}">User is a manager</p>
  <p th:case="*">User is some other thing</p>
</div>
```

 **直接文字加入**

```html
<!-- 没有标签，直接用，可以直接获取到session中的数据 -->
[[${session.loginUser.userName}]]
```

**属性优先级**

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1354552/1605498132699-4fae6085-a207-456c-89fa-e571ff1663da.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_44%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10) 

##### **thymeleaf使用**

1.  引入Starter 

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
```

2.	自动配置好了thymeleaf 

```java
@Configuration(proxyBeanMethods = false)
@EnableConfigurationProperties(ThymeleafProperties.class)
@ConditionalOnClass({ TemplateMode.class, SpringTemplateEngine.class })
@AutoConfigureAfter({ WebMvcAutoConfiguration.class, WebFluxAutoConfiguration.class })
public class ThymeleafAutoConfiguration { }
```

自动配好的策略

- 所有thymeleaf的配置值都在 ThymeleafProperties

- 配置好了 SpringTemplateEngine ( Thymeleaf模板引擎 )

- 配好了 ThymeleafViewResolver （ Thymeleaf视图解析器 ）

- 我们只需要直接开发页面


```java
 	<!-- 页面前缀，就是说以后的静态html页面要放入这个文件夹内 -->
  public static final String DEFAULT_PREFIX = "classpath:/templates/";
	<!-- 默认访问的是后缀为.html的页面 -->
  public static final String DEFAULT_SUFFIX = ".html";  //xxx.html
```

 3.	页面开发 

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1 th:text="${msg}"></h1>
<h2>
<a th:href="${link}">百度1</a><br/>
<a th:href="@{https://www.baidu.com}">百度2</a>
<a th:href="@{gotosuccess}">百度3</a>
</h2>
</body>
</html>
```

```java
@GetMapping("/gotosuccess")
public String success(Model model){
    model.addAttribute("msg","Thymeleaf Test Success!");
    model.addAttribute("link","https://www.baidu.com");
    return "success";
}
```

```yml
server:
  servlet:
    context-path: /world
    #可以在这里设置网页访问前缀，这样网页所有的请求都必须以 /world 为前缀，否则无法访问
```

##### **例子**

登录案例（简单的逻辑处理）

Controller

```java
@Controller
public class IndexController {
    /**
     * 跳转登录页
     * @return
     */
    @GetMapping(value = {"/","/login"})
    public String loginPage(){
        return "/login";//这个是网页
    }
    //表单数据处理，用于页面url显示 和 区分判断页面
    @PostMapping("/login")
    public String indexpage(User user, Model model,HttpSession session){
        if(StringUtils.hasText(user.getUserName()) && StringUtils.hasText(user.getUserName()) && "111".equals(user.getPassword())){
            session.setAttribute("loginUser",user);
            return "redirect:/index.html";//这个是方法映射
            //redirect本质上是两次请求，所以只能用session，session是一次会话中的数据，只要网页不关就不会消失
            /*
            model中的数据只能在下一次的网页中获取到，是request级别的，之所以这里model数据娶不到，
            就是因为redirect本质上是两次请求，数据在第一次请求后消失了*/
            /*
            * 那么这里可以换成forward吗？
            * 这里的forward是Post请求，那么他的服务方也应该是POST请求，但是若是POST请求，就无法实现直接访问返回登陆页面的需求的需求,所以，这是这里这么写的原因
            * forward的请求是取决于最初那个网页的请求方式
            * redirect的请求是get请求
            * */
        }else {
            model.addAttribute("msg", "用户名密码错误！");
            return "/login";//这个也是网页
        }
    }
    //这个方法的本质是为了不重复提交表单，若是用了forward，在刷新页面时就会重复提交表单。
    @GetMapping("/index.html")
    public String mainPage(HttpSession session,Model model) {
        if (session.getAttribute("loginUser") != null) {
            return "/index";//网页
        } else {
            model.addAttribute("msg", "请重新登录!");
            return "/login";//网页
        }
    }
}
```

Login

```html

<form class="form-signin" action="/login.html" th:action="@{/login}" method="post"></form>
<!-- 没全复制，只要直到这里的表单是POST方法就行 -->
```

Index

```html
[[${session.loginUser.userName}]]
<!-- 这里是thymeleaf直接内容取值的方法 -->
```

```tex
说一下需求：
1.默认应该从login页面登陆，若是从index页面登录，直接去到login页面，并提示需要重新登录
2.从login页面登陆，输入正确的用户名密码可以进入index页面，且能获取到登录用户的用户名
	若是用户名或密码错误，提示‘用户名密码错误’，
思路：业务逻辑
· 默认从login登录，因此必有一个@PostMapping负责login入口
· 能直接而进入index页面然后返回login，说明index的入口方法也必须是@GetMapping
· 为了让index不因为刷新而每次都重新提交表单，应该有一个中转站，不重复获取表单内容
· 这个中转站首先要接收表单的数据，但由于GET请求方式已经被使用，所以只能用POST
数据：
1.登陆失败提示用户名密码错误：model比较好，因为他只作用于下一个页面
2.登陆成功，index显示用户名：session比较好，既可以防止重复提交表，也可以在之后的页面中使用
3.直接访问index，跳转login显示重新登陆：model比较好，因为他只作用于下一个页面
```

抽取公共页面

对于相同的内容，例如：管理页面的导航栏，头部信息等，可以提取在同一个页面中，要使用时直接取。

需要加入thymeleaf的xmlns：*xmlns:th="http://www.thymeleaf.org"*

```html
<!-- 公共页面有2种命名方式 -->
<!-- 第一种 -->
<div th:fragment="commonheader"></div>
<!-- 第二种 -->
<div id="headAndSection"></div>
```

在页面中取公共信息时,有三种方式 语法为: **th:include/replace/insert = "common :: (#) commonheader "**

```html
<!-- 第一种方式:include,官方现在不推荐使用,作用是直接将内容复制过来 -->
<div th:include="common :: commonheader"></div>
<!-- 第二种方式:replace,在公共页面中用什么标签存放,他会把那个存放标签也带过来 -->
<div th:replace="common :: #headAndSection"></div>
<!-- 第三种方式:insert,在哪个公共页面中用什么标签存放,他会把那个页面标签和存放标签都带过来 -->
<div th:insert="common :: commonscript"></div>
<!-- 三种都不影响使用.其中那个 # 是表示选择器,当公共页面中存放公共部分的命名方式为id时,需要用#来选择
	若是用th:fragment标签表示时,可以直接取
-->
```

Thymleaf的循环遍历以及自动编号

再将数据放入model之后就可以在前端用Thymleaf获取

遍历语法：< th:each = "user,stats:${user}" >

```html
<tr class="gradeX" th:each="user,stats:${user}">
    <td th:text="${stats.count}"></td>
    <td th:text="${user.getUserName()}"></td>
    <td th:text="${user.getPassword()}">Win 95+</td>
</tr>
```

#### 拦截器

##### HandlerInterceptor 接口

有三个方法：

- preHandle：在目标方法处理之前进行处理
- postHandle：在目标方法处理完成后，到达视图前进行处理
- afterCompletion：在整个请求结束之后进行处理

步骤

1. 编写一个拦截器实现HandlerInterceptor接口
2. 拦截器配置到容器中（实现WebMvcConfigure的addInterceptors方法）
3. 指定拦截器规则【如果是拦截所有请求，那么静态资源也会被拦截】

```java
@Slf4j
public class LoginIntercepter implements HandlerInterceptor {

    //目标方法执行之前进行处理
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        log.info("拦截的请求{}",request.getRequestURI());
        //登陆检查逻辑
        HttpSession session = request.getSession();
        Object loginUser = session.getAttribute("loginUser");
        if (loginUser != null) {
            //放行
            return true;
        }
        //拦截
        //重定向到登陆界面
        request.setAttribute("msg","请先登录");
        request.getRequestDispatcher("/").forward(request,response);
        return false;
    }
	//目标方法执行之后进行处理
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        log.info("postHandler请求处理{}",modelAndView);
    }
	//请求处理完成后进行处理
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        log.info("afterCompletion请求处理,异常{}",ex);
    }
}
```

##### 配置拦截器

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginIntercepter())
                .addPathPatterns("/**")//所有请求都会被拦截，包括静态资源
                .excludePathPatterns("/","/login","/css/**","/fonts/**","/images/**","/js/**");//放出静态资源
    }
}
```

##### 拦截器原理

1. 根据当前请求，找到**HandlerExecutionChain（拦截器执行链）【**可以处理请求的handler以及handler的所有 拦截器】
2. 先来**顺序执行** 所有拦截器的 preHandle方法
   - 如果当前拦截器prehandler返回为true。则执行下一个拦截器的preHandle
   - 如果当前拦截器返回为false。直接    倒序执行所有已经执行了的拦截器的  afterCompletion；

3. **如果任何一个拦截器返回false。直接跳出不执行目标方法**

4. 所有拦截器都返回True。执行目标方法**

5. **倒序执行所有拦截器的postHandle方法。**

6. **前面的步骤有任何异常都会直接倒序触发** afterCompletion

7. 页面成功渲染完成以后，也会倒序触发 afterCompletion

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1605764129365-5b31a748-1541-4bee-9692-1917b3364bc6.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_44%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" /> 

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1605765121071-64cfc649-4892-49a3-ac08-88b52fb4286f.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_35%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" /> 

#### 文件上传

文件上传例子

```java
//文件上传测试
@Slf4j
@Controller
public class FormTestController {
    //上传主页跳转
    @GetMapping("/form_layouts")
    public String form_layouts(){
        return "/form/form_layouts";
    }
    @PostMapping("/upload")
    public String upload(@RequestParam("email") String email,//注意一下@RequestParam是由name属性确定数据位置的
                         @RequestParam("username") String username,
                         @RequestPart("headerImg") MultipartFile headerImg,
                         @RequestPart("photos") MultipartFile[] photos) throws IOException {
            log.info("email={},username={},headerImg={},photos=}"
                     ,email,username,headerImg.getSize(),photos.length);
            if(!headerImg.isEmpty()){
                headerImg.transferTo(new File("F:\\springBootTestFile\\" + headerImg.getOriginalFilename()));
            }
            if(photos.length>0){
                for (MultipartFile photo:photos){
                    if(!photo.isEmpty()){
                        photo.transferTo(new File("F:\\springBootTestFile\\"+photo.getOriginalFilename()));
                    }
                }
            }
            return "/index";
    }
}
```

##### 自动配置原理

**文件上传自动配置类-MultipartAutoConfiguration-****MultipartProperties**

- 自动配置好了 **StandardServletMultipartResolver   【文件上传解析器】**
- **原理步骤**

- - **1、请求进来使用文件上传解析器判断（**isMultipart**）并封装（**resolveMultipart，**返回**MultipartHttpServletRequest**）文件上传请求**
  - **2、参数解析器来解析请求中的文件内容封装成MultipartFile**

- - **3、将request中文件信息封装为一个Map；**MultiValueMap<String, MultipartFile>

**最后MultiPartFile的FileCopyUtils方法来实现文件流的拷贝**

### 异常处理

#### 错误处理

##### 默认规则

- 默认情况下，Spring Boot提供`/error`处理所有错误的映射
- 对于机器客户端，它将生成JSON响应，其中包含错误，HTTP状态和异常消息的详细信息。对于浏览器客户端，响应一个“ whitelabel”错误视图，以HTML格式呈现相同的数据

- ![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606024421363-77083c34-0b0e-4698-bb72-42da351d3944.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_14%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10) ![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606024616835-bc491bf0-c3b1-4ac3-b886-d4ff3c9874ce.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_28%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)
- **要对其进行自定义，添加View解析为error**

- 要完全替换默认行为，可以实现 `ErrorController `并注册该类型的Bean定义，或添加`ErrorAttributes类型的组件`以使用现有机制但替换其内容。
- error/下的4xx，5xx页面会被自动解析；

- - ![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606024592756-d4ab8a6b-ec37-426b-8b39-010463603d57.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_15%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10) 

##### 定制错误处理逻辑

- 定义错误页

- - error/404.html   error/5xx.html；有精确的错误状态码页面就匹配精确，没有就找 4xx.html；如果都没有就触发白页

- @ControllerAdvice+@ExceptionHandler处理全局异常；底层是 **①ExceptionHandlerExceptionResolver 支持的**
- @ResponseStatus+自定义异常 ；底层是 **②ResponseStatusExceptionResolver ，把responsestatus注解的信息底层调用** **response.sendError(statusCode, resolvedReason)；tomcat发送的/error**

- Spring底层的异常，如 参数类型转换异常（Tomcat异常）；**③DefaultHandlerExceptionResolver 处理框架底层的异常。**

  - response.sendError(HttpServletResponse.SC_BAD_REQUEST, ex.getMessage()); 

  ![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606114118010-f4aaf5ee-2747-4402-bc82-08321b2490ed.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_19%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10) 

- 自定义实现 HandlerExceptionResolver 处理异常；可以作为默认的全局异常处理规则

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606114688649-e6502134-88b3-48db-a463-04c23eddedc7.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_16%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10) 

**ErrorViewResolver**  实现自定义处理异常；

- response.sendError 。error请求就会转给controller
- 你的异常没有任何人能处理。tomcat底层 response.sendError。error请求就会转给controller
- **basicErrorController 要去的页面地址是** **ErrorViewResolver 处理的**；

##### 几个异常例子

​	例子1：**@ControllerAdvice+@ExceptionHandler** 这是全局异常处理，所有网页只要出现指定异常，都来此处理

```java
@Slf4j
@ControllerAdvice       //标记为异常处理控制器注解
public class GlobalExceptionHandller {
    //这是对指定错误类型进行处理，这里一个是空指针，一个是数学异常
    @ExceptionHandler({NullPointerException.class,ArithmeticException.class})
    public String handerArithException(Exception e){
        log.info("异常为：{}",e.getMessage());
        //这个方法也是个映射，Stirng类型也会返回到地址网页
        return "/login";
    }
}
```

​	例子2：@ResponseStatus+自定义异常 

```java
@ResponseStatus(value = HttpStatus.FORBIDDEN,reason = "Too Many People!")
//指定异常类型，编写异常原因
public class UserTooManyException extends RuntimeException{
    public UserTooManyException() {
    }
    public UserTooManyException(String msg) {
        super(msg);
    }
}
//发生异常时直接抛出即可 throw new UserTooManyException();
```

​	例子3：完全自定义异常处理机制

```java
//数字越小，优先级越大
//由于若是不设置优先级的话，这个自定义异常处理是优先级最次的（tomcat异常和java异常都不属于他），也就是只能处理自定义异常
//若是设置最高级的话，那么这个异常就是处理所有的异常
@Order(value = Ordered.HIGHEST_PRECEDENCE)
@Component
public class CustomerHandlerException implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
        try {
            response.sendError(511,"自定义error处理");
        } catch (IOException e) {
            e.printStackTrace();
        }
        return new ModelAndView();//不写视图就是默认的view，也会默认找error页面
    }
}
```

​	这个错误机制设置之后，可以在error界面直接获取到

```html
<h2 th:text="${status}">OOOPS!!!</h2>
<h3 th:text="${message}">Something went wrong.</h3>
```

- 

##### 异常处理自动配置原理

- **ErrorMvcAutoConfiguration  自动配置异常处理规则**
  - **容器中的组件（错误信息）：类型：DefaultErrorAttributes ->** **id：errorAttributes**
    - **public class** **DefaultErrorAttributes** **implements** **ErrorAttributes**, **HandlerExceptionResolver**
    - **DefaultErrorAttributes**：定义错误页面中可以包含哪些数据。
    - <img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1606044430037-8d599e30-1679-407c-96b7-4df345848fa4.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_28%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:50%;" /> 
    - <img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1606044487738-8cb1dcda-08c5-4104-a634-b2468512e60f.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_31%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:50%;" /> 
  - **容器中的组件（错误页路径）：类型：BasicErrorController --> id：basicErrorController（json+白页 适配响应）**
    - **处理默认/error 路径的请求；页面响应** new ModelAndView("error", model)；
  
      - **容器中有组件 View**->**id是error**；（响应默认错误页）
      - **容器中放组件 BeanNameViewResolver（视图解析器）；按照返回的视图名作为组件的id去容器中找View对象。**
    - **容器中的组件（错误页）：**类型：**DefaultErrorViewResolver -> id：**conventionErrorViewResolver
        - 如果发生错误，会以HTTP的状态码 作为视图页地址（viewName），找到真正的页面
        - error/404、5xx.html

##### 异常处理步骤流程

1. 执行目标方法，目标方法运行期间有任何异常都会被catch、而且标志当前请求结束；并且用 **dispatchException** 

   进入视图解析流程（页面渲染？） 

   processDispatchResult(processedRequest, response, mappedHandler, **mv**, **dispatchException**);

2. **mv** = **processHandlerException**；处理handler发生的异常，处理完成返回ModelAndView；

3. 遍历所有的 **handlerExceptionResolvers，看谁能处理当前异常【HandlerExceptionResolver处理器异常解析器】**

   <img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1606047252166-ce71c3a1-0e0e-4499-90f4-6d80014ca19f.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_28%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:67%;" /> 

4. **系统默认的  异常解析器；**

   <img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1606047109161-c68a46c1-202a-4db1-bbeb-23fcae49bbe9.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_17%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:67%;" /> 

   - **DefaultErrorAttributes先来处理异常。把异常信息保存到request域，并且返回null；**

   - **默认没有任何人能处理异常，所以异常会被抛出**

     - **如果没有任何人能处理最终底层就会发送 /error 请求。会被底层的BasicErrorController处理**

     - **解析错误视图；遍历所有的  ErrorViewResolver  看谁能解析。**

       <img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1606047900473-e31c1dc3-7a5f-4f70-97de-5203429781fa.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_14%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" /> 

     - **默认的** **DefaultErrorViewResolver ,作用是把响应状态码作为错误页的地址，error/500.html** 

     - **模板引擎最终响应这个页面** **error/500.html** 

### Web原生组件注入（Servlet、Filter、Listener）

#### 使用Servlet API

- @ServletComponentScan(basePackages = **"com.atguigu.admin"**) :**指定原生Servlet组件都放在那里**
- @WebServlet**(urlPatterns = "/my")**：**效果：直接响应**，**但是没有经过Spring的拦截器**
- @WebFilter**(urlPatterns={"/css/\*","/images/\*"})**
- @WebListener

**推荐这种方式**

Servlet

```java
@WebServlet(urlPatterns = "/ms")//访问路径
public class MyServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.getWriter().write("servlet on...");//请求处理
    }
}
```

Listener

```java
@Slf4j
@WebListener
public class MyListener implements ServletContextListener {//项目监听器
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        log.info("项目初始化成功！");
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
//        ServletContextListener.super.contextDestroyed(sce);
        log.info("项目已被销毁！");
    }
}
```

Filter

```java
@Slf4j
@WebFilter(urlPatterns = {"/css/*","/ms"})
public class MyFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        log.info("myfilter initialized");
    }
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        log.info("myfilter is working on...");
        //这里在处理完请求之后需要在将请求放行，不然数据无法到达
        filterChain.doFilter(servletRequest,servletResponse);
    }
    @Override
    public void destroy() {
        log.info("myfilter has been destroyed");
    }
}
```

主入口

```java
//这个扫描注解是扫描指定包下所有的web原生组件
@ServletComponentScan(basePackages = "com.mcl.springbootwebexample")
@SpringBootApplication
public class SpringBootWebExampleApplication {
    public static void main(String[] args) {
        SpringApplication.run(SpringBootWebExampleApplication.class, args);
    }
}
```

#### 使用RegistrationBean

`ServletRegistrationBean`, `FilterRegistrationBean`, and `ServletListenerRegistrationBean`

以上这些类型直接放在配置类中，返回他们的实例就可以生效了

*用以上方法的话，就不用再上面加上@WebServlet、@WebFilter、@WebListener

​	甚至可以不加@ServletComponentScan来设置他的扫描路径，他不与这种方式匹配

​	**这里的Configuration不要设置proxyBeanMethod，它默认为true，即所有的组件默认都是单实例，若是为flase，那么每次访问servlet都是访问的新的组件，功能不一定会无效，但是会让Spring容器中充满了大量的冗余组件** 

```java
@Configuration
public class ServletConfig {
    //@Bean不要缺了 ，缺了不生效（不放入spring容器中）
    @Bean
    public ServletRegistrationBean myServlet(){
        MyServlet myServlet = new MyServlet();
        //这里的"/ms1","ms"，都代表了这个servlet的路径，即访问这个路径时，这个servlet就会产生作用
        return new ServletRegistrationBean(myServlet,"/ms1","/ms");
    }
    @Bean
    public FilterRegistrationBean myFilter(){
        MyFilter myFilter = new MyFilter();
        //可以拦截指定的servlet，如下即可
//        return new FilterRegistrationBean(myFilter,myServlet());//这是拦截指定servlet的拦截器
        //也可以拦截自定义路径下的资源，如下
        //但这个filterRegistrationBean方法只有一个带参数的构造方法，所以需要用setUrlPatterns放入指定格式的路径
        FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean(myFilter);
        filterRegistrationBean.setUrlPatterns(Arrays.asList("/ms1","/css/*"));//设置自定义拦截路径
        return filterRegistrationBean;
    }
    @Bean
    public ServletListenerRegistrationBean myListener(){
        //创建的Listener监听谁，这里直接放入就行
        MyListener myListener = new MyListener();
        return new ServletListenerRegistrationBean(myListener);
    }
}
```

#### 扩展DispatchServlet原理

DispatchServlet 如何注册进来

首先要知道的是，项目启动时，会有两个servlet在容器中，一个是默认的DispatchServlet，一个是自定义的MyServlet

**对于DispatchServlet（Spring流程）**

- 容器中自动配置了  DispatcherServlet  属性绑定到 WebMvcProperties；对应的配置文件配置项是 **spring.mvc。**
- 通过 **ServletRegistrationBean**<DispatcherServlet> 把 DispatcherServlet  配置进来。

- 默认映射的是 / 路径。

**MyServlet（Tomcat流程）**

- 多个Servlet都能处理到同一层路径，精确优选原则

  A： /my/

  B： /my/1

  访问 **/my/1** 则直接进入**/my/1**，访问 **/my/2** 则访问 **/my**

*所以对于为什么加入自定义servlet不经过spring拦截器就是这个道理

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1606284869220-8b63d54b-39c4-40f6-b226-f5f095ef9304.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_32%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:67%;" /> 

### 嵌入式Servlet容器

#### 切换嵌入式Servlet容器

- 默认支持的webServer

- - `Tomcat`, `Jetty`, or `Undertow`
  - `ServletWebServerApplicationContext 容器启动寻找ServletWebServerFactory 并引导创建服务器`

- 切换服务器

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1606280937533-504d0889-b893-4a01-af68-2fc31ffce9fc.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_26%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="img" style="zoom:67%;" /> 

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <!-- 排除tomcat服务器 -->
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<!-- 加入新的undertow服务器 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-undertow</artifactId>
</dependency>
```

- 原理

- - SpringBoot应用启动发现当前是Web应用。web场景包-导入tomcat
  - web应用会创建一个web版的ioc容器 `ServletWebServerApplicationContext` 
  - `ServletWebServerApplicationContext` 启动的时候寻找 `ServletWebServerFactory（Servlet 的web服务器工厂---> Servlet 的web服务器）` 
  - SpringBoot底层默认有很多的WebServer工厂：`TomcatServletWebServerFactory`, `JettyServletWebServerFactory`, or `UndertowServletWebServerFactory`
  - `底层直接会有一个自动配置类。ServletWebServerFactoryAutoConfiguration`
  - `ServletWebServerFactoryAutoConfiguration`导入了`ServletWebServerFactoryConfiguration`（配置类
  - `ServletWebServerFactoryConfiguration`配置类 根据动态判断系统中到底导入了那个Web服务器的包。（默认是web-starter导入tomcat包），容器中就有 `TomcatServletWebServerFactory`
  - `TomcatServletWebServerFactory` 创建出Tomcat服务器并启动；`TomcatWebServer` 的构造器拥有初始化方法`initialize---this.tomcat.start();`
  - 内嵌服务器，就是手动把启动服务器的代码调用（tomcat核心jar包存在或者切换其他服务器依赖）

#### **定制Servlet容器**

- 实现  **WebServerFactoryCustomizer< ConfigurableServletWebServerFactory > **

- - 把配置文件的值和**`ServletWebServerFactory 进行绑定`**

    ```java
    @Component
    public class testConfig implements WebServerFactoryCustomizer<ConfigurableWebServerFactory> {
        @Override
        public void customize(ConfigurableWebServerFactory factory) {
            factory.setPort(9000);
        }
    }
    ```

- 修改配置文件 **server.xxx**（类似server.port = 9000 等）

  ```yaml
  server.port = 9000
  ```

- 直接自定义 **ConfigurableServletWebServerFactory** 

  ```java
  @Component
  public class testConfig implements ConfigurableTomcatWebServerFactory {
  	//有很多默认实现的方法，根据需要改就行了，这里就不直接放代码了，费空间
      @Override
      public void setPort(int port) {
          port=9000;
      }
  }
  ```

### 定制化原理

#### 定制化的常见方式 

- 修改配置文件；例：xxx.yml、applicationContext.properties

- **xxxxxCustomizer；**例：WebServerFactoryCustomizer，见上

- **修改底层request，response，exception处理方式**（不熟不建议用）

  配置类中存放@WebMvcRegistrations，或者创建一个类加入这个组件也是一样的

  ```java
  @Configuration
  public class WebConfig implements WebMvcConfigurer {
      @Bean
      public WebMvcRegistrations webMvcRegistrations(){
          return new WebMvcRegistrations(){
              @Override
              public RequestMappingHandlerMapping getRequestMappingHandlerMapping() {
                  return WebMvcRegistrations.super.getRequestMappingHandlerMapping();
              }
              @Override
              public RequestMappingHandlerAdapter getRequestMappingHandlerAdapter() {
                  return WebMvcRegistrations.super.getRequestMappingHandlerAdapter();
              }
              @Override
              public ExceptionHandlerExceptionResolver getExceptionHandlerExceptionResolver() {
                  return WebMvcRegistrations.super.getExceptionHandlerExceptionResolver();
              }
          };
      }
  }
  ```

- **编写自定义的配置类   xxxConfiguration +** **@Bean替换、增加容器中默认组件,视图解析器** 

  ```java
  @Configuration
  public class WebConfig implements WebMvcConfigurer{
  	//自定义访问前缀
      //用于设置各种请求（PUT等）
      @Bean
      public HiddenHttpMethodFilter hiddenHttpMethodFilter(){
          HiddenHttpMethodFilter methodFilter = new HiddenHttpMethodFilter();
          methodFilter.setMethodParam("_m");
          return methodFilter;
      }
  }
  ```

- **Web应用 编写一个配置类实现** **WebMvcConfigurer 即可定制化web功能+ @Bean给容器中再扩展一些组件**

  ```java
  @Configuration
  public class WebConfig implements WebMvcConfigurer {
      @Override
      public void addInterceptors(InterceptorRegistry registry) {
          registry.addInterceptor(new LoginIntercepter())
                  .addPathPatterns("/**")//所有请求都会被拦截，包括静态资源
                  .excludePathPatterns("/","/login","/css/**","/fonts/**","/images/**","/js/**");
      }
  }
  ```

- **@EnableWebMvc + WebMvcConfigurer + @Bean**  可以**全面接管SpringMVC**，所有规则全部自己重新配置，实现定制和扩展功能

-  即之前springboot自动配置的所有组件包括静态资源访问路径，欢迎页，错误页等等都会失效，需要完全由自己制定。**慎用**	

- **原理**

- 1. WebMvcAutoConfiguration  默认的SpringMVC的自动配置功能类。静态资源、欢迎页.....

  2. 一旦使用 @EnableWebMvc 会 @Import(DelegatingWebMvcConfiguration.**class**)

  3. **DelegatingWebMvcConfiguration** 的 作用，只保证SpringMVC最基本的使用

     - 把所有系统中的 WebMvcConfigurer 拿过来。所有功能的定制都是这些 WebMvcConfigurer  合起来一起生效
     - 自动配置了一些非常底层的组件。**RequestMappingHandlerMapping**、这些组件依赖的组件都是从容器中获取
     - **public class** DelegatingWebMvcConfiguration **extends** **WebMvcConfigurationSupport**

  4. **`WebMvcAutoConfiguration`** 里面的配置要能生效必须满足以下条件成立

     **`@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)`**

     而**`DelegatingWebMvcConfiguration`**继承了**`WebMvcConfigurationSupport`**，所以**`DelegatingWebMvcConfiguration`**使**`WebMvcAutoConfiguration`**失效

  5. **@EnableWebMvc**  导致了 **WebMvcAutoConfiguration  没有生效。**

- - ```java
    @EnableWebMvc
    @Configuration
    public class WebConfig implements WebMvcConfigurer {
        @Override
        public void addResourceHandlers(ResourceHandlerRegistry registry) {
            //这句话的意思是 默认加载以/aa/为前缀的请求下 在/static/目录下的所有静态资源
            //但是若是设置了拦截器，也要把这个请求排除掉，不然进不去
            registry.addResourceHandler("/aa/**").addResourceLocations("classpath:/static/");
        }
    }
    ```

### 原理分析套路

​	**场景starter** **- xxxxAutoConfiguration - 导入xxx组件 - 绑定xxxProperties --** **绑定配置文件项** 





## 数据访问

### SQL

#### **maven依赖设置**

​	**默认连接池：HikariDataSource**

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jdbc</artifactId>
        </dependency>
```

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1606366100317-5e0199fa-6709-4d32-bce3-bb262e2e5e6a.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_20%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:67%;" /> 

JDBC依赖包不带数据库驱动，因为官方也不知道要用什么数据库，所以数据库需要自己导入。

```xml
<!--本机springboot默认版本-->
<mysql.version>8.0.22</mysql.version>
<!--我自己加入的依赖-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.27</version>
</dependency>
<!--若想要修改版本
	1、直接依赖引入具体版本（maven的就近依赖原则）
	2、重新声明版本（maven的属性的就近优先原则）
	以下为修改例子，我还是用的8.xx的数据库
-->
    <properties>
        <java.version>1.8</java.version>
        <mysql.version>5.1.49</mysql.version>
    </properties>
```

springboot场景包中有自带的mysql版本号，但是要注意与要使用的数据库版本号对应！

#### 分析自动配置

##### 自动配置的类

- DataSourceAutoConfiguration ： 数据源的自动配置

- - 修改数据源相关的配置：**spring.datasource**
  - **数据库连接池的配置，是自己容器中没有DataSource才自动配置的**

- - 底层配置好的连接池是：**HikariDataSource**

    ```java
      @Configuration(proxyBeanMethods = false)
      @Conditional(PooledDataSourceCondition.class)
      @ConditionalOnMissingBean({ DataSource.class, XADataSource.class })
      @Import({ DataSourceConfiguration.Hikari.class, DataSourceConfiguration.Tomcat.class,
          DataSourceConfiguration.Dbcp2.class, DataSourceConfiguration.OracleUcp.class,
          DataSourceConfiguration.Generic.class, DataSourceJmxConfiguration.class })
      protected static class PooledDataSourceConfiguration{}
    ```

  - DataSourceTransactionManagerAutoConfiguration： 事务管理器的自动配置

  - JdbcTemplateAutoConfiguration： **JdbcTemplate的自动配置，可以来对数据库进行crud**（轻量级开发工具）

    - 可以修改这个配置项@ConfigurationProperties(prefix = **"spring.jdbc"**) 来修改JdbcTemplate
    - @Bean@Primary    JdbcTemplate；容器中有这个组件

  - JndiDataSourceAutoConfiguration： jndi的自动配置

  - XADataSourceAutoConfiguration： 分布式事务相关的

##### 修改配置项

```yml
spring:
  datasource:
  #还能拼错我是没想到的：emploryment？
    url: jdbc:mysql://localhost:3306/emploryment
    username: mcl
    password: DNF321
    driver-class-name: com.mysql.cj.jdbc.Driver
```

##### 测试

```java
    @Autowired
    JdbcTemplate jdbcTemplate;
    @Test
    void jdbcTest(){
        Integer counts = jdbcTemplate.queryForObject("select count(*) from employment",Integer.class);
        log.info("记录数为：{}",counts);
    }
```

```shell
#默认数据库连接池Hikari
2021-10-24 15:17:48.838  INFO 11864 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Starting...
2021-10-24 15:17:49.300  INFO 11864 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Start completed.
#查询到的数据
2021-10-24 15:17:49.905  INFO 11864 --- [           main] m.s.SpringBootWebExampleApplicationTests : 记录数为：18
```

#### 使用Druid数据源

##### 自定义方式

1. 加入依赖

   ```xml
   <dependency>
          <groupId>com.alibaba</groupId>
          <artifactId>druid</artifactId>
          <version>1.2.6</version>
   </dependency>
   ```

2. 加入组件，配置类的设置

   ```java
   @Controller
   public class DuridDataSourceConfig {
       @Bean
       //这个就是用来将druid的配置绑定在spring.datasource的属性下
       @ConfigurationProperties("spring.datasource")
       public DataSource druidDataSource() throws SQLException {
           DruidDataSource druidDataSource = new DruidDataSource();
           //stat打开sql监控功能,wall打开防火墙
           druidDataSource.addFilters("stat,wall");
           return druidDataSource;
       }
       /**
        * 配置druid监控页面
        * @return
        */
       @Bean
       public ServletRegistrationBean statViewServlet(){
           StatViewServlet viewServlet = new StatViewServlet();
           ServletRegistrationBean<StatViewServlet> statViewServlet = new ServletRegistrationBean<>(viewServlet,"/druid/*");
           //设置访问durid监控页面的登录名密码
           statViewServlet.addInitParameter("loginUsername","admin");
           statViewServlet.addInitParameter("loginPassword","111");
           return statViewServlet;
       }
   
       /**
        * web应用监控开启配置
        * @return
        */
       @Bean
       public FilterRegistrationBean webStatFilter(){
           WebStatFilter webStatFilter = new WebStatFilter();
           FilterRegistrationBean<WebStatFilter> registrationBean = new FilterRegistrationBean<>(webStatFilter);
           registrationBean.addInitParameter("exclusions","*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*");
           registrationBean.setUrlPatterns(Arrays.asList("/*"));
           return registrationBean;
       }
   
   }
   
   ```

   ```yml
   #数据库的yml配置
   spring:
     datasource:
       url: jdbc:mysql://localhost:3306/emploryment
       username: mcl
       password: DNF321
       driver-class-name: com.mysql.cj.jdbc.Driver
   #    开启druid的sql监控和防火墙，用于替代组件中单独的add功能
   #    filters: stat,wall
   #    max-active: 12
   ```

   

##### starter方式

1. 引入druid-starter

   ```xml
   <dependency>
       <groupId>com.alibaba</groupId>
       <artifactId>druid-spring-boot-starter</artifactId>
       <version>1.1.17</version>
   </dependency>
   ```

2. 分析自动配置

- 扩展配置项 **spring.datasource.druid**

- DruidDataSourceAutoConfigure引入了以下4个类

  - DruidSpringAopConfiguration.**class**,   监控SpringBean的；配置项：**spring.datasource.druid.aop-patterns**
  - DruidStatViewServletConfiguration.**class**, 监控页的配置：**spring.datasource.druid.stat-view-servlet；默认开启**
  -  DruidWebStatFilterConfiguration.**class**, web监控配置；**spring.datasource.druid.web-stat-filter；默认开启**
  - DruidFilterConfiguration.**class**}) 所有Druid自己filter的配置

  ```java
      private static final String FILTER_STAT_PREFIX = "spring.datasource.druid.filter.stat";
      private static final String FILTER_CONFIG_PREFIX = "spring.datasource.druid.filter.config";
      private static final String FILTER_ENCODING_PREFIX = "spring.datasource.druid.filter.encoding";
      private static final String FILTER_SLF4J_PREFIX = "spring.datasource.druid.filter.slf4j";
      private static final String FILTER_LOG4J_PREFIX = "spring.datasource.druid.filter.log4j";
      private static final String FILTER_LOG4J2_PREFIX = "spring.datasource.druid.filter.log4j2";
      private static final String FILTER_COMMONS_LOG_PREFIX = "spring.datasource.druid.filter.commons-log";
      private static final String FILTER_WALL_PREFIX = "spring.datasource.druid.filter.wall";
  ```

3. 配置示例

```yml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/emploryment
    username: mcl
    password: DNF321
    driver-class-name: com.mysql.cj.jdbc.Driver
    druid:
      #对这个路径下的文件做切面,开启SpringBean监控
      aop-patterns: com.mcl.springbootwebexample.*
      #stat开启sql监控，wall开启防火墙
      filters: stat,wall
      #druid监控视图（页面）
      stat-view-servlet:
        #开启druid监控视图
        enabled: true
        #登录druid监控视图得账号密码
        login-username: admin
        login-password: 111
        #关闭数据重置操作
        reset-enable: false

      #Web应用监控设置
      web-stat-filter:
        #卡其web监控
        enabled: true
        #监控所有请求
        url-pattern: /*
        #设置静态资源访问不需要监控
        exclusions: '*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*'

      filter:
        #监视器中对sql得细致配置
        stat:
          #1000毫秒之外响应属于慢语句
          slow-sql-millis: 1000
          #日志记录慢sql语句
          log-slow-sql: true
          #开启stat监视sql
          enabled: true
        wall:
          #开启防火墙
          enabled: true
          #config:
            #允许更新操作
            #update-allow: true
```

SpringBoot配置示例

​	https://github.com/alibaba/druid/tree/master/druid-spring-boot-starter

配置项列表

​	https://github.com/alibaba/druid/wiki/DruidDataSource%E9%85%8D%E7%BD%AE%E5%B1%9E%E6%80%A7%E5%88%97%E8%A1%A8

#### 整合MyBatis操作

https://github.com/mybatis

starter

SpringBoot官方的Starter：spring-boot-starter-*

第三方的： *-spring-boot-starter

​	**依赖**

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.4</version>
</dependency>
```

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1606704096118-53001250-a04a-4210-80ee-6de6a370be2e.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_20%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:67%;" /> 

##### 配置模式

- 全局配置文件
- SqlSessionFactory: 自动配置好了

- SqlSession：自动配置了 **SqlSessionTemplate 组合了SqlSession**
- @Import(**AutoConfiguredMapperScannerRegistrar**.**class**）；

- Mapper： 只要我们写的操作MyBatis的接口标准了 **@Mapper 就会被自动扫描进来**

  ```java
  @EnableConfigurationProperties(MybatisProperties.class) ： MyBatis配置项绑定类。
  @AutoConfigureAfter({ DataSourceAutoConfiguration.class, MybatisLanguageDriverAutoConfiguration.class })
  public class MybatisAutoConfiguration{}
  
  @ConfigurationProperties(prefix = "mybatis")
  public class MybatisProperties
  ```

可以修改配置文件中 mybatis 开始的所有

```xml
<!--
# 配置mybatis规则
mybatis:
  config-location: classpath:mybatis/mybatis-config.xml  #全局配置文件位置
  mapper-locations: classpath:mybatis/mapper/*.xml  #sql映射文件位置
Mapper接口---绑定Xml
-->
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.mcl.springbootwebexample.mapper.DutyMapper">
	<select id="getDuty" resultType="com.mcl.springbootwebexample.bean.Duty">
        select * from duty where id = #{id};
	</select>

</mapper>
```

配置 **private** Configuration **configuration**; mybatis.**configuration下面的所有，就是相当于改mybatis全局配置文件中的值**

```yml
# 配置mybatis规则
mybatis:
#  config-location: classpath:mybatis/mybatis-config.xml
  mapper-locations: classpath:mybatis/mapper/*.xml
  configuration:
    map-underscore-to-camel-case: true
 可以不写全局；配置文件，所有全局配置文件的配置都放在configuration配置项中即可
```

- 导入mybatis官方starter
- 编写mapper接口。标准@Mapper注解

- 编写sql映射文件并绑定mapper接口
- 在application.yaml中指定Mapper配置文件的位置，以及指定全局配置文件的信息 （建议；**配置在mybatis.configuration**）

**MyBatis与JDBC**

​	MyBatis是对JDBC的整合包装，让用户专注于处理业务，所以再用SpringBoot整合MyBatis时，若是已经配置了JDBC的一些信息（如：账号、密码等），在MyBatis中不需要额外配置，事实上Mybatis的整合包（"mybatis-spring-boot-starter"）也没有提供数据库连接的配置（这里的"配置"指的是yml中可以配置的信息），当然在Mybatis官方文档中的例子还是会加入类似property这种来设置用户名密码，这样的话就一定要记得在yml中将mybatis-config（Mybatis配置文件）导入进去，没有试过，应该也行。

​	如果用yml配置文件来配置mybatis的话，其实是可以不写mybatis-config文件的，只写mapper就行

​	MyBatis使用流程：

依赖 ---> 配置文件(可不写) --- > yml文件配置 ----> domain类  ---> mapper接口 ---->mapper映射xml文件 ---->Service实现接口方法----> Controller请求调用service处理

例子：

domain类

```java
@NoArgsConstructor
@AllArgsConstructor
@Data
public class Duty {
    private Integer id;
    private String name;
    private Integer attend_day;
    private Double attend_rate;
}
```

mapper接口

```java
@Mapper
public interface DutyMapper {
    public Duty getDuty(Integer id);
}
```

service类（实现mapper中的方法）

```java
@Service
public class DutyService {
    @Autowired
    DutyMapper dutyMapper;

    public Duty getDuty(Integer id){
        return dutyMapper.getDuty(id);
    }
}
```

mapper映射的xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.mcl.springbootwebexample.mapper.DutyMapper">
<select id="getDuty" resultType="com.mcl.springbootwebexample.bean.Duty">
        select * from duty where id = #{id};
</select>
</mapper>
```

mybatis的配置文件（可不写，看看就行）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
<!--
这是开启驼峰命名方式 即将java语言中的大写映射到数据库中时用 _+小写 完成 例：P - - > _p
mybatis:
  configuration:
    map-underscore-to-camel-case:
    也可以在yml文件中 用这种方式写
    但若是用这种方式写的话，就不能指定配置文件
<settings>
    <setting name="mapUnderscoreToCamelCase" value="true"/>
</settings>
-->
</configuration>
```

Controller处理请求

```java
@Controller
public class MybatisController {
    @Autowired
    DutyService dutyService;
    @ResponseBody
    @GetMapping("/batis")
    public Duty getDuty(@RequestParam("id")Integer id){
        return dutyService.getDuty(id);
    }
}
```

##### 注解模式

domain实体

```java
//SpringBoot有一个绑定机制，它可以自动寻找表名与类名相同的表并与之绑定，这样就可以实现对表直接操作而不需要指定表
//若是表名不同，也可以通过@TableName的注解设置
@TableName("City")//与表名一致
@Data
public class City {
    private Long id;
    private String name;
    private String state;
    private String country;
}
```

```java
@Mapper
public interface CityMapper {
    //这些select，Insert等注解可以代替mapper.xml文件，但是一些复杂的功能可能不能完全实现
    @Select("select * from city where id = #{id}")
    public City getById(Integer id);
    
    @Insert("insert into city(`name`,`state`,`country`) value (#{name},#{state},#{country})")
    @Options(useGeneratedKeys = true,keyProperty = "id") 
    public void insert(City city);
}
```

 ** 以上@Insert和@Options注解可以在xml中设置，具体方法看混合模式

```java
@ResponseBody
@PostMapping("/insert")
public City insert(City city){
    cityService.insert(city);
    return city;
}
```

```java
@ResponseBody
@GetMapping("/city")
public City getById(@RequestParam("id") Integer id){
    return cityService.getById(id);
}
```

##### 混合模式

mapper接口

```java
@Mapper
public interface CityMapper {
    @Select("select * from city where id = #{id}")
    public City getById(Integer id);

    
    public void insert(City city);
}
```

```xml
<mapper namespace="com.mcl.springbootwebexample.mapper.CityMapper">
<!--    userGenratedKeys=true开启自增，keyProperty=“id”自动返回名为id的属性-->
    <insert id="insert" useGeneratedKeys="true" keyProperty="id">
        insert into city(`name`,`state`,`country`) value (#{name},#{state},#{country})
    </insert>
<!--相当于在mapper接口上加上这两个注解
	@Insert("insert into city(`name`,`state`,`country`) value (#{name},#{state},#{country})")
    @Options(useGeneratedKeys = true,keyProperty = "id") -->
</mapper>
```

** 对于@Mapper注解，其实也可以在任意一个配置类上加上@MapperScan("mapper接口的路径")，这样就可以不在mapper映射接口上写@Mapper注释了

**最佳实战：**

- **引入mybatis-starter**
- **配置application.yaml中，指定mapper-location位置即可**

- **编写Mapper接口并标注@Mapper注解**
- **简单方法直接注解方式**

- **复杂方法编写mapper.xml进行绑定映射**
- ***@MapperScan("com.atguigu.admin.mapper") 简化，其他的接口就可以不用标注@Mapper注解***

#### 整合 MyBatis-Plus 完成CRUD

##### 什么是MyBatis-Plus

[MyBatis-Plus](https://github.com/baomidou/mybatis-plus)（简称 MP）是一个 [MyBatis](http://www.mybatis.org/mybatis-3/) 的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

[mybatis plus 官网](https://baomidou.com/)

建议安装 **MybatisX** 插件

##### 整合MyBatis-Plus

```xml
<!--    MyBatis-plus的依赖，自带JDBC和MyBatis依赖，可以不单独加入了-->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.3.4</version>
</dependency>
```

**自动配置**

- **`MybatisPlusAutoConfiguration 配置类，MybatisPlusProperties 配置项绑定。mybatis-plus：xxx 就是对mybatis-plus的定制`**
- **`SqlSessionFactory 自动配置好。底层是容器中默认的数据源`**

- **`mapperLocations 自动配置好的。有默认值。classpath*:/mapper/**/*.xml；任意包的类路径下的所有mapper文件夹下任意路径下的所有xml都是sql映射文件。  建议以后sql映射文件，放在 mapper下`**
- **`容器中也自动配置好了 SqlSessionTemplate`**

- **`@Mapper 标注的接口也会被自动扫描；建议直接 @MapperScan("springbootwebexample.mapper") 批量扫描就行`**

**Mapper接口继承BaseMapper<T>的好处**

- **`只需要我们的Mapper继承 BaseMapper 就可以拥有crud能力(封装了一些常用的数据库操作)`**

##### CRUD的功能

1. ###### 对service层的优化

   上面说了让mapper接口继承BaseMapper并指定泛型为指定对象，就可以省去写简单的CURD语句

   在这基础上，在建service接口时，可以让接口继承IService，然后让他的实现类继承ServiceImpl<指定对应mapper，指定返回类型>并实现对应的接口，就可以有很多完善的方法可以直接调用（主要是page方法）

   ```java
   public interface UserMapper extends BaseMapper<User> {}
   ```

   ```java
   public interface UserService extends IService<User> {}
   ```

   ```java
   @Service
   public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService {}
   ```

2. ###### 对CRUD的优化（分页）

   按照以前的操作，可以用优化后的service的list方法直接过去到数据库表中的所有数据，但这样的话，前端对导航栏，分页表的操作不是很好进行，所以可以用page方法来操作

   ```java
       @GetMapping("/dynamic_table")
       public String dtable(@RequestParam(value = "pn",defaultValue = "1") Integer pn, Model model){
           //分页查询的对象结果，里面包含了User的list数据，以及显示的页码pn，以及每页显示的数据数2
           Page<User> userPage = new Page<>(pn, 2);
           //将分页查询的对象放入这个page对象，查询条件设置为null
           Page<User> page = userService.page(userPage,null);
   //        page.getRecords()这个方法就是相当于查询到的users的list数据
           model.addAttribute("page",page);
           return "table/dynamic_table";
       }
   ```

   这个例子的page对象中保存的是对应pn页中的2条记录，pn可由前端传输。接下来具体说一下各个参数的作用

   - pn：获取前端传过来的页码，defaultValue设置pn的默认值
   - Page对象保存的是设置的页码中的n条数据对象
   - page有几个方法，是mybatis-plus提供的
     - current：当前页码
     - pages：总页数
     - total：表中数据总数
     - records：表数据对象，类似service方法提供的list方法返回的数据集

3. ###### 前端的优化

   解释一下下面Themleaf的用法

   - 循环数据：**th:each="user,stats:${page.records}"** ：相当于增强for循环，这个stats是可以给当前表的数据加上编号
     用法就是**${stats.count}**
   - 获取**当前页码--->[[\${page.current}]]  总页数--->[[\${page.pages }]]   数据总数--->[[\${page.total}]]**
     前提是之前将page对象放入了Attribute中了
   - 高亮判断：**th:class="${num == page.current ? 'active':''}"**  由于这个例子的js是将active的class标记为高亮，其实就是当前页码高亮，如果不是当前页面不需要高亮，所以需要做一个判断
   - 页码动态循环：**th:each="num:\${\#numbers.sequence(1,page.pages)}**，numbers是Themleaf的内置对象，这句话的意思就是获取1到page.pages的循环创建子标签，并将这个数字放入num中
   - 动态传输页码：**th:href="@{/dynamic_table(pn=\${num})}"** ,动态获取页码，与上面的num是同一个值，点击按钮就会传输当前按钮的num值

   ```html
   <table class="display table table-bordered table-striped" id="dynamic-table">
       <thead>
       <tr>
           <th>#</th>
           <th>id</th>
           <th>name</th>
           <th class="hidden-phone">age</th>
           <th class="hidden-phone">email</th>
       </tr>
       </thead>
       <tbody>
       <tr class="gradeX" th:each="user,stats:${page.records}">
           <td th:text="${stats.count}"></td>
           <td th:text="${user.id}"></td>
           <td th:text="${user.getName()}">Win 95+</td>
           <td class="center hidden-phone" th:text="${user.getAge()}">4</td>
           <td class="center hidden-phone" th:text="${user.getEmail()}">X</td>
       </tr>
       </tbody>
       <tfoot>
       <tr>
           <th>Rendering engine</th>
           <th>Browser</th>
           <th>Platform(s)</th>
           <th class="hidden-phone">Engine version</th>
           <th class="hidden-phone">CSS grade</th>
       </tr>
       </tfoot>
   
   </table>
   <div class="row-fluid">
       <div class="span6">
           <div class="dataTables_info" id="dynamic-table_info">
               Showing [[${page.current}]] to [[${page.pages}]] of [[${page.total}]]
               entries
           </div>
       </div>
       <div class="span6">
           <div class="dataTables_paginate paging_bootstrap pagination">
               <ul>
                   <li class="prev disabled"><a href="#">← Previous</a></li>
                   <li th:class="${num == page.current ? 'active':''}" th:each="num:${#numbers.sequence(1,page.pages)}">
                       <a th:href="@{/dynamic_table(pn=${num})}" >[[${num}]]</a></li>
                   <li class="next"><a href="#">Next → </a></li>
               </ul>
           </div>
       </div>
   </div>
   ```

   效果

   <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211027230756249.png" alt="image-20211027230756249" style="zoom:67%;" /> 

4. ###### 以删除按钮为例的优化

   这个删除按钮应该可以满足在点击删除按钮之后，数据库删除这个数据并且前端页面返回到删除之前的页面，但数据已经刷新了

   新增的按钮：

   ```html
   <td >
   <a class="btn btn-danger btn-sm" th:href="@{/user/delete/{id}(id = ${user.id},pn=${page.current})}" type="button">删除</a>
   </td>
   ```

   **@{/user/delete/{id}(id = \${user.id},pn=\${page.current})}**就会有如下url
   	localhost:8080/user/delete/2?pn=1，这样在发出删除请求的同时还会附带页码，之后直接返回表单处理映射即可

   ```java
   @GetMapping("/user/delete/{id}")
   public String deleteUser(@PathVariable("id") Long id, //获取路径中删除的id编号
                            @RequestParam(value = "pn",defaultValue = "1") Integer pn, //获取当前页码，默认值1
                            RedirectAttributes ra){ //重定向的携带参数，将pn携带过去
       userService.removeById(id);
       ra.addAttribute("pn",pn);
       return "redirect:/dynamic_table";
   }
   ```

### NoSQL

​	即非关系型数据库，例如：Redis

​	Redis 是一个开源（BSD许可）的，**内存中的数据结构存储系统**，它可以用作数据库、缓存和消息中间件。 它支持多种类型的数据结构，如 [字符串（strings）](http://www.redis.cn/topics/data-types-intro.html#strings)， [散列（hashes）](http://www.redis.cn/topics/data-types-intro.html#hashes)， [列表（lists）](http://www.redis.cn/topics/data-types-intro.html#lists)， [集合（sets）](http://www.redis.cn/topics/data-types-intro.html#sets)， [有序集合（sorted sets）](http://www.redis.cn/topics/data-types-intro.html#sorted-sets) 与范围查询， [bitmaps](http://www.redis.cn/topics/data-types-intro.html#bitmaps)， [hyperloglogs](http://www.redis.cn/topics/data-types-intro.html#hyperloglogs) 和 [地理空间（geospatial）](http://www.redis.cn/commands/geoadd.html) 索引半径查询。 Redis 内置了 [复制（replication）](http://www.redis.cn/topics/replication.html)，[LUA脚本（Lua scripting）](http://www.redis.cn/commands/eval.html)， [LRU驱动事件（LRU eviction）](http://www.redis.cn/topics/lru-cache.html)，[事务（transactions）](http://www.redis.cn/topics/transactions.html) 和不同级别的 [磁盘持久化（persistence）](http://www.redis.cn/topics/persistence.html)， 并通过 [Redis哨兵（Sentinel）](http://www.redis.cn/topics/sentinel.html)和自动 [分区（Cluster）](http://www.redis.cn/topics/cluster-tutorial.html)提供高可用性（high availability）。

#### Redis自动配置

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1606745732785-17d1227a-75b9-4f00-a3f1-7fc4137b5113.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_17%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:67%;" /> 

**自动配置：**

- RedisAutoConfiguration 自动配置类。RedisProperties 属性类 --> **spring.redis.xxx**是对redis的配置
- 连接工厂是准备好的。**Lettuce**ConnectionConfiguration、**Jedis**ConnectionConfiguration

- 自动注入了**RedisTemplate<Object, Object>** ： xxxTemplate；
- 自动注入了**StringRedisTemplate**；k：v都是String

- **key：value**
- **底层只要我们使用** **StringRedisTemplate、RedisTemplate就可以操作redis**



**redis环境搭建**

**1、阿里云按量付费redis。经典网络**

**2、申请redis的公网连接地址**

**3、修改白名单  允许0.0.0.0/0 访问**

#### RedisTemplate与Lettuce

redis默认使用Lettuce来处理redis数据

这是redis测试

```java
    @Autowired
    StringRedisTemplate redisTemplate;
    @Test
    void redisTest() {
        ValueOperations<String, String> operations = redisTemplate.opsForValue();
        operations.set("mcl", "yyds");
        String mcl = operations.get("mcl");
        System.out.println(mcl);
    }
```

#### 切换至jedis

这没试过，以后再说

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>

<!--        导入jedis-->
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
</dependency>
```

```yml
spring:
  redis:
      host: r-bp1nc7reqesxisgxpipd.redis.rds.aliyuncs.com
      port: 6379
      password: lfy:Lfy123456
      client-type: jedis
      jedis:
        pool:
          max-active: 10
```

#### redis操作实例

需求：redis中保存网页访问次数，然后在页面中显示网页访问次数

​	redis依赖

```xml
<!--redis-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

​	yml配置

```yml
srping:
    redis:
      host: r-uf6qxrz5qegi5wm6bwpd.redis.rds.aliyuncs.com
      password: mcl:Mcl255013!
      port: 6379	
```

​	过滤器，用于记录网页访问次数

```java
@Component
public class RedisCountIntercepter implements HandlerInterceptor {
    @Autowired
    StringRedisTemplate redisTemplate;
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        String uri = request.getRequestURI();

        //自动设置访问计数+1
        redisTemplate.opsForValue().increment(uri);
        return true;
    }
}
```

​	将过滤器放入配置器中

```java
//只截取了部分
@Configuration
public class WebConfig implements WebMvcConfigurer {
    //这个过滤器不像之前的过滤器那样直接new进来，因为过滤器中要用到redis实例，若是直接重新创建，自动注入redis就失效了，所以治理直接自动注入过滤器再放入就好了
    @Autowired
    RedisCountIntercepter redisCountIntercepter;
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(redisCountIntercepter)
                .addPathPatterns("/**")
                .excludePathPatterns("/","/login","/css/**","/fonts/**","/images/**","/js/**","/aa/**");
    }
}
```

​	在Controller中添加redis信息

```java
 	@Autowired
    StringRedisTemplate stringRedisTemplate;
    @GetMapping("/index.html")
    public String mainPage(HttpSession session,Model model) {
        
        //从redis中通过key直接获取到值value
        String s = stringRedisTemplate.opsForValue().get("/index.html");
        String s1 = stringRedisTemplate.opsForValue().get("/dynamic_table");
        model.addAttribute("indexCount",s);
        model.addAttribute("dynamicCount",s1);
        return "/index";
    }
```

​	最后在页面中直接获取

```xml
<div class="state-value">
    <div class="value" th:text="${indexCount}">230</div>
    <div class="title">/index_html</div>
</div>
		......
<div class="state-value">
    <div class="value" th:text="${dynamicCount}">3490</div>
    <div class="title">/dynamic.html</div>
</div>
```

​	这样在网页中就可以实时实现网页访问计数

最后记得释放阿里云的redis，不然扣费

## 单元测试

**Spring Boot 2.2.0 版本开始引入 JUnit 5 作为单元测试默认库**

### JUnit5 的变化

作为最新版本的JUnit框架，JUnit5与之前版本的Junit框架有很大的不同。由三个不同子项目的几个不同模块组成。

> **JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage**

**JUnit Platform**: Junit Platform是在JVM上启动测试框架的基础，不仅支持Junit自制的测试引擎，其他测试引擎也都可以接入。

**JUnit Jupiter**: JUnit Jupiter提供了JUnit5的新的编程模型，是JUnit5新特性的核心。内部 包含了一个**测试引擎**，用于在Junit Platform上运行。

**JUnit Vintage**: 由于JUint已经发展多年，为了照顾老的项目，JUnit Vintage提供了兼容JUnit4.x,Junit3.x的测试引擎。

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1606796395719-eb57ab48-ae44-45e5-8d2e-c4d507aff49a.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_19%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10%2Fresize%2Cw_683%2Climit_0" alt="img" style="zoom:80%;" /> 

**注意：**

​	SpringBoot 2.4 以上版本移除了默认对 Vintage 的依赖。如果需要兼容junit4需要自行引入（不能使用junit4的功能 @Test）

JUnit 5’s Vintage Engine Removed from spring-boot-starter-test,如果需要继续兼容junit4需要自行引入vintage

```xml
<dependency>
    <groupId>org.junit.vintage</groupId>
    <artifactId>junit-vintage-engine</artifactId>
    <scope>test</scope>
    <exclusions>
        <exclusion>
            <groupId>org.hamcrest</groupId>
            <artifactId>hamcrest-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1606797616337-e73010e9-9cac-496d-a177-64b677af5a3d.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_18%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" /> 

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-test</artifactId>
  <scope>test</scope>
</dependency>
```



### JUnit5常用注解

JUnit5的注解与JUnit4的注解有所变化

https://junit.org/junit5/docs/current/user-guide/#writing-tests-annotations

- **@Test :**表示方法是测试方法。但是与JUnit4的@Test不同，他的职责非常单一不能声明任何属性，拓展的测试将会由Jupiter提供额外测试
- **@ParameterizedTest :**表示方法是参数化测试，下方会有详细介绍

- **@RepeatedTest :**表示方法可重复执行，下方会有详细介绍
- **@DisplayName :**为测试类或者测试方法设置展示名称

- **@BeforeEach :**表示在每个单元测试之前执行
- **@AfterEach :**表示在每个单元测试之后执行

- **@BeforeAll :**表示在所有单元测试之前执行
- **@AfterAll :**表示在所有单元测试之后执行

- **@Tag :**表示单元测试类别，类似于JUnit4中的@Categories，其实就是个标记
- **@Disabled :**表示测试类或测试方法不执行，类似于JUnit4中的@Ignore

- **@Timeout :**表示测试方法运行如果超过了指定时间将会返回错误
- **@ExtendWith :**为测试类或测试方法提供扩展类引用

**例子**

```java
@SpringBootTest	
//这个注解主要使用来调用springboot整合的，它会将springboot的整体功能以及容器中的组件都放进去，ExtendWith注解也是这个作用
@DisplayName("JUnit5Test..")
public class JUnit5Test {
    @Test
    @DisplayName("DisableName Test ....")	//设置测试别名
    void testDiableName(){
        System.out.println(11);
    }
    @Disabled	//使用全员测试时，不运行该测试
    @Test
    void test2(){
        System.out.println(22);
    }
    @BeforeEach		//每次单元测试之前都运行这个方法
    void testBeforeEach(){
        System.out.println("Test beginning......");
    }
    @AfterEach		//每次单元测试之后都运行这个方法
    void testAfterEach(){
        System.out.println("Test Over......");
    }
    @BeforeAll		//整合测试之前运行此方法，由于只运行一次，所以是静态方法
    static void testBeforeAll(){
        System.out.println("ALL TESTS ARE BEGINNING!");
    }

    @AfterAll		//整合测试之后运行此方法，由于只运行一次，所以是静态方法
    static void testAfterAll(){
        System.out.println("ALL TESTS ARE OVER!");
    }
    @Test
    @Timeout(value = 300,unit = TimeUnit.MILLISECONDS)		//设置超时时间，方法要是运行时间超过规定时间，报异常
    void test03() throws InterruptedException {
        Thread.sleep(400);
    }

    @RepeatedTest(5)		//重复测试（每次都会执行该方法时BeforeEach和AfterEach也都会执行）
    void test04(){
        System.out.println("重复测试！");
    }
}
```

### 断言（assertions）

断言（assertions）是测试方法中的核心部分，用来对测试需要满足的条件进行验证。这些断言方法都是 **org.junit.jupiter.api.Assertions** 的静态方法。JUnit 5 内置的断言可以分成如下几个类别：

- **检查业务逻辑返回的数据是否合理。**
- **所有的测试运行结束以后，会有一个详细的测试报告；**

​     **若是一个测试方法中有多个断言，其中一个断言失败就会导致后面的断言失效。断言一般用于在某一模块都完成之后进行测试，可以快速找出所有测试不通过的测试方法，并对症下药**

#### 	简单断言

用来对单个值进行简单的验证。如：

| 方法            | 说明                                 |
| --------------- | ------------------------------------ |
| assertEquals    | 判断两个对象或两个原始类型是否相等   |
| assertNotEquals | 判断两个对象或两个原始类型是否不相等 |
| assertSame      | 判断两个对象引用是否指向同一个对象   |
| assertNotSame   | 判断两个对象引用是否指向不同的对象   |
| assertTrue      | 判断给定的布尔值是否为 true          |
| assertFalse     | 判断给定的布尔值是否为 false         |
| assertNull      | 判断给定的对象引用是否为 null        |
| assertNotNull   | 判断给定的对象引用是否不为 null      |

以下均为成功实现

```java
    @Test
    @DisplayName("简单断言--->")
    void simpleAssert(){
        //判断两个对象或两个原始类型是否相等
        assertEquals(1,1,"预期与实际不相符");
        
        //判断两个对象或两个原始类型是否不相等
        assertNotEquals(1,2,"预期与实际居然相等？");
        
        //判断两个对象引用是否指向同一个对象
        Object obj1 = new Object();
        Object obj2 = new Object();
        assertSame(obj1,obj1,"两个对象地址不相同");
        
        //判断两个对象引用是否指向不同的对象
        assertNotSame(obj1,obj2,"两个对象地址居然相同？");
        
        //判断给定的布尔值是否为 true
        assertTrue(true,"不是true啊");
        
        //判断给定的布尔值是否为 false
        assertFalse(false,"不是false啊");
        
        //判断给定的对象引用是否为 null
        assertNull(null,"怎么是null");
        
        //判断给定的对象引用是否不为 null
        assertNotNull(1,"怎么不是null？");
        
        //判断两个对象或原始类型的数组是否相等
        assertArrayEquals(new int[]{1,2},new int[]{1,2},"两个数组不一样");
    }
```

#### 	数组断言

通过 assertArrayEquals 方法来判断两个对象或原始类型的数组是否相等

- assertArrayEquals：判断两个数据中的数据是否相同

```java
@Test
void arrayAssert(){
    //判断两个对象或原始类型的数组是否相等
    assertArrayEquals(new int[]{1,2},new int[]{1,2},"两个数组不一样");
}
```

#### 	组合断言

```java
@Test
void multiplyAssert(){
    //组合断言
    assertAll("Math",
              () -> assertEquals(2, 1 + 1),
              () -> assertTrue(1 > 0)
             );
}
```

#### 	异常断言

```java
@Test
void exceptionAssert(){
    //异常断言,判断是否扔出异常
    Assertions.assertThrows(ArithmeticException.class, () -> System.out.println(1 % 0));
}
```

#### 	超时断言

```java
 @Test
// 超时断言
void timeoutAssert(){
    //如果测试方法时间超过1s将会异常
    Assertions.assertTimeout(Duration.ofMillis(1000), () -> Thread.sleep(500));
}
```

#### 	快速失败

```java
@Test
//快速失败
void quickfailAssert(){
    //预期应该是失败，若是没有失败，就抛异常
    if(1==0){
        fail("这就该失败");
    }
}
```

### 前置条件（assumptions）

JUnit 5 中的前置条件（**assumptions【假设】**）类似于断言，不同之处在于**不满足的断言会使得测试方法失败**，而不满足的**前置条件只会使得测试方法的执行终止**。前置条件可以看成是测试方法执行的前提，当该前提不满足时，就没有继续执行的必要。

```java
//直观判断就是，若是出现不满足前置条件的情况会直接跳过这个测试方法。类似于@Disabled,不过前置条件是被动跳过的
@Test
void testAssumption(){
    Assumptions.assumeTrue(false,"怎么不是true");
    System.out.println(1);
}
```

- ​	assumeTrue 和 assumFalse 确保给定的条件为 true 或 false，不满足条件会使得测试执行终止。
- ​	assumingThat 的参数是表示条件的布尔值和对应的 Executable 接口的实现对象。
- ​	只有条件满足时，Executable 对象才会被执行；当条件不满足时，测试执行并不会终止。

### 嵌套测试

​	JUnit 5 可以通过 Java 中的内部类和@Nested 注解实现嵌套测试，从而可以更好的把相关的测试方法组织在一起。在内部类中可以使用@BeforeEach 和@AfterEach 注解，而且嵌套的层次没有限制。

```java
@DisplayName("嵌套测试,本次测试为标准正确测试")
class NestTest {
    /**
     * 嵌套测试内层可以驱动外层的before/afterEach/All之类的，但外层不能调用内层的before/afterEach/All
     */
    @Nested
    @DisplayName("A stack")
    class TestingAStackDemo {

        Stack<Object> stack;


        //stack是否为null  ---> yes
        @Test
        @DisplayName("is instantiated with new Stack()")
        void isInstantiatedWithNew() {
            new Stack<>();
            assertNull(stack);
        }
        //嵌套类测试
        @Nested
        @DisplayName("when new")
        class WhenNew {

            @BeforeEach
            void createNewStack() {
                stack = new Stack<>();
            }
            //外层stack没有放元素，所以stack当然是空的 ---> pass
            @Test
            @DisplayName("is empty")
            void isEmpty() {
                assertTrue(stack.isEmpty());
            }
            //stack中没有放数据，所以在弹栈时会报空栈异常 ---> pass
            @Test
            @DisplayName("throws EmptyStackException when popped")
            void throwsExceptionWhenPopped() {
                assertThrows(EmptyStackException.class, stack::pop);
            }
            //stack中仍然没有数据，所以在查看第一个栈元素是也会出空栈异常 ---> pass
            @Test
            @DisplayName("throws EmptyStackException when peeked")
            void throwsExceptionWhenPeeked() {
                assertThrows(EmptyStackException.class, stack::peek);
            }
            //再次嵌套类测试
            @Nested
            @DisplayName("after pushing an element")
            class AfterPushing {

                String anElement = "an element";
                //每次运行测试方法在栈中放入一个元素
                @BeforeEach
                void pushAnElement() {
                    stack.push(anElement);
                }
                //栈中有一个元素，所以栈空判断为false ---> pass
                @Test
                @DisplayName("it is no longer empty")
                void isNotEmpty() {
                    assertFalse(stack.isEmpty());
                }
                //栈中与一个元素，在弹出这个元素后，栈判空为true ---> pass
                @Test
                @DisplayName("returns the element when popped and is empty")
                void returnElementWhenPopped() {
                    assertEquals(anElement, stack.pop());
                    assertTrue(stack.isEmpty());
                }
                //栈中的元素为emelent，与emelent相同，简单断言为true ---> pass
                //栈中有元素，所以简单断言栈空为false ---> pass
                @Test
                @DisplayName("returns the element when peeked but remains not empty")
                void returnElementWhenPeeked() {
                    assertEquals(anElement, stack.peek());
                    assertFalse(stack.isEmpty());
                }
            }
        }
    }
}
```

### 参数化测试

​	参数化测试是JUnit5很重要的一个新特性，它使得用不同的参数多次运行测试成为了可能，也为我们的单元测试带来许多便利。

​	利用**@ValueSource**等注解，指定入参，我们将可以使用不同的参数进行多次单元测试，而不需要每新增一个参数就新增一个单元测试，省去了很多冗余代码。

- **@ValueSource**: 为参数化测试指定入参来源，支持八大基础类以及String类型,Class类型
- **@NullSource**: 表示为参数化测试提供一个null的入参
- **@EnumSource**: 表示为参数化测试提供一个枚举入参
- **@CsvFileSource**：表示读取指定CSV文件内容作为参数化测试入参
- **@MethodSource**：表示读取指定方法的返回值作为参数化测试入参(注意方法返回需要是一个流)

​     当然如果参数化测试仅仅只能做到指定普通的入参还达不到让我觉得惊艳的地步。让我真正感到他的强大之处的地方在于他可以支持外部的各类入参。如:CSV,YML,JSON 文件甚至方法的返回值也可以作为入参。只需要去实现**ArgumentsProvider**接口，任何外部文件都可以作为它的入参。

```java
@ParameterizedTest
@DisplayName("参数化测试{数组}")
@ValueSource(ints = {2, 3, 1, 5, 23, 12})
void testParameterized(int i){
    System.out.println(i);
}

@ParameterizedTest
@DisplayName("参数化测试{方法}")
@MethodSource("method1")
void testParameterized(String str){
    //参数化测试方法必须为 “静态流” 类型
    System.out.println(str);
}
static Stream<String> method1(){
    return Stream.of("mcl","yyds","lol");
}
```

### 迁移指南

JUnit4 ---> JUnit5

在进行迁移的时候需要注意如下的变化：

- 注解在 org.junit.jupiter.api 包中，断言在 org.junit.jupiter.api.Assertions 类中，前置条件在 org.junit.jupiter.api.Assumptions 类中。
- 把@Before 和@After 替换成@BeforeEach 和@AfterEach。

- 把@BeforeClass 和@AfterClass 替换成@BeforeAll 和@AfterAll。
- 把@Ignore 替换成@Disabled。

- 把@Category 替换成@Tag。
- 把@RunWith、@Rule 和@ClassRule 替换成@ExtendWith。

## 指标监控

### SpringBoot Actuator

#### 	简介

​	未来每一个微服务在云上部署以后，我们都需要对其进行监控、追踪、审计、控制等。SpringBoot就抽取了Actuator场景，使得我们每个微服务快速引用即可获得生产级别的应用监控、审计等功能。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1606886483335-697ee1c1-2f69-43ab-bddc-3a038382319c.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_19%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" /> 

#### 	1.x与2.x的不同

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1606884394162-ac7f2d8e-7abb-44df-9998-fb0f2705f238.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_30%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" /> 

#### 	如何使用

- 引入场景
- 访问 http://localhost:8080/actuator/**

- 暴露所有监控信息为HTTP

  ```yml
  management:
    endpoints:
      enabled-by-default: true #默认暴露所有端点信息
      web:
        exposure:
          include: '*'  #以web方式暴露(暴露所有接口)
  ```

- 测试

http://localhost:8080/actuator/beans

http://localhost:8080/actuator/configprops

http://localhost:8080/actuator/metrics

http://localhost:8080/actuator/metrics/jvm.gc.pause

[http://localhost:8080/actuator/](http://localhost:8080/actuator/metrics)具体的json属性/在具体的细节

#### 	可视化

https://github.com/codecentric/spring-boot-admin

分为两部分：**监视服务器**和**被监视服务器**

##### 	监视服务器

​	监视服务器就是另一个springboot对象，但是要引入web场景，因为他是web端监视

​	步骤：

- 新建一个springboot的web应用，记得勾选web场景

- 引入依赖

  ```java
  <!--        数据可视化-->
  <dependency>
      <groupId>de.codecentric</groupId>
      <artifactId>spring-boot-admin-starter-server</artifactId>
      <version>2.3.1</version>
  </dependency>
  ```

- 添加注释

  ```java
  @EnableAdminServer	//启动监听服务
  @SpringBootApplication
  public class SbVisibleTestApplication {
      public static void main(String[] args) {
          SpringApplication.run(SbVisibleTestApplication.class, args);
      }
  
  }
  ```

- 修改端口（不要与被监听服务器冲突）

  ```properties
  server.port=8888
  ```

##### 被监听服务器

- 引入依赖

  ```xml
  <dependency>
      <groupId>de.codecentric</groupId>
      <artifactId>spring-boot-admin-starter-client</artifactId>
      <version>2.3.1</version>
  </dependency>
  ```

- yml设置传入数据的服务器地址

  ```properties
  spring.boot.admin.client.url=http://localhost:8888  #数据传入的地址
  management.endpoints.web.exposure.include=*  #开启所有的监听信息
  ```

之后开启两个服务器，就可以在**监听服务器地址**上看到**被监听服务器**的所有信息

​	http://localhost:8888/applications

### Actuator Endpoint

#### 最常使用的端点

| ID                  | 描述                                                         |
| ------------------- | :----------------------------------------------------------- |
| `auditevents`       | 暴露当前应用程序的审核事件信息。需要一个`AuditEventRepository组件`。 |
| `beans`             | 显示应用程序中所有Spring Bean的完整列表。                    |
| `caches`            | 暴露可用的缓存。                                             |
| `conditions`        | 显示自动配置的所有条件信息，包括匹配或不匹配的原因。         |
| `configprops`       | 显示所有`@ConfigurationProperties`。                         |
| `env`               | 暴露Spring的属性`ConfigurableEnvironment`                    |
| `flyway`            | 显示已应用的所有Flyway数据库迁移。 需要一个或多个`Flyway`组件。 |
| `health`            | 显示应用程序运行状况信息。                                   |
| `httptrace`         | 显示HTTP跟踪信息（默认情况下，最近100个HTTP请求-响应）。需要一个`HttpTraceRepository`组件。 |
| `info`              | 显示应用程序信息。                                           |
| `integrationgraph`  | 显示Spring `integrationgraph` 。需要依赖`spring-integration-core`。 |
| `loggers`           | 显示和修改应用程序中日志的配置。                             |
| `liquibase`         | 显示已应用的所有Liquibase数据库迁移。需要一个或多个`Liquibase`组件。 |
| **`metrics`(常用)** | 显示当前应用程序的“指标”信息。                               |
| `mappings`          | 显示所有`@RequestMapping`路径列表。                          |
| `scheduledtasks`    | 显示应用程序中的计划任务。                                   |
| `sessions`          | 允许从Spring Session支持的会话存储中检索和删除用户会话。需要使用Spring Session的基于Servlet的Web应用程序。 |
| `shutdown`          | 使应用程序正常关闭。默认禁用。                               |
| `startup`           | 显示由`ApplicationStartup`收集的启动步骤数据。需要使用`SpringApplication`进行配置`BufferingApplicationStartup`。 |
| `threaddump`        | 执行线程转储。                                               |

如果您的应用程序是Web应用程序（Spring MVC，Spring WebFlux或Jersey），则可以使用以下附加端点

| ID           | 描述                                                         |
| ------------ | ------------------------------------------------------------ |
| `heapdump`   | 返回`hprof`堆转储文件。                                      |
| `jolokia`    | 通过HTTP暴露JMX bean（需要引入Jolokia，不适用于WebFlux）。需要引入依赖`jolokia-core`。 |
| `logfile`    | 返回日志文件的内容（如果已设置`logging.file.name`或`logging.file.path`属性）。支持使用HTTP`Range`标头来检索部分日志文件的内容。 |
| `prometheus` | 以Prometheus服务器可以抓取的格式公开指标。需要依赖`micrometer-registry-prometheus`。 |

最常用的Endpoint

- **Health：监控健康状况**
- **Metrics：运行时指标**

- **Loggers：日志记录**

#### Health Endpoint

​	健康检查端点，我们一般用于在云平台，平台会定时的检查应用的健康状况，我们就需要Health Endpoint可以为平台返回当前应用的一系列组件健康状况的集合。

重要的几点：

- health endpoint返回的结果，应该是一系列健康检查后的一个汇总报告
- 很多的健康检查默认已经自动配置好了，比如：数据库、redis等

- 可以很容易的添加自定义的健康检查机制

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1606908975702-4f9a3208-15ca-4a78-9f76-939ef986db7e.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_12%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10) 

```yml
management:
  endpoints:
    enabled-by-default: true #暴露所有端点信息
    web:
      exposure:
        include: '*'  #以web方式暴露(暴露所有接口)
  endpoint:
    health:
      show-details: always  #显示health接口下的所有详细信息
```

#### Metrics Endpoint

提供详细的、层级的、空间指标信息，这些信息可以被pull（主动推送）或者push（被动获取）方式得到；

- 通过Metrics对接多种监控系统
- 简化核心Metrics开发

- 添加自定义Metrics或者扩展已有Metrics

  <img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1606909073222-c6e77ca3-4b1c-4f38-a1c6-8614dec4f7bc.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_16%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:67%;" /> 

#### 管理Endpoints

##### 开启与禁用Endpoints

默认所有的Endpoint除过shutdown都是开启的。

- 需要开启或者禁用某个Endpoint。配置模式为  management.endpoint.<endpointName>.enabled = true

```yml
management:
  endpoint:
    beans:
      enabled: true
```

- 或者禁用所有的Endpoint然后手动开启指定的Endpoint

```yml
management:
  endpoints:
    enabled-by-default: false
  endpoint:
    beans:
      enabled: true
    health:
      enabled: true
```

##### 暴露Endpoints

支持的暴露方式

- HTTP：默认只暴露**health**和**info** Endpoint
- **JMX**：默认暴露所有Endpoint（Java应用管理软件：Jconsole）

- 除过health和info，剩下的Endpoint都应该进行保护访问。如果引入SpringSecurity，则会默认配置安全访问规则

| ID                 | JMX  | Web  |
| ------------------ | ---- | ---- |
| `auditevents`      | Yes  | No   |
| `beans`            | Yes  | No   |
| `caches`           | Yes  | No   |
| `conditions`       | Yes  | No   |
| `configprops`      | Yes  | No   |
| `env`              | Yes  | No   |
| `flyway`           | Yes  | No   |
| `health`           | Yes  | Yes  |
| `heapdump`         | N/A  | No   |
| `httptrace`        | Yes  | No   |
| `info`             | Yes  | Yes  |
| `integrationgraph` | Yes  | No   |
| `jolokia`          | N/A  | No   |
| `logfile`          | N/A  | No   |
| `loggers`          | Yes  | No   |
| `liquibase`        | Yes  | No   |
| `metrics`          | Yes  | No   |
| `mappings`         | Yes  | No   |
| `prometheus`       | N/A  | No   |
| `scheduledtasks`   | Yes  | No   |
| `sessions`         | Yes  | No   |
| `shutdown`         | Yes  | No   |
| `startup`          | Yes  | No   |
| `threaddump`       | Yes  | No   |

#### 定制 Endpoint

##### 定制 Health 信息

```java
import org.springframework.boot.actuate.health.Health;
import org.springframework.boot.actuate.health.HealthIndicator;
import org.springframework.stereotype.Component;
@Component
public class MyHealthIndicator implements HealthIndicator {

    @Override
    public Health health() {
        int errorCode = check(); // perform some specific health check
        if (errorCode != 0) {
            return Health.down().withDetail("Error Code", errorCode).build();
        }
        return Health.up().build();
    }

}
//构建Health
Health build = Health.down()
                .withDetail("msg", "error service")
                .withDetail("code", "500")
                .withException(new RuntimeException())
                .build();
```

```yml
management:
    health:
      enabled: true
      show-details: always #总是显示详细信息。可显示每个模块的状态信息
```

```java
@Component
public class MyComHealthIndicator extends AbstractHealthIndicator {

    /**
     * 真实的检查方法
     * @param builder
     * @throws Exception
     */
    @Override
    protected void doHealthCheck(Health.Builder builder) throws Exception {
        //mongodb。  获取连接进行测试
        Map<String,Object> map = new HashMap<>();
        // 检查完成
        if(1 == 2){
//            builder.up(); //健康
            builder.status(Status.UP);
            map.put("count",1);
            map.put("ms",100);
        }else {
//            builder.down();
            builder.status(Status.OUT_OF_SERVICE);
            map.put("err","连接超时");
            map.put("ms",3000);
        }

        builder.withDetail("code",100)
                .withDetails(map);
    }
}
```

##### 定制info信息

常用两种方式

###### 编写配置文件

```yml
info:
  appName: boot-admin	#这个appName和Version都是可以随便写的
  version: 2.0.1
  mavenProjectName: @project.artifactId@  #使用@@可以获取maven的pom文件值
  mavenProjectVersion: @project.version@
```

###### 编写InfoContributor

```java
import java.util.Collections;
import org.springframework.boot.actuate.info.Info;
import org.springframework.boot.actuate.info.InfoContributor;
import org.springframework.stereotype.Component;

@Component
public class ExampleInfoContributor implements InfoContributor {

    @Override
    public void contribute(Info.Builder builder) {
        builder.withDetail("example",
                Collections.singletonMap("key", "value"));
    }
}
```

http://localhost:8080/actuator/info 会输出以上方式返回的所有info信息

#### 定制Metrics信息

##### SpringBoot支持自动适配的Metrics

- JVM metrics, report utilization of:

- - Various memory and buffer pools
  - Statistics related to garbage collection

- - Threads utilization
  - Number of classes loaded/unloaded

- CPU metrics
- File descriptor metrics

- Kafka consumer and producer metrics
- Log4j2 metrics: record the number of events logged to Log4j2 at each level

- Logback metrics: record the number of events logged to Logback at each level
- Uptime metrics: report a gauge for uptime and a fixed gauge representing the application’s absolute start time

- Tomcat metrics (`server.tomcat.mbeanregistry.enabled` must be set to `true` for all Tomcat metrics to be registered)
- [Spring Integration](https://docs.spring.io/spring-integration/docs/5.4.1/reference/html/system-management.html#micrometer-integration) metrics

##### 增加定制Metrics

```java
class MyService{
    Counter counter;
    public MyService(MeterRegistry meterRegistry){
         counter = meterRegistry.counter("myservice.method.running.counter");
    }

    public void hello() {
        counter.increment();
    }
}
//也可以使用下面的方式
@Bean
MeterBinder queueSize(Queue queue) {
    return (registry) -> Gauge.builder("queueSize", queue::size).register(registry);
}
```

#### 定制Endpoint

```java
@Component
@Endpoint(id = "container")
public class DockerEndpoint {


    @ReadOperation	//读数据
    public Map getDockerInfo(){
        return Collections.singletonMap("info","docker started...");
    }

    @WriteOperation	//操作/写数据
    private void restartDocker(){
        System.out.println("docker restarted....");
    }
}
```

场景：开发**ReadinessEndpoint**来管理程序是否就绪，或者**LivenessEndpoint**来管理程序是否存活；

当然，这个也可以直接使用 https://docs.spring.io/spring-boot/docs/current/reference/html/production-ready-features.html#production-ready-kubernetes-probes

## **原理解析**

### Profile功能

为了方便多环境适配，springboot简化了profile功能。

#### application-profile功能

- 默认配置文件  application.yaml；任何时候都会加载
- 指定环境配置文件  application-{env}.yaml

- 激活指定环境

- - 配置文件激活
  - 命令行激活：java -jar xxx.jar --**spring.profiles.active=prod  --person.name=haha**

- - - **修改配置文件的任意值，命令行优先**

- 默认配置与环境配置同时生效
- 同名配置项，profile配置优先

#### @Profile条件装配功能

```java
//方法和类上都可以用这个注释
@Configuration(proxyBeanMethods = false)
@Profile("production")
public class ProductionConfiguration {
    // ...
}
```

#### profile分组

```properties
spring.profiles.group.production[0]=proddb
spring.profiles.group.production[1]=prodmq

使用：--spring.profiles.active=production  激活
```

### 外部化配置

https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config

1. Default properties (specified by setting `SpringApplication.setDefaultProperties`).
2. `@PropertySource` annotations on your `@Configuration` classes. Please note that such property sources are not added to the `Environment` until the application context is being refreshed. This is too late to configure certain properties such as `logging.*` and `spring.main.*` which are read before refresh begins.

1. **Config data (such as** `**application.properties**` **files)**
2. A `RandomValuePropertySource` that has properties only in `random.*`.

1. OS environment variables.
2. Java System properties (`System.getProperties()`).

1. JNDI attributes from `java:comp/env`.
2. `ServletContext` init parameters.

1. `ServletConfig` init parameters.
2. Properties from `SPRING_APPLICATION_JSON` (inline JSON embedded in an environment variable or system property).

1. Command line arguments.
2. `properties` attribute on your tests. Available on `@SpringBootTest` and the [test annotations for testing a particular slice of your application](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-testing-spring-boot-applications-testing-autoconfigured-tests).

1. `@TestPropertySource` annotations on your tests.
2. [Devtools global settings properties](https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-devtools-globalsettings) in the `$HOME/.config/spring-boot` directory when devtools is active.

#### 外部配置源

常用：**Java属性文件**、**YAML文件**、**环境变量**、**命令行参数**；

#### 配置文件查找位置

(1) classpath 根路径

(2) classpath 根路径下config目录

(3) jar包当前目录

(4) jar包当前目录的config目录

(5) /config子目录的直接子目录

#### 配置文件加载顺序

1. 　当前jar包内部的application.properties和application.yml
2. 　当前jar包内部的application-{profile}.properties 和 application-{profile}.yml
3. 　引用的外部jar包的application.properties和application.yml
4. 　引用的外部jar包的application-{profile}.properties 和 application-{profile}.yml

**下覆盖上**



**指定环境优先，外部优先，后面的可以覆盖前面的同名配置项（类似cmd配置可以覆盖属性配置文件）**





### 自定义starter



#### starter启动原理

- starter-pom引入 autoconfigurer 包

  ![image-20211106153829530](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211106153829530.png)

- autoconfigure包中配置使用 **META-INF/spring.factories** 中 **EnableAutoConfiguration 的值，使得项目启动加载指定的自动配置类**
- **编写自动配置类 xxxAutoConfiguration -> xxxxProperties**

- - **@Configuration**
  - **@Conditional**

- - **@EnableConfigurationProperties**
  - **@Bean**

- - ......

**引入starter** **--- xxxAutoConfiguration --- 容器中放入组件 ---- 绑定xxxProperties ----** **配置项**

#### 自定义starter

- **atguigu-hello-spring-boot-starter（启动器）**
- **atguigu-hello-spring-boot-starter-autoconfigure（自动配置包）**

例子：

1. 创建一个空的Start场景

    **boot-09-customer-starter**

2. 创建两个具体模块

   <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211106164649380.png" alt="image-20211106164649380" style="zoom:80%;" /> 

3. 将starter场景中没必要的东西删掉，在pom文件中加入自动配置场景

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <groupId>com.atguigu</groupId>
       <artifactId>atguigu-hello-spring-boot-starter</artifactId>
       <version>1.0-SNAPSHOT</version>
   	<!-- 主要是以下这段 -->
       <dependencies>
           <dependency>
               <groupId>com.atguigu</groupId>
               <artifactId>atguigu-hello-spring-boot-starter-autoconfigure</artifactId>
               <version>0.0.1-SNAPSHOT</version>
           </dependency>
       </dependencies>
   
   </project>
   ```

4. 配置自动配置类，删除没必要的东西，并引入Springboot的启动依赖

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
       <parent>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-parent</artifactId>
           <version>2.4.0</version>
           <relativePath/> <!-- lookup parent from repository -->
       </parent>
       <groupId>com.atguigu</groupId>
       <artifactId>atguigu-hello-spring-boot-starter-autoconfigure</artifactId>
       <version>0.0.1-SNAPSHOT</version>
       <name>atguigu-hello-spring-boot-starter-autoconfigure</name>
       <description>Demo project for Spring Boot</description>
   
       <properties>
           <java.version>1.8</java.version>
       </properties>
   	<!-- 主要是下面这段 -->
       <dependencies>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter</artifactId>
           </dependency>
   
       </dependencies>
   
   </project>
   ```

5. 在resources文件夹中创建一个META-INF文件，放入string.factories配置文件，文件内容：

   ```properties
   # Auto Configure	相当于自动配置以下场景
   org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
   com.atguigu.hello.auto.HelloServiceAutoConfiguration
   ```

6. 在main的java文件下写具体的配置信息，以下均为例子

   1. 这是文件夹的结构
      <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211106165257571.png" alt="image-20211106165257571" style="zoom:80%;" /> 

   2. auto中放自动配置类

      ```java
      @Configuration
      @EnableConfigurationProperties(HelloProperties.class)  //默认HelloProperties放在容器中
      public class HelloServiceAutoConfiguration{
          @ConditionalOnMissingBean(HelloService.class)
          @Bean
          public HelloService helloService(){
              HelloService helloService = new HelloService();
              return helloService;
          }
      ```

   3. bean中放一些基本容器类

      ```java
      @ConfigurationProperties("atguigu.hello")	//与配置文件绑定，可以以atguigu.hello为开头配置属性信息
      public class HelloProperties {
      
          private String prefix;
          private String suffix;
      
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

   4. service中放一些方法

      ```java
      /**
       * 默认不要放在容器中，因为配置类中有关于这个的设置
       */
      public class HelloService {
      
          @Autowired
          HelloProperties helloProperties;
      
          public String sayHello(String userName){
              return helloProperties.getPrefix() + "："+userName+"》"+helloProperties.getSuffix();
          }
      }
      ```

7. 最后用mvn进行清除（clean）安装（install）到本地库，然后在其他场景中直接应用就可以了。



### SpringBoot原理

Spring原理【[Spring注解](https://www.bilibili.com/video/BV1gW411W7wy?p=1)】、**SpringMVC**原理、**自动配置原理**、SpringBoot原理

#### SpringBoot启动过程

- 创建 **SpringApplication**

- - 保存一些信息。
  - 判定当前应用的类型。ClassUtils。Servlet

- - **bootstrappers****：初始启动引导器（**List<Bootstrapper>**）：去spring.factories文件中找** org.springframework.boot.**Bootstrapper**
  - 找 **ApplicationContextInitializer**；去**spring.factories****找** **ApplicationContextInitializer**

- - - List<ApplicationContextInitializer<?>> **initializers**

- - **找** **ApplicationListener  ；应用监听器。**去**spring.factories****找** **ApplicationListener**

- - - List<ApplicationListener<?>> **listeners**

- 运行 **SpringApplication**

- - **StopWatch**
  - **记录应用的启动时间**

- - **创建引导上下文（Context环境）****createBootstrapContext()**

- - - 获取到所有之前的 **bootstrappers 挨个执行** intitialize() 来完成对引导启动器上下文环境设置

- - 让当前应用进入**headless**模式。**java.awt.headless**
  - **获取所有** **RunListener****（运行监听器）【为了方便所有Listener进行事件感知】**

- - - getSpringFactoriesInstances 去**spring.factories****找** **SpringApplicationRunListener**. 

- - 遍历 **SpringApplicationRunListener 调用 starting 方法；**

- - - **相当于通知所有感兴趣系统正在启动过程的人，项目正在 starting。**

- - 保存命令行参数；ApplicationArguments
  - 准备环境 prepareEnvironment（）;

- - - 返回或者创建基础环境信息对象。**StandardServletEnvironment**
    - **配置环境信息对象。**

- - - - **读取所有的配置源的配置属性值。**

- - - 绑定环境信息
    - 监听器调用 listener.environmentPrepared()；通知所有的监听器当前环境准备完成

- - 创建IOC容器（createApplicationContext（））

- - - 根据项目类型（Servlet）创建容器，
    - 当前会创建 **AnnotationConfigServletWebServerApplicationContext**

- - **准备ApplicationContext IOC容器的基本信息**  **prepareContext()**

- - - 保存环境信息
    - IOC容器的后置处理流程。

- - - 应用初始化器；applyInitializers；

- - - - 遍历所有的 **ApplicationContextInitializer 。调用** **initialize.。来对ioc容器进行初始化扩展功能**
      - 遍历所有的 listener 调用 **contextPrepared。EventPublishRunListenr；通知所有的监听器****contextPrepared**

- - - **所有的监听器 调用** **contextLoaded。通知所有的监听器** **contextLoaded；**

- - **刷新IOC容器。**refreshContext

- - - 创建容器中的所有组件（Spring注解）

- - 容器刷新完成后工作？afterRefresh
  - 所有监听 器 调用 listeners.**started**(context); **通知所有的监听器** **started**

- - **调用所有runners；**callRunners()

- - - **获取容器中的** **ApplicationRunner** 
    - **获取容器中的**  **CommandLineRunner**

- - - **合并所有runner并且按照@Order进行排序**
    - **遍历所有的runner。调用 run** **方法**

- - **如果以上有异常，**

- - - **调用Listener 的 failed**

- - **调用所有监听器的 running 方法**  listeners.running(context); **通知所有的监听器** **running** 
  - **running如果有问题。继续通知 failed 。****调用所有 Listener 的** **failed；****通知所有的监听器** **failed**

```java
public interface Bootstrapper {

	/**
	 * Initialize the given {@link BootstrapRegistry} with any required registrations.
	 * @param registry the registry to initialize
	 */
	void intitialize(BootstrapRegistry registry);

}
```

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1607005958877-bf152e3e-4d2d-42b6-a08c-ceef9870f3b6.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_18%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" /> 

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1607004823889-8373cea4-6305-40c1-af3b-921b071a28a8.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_20%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" /> 

<img src="https://cdn.nlark.com/yuque/0/2020/png/1354552/1607006112013-6ed5c0a0-3e02-4bf1-bdb7-423e0a0b3f3c.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_18%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="image.png" style="zoom:80%;" /> 

```java
@FunctionalInterface
public interface ApplicationRunner {
	/**
	 * Callback used to run the bean.
	 * @param args incoming application arguments
	 * @throws Exception on error
	 */
	void run(ApplicationArguments args) throws Exception;

}
```

```java
@FunctionalInterface
public interface CommandLineRunner {

	/**
	 * Callback used to run the bean.
	 * @param args incoming main method arguments
	 * @throws Exception on error
	 */
	void run(String... args) throws Exception;

}
```

#### Application Events and Listeners

https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-application-events-and-listeners

- **ApplicationContextInitializer**
- **ApplicationListener**
- **SpringApplicationRunListener**

#### ApplicationRunner 与 CommandLineRunner
