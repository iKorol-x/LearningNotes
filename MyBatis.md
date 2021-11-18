# MyBatis

## 框架概述

### 	软件开发的常用架构

#### 	三层架构

​		界面层（User Interface layer）：视图层，主要功能是接收用户的数据，显示请求的处理结果，使用web页面和用户交互，手机app也是表示层，用户在app中操作，业务逻辑在服务端处理（例：jsp、html、servlet）

​		业务逻辑层（Business Logic Layer）：接受表示传递过来的数据，检查数据，计算业务逻辑，调用数据访问层获取数据

​		数据访问层（Data Access Layer）：与数据库打交道，主要是实现对数据库的增删改查，将数据库中的数据提交给业务层，同时将业务层的数据保存到数据库

​	对应的包：

​		界面层：controller包（servlet）

​		业务逻辑层：service包（XXservlet类）

​		数据访问层：dao包（XXDao）

三层中类的交互

​	用户使用的界面-->业务逻辑层-->数据访问层（持久层）-->数据库（mysql）

三层对应的处理框架

​	界面层--->servlet---->springmvc

​	业务逻辑层--->service类--->spring

​	数据访问层--->dao类--->mybatis

### MyBatis框架

​	Mybatis是Mybatis SQL Mapper Framework for Java（sql映射框架）

1. sql mapper：sql映射，可以把数据库表中的一行数据，映射为一个java对象，一行数据可以看作是一个java对象，操作这个对象，就相当于操作数据库
2. Data Access Objects（DAOs）：数据访问，对数据库执行增删改查

### Mybatis提供了哪些功能

1. 提供了创建Connectuin、Statement、ResultSet的能力，不需要开发人员创建这些对象了。

2. 提供了执行sql语句的能力，不用个人执行sql

3. 提供了循环sql，把sql的结果转为java对象，List集合的能力

4. 提供了关闭资源的能力，不用手动关闭Connection，Statement，ResultSet

   开发人员只需要提供sql语句

   开发人员提供sql语句 -->mybatis处理sql-->开发人员的到List集合或者java对象（表中的数据）

   mybatis是一个sql映射，提供数据库的操作能力，增强的JDBC，使用mybatis让开发人员集中写sql就行，不必关心Connection，Statement，ResultSet的开启关闭以及sql的执行。
   
   以此表为例

```sql
create table student
(
   id int not null,
   name varchar(80) null,
   email varchar(100) null,
   age int null,
   constraint student_pk
      primary key (id)
);
```

实现步骤：

1. 新建Student表

2. 加入maven的batis坐标，mysql驱动的坐标

   ```xml
     <!--mysql驱动-->
       <dependency>
         <groupId>mysql</groupId>
         <artifactId>mysql-connector-java</artifactId>
         <version>8.0.23</version>
       </dependency>
       <!--mybatis框架-->
       <dependency>
         <groupId>org.mybatis</groupId>
         <artifactId>mybatis</artifactId>
         <version>3.5.7</version>
       </dependency>
   ```

   

3. 创建实体类，student--保存表中的一行数据的

4. 创建持久层的dao接口，定义操作数据库的方法

5. 创建一个mybatis使用的配置文件，叫做sql映射文件：写sql语句的一般一个表一个sql映射文件。这个文件是xml文件

   对StudentDao的xml配置文件（放在Dao目录下，与接口类同一个目录下）

   ```xml
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.skd.dao.StudentDao">
       <!--
      sql映射文件：写sql语句的，mybatis会执行这些sql
      1.指定约束文件
      <!DOCTYPE mapper
              PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
              "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
        mybatis-3-mapper.dtd是约束文件的名称，扩展名是dtd的
      2.约束文件的作用：限制，检查在当前文件中出现的标签，属性必须符合mybatis的要求。
      3.mapper是当前文件的根标签，必须的、
              namespace：叫做命名空间，唯一值，可以是自定义的标签，
                          但这个标签必须是dao接口的全限定名称
      4.在当前文件中，可以使用特定的标签，表示数据库的特定操作。
          <select>:表示执行查询
          <update>:表示执行更新数据库操作,就是在<update>标签中，写的是update sql语句
          <insert>:表示插入语句，放的是insert语句
          <delete>:表示删除，执行的是delete语句
      -->
       <!--
       select
           id:要执行的sql语法的唯一标识，mybatis会使用这个id的值来找到要执行的sql语句
               可以自定义但是要求使用接口中的方法名称。
           resultType:表示结果类型，是sql语句执行后得到的ResultSet，
               遍历这个ResultSet得到的java对象的类型。
               值写类型的全限定名称
       -->
       <select id="selectStudents" resultType="com.skd.domain.Student">
           select * from test.student order by id
       </select>
   </mapper>
   ```

6. 创建mybatis的主配置文件：

   一个项目就一个主配置文件。主配置文件提供了数据库的连接信息和sql映射的位置信息

   连接数据库的xml文件（放在resource文件中，名字可以自定义）

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
       <!--
           环境配置：数据库的连接信息
           default:必须和某个environment的id值一样
           告诉mybatis使用那个数据库的连接信息，也就是访问那个数据库
       -->
       <environments default="mydev">
           <!--
               environment:一个数据库信息的配置，环境
               id是唯一值，自定义表示环境的名称
           -->
           <environment id="mydev">
               <!--
                   transactionManager：mybatis的事务类型
                       type：JDBC（表示使用jdbc中的Connection对象的commit，rollback做事务处理）
               -->
               <transactionManager type="JDBC"/>
   
               <!--
                   dataSource：表示数据源，连接数据库的
                       type：表示数据源的类型，POOLED表示使用连接池
               -->
               <dataSource type="POOLED">
                   <!--
                       driver、url、username、password这些是固定的，不能自定义
                   -->
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
       </environments>
       <mappers>
           <!--指向mapper文件-->
           <mapper resource="org/mybatis/example/BlogMapper.xml"/>
       </mappers>
   </configuration>
   <!--
           mybatis的主配置文件：主要定义了数据库的配置信息，sql映射文件的位置
               1.约束文件
                   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
           mybatis-3-config.dtd：约束文件的名称
           
               2.configuration：根标签
   -->
   ```

   

7. 创建使用mybatis对象SqlSession，通过他的方法访问数据库。

#### Select操作

​	结构如下

![image-20210619202036601](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210619202036601.png)

​	dao包下的StudentDao接口

```java
public interface StudentDao {
    public List<Student> selectStudents();
}	
```

​	dao包下的xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.skd.dao.StudentDao">
    <!--
   sql映射文件：写sql语句的，mybatis会执行这些sql
   1.指定约束文件
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
     mybatis-3-mapper.dtd是约束文件的名称，扩展名是dtd的
   2.约束文件的作用：限制，检查在当前文件中出现的标签，属性必须符合mybatis的要求。
   3.mapper是当前文件的根标签，必须的、
           namespace：叫做命名空间，唯一值，可以是自定义的标签，
                       但这个标签必须是dao接口的全限定名称
   4.在当前文件中，可以使用特定的标签，表示数据库的特定操作。
       <select>:表示执行查询
       <update>:表示执行更新数据库操作,就是在<update>标签中，写的是update sql语句
       <insert>:表示插入语句，放的是insert语句
       <delete>:表示删除，执行的是delete语句
   -->
    <!--
    select
        id:要执行的sql语法的唯一标识，mybatis会使用这个id的值来找到要执行的sql语句
            可以自定义但是要求使用接口中的方法名称。
        resultType:表示结果类型，是sql语句执行后得到的ResultSet，
            遍历这个ResultSet得到的java对象的类型。
            值写类型的全限定名称
    -->
    <select id="selectStudents" resultType="com.skd.domain.Student">
        select * from test.student order by id
    </select>
</mapper>
```

​	domain下的Student类

```java
package com.skd.domain;

public class Student {
    private Integer id;
    private String name;
    private String email;
    private Integer age;
    public Integer getId() {return id;}
    public void setId(Integer id) {this.id = id;}
    public String getName() {return name;}
    public void setName(String name) {this.name = name;}
    public String getEmail() {return email;}
    public void setEmail(String email) {this.email = email;}
    public Integer getAge() {return age;}
    public void setAge(Integer age) {this.age = age;}
    @Override
    public String toString() {
        return "Student{" +"id=" + id +", name='" + name + '\'' +
                ", email='" + email + '\'' +
                ", age=" + age +'}';
    }
}
```

​	resource下的mybatis主配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!--
        环境配置：数据库的连接信息
        default:必须和某个environment的id值一样
        告诉mybatis使用那个数据库的连接信息，也就是访问那个数据库
    -->
    <environments default="mydev">
        <!--
            environment:一个数据库信息的配置，环境
            id是唯一值，自定义表示环境的名称
        -->
        <environment id="mydev">
            <!--
                transactionManager：mybatis的事务类型
                    type：JDBC（表示使用jdbc中的Connection对象的commit，rollback做事务处理）
            -->
            <transactionManager type="JDBC"/>
            <!--
                dataSource：表示数据源，连接数据库的
                    type：表示数据源的类型，POOLED表示使用连接池
            -->
            <dataSource type="POOLED">
                <!--
                    driver、url、username、password这些是固定的，不能自定义
                -->
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
    </environments>
    <!--
      sql mapper(sql映射文件的位置)
      -->
    <mappers>
        <mapper resource="com/skd/dao/StudentDao.xml"/>
    </mappers>
</configuration>
<!--
        mybatis的主配置文件：主要定义了数据库的配置信息，sql映射文件的位置
            1.约束文件
                <!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
        mybatis-3-config.dtd：约束文件的名称

            2.configuration：根标签
-->
```

​	测试类Test

```JAVA
    @Test
    public void shouldAnswerWithTrue() throws IOException {
        //访问mybatis读取student数据
        //1.定义mybatis主配置文件的名称，从类路径开始
        String config ="mybatis.xml";
        //2.读取这个config表示的文件
        InputStream in = Resources.getResourceAsStream(config);
        //3.创建了SqlSessionFactoryBuilder对象
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        //4.创建SqlSessionFactory对象
        SqlSessionFactory factory = builder.build(in);
        //5.获取SqlSession对象，从SqlSessionFactory中获取SqlSession
        SqlSession sqlSession = factory.openSession();
        //6.指定要执行的Sql语句的标识。sql映射文件中的namespace+"."+标签的id值
        String sqlID = "com.skd.dao.StudentDao"+"."+"selectStudents";
        //7.执行sql语句，通过sqlID找到语句
        List<Student> studentList = sqlSession.selectList(sqlID);
        //8.输出结果
        studentList.forEach(student -> System.out.println(student));
        //9.关闭SqlSession对象
        sqlSession.close();
    }

//对于update，delete，insert操作，mybatis在执行操作之后不会提交事务，需要手动提交事务才会对数据库执行操作。
//即在执行完sql语句之后，执行 sqlSession.commit();
```

#### 日志

```xml
    <settings><!--settings是控制mybatis的全局行为 -->
        <setting name="logImpl" value="STDOUT_LOGGING"/><!--作用就是会在控制台输出对数
			据库操作的信息，mybatis的输出日志行为-->
    </settings>
```

## 主要类的介绍

主要类的介绍：

1. Resources：mybatis中的一个类，负责读取主配置文件

   ```java
   InputStream in=Resources.getResourceAsStream(config);
   ```

   

2. SqlSessionFactoryBuilder：创建SqlSessionFactory对象

   ```java
    SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
           //4.创建SqlSessionFactory对象
           SqlSessionFactory factory = builder.build(in);
   ```

3. SqlSessionFactory重量级对象，程序创建一个对象的时间比较长，使用资源比较多，在整个项目中一个就够用了

   SqlSessionFactory接口：接口实现类DefaultSqlSessionFactory

   SqlSessionFactory作用：获取SqlSession对象SqlSession sqlsession=factory.openSession()；

   openSession方法说明：

   - openSession()：无参数，获取的是非自动提交事务的SqlSession

   - openSession(boolean)：openSession(true) 获取自动提交事务的SqlSession

     ​										openSession(false)获取非自动提交事务的SqlSession

4. SqlSession

   SqlSession接口：定义了操作数据的方法，例如selectOne()，selectList()，insert()，update()，delete()，commit()，rollback()

   SqlSession的接口实现类DefaultSqlSession。

   使用要求：SqlSession对象不是线程安全的，需要在方法内部使用，在执行sql语句之前，使用openSession()获取SqlSession对象，在执行完sql语句后，需要关闭他，执行SqlSession.close()，这样能确保线程是安全的。

```java
List<Student> studentList = sqlSession.selectList(sqlID);
//8.输出结果
studentList.forEach(student -> System.out.println(student));
//9.关闭SqlSession对象
sqlSession.close();
```

由于最终目的是创建SqlSession，所以可以写一个工具类来避免重复的代码。

MyBatisUtils.java工具类

```java
public class MyBatisutils {
    private static SqlSessionFactory factory=null;
    //用静态代码块，只需要扫描一次mybatis的配置文件
    static{
        String config = "mybatis.xml";
        try {
            InputStream is = Resources.getResourceAsStream(config);
            //创建SqlSessionFactory对象工厂
            factory =new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    //获取SqlSession对象
    public static SqlSession getSqlSession(){
        SqlSession sqlSession =  null;
        if(factory!=null){
            sqlSession=factory.openSession();
        }
        return sqlSession;
    }
}
```

Test

```java
SqlSession sqlSession = MyBatisutils.getSqlSession();
//6.指定要执行的Sql语句的标识。sql映射文件中的namespace+"."+标签的id值
String sqlID = "com.skd.dao.StudentDao"+"."+"selectStudents";
//7.执行sql语句，通过sqlID找到语句
List<Student> studentList = sqlSession.selectList(sqlID);
//8.输出结果
studentList.forEach(student -> System.out.println(student));
//9.关闭SqlSession对象
sqlSession.close();
```

这样就可以简化很多代码
