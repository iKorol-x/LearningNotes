# 1、JavaScript

	JavaScript语言是在10天时间内设计出来的，虽然语言的设计者水平非常NB，但谁也架不住“时间紧，任务重”，所以，JavaScript有很多设计缺陷，我们后面会慢慢讲到。
	
	此外，由于JavaScript的标准——ECMAScript在不断发展，最新版ECMAScript 6标准（简称ES6）已经在2015年6月正式发布了，所以，讲到JavaScript的版本，实际上就是说它实现了ECMAScript标准的哪个版本。
	
	由于浏览器在发布时就确定了JavaScript的版本，加上很多用户还在使用IE6这种古老的浏览器，这就导致你在写JavaScript的时候，要照顾一下老用户，不能一上来就用最新的ES6标准写，否则，老用户的浏览器是无法运行新版本的JavaScript代码的。

# 2、快速入门

## 2.1、引入JavaScript

**内部标签引用**

```html
<script>
	//...
</script>
```

**外部引入**

	fir.js

```javascript
alert("Hello JavaScript")
```

	test.html

```html
<script src="fir.js"></script>
```

### 2.2、基本语法入门

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>


```javascript
// 1、定义变量
var score = 75;

// 2、条件控制
if (score>60 && score<70){
    alert("60~70");
}else if (score>70 && score<80){
    alert("70~80");
}else{
    alert("other");
};

//在控制台打印变量
console.log(score)
```

### 2.3数据类型

**变量**

```javascript
var 
```

**number**

	js不区分整数和小数

```html
123	   //整数123
123.1  //浮点数
123.11.123e3   //科学计数法
-99	  //复数
NaN  // not a number
Infinity //表示无限大
```

**字符串**

```
‘abc’ “abc”
```

**布尔值**

```
true false
```

**逻辑运算**

```
&&
||
!
```

**比较远算符（重要）**

```
= 		 赋值
==   	 等于（类型不一样，值一样，也会判断为true）	1 和 "1"的值
===      绝对等于（类型和值都一样，结果为true）
```

	这是JS的一个缺陷，坚持不要使用 **==** 比较

**null和undefined**

```
null 空
undefined 未定义
```

**数组**

```js
var arr = [1.2,3,"Hello",null,true]
```

	取数组下标:如果越界了，就会undefined

**对象**

	对象是大括号，数组是中括号

```js
var person = {
    name:"zhangfeng",
    age:"3",
    tags:['js','java']
}	
```

取对象的值

```
person. name
> "qinjiang"
person.age
> 3
```

## 2.4、严格检查模式

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210718203236744.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTUzNTg3Mw==,size_16,color_FFFFFF,t_70#pic_center)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--
    前提: IEDA需要设置支持ES6语法 'use strict';
    严格检查模式，预防javascript的随意性导致产生的一些问题
    必须写在Javascript的第一行!
    局部变量建议都使用let去定义-->
    <script>
        'use strict'
        // 1、定义变量
         let score = 75;

        // 2、条件控制
        if (score>60 && score<70){
            alert("60~70");
        }else if (score>70 && score<80){
            alert("70~80");
        }else{
            alert("other");
        };

        //在控制台打印变量
        console.log(score)
    </script>
</head>
<body>

</body>
</html>
```

# 3、数据类型

## 3.1、字符串

	1、正常字符串我们使用单引号，或者双引号包裹
	
	2、注意转义字符 \

```
\'
\n
\t
\u4e2d   \u#### Unicode字符
\x41    Ascll字符
```

**3、多行字符串编写（重点）**

```js
//tab 上面   esc键下面 `
var msg =`he11o
  		wor1d
		你好`
```

**4、模板字符串**

```javascript
let name = "qinjiang";
let age = 3;
let msg =`你好呀，${name}`
```

**5、字符串长度**

```js
let str = "student"
console.log(str.length)
>7
```

**6、字符串的可变性为不可变**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210718203226213.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTUzNTg3Mw==,size_16,color_FFFFFF,t_70#pic_center)

**7、大小写转换**

```js
//注意这里是方法，不是属性
let stu = "student";
stu.toUpperCase();
stu.toLowerCase();
```

**8、字符串截取（重点）**

	substring左包括，右不包括 [)

```js
let stu = "student";
stu.substring(1)//从第一个字符串截取到最后一个字符串
stu.sunstring(1,3) //tu
```

## 3.2、数组

**Array可以包含任意的数据类型**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210718203211397.png#pic_center)

	1、长度
	
	2、indexOf,通过元素获得下标索引
	
	3、截取字段 slice（）

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210718203202879.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTUzNTg3Mw==,size_16,color_FFFFFF,t_70#pic_center)

4、尾部加/减 push() / pop()![在这里插入图片描述](https://img-blog.csdnimg.cn/20210718203154377.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTUzNTg3Mw==,size_16,color_FFFFFF,t_70#pic_center)

5、头部加减 unshift() / shift（）![在这里插入图片描述](https://img-blog.csdnimg.cn/20210718203146716.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTUzNTg3Mw==,size_16,color_FFFFFF,t_70#pic_center)

6、排序srot（）

```js
let arr = ['B','A','C']
arr.sort()
['A','B','C']
```

7、元素反转 reverse()

```js
let arr = ['A','B','C']
arr.reverse()
['C','B','A']
```

8、数组拼接 concat()

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210718203134205.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTUzNTg3Mw==,size_16,color_FFFFFF,t_70#pic_center)

9、连接符join()，数组转化为字符串

```js
let arr = ['A','B','C']
arr.join("-")
"C-B-A"
```

10、split()，字符串转化为数组

```js
let s = '1,2,3,4,5,6'
let arr2 = s.split(',')
console.log(arr2)
```

## 3.3、对象

**JavaScript中的所有的键都是字符串，值是任意的对象**

	若干个键值对

```js
var people = {
	name:"kuangshen",
    email:"123e21@qq.com"
	age:3
}
```

	1、对象赋值

```js
person.name = "zhanga"
person.age = 5	
```

2、使用一个不存在的对象属性不会报错

```javascript
>person.haha
>undefined
```

3、动态的删减属性,通过delete删除对象属性

```javascript
delete person.name
```

4、动态的增加属性，直接给新的属性添加值即可

```javascript
person.haha = "hahaa"
```

5、判断属性值是否在这个对象中 xx in xx

```javascript
'age' in person
true
//继承于父类
'toString' in person
true
```

6、判断一个属性是否是这个对象自生拥有的hasOwnProperty()

```javascript
person.hasOwnProperty('toString')
flase
person.hasOwnProperty('name')
true
```

7、遍历对象

```js
//遍历对象
for(let key in people){
    console.log(key+":"+people[key])
}
```

## 3.4、流程控制

**forEach循环**


```html
 <script>
     'use strict'
    let arr = [1,2,41,67,2345,12];
    //函数
    arr.forEach(function (value){
        console.log(value)
    })
</script>
```

**for···in**

```html
<script>
    'use strict'
    let arr = [1,2,41,67,2345,12];
    for(let num in arr){
        console.log(arr[num])
    }
</script>
//结果
1 
2
41 
67 
2345 
12
```

**for···of**

```js
let arr = [1,2,41,67,2345,12];
for(let num of arr){
    console.log(num)
}
//结果
1 
2
41 
67 
2345 
12
```

## 3.5、Map和Set

	**Map**

```js
let map = new Map([['tom',12],['jack',90],['haha',21]])
let name = map.get('tom')   //通过key获得value
map.set('admin',1221);      //新增或修改
map.delete('tom');          //删除
```

	**Set：无序不重复**

```html
<script>
    'use strict'
    let set = new Set([12,42,54])
    set.add(2)      // 添加
    set.delete(12)  //删除
    console.log(set.has(54))    //是否包含某个元素
</script>
```

## 3.6、itertor

**遍历数组**

```javascript
let arr = [1,2,41,67,2345,12];
for(let num of arr){
    console.log(num)
}
//结果
1 
2
41 
67 
2345 
12
```

**遍历map**

```javascript
let map = new Map([['tom',21],['jack',121]])
for(let x of map){
    console.log(x)
}

//结果
Array [ "tom", 21 ]
Array [ "jack", 121 ]
```

**遍历Set**

```javascript
let ser = new Set([1,2,412,3])
for(let x of set){
    console.log(x)
}

//结果
1 
2 
412 
3
```

# 4、函数

## 4.1、定义函数

**定义方式一**

```js
function abs(x){
            if (x>0){
                return x;
            }else {
                return -x;
            }
        };
```

—旦执行到return代表函数结束，返回结果!

如果没有执行return，函数执行完也会返回结果，结果就是undefined

**定义方式二**

```js
let abs = function(x){
            if (x>0){
                return x;
            }else {
                return -x;
            }
        };
```

function(x)t…} 这是一个匿名函数。但是可以把结果赋值给abs，通过abs就可以调用函数!

按照完整语法需要在函数体末尾加一个`;`，表示赋值语句结束。

**方式一和方式二等价**

调用函数

```js
abs(10); // 返回10
abs(-9); // 返回9
```

由于JavaScript允许传入任意个参数而不影响调用，因此传入的参数比定义的参数多也没有问题，传入的参数比定义的少也没有问题

```js
abs(10, 'blablabla'); // 返回10
abs(-9, 'haha', 'hehe', null); // 返回9
abs(); // 返回NaN
```

**arguments**

`arguments`是一个JS免费赠送的参数

它代表的是传递进来的所有参数是一个数组，他可以获取所有传递进来的参数

```js
let abs = function(x){
    for (let i = 0; i < arguments.length; i++) {
        console.log(arguments[i])
    }
};

//结果
1
21     
2134   
1234   
12     
3      
12
```

**rest**

以前：获取除了已经定义了的参数以外的所有参数

```js
let abs = function(x){
    if (arguments.length>1){
        for (let i = 1; i < arguments.length; i++) {
            console.log(arguments[i])
        }
    }
    if
};

//abs(1,123,12,43,21)
//结果
123
12
43
21
```

ES6新特性，获取除了已经定义了的参数意外的所有参数

```js
let abs = function(a,b,...rest){
    console.log("a=>"+a)
    console.log("b=>"+b)
    console.log("rest=>"+rest)
};

//结果
abs(12,123,1,233,123,12,3,12)
a=>12 
b=>123 
rest=>1,233,123,12,3,12
```

`rest`参数只能写在最后面，必须用…表识别

## 4.2、变量作用域

在javascript中, var定义变量实际是有作用域的。

假设在函数体中声明，则在函数体外不可以使用~

```js
function zzf(){
    var x = 1;
    x = x + 1;
}
x = x +2;	//Uncaught ReferenceErroe:x is not defined
```

内部函数可以访问外部函数的成员，反之则不行

```js
function qj( i
	var x = 1;//内部函数可以访问外部函数的成员，反之则不行
    function qj2( {
		var y =x +1;	// 2
	}
	var z =y + 1;// Uncaught ReferenceError: y is not defined
}
```

假设，内部函数变量和外部函数的变量，重名!

```js
function qjo {
	var x = 1;
	function qj2( {
		var x = 'A';
		console.log('inner:'+x);// outer:1
	}
	conso1e.1og( ' outer:'+x); //inner:A
    qj2()
}
qj()
```

假设在JavaScript中函数查找变量从自身函数开始~，由"内”向“外”查找.假设外部存在这个同名的函数变量，则内部函数会屏蔽外部函数的变量。

**提升变量作用域**

```js
function zzf(){
    var x = 'x' + y;
    console.log(x)
    var y = 'y';
}
//结果
 x undefined
```

说明：js执行引擎，自动提升了y的声明，但是不会提升变量y的赋值，函数zzf()==**等价于**==zzf2()

```js
function zzf(){
    var y;
    var x = 'x' +y;
    console.log(x);
    y = 'y';
}
```

这个是在JavaScript建立之初就存在的特性。**养成规范**:所有的变量定义都放在函数的头部，不要乱放，便于代码维护;

```js
function qj2( {
    var x = 1,
    y = × + 1,z,i,a;  //undefined
    //之后随意用
}
```

**全局作用域的变量**

```js
//全局变量
x = 1;
function f(){
    console.log(x);	//1
}
f()
console.log(x)	//1
1
```

**全局对象 window**

```html
<scrtipt>
	let x = '1';
	console.log(window.x);
</script>
//结果
1
```

**规范**

由于我们所有的全局变量都会绑定到我们的window 上。如果不同的js文件，使用了相同的全局变量，冲突~>如果能够减少冲突?

```js
//唯一全局变量
let ks ={};
//定义全局变量
ks.name = "mcl";
ks.add=function (a,b){
    return a+b;
}
console.log(ks.name);
ks.add(1,3)
4
```

把自己的代码全部放入自己定义的唯一空间名字中，降低全局命名冲突的问题~

**局部作用域**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210718203123422.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTUzNTg3Mw==,size_16,color_FFFFFF,t_70#pic_center)

**常量 const**

ES6之前，怎么定义常量：只有用全部大写字母命名的变量就是常量;

```js
var PI = '3.14';
//规范，默认全部使用大写命名的变量就是常量
```

ES6引入了常量关键字`const`

```js
const PI = '3.14'
```

## 4.3、方法

**定义方法**

方法就是把函数放在对象的里面，对象只有两个东西:属性和方法

```js
  let student={
    name : "mcl",
    birth : 1999,
    sex : "男",
    age : function (){
      let date=new Date();
      return date.getFullYear()-this.birth+1;
    }
  }
//属性
zhang.name
//方法,一定要带()
zhang.age()
```

**this.代表什么？**

```js
function getAge(){
        //今年-出生的年
       let now =  new Date().getFullYear()
        return now - this.birth;
    }
let zhang = {
    name:'张',
    birth:2000,
    age:getAge
};
//zhang.age()  OK
//getAge()   NaN  this指向window，不存在getAge()函数
```

**apply**

```js
function getAge(){
        //今年-出生的年
       let now =  new Date().getFullYear()
        return now - this.birth;
    }

let zhang = {
    name:'张',
    birth:2000,
    age:getAge
};
//zhang.age()    OK
//this指向了zhang，参数为空
getAge.apply(zhang,[]);
```

# 5、内部对象

## 5.1、Date

**基本使用**

```js
let date = new Date();
date.getFullYear(); //年
date.getMonth();    //月 0~11
date.getDate();     //日
date.getDay();      //星期几
date.getHours();    //时
date.getMinutes();  //分
date.getSeconds();    //秒
date.getTime();     //时间戳 毫秒数
```

**转换**

```js
//时间戳转换为本地时间
console.log(new Date(121232121231321));
Date Wed Sep 11 5811 21:13:51 GMT+0800 (中国标准时间)
date.toLocaleString()
"2021/4/13 下午7:28:31"
date.toLocaleDateString()
"2021/4/13"
```

**计时事件**

在一个设定的时间间隔之后来执行代码，而不是在函数被调用后立即执行。我们称之为计时事件。计时事件两个关键方法是:

- **setInterval()** - 间隔指定的毫秒数不停地执行指定的代码。
- **clearInterval()** - 方法用于停止 setInterval() 方法执行的函数代码。
- **setTimeout()** - 在指定的毫秒数后执行指定代码。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210718203110599.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTUzNTg3Mw==,size_16,color_FFFFFF,t_70#pic_center)

**clearInterval()用法**

```js
  let i=1;
  function show(){
    console.log(i++ + "s间隔");
    if(i>3){
      clearInterval(cir);
    }
  }
  let cir = setInterval(show,1000);
```

**计时器**

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<body>
		<input type="button" value="开始" onclick="delay()"/>
		&nbsp;&nbsp;&nbsp;
		<input type="button" value="停止" onclick="stop()"/>
		<p id="show"></p>
	</body>
</html>
<script>
//setimeout()-- 延时调用方法
//setInterval()-- 间隔一段时间重复调用方法
//clearInterval()--方法用于停止 setInterval() 方法执行的函数代码

function getDateTime(){
	//获取当前的时间
	var date=new Date();
	//取时间戳
	//var timestram=date.getTime();
	var year=date.getFullYear();//年
	var month=date.getMonth()+1;//月
	var day=date.getDate()//号
	var hours=date.getHours();//时
	var min=date.getMinutes();//分
	var second=date.getSeconds();//秒
	var time=year+'-'+(month)+'-'+val2(day)+' '
				+val2(hours)+':'+val2(min)+':'+val2(second);
	document.getElementById('show').innerHTML=time;
}
var c;
//延时调用
function delay(){
	//setTimeout('getDateTime()',1000);//单位：毫秒
	c=setInterval('getDateTime()',1000);
}
//停止调用
function stop(){
	clearInterval(c);
}
function val2(num){
	if(num<10){
		num='0'+num;
	}
	return num;
}
</script>

```

## 5.2、JSON

**json是什么**

- JSON(JavaScript Object Notation,JS对象简谱)是一种轻量级的数据交换格式。
- 简洁和清晰的**层次结构**使得JSON成为理想的数据交换语言。
- 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。

在JavaScript 一切皆为对象、任何js支持的类型都可以用JSON来表示;

**格式**

- 对象都用{}
- 数组都用[]
- 所有的键值对都使用key:value

**JSON和JS的转换**

```js
let user = {
    name:'zhang',
    age:3,
    sex:'男'
};

//对象转换为JSON字符串
let jsonUser = JSON.stringify(user)
console.log(jsonUser);      //{"name":"zhang","age":3,"sex":"男"}

//JSON字符串转换为对象，参数为JSON字符串
let json = JSON.parse('{"name":"zhang","age":3,"sex":"男"}');
console.log(json);    //Object { name: "zhang", age: 3, sex: "男" }
```

**JSON和JS区别**

```js
var obj = {a:'hello',b:'springmvc'}
var json = {"a":"hello","b":"springmvc"}
```

# 6、面向对象编程

**class**

```js
<script>
    'use strict'
class Student {
    //有参构造
   constructor(name,sex) {
      this.name = name;
      this.sex = sex;
    }
    //方法
   run(){
      console.log(this.name+" is running");
      console.log("sex----->"+this.sex);
    }
}
let mcl = new Student('mcl','男');
let mcl1 = new Student('mcl1','女');
mcl.run()
mcl1.run()
</script>
```

**继承**

```js
<script>
    'use strict'
    class NewStudent extends  Student{
    constructor(name,sex,age) {
      //有参构造的super中需要写具体的构造属性，不然就是undefined
      super(name,sex);
      this.age=age;
    }
    fly(){
      console.log(this.name+" is flying");
      console.log("sex----->"+this.sex);
      console.log("age----->"+this.age);
    }
  }
  let mcl2 = new NewStudent('mcl2','男',24);
</script>
```

# 7、BOM对象

**浏览器介绍**

JS的诞生就是为了能够让他在浏览器中运行

BOM：浏览器对象模型

- IE
- Chrome
- Safari
- FireFox

**window（重要）**

window代表浏览器窗口

```js
//浏览器外部高度
window.outerHeight
804
//浏览器外部宽度
window.outerWidth
699
//浏览器内部高度
window.innerHeight
101
//浏览器内部宽度
window.innerWidth
688
```

**navigator**

navigator，封装了浏览器的信息

```js
navigator.appName 
"Netscape" 

navigator.userAgent 
"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/92.0.4515.159 Safari/537.36"

navigator.platform 
"Win32" 
```

大多数时候，我们不会使用navigator对象，因为会被人为修改!

不建议使用这些属性来判断和编写代码

**screen**

```json
screen.width
1536 px
screen.height
864 px
```

**location（重要）**

```json
host:"www.baidu.com"  //主机
href:"https://www.baidu.com/"  //当前指向的位置
protocol:"https:"  //协议
reload:f reload()	//刷新页面
location.assign("https://www.qq.com")   //转向新的地址

```

**document**

document代表当前页面

```js
document.title
"百度一下，你就知道"
document.title='mcl'
"mcl"
```

- 获取具体的文档树节点

```html
<dl id = "app">
    <dt>JAVA</dt>
    <dd>javaSE</dd>
    <dd>javaEE</dd>
</dl>
<script>
	let dl = document.getElementById("app");
</script>
```

- 获取cookie

```html
document.cookie
```

劫持cookie原理

```html
<script src = "劫持js"></script>
恶意人员的页面，它引入一个js文件，里面有获取你浏览器的cookie的js代码，然后发送到他的服务器
```

**服务端可以设置cookie：httpOnly,这样比较安全**

**history**

history代表浏览器的历史记录

```js
history.back()  //返回前一个页面
history.forward()  //前进
```

# 8、DOM对象

**核心**

浏览器网页就是一个Dom树形结构!

- 更新:更新Dom节点
- 遍历dom节点:得到Dom节点
- 删除:删除一个Dom节点
- 添加:添加一个新的节点

要操作一个Dom节点，就必须要先获得这个Dom节点

**获得Dom节点**

```html
 <script>
        'use strict'
        let h1 = document.getElementsByTagName('h1')
        let p1 = document.getElementById('p1')
        let p2 = document.getElementsByClassName('p2')
        
        let father = document.getElementById('father')
        let childrens = father.children//获取父节点下的所有子节点
        father.firstChild
        father.lastChild
    </script>
<div>
    <h1>标题一</h1>
    <p id="p1">p1</p>
    <p class="p2">p2</p>
    
</div>
```

**更新节点**

```html
<div id="id1">
    
</div>
<script>
	let id1 = document.getElementById('id1')
</script>
```

**操作文本**

- `id1.innerText = '123'` 修改文本的值
- `id1.innerHTML = '<strong>123</strong>'`可以解析HTMl文本

**操作JS**

```js
id1.style.color = 'yellow';  //属性使用字符串包裹
id1.style.fontSize = '20px';
id1.style.padding = '1em';
```

**删除节点**

```html
<div id="father">
	<h1>标题一</h1>
    <p id="p1">p1</p>
    <p class="p2">p2</p>
</div>
<script>
	var self = document. getE1ementById( 'p1');
    var father = p1.parentElement;
	father.removechild(self)
    
    //删除是一个动态的过程;
	father.removechild(father.children[0])
    father.removechild(father.children[1])
    father.removechild(father.children[2])
 
</script>
```

注意:删除多个节点的时候,children是在时刻变化的，删除节点的时候一定要注意!

**插入节点**

	**追加**

```html
<p id="js">JS</p>
<div id="list">
    <p id="se">JavaSE</p>
    <p id="ee">JavaEE</p>
    <p id="me">JavaME</p>
</div>
<script>
    let js = document.getElementById('js');
    let list = document.getElementById('list');
    list.appendChild(js);   //追加到后面
</script>
```

	**效果：**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210718203049170.png#pic_center)

**创建一个新的标签，实现插入**

```html
<script>
    let list = document.getElementById('list');
    //通过JS创建一个新的节点
    let newP = document.createElement('p');//创建一个p标签
    newP.id = 'newP';
    newP.innerText = 'Hello kuang';
    list.appendChild(newP);
	
    //创建一个标签节点并插入
    let myScript = document.creatElement('script');
    myScript.setAttribute('type','text/javascript');
    list.appendChild(myScript);

</script>
```

**insertBefore**

```js
let ee = document.getElementById('ee');
let js = document.getElementById('js');
let list = document.getElementById('list');
list.insertBefore(js,ee);
```

# 9、操作表单（md5加密验证）

**表单**

- 文本框 text
- 下拉框
- 单选框 radio
- 多选框 checkboox
- 隐藏域 hidden
- 密码框 password
- …

表单的目的：提交信息

**获得要提交的信息**

```html
<form action="post">
    <span>用户名：</span>
    <input type="text" id="username"><br/>

    <span>性别：</span>
    <input type="radio" id="boy" name="sex" value="man">男
    <input type="radio" id="girl" name="sex" value="woman">女

</form>

<script>
    let input_text = document.getElementById('username')

    //获取输入框的值
    input_text.value;
    //修改输入框的值
    input_text.value = '123';

    let boy_radio =  document.getElementById('boy');
    let girl_radio =  document.getElementById('girl');
    boy_radio.checked;  //查看返回结果，如果为true，则被选中
    girl_radio.checked = true;  //赋值
    
</script>
```

**提交表单，md5加密，表单优化**

```html
<script src="https://cdn.bootcss.com/blueimp-md5/2.10.0/js/md5.js"></script>
```

```html
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    //引入md5加密的js文件
     <script src="js/md5.js"></script>

</head>
<body>

<!--表单绑定提交事件-->
<form action="https://www.baidu.com/" method="post" onsubmit="return check()" >
    用户名：<input type="text" id="username" name="username"><br/>
    密码：<input type="password" id="input-password" ><br/>
    <!--加密-->
    <input type="hidden" id="md5-password" name="password">

    <input type="submit" value="提交">

</form>

<script>
    function check(){
        let uname = document.getElementById('username');
        let pwd = document.getElementById('input-password');
        let md5pwd = document.getElementById('md5-password');
        md5pwd.value = md5(pwd.value);
        //可以校验表单的内容，true就是通过提交，false就是阻止提交
        return true;
    }
</script>
```

# 10、jQuery

javaScript和jQuery的关系？

jQuery库，里面存在大量的JavaScript函数

**公式：$(selector).action()**

```html
<!DOCTYPE html>
<html lang = "en">
    <head>
        <meta charset = "UTF-8">
        <title>Title</title>
        <script src="lib/jquery-3.4.1.js"></script>
    </head>
    <body>
        <a href="" id = "test-jquery">点我</a>
        <script>
        	//选择器就是css选择器,#+id
            $('#test-jquery').click(function(){
                alert('hello,jQuery!');
            });
        </script>
    </body>
</html>
```

**选择器**

```js
//原生js，选择器少，麻烦不好记
//标签
document.getElementByTagName();
//id
document.getElementById();
//class
document.getElementByClassName();

//jQuery css中的选择器它全部都能用！
$('p').click();//标签选择器
$('#id1').click();//id选择器
$('.class1').click;//class选择器
$(":type 'text'")    
$(":text")//选取所有单行文本
//
```

文档工具站：https://jquery.cuishifeng.cn/

**事件**

鼠标事件、键盘事件，其他事件

```js
mousedown()(jQuery)----按下
mouseenter()(jQuery)
mouseleave()(jQuery)
mousemove()(jQuery)----移动
mouseout()(jQuery)
mouseover()(jQuery)
mouseup()(jQuery)
```

```html
<!DOCTYPE html>
<html lang = "en">
    <head>
        <meta charset = "UTF-8">
        <title>Title</title>
        <script src="lib/jquery-3.4.1.js"></script>
        <style>
            #divMove{
                width:500px;
                height:500px;
                border:1px solid red;
            }
        </style>
    </head>
    <body>
        <!--要求：获取鼠标当前的一个坐标-->
        mouse：<span id = "mouseMove"></span>
        <div id = "divMove">
            在这里移动鼠标试试
        </div>
        <script>
        	//当网页元素加载完毕之后，响应事件
            //$(document).ready(function(){})
            $(function(){
                $('#divMove').mousemove(function(e){
                    $('#mouseMove').text('x:'+e.pageX+"y:"+e.pageY)
                })
            });
        </script>
    </body>
</html>

```

**操作DOM**

节点文本操作

```js
$('#test-ul li[name=python]').text();//获得值
$('#test-ul li[name=python]').text('设置值');//设置值
$('#test-ul').html();//获得值
$('#test-ul').html('<strong>123</strong>');//设置值

```

CSS的操作

```js
$('#test-ul li[name=python]').css({"color","red"});
```

元素的显示和隐藏，：本质display:none

```js
$('#test-ul li[name=python]').show();
$('#test-ul li[name=python]').hide();
```

娱乐测试

```js
$(window).width()//页面宽度
$(window).height()//页面高度
$('#test-ul li[name=python]').toggle();//状态切换，例如display的hide和show
```

未来ajax()

```js
$('#form').ajax()
$.ajax({url:"test.html",context:document.body,success:function(){
	$(this).addClass("done");
}})
```

**小技巧**

1、如何巩固JS（看jQuery源码，看游戏源码！）

2、巩固HTML、CSS（扒网站，全部down下来，然后对应修改看效果~）