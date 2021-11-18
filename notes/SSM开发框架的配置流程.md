SSM开发框架的配置流程

1. 添加maven依赖（maven）

   - JDK1.8
   - junit单元测试
   - servlet依赖：servlet-api
   - jsp依赖：jsp-api
   - springmvc依赖：spring-webmvc
   - 事务相关的spring依赖：spring-tx
   - spring连接数据库依赖：spring-jdbc
   - json数据绑定，jackson依赖：jackson-databind
   - jackson核心依赖：jackson-core
   - mybatis依赖：mybatis
   - spring和mybatis整合依赖：mybatis-spring
   - java连接sql依赖：mysql-connector-java
   - 阿里巴巴连接池：druid

2. maven指定配置文件位置插件

   - 代码目录：src/main/java
   - 属性目录：**/ *.properties
   - 编译插件：maven-compiler-plugin

3. 配置java目录（maven）

   - 控制类目录：controller
   - 数据库连接层目录：dao
     - dao层中对应的映射文件（.xml），需要配置对应的dao接口
   - 实体类目录：enity 或者 domain
   - 服务层目录：service
     - 服务层目录下的实现方法目录

4. 配置资源文件

   - spring配置文件：applicationContext
     - 数据库连接池以及数据库信息
     - 声明SqlSessionFactoryBean，使其能创建sqlSesson来操作数据库
     - 声明mybatis配置文件的扫描器，创建dao对象
     - 声明注解@Servlce所在位置
     - （可选）事务配置：注解和aspect配置

   - JDBC属性文件
     - 数据库：url
     - 用户名：username
     - 密码：password
     - 最大并发事务数量：maxActive
   - mybatis属性文件
     - 输出日志配置
     - 实体层（enity&domain）扫描器
     - 数据库连接层（dao）扫描器
   - springmvc配置文件
     - 控制层扫描器（controller）
     - 保密性前后缀配置（配置完成后前端页面可以放在web-inf目录下）
     - ajax请求返回json的声明

5. web.xml的配置

   - 注册中央调度器：DispatcherServlet，注册springmvc配置
   - 注册servlet处理过滤：*.do
   - 注册spring容器：applicationContext
   - 注册监听器
   - 注册字符集过滤器

注意点：

1. 数据库（dao）连接层中的数据库方法接口和xml映射写在同一个目录下（且不能放在不同子目录），而且接口名和映射文件名必须完全相同。
2. @Service是使用在服务层上的注解，作用是可以将当前类直接注入到spring容器中，不需要再applicationContext中配置bean。例如在控制层（controller）中，若是用到服务层的类，则可以直接用@Autowired赋值
3. @Conponent也是创建类的注解和@Servlce，@Repository，@Controller一样的用法，对于不知道是什么层的类可以用这个注解，这个注解一般用于实体类，但是mybatis配置文件中可以用typeAliases标签直接扫描实体类且实体类本身不需要使用注解就可以被创建对象（对象名为类名的首字母小写）。
4. @Repository注解是使用在数据库连接（dao）层的注解，作用是创建dao层的类对象，但在mybatis配置文件中，可以用mappers标签直接扫描dao目录获取映射。
5. @Value是对简单类型属性赋值







学完springboot2之后 ------------>写个项目 ------------->重新学mysql数据库 ------------>人工智能-------------->pmp证书