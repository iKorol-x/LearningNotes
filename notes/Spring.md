# Spring全家桶

spring 、springmvc 、spring boot 、spring cloud

	特点：

1. 轻量
2. 针对接口编程，解耦合
3. AOP编程的支持，提高了开发效率 和质量
4. 方便集成各种优秀框架<!--如Struts、Hibernate、MyBatis等-->

**IoC & AOP**

IoC（Inversion of Control）:控制反转

AOP:面向切面编程

# IoC

IoC是一种思想，一种理论，描述的是：把对象的创建、赋值，管理工作都交给代码之外的容器实现，也就是对象的创建是由其他外部资源完成的。

	控制：创建对象、对象属性的舒服，对象之间的关系管理。
	
	反转：把原来由开发人员管理、创建对象的权限转移给代码之外的容器实现。由容器代替开发人员管理对象，创建对象和给属性赋值。
	
	正转：即正常情况下，开发人员通过构造方法主动new一个对象，即为正转。
	
	容器：容器可以是一个服务器软件，也可以是一个框架（例：Spring）

IoC的作用：可以减少对代码的改动，也可以实现不同的功能。实现解耦合。

## java中创建对象的方法？

- 构造方法
- 反射
- 序列化
- 克隆
- 动态代理
- IoC：即容器创建对象，与前面几种不一样

## IoC的体现

servlet

1. 创建类继承HttpServlet

2. 在web.xml注册servlet，使用标签语言

   ```xml
   <servlet-name>myservlet</servlet-name>
   <servlet-class>com.controller.myservlet</servlet-class>
   ```

   	Servlet的实现方式就是一种IoC的体现，代码中从来没有创建过Servlet对象，因为这个Servlet对象实际上是Tomcat服务器创建的。Tomcat也可以叫做容器，Tomcat作为容器，里面存放的有Servlet对象，Filter过滤器对象以及Listener监听器对象。

## IoC的技术实现

	DI（Dependency Injection）：依赖注入。只需要在程序中提供要使用的对象名称就可以，至于对象如何在容器中创建、赋值、查找都有容器内部实现。
	
	Spring就是使用DI技术实现了IoC的功能，Spring底层创建对象使用的是反射机制。

### 步骤

1. 创建maven项目

2. 加入maven依赖

   ```xml
    <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
         <version>5.3.3</version>
       </dependency>
   ```

3. 创建类（接口和他的实现类）

4. 创建spring需要的配置文件

   ```xml
   beans.xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
   		
            <!--
               1.beans:是根标签，spring把java对象称为bean
               2.spring-beans.xsd是约束文件，和mybatis的dtd文件一样
           -->
   
   
      		 <!--
               首先告诉spring创建的对象
               声明bean，就是告诉spring创建某个类的对象
               id:对象的自定义名称，唯一值，spring就通过这个名称找到对象(key)
               class:类的全限定名称（不能是接口，因为spring底层是通过反射机制创建的对象，必须使用类）
      		 -->
       <bean id="hello1" class="org.example.Hello"></bean>
       	<!--
               此时spring就完成了Hello hello1 = new Hello();
               spring是把创建好的对象放入到map中，spring框架有一个map专门存放对象。
               springMap.put(id,对象);//就是以这种格式放入map对象
                   例如：springMap.put(hello1,new Hello());
               一个bean标签只声明一个对象，多个对象需要多个bean标签
       	-->
   </beans>
   ```

5. 测试spring创建的类。

   ```java
    @Test
       public void Test01()
       {
           //正转
           Hello h1 = new Hello();
           h1.saysome();
       }
   
       @Test
       public void Test02(){
           //spring创建对象调用
   
           String config="beans.xml";
   		return i;
   
           /**
            使用spring容器创建对象
            1.指定spring配置文件的名称(即上面的config)
            2.创建表示spring容器的对象，ApplicationContext(在java中，这个类就是代表了spring对象，是一个接口类)
            3.ClassPathXmlApplicationContext()是实现了ApplicationContext接口的一个类。
               这个方法的作用就是通过类路劲(即工程文件target中的路径)，加载到spring的配置文件(即config)
            */
           ApplicationContext applicationContext = new ClassPathXmlApplicationContext(config);
   
           /**
            如何获取指定的对象呢？
               使用ApplicationContext对象的getBean()方法：
                   applicationcontext。getBean(“配置文件中bean对象的ID”)
               获取到这个类之后还要进行向下转型，因为getBean方法默认返回的是Object类型
               在向下转型之后才能使用对象的方法。
            */
           HelloIF hello =(HelloIF) applicationContext.getBean("hello1");
           System.out.println("Spring创建类！");
           hello.saysome();
   
           /*
           这就是一个典型的DI方式
           * */
       }
   ```

   Spring默认创建对象的时机：在创建spring的容器时，就会创建配置文件中的所有对象。
   
   创建对象时默认调用无参构造方法。

## 获取Spring容器中的信息

	两个方法：
	
	getBeanDefinitionCount()：int类型，获取Spring容器中对象的数量。
	
	getBeanDefinitionNames()：String数组类型，获取Spring容器中的对象名字。

```java
	 @Test
    public void Test03(){
        String config="beans.xml";
        ApplicationContext atc = new ClassPathXmlApplicationContext(config);
        //获取Spring容器中对象的数量。
        int nums = atc.getBeanDefinitionCount();
        System.out.println("容器中对象的数量："+nums);
        //获取Spring容器中的对象名字
        String[] names = atc.getBeanDefinitionNames();
        for (String name:names){
            System.out.println(name);
        }
    }
```

## Spring创建一个非自定义的对象

xml设置

```xml
<!--    Spring 创建非自定义对象（日期类）-->
    <bean id="date" class="java.util.Date"></bean>
```

测试

```java
Date date = (Date) atc.getBean("date");
System.out.println(date);
```

## 属性赋值

在Spring的配置文件中，给java对象的属性赋值。

DI：依赖注入，表示创建对象，给属性赋值。

DI的实现方式：

- 在Spring的配置文件中，使用标签和属性完成，叫做基于XML的DI实现。
- 使用Spring中的注解，完成属性赋值，叫做基于注解的ID实现

### DI的语法分类

- #### **set注入（设值注入）**

  Spring调用类的set方法，用set方法可以实现属性的赋值。（80%的开发都是用的这种方法）

  	1）对于简单类型（基本数据类型）：

  用bean标签的property属性

  java类需要有set方法。

  ```xml
  applicationContext.xml
  <!--简单类型的set注入-->
      <bean id="stu01" class="org.Test01.Student">
          <!--  在Spring容器创建这个对象时，会直接调用他的set方法，通过set方法赋值 -->
          <property name="age" value="41"></property>
          <property name="name" value="mcl"></property>
          <!-- 没有set方法会报错 -->
      </bean>
  ```

  	这个set方法只能单纯的被property属性调用，但具体set执行是什么效果还是可以控制的。也就是说，若是没有这个属性，但有这个方法，只要不涉及到这个属性的类型还是可以执行的。
	
  	属性所有的值都需要放在双引号中（不管是int还是string），这是xml文件的规则
	
  	set方法的调用在构造方法之后。
	
  	2）对于引用类型：

  主要是将value改成ref，ref后面的值应该是已经加载过的类。

  ```xml
  <!--    引用类型的set注入-->
      <bean id="stu02" class="org.Test02.Student">
          <property name="age" value="21"></property>
          <property name="name" value="mcl2"></property>
          <property name="school" ref="school"></property>
      </bean>
      <bean id="school" class="org.Test02.School">
          <property name="name" value="苏科大"></property>
       </bean>
  ```

  	测试类基本不变，但最好换一个类，不要在同一个个类中重复测试。

- #### **构造注入**

  Spring调用类的有参数构造方法，创建对象。在构造方法中完成赋值。

  	构造注入使用<constructor-arg>标签。
  	
  	<construct-arg>标签的属性：
  	
  		name：表示构造方法的形参名
  	
  		index：表示构造方法的参数的位置，参数从左往右依次为0 ，1 ， 2 ...的顺序
  	
  		value：构造方法的形参类型是简单类型
  	
  		ref：构造方法的形参类型是引用类型

  ```xml
   <bean id="scl" class="org.Test02.School">
          <property name="name" value="苏科大"></property>
       </bean>
  <!-- 使用name属性 -->
      <bean id="stu02" class="org.Test02.Student">
          <constructor-arg  value="31"></constructor-arg>
          <constructor-arg name="name" value="mcl3" />
          <constructor-arg name="school" ref="scl"/>
      </bean>
  <!-- 使用index属性 -->
      <bean id="stu03" class="org.Test02.Student">
          <constructor-arg index="0" value="31"></constructor-arg>
          <constructor-arg index="1" value="mcl3" />
          <constructor-arg index="2" ref="scl"/>
     </bean>
  <!-- 也可以不写name和index属性。但值的顺序要与有参构造的参数顺序相同。-->
       
      
  ```

  	对于<construct-arg>，类中的有参数构造有几个参数就要有几个<construct-arg>的属性标签

  文件类的构造注入

  	xml文件配置

  ```xml
     <bean id="myfile" class="java.io.File">
          <constructor-arg name="parent" value="D:\"></constructor-arg>
          <constructor-arg name="child"   value="hw3_input.txt"></constructor-arg>
      </bean>
  ```

  	测试类

  ```java
      @Test
      public void test04(){
          String config = "Test02/applicationContext.xml";
          ApplicationContext ac = new ClassPathXmlApplicationContext(config);
          File file = (File) ac.getBean("myfile");
          String filename = file.getName();
          System.out.println(filename);
      }
  ```

  ### **引用类型简化**

  	对于引用类型，多次的调用会让整个xml看起来冗余而复杂，所以有两种自动注入引用类型的方法：

  1. byName()：按名称注入，java类中引用类型的属性名和spring容器中（即配置文件）<bean>的id名称一样，且数据类型一直，这样的容器中的bean，spring能够赋值给引用类型。

     ```xml
     <bean id="school" class="org.Test03.School">
             <property name="name" value="苏科大"></property>
          </bean>
     <!--这里用了autowire的byName属性，即自动引入引用类型，
         但这里被自动调用的引用类（即在xml文件中配置的bean的id）必须名称与Student类的变量名相同，
         Student中调用的引用对象的属性也必须与对象属性相同--> 
         <bean id="stu" class="org.Test03.Student" autowire="byName">
             <property name="name" value="mcl"/>
             <property name="age" value="23"/>
     <!--        <constructor-arg index="2" ref="school"/>-->
         </bean>
     ```

     		反应在本例中就是，Student的School类的属性名是school，所以School的bean的id就必须是school，然后这个被赋值的属性school也必须是School类型，不然接收不了。

  2. byType()：按类型注入，Java中引用类型的数据类型和Spring容器中（配置文件）<bean>的class路径是同源关系，这样的bean能够赋值给引用类型。

     	同源的意思：

     - java类中引用类型的数据类型和bean的class类是一样的。
     - java类中引用类型的数据类型和bean的class的类是父子类关系。
     - java类中引用类型的数据类型和bean的class的类是接口和实现类关系。

     ```xml
      <bean id="scL" class="org.Test03.School">
             <property name="name" value="苏科大"></property>
         </bean>
         <!--
             byType属性：标记byType的类，若是有引用类型的属性，则spring会自动在容器中寻找同源类，将同源类中的值与引用类型值对应匹配。
     
         -->
         <bean id="stu" class="org.Test03.Student" autowire="byType">
             <property name="name" value="mcl"/>
             <property name="age" value="23"/>
             <!--        <constructor-arg index="2" ref="school"/>-->
         </bean>
     ```

     byType方法在找对应的bean时，bean不能有多个同源文件，即bean对应的路径不能是同源的，否则会报错。

## 关于单元测试Junit

1. 需要加入依赖

```xml
 <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
```

2. 创建的测试类位置要在src/test/java目录下

3. 创建测试方法

   - public方法

   - 没有返回值void

   - 方法名称自定义，建议名称是test+测试方法名称

   - 方法没有参数

   - 方法的上面需要加入@Test注解

     ```java
      @Test
     public void test01(){}
     ```

   一些注意点
   
   	若是有同名类不要写在一个测试类里面，会找不到这个类。

## 包含关系的配置文件

**多个配置的必要性**

1. 多个配置文件比一个文件小很多，效率高。

2. 由于项目很可能不是同一个人开发的，所以难免会对配置文件进行冲突修改，多配置文件可以避免多人竞争带来的冲突。

   假如项目由多个模块组成，一个模块一个配置文件。

   	例：学生考勤一个模块，学生成绩另一个模块，由不同的人设计编写

   多文件的分配方式：

   - 按功能的模块：一个模块一个配置文件
   - 按类的功能：例如，数据库相关的一个配置文件、事务功能相关的一个配置文件...

**使用方法**

	多个xml配置文件对应不同的模块，但可以设置一个spring-total.xml文件作为总的配置文件。
	
	这个配置文件包含其他的配置文件，但主配置文件一般不定义对象。
	
	语法：<import resource="classpath：其他配置文件路径"/>
	
	关键字 "classpath："表示类的路径（classes文件所在的目录，即目标工程下的目录），在spring'的配置文件中要指定其他文件的位置，使用classpath关键字告诉spring要去哪里找这个文件。

```xml
    <import resource="classpath:Test01P/applicationContext.xml"/>
    <import resource="classpath:Test02/applicationContext.xml"/>
```

	这样就可以在读取配置文件时，读取主配置文件即可使用子配置文件中的所有bean。
	
	在包含关系的配置文件中也可以使用通配符

```xml
 <import resource="classpath:Test*.xml"/><!-- *可以代表任意字符 -->
```

	就相当于前缀时Test的所有xml文件都会被导入到这里面。但需要注意的是，主配置文件的名字不能包含在通配符的范围内，不然会重复导入造成死循环。 还有就是如果要使用通配符，xml文件必须是在一个目录下的，且必须是resources下的目录下的文件，这是写在源代码里面的，是规定。

## 基于注解的DI

	通过注解完成java对象的创建，属性赋值。

使用注解的步骤：

1. 加入maven依赖spring-context，再加入spring-context的同时，会间接加入spring-aop依赖。注解的使用依赖于spring-aop依赖。

2. 在类中加入spring注解（多个不同功能的注解）

3. 在spring的配置文件中，加入一个组件扫描器的标签，说明注解在项目中的位置

   注解扫描器：component-scan，组件就是java对象

   属性：base-package：指定注解在项目中的命名

   工作方式：spring会扫描遍历base-package指定的包，把包和子包中的所有类都扫描一遍，找到类中的注解，按照注解的功能创建对象，或者给属性赋值。

```xml
<!--	
		这是在添加组件扫描器的时候自动添加的东西，了解一下就行
		xmlns:context="http://www.springframework.org/schema/context"	//context指向context
		http://www.springframework.org/schema/context	//这是一个命名控件，url的别名，context指向url
		https://www.springframework.org/schema/context/spring-context.xsd	//这是约束文件的url，可以在网上访问
-->
<context:component-scan base-package="org.Test04"/>
```

	扫描多个包的方法：

1. 多次使用component-scan标签

   ```xml
   <context:component-scan base-package="org.Test03"/>
   <context:component-scan base-package="org.Test04"/>
   ```

2. 中间用分隔符（；或者，）将包隔开

   ```xml
   <context:component-scan base-package="org.Test03;org.Test04"/>
   ```

3. 导入父包

   ```xml
   <context:component-scan base-package="org"/>	<!-- 不建议写顶级包，费时，会进行无用的扫描 -->
   ```

   

**加入组件扫描器对配置文件的影响**

1. 	加入了一个新的约束文件：spring-context.xsd
2.     给这个新的约束文件起了一个命名

需要学习的注解：

- **@Component**
  创建对象的注解，等同于<bean>的功能

  	属性：value就是对象的名称，也就是bean的id值，value的值是唯一的，创建的对象在整个spring容器中就一个

  这个注解就相当于在xml中设置了一个id为animal01，class为这个类的bean。

  ```java
  @Component(value = "animal01")	
  //这个value可以省略，直接写id值也可以，比较常用		
  @Component("animal01")	
  //也可以直接不写，由Spring默认提供一个名称：首字母小写的类名
  @Component	//value默认为 animal 	
  public class Animal { ... } 
  ```

  以下三个（@Repository、@Service、@Controller）的使用语法与@Component一致，都能创建对象，都有自己相应的功能。

  他们的作用：给项目对象分层。

  若是不确定或者不知道对象属于什么层，就用@Component。

- **@Repository**

  	@Repository的功能和@Component一样，但@Repository是放在dao的实现类上面，表示创建dao对象，dao对象是能访问数据库的，这个注解属于持久层上的注解。

- **@Service**

  	@Service的功能也和@Component一样，但@Service是放在service层上的，创建service对象，service对象是做业务处理的，可以有事务等功能的，属于业务层上的注解。

- **@Controller**

  	@Controller的功能也和@Component一样，但@Controller是放在Controller（控制器）层上的，创建Controller对象，Controller对象是接收用户提交的参数，显示请求的处理结果，属于控制层上的注解。

- **@Value**

  即简单类型的属性赋值：属性value是String类型，表示简单类型的属性值

  	位置： 1.在属性定义的上面，无需set方法，推荐使用

  ```java
  	@Value(value = "张三")
      private String name;
      @Value("true")
      private boolean sex;
  	//加不加“value = ”都行，直接写就是默认value
  
  ```

  				2.在set方法的上面

  ```java
     @Value("张三")
      public void setName(String name) {
          this.name = name;
      }
  ```

- **@Autowired**

  @Autowired（引用类型的注入）：spring框架提供的注解，实现引用类型的注入。

  	Spring中通过注解给引用类型赋值，使用的是自动注入原理，支持byName，byType
	
  	@Autowired默认使用的是byType方式注入
	
  	属性：required，是一个boolean类型，默认为true（一般都为true，可以尽早发现错误）
	
  		required = true：若是赋值失败，则程序终止执行，并报错
	
  		required = false：若是赋值失败，则程序正常执行，引用类型的值为null

  位置：

  	1.在属性定义上面，无需set方法，推荐使用
	
  	2.在set方法上面

  ```java
     @Autowired
      private Place forest;
  ```

  Place类也需要称为spring里的对象才行

  ```java
  @Component("forest")
  public class Place {
      @Value("朱拉大森林")
      private String forest;
      @Override
      public String toString() {
          return "Place{" +
                  "forest='" + forest + '\'' +
                  '}';
      }
  ```

  若是想使用byName方式注入，则：

   	1.	在属性上面添加@Autowired注释
   	2.	在属性上面添加@Qualifier(value = "bean的id")

  这就表示使用指定的bean完成赋值

  ```java
   @Autowired
   @Qualifier("forest")
      private Place forest;
  ```

  代表了通过byName将bean的id为"forest"的对象加入到Place属性中去。

- **@Resource**

  	也用于引用类型赋值，但这是来自jdk中的注解，Spring框架提供了对这个注解的功能支持，可以使用它给引用类型赋值，使用的也是自动注入原理，支持byName,byType，默认是byName。
  	
  	位置：1.	在属性定义的上面，无需set方法，推荐使用
  	
  			   2.	在set方法上面

  但他有一个规则：先是默认使用byName自动注入，若是byName赋值失败，在使用byType。

  ```java
  import javax.annotation.Resource;
  //这个包，新版的Jakarta好像不能注入，能找到这个值，但不能传入其他类中 
  @Component("animal01")
  public class Animal {
  	@Resource
      private Place forest1;
      //即使这里的forest1名字与forest不同仍然可以通过byType获取到
  }
  ```

  ```java
  @Component("forest")
  public class Place {
      @Value("朱拉大森林")
      private String forest;
  }
  ```

  也可以指定只用byName的方式 ： @Resource(name =  " bean 的 id ")；

## Porperties文件与注解的结合使用

properties文件

```properties
name = zhangsan 
sex = true
```

bean文件的value设置

```java
//这个"${}"表达式就是通过properties写的key值获取value值
@Value("${name}")
private String name;
@Value("${sex}")
private boolean sex;
```

xml的配置文件导入properties文件

```xml
 <context:property-placeholder location="Test.properties"/>
```

# AOP

	即面向切面编程（Aspect Orient Programming），AOP就是动态代理的规范化，把动态代理的实现步骤，方式都定义好了，让开发人员用一种统一的方式，就是动态代理。
	
	Aspect：切面，是给你的目标类增加的功能就是切面，如日志，事务等。
	
			切面的特点：一般是非业务方法，独立使用
	
	Orient：对着，面向

怎么理解面向切面编程？

1. 	需要在分析项目功能时，找出切面
2.     合理安排切面的执行时间（在目标方法前还是目标方法后）
3.     合理的安排切面的执行位置，在那个类，那个方法增强功能

术语

1. Aspect：切面，表示增强的功能，就是一堆代码，完成某一个功能，非业务功能。常见的切面功能有：日志，事务，统计信息，参数检查，权限验证等。
2. JoinPoint：连接点，连接业务方法和切面的位置，理解上就是某类中的业务方法。
3. Pointcut：切入点，指多个连接点方法的集合，多个需要加入切面的方法。
4. 目标对象：给那个类增加功能，这个类就是目标对象。
5. Advice：通知，表示切面功能的执行时间。

切面的关键三要素

1. 切面的功能代码，切面要干什么
2. 切面的执行位置，使用Pointcut表示切面的执行位置
3. 切面的执行时间，使用Advice表示时间，在目标方法之前还是目标方法之后

## 动态代理

	用于对象的创建，创建对象之后在不改变原来代码的前提下对对象的功能进行增加或者增强。

通过动态代理的各种方法，可以将日志和事务与业务分离开来，实现解耦合，让业务方法专注于业务逻辑，不用分心去做其他处理。

### 实现方式

AOP是一个规范，是一个动态的规范化，一个标准。

#### JDK动态代理

	使用JDK包中的3个类：Proxy、Method和InvocationHandler，来实现代理的功能。但JDK代理有一个前提，就是目标对象必须是实现接口的，这是JDK动态代理的要求。
	
	实现步骤：

1. 创建目标类，即要添加、增强方法，输出时间，提交事务的类

   	接口Test01

   ```java
   public interface Test01 {
       public void doSome();
       public void doOther();
   }
   ```

   实现类TestImp

   ```java
   public class TestImp implements Test01 {
       @Override
       public void doSome() {
           System.out.println("doSome方法执行！");
       }
       @Override
       public void doOther() {
           System.out.println("doOther方法执行！");
       }
   }
   ```

   工具类TestTools01

   ```java
   public class TestTools01 {
       public static void doLog(){
           System.out.println("方法运行时间："+new Date());
       }
       public static void doTrans(){
           System.out.println("事务提交");
       }
   }
   ```

2. 创建InvocationHandler接口的实现类，在这个类实现给目标方法增加功能

   InvocationHandler接口的实现类MyTest01

   ```java
   public class MyTest01 implements InvocationHandler {
       private Object target;
       //用构造方法获取进来的参数对象
       //这种方式可以让类动态赋给target
       public MyTest01(Object target) {
           this.target = target;
       }
       @Override
       public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
           Object res = null;
           String name = method.getName();
           if("doOther".equals(name)) {//对方法进行筛选
               TestTools01.doLog();
               res = method.invoke(target, args);
               TestTools01.doTrans();
           }else{
               res = method.invoke(target, args);
           }
           return res;
       }
   }
   ```

   

3. 使用JDK中的类 Proxy，创建代理对象，实现创建对象的能力

   测试类test01

   ```java
   public class test01 {
       public static void main(String[] args) {
           //创建目标对象
           Test01 target = new TestImp();
           //创建InvocationHandler对象
           InvocationHandler handler = new MyTest01(target);
           //用Proxy的newProxyInstance方法，创建代理对象，他的三个参数：
           //第一个是目标类的类加载器，用反射机制获取
           //第二个是目标类的接口，也要用反射机制获取
           //第三个参数是实现了InvocationHandler的对象类
           Test01 proxy = (Test01) Proxy.newProxyInstance(
                   target.getClass().getClassLoader(),
                   target.getClass().getInterfaces(),
                   handler);
           //这个doSome经过JDK处理，会让invoke方法中的method方法变成target对象的doSome方法
           proxy.doOther();
           System.out.println("============================================================");
           //若是需要对方法进行筛选（即并不需要所有的方法都执行日志和事务提交，可以在invoke方法中用if语句判断）
           proxy.doSome();
       }
   }
   ```


##### AOP的技术实现框架

1. spring：spring在内部实现了AOP规范，能做AOP工作，spring主要是在事务处理时使用AOP，现在开发项目中也很少使用spring的AOP，因为spring的AOP比较笨重

2. aspectJ：一个开源的专门做AOP的框架，spring框架中集成了aspectJ框架，通过spring就能使用aspectJ的功能。

   	aspcetJ框架实现AOP的两种实现方式：

   - 使用xml的配置文件：配置全局事务
   - 使用注解，我们项目中要做AOP功能，一般都使用注解，aspectJ有5个注解。

##### aspectJ的使用

1. 	切面的执行时间，这个执行时间在规范中叫做Advice（通知，增强）在aspectJ框架中使用注解表示的，也可以使用xml配置文件中的标签
   - @Before
   - @AfterReturning
   - @Around
   - @AfterThrowing
   - @After
2. AOP的实现方式

AspectJ的切入点表达式：AspectJ定义了专门的表达式用于指定切入点，表达式的原型为：

		execution（modifiers-pattern ？ret-type-parttern 
							declaring-type-parttern ？ name-parttern（param-pattern）
							throws-pattern？）
	
	名词解释：
	
		modifiers-pattern：访问权限类型
	
		ret-type-pattern：返回值类型
	
		declaring-type-pattern：包名类型
	
		name-pattern（param-pattern）：方法名（参数类型和参数个数）
	
		throws-pattern：抛出异常类型
	
		？表示可选部分
	
	即 	execution（访问权限	方法返回值	方法声明（参数）	异常类型）。方法返回值和方法声明（参数）为必填项
	
	在aspectJ的表达式中也可以用以下通配符：
	
		* ：0至多个字符
	
		.. ：用在方法参数中，表示任意多个参数。用在包名后，表示当前包及其子包路径
	
		+ ：用在类名后，表示当前类及其子类。用在接口后面，表示当前接口及其实现类
	
	使用步骤

1. 新建maven项目

2. 加入依赖

   - spring依赖

   - aspectJ依赖

   - Junit单元测试依赖

     ```xml
     <!--spring依赖-->
     <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
         <version>5.3.3</version>
     </dependency>
     <!--aspectj依赖-->
     <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-aspects</artifactId>
         <version>5.3.8</version>
     </dependency>
     <!--单元测试依赖-->
     <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
         <version>4.11</version>
         <scope>test</scope>
     </dependency>
     ```

     

3. 创建目标类：接口和他的实现类（要给类中的方法添加功能）

   接口类SomeService

   ```java
   public interface SomeService {
       void doSome(String name,Integer age);
   }
   ```

   实现类SomeServiceImpl

   ```java
   public class SomeServiceImpl implements SomeService{
       @Override
       public void doSome(String name, Integer age) {
           System.out.println("============doSome方法执行============");
           System.out.println(name +" : "+age);
       }
   }
   ```

   

4. 创建切面类：普通类

   - 在类的上面加上@Aspect

   - 在类中定义方法，方法就是切面要执行的功能代码，在方法上面加入aspectj中额通知注解例如@Before，有需要的话指定切入点表达式execution()

     切面类MyAspect

     ```java
     @Aspect
     public class MyAspect {
         /*
          * 定义方法：方法是实现切面功能的
          * 方法定义的要求：
          *  1.公共方法public
          *  2.方法没有返回值
          *  3.方法名称自定义
          *  4.方法可以有参数也可以没有参数
          *      如果有参数，参数不是自定义的，有几个参数类型可以定义
          */
         /*
          * @Before：前置通知注解
          *  属性：value，是切入点表达式，表示切面的功能执行的位置
          *  位置：在方法上面
          * 特点：
          *  1.在目标方法执行之前执行
          *  2.不会改变目标方法的执行结果
          *  3.不会影响目标方法的执行
          */
         
         //
         @Before("execution(public void org.example.SomeServiceImpl.doSome(String,Integer))")
         public void myBefore(){
             //切面执行的功能代码
             System.out.println("前置通知，切面功能："+new Date());
         }
     ```

     

5. 创建spring的配置文件：声明对象，将对象交给容器统一管理。可以使用注解或者xml配置文件<bean>

   - 声明目标对象

   - 声明切面对象

   - 声明aspectj框架中的自动代理生成器标签。

     自动代理生成器：用来完成代理对象的自动创建功能。

     Spring配置文件：applicationContext.xml

     ```xml
     	<!--将对象统一交给Spring容器，有Spring容器同意创建管理-->
         <!--声明目标对象-->
         <bean id="someService" class="org.example.SomeServiceImpl"/>
         <!--声明切面类对象-->
         <bean id="myAspect" class="org.example.MyAspect"/>
         <!--声明自动代理生成器：使用aspectj框架内部的功能，创建目标对象的代理对象
             创建代理对象是在内存中实现的，修改目标对象的内存中的结构，创建为代理对象
             所以目标对象就是被修改后的代理对象
     
             aspectj-autoproxy：会把spring容器中所有的目标对象，一次性都生成代理对象
         -->
         <aop:aspectj-autoproxy/>
         <!--
         xmlns:aop="http://www.springframework.org/schema/aop"
         http://www.springframework.org/schema/aop
         https://www.springframework.org/schema/aop/spring-aop.xsd
             这些都是aspectj的约束
         -->
     ```

     

6. 创建测试类，从spring容器中获取目标对象（实际上是代理对象）。通过代理执行对象方法，实现aop的功能增强。

   测试类test01

   ```java
    public void test01(){
           ApplicationContext atx = new ClassPathXmlApplicationContext("applicationContext.xml");
        	//这里的类型只能是接口类型接收，不然就报错，已经是体验过了
           SomeService service  = (SomeService) atx.getBean("someService");
           //com.sun.proxy.$Proxy8这是动态代理生成的对象
         	System.out.println(service.getClass().getName());
           service.doSome("mcl",20);
       }
   ```

   ###### JoinPoint的使用

   	指定通知方法中的参数：JoinPoint，JoinPoint在5个注解中都可以用
   	
   	JoinPoint：业务方法，要加入切面功能的业务方法。
   	
   		作用是：可以在通知方法中获取方法执行时的信息，例如方法名称，方法的实参等。如果切面功能中需要使用到方法的信息，就加入JoinPoint。这个JoinPoint是由框架赋予的，必须是第一个位置的参数

   ```java
    @Before("execution(* *..*.do*(..))")
       public void myBefore(JoinPoint jp){
           //方法的完整定义getSignature
           System.out.println("方法的完成定义 ----->"+jp.getSignature());
           //方法的名称getSignature().getName()
           System.out.println("方法的名称 ----->"+jp.getSignature().getName());
           //获取方法的参数，是数组类型
           Object[] args = jp.getArgs();
           for(Object arg : args){
               System.out.println("方法的参数 ----->"+arg);
           }
           //切面执行的功能代码
           System.out.println("===============前置通知，切面功能："+new Date());
       }
   ```

   ###### @AfterReturning使用方法

   - 切面类MyAspect

   ```java
    /**
        * @AfterReturning：后置通知注解
        *  属性：1.value，是切入点表达式，表示切面的功能执行的位置
        *       2.returning自定义的变量，表示目标发放返回值的。
        *          自定义变量名必须和通知方法的形参名一样
        *  位置：在方法上面
        * 特点：
        *  1.在目标方法执行之后执行
        *  2.能够获取到目标方法的返回值，可以根据这个返回值做不同的处理功能
        *  3.可以修改这个返回值
        *  执行过程：
        *      Object res = doOther();
        *      传参：myAfterReturning(res)
        *      System.out.println("res="+res);
        */
       @AfterReturning(value = "execution(* *..SomeServiceImpl.doOther(..))", returning = "res")
       public void myAfterReturning(Object res){
           //道理上来讲String类型的应该是不能改的，但看来新版本是可以改的了
           res = "Changed";
           System.out.println("后置通知---->获取方法返回值---->"+res);
       }
   ```

   实现类：SomeServiceImpl.java

   ```java
    @Override
       public String doOther(String name, Integer age) {
           System.out.println("------------>doOther方法执行<-----------");
           System.out.println("------------>"+name+" : "+age+"<--------------");
           return "Complete!";
       }
   ```

   测试类：Test.java

   ```java
    @Test
       public void test02(){
           ApplicationContext atx = new ClassPathXmlApplicationContext("applicationContext.xml");
           SomeService service = (SomeService) atx.getBean("someService");
           service.doOther("飞天小女警",12);
       }
   ```

   ###### @Around的使用
   
   	@Around环绕通知方法的定义格式：
   
   1. public
   2. 必须有一个返回值，推荐使用Object
   3. 方法名称自定义
   4. 方法有参数，固定的参数：ProceedingJoinPoint
   
   属性：value（切入点表达式）
   
   位置：在方法定义之前
   
   特点
   
   1. 功能最强的通知
   2.  在目标方法的前后都能增强功能
   3. 控制目标方法是否被调用执行
   4. 修改原来目标方法的执行结果。影响最后的调用结果
   
   环绕通知等同于JDK动态代理中的InvocationHandler接口
   
   参数ProceedingJoinPoint等同于JDK动态代理中的method参数，作用也是执行目标方法。
   
   返回值就是目标方法的执行结果，是可以被修改的
   
   环绕通知：经常做事务，在目标方法之前开启事务，执行目标方法，在目标方法之后提交事务
   
   测试：
   
   目标类方法
   
   ```java
    @Override
       public String doAround(String name) {
           System.out.println("-------->doAround<--------");
           System.out.println("-------->"+name+"<--------");
           return name;
       }
   ```
   
   切面方法MyAround
   
   ```java
    //环绕通知
       @Around("execution(* *..SomeServiceImpl.doAround(..))")
       public Object MyAround(ProceedingJoinPoint pjp) throws Throwable {
           //ProceedingJoinPoint是JoinPoint的子类，所以能够继承JoinPoint的所有方法
           //也能够对传进来的参数，方法的名称等进行判断是否执行通知
           Object result=null;
           System.out.println("前置通知-------------->"+new Date());
           //这句话等同于JDK动态代理中的proxy.invoke()
           //Object result = doAround();
           result = pjp.proceed();
           System.out.println("后置通知-------------->事务提交");
           result = "黑猫警长";
   //        System.out.println("result----->"+result);
           return result;
       }
   ```
   
   测试类Test
   
   ```java
       @Test
       public void test04(){
           ApplicationContext atx = new ClassPathXmlApplicationContext("applicationContext.xml");
           SomeService proxy = (SomeService) atx.getBean("someService");
           //在调用doAround时，实际上调用的时MyAround方法，所以返回值就是MyAround的返回值
           //res = proxy.MyAround();
           String res = proxy.doAround("around");
           System.out.println("res ------------>"+res);
       }
   ```
   
   ###### @AfterThrowing
   
   异常通知方法的定义格式
   
   1. public
   2. 没有返回值
   3. 方法名称自定义
   4. 方法有一个参数Exception，如果还有那就是JoinPoint
   
   @AfterThrowing的属性
   
   1. value：切入点表达式
   2. throwing：自定义变量，表示目标方法抛出的异常，变量名必须与方法的参数名相同
   
   特点：
   
   1. 在目标方法抛出异常时执行的
   2. 可以做异常的监控程序，监控目标方法的执行是否有异常，如果有可以发送邮件、短信进行通知
   
   实际执行过程：
   
   ```java
   try{
   	SomeServiceImpl.doAfterThrowing(..)
   }catch(Exception ex){
   	MyThrowing(ex)
   }
   ```
   
   目标类方法doAfterThrowing
   
   ```java
      @Override
       public void doAfterThrowing() {
           System.out.println("-------->doAtferThrowing<-------");
           //如果没有异常，异常通知不会执行
           System.out.println("-------->"+10/0+"<-------");
       }
   ```
   
   切面类MyThrowing
   
   ```java
   //异常通知
   @AfterThrowing(value = "execution(* *..SomeServiceImpl.doAfterThrowing(..))",throwing = "ex")
   public void MyThrowing(Exception ex){
       System.out.println("异常通知------------>"+ex.getMessage());
   }
   ```
   
   测试类Test
   
   ```java
    @Test
       public void test05(){
           ApplicationContext atx = new ClassPathXmlApplicationContext("applicationContext.xml");
           SomeService proxy = (SomeService) atx.getBean("someService");
           proxy.doAfterThrowing();
       }
   ```
   
   ###### @After
   
   最终通知
   
   	属性：value 切入点表达式
   	
   	位置：方法的上面
   
   特点：
   
   1. 总是会执行
   
   2. 在目标方法之后执行的
   
      执行过程：
   
      ```java
      try{
      	SomeServiceImpl.doAfter();
      }catch(Exception e){
      
      }finally{
      	myAfter();
      }
      ```
   
      目标类doAfter()
   
      ```java
       @Override
          public void doAfter() {
              System.out.println("-------->doAfter<--------");
          }
      ```
   
      切面类MyAfter()
   
      ```java
       @After("execution(* *..SomeServiceImpl.doAfter(..))")
          public void MyAfter(){
              System.out.println("-------------->最终通知");
          }
      ```
   
      测试类test06
   
      ```java
       @Test
          public void test06(){
              ApplicationContext atx = new ClassPathXmlApplicationContext("applicationContext.xml");
              SomeService proxy = (SomeService) atx.getBean("someService");
              proxy.doAfter();
          }
      ```
   
      ###### @Pointcut
   
      定义管理切入点，如果项目中有多个切入点表达式是重复的，可以复用的，可以使用@Pointcut
   
      	属性：value 切入点表达式
      	
      	位置：在自定义方法的上面
   
      特点：
   
      	当使用@Pointcut定义在一个方法的上面，此时这个方法的名称就是切入点表达式的别名。其他通知中，value属性就可以使用这个方法名称代替切入点表达式了。
      	
      	切面类使用
   
      ```java
       	//最终通知
      	@After("MyPointcut())")
          public void MyAfter1(){
              System.out.println("-------------->最终通知");
          }
      	//前置通知
          @Before("MyPointcut()")
          public void MyAfter2(){
              System.out.println("-------------->前置通知");
          }
      	//由于切入点表达式一样，所以这里可以用@Pointcut来代表切入点表达式
      	//减少了代码的重复
          @Pointcut("execution(* *..SomeServiceImpl.doAfter(..))")
          public void MyPointcut(){
          }
      ```

#### CGLIB动态代理

	CGLIB( Code Generation Libiary )是一个工具库，CGLIB的生成原理是继承（生成目标的子类），在子类中通过重写父类的方法来实现功能的增强和增加。由于是继承实现，所以要求这个类不能是final（因为final不能被继承），而他的方法也不能是final（因为final的方法不能修改）。这种动态代理在Spring、Mybatis中广泛使用。他与JDK的动态代理相比，CGLIB比JDK效率要高一些，速度快一些，而且CGLIB对类的要求低，只要不是不能被继承的都可以用这种方式。
	
	目标类SomeService

```java
public class SomeServiceImpl{
    public void doAfter() {
        System.out.println("-------->doAfter<--------");
    }
}
```

	切面类MyAspect

```java
@Aspect
public class MyAspect {
    @After("MyPointcut()")
    public void MyAfter1(){
        System.out.println("-------------->最终通知");
    }
    @Before("MyPointcut()")
    public void MyAfter2(){
        System.out.println("-------------->前置通知");
    }
    @Pointcut("execution(* org.Cglib.SomeServiceImpl.doAfter(..))")
    public void MyPointcut() {
    }
}
```

测试类TestB

```java
 		//Cglib代理方式，与JDK不同的是不需要实现接口类，可以直接使用实体类型转换
		//少写一个接口类，但是仍然需要在xml文件中，放入spring代理
        @Test
        public void test07(){
            ApplicationContext atx = new ClassPathXmlApplicationContext("applicationContext.xml");
            SomeServiceImpl proxy = (SomeServiceImpl) atx.getBean("ss");
            proxy.doAfter();
            System.out.println(proxy.getClass().getName());//类名中有CGLIB
        }aop:aspectj-autoproxy/
```

	对于有接口的实体类，如果要用cglib的方式代理，需要在< aop:aspectj-autoproxy />的标签后面加上proxy-target-class=true,代表代理时就默认使用CGLIB代理
	
	applicationContext.xml

```xml
... 
<!-- 加上proxy-target-class="true"这个属性代表创建代理默认使用CGLIB方式-->
<aop:aspectj-autoproxy proxy-target-class="true"/>
```

## AOP的使用时机

1. 当需要给系统中存在的类修改功能，但是原有类的功能并不完善，但有源代码，使用AOP就增加功能

2. 当需要给项目中的多个类增加一个相同的功能，使用AOP

3. 给业务方法增加事务，日志输出，也可以使用AOP

   

# Spring与Mybatis集成

把mybatis框架和spring集成在一起，像一个框架一样使用。

技术：ioc

whyIOC ：能把mybatis和spring集成在一起，像一个框架，是因为ioc能创建对象，开发人员就不用同时面对两个或多个框架了，就面对一个spring

**mybatis的使用步骤，对象**

1. 定义dao接口，studentDao
2. 定义mapper文件，StudentDao.xml
3. 定义mybatis的主配置文件
4. 创建dao的代理对象，StudentDao dao = SqlSession.getMapper(StudentDao.class);
5. List<Student> students = dao.selectStudents();

**使用dao对象，需要使用getMapper()方法，使用getMapper()方法的条件**

1. 获取SqlSession对象，需要使用SqlSessionFactory的openSession()方法、
2. 创建SqlSessionFactory对象，通过读取mybatis的主配置文件，能创建SqlSessionFactory对象

总结：需要SqlSessionFactory对象--->使用Factory能获取SqlSession---->通过SqlSession获取dao对象

主配置文件：

1. 数据库信息

   ```xml
     <environment id="mydev">
               <transactionManager type="JDBC"/>
               <dataSource type="POOLED">
                   <!--数据库的驱动类名-->
                   <property name="driver" value="com.mysql.jdbc.Driver"/>
                   <!--连接数据库的url字符串-->
                   <property name="url" value="jdbc:mysql://localhost:3306/test"/>
                   <!--访问数据库的用户名-->
                   <property name="username" value="mcl"/>
                   <!--密码-->
                   <property name="password" value="DNF321"/>
               </dataSource>
           </environment>
   ```

   

2. mapper文件的位置

```xml
<mappers>
        <mapper resource="com/skd/dao/StudentDao.xml"/>
    </mappers>
```

但在这里会使用独立的连接池类替换mybatis默认自带的（POOLED），把连接池类也交给spring创建

Spring需要创建对象

1. 独立的连接池类的对象，使用阿里的druid连接池
2. SqlSessionFactory对象
3. 创建出dao对象

需要学习的就是上面三个对象的创建语法，使用xml的bean标签。

## Mybatis&Spring项目的创建

	步骤：

1. 新建maven项目

2. 加入maven依赖

   - spring依赖

     ```xml
     <!--spring依赖-->
         <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-context</artifactId>
           <version>5.3.3</version>
         </dependency>
     ```

   - mybatis依赖

     ```xml
     <!--mybatis依赖-->
         <dependency>
           <groupId>org.mybatis</groupId>
           <artifactId>mybatis</artifactId>
           <version>3.5.7</version>
         </dependency>
     ```

   - mysql驱动

     ```xml
     <!--mysql驱动-->
         <dependency>
           <groupId>mysql</groupId>
           <artifactId>mysql-connector-java</artifactId>
           <version>8.0.23</version>
         </dependency>
     ```

   - spring的事务依赖

     ```xml
     <!--spring事务依赖-->
         <!-- https://mvnrepository.com/artifact/org.springframework/spring-tx -->
         <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-tx</artifactId>
           <version>5.3.8</version>
         </dependency>
     ```

     

   - mybatis和spring集成的依赖：mybatis官方用的，用在spring项目中创建mybatis的SqlSessionFactory，dao对象

     ```xml
         <!--spring和mybatis的集成依赖-->
         <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis-spring -->
         <dependency>
           <groupId>org.mybatis</groupId>
           <artifactId>mybatis-spring</artifactId>
           <version>2.0.6</version>
         </dependency>
     ```

   - 阿里巴巴的数据库连接池依赖

     ```xml
     <!--阿里公司的数据库连接池-->
         <!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
         <dependency>
           <groupId>com.alibaba</groupId>
           <artifactId>druid</artifactId>
           <version>1.2.6</version>
         </dependency>
     ```

   - 还有一个spring的jdbc依赖，不添加就出错

     ```xml
        <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-jdbc</artifactId>
           <version>5.3.8</version>
         </dependency>
     ```

     

3. 创建实体类

   Student类

   ```java
   public class Student {
       private Integer id;
       private String name;
       private String email;
       private Integer age;
   
       public Student() {
       }
   
       public Student(Integer id, String name, String email, Integer age) {
           this.id = id;
           this.name = name;
           this.email = email;
           this.age = age;
       }
   
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
   
       public String getEmail() {
           return email;
       }
   
       public void setEmail(String email) {
           this.email = email;
       }
   
       public Integer getAge() {
           return age;
       }
       public void setAge(Integer age) {
           this.age = age;
       }
   
       @Override
       public String toString() {
           return "Student{" +
                   "id=" + id +
                   ", name='" + name + '\'' +
                   ", email='" + email + '\'' +
                   ", age=" + age +
                   '}';
       }
   }
   ```

4. 创建dao接口和mapper文件

   dao包下的student类接口StudentDao

   ```java
   import java.util.List;
   
   public interface StudentDao {
       int insertStudent(Student student);
       List<Student> selectStudents();
   }
   
   ```

   与StudentDao对应的mapper

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="org.example.dao.StudentDao">
       <insert id="insertStudent">
           insert into student values(#{id},#{name},#{email},#{age})
       </insert>
       <select id="selectStudents" resultType="org.example.domain.Student">
           select * from test.student order by id desc
       </select>
   </mapper>
   ```

   

5. 创建mybatis主配置文件

   resource资源文件下的mybatis.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
   
       <settings>
           <!--mybatis输出日志-->
           <setting name="logImpl" value="STDOUT_LOGGING"/>
       </settings>
       <!--实体类-->
    <typeAliases>
        <package name="org.example.domain"/>
    </typeAliases>
       <mappers>
           <package name="org.example.dao"/>
       </mappers>
   </configuration>
   ```

   

6. 创建service接口和实现类，属性是dao

   service包下的接口StudentService

   ```java
   public interface StudentService {
       int addStudent(Student student);
       List<Student> querryStudent();
   }
   ```

   service包下的Impl包下的StudentService实现类StudentServiceImpl

   ```java
   public class StudentServiceImpl implements StudentService {
   
       private StudentDao studentDao;
       //使用set注入赋值
       public void setStudentDao(StudentDao studentDao) {
           this.studentDao = studentDao;
       }
       @Override
       public int addStudent(Student student) {
           int nums = studentDao.insertStudent(student);
           return nums;
       }
       @Override
       public List<Student> querryStudent() {
           List<Student> students = studentDao.selectStudents();
           return students;
       }
   }
   ```

7. 创建spring的配置文件：声明mybatis对象交给spring创建

   - 数据源

   - SqlSessionFactory

   - Dao对象

   - 声明自定义的service

     applicationContext.xml：spring配置文件

     ```xml
     <?xml version="1.0" encoding="UTF-8"?>
     <beans xmlns="http://www.springframework.org/schema/beans"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
     
         <!--声明数据源DataSource,用于连接数据库-->
         <bean id="myDataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
             <!--set注入-->
             <property name="url" value="jdbc:mysql://localhost:3306/test?useUnicode=true?characterEncoding=UTF-8"/>
             <property name="username" value="mcl"   />
             <property name="password" value="DNF321"    />
             <property name="maxActive" value="20"   />
         </bean>
     
         <!--声明mybatis中所提供的SqlSessionFactoryBean类，这个类内部创建SqlSession的-->
         <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
             <!--set注入，把数据库连接池付给了dataSource属性-->
             <property name="dataSource" ref="myDataSource"  />
             <!--mybatis主配置文件的位置-->
             <!--这个configLocation的属性是resource，是spring中的，用于读取配置文件
                 他的赋值使用value，指定文件路径，使用classpath：表示文件位置
             -->
             <property name="configLocation" value="classpath:mybatis.xml"/>
         </bean>
         <!--创建dao对象，使用SqlSession的getMapper（StudentDao.class）
             MapperScannerConfigurer:在内部调用getMapper()生成每个dao接口的代理对象
         -->
         <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
             <!--指定SqlSessionFactory对象的id-->
             <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
             <!--指定包名，包名是dao接口所在的包名。
                 MapperScannerConfigurer会扫描这个包的所有接口，把每个接口都执行一个getMapper()方法，
                 得到每个接口的dao对象。创建好的dao对象放入spring容器中，dao对象的默认名称是 接口首字母小写
                 若有多个包，可以用逗号隔开
             -->
             <property name="basePackage" value="org.example.dao"/>
         </bean>
            <!--这里的studentDao就是上面的mapper创建的，默认是首字母小写的对象名-->
         <bean id="studentService" class="org.example.service.Imlp.StudentServiceImpl">
             <property name="studentDao" ref="studentDao"/>
         </bean>
     </beans>
     ```
     
     

8. 创建测试类，获取service对象，通过service调用dao完成对数据库的访问

   两个测试类：

   	test01（插入测试）

   ```java
      @Test
       public void test01(){
           String config = "applicationContext.xml";
           ApplicationContext atx = new ClassPathXmlApplicationContext(config);
           String[] names = atx.getBeanDefinitionNames();
           for(String name:names){
               System.out.println("容器中的对象："+name);
           }
   
   //        StudentDao stuDao = (StudentDao) atx.getBean("studentDao");
           StudentService service = (StudentService) atx.getBean("studentService");
           Student student =new Student();
           student.setId(1006);
           student.setName("古天乐");
           student.setEmail("gutianle@qq.com");
           student.setAge(24);
           int num = service.addStudent(student);
           //Spring和mybatis集成在一起使用，事务是自动提交的。无需执行SqlSession.commit();方法
           System.out.println("num--->"+num);
       }
   ```

   test02（查询测试）

   ```java
     @Test
       public void test02(){
           String config = "applicationContext.xml";
           ApplicationContext atx = new ClassPathXmlApplicationContext(config);
           StudentService service = (StudentService) atx.getBean("studentService");
           List<Student> students = service.querryStudent();
           for (Student stu:students) {
               System.out.println(stu);
           }
       }
   ```

   真正开发中，jdbc的配置（driver、name、password都是写在配置文件中的）

   	jdbc.properties属性文件

   ```properties
   jdbc.url = jdbc:mysql://localhost:3306/test
   jdbc.username = mcl
   jdbc.passwd = DNF321
   jdbc.maxActive = 20
   ```

   applicationContext.xml使用properties文件

   ```xml
    <!--扫描properties文件，必须要有-->
   <context:property-placeholder location="classpath:jdbc.properties"/>
   
       <!--声明数据源DataSource,用于连接数据库-->
       <bean id="myDataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
           <!--set注入-->
           <!--导入properties文件之后，可以通过key值获取value，格式${key}-->
           <property name="url" value="${jdbc.url}"/>
           <property name="username" value="${jdbc.username}"   />
           <property name="password" value="${jdbc.passwd}"    />
           <property name="maxActive" value="${jdbc.maxActive}"   />
       </bean>
   
   ```


# Spring事务

**什么是事务**

	mysql时，提出了事务。事务就是一组sql语句的集合，集合中有多条sql语句，可能是insert、delete、update、select，这些语句应该是都能成功，或都会失败，这些sql语句的执行是一致的，作为一个整体去执行。

**什么时候用事务**

	在操作时，涉及到多个表，或者多个sql语句的insert、update、delete。需要保证这些语句都能成功或者失败，保证操作符合要求。
在java代码中写程序，控制事务，此时的事务应该放在service类的业务方法上，因为业务方法会调用多个dao方法，执行多个sql语句。

**通常使用jdbc和mybatis访问数据库，他们是怎么处理事务的**

	jdbc访问数据库，处理事务：Connection conn ；conn.commit()；conn.rollback();
	mybatis访问数据，处理事务：SqlSession.commit();SqlSession.rollback();
	hibernate访问数据库，处理事务：Session.commit();Session.rollback();

**上述方式对事务的处理方式有什么不足**

不同的数据库访问技术，处理事务的对象，方法都不同，需要掌握不同数据库的访问技术和使用事务的逻辑。

1. 掌握多种数据库中事务的处理逻辑，什么时候提交事务，什么时候回顾事务

2. 处理事务的多种方法。

   总结：多种数据库访问技术，有不同的事务处理机制、对象和方法。

**解决不足的办法**

	spring提供一种处理事务的统一模型，能使用统一的步骤、方式完成多种不同数据库访问技术的事务处理。
		使用spring的事务处理机制，可以完成mybatis访问数据库的事务处理
		使用spring的事务处理机制，可以完成对Hibernate访问数据库的事务处理
		...

**处理事务，需要怎么做，做什么**

	spring处理事务的模型，使用的步骤是固定的，把事务使用的信息提供给spring就可以了。

1. 事务内部的提交、回滚事务，使用的是事务管理对象，代替完成commit和rollback
   事务管理器是一个接口和他的众多实现类。
   接口：PlatformTransactionManager，定义了事务的重要方法commit、rollback
   实现类：spring把每种数据库访问技术对应的事务处理类都写好了。
   			mybatis访问数据库------spring创建好的是：DataSourceTransactionManager
   			hibernate访问数据库------spring创建好的是：HibernateTransactionManager
   怎么使用：告诉spring用的是那种数据库的访问技术
   怎么告诉Spring：声明数据库访问计数对于事务管理器的实现类，在spring的配置文件中使用<bean>声明就可以了。
   例：<bean id = "xxx"  class="...DataSourceTransactionManager" />,就是mybatis的事务处理类

2. 说明需要事务的类型

   五个事务隔离级别常量：这些常量均以ISOLATION_开头，形如IOSLATION_XXX

   - DEFAULT：采用DB默认的事务隔离级别。MySql的默认是REPEATABLE_READ；Oracle默认的是READ_COMMITED
   - READ_UNCOMMITED：读未提交。未解决仍和并发问题。
   - READ_COMMITED：读已提交。解决脏读，存在不可重复读与幻读。
   - REPEATABLE_READ：可重复读。解决脏读、不可重复读，存在幻读。
   - SERIALIZABLE：串行化。不存在并发问题。

   事务的超时时间：表示一个方法最长的执行时间，如果方法的执行时间超过了这个时间，事务就回滚。单位是秒，整数值，默认是-1

   事务的传播行为：控制也是方法是不是有事务的，是什么样的是事务。

   		有7种传播行为，表示业务调用时，事务在方法之间是如何使用的。

   - **PROPAGATION_REQUIRED**
     	这个事务时spring默认使用的，在一个方法调用另一个方法时，若是被调用的方法有自己的事务，则他子类的事务也会加入到这个父类的事务中。但若是父类没有事务，那么子类就会单独创建一个事务，用于自己的事务处理。
   - **PROPAGATION_REQUIRED_NEW**
         总是新建一个事务，若当前存在事务，就将当前事务挂起，直到新事务执行完毕
   - **PROPAGATION_SUPPORTS**  
         这个行为是指，指定的方法支持事务，但若是没有事务，也可以非事务执行
   - PROPAGATION_MANDATORY
   - PROPAGATION_NESTED
   - PROPAGATION_NEVER
   - PROPAGATION_NOT_SUPPORTED

3. spring提交事务，回滚事务的时机

   - 当业务方法执行成功，没有异常抛出，当方法执行完毕，spring在方法执行后提交事务。commit
   - 当业务方法抛出异常或ERROR，spring执行回滚，调用事务处理器rollback
     运行时异常的定义：RuntimeException和他的子类都是运行时异常，例如：NullPointException、NumberFormatException等
   - 当业务方法抛出非运行时异常，主要是受查异常时，提交事务commit
     受查异常：在写代码时，必须处理的异常。例如：IOException，SQLException

   总结Spring业务：

   1. 管理事务的是 事务管理器

   2. spring的事务是一个统一模型

      - 指定要使用的事务管理器实现类，使用<bean>

      - 指定哪些类，哪些方法需要加入事务的功能

      - 指定方法需要的隔离级别，传播行为、超时

        程序员需要告诉spring，你的项目中类信息，方法的名称，方法的事务传播行为

事务的例子：

数据库数据：

goods商品表

```sql
create table test.goods
(
    id     int          not null
        primary key,
    name   varchar(100) null,
    amount int          null,
    price  float        null
);
```

sales销售表

```sql
create table test.sales
(
    id   int auto_increment
        primary key,
    gid  int not null,
    nums int null
);
```

goods商品信息

```sql
insert into goods values (1001,'笔记本',10,15),
                         (1002,'手机',20,3000)
```

GoodsDao

```java
public interface GoodsDao {
    //更新库存，goods表示本次用户购买的商品信息，id，购买数量
    int updateGoods(Goods goods);
    Goods selectGoods(Integer id);
}	
```

GoodsDAO.xml

```xml
<mapper namespace="com.transction.dao.GoodsDao">
    <select id="selectGoods" resultType="com.transction.domain.Goods">
        select id,name,amount,price from goods where id = #{id}
    </select>
    <update id="updateGoods">
        update goods set amount = amount - #{amount} where id=#{id}
    </update>
</mapper>
```

SaleDao

```java
public interface SaleDao {
    //增加销售记录
    int insertSale(Sale sale);
}
```

SaleDao.xml

```xml
<mapper namespace="com.transction.dao.SaleDao">
<insert id="insertSale">
    insert into sales(gid,nums) value (#{gid},#{nums})
</insert>
</mapper>
```

Goods

```java
public class Goods {
    private Integer id;
    private String name;
    private Integer amount;
    private Float prica;
    //get/set方法
    }
```

Sale

```java
public class Sale {
    private Integer id;
    private Integer gid;
    private Integer nums;
    //get/set方法
    }
```

一个自定义NotEnoughException

```java
public class NotEnoughException extends RuntimeException{
    public NotEnoughException() {
        super();
    }
    public NotEnoughException(String message) {
        super(message);
    }
}
```

service包

BugGoodsService

```java
public interface BugGoodsService {
    //购买商品的方法
    void buy(Integer goodsId,Integer nums);
}
```

BugGoodsServiceImpl

```java
public class BugGoodsServiceImpl implements BugGoodsService {
    private SaleDao saleDao;
    private GoodsDao goodsDao;
    public void setSaleDao(SaleDao saleDao) {
        this.saleDao = saleDao;
    }
    public void setGoodsDao(GoodsDao goodsDao) {
        this.goodsDao = goodsDao;
    }
    @Override
    public void buy(Integer goodsId, Integer nums) {
        System.out.println("==================buy方法开始====================");
        //记录销售信息，向销售表添加记录
        Sale sale = new Sale();
        sale.setGid(goodsId);
        sale.setNums(nums);
        saleDao.insertSale(sale);
        //更新库存
        Goods goods = goodsDao.selectGoods(goodsId);
        if(goods == null){
            throw new NotEnoughException("商品编号为->"+goodsId+"的商品不存在");
        }else if(goods.getAmount()<nums){
            throw new NotEnoughException("商品编号为->"+goodsId+"的商品数量不足");
        }
        Goods buyGood = new Goods();
        buyGood.setId(goodsId);
        buyGood.setAmount(nums);
        goodsDao.updateGoods(buyGood);
        System.out.println("==================buy方法结束====================");
    }
```

测试类

```java
    @Test
    public void test01(){
        String config = "applicationContext.xml";
        ApplicationContext atx = new ClassPathXmlApplicationContext(config);
        BugGoodsServiceImpl service = (BugGoodsServiceImpl) atx.getBean("buyService");
        service.buy(1001,10);
    }
```

## 使用Spring的事务注解管理事务

	通过@Transactional注解方式，可将事务置入相应的public方法中，实现事务管理

### @Transactional

	属性

1. propagation：用于设置事务传播属性，该属性类型为Propagation枚举，默认值为Propagation.REQUIRED
2. isolation：用于设置事务的隔离级别，这个属性类型为Ioslation枚举，默认值为Isolation.DEFAULT
3. readOnly：用于设置该方法对数据库的操作是否是只读的。该属性为boolean类型，默认为false，一般当数据库只有查询操作时会用
4. timeout：用于设置本操作与数据库连接的超时时限，单位为秒，类型为int，默认值-1，既没有时限，这个属性设置有很多因素，一般不设置
5. rollbackFor：指定需要回滚的异常类，类型为class[]，默认值为空数组，若只有一个异常类，可以不使用数组
6. rollbackForClassName：指定需要回滚的异常类类名，类型为String[]，默认值为空数组，只有一个异常类也可以不使用数组
7. noRollbackFor：指定不需要的回滚的异常类。类型为class[]，默认为空数组，只有一个异常类可以不使用数组
8. noRollbackForClassName：指定不需要回滚的异常类类名，类型为String[]，默认值为数组，只有一个异常类可以不使用数组

使用@Transactional的步骤

1. 需要声明事务管理对象

   <bean id="xx" class="DataSourceTransactionManager">

   在applicationContext配置文件中加入

   ```xml
       <!--使用spring的事务处理-->
       <!--声明事务管理器-->
       <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
           <!--连接的数据库，指定数据源-->
           <property name="dataSource" ref="myDataSource"></property>
       </bean>
       <!--开启事务注解驱动，告诉spring使用注解管理事务，创建代理对象
           transaction-manager:事务管理器id
       -->
       <tx:annotation-driven transaction-manager="transactionManager"/><!--这就是第二步-->
   ```

2. 开启事务注解驱动，告诉Spring框架，要使用注解的方式管理事务

   spring使用aop机制，创建@Transactional所在的类代理对象，给方法加入事务的功能，

   spring在业务方法中加入事务：

   	在业务方法执行之前，先开启事务，在业务方法之后提交或者回滚事务，使用aop环绕通知
	
   	@Around（”要增加的事务功能的业务方法名称“）
	
   	Object myAround( ){
	
   	开启事务，spring开启
	
   	try{
	
   	buy(1001,10);
	
   	spring事务管理器.commit();
	
   	}catch(Exception e){
	
   	spring事务管理器.rollback();
	
   	}

   }

3. 在方法上面加入@Trancational

   即在BuyGoodsServiceImpl下的buy方法上面加入以下注解

```java
//    @Transactional若是以下默认值的话，可以只写一个注释，不加括号内的内容
    @Transactional(propagation = Propagation.REQUIRED,
            	  isolation = Isolation.DEFAULT,
            	  readOnly = false,
            	  rollbackFor = {NullPointerException.class , NotEnoughException.class})
/**
rallbackFor:表示发生指定的异常一定回滚。
	处理逻辑：
		1.spring框架会首先检查方法抛出的异常是不是在rollbackFor的属性中，如果异常在rollbackFor列表中，
			不管是什么异常都会回滚
		2.如果抛出的异常不在rollbackFor列表中，spring会判断异常是不是RuntimeException，如果是一定回滚
*/
```

## 使用aspectJ的AOP配置事务管理

	适合大型项目，有很多类，方法，需要大量的配置事务，使用aspectj框架功能，在spring配置文件中声明类，方法需要的事务。这种方式业务方法和事务配置完全分离

applicationContext.xml配置添加

```xml
    <!--声明式业务处理，与源码完全分离的-->
    <!--声明事务管理器对象-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="myDataSource"/>
    </bean> 
<!--声明业务方法的事务属性（隔离级别，传播行为，超时时间）
        id:自定义名称，表示<tx:advice></tx:advice>之间的内容
        transaction-manager:事务管理器对象的id
    -->
    <tx:advice id="myAdvice" transaction-manager="transactionManager">
        <!--tx:attributes:配置事务属性-->
        <tx:attributes>
            <!--tx:method:给具体的方法配置事务属性，method可以有多个，分别给不同的方法设置事务属性
                name：方法名称 1）完整的方法名，不带有包和类
                             2）方法可以使用通配符，*表示任意字符
                propagation：传播行为，枚举值
                isolation：隔离级别
                rollback-for：指定的异常类名，全限定类名，发生异常时一定回滚
            -->
            <tx:method name="buy" propagation="REQUIRED" isolation="DEFAULT"
                       rollback-for="java.lang.NullPointerException,com.transaction.exec.NotEnoughException"/>
            <!--使用通配符，指定很多方法-->
            <tx:method name="add*" propagation="REQUIRES_NEW"/>
            <!--指定修改方法-->
            <tx:method name="modify*"/>
            <!--删除方法-->
            <tx:method name="remove*"/>
            <!--查询方法-->
            <tx:method name="*" propagation="SUPPORTS" read-only="true"/>
            <!--spring找方法是先找完整的，再找有通配符的，最后再找只有通配符的-->
        </tx:attributes>
    </tx:advice>

    <!--对于有多个包，多种方法,需要指定包的情况下，还需要配置aop的切入点-->
    <aop:config>
        <!--配置切入点表达式：指定那些包中类要使用事务
            id：切入点表达式的名称，唯一值
            expression：切入点表达式，指定那些类需要使用事务，aspectj会创建代理对象

            例：
            com.transaction.service
            com.cm.service
            com.mcl.service
            ...
        -->
        <!--所有的service包下的所有方法-->
        <aop:pointcut id="servicePT" expression="execution(* *..service..*.*(..))"/>

        <!--配置增强其：关联advice和pointcut
            advice-ref：通知，与上面的tx：advice的id对应
            pointcut-ref：切入点表达式的id
        -->
        <!--即，在servicePT中的切入点表达式所表示的包下的所有方法，与之前myAdvice的事务通知名称符合的，才能执行通知-->
        <aop:advisor advice-ref="myAdvice" pointcut-ref="servicePT"/>
    </aop:config>
```

# SpringMVC

 web项目怎么使用容器对象		项目：ch05-spring-web（未实现）

1. 做的是javase项目中有main方法的，执行代码是执行main方法，在main里面创建容器对象

   ApplicationContext atx = new ClassPathXmlApplicationContext(config);

2. web项目是在tomcat服务器上运行的，tomcat一启动项目一直运行

需求：web项目中容器对象只创建一次，把容器对象放入全局作用域ServletContext中

怎么实现：使用监听器，当全局作用域对象被创建时，创建容器，存入ServletContext

监听器作用：

1. 	创建容器，执行ApplicationContext atx = new ClassPathXmlApplicationContext(config);
2.     把容器对象放入ServletContext,ServletContext.setAttribute(key,ctx);

监听器可以自己创建，也可以使用框架中提供的ContextLoaderListener

ApplicationContext : javase项目中的容器对象

WebApplicationContext : web项目中使用的容器对象

例：servlet类

```java
 @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String stuId=req.getParameter("id");
        String stuName=req.getParameter("name");
        String stuEmail=req.getParameter("email");
        String stuAge=req.getParameter("age");



        //加入监听器之前
        //这是方式每次访问网页都会创建新的容器，所以就有了监听器的需求
        //监听器的目的是创建容器对象，创建了容器对象，就能把spring的配置文件中的所有对象创建好，用户发起请求就可以直接适用对象了
/*        String config="applicationContext.xml";
        ApplicationContext atx = new ClassPathXmlApplicationContext(config);*/
        //加入监听器之后
        WebApplicationContext ctx = null;
        //以下是自己写的方式
        /*
        String key = WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE;//固定值
        Object attr = getServletContext().getAttribute(key);
        if(attr!= null){
            ctx=(WebApplicationContext) attr;
        }*/
        //使用框架中的方法
        ServletContext sc = getServletContext();
        ctx = WebApplicationContextUtils.getRequiredWebApplicationContext(sc);

        System.out.println("===================容器对象信息================");

        StudentService service = (StudentService) ctx.getBean("studentService");
        Student student = new Student();
        student.setId(Integer.parseInt(stuId));
        student.setName(stuName);
        student.setAge(Integer.parseInt(stuAge));
        student.setEmail(stuEmail);
        service.addStudent(student);

        req.getRequestDispatcher("/result.jsp").forward(req,resp);
    }
```

表单index.jsp

```jsp
 <p>注册学生</p>
    <form action="/reg" method="post">
        <table>
            <tr>
                <td>id:</td>
                <td><input type="text" name="id"></td>
            </tr>
            <tr>
                <td>姓名：</td>
                <td><input type="text" name="name"></td>
            </tr>
            <tr>
                <td>email:</td>
                <td><input type="text" name="email"></td>
            </tr>
            <tr>
                <td>年龄：</td>
                <td><input type="text" name="age"></td>
            </tr>
            <tr>
                <td></td>
                <td><input type="submit" value="注册学生"></td>
            </tr>
        </table>

    </form>
```