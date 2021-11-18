# Ajax

## 全局刷新与局部刷新

全局刷新：网页内容全部更改

局部刷新：网页内容部分修改，例如：百度搜索框中输入部分数据之后显示的提示框

ajax：局部刷新就是用ajax创建异步对象，使网页实现局部刷新的一种技术。

## 异步请求对象

XMLHttpRequest

创建异步对象

```js
var xmlhttp=new XMLHttpRequest();
```

### AJAX全称：Asynchronous  JavaScript and XML

asynchronous：异步

JavaScript：java脚本

XML：一种数据格式

## AJAX异步实现方式

- **创建对象**

  ```js
  var xmlhttp = new XMLHttpRequest();
  ```

- **给异步对象绑定事件**
   onreadystatechange：当异步对象发起请求，获取了数据都会触发这个事件，这个事件需要指定一个函数，在函数中处理对象的变化

  ```js
  btn.onclick = fun1();
  function fun1(){
  	alert('点击事件');
  }
  ```

  例：

  ```js
  xmlhttp.onreadystatechange = function(){
      //处理请求参数的变化
      //处理请求的状态变化
      if(xmlHttp.readyState == 4 && xmlHttp.status==200){
          //处理服务端的数据，更新当前页面
          //获取异步对象的属性responseText，并处理
          var data = xmlHttp.responseText;
          document.getElementById("name").value = data;
      }
  }
  ```

  readyState属性

  存有XMLHttpRequest的状态，从0-4变化

  - 0：请求未初始化，创建异步请求对象 var xmlhttp = new XMLHttpRequest();
  - 1：初始化异步请求对象，xmlHttp.open（请求方式 ，请求地址 ，true）
  - 2：异步对象发送请求，xmlHttp.send()
  - 3：异步对象接收应答数据，从服务器返回数据。XMLHttpRequest内部处理
  - 4：异步请求对象已经把数据解析完毕。此时可以读取数据

  Status属性，表示网络请求的状况，200，404，500...

- **初始异步请求对象**
  异步方法open()

  xmlHttp.open（请求方式get|post ，"服务端的访问地址" ，同步|异步请求(默认为true，异步)）

  例

  ```js
  xmlHttp.open('get','loginServlet?name=zs&pwd=123',true)
  ```

- **使用异步对象发送请求**

  xmlHttp.send()

获取服务端返回的数据，使用异步对象的属性responseText

回调：当请求的状态变化时，异步对象会自动调用onreadystatechange事件对应的函数。

**ajax实例1**

servlet

```java
public class BMItest01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String strName = req.getParameter("name");
        String strHeight = req.getParameter("h");
        String strWeight = req.getParameter("w");
        float h = Float.valueOf(strHeight);
        float w = Float.valueOf(strWeight);
        float bmi = w/(h*h);
        String msg="";
        if(bmi<18.5)    msg = "太瘦";
        else if (bmi>=18.5 && bmi<24) msg="正常";
        else if (bmi>=24 && bmi<27) msg="小胖";
        else msg="巨胖";
        msg=strName+"的BMI值为："+bmi+"     属于 "+ msg;
        //响应ajax的值
        resp.setContentType("text/html;charset=utf-8");
        PrintWriter pw = resp.getWriter();
        //关于异步对象的responseText返回的数据，一般是放入pw.println方法中的数据
        pw.println(msg);
        pw.flush();
        pw.close();
    }
}
```

jsp内容

```html
  <head>
    <title>局部刷新-ajax</title>
    <script type="text/javascript">
      function doAjax() {
        //异步对象创建
        var xmlHttp = new XMLHttpRequest();
        //绑定事件
        xmlHttp.onreadystatechange = function () {
          //处理服务器返回数据，更新当前页面(DOM)
          // alert("readyState=====>"+xmlHttp.readyState+" | status==>"+xmlHttp.status)
          if(xmlHttp.readyState==4&&xmlHttp.status==200){
            //alert(xmlHttp.responseText)
            var data = xmlHttp.responseText;
            document.getElementById('mydata').innerText = data;
          }
        }
        //初始请求的数据
        var name =document.getElementById('name').value;
        var h = document.getElementById('height').value;
        var w = document.getElementById('weight').value;
        var param = "name="+name +"&w="+w+"&h="+h;
      	//alert(param);
   		//open方法会再次调用事件方法，即上面绑定的事件方法
          //之所以这里没有出现弹窗，是因为状态码使3，但if判断为4，所以不响应
        xmlHttp.open("get", "bmiAjax?"+param, true);
        //发起请求，这里也会调用上面的绑定方法
        xmlHttp.send();
      }
    </script>
  </head>
  <body>
  <%--用ajax处理数据可以不用form--%>
    姓名：<input type="text" id="name"><br/>
    身高：<input type="text" id="height"><br/>
    体重：<input type="text" id="weight"><br/>
    <button type="submit" onclick="doAjax()">提交</button>
    <div id="mydata">等待刷新数据....</div>
  </body>
```

web.xml的配置（复习一下吧）

```xml
    <servlet>
        <servlet-name>bmItest01</servlet-name>
        <servlet-class>com.ajax.BMItest01</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>bmItest01</servlet-name>
        <url-pattern>/bmiAjax</url-pattern>
    </servlet-mapping>
```

## JDBC连接数据库（复习）

```java
public class JDBC {
    public String querryData(Integer id){
        Connection conn = null;
        PreparedStatement pst = null;
        ResultSet rs = null;
        String sql="";

        String url = "jdbc:mysql://localhost:3306/testDB";
        String username ="mcl";
        String password = "DNF321";

        String data="";
        try {
            //加载驱动
            Class.forName("com.mysql.jdbc.Driver");
            conn = DriverManager.getConnection(url,username,password);
            //创建PreparedStatement,?表示占位符
            sql ="select * from testDB where id=?";
            pst = conn.prepareStatement(sql);
            //用传入的参数替代占位符”？“
            pst.setInt(1,id);
            //执行查询语句
            rs = pst.executeQuery();
            //遍历rs数组
            while(rs.next()){
                data = rs.getString("name");
            }
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        }finally {
            try {
                if(rs!=null){
                    rs.close();
                }
                if(pst!=null){
                    rs.close();
                }
                if(conn!=null){
                    conn.close();
                }
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        return data;
    }
}
```

## JSON

JSON的依赖包

- jackson-annotation-2.9.0.jar
- jsckson-core-2.9.0.jar
- jackson-databind-2.9.0.jar

### JSON的优点

1. json格式好理解
2. json格式数据在多种语言中，比较容易处理，使用java，javascript读写json数据比较容易
3. json数据格式占用空间小，在网络中传输速率快，用户体验好

### JSON的工具库

- gson(google)
- fastjson：速度快，但不是最符合json处理规范的
- jackson：性能好，规范好
- json-lib：性能差，依赖多，不常使用

### JSON的使用

实例

Province

```java
public class Province {
    private Integer id;
    private String name;
    private String jiancheng;
    private String shenghui;

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

    public String getJiancheng() {
        return jiancheng;
    }

    public void setJiancheng(String jiancheng) {
        this.jiancheng = jiancheng;
    }

    public String getShenghui() {
        return shenghui;
    }

    public void setShenghui(String shenghui) {
        this.shenghui = shenghui;
    }
}
```

jdbc

```java
    public Province querryData(Integer id){
        Connection conn = null;
        PreparedStatement pst = null;
        ResultSet rs = null;
        String sql="";

        String url = "jdbc:mysql://localhost:3306/springdb";
        String username ="mcl";
        String password = "DNF321";

        Province p=null;
        try {
            //加载驱动
            Class.forName("com.mysql.jdbc.Driver");
            conn = DriverManager.getConnection(url,username,password);
            //创建PreparedStatement,?表示占位符
            sql ="select id,name,jiancheng,shenghui from province where id=?";
            pst = conn.prepareStatement(sql);
            //用传入的参数替代占位符”？“
            pst.setInt(1,id);
            //执行查询语句
            rs = pst.executeQuery();
            //遍历rs数组
            while(rs.next()){
                p = new Province();
                p.setId(rs.getInt("id"));
                p.setName(rs.getString("name"));
                p.setJiancheng(rs.getString("jiancheng"));
                p.setShenghui(rs.getString("shenghui"));
            }
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        }finally {
            try {
                if(rs!=null){
                    rs.close();
                }
                if(pst!=null){
                    rs.close();
                }
                if(conn!=null){
                    conn.close();
                }
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        return p;
    }
```

servlet

```java
public class JsonServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String json="{}";
        String strProid = req.getParameter("proid");
        //trim()去空格
        if(strProid!=null &&strProid.trim().length()>0){
            Province p = new Province();
            JDBC jdbc = new JDBC();
            p= jdbc.querryData(Integer.valueOf(strProid));
            ObjectMapper om = new ObjectMapper();
            json = om.writeValueAsString(p);
        }
        //指定服务器端返回给浏览器的是json数据
        resp.setContentType("application/json;charset=utf-8");
        PrintWriter pw = resp.getWriter();
        pw.println(json);//输出数据，数据会付给ajax的responseText属性

    }


}
```

前端代码

```html
<html>
<head>
    <title>Title</title>
    <script>
        function doSearch(){
            var name = document.getElementById("name");
            var jiancheng=document.getElementById("jiancheng");
            var shenghui = document.getElementById("shenghui")
            var xmlhttp =new  XMLHttpRequest();
            xmlhttp.onreadystatechange=function (){
                var json = xmlhttp.responseText;
                if(xmlhttp.status==200&&xmlhttp.readyState==4){
                    // alert(json);
                    //eval是执行括号中的代码，把json数据转化为json对象
                    var jsonobj = eval("("+json+")");
                    name.value = jsonobj.name;
                    jiancheng.value=jsonobj.jiancheng;
                    shenghui.value=jsonobj.shenghui;

                }
            }
            var proid = document.getElementById("proid").value;
            xmlhttp.open("get","querryjson?proid="+proid,true)
            xmlhttp.send();
        }
    </script>
</head>
<body>
<table>
    <tr><td>编号：</td>
        <td><input type="text" id="proid"></td>
        <td><input type="button" onclick="doSearch()" value="搜索"></td>
    </tr>
    <tr><td>名称：</td>
        <td><input type="text" id="name"></td>
    </tr>
    <tr>
        <td>简称：</td>
        <td><input type="text" id="jiancheng"></td>
    </tr>
    <tr>
        <td>省会：</td>
        <td>
            <input type="text" id="shenghui">
        </td>
    </tr>
</table>
</body>
</html>
```

## 同步与异步

> 同步，open的最后一个属性为false

	同步对象必须处理完成请求从服务端获取数据之后，才能执行send方法之后的代码，任意时刻只能执行一个异步请求

> 异步，open的最后一个属性为true

	异步处理请求，在使用异步对象发出请求后，不用等待请求处理结果完毕就可以执行其他操作

## AJAX在JQuery中的使用

	没有JQ的话，使用ajax需要使用XMLHttp的4个步骤，JQ简化了ajax请求的处理。使用三个函数就可以实现。
	
	JQ中 $ 就代表了JQ类，后面的方法都可以“.”出来，例如each方法

1. $.ajax()：jquery中实现ajax的核心函数
2. $.post()：使用post方式做ajax请求
3. $.get()：使用get方法发送ajax请求

$.post()和$.get()函数内部都是调用的$.ajax函数。

### $.ajax的使用方法

**$.ajax( {名称：值 ，名称：值 ，名称：值 ... } )**

例：

```js
 $.ajax( {
    async:true,
    contentType:"application/json",
    data:{name:"zhangsan",age:20},
    dataType:"json",
    error: function(){  ...  } ,
    success:function( data ){
    //data就是responseText，是处理后的数据
    },
    url:"bmiAjax",
    type:"get"
})
```



> json结构的参数

- async：boolean类型的值，默认为true，表示异步请求，可以不写，与之前的xmlHttp.open(get,url,true)的第三个参数一样的意思。
- contentType：一个字符串，表示从浏览器发送给服务器的参数类型，可以不写，例如想表示请求的参数为json格式，可以写application/json
- data：可以是字符串、数组、json，表示请求的参数和参数值，常用的是json格式的数据
- dataType：表示期望从服务器端返回的数据格式，可选的有：xml，html，text，json。当使用$.ajax发送请求时，会把dataTyoe的值发送给服务器，然后servlet能够读取到dataType的值，就知道浏览器需要的时json或者xml的数据，那么服务器就可以返回需要的数据格式。
- error：一个function，表示请求发生错误时，执行的函数。
  例：error：function(){ .... }
- success：也是一个function，请求成功了，从服务器端返回了数据，会执行success指定函数
- url：请求的地址
- type：请求的方式，get或者post，不用区分大小写，默认为get

主要使用 url，data，dataType，success，type

### $.get和$.post的使用方法

#### $.get

使用HTTP GET请求从服务器加载数据

语法：$.get( url，data，function（resp），dataType )

#### $.post

使用HTTP POST请求从服务器加载数据

语法：$.post(url，data，function（resp），dataType)

## 使用AJAX级联查询

	级联查询要实现的是，某个功能的实现或者变换依赖于另一个功能的实现或变换。

实例：省市级联查询

index.jsp

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>省市级联查询</title>
    <script src="js/JQuery.js"></script>
    <script type="text/javascript">
        function loadDataAjax(){
            $.ajax({
                url:"queryProvince",
                dataType:"json",
                //resp就是json数据格式
                success:function (resp){
                    //删除旧数据并添加初始数据
                    $("#province").empty().append("<option value='0'>请选择...</option>");
                    // alert(resp);
                    $.each(resp,function (i,e){
                        $("#province").append("<option value='"+e.id+"'>"+e.name+"</option>")
                    })
                }
            })
        }
        function callback(resp){
            $("#city").empty().append("<option value='0'>请选择...</option>");
            $.each(resp,function (i,n){
                // alert(1)
                $("#city").append("<option value='"+n.id+"'>"+n.name+"</option>")
            })
        }
        $(function (){
            $("#btnload").click(function (){
                loadDataAjax();
            });
            $("#province").change(function (){
                // alert(111);
                var obj = $("#province>option:selected");
                // alert(obj.val()+"------>"+obj.text())
                var provinced = obj.val();
                $.post("queryCity",{proid:provinced},callback,"json")
            })
        })
    </script>
</head>
<body>
<p>省市级联查询</p>
<div>
    <table border="1" cellpadding="0" cellspacing="0">
        <tr>
            <td>省份名称：</td>
            <td>
                <select id="province">
                    <option value="0">请选择...</option>
                </select>
                <input type="button" value="load数据" id="btnload">
            </td>
        </tr>
        <tr>
            <td>城市：</td>
            <td>
                <select id="city">
                    <option value="0">请选择...</option>
                </select>
            </td>
        </tr>
    </table>
</div>
</body>
</html>
```

两个servlet：QueryCityServlet和QueryProvinceSerclet

```java
public class QueryCityServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String json = "{}";
        String id = req.getParameter("proid");
       if(id!=null && !"".equals(id.trim())){
           QueryDao dao = new QueryDao();
           List<City> cities = dao.queryCityList(Integer.valueOf(id));
           ObjectMapper om = new ObjectMapper();
           json = om.writeValueAsString(cities);
    }
        resp.setContentType("application/json;charset=utf-8");
        PrintWriter pw = resp.getWriter();
        pw.println(json);
        System.out.println(json);
        pw.flush();
        pw.close();
    }
}
```

```java
public class QueryProvinceServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String json = "{}";
        QueryDao dao = new QueryDao();
        List<Province> provinces=dao.querryProvinceList();
        if(provinces!=null){
            ObjectMapper om = new ObjectMapper();
            json = om.writeValueAsString(provinces);
        }
        resp.setContentType("application/json;charset=utf-8");
        PrintWriter pw = resp.getWriter();
        pw.println(json);
        System.out.println(json);
        pw.flush();
        pw.close();
    }
}
```

两个JDBC的dao层方法

```java
public class QueryDao {
    private Connection conn;
    private PreparedStatement pst;
    private ResultSet rs;
    private String sql;
    private String url="jdbc:mysql://localhost:3306/springdb";
    private String username="mcl";
    private String password="DNF321";
    public List<Province> querryProvinceList(){
        List<Province> provinces = new ArrayList<>();
        try {
            Province p =null;
            Class.forName("com.mysql.jdbc.Driver");
            conn = DriverManager.getConnection(url,username,password);
            sql="select id,name,jiancheng,shenghui from province order by id";
            pst = conn.prepareStatement(sql);
            rs = pst.executeQuery();
            while (rs.next()){
                p=new Province();
                p.setId(rs.getInt("id"));
                p.setName(rs.getString("name"));
                p.setJiancheng(rs.getString("jiancheng"));
                p.setShenghui(rs.getString("shenghui"));
                provinces.add(p);
            }
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        }finally {
                try {
                    if(rs!=null) rs.close();
                    if(pst!=null) pst.close();
                    if(conn!=null) conn.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }

        }
            return provinces;
    }
    public List<City> queryCityList(Integer provinceId){
        List<City> cities = new ArrayList<>();
        try {
            City c =null;
            Class.forName("com.mysql.jdbc.Driver");
            conn = DriverManager.getConnection(url,username,password);
            sql="select id,city from city where provinceid=?";
            pst = conn.prepareStatement(sql);
            pst.setInt(1,provinceId);
            rs = pst.executeQuery();
            while (rs.next()){
                c=new City();
                c.setId(rs.getInt("id"));
                c.setName(rs.getString("city"));
//                c.setProvinceid(rs.getInt("provinceid"));
                cities.add(c);
            }
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        }finally {
            try {
                if(rs!=null) rs.close();
                if(pst!=null) pst.close();
                if(conn!=null) conn.close();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            }
        }
        return cities;
    }
}
```

实体类：City和Province

```java
public class City {
    private Integer id;
    private String name;
    private Integer provinceid;
	...get/set方法
}
```

```
public class Province {
    private Integer id;
    private String name;
    private String jiancheng;
    private String shenghui;
	...get/set方法
}
```

数据库：由于是通过省份的id与城市的provinceid对应来确定城市属于那个省份，因此需要让province表的provinceid与省份id建立外键