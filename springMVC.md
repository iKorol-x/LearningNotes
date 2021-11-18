# **SpringMVC**

## 概述

springMVC是基于spring的一个框架，实际上基于是spring的一个模块，专门是做web开发的，理解是servlet的一个升级

web开发底层是servlet，框架是在servlet基础上面的加入一些功能

springMVC就是一个Spring，Spring是容器，ioc能够管理对象，使用<bean>，@Conponent，@Repository，@Service，@Controller

SpringMVC能够创建对象放到容器（SpringMVC容器）中，springmvc容器中放的是控制器对象。

我们需要做的是，使用@Controller创建控制器对象，把对象放入到springmvc容器中，把创建的对象作为控制器使用，这个控制器对象能接受用户的请求，显示处理结果，可以当作一个servlet来用。

使用@Controller注解创建的是一个普通类的对象，不是servlet，只是springMVC赋予了控制器对象一些额外的功能

web开发底层是servlet，springmvc中有一个对象是servlet：DispatherServlet（中央调度器）

DispatherServlet：负责接收用户的所有请求，用户把请求给了DispatherServlet，之后DispatherServlet把请求转发给Controller对象，最后是Controller对象处理了请求

## 初试

在web.xml中注册springmvc框架的核心都西昂DispatcherServlet

1. DispatcherServlet教做中央调度器，是一个servlet，他的父类是集成HttpServlet的
2. DispatcherServlet也叫做前端控制器（front controller）
3. DispatcherServlet负责接受用户提交的请求，调用其他的控制器对象，并把请求的主力结果显示给用户。

依赖pom.xml

```xml
 <!--servlet依赖-->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>servlet-api</artifactId>
      <version>2.5</version>
    </dependency>
    <!--springmvc的依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
```

web.xml的配置（先版本更新一下）

```xml
<!--首先要注册springmvc的核心对象DispatcherServlet
    要求是在tomcat服务器启动后，创建DispatcherServlet对象的实例
    为什么要创建DispatcherServlet实例呢？
    因为在创建DispatcherServlet实例的过程中，会执行他的init方法，而他的init方法会同时创建
    springmvc容器的对象，读取springmvc配置文件，把这个配置文件中的对象都创建好，当用户发起
    请求时就可以直接使用对象了
    DispatcherServlet的init方法会执行：
    Application atx = new ClassPathXmlApplicationContext("springmvc.xml");//创建容器
    getServletContext().setAttribute(key,ctx);//然后让如到servletContext中。
-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

<!--        由于直接执行的画，默认寻找的springmvc配置文件是在web-inf目录下的，不灵活，所以需要自定义路径-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>

        <!--表示在tomcat启动时进行加载，创建servlet对象-->
<!-- load-on-startup>=0       -->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
<!--
            使用框架时，url-pattern可以使用两种值
            1.使用扩展名，语法*.xxx,xxx是自定义的扩展名，常用的方式*.do ,*.action ,*.mvc等
                http://localhost:8080/myweb/some.do
                http://localhost:8080/myweb/other.do
                表示凡是带着个后缀名的请求都交给这个servlet处理
            2.使用斜杠“/”
-->
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>
```

Controller.java

```java
@Controller
public class MyController {
    /**
     * springmvc中使用方法来处理请求
     * 使用dosome方法来处理some.do请求
     * @RequestMapping：请求映射，作用是将一个请求地址和一个方法绑定在一起
     *                  一个请求指定一个处理方法
     *           属性：1.value，字符串，表示uri地址（some.do）
     *                  value的值必须是唯一的，不能重复，使用时推荐以“/”开头
     *           位置：1.在方法的上面，常用
     *                  2.在类的上面
     * @RequestMapping修饰的方法叫做处理器方法或者控制器方法，使用@RequestMapping修饰的方法
     * 可以处理请求，类似servlet中的doGet，doPost方法。
     *
     * 返回值：ModelAndView表示本次请求的处理结果
     * model：数据，请求处理完成后要显示给用户的数据
     * view：视图，jsp等
     */
    @RequestMapping(value = "/some",method = RequestMethod.GET)
    public ModelAndView doSome(){
        //处理请求，相当于service方法
        System.out.println("1111");
        ModelAndView mv = new ModelAndView();
        //添加数据，框架在请求的最后把数据放入request作用域
        //相当于request的setAttribute方法存入数据
        mv.addObject("test01","welcome!");
        mv.addObject("test02","mcl");
        //指定视图，指定视图的完整路径
        //框架对视图执行的是重定向操作
        mv.setViewName("show");

        //返回
        return mv;
    }
 }
```

视图放入web-inf中

![image-20210709151423430](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210709151423430.png)

SpringMVC请求处理的流程：

1. 发起some.do请求
2. tomcat（扫描web.xml，根据url-pattern知道*.do的请求给DispatcherServlet）
3. DispatcherServlet（根据springmvc.xml配置知道some.do对应的dosome方法）
4. DispatcherServlet把some.do转发给MyController.doSome（）方法
5. 框架执行doSome，将得到的ModelAndView进行处理，转发到show.jsp

some.do-->DispatcherServlet--->MyController

springMVC源码分析

1. tomcat启动，创建容器的过程
   通过load-on=start标签指定的1，创建DispatcherServlet对象，DisapatcherServlet他的父类是集成HttpServlet的，它是一个Servlet，在被创建时会执行init()方阿飞，在init（）方法中会创建容器，并将容器放入ServletContext中。
   创建这个容器的作用是，创建@controller注解所在的类的对象，创建MyController对象，这个对象放入到SpringMVC的容器中，容器时map，leisimao.put()
2. 请求的处理过程
   - 执行servlet的service（）
     protected void service();
     protected void doService();
     this.doDispatch(req,resp);这个方法会调用myController的方法

## 注解式开发

### @RequestMapper

​	请求映射

​	属性：method，表示请求的方式，他是RequestMethod类枚举值

​			例如：表示get请求的方式：RequestMethod.GET

​					  表示post请求方式：RequestMethod.POST

​	不用对应的请求方式访问，则出现405错误（请求方式出错）

### 处理器方法的参数

接受请求参数，使用处理器方法的形参

- HttpServletRequest
- HttpServletResponse
- HttpSession
- 用户提交的数据

接收用户提交的参数

1. #### 逐个接收	

请求中所携带的参数，放在方法的形参里面，框架会自动为他添加信息，就当作servlet用。

```java
 /**
     *
     *  @RequestMapper：请求映射
     * 属性：method，表示请求的方式，他是RequestMethod类枚举值
     * 			例如：表示get请求的方式：RequestMethod.GET
     * 				  表示post请求方式：RequestMethod.POST
     * 	不用对应的请求方式访问，则出现405错误（请求方式出错）
     */
    /**
     * 逐个请求参数：
     *  要求：处理器（控制器）方法的形参名必须和请求中的参数名相同
     *  框架接收请求参数：
     *      1.使用request对象接受请求的参数
     *          String name = request.getParameter("name");
     *          String age = request.getParameter("age");
     *      2.springmvc框架通过DispatcherServlet调用MyController的doSome()方法
     *          调用方法时，按名称对应，把接收到的参数赋值给形参
     *          receive(strName,Integer.valueOf(strAge))
     *          框架会提供类型转换功能，能把String转化为int,long,float,double等类型
     * */
    @RequestMapping(value = "/receive01.do",method = RequestMethod.POST)
    public ModelAndView receive01(String name,int age){
        ModelAndView mv = new ModelAndView();
        mv.addObject("myname",name);
        mv.addObject("myage",age);
        mv.setViewName("show");
        return mv;
    }
```



```xml
<form method="post" action="/test/receive01.do">
    name:<input type="text" name="name">
    age:<input type="text" name="age">
    <input type="submit">
</form>
```

注意：在提交参数时，get请求方式中文没有乱码

​	使用post提交请求时，中文有乱码，需要用过滤器处理乱码问题

​	过滤器可以自定义，也可以使用框架中提供的过滤器CharacterEncodingFilter

以下是框架过滤器的使用方法，在web.xml中添加

```xml
        <!--注册声明过滤器，解决post请求乱码问题-->
    <filter>
        <filter-name>characterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <!--设置项目中使用的编码方式-->
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <!--强制请求对象（forceRequestEncoding）使用encoding编码-->
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <!--强制应答对象（forceResponseEncoding）使用encoding编码-->
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>characterEncodingFilter</filter-name>
        <!--
            /*：表示强制所有的请求先通过过滤器处理
        -->
        <url-pattern>/*</url-pattern>
    </filter-mapping>

```

2. #### 对象接受

   处理器方法的形参时java对象，这个对象的属性名和请求中的参数名相同

   框架会创建形参的java对象，给属性赋值，请求中的参数名为name，框架会调用setName()

   ```java
    @RequestMapping(value = "/receive02.do")
   //用对象接收数据时，不能用@RequestParam注解，因为没有意义
       public ModelAndView receiveOBJ(Student student){
           System.out.println("receiveOBJ--name="+student.getName()+",age="+student.getAge());
           ModelAndView mv = new ModelAndView();
           mv.addObject("myname",student.getName());
           mv.addObject("myage",student.getAge());
           mv.addObject("mystu",student);
           mv.setViewName("show");
           return mv;
       }
   ```

   

### @RequestParam

当请求中的参数名称和处理器方法的形参名不同时，@RequestParam可以解决

​	@RequestParam：解决请求参数名和形参名不一样的问题

​	属性：value，请求中参数的名称

​				request，一个boolean类型，默认为true

​						true：表示请求中必须包含此参数，否则400错误

​						false：则表示可以不包含此参数，没有为null

​	位置：在处理器方法的形参定义前面

```java
    @RequestMapping(value = "/receive01.do",method = RequestMethod.POST)
   public ModelAndView receive01(@RequestParam(value = "rname",required = false) String name, @RequestParam(value = "rage",required = false) String age){
       ...
    }
```

```xml
<form method="post" action="/test/receive01.do">
    name:<input type="text" name="rname">
    age:<input type="text" name="rage">
    <input type="submit">
</form>
```

### 处理器方法的返回值

#### 返回ModelAndView

​	既有视图又有数据，ModelAndView的视图跳转使用的请求转发（forward），再转向视图时，也会转送数据。当只需要转发页面，或者只需要传数据时，就可以用其他的返回值。例：见上

#### 返回String

​	String表示视图，可以是逻辑名称，也可以是完整视图路径

```java
 /**
     * 处理器方法返回String---表示逻辑视图名称，需要配置视图解析器
     */
    @RequestMapping(value = "/returnString-view.do")
    public String doReturnView(HttpServletRequest request,String name,String age){
        System.out.println("name="+name+",age="+age);
//        若是想要传数据们可以手动添加数据
        request.setAttribute("name",name);
        request.setAttribute("age",age);
//      ‘show’：逻辑视图名称，项目中配置了视图解析器，也是请求转发操作
//		若是用完整类名，则不能用视图解析器
        return "show";
    }
```

```xml
<form method="post" action="/test/returnString-view.do">
    name:<input type="text" name="name"><br>
    age:<input type="text" name="age"><br>
    <input type="submit">
</form>
```



#### 返回void

​	void不能表示数据，也不能表示视图，一般在处理ajax的时候，可以使用void返回值

​	通过HttpServletReqponse输出数据。相应ajax请求，ajax请求服务端返回的就是数据，没有视图

JQuery代码

```jsp
<head> 
<script type="text/javascript" src="js/JQuery.js"></script>
    <script type="text/javascript">
        $(function (){
            $("button").click(function (){
                // alert("点击");
                $.ajax({
                    url:"/test/returnVoid-ajax.do",
                    data:{
                        name:"张三",
                        age:32
                    },
                    type:"post",
                    //想用json数据
                    dataType:"json",
                    success:function (resp){
                        //resp从服务端返回的是json格式的字符串{”name“：”zhangsan“，”age“：20}
                        //jquery会把字符串转为json对象，赋值给resp形参
                        alert("name="+resp.name+",age="+resp.age);
                    }
                })
            })
        })
    </script>
</head>
<body>
    <button id="btn">发起ajax请求</button>
</body>
```

controller

```java
    @RequestMapping(value = "/returnVoid-ajax.do")
    public void doReturnVoidAjax(HttpServletResponse response,String name, Integer age) throws IOException {
        System.out.println("doReturnVoidAjax,name="+name+",age="+age);
        Student student = new Student();
        student.setAge(age);
        student.setName(name);
        String json="";
        System.out.println("11");
        if(student!=null){
            ObjectMapper om = new ObjectMapper();
            json =om.writeValueAsString(student);
        }
//        告诉浏览器数据是json数据
        response.setContentType("application/json;charset=utf-8");
        PrintWriter pw = response.getWriter();
        pw.println(json);
        pw.flush();
        pw.close();

    }
```



#### 返回对象Object

Object可以返回对象，这个对象可以是Integer、String、自定义对象、Map、List等，返回的对象不是视图，而是直接显示在页面上的数据

Object用于框架处理ajax数据

现在做ajax，主要使用json的数据格式。实现步骤

	1.	加入处理json的工具库依赖，springmvc默认使用jackson
	2.	在Springmvc配置文件中加入< mvc:annotation-driven >注解驱动。
	 json = om.writeValueAsString(student);
	3.	在处理器方法的上面加上@RequestBody注解
	 response.setContentType("application/json;charset=utf-8");
	 PrintWriter pw = response.getWriter();
	 pw.println(json);

springmvc处理返回object，可以转为json输出到浏览器，相应ajax的内部原理

 1. < mvc:annotation-driven >注解驱动。
    	实现的功能：完成java对象的json，xml，text，二进制等数据格式的转换

    ​	< mvc:annotation-driven >再加入springmvc配置文件后，会自动创建HttpMessageConverter接口的7个实现类，包括MappingJackson2HttpMessageConverter（使用jackson工具库中的ObjectMapper实现java对象转为json字符串）

    ​	主要就是两个：
    ​			StringHttpMessageConverter:负责和写出读取字符串格式的数据
    ​			MappingJackson2HttpMessageConverter：负责读取和写入json，利用jackson的ObjectMapper读写json数据

    ​	HttpMessageConverter接口：消息转换器，定义了java对象转为json，xml等数据格式的方法，这个接口有很多实现类，这些实现类完成了java到json，java对象到xml，Java对象到二进制数据的转换


    	下面两个方法是控制器类把结果输出给浏览器时使用
    
    ​		boolean canWrite（）
    
    ​		void write（）

 2. @ResponseBody注解
    放在处理器方法的上面，通过HttpServletResponse输出数据，响应ajax请求。对应以下功能

    ```java
    PrintWriter pw = response.getWriter();
    pw.println(json);
    pw.flush();
    pw.close();
    ```

    加与不加注释的区别![image-20210709232716005](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210709232716005.png)

    需要有MappingJackson2HttpMessageConverter才能实现java对象与json数据之间的转换

    实例：

    ```java
     /*
        * @ResponseBody:
        *   作用：把处理器方法返回对象转为json后，通过HttpServletResponse输出给浏览器
        *   位置：在方法定义上面
        * 返回对象框架的处理流程：
        * 1.框架会吧返回的student类型调用ArrayList<HttpMessageConverter>中的每一个类的canWrite()方法
        *   检查那个HttpMessageConverter接口的实现类能处理student类型的数据----MappingJackson2HttpMessageConverter
        * 2.框架会调用实现类的write()，MappingJackson2HttpMessageConverter的write方法把student对象转化为json，调用	
        *	jackson的ObjectMapper实现转化为json
        *	contentType:application/json;charset=utf-8
        * 3.框架会调用@ResponseBody把2的结果数据输出到浏览器，ajax的请求完成
        * */
        @RequestMapping(value = "/returnStudentJsom.do")
        @ResponseBody
        public Student doStudentJsonObject(String name,Integer age) {
                Student student = new Student();
                student.setName("lisi");
                student.setAge(24);
                return student;
        }
    ```

    

    ```xml
     <!--注解支持-->
    <!--对于这个annotation-driven可能有很多个版本，选择时应该选择后缀为mvc的-->
        <mvc:annotation-driven/>
    ```

    当Object对象时String类型时，此时就不是返回的视图，而是返回的数据（因为有@ResponseBody）

    ```java
     /*
        * 处理器返回String类型，但这个String类型返回的不是视图，而是数据，原因就是这个@RequestMapping的存在
        *   这里@RequestMapping的默认编码方式是“text/plain;charset=ISO-8859-1“作为contentType，导致中文乱码
        *   解决方案：在@RequestMapping中加一个属性produces，用这个属性来指定新的contentType
        * */
        @RequestMapping(value = "/returnJsonString.do",produces ="text/plain;charset=utf-8")
        @ResponseBody
        public String doReturnJsonString(){
            return "returnJsonString的String数据";
        }
    ```

    对于传送的String数据，在ajax的type需要指定为text文本，或者不写。若是写json格式，则会出错，页面不显示，语法错误

    ```js
    $.ajax({
        url:"/test/returnJsonString.do",
        data:{
            name:"张三",
            age:32
        },
        type:"post",
        //想用json
        dataType:"text",
        success:function (resp){
            //resp从服务端返回的是json格式的字符串{"name"："zhangsan"，"age"：20}
            //jquery会把字符串转为json对象，赋值给resp形参
            // alert("name="+resp.name+",age="+
            /*
            循环遍历，两个弹窗
            $.each(resp,function (i,n){
            	alert(n.name+"   "+n.age)
            })
            */
            alert("String类型数据："+resp)
        }
    })
    ```

    处理流程和ArrayList处理方式差不多，参考一下就行。

### url-pattern

​	tomcat本身能够处理一些静态资源的访问，例如html，图片，js文件都是静态资源，tomcat的web.xml文件中，有一个servlet叫default，在服务器创建时启动，这个servlet的作用就是**加载静态资源**，**处理未映射到其他servlet的请求**

​	处理未映射到其他servlet的请求：意思就是没有在web.xml中注册过的，是servlet但没有注册过的页面就是交给tomcat的default处理的

​	除了使用 * .do, * .action等方式，还有一种方式是使用“/”。当项目中使用了“/”，他会替代tomcat中的defalt，导致所有的静态资源都给DispatcherServlet处理，默认情况下DispatcherServlet没有处理静态资源的能力，没有控制器对象能处理静态资源的访问，所以所有静态资源（html，js，图片，css）都是404

​	解决方式：

1. 在springmvc配置文件中加入< mvc:default-servlet-handler >,原理是使用了这个标签之后，框架会创建控制器对象DefaultServletHttpRequestHandler（类似自己创建的Controller），这个对象可以把接受的请求转发给tomcat的default这个servlet
   	但加了这个标签之后，若是不加入注解解析器（< mvc:annotation-driven >），那么这个处理器标签会与@RequestMapping冲突，导致无法访问原本交给DispatcherServlet的请求，只有加上了注解解析器才能正常访问，解决这个冲突。

   ```xml
       <!--注解支持-->
       <mvc:annotation-driven/>
       <!--静态资源访问权限-->
       <mvc:default-servlet-handler/>
   ```

2.使用<mvc:resources />标签，这个标签加入之后，框架会创建ResourceHttpRequestHandler这个处理器对象，让这个对象处理静态资源的访问，不依赖于tomcat服务器。但这个依赖与@RequestMapping也有些冲突，所以仍然需要加入注解解析器<mvc:annotation-driven />，这也是目前的主流方式

两个属性：

​	mapping：表示访问静态资源的uri地址，使用通配符 **；

​	location：静态资源在项目中的目录位置。

```xml
<mvc:annotation-driven/>
<mvc:resources mapping="/html/**" location="/html/" />
<mvc:resources mapping="/js/**" location="/js/"/>
<!-- location的左斜杠表示根目录，右斜杠表示是个目录，都不能去掉 -->
<!-- 真正开发时，会把静态资源统一放在一个static静态资源目录中，这样resource标签就只要写一次：
	<mvc:resources mapping="/static/**" location="/static/" /> 
-->
```

### 路径问题

即加不加"/"的问题。在jsp，html中使用的地址都是前端页面的地址，都是相对地址

地址分类：

- 绝对地址，带协议名称的是绝对地址。例如：http://www.baidu.com，ftp://202.122.24.04等
- 相对地址，不带协议名称的是相对地址。例如：user/some.do。相对地址不能独立使用，必须有一个参考地址，通过参考地址+相对地址本身，才能指定资源

**若是不加“/”**

​	访问的是：http://localhost:8080/ch06_path/index.jsp

​	路径：http://localhost:8080/ch06_path/

​	资源：index.jsp

​	在index.jsp发起user/some.do请求，访问地址变为http://localhost:8080/ch06_path/user/some.do，当连接没有斜杠开头，例如user/some.do，当点击链接时，访问地址是当前地址加上链接地址http://localhost:8080/ch06_path/ + user/some.do

​	本质上就是地址的累加，对于重复访问某个地址，不加斜杠可能会出错。

​	解决方案：

1. 用加斜杠的方式，见下加斜杠方式

2. 加入一个base标签，是html语言中的标签，表示当前页面中访问地址的基地址，页面中所有没有斜杠的开头的地址，都是以base标签中的地址作为参考地址

   ```jsp
   <head>
       <title>Title</title>
       <base href="http://localhost:8080/"><!-- 这样项目中不带斜杠的地址也不会因为地址累加而出错 -->
   </head>
   <body>
       <a href="some.do" >发起some.do请求</a>
   </body>
   ```

   动态获取base

   ```java
       String basePath=request.getScheme()+"://" //获取协议
           		+request.getServerName()+":"  //获取服务器名称
           		+request.getServerPort()	//获取服务器接口
           		+request.getContextPath()+"/";	//获取项目名称
   
   ```

   

**若是加"/"**

​	访问的是：http://localhost:8080/ch06_path/index.jsp

​	路径：http://localhost:8080/ch06_path/

​	资源：index.jsp

​	点击 /user/some.do 访问地址变为http://localhost:8080/user/some.do，参考地址变为服务器地址，即http://localhost:8080。

解决方案：用EL表达式${pageContext.request.contextPath}表示项目访问的值。

```jsp
<a href="${pageContext.request.contextPath}/user/some.do">bb</a>
```

## SSM整合开发

实现步骤

1. 准备数据库，新建maven项目
2. 加入依赖
   springmvc，spring，mybatis三个框架依赖，jackson依赖，mysql依赖，druid连接池，jsp，servlet依赖
3. 写web.xml
   - 注册DispatcherServlet
     - 目的1：创建springmvc容器对象，才能创建Controller类
     - 目的2：创建的是Servlet，才能接收用户请求
   - 注册spring监听器：ContextLoaderListener，目的：创建spring容器，才能创建service，dao等对象
   - 注册字符集过滤器，解决post请求乱码的问题
4. 创建包，Controller包，service，dao，实体包创建
5. 写springmvc，spring，mybatis配置文件
   - springmvc配置文件
   - spring配置文件
   - mybatis主配置文件
   - 数据库的属性配置文件
6. 写代码，dao接口和mapper文件，service和实现类，controller，实体类
7. 写页面

SSM整合代码结构

![image-20210710140912931](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210710140912931.png)![image-20210710140924341](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210710140924341.png)

StudentController.java

```java
@Controller
@RequestMapping("/student")
public class StudentController {
    @Autowired
    private StudentService service;
    //注册
    @RequestMapping("/addStudent.do")
    public ModelAndView addStudent(Student student){
        ModelAndView mv= new ModelAndView();
        String tips = "注册失败";
        //调用service处理student
        System.out.println(student);
        int nums = service.addStudent(student);
        System.out.println(nums);
        if(nums>0){
            //注册成功
            tips = "学生 ["+student.getName()+"] 注册成功";
        }
        mv.addObject("tips",tips);
        mv.setViewName("result");
        return mv;
    }
    @RequestMapping("/querryStudent.do")
    @ResponseBody
    public List<Student> queryStudent(){
        //参数检查
        List<Student> students = service.findStudents();
        return students;
    }
}
```

StudentDao.java

```java
public interface StudentDao {
    int insertStudent(Student student);
    List<Student> selectStudents();
}
```

StudentDao.xml

```xml
<mapper namespace="com.dao.StudentDao">

    <select id="selectStudents" resultType="Student">
        select id,name,age from student order by id desc
    </select>
    <insert id="insertStudent">
        insert into student(name,age) values(#{name},#{age})
    </insert>
</mapper>
```

Student.java

```java
public class Student {
    private Integer id;
    private String name;
    private Integer age;

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
                ", age=" + age +
                '}';
    }
}
```

StudentService.java

```java
public interface StudentService {
    int addStudent(Student student);
    List<Student> findStudents();
}
```

StudentServiceImpl.java

```java
@Service
public class StudentServiceImpl implements StudentService {
    /*spring配置文件中创建了dao对象，用这个注解可以自动赋值*/
    @Autowired
    private StudentDao studentDao;

    @Override
    public int addStudent(Student student) {
        int nums = studentDao.insertStudent(student);
        return nums;
    }

    @Override
    public List<Student> findStudents() {
        return studentDao.selectStudents();
    }
}
```

applicationContext.xml

```xml

    <!--spring配置文件，声明service，dao，工具类等对象-->
    <!--声明数据源连接数据库-->
    <context:property-placeholder location="classpath:conf/jdbc.properties" />
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
        <property name="url" value="${jdbc.url}" />
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
        <property name="maxActive" value="${jdbc.maxActive}"/>
    </bean>

    <!--声明SqlSessionFactoryBean创建SqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <property name="configLocation" value="classpath:conf/mybatis.xml" />
    </bean>

    <!--声明mybatis的扫描器，创建dao对象-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
        <property name="basePackage" value="com.dao"/>
    </bean>

    <!--声明@Service注解所在的包名位置-->
    <context:component-scan base-package="com.service"/>

    <!--事务的配置：注解的配置或者aspect的配置-->

```

jdbc.properties

```properties
jdbc.url=jdbc:mysql://localhost:3306/test
jdbc.username=mcl
jdbc.password=DNF321
jdbc.maxActive=20
```

mybatis.xml

```xml

    <settings>
        <!--mybatis输出日志-->
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
    <typeAliases>
        <package name="com.domain"/>
    </typeAliases>
    <!--用这个包加载有两点要求
        1.mapper文件名称和dao接口名称必须完全一样，包括大小写
        2.mapper文件和dao接口必须在同一目录
    -->
    <mappers>
        <package name="com.dao"/>
    </mappers>
```

springmvc.xml

```xml
    <!--springmvc配置文件，声明Controller和其他相关对象-->
    <context:component-scan base-package="com.controller" />
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/" />
        <property name="suffix" value=".jsp" />
    </bean>
    <mvc:annotation-driven />
    <!--
        1.响应ajax请求返回json
        2.解决静态资源访问
    -->
```

web.xml

```xml
   <!--注册中央调度器-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:conf/springmvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>
    <!--注册监听器-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:conf/applicationContext.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>


    <!--注册字符集过滤器-->
    <filter>
        <filter-name>characterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>

        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>characterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

index.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%
    //动态获取基路径
    String basePath=request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+request.getContextPath()+"/";
%>
<html>
<head>
    <title>功能入口</title>
    <base href="<%=basePath%>">
</head>
<body>
<div align="center">
    <p>SSM整合框架</p>
    <img src="">
    <table>
        <tr>
            <td><a href="addStudent.jsp">注册学生</a></td>
        </tr>
        <tr>
            <td><a href="listStudent.jsp"> 浏览学生</a></td>
        </tr>
    </table>
</div>
</body>
</html>
```

addStudent.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>

<%
    //动态获取基路径
    String basePath=request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+request.getContextPath()+"/";
%>
<html>
<head>
    <title>Title</title>
    <base href="<%=basePath%>">
</head>
<body>
<div align="center">
    <form action="student/addStudent.do" method="post">
        <table>
            <tr>
                <td>姓名：</td>
                <td><input type="text" name="name"></td>
            </tr>
            <tr>
                <td>年龄：</td>
                <td><input type="text" name="age"></td>
            </tr>
            <tr>
                <td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</td>
                <td><input type="submit" value="注册"></td>
            </tr>
        </table>
    </form>
</div>
</body>
</html>
```

listStudent.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%
    //动态获取基路径
    String basePath=request.getScheme()+"://"+request.getServerName()+":"+request.getServerPort()+request.getContextPath()+"/";
%>
<html>
<head>
    <title>Title</title>
    <base href="<%=basePath%>">
    <script type="text/javascript" src="js/JQuery.js"></script>
    <script type="text/javascript">
        $(function (){
            loadStudentData();
            $("#btnloader").click(function (){
                loadStudentData();
            })
        })
        function loadStudentData(){
            $.ajax({
                url:"student/querryStudent.do",
                type:"get",
                dataType:"json",
                success:function (data){
                    //清楚旧数据
                    $("#info").html("");
                    //增加新数据
                    $.each(data,function (i,n){
                        $("#info").append("<tr>")
                            .append("<td>"+n.id+"</td>")
                            .append("<td>"+n.name+"</td>")
                            .append("<td>"+n.age+"</td>")
                            .append("</tr>")
                    })
                }
            })
        }
    </script>
</head>
<body>
<div align="center">
<table>
    <thead>
    <tr>
        <td>学号</td>
        <td>姓名</td>
        <td>年龄</td>
    </tr>
    </thead>
    <tbody id="info">

    </tbody>
</table>
    <input type="button" id="btnloader" value="查询数据">
</div>
</body>
</html>
```

result.jsp

```jsp
<body>
   result.jsp结果页面，注册结果：${tips}
</body>
```

## 转发与重定向

forward和rediect都是关键字，共同特点是都不和视图解析器一起工作

### forward

​	主要用于转发到视图解析器以外的页面，因为存在视图解析器，所以所有寻常方式跳转的页面都会默认添加前后缀，若是页面不在同一个文件夹中，则无法跳转，但forward可以实现，因为他不受视图解析器的影响

```java
    @RequestMapping("/doForward.do")
    public ModelAndView doforwar(){
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","springmvc");
        mv.addObject("fun","某个方法");
        //显式转发，文件在web-inf目录中
        mv.setViewName("forward:/WEB-INF/view/show.jsp");//forward不受视图解析器的影响
        /**
        文件在webapp中，不在web-inf里面
        mv.setViewName("forward:/showOut1.jsp");
        */
        return mv;
    }
```

​	只要在WEB-INF里面的就要加WEB-INF。但是要是在web-inf外面，即webapp中，是可以直接访问的，直接用斜杠+地址就可以了

### rediect

​	rediect特点：不和视图解析器同一项目使用，就当项目中没有视图解析器。

框架对重定向操作的处理：

1. 框架会把model中的简单数据类型的数据，转为String类型的字符串，作为hello.jsp的get请求参数使用。目的是在doRedirect.do和hello.jsp两次请求之间传递数据
2. 在目标hello.jsp页面可以使用参数集合对象${param}获取请求参数的值。${param.msg}

```java
  //ModelAndView实现rediect重定向
    @RequestMapping("/doRediect.do")
    public ModelAndView doRediect(String name,String age){
        ModelAndView mv = new ModelAndView();

        //数据放入request作用域
        mv.addObject("msg",age);
        mv.addObject("fun",name);
        //重定向
        mv.setViewName("redirect:/hello.jsp");//forward不受视图解析器的影响
//        mv.setViewName("show");
        return mv;
    }
```

前端hello.jsp

```jsp
    <h3>test01数据：${param.msg}</h3>
    <h3>test02数据：${param.fun}</h3>
    相当于<%=request.getParameter("msg")%>
```

**注意：**重定向不能访问web-inf下面的资源

## 异常处理

​	springmvc框架采用的是统一，全局的异常处理，把controller中的所有异常处理都集中到一个地方。采用的是aop的思想，把业务逻辑和异常处理代码分开。解耦合。

​	业务需求：寻常处理异常的方式一般为try..catch和throw，但这两种方式都是需要针对不同的异常处理，且会让代码看起来很杂乱，若是更改了异常可能又要重新处理。

​	两个注解

- @ExceptionHandler
- @ControllerAdvice

步骤：

1. 新建一个自定义异常类MyUserException，定义它的子类NameException和AgeException
2. 在Controller中抛出AgeException和NameExceopion
3. 创建一个普通类，作为全局异常处理类
   - 在类的上面加入@Controllerannotation-drivenAdvice
   - 在类中定义方法，方法的上面加入ExceptionHandler
4. 创建处理异常类的视图
5. 创建springmvc的配置文件
   - 组件扫描器，扫描@Controller注解
   - 组件扫描器，扫描@ControllerAdvice所在的包名
   - 声明注解驱动

例子：

MyUserException

```java
public class MyUserException extends Exception{
    public MyUserException() {
        super();
    }

    public MyUserException(String message) {
        super(message);
    }
}
```

两个子类AgeException和NameException

```java
public class AgeException extends MyUserException{
    public AgeException() {
        super();
    }
    public AgeException(String message) {
        super(message);
    }
}
```

```java
public class NameException extends MyUserException{
    public NameException() {
        super();
    }
    public NameException(String message) {
        super(message);
    }
}
```

统一处理异常的类GlobalException

```java

/**
 * @ControllerAdice:控制器增强（给控制器增加功能---异常处理功能）
 * 位置：在类的上面
 * 特点：必须让框架知道这个注解的所在包名，需要在springmvc配置文件中声明组件扫描器
 * 指定@ContollerAdvice所在的包
 */

@ControllerAdvice

public class GlobalException {
    //定义方法处理发生的异常
    /*
    * 处理异常的方法和控制器定义的方法一样，可以有多个参数，可以有ModelAndView，String，void，对象类型的返回值
    * */
    //形参：Exception表示Controller中抛出的异常对象，通过Exception获取发生的异常信息
    //@ExceptionHandler（异常的class）：表示异常的类型，当发生此异常时，由当前方法来处理
    
    @ExceptionHandler(value = NameException.class)
    
    public ModelAndView doNameException(Exception ex){
        /*
        异常的处理逻辑：
            1.需要把异常记录下来，记录到数据库，日志文件，记录日志发生的时间，哪个方法，异常的错误内容
            2.发送通知，把异常的信息通过邮件，短信，微信发送给相关人员
            3.给用户友好的提示
         */
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","得是zs");
        mv.addObject("ex",ex);
        mv.setViewName("nameError");
        return mv;
    }
    @ExceptionHandler(value = AgeException.class)
    public ModelAndView doAgeException(Exception ex){
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","age错误");
        mv.addObject("ex",ex);
        mv.setViewName("ageError");
        return mv;
    }

    //处理其他异常，name和age以外的不知类型的异常
    @ExceptionHandler
    public ModelAndView doOtherException(Exception ex){
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg","其他错误");
        mv.addObject("ex",ex);
        mv.setViewName("defaultError");
        return mv;
    }
}
```

MyController的方法

```java
 @RequestMapping(value = "some.do")
    public ModelAndView doSome(String name,Integer age) throws MyUserException {
        ModelAndView mv = new ModelAndView();
        //异常的例子
        //根据请求参数抛出异常
        if(!("zs".equals(name))){
            throw new NameException("姓名不正确");
        }
        if(age==null || age>80){
            throw new AgeException("年龄不正确");
        }
        mv.addObject("name",name);
        mv.addObject("age",age);
        mv.setViewName("show");
        return mv;
    }
```

需要添加的注解(springmvc)

```xml
<!--组件扫描器-->
<context:component-scan base-package="com.handler"/>
<!--注解扫描器-->
<mvc:annotation-driven/>
```

​	一个异常处理类（ExceptionHandler）可以对多个controller类中的相同类型异常进行统一处理，与业务逻辑完全分开，做到了解耦合。



## 拦截器

​	可以看作是多个Controller中共用的功能，集中到拦截器统一处理，使用的是Aop的思想

1. 拦截器是springmvc的一种，需要实现HandlerInterceptor接口
2. 拦截器和过滤器类似，功能方向侧重点不同。拦截器是拦截用户请求，做请求做判断处理的。过滤器是用来过滤请求参数，设置编码字符集等工作。
3. 拦截器是全局的，可以对多个Controller做拦截请求。一个项目中可以有0个或多个拦截器，他们在一起拦截用户的请求。
   拦截器常用在：用户登陆处理，权限检查，记录日志。

拦截器的使用步骤

1. 定义类实现HandlerIntercepter接口
2. 在springmvc的配置文件中，声明拦截器，让框架知道拦截器的存在。

拦截器的执行时间

1. 在请求处理之前，也就是controller类中的方法执行之前被拦截
2. 在控制器方法执行之后也会执行拦截器
3. 在请求处理完成后也会执行拦截器

实现步骤

1. 创建一个普通的类，作为拦截器使用
   - 实现HandlerInterceptor接口
   - 实现接口中的三个方法
2. 创建视图
3. 创建springmcv的配置文件
   - 组件扫描器，扫描@Controller注解
   - 声明拦截器，并指定拦截的请求uri地址

**一个拦截器**

例：

Controller部分

```java
public class MyController {
    @RequestMapping(value = "some.do")
    public ModelAndView doSome(String name,Integer age){
        ModelAndView mv = new ModelAndView();
        mv.addObject("name",name);
        mv.addObject("age",age);
        mv.setViewName("show");
        return mv;
    }
}
```

拦截器MyInterceptor部分

```java
public class MyInterceptor implements HandlerInterceptor {
    private long btime=0;
    /*
    preHandle:预处理方法，这个方法很重要，是整个程序的入口和门户
        当请求返回为true，则请求可以被处理
        当请求返回为false，则请求到此为止
    参数：Object handler：被拦截的控制器对象
    返回值：
        true：请求通过拦截器验证，可以执行处理器方法
            拦截器的preHandle方法
            doSome方法
            拦截器的postHandle方法
            拦截器的afterCompletion方法
        false：
            请求没有通过拦截器的验证，请求到这里就停止了，请求没有被处理
            只执行了 拦截器的preHandle方法 但返回是false，所以就没有后面的拦截处理

    预处理方法的特点：
        1.方法在控制器方法（MyController的doSome方法）之前先执行，用户的请求先到达此方法
        2.在这里方法可以验证请求信息，验证请求是否符合要求，可以验证用户是否登录，验证用户是否有权限访问某个链接地址（url）
            如果验证失败，可以截断请求，请求不能被处理
            如果验证成功，可以放行请求，此时控制器方法才能执行
    */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("拦截器的preHandle方法");
        btime = System.currentTimeMillis();
        //在拦截器中，可以控制错误时，页面的转向
        //像这样“/tips.jsp”的形式，页面需要放在WEB-INF外面
//        request.getRequestDispatcher("/tips.jsp").forward(request,response);
        return true;
    }
    /*
    postHandle：后处理方法
    参数：Object handler：被拦截的处理器对象MyController
    ModelAndView modelAndView：处理器方法的返回值
    特点：
        1.在处理器方法之后执行
        2.能够获取到处理器方法返回的ModelAndView，可以修改ModelAndView中的数据和视图，可以影响到最后的执行结果
        3.主要是针对原来的数据结果做二次修正
     */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("拦截器的postHandle方法");

        if(modelAndView!=null){
            //修改数据
            modelAndView.addObject("date",new Date());
            //修改视图
            modelAndView.setViewName("other");
        }
        HandlerInterceptor.super.postHandle(request, response, handler, modelAndView);
    }
    /*
    afterCompletion：最后执行的方法
    参数：
    Object handler：被拦截的处理器对象
    Exception exception：程序中发生的异常
    特点：
        1.在请求处理完成后执行，框架中规定在视图处理完成之后，即对视图执行了forward，就认为请求处理完成
        2.一般做资源回收工作的。程序请求过程中创建了一些对象，在这里可以删除，把占用的内存回收
     */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("拦截器的afterCompletion方法");
        HandlerInterceptor.super.afterCompletion(request, response, handler, ex);
        /*这个方法可以用来计算一个请求开始到完成所需要花费的时间
          这个时间一般只有第一次是比较长的，因为要进行jsp的初始化，将jsp转为servlet等操作
          之后的请求响应时间都会比较短
        */
        long etime = System.currentTimeMillis();
        System.out.println("整个请求所花费的时间："+(etime-btime));
    }
}
```

配置文件springmvc部分

```xml
    <!--声明拦截器：拦截器可以是0个或多个-->
    <mvc:interceptors>
        <mvc:interceptor>
            <!--指定拦截器的uri地址
                path：uri地址，可以使用通配符 **
                **：表示任意的字符，文件或者多级目录和目录中的文件
                例：
                /**表示所有的请求都拦截
                /users/**表示所有users目录下的子目录中所有的文件都经过拦截器
            -->
            <mvc:mapping path="/**"/>
            <!--声明拦截器对象-->
            <bean class="com.handler.MyInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>

```

show.jsp（WEB-INF内）

```jsp
    <h3>name数据：${name}</h3>
    <h3>age数据：${age}</h3>
```

other.jsp（WEB-INF内）

```jsp
    <h3>postHandler处理后的页面</h3>
    <h3>myname:${name}</h3>
    <h3>age:${age}</h3>
    <h3>mydate:${date}</h3>
```

tips.jsp（WEB-INF外）

```jsp
tips.jsp<br/>
preHandler方法返回为false
```

2个拦截器

例：

第一个拦截器同上

第二个拦截器

```java
public class MyInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("拦截器1的preHandle方法");
        return true;
    }
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("拦截器1的postHandle方法");
    }
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("拦截器1的afterCompletion方法");
    }
}
```

配置文件springmvc

```xml
    <!--
		拦截器的执行顺序是先声明的先执行，因为拦截器在框架中的保存形式是ArrayList
      按照声明的先后顺序放入ArrayList
    -->
    <!--声明拦截器：拦截器可以是0个或多个-->
    <mvc:interceptors>
        <!--第一个拦截器-->
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <!--声明拦截器对象-->
            <bean class="com.handler.MyInterceptor"/>
        </mvc:interceptor>
        <!--第二个拦截器-->
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <!--声明拦截器对象-->
            <bean class="com.handler.MyInterceptor2"/>
        </mvc:interceptor>
    </mvc:interceptors>
```

结果：

```tex
拦截器1的preHandle方法
拦截器2的preHandle方法
---doSome方法---
拦截器2的postHandle方法
拦截器1的postHandle方法
拦截器2的afterCompletion方法
拦截器1的afterCompletion方法
```

请求顺序图例

![](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210715234012755.png)

- 当第一个拦截器的priHandler为true，第二个拦截器的preHandler为false

结果：

```
拦截器1的preHandle方法
拦截器2的preHandle方法
拦截器1的afterCompletion方法
```

分析：拦截器1的preHandler为true，所以进入拦截器2的perHandler，但拦截器2的preHandler为false，所以请求到此为止，但由于拦截器1的priHandler为true，所以还是会执行拦截器1的afterHandler方法，所以结果如上。

- 当第一个拦截器的preHandler为false时，不管第二个拦截器preHandler的返回值如何都不会继续执行，请求到此为止了，结果如下：

  ```
  拦截器1的preHandle方法
  ```

总结：任意一个请求的拦截器返回为false，请求就不会执行

**拦截器和过滤器的区别**

1. 过滤器是servlet中的对象，拦截器是框架中的对象
2. 过滤器实现Filter接口，拦截器实现HandlerInterceptor接口
3. 拦截器用来设置request和response的参数、属性的，侧重对数据的过滤
   拦截器用来验证请求，能截断消息
4. 过滤器是在拦截器之前先执行的
5. 过滤器是tomcat服务器创建的对象，拦截器是springmvc容器中创建的对象
6. 拦截器有三个执行点，过滤器只有一个
7. 过滤器可以处理JSP,JS,HTML等等，但拦截器侧重于拦截对Controller的对象，如果请求不能被DispatcherServlet接收，这个请求就不会被拦截器接收
8. 拦截器拦截普通类的方法的执行，过滤器过滤servlet请求响应

**用拦截器实现登录验证**

实现步骤：

1. index.jsp发起请求
2. 创建Controller处理请求
3. 创建结果show.jsp
4. 创建一个login.jsp模拟登录（将用户信息放入session）
   创建一个loginout.jsp，模拟退出登录（将用户信息从session中移除）
5. 创建拦截器从session中获取用户登录数据，验证是否能访问系统
6. 创建一个验证的jsp，如果验证出错，给出提示
7. 配置文件 

处理逻辑：在登陆之前（也就是将数据放入session之前）用户无法进入展示界面（show.jsp），处理器访问不到数据就会一直进入tips.jsp，只有先进入登陆页面（login.jsp）将数据放入session中，之后再通过index.jsp进入展示页面，此时拦截器也能访问到数据，于是就进入show.jsp，若是之后要退出系统（进入过loginout.jsp），则删除session中的数据，导致无法在进入到展示页面show.jsp.

login.jsp

```jsp
模拟登陆,zs
<%
    session.setAttribute("name","zs");
%>
```

loginout.jsp

```jsp
退出系统
<%
session.removeAttribute("name");
%>
```

index.jsp

```jsp
<form action="some.do" method="post">
    年龄：<input type="text" name="name"><br/>
    年龄：<input type="text" name="age"><br/>
    <input type="submit" value="提交请求">
</form>
```

tips.jsp

```jsp
tips.jsp<br/>
preHandler方法返回为false<br/>
不是zs
```

拦截器

```java
public class MyInterceptor implements HandlerInterceptor {
        //验证用户登录信息
        @Override
        public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
            System.out.println("拦截器1的preHandle方法");
            Object attr = request.getSession().getAttribute("name");
                String name = "";
            if(attr!=null){
                name= (String) attr;
            }
            if(!"zs".equals(name)){
                request.getRequestDispatcher("/tips.jsp").forward(request,response);
                return false;
            }
            return true;
        }
}
```

controller的方法

```java
    @RequestMapping(value = "some.do")
    public ModelAndView doSome(String name,Integer age){
        ModelAndView mv = new ModelAndView();
        mv.addObject("name",name);
        mv.addObject("age",age);
        mv.setViewName("show");
        return mv;
    }
```

配置文件

```xml
   <mvc:interceptors>
        <!--第一个拦截器-->
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <!--声明拦截器对象-->
            <bean class="com.handler.MyInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
```

## springmvc内部请求的处理流程

springmvc接受请求到处理完成的过程

![image-20210717003926436](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210717003926436.png)

1. 用户发起请求some.do
2. DispatcherServlet接受请求some.do,把请求转交给处理器映射器
   处理器映射器：springmvc框架中的一种对象，框架把实现HandlerMapping接口的类都叫做映射器（可以有多个）
   处理器映射器的作用：根据请求，从springmvc容器对象中获取处理器对象（MyController controller = ctx.getBean(“some.do”)），框架把找到的处理器对象放到一个叫做处理器执行链（HandlerExecutionChain）的类中保存
   HandlerExecutionChain：类中保存的是：1. 处理器对象（MyController）2.项目中的所有拦截器List< HandlerInterceptor >
3. DispathcerServlet把2中的HandlerExecuteChain中的处理器对象交给了处理器适配器对象（也有多个）
   处理器适配器：springmvc框架中的对象，需要实现HandlerAdapter接口
   处理器适配器的作用：执行处理器方法（调用MyController.doSome()得到返回值ModelAndView）
4. DispatcherSerclet把3中获取的ModelAndView交给了视图解析器对象
   视图解析器：springmvc中的对象，需要实现ViewResoler接口（可以有多个）
   视图解析器的作用：组成视图完整的路劲，使用前缀，后缀，并创建View对象
   View是一个接口，表示使徒的，在框架中jsp,html不是String表示，而是使用View和他的实现类表示
   InterNalResourceView：视图类，表示jsp文件，视图解析器会创建InternalResourceView对象。这个对象的里面有一个属性url = /WEB-INF/view/show.jsp
5. DispatcherServlet把4步骤中创建的View对象获取到，调用View类自己的方法，把Model数据放入request作用域。执行对象视图的forward。请求结束。

