# JQuery

jQuery兼容市面上主流的浏览器：IE，FireFox，Chrome

使用jQuery定位DOM对象的三种方式

- document.getElementById()---------->$("#id")

- document.getElementByClassName------>$(".class名")

- document.getElementByTagName-------->$("标签名“)

  对应了，通过id获取对象，通过class属性获取对象，通过标签名获取对象

jQuery中的对象选择器其实本质就是一种函数

**jQuery语法表示的对象是一个数组**，例如：var a = $("txt1");，这里面a就是jQuery对象，他是只有一个值的数组

例如：

```js
function $(id){
	var obj = document.getElementById(id);
	return obj;
}
var id = $("id");
/*
	其实这里就是相当于自己写了个fun类，然后就可以直接调用这个方法，$符号在html中可以作为函数名使用
*/
```

## 一个例子

```html
<script type="text/javascript" src="js/JQuery.js">	</script>
<script type="text/javascript">
    /*
    $(document),$符号是jQuery中的函数名称，document是函数的参数，作用是将document对象转化为jQuery函数库中可以使用的对象
    ready：是jQuery中的函数，当页面的DOM对象加载完成之后会执行ready函数的内容，ready相当于js中的onLoad事件
    */
    $(document).ready(function(){
        alert("ready?");
    })
</script>

```

上述是标准写法，以下是简单写法

```html
<script type="text/javascript">
    $(function(){
        alert("ready?");
    })
</script>
```

## DOM和jQuery

DOM对象和jQuery对象可以互相转化：

- DOM对象转化为jQuery对象：$(DOM对象)
- jQuery对象转化为DOM对象：从数组中获取第一个对象，第一个对象就是DOM对象，使用[0]或get[0]。（$(DOM对象)[0]或者.get[0]）

> jQuery和DOM对象为什么要互相转化？

​	DOM对象可以使用DOM对象的属性和方法，jQuery对象可以使用jQuery函数库中的方法。

## 选择器

一个字符串，用于定位dom对象的，定位了DOM对象就可以通过jQuery的函数操作DOM

### 基本选择器

> 常用的选择器

- id选择器，语法：$("#DOM对象的id值")，通过id找对象，此id是唯一值
- class选择器，语法：$(".class样式名")，class表示css中的样式，使用样式定位dom对象
- 标签选择器，语法：$("标签名称")，通过标签名称定位DOM对象

```html
<head>
		<meta charset="utf-8">
		<title></title>
		<script type="text/javascript" src="js/JQuery.js"></script>
		<script type="text/javascript">
            //id选择器
			function fun1(){
			var aa=$("#first");
			aa.css("background","blue");
			}
            //样式选择器
			function fun2(){
				var second = $(".second");
				second.css("background","green")
			}
            //标签选择器
			function fun3(){
				var third = $("div");
				third.css("background","yellow");
			}
            //所有选择器（全部的DOM）
            function fun4(){
				var all = $("*");
				all.css("background","purple")
			}
            //组合选择器
            function fun5(){
				var select = $("#first,span");
				select.css("background","orange")
			}
		</script>
		<style type="text/css">
		div{
			background:gray;
			width:200px;
			height: 100px;
		}
		.second{
			background: #332465;
		}
		</style>
<body>
	<div id="first">first:id选择器</div><br/>
	<div class="second">second：样式选择器</div><br/>
	<span>标签选择器</span><br/>
	<button onclick="fun1()">点我1</button><br/>
	<button onclick="fun2()">点我2</button><br/>
	<button onclick="fun3()">点我3</button><br/>
    <button onclick="fun4()">点我4</button><br/>
    <button onclick="fun5()">点我5</button>
</body>
```

### 表单选择器

在实际开发中，会用到很多表单（<form>），这些表单中会有很多其他的控件

- <input type="text" >
- <input type="password" >
- <input type="radio" >
- <input type="checkbox" >
- <input type="button" >
- <input type="file" >
- <input type="submit" >
- <input type="reset" >

这些标签的定位就叫做表单选择器，但即使这些标签不在form表单中仍然可以选择到。

这些选择器只能选type属性下的类型，类似<tr>这些是选不了的

> 语法：$(":type属性值")

例如：

```js
$(":text")	//选取所有单行文本狂
$(":password")  //选取所有密码框
$(":radio")	//选取所有单选框
$(":checkbox")	//选取所有复选框
```

## 过滤器

过滤器也是一个字符串，是对已经定位的dom对象再进行筛选。过滤器不能单独使用，只能和选择器一起使用。

选择器对于 $("div") == [dom1,dom2,dom3...]

### 表单过滤器

#### 语法

> **选择第一个first，只保留数组中第一个DOM对象**

​	语法：$("选择器：first")

> **选择最后一个last，只保留数组中最后一个DOM对象**

​	语法：$("选择器：last")

> **选择数组中指定对象**

​	语法：$("选择器：eq(数组索引)")

> **选择数组中小于（不等于）指定索引的所有DOM对象**

​	语法：$("选择器：lt(数组索引)")

> **选择数组中大于（不等于）指定索引的所有DOM对象**

​	语法：$("选择器：gt(数组索引)")

#### 实例

```html
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="js/JQuery.js"></script>
		<script type="text/javascript">
		$(function(){
            //选择第一个div
			$("#btn1").click(function(){
				$("div:first").css("background","blue")
			})
            //选择最后一个div
			$("#btn2").click(function(){
				$("div:last").css("background","orange")
			})
            //选择编号等于3的div
			$("#btn3").click(function(){
				$("div:eq(3)").css("background","yellow")
			})
            //选择编号小于3的div
			$("#btn4").click(function(){
				$("div:lt(3)").css("background","green")
			})
            //选择编号大于3的div
			$("#btn5").click(function(){
				$("div:gt(3)").css("background","purple")
			})
		})
		</script>
	</head>
	<body>
		<div id="one">div-0</div>
		<div id="two">div-1</div>
		<div>div-2
			<div>div-3</div>
			<div>div-4</div>
		</div>
		<div>div-5</div>
		<span>span</span><br>
		<input type="button" value="div-first" id="btn1"/>   <br>
		<input type="button" value="div-last" id="btn2"/>   <br>
		<input type="button" value="div-third" id="btn3"/>   <br>
		<input type="button" value="div<4" id="btn4"/>   <br>
		<input type="button" value="div>3" id="btn5"/>	 <br>
	</body>
</html>

```

### 表单属性过滤器

根据表单中dom对象的状态情况，定位dom对象

三种状态：

- 启用状态：enabled
- 不可用状态：disabled
- 选择状态：checked，例如radio，checkbox

#### 语法

> **选择可用文本框**

​	语法：$( " : text : enabled " )

> **选择不可用文本框**

​	语法：$( " : text : disabled " )

> **复选框选中元素**

​	语法：$( " : checkbox : checked " )

> **选择下拉列表的被选中元素**

​	语法：$( " select>option:selected " )

#### 实例

```html
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="js/JQuery.js"></script>
		<script type="text/javascript">
		$(function(){
            //选择可用文本框enabled
			$("#btn1").click(function(){
				var text1 = $(":text:enabled");
				text1.css("background","yellow");
				text1.val("hello")
			});
            //选择不可用文本框disabled
			$("#btn2").click(function(){
				var text2 = $(":text:disabled");
				text2.css("background","blue");
				text2.val("world")
			});
            //选择已选复选框$(":checkbox:checked")
			$("#btn3").click(function(){
				var check = $(":checkbox:checked");
				for (var i = 0; i < check.length; i++) {
					alert(check[i].value)
				}
			});
            //选择已选选择框内容$("select>option:selected");
			$("#btn4").click(function(){
				var select1 = $("select>option:selected");
				alert(select1.val());
			});
		})
		</script>
	</head>
	<body>
		<p>文本框</p>
		<input type="text" id="text1" value="text1"/>         <br>
		<input type="text" id="text2" value="text2" disabled/><br>
		<input type="text" id="text3" value="text3"/>         <br>
		<input type="text" id="text4" value="text4" disabled/><br>
		<p>复选框</p>
		<input type="checkbox" value="basketball" />篮球<br>
		<input type="checkbox" value="football" />足球<br>
		<input type="checkbox" value="volleyball" />排球<br>
		<p>选择框</p>
		<select id="selector">
			<option value="java">java</option>
			<option value="c" selected>C</option>
			<option value="python">python</option>
		</select>
		<p><input type="button" id="btn1" value="选择可用文本框"></p>
		<p><input type="button" id="btn2" value="选择不可用文本框"></p>
		<p><input type="button" id="btn3" value="选择已选复选框"></p>
		<p><input type="button" id="btn4" value="选择已选选择框"></p>
	</body>
</html>
```

## 函数

### 第一组函数

> **val**

操作数组中DOM对象的value属性

- $( 选择器 ).val( )：无参调用形式，读取数组中第一个DOM对象的value属性值
- $( 选择器 ).val( 值 )：有参数调用，对数组所有DOM对象的value属性值进行统一赋值

> **text**

操作数组中所有DOM对象的文字显示内容（<p>text</p>）

- $( 选择器 ).text( )：无参调用形式，读取数组中所有DOM对象的文字显示内容，将得到的内容拼接为一个字符串返回
- $( 选择器 ).text( 值 )：有参调用形式，对数组中所有DOM对象的文字显示内容进行统一赋值

> **attr**

对val，text之外的所有其他属性操作

- $( 选择器 ).attr( " 属性名 " )：获取DOM数组第一个对象的属性值
- $( 选择器 ).attr( " 属性名 "，" 值 " )：对数组中所有DOM对象的属性设置为新值

实例演示

```html
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="js/JQuery.js"></script>
		<style type="text/css">
			div{
				background:blue;
			}
			img{
				weight:200px;
				height: 200px;
			}
		</style>
		<script type="text/javascript">
			$(function(){
                //获取值
				$("#btn1").click(function(){
					alert($(":text").val());
				})
                //修改全部值
				$("#btn2").click(function(){
					$(":text").val("数据结构真的很难");
				})
                //获取全部text
				$("#btn3").click(function(){
					alert($("div").text());
				})
                //修改全部text
				$("#btn4").click(function(){
					$("div").text("div*3");
				})
                //获取对象属性
				$("#btn5").click(function(){
					alert($("#img1").attr("src"));
				})
                //修改对象属性
				$("#btn6").click(function(){
					$("#img1").attr('src','pic/生活照.jpg')
				})
			})
		</script>
	</head>
	<body>
	<input type="text" value="刘备" />  <br>
	<input type="text" value="关羽" />  <br>
	<input type="text" value="张飞" />  <br>
	 <div>我是第一个div</div>
	 <div>我是第二个div</div>
	 <div>我是第三个div</div>
	 <img src="pic/学生证.jpg" id="img1"/><br>
	<input type="button" id="btn1" value="获取DOM值--val()"/> <br>
	<input type="button" id="btn2" value="修改DOM值--val(值)"/> <br>
	<input type="button" id="btn3" value="获取DOM文本--text()"/> <br>
	<input type="button" id="btn4" value="修改DOM文本--text(值)"/> <br>
	<input type="button" id="btn5" value="获取DOM属性值--attr('src')"/> <br>
	<input type="button" id="btn6" value="修改DOM属性值--attr('background')"/> <br>
    </body>
</html>
```

### 第二组函数

> **remove**

​	语法：$( 选择器 ).remove( )：将函数中所有DOM对象及其子对象一并删除

> **empty**

​	语法：$( 选择器 ).empty( )：将函数中所有的DOM对象的子对象删除

> **append**

​	语法：$( 选择器 ).append( " <div>动态添加的div</div> " )：为数组中所有DOM对象添加子对象	

> **html**

​	设置或者返回被选元素的内容（innerHTML），这个返回的是整个html元素，包括标签自身以及其属性以及标签里的内容，但不包括被选择的对象本身

​	注意与text()区分，text只返回所有被标签包含的内容

​	语法：

- $( 选择器 ).html( )：无参调用方法，获取DOM数组第一个元素的内容
- $( 选择器 ).html( 值 )：有参调用，用于设置DOM数组中所有元素的内容

> **each**

each可以对数组，json，dom数组循环处理。数组，json中的每个成员都会调用一次处理函数。

var arr = [1,2,3]
var json = { "name" : "zhangsan","age" : 20 }
var obj = $( " :text " );

语法

- $.each( 循环的内容 ，处理函数 )：表示使用jQuery的each，循环数组每个数组成员都会执行一次后面的”处理函数“
  处理函数：function( index , emelent ) ，index和element都是自定义的形参，名称自定义

  - index：循环的索引
  - element：数组中的成员

  原来在js中，循环需要写fori，然后再在循环体中写处理函数，现在都可以省略，index就是相当于循环变量，element就是数组成员

  

实例

```html
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="js/JQuery.js"></script>
		<script type="text/javascript">
			$(function(){
				//remove
				$("#btn1").click(function(){
					$("select").remove();
				})
				//empty
				$("#btn2").click(function(){
					$("select").empty();
				})
				//append
				$("#btn3").click(function(){
					$("select").append("<option>新添加的</option>");
				})
				//html
				$("#btn4").click(function(){
					alert($("div").html());
				})
				$("#btn5").click(function(){
					$("div").html("new div<br>new div<br>new div<br>new div<br>");
				})
				//each
					//静态数组
				$("#btn6").click(function(){
					var arr=[1,2,3,4];
					$.each(arr,function(index,element){//function(i,e)也可
						alert(index+"--------------->"+element);
					})
				})
					//json数组
				$("#btn7").click(function(){
					var json = { "name" : "zhangsan","age" : 20 }
					$.each(json,function(i,e){//function(i,e)也可
						alert("key="+i+"--------------->"+"value="+e );
					})
				})
					//dom数组
				$("#btn8").click(function(){
					var dobj = $("select").children();
					$.each(dobj,function(i,e){//function(i,e)也可
						alert(i+"--------------->"+e.value);
					})
				})
					//jQuery数组
				$("#btn9").click(function(){
					/* 
					 $(":text").each(function(i,e){
						 alert("i:"+i+","+"e:"+e.value);
					 })
					 */
					$("select").children().each(function(i,e){//function(i,e)也可
						alert(i+"--------------->"+e.value);
					})
				})
			})
		</script>
	</head>
	<body>
	<select>
		<option value="tiger">老虎</option>
		<option value="lion">狮子</option>
		<option value="cat">猫</option>
	</select><br>
	<select>
		<option value="asion">亚洲</option>
		<option value="afica">非洲</option>
		<option value="america">美洲</option>
	</select><br>
	<div id="father">
		div-father
	</div>
	<input type="button" id="btn1" value="删除父对象和子对象"/> <br>
	<input type="button" id="btn2" value="删除父对象的子对象"/> <br>
	<input type="button" id="btn3" value="添加子对象"/> <br>
	<input type="button" id="btn4" value="返回元素内容"/> <br>
	<input type="button" id="btn5" value="修改元素内容"/> <br>
	<input type="button" id="btn6" value="静态数组循环遍历"/> <br>
	<input type="button" id="btn7" value="json数组循环遍历"/> <br>
	<input type="button" id="btn8" value="dom数组循环遍历"/> <br>
	<input type="button" id="btn9" value="jquery对象数组循环遍历"/> <br>
</body>
</html>
```

## 事件

### 普通事件

语法：$（选择器）.监听事件名称（处理函数）

例：

```js
$("#btn").click(function(){
	alert("btn单击了");
});
```

### on事件

语法：$( 选择器 ).on( 事件名称，事件处理函数 )

监听事件的名称是js事件去掉on之后的内容，例如js的onclick的jQuery事件就是click

实例

```html
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<script src="js/JQuery.js"></script>
		<script type="text/javascript">
			$(function(){
				$("#btn1").click(function(){
					$("div").append("<input id='text1' type='text' value='新的按钮' />")
					// alert("新按钮被创建了")
				})
                $("#btn2").click(function(){
                    // document.getElementById("text1").value="????"
                    $("#text1").val("修改了文本框的内容")
                })
			});
		</script>
		<style type="text/css">
			div{
				background: #6495ED;
			}
		</style>
	</head>
	<body>
		<div>
			这个div需要一个按钮<br>
		</div>
			<button id="btn1">在div中创建一个button按钮</button><br>
			<button id="btn2">改变新创建的text内容</button>
	</body>
</html>
```

