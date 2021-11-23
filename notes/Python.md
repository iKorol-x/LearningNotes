# Python

## print

> print函数对于数字（整型、浮点型）是可以直接输出得

```python
print(11)
print(22.1)
```

> 对于字符型，或者字符串型，需要用单引号，或者双引号

```python
print('zz')
print("zz")
```

> print函数也可以将结果输出到指定文件中

```python
fs = open("D:\hello.txt","a+")
print("hello",file=fs)
fs.close()
#open表示打开指定位置得文件
#a+表示若是不存在这个文件就创建一个，若是这个文件有内容就在内容后面添加信息
#记得加file，不然输出不上
#文件流都需要关闭
```

> print函数默认是换行输出的，但也可以不换行，内容之间用逗号隔开就可以了

```python
print("time","is","tight")
```

> 不换行输出end

```python
for _ in range(5):
    print('*', end='\t')
```

## 转义字符 \

> 例

```
反斜杠： \\
单引号： \'
双引号： \"
换行： \n
回的(return)： \r
水平制表符(tab)： \t
退格： \b
```

> \t制表符她时大时小的原因

​	\t占用了4个位置（可以理解为4个空格）,每4个格子占用一个/t，当这4个格子没有被人占满时，输入\t他就会将这4个格子补上空格之后再开一个\t的位置，若是刚好被人占满，他就会先占4个，后面的输入的信息就会在这个\t之后

> 注意

- 若是不希望转义字符起作用，可以在原字符之前加上r或R

```python
print(r"zz \n zz")
```

- 最后一个字符不能是反斜杠

```python
# print("zzzzzz\") 会报错
```

## 标识符和保留字

这些字符和字不能用于自定义命名

> 查看保留字

```python
import keyword
print(keyword.kwlist)
```

```
['False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
```

> 标识符规则

- 可以使用字符、数字、下划线
- 不能以数字开头
- 不能是保留字
- 严格区分大小写

## 变量

变量可以直接赋值

> 可以用id、type、value来查看变量的编号，类型和值

```python
name = 'xcc'
print('编号：',id(name))
print('类型：',type(name))
print('值：',name) #value可省略
```

## 数据类型

整型（int），浮点型（float），布尔类型（bool），字符串类型（str）

> 整型 int

- 十进制：默认的表示形式
- 二进制：以0b开头
- 八进制：以0o开头
- 十六进制：以0x开头

> 浮点型 float

- 使用浮点型进行计算时，会出现小数位数不确定的情况

  ```python
  a=1.1
  b=2.2
  print(a+b)
  #3.3000000000000003
  #原因是因为计算机用二进制进行计算，再转2进制时会出现不精确的情况
  ```

- 解决位数不确定的情况

  ```python
  from decimal import Decimal
  print(Decimal('1.1')+Decimal('2.2'))
  ```

> 布尔型 bool

- True、False

- 布尔值可以转化为整数,运算时也可以直接当0，1运算

  - True —> 1
  - False —> 0

  ```python
  print(True+1)
  print(False+1)
  ```

> 字符串 str

- 字符串被称为不可变的字符序列

- 可以用单引号，双引号，三引号来表示

- 单引号、双引号定义的字符串必须再同一行，三引号可以不再同一行

  ```python
  str1='测试1'
  str2="测试2"
  str3="""测
  		试3"""
  str4='''测
  		试4'''
  ```

## 数据类型转换

用于不同类型数据的拼接

> 以str为例

```python
name = 'ccc'
age = 20
print('名字：'+name+'\t年龄：'+str(age))
#print('名字：'+name+'\t年龄：'+age),这样会报错，因为str类型不能和int类型拼接在一起，需要类型转换
```

> 另外两个：int()、float()

```python
int('1111')
int(9.6)
#int转化浮点型时会抹零，即直接去掉小数点后面的那位
```

```python
float('9.9')
float(9)
#float转化整型会在小数点后面加个0
```

## 注释

> 单行注释

#开头，换行结束

> 多行注释

一般三引号中的内容不赋给变量就算多行注释

> 中文编码声明（python3之后不需要指定了）

```python
#coding:GBK
#在保存文件时，就会指定为GBK编码形式表示，python3默认为UTF-8
```

## 输入函数

input()

> 作用

接受来自用户的输入

> 返回值类型

输入值得类型为str

> 值得存储

使用=对输入的值进行存储

```python
p = input('输入一个数字') #input中的内容是提示语，之后输入的信息会赋给p
print(p)
```

## 运算符

> 算数运算符

- **标准算符运算符：**加(+)，减(-)，乘(*)，除(/)，整除(//)

- **取余运算符：**%

- **幂运算符：**\*\*

```python
print(1/2) #直接除是浮点型 0.5
print(4//3) #1
print(1%2) #1
print(2**3) #8

#整除//，当一正一负时，向下取整
print(-6 // 4)  # -2
print(-20 // 3)  # -7

#取余%，一正一负时，余数=被除数 - 除数*商
print(9 % -4)  # 9-(-4)*(-3) = -3
```

> 赋值运算符

- 执行顺序：右	—>	左
- 支持链式赋值
- 支持参数赋值(+=,-=,*=,/=,//=)
- 支持系列解包赋值

```python
# 链式赋值
a = b = c = 20
print(a, b, c)
# 参数赋值
a += 1
print(a)
# 解包赋值
a, b, c = 10, 20, 30
print(a, b, c)
# 解包的应用 ——> 交换值
a, b = 10, 20
print(a, b)
a, b = b, a
print(a, b)
```

> 比较运算符

- 大小的比较：>,<,>=,<=,!=
- 对象value的比较：==
- 对象id的比较：is,is not

```python
a = str(10)
b = 10
print(int(a) == b)  # True，表示a的值=b的值
print(a is b)  # True,表示a的id = b的id，这里输出的是false，因为a，b不是同一类型

a = [11,2,3,4]
b=[11,2,3,4]
print(a==b) #True
print(a is not b) #True
```

> 布尔运算符

and、or、not、in、not in

```python
# and 就是 与运算
a = True
b = True
print(a and b)  # True
print(a == False and b)  # Flase
print(a == False and b == False)  # False

# or 就是 或运算
print(a or b)  # True
print(a == False or b)  # True
print(a == False or b == False)  # False

# not 就是 非运算
print(not a)

# in 就是包含 not in 就是不包含
a = 'mclsssss'
print('c' in a)  # True
print('z' in a)  # False
print('z' not in a)  # True
```

> 位运算符（对每一位进行计算，输出为10进制）

- 位与&：全1为1，否则都为0
- 位或|：全0为0，否则为1
- 左移运算符<<：高位溢出舍弃，低位补0
- 右移运算符>>：高位溢出舍弃，低位补0

```python
# 位与&
print(1 & 1)  # 1
print(1 & 2)  # 0
print(2 & 2)  # 2
# 位或|
print(0 | 0)  # 0
print(0 | 1)  # 1
print(3 | 3)  # 3
# 位移<<,>>
print(1 << 2)  # 4
print(4 >> 1)  # 2
```

> 运算符优先级

```
由高到低
0   *\* 
1   *,/,//,%
2   +,-
3    <<,>>
4    &
5    |
6    <,>,<=,>=,==,!=
7    and
8    or
9    =
```

**算数运算符 > 位运算符 > 比较运算符 > 赋值运算符**

## 对象的bool值

python一切皆对象，所有对象都有一个布尔值

> 查看对象的bool值

```
a = 'zd'
print(bool(a))
```

> bool值为false的对象

```
	False
	数值0
	None
	空字符串
	空列表
	空元组
	空字典
	空集合
```

## 组织结构

### 单分支结构if

```
if 条件表达式:
	执行条件体
```

> 例

```python
a = input('输入一个数')
if int(a) >= 10:
    print('a>10')
```

### 双分支结构if...else

```
if 条件表达式:
	执行条件体1
else:
	执行条件体2
```

> 例

```python
a = input('输入一个数')
if int(a) >= 10:
    print('a>10')
else:
    print('a<10')
```

### 多分支结构

```
if 条件表达式1:
	执行条件体1
elif 条件表达式2:
	执行条件体2
elif 条件表达式3:
	执行条件体3
else:
	执行条件体4
```

> 例

```python
a = int(input('在输入一个数'))
if a < 10:
    print('a<10')
elif a == 10:
    print('a==10')
elif a < 20:
    print('10<a<20')
elif a == 20:
    print('a==20')
else:
    print("a>20")
```

### 嵌套if

```
if 条件表达式1:
	if 条件表达式1.1:
		条件执行体1.1
	else:
		条件执行体1.2
else:
	条件执行体2
```

```python
a = input('你是男是女？')
b = int(input('你几岁啊？'))
if a == 'man':
    if b<30:
        print('年轻男人！')
    else:
        print('老男人！')
else:
    print('不是男人')
```

### 条件表达式

```
x if 判断条件 else y
#有点像三目运算符
```

```python
a = int(input('num1'))
b = int(input('num2'))
x = a if a>b else b		#如果a>b,则x=a,否则x=b
print(x)
'''
    num1-->10
    num2-->1
    10

    num1-->10
    num2-->20
    20
'''
```

### pass

pass语句什么都不做，只是一个占位符，用在语法需要语句的地方

一般用于构造好了语法结构但没有想好具体怎么写的时候

可用于：条件执行体，循环体，函数体

## 内置函数

### range()函数

用于生成一个整数数列

> 方式

- range（stop）：创建一个（0，stop）之间的整数序列，步长为1
- range(start,stop)：创建一个(start,stop)之间的整数数列，步长为1
- range(start,stop,step)：创建一个(start,stop)之间的整数数列，步长为step

> 特点

- 返回值是一个迭代器对象
- 不管range对象的整数序列有多长，range对象所占用的内存空间都是一样的，因为存储的仅仅只是start，stop和step，只有真正用到这个range对象时，才会去计算这个序列
- in和not in可以判断整数序列中是否存在（不存在）指定的整数

### while循环

> 例

```python
#偶数和
a=0
sum = 0
while a<101:
    sum+=a
    a+=2
print(sum)
```

### for-in循环

```python
#遍历for-in循环
for i in range(10):
    print(i)
#遍历字符串
for str in 'This is a word':
    print(str)
#不需要使用变量时的循环
for _ in range(5):
    print('这句话执行5次')
#指定步长的循环
sum = 0;
for i in range(0,101,2):
    sum+=i;
print(sum)
```

```python
#1000以内的水仙花数
for i in range(100, 1000):
    sum = 0
    k = i
    while k != 0:
        sum += (k % 10) ** 3
        if sum>i: break
        k //= 10
    if sum == i:
        print(sum)
```

### break、continue、else语句

break：跳出当前循环

```
见上
```

continue：直接进入下次循环

```python
#输出100以内5的倍数
for i in range(101):
    if not bool(i % 5):
        print(i)
    else:
        continue
```

else：有三种else的使用情况，if-else、while-else、for-else

for-else和while-else中，else执行的情况是在循环中没有出现break或着其他中断循环的条件时会进入else语句

```python
for _ in range(3):
    a=int(input('输入密码：'))
    if a == 1111:
        print('yes')
        break
    else:
        print('no')
else: 	#循环正常结束时，会执行以下语句
    print('times out')
```

### 嵌套循环

while、for-in、break等都可以嵌套使用

## 列表

相当于java中的数组

### 特点

- 列表元素有序
- 索引对应唯一一个数据元素
- 列表可以存储重复数据
- 列表可以存储任意数据类型
- 根据需要动态分配和回收内存

### 创建列表

```python
#两种方式
#静态列表
a = ['come','baby',778]
print(a)
#用list创建
a = list(['你','在','赣','神','魔',778])
print(a)
```

### 查询列表

```python
# 查找指定元素的索引，多个相同元素只返回第一个找到的
print(a.index('在'))
# 查找指定范围内的某个元素的索引
print(a.index('神', 1, len(a)))
# 查找某个元素在列表中出现的次数
print(a.count('你'))
# 查找某个索引对应的元素
# 正向查找，0开始
print(a[2])
# 逆向查找，列表末尾开始
print(a[-2])

# 列表的切片操作 list[ start : stop : step ]
# 默认步长为1
print(a[1:len(a)])
# 指定步长
print(a[0:len(a):2])
# 默认从1查完列表,甚至1不写，直接 ::2 也可以，相当于指定步长从0开始查完列表
print(a[1::2])
# 逆序输出列表，即步长为负,逆序输出时，若是要指定长度，则start>stop,否则输出空表，start和stop不写则逆序整个列表
print(a[::-1])

# 列表判断 in & not in
print('你' in a)
print('你' not in a)
print(11 in a)
print(778 in a)
print(7 in a)

# 某个元素是否在列表的某个元素对象中
a = ['python', 'java', 'c++']
for item in a[::]:
    print('p' in item)
```

### 列表的CRUD

1. 增（C）

   - append()：列表末尾添加一个元素

   - extend()：列表末尾至少添加一个元素

   - insert()：列表任意位置添加一个元素

   - 切片：在列表任意位置至少添加一个元素

     ```python
     a = [1, 2, 3, 4, 5, 'zz']
     print(a, id(a))
     # append(),不创建新的列表对象
     a.append('cxc')
     print(a, id(a))
     # extend()
     b = list(['aaaa', 'cccc'])
     a.append(b)
     print('append----->' + str(a) + str(id(a)))
     a.extend(b)
     print('extend----->' + str(a) + str(id(a)))
     '''
         extend和append的区别就是
         extend添加多个元素时将列表中的元素挨个放入原列表中
         append添加多个元素时会将整个列表放入原列表中
         两种方式都不会创建新的数组对象
     '''
     
     # insert
     '''
         insert在指定的位置上添加一个元素，这个位置后面的元素（包括原来这个位置的元素）都会向后移
     '''
     a.insert(1, '每次')
     print(a)
     
     # 切片
     '''
         切片就相当于将一个列表中的元素放入另一个列表的指定位置，
         原列表原来位置及stop位置上的的元素都会被覆盖
     '''
     b = ['mckls', True, 'zz']
     # 不删除原列表任何元素直接加入
     a[1:1] = b
     print(a)
     # 覆盖一些原列表中的元素,左闭右开
     n=['ccccc',False]
     a[2:5] = n
     print(a)
     
     ```

2. 删（D）

   - remove()：一次只删除一个元素，重复元素只删除第一个，元素不存在抛出ValueError

   - pop()：删除一个指定索引上的元素，指定索引不存在抛出IndexError，不指定索引就删除列表中最后一个元素

   - 切片：一次删除多个元素

   - clear()：清空列表

   - del：删除列表

     ```python
     # 删除元素
     # remove()
     a.remove('zz')
     print(a)
     # pop()
     a.pop()
     print(a)
     a.pop(2)
     print(a)
     # 切片 切片之后会产生新的列表
     # 产生新的列表
     new_list = a[1:3]
     print(a, id(a))
     print(new_list, id(new_list))
     # 源列表上修改，不产生新的对象
     a[1:3] = []
     print(a, id(a))
     # 清空列表
     a.clear()
     print(a)
     # 删除列表
     del a
     ```

3. 改（U）

   ```python
   # update
   # 修改单个值
   a = ['cc','qdas',110,20]
   a[1]=24
   print(a)
   # 修改多个值,指定位的所有值都会被修改，个数可以大于指定位置的个数
   a[1:3] = ['ccc',1212,'mcl','ppp']
   print(a)
   ```

### 列表元素的排序

- sort()：默认为从小到大升序，reverse=True可以进行降序
- 内置函数sorted()：指定reverse=True进行降序，原列表不发生改变

```python
# 排序
a = [10, 4, 24, 13, 22, 31, 2]
# 升序
a.sort()
print(a)
# 降序
a.sort(reverse=True)
print(a)
# sorted内置函数,不改变原函数，即产生新列表
b = sorted(a)
print(b,a)
b = sorted(a,reverse=True)
print(b,a)
```

### 列表的推导式

```python
# 列表生成式，for之前的item表示存入的值，可以计算
c = [item for item in range(1, 10)]
d = [i * 10 for i in range(2, 20)]
print(c)
print(d)

# 将2，4，6，8，10存入列表
c = [i * 2 for i in range(1, 6)]
print(c)
```

## 字典

### 定义

类似HashMap

- 是python内置的数据结构之一，与列表一样是一个可变序列
- 以键值对的方式存储数据，字典是一个无序的序列（即第一个放入字典中的数据遍历时不一定第一个被输出）
- 字典的key不能重复，value可以重复
- 字典可以动态伸缩
- 字典会浪费大量内存，是一种用空间换时间的数据结构
- 字典中不能存放字典

```python
scores = {'zhangsan':100 , 'lisi':60 , 'wangwu': 99}
```

### 字典的原理

- 其实就是通过hash函数计算key的位置，然后再key的位置放入value值

- 查找原理就是通过key找value

### 字典的创建和删除

```python
# 两种创建方式
# 大括号直接插入
scores = {'zhangsan': 100, 'lisi': 60, 'wangwu': 99}
# dict创建
name = dict(name='java', socre=89, wangwu=99)
print(scores, name) 
# {'zhangsan': 100, 'list': 60, 'wangwu': 99} {'name': 'java', 'socre': 89, 'wangwu': 99}
# 空字典
a = dict()
b = {}
print(a, b)  # {} {}
```

### 字典的查询操作

两种方式：[]（方括号），get()方法

```python
# 获取字典信息
# []
print(scores['zhangsan'])
# get()
print(scores.get('zhangsan'))
# 不存在，返回固定值，只能用get()方法实现
print(scores.get('cc', '没有'))
# key的判断
print('cc' not in scores) # True
print('zhangsan' in scores) #True
```

### 字典的增、删、改

```python
# 添加键值对
scores['cc'] = 888
# {'zhangsan': 100, 'lisi': 60, 'wangwu': 99, 'cc': 888}
print(scores)
# 修改
scores['cc'] = 777
# {'zhangsan': 100, 'lisi': 60, 'wangwu': 99, 'cc': 777}
print(scores)
# 删除
del scores['cc']
# {'zhangsan': 100, 'lisi': 60, 'wangwu': 99}
print(scores)
```

### 获取字典的视图

1. 获取字典的所有key：keys()
2. 获取字典中所有的value：values()
3. 获取字典中所有的key，value：items()
4. 字典的遍历

```python
# ['zhangsan', 'lisi', 'wangwu'] <class 'dict_keys'> 也可以转为list列表
print(list(scores.keys()),type(scores.keys()))

# dict_values([100, 60, 99]) <class 'dict_values'>
print(scores.values(),type(scores.values()))

# dict_items([('zhangsan', 100), ('lisi', 60), ('wangwu', 99)]) <class 'dict_items'>
print(scores.items(),type(scores.items()))

# 字典遍历,这里的item是指字典中的key
for item in scores:
    print(item,scores[item])
```

### 字典推导式

- 内置函数：zip()

```python
# 字典的生成式，若是字典key和value个数不相等时，以个数少的为准
items=['a','b','c']
prices = [11,22,33]
a = { item.upper() : price  for item,price in zip(items,prices)}
print(a)
```

## 元组

元组时python内置的数据结构之一，是一个不可变的序列

> 不可变序列与可变序列的区别

- 不可变序列有：字符串，元组
  - 没有增、删、改操作
- 可变序列：列表、子弹
  - 可以进行增删改操作，对象地址不发生改变

```python
# 字符的添加操作相当于重新创建了一个字符串对象，地址发生改变，所以他是不可变，元组同理
e = 'ccc'
print(id(e)) # 1666740562160
e +='aaa'
print(id(e)) #1666740586352
print(e)
```

### 元组的创建方式

- 元组的小括号可以省略，但用函数创建时不能省略
- 只有一个元素的元组必须在后面加一个逗号（，），不然不是元组类型，这里的小括号也可以省

```python
# 元组
# 小括号直接创建,小括号可省
t = ('python','java',99)
print(t,type(t))
# 使用tuple()函数创建
t = tuple(('this','is','tuple'))
print(t)
# 只有一个元素的元组
t = 1,
print(t,type(t))
# 空元组的创建
t = ()
print(t)
s = tuple()
print(s)
```

> 设计不可变序列的原因

- 多任务环境下，同时操作对象时，不需要加锁

- 不可变序列代表对象不可变，也就避免了多用户修改时发生的同步问题

- 注意

  - 如果元组中的元素对象本身不可变，则不能在引用其他对象

  - 如果元组中的对象是可变对象，则可变对象的引用不能改变，但数据可以改变

    ```
    可以这么理解，放入元组的对象地址就不能发生改变，只能看，不能改，但是若是指向的地址不是不可变的，那么这个地址对应的对象存储的数据是可以改变的，只是布恩那个改变外层元组中指向它的地址，但可以改变地址指向的对象中的内容。
    ```

    ```python
    # 元组的不可变特性
    # 修改元组中的可变对象
    a = [1,2]
    t = (1,a,'c') # (1, [1, 2], 'c')
    print(t)
    a[:] = [2]
    print(t) # (1, [2], 'c')
    ```

### 元组的遍历

两种方式：下标直接获取（从0开始），for-in遍历

```python
# 获取元组数据
# 下标方式获取
print(t[1])
# 遍历元组
for item in t:
    print(item)
```

## 集合

- python内置的数据结构，集合中的数据不可重复
- 与列表、字典一样属于可变序列
- 集合是没有value的字典（类似java的set集合）
- 存储位置由hash函数计算得出，放入顺序与取出顺序不一定相同

### 集合的创建

- 直接大括号形式：{}
- set函数形式：set()
  - set()函数+大括号
  - set()函数+中括号
  - set()函数+小括号
  - **set()函数+字符串**
  - set()函数+range()函数生成集合
  - 空集合，直接大括号生成的空集合是字典类型，所以生成空集合只能用set()函数
- set函数生成的所有集合，不管用什么括号，生成的都是集合对象，与直接大括号生成没有区别

```python
# 集合
# 大括号形式
lis = {'python', 'yyds', 'nb'}
print(lis)
# 标准set函数+大括号形式
lis = set({'who', 'is', 'your', 'daddy', '?'})
print(lis)
# set函数+中括号形式
lis = set([1, 2, 3])
print(lis)
# set函数+小括号形式
lis = set((1, 23, 33))
print(lis)
# set函数+字符串形式
''' 对字符串进行处理时，set会将字符串拆分放入集合中，且放入集合中的元素不能有重复的'''
lis = set('aacbb')
print(lis)
# 空集合
lis = set()
print(lis)
# set函数+range函数 集合生成
lis = set(range(55))
print(lis)
```

### 集合的CRUD

> 集合元素的判断

- in
- not in

> 集合元素的增加

- add()方法，一次添加一个元素
- update()方法，一次添加至少一个元素（多个）,可以是集合、列表、元组

> 集合元素的删除

- remove()方法，一次删除一个指定元素，不存在抛KeyError异常
- discard()方法，一次删除一个指定元素，不存在不抛异常
- pop()方法，一次删除任意一个元素
- clear()方法，清空集合

```python
lis = set('python')
print(lis)  # {'o', 'n', 'p', 'y', 'h', 't'}
# 集合的判断
# in & not in
print('o' in lis) # True
print('z' not in lis) # True
# add()
lis.add('c')
print(lis) # {'t', 'n', 'c', 'h', 'p', 'y', 'o'}
# update()
lis.update({1, 2, 3})
print(lis)  # {1, 't', 'n', 'y', 'c', 2, 3, 'p', 'o', 'h'}
# remove()
lis.remove(1)
print(lis)  # {2, 3, 'n', 'y', 'p', 'h', 'c', 't', 'o'}
# discard()
lis.discard(100)
print(lis)  # {'n', 2, 3, 't', 'c', 'p', 'o', 'y', 'h'}
# pop()
lis.pop()
print(lis)  # {2, 'n', 3, 'o', 'h', 't', 'p', 'y'}
# clear()
lis.clear()
print(lis)  # set()
```

### 集合间的关系

- 集合是否相等
  - 可以使用 == 或者 ！= 进行判断
- 一个集合是否为另一个集合的子集
  - 可以调用issubset进行判断
  - B是A的子集
- 一个集合是否是另一个集合的超集
  - 可以调用issuperset进行判断
  - A是B的超集
- 两个集合是否没有交集
  - 可以调用isdisjoint进行判断

```python
# == & !=
s1 = {10, 22, 33, 44}
s2 = {22, 10, 44, 33}
print(s1 == s2)  # True
print(s1 != s2)  # False
# issubset
s1 = {1, 2, 3, 4}
s2 = {2, 4}
print(s2.issubset(s1))  # True
# issuperset
print(s1.issuperset(s2))  # True
# isdisjoint
s1 = {1, 2, 3, 4}
s2 = {5, 6, 7}
print(s1.isdisjoint(s2))  # True
```

### 集合的数学操作

**数学操作不改变原集合**

- 交集
- 并集
- 差集
- 对称差集

```python
s1 = {10, 20, 30, 40}
s2 = {30, 40, 50, 70}
# 交集  intersection 和 & 等价
print(s1.intersection(s2))  # {40, 30}
print(s1 & s2)
# 并集 union 和 | 等价
print(s1.union(s2))  # {70, 40, 10, 50, 20, 30}
print(s1 | s2)
# 差集 difference 和 - 等价
print(s1.difference(s2))  # {10, 20}
print(s1 - s2)
print(s2 - s1)  # {50, 70}
# 对称差集 symmetric_difference 和 ^ 等价
print(s1.symmetric_difference(s2))  # {70, 10, 50, 20}
print(s1 ^ s2)
```

### 集合的生成式

```python
# 集合生成式,没有元组生成式
# 将列表生成式的中括号改为大括号就行了
a = {i for i in ['c', 'd', 11, 22]}  # 里面的中括号换成大括号小括号都行
print(a)  # {11, 22, 'c', 'd'}
a = {i for i in range(1, 6)}
print(a)  # {1, 2, 3, 4, 5}
```

### 列表、元组、字典和集合的总结

|   数据结构    | 是否可变 |         是否重复         | 是否有序 |    定义符号     |
| :-----------: | :------: | :----------------------: | :------: | :-------------: |
| 列表（list）  |   可变   |          可重复          |   有序   |       [ ]       |
| 元组（tuple） |  不可变  |          可重复          |   有序   |       ( )       |
| 字典（dict）  |   可变   | key不可重复，value可重复 |   无序   | { key : value } |
|  集合（set）  |   可变   |         不可重复         |   无序   |       { }       |

## 字符串

### 字符串驻留机制

​	其实和java的机制一样，字符串是不可变的，出现新的字符串时就会产生一个不可变的对象存储这个字符串，且这个字符串再程序结束之前都不会消失，若是有相同的字符串，则它们指向的地址一定是相同的。

> 驻留的几种情况

- 字符串长度为0或1时
- 符合标识符的字符串
- 字符串只在编译时驻留，而非运行时（调用方法拼接的目标字符串和目标字符串本身地址不一定相同，因为方法是在运行时起作用的）
- [-5,256]之间的整数数字

> 强制驻留

```python
a = sys.intern(b) #让a强制指向b
```

> PyCharm对字符串进行了优化处理

对相同的字符串都进行了强制驻留

> 字符串驻留的优缺点

- 需要值相同的字符串时，可以直接从字符串池中拿来用，避免频繁的创建与销毁，提升效率节约内存，因此拼接字符串和修改字符串是比较影响性能的
- 在需要进行拼接字符串时建议使用str类型的join方法，而非 + ，因为join()方法是先计算出所有字符串的长度，然后再拷贝，只new一次对象，效率比" + "要高

### 字符串的常用操作

> 查询操作

- index()方法：查找子串substr第一次出现的位置，如果子串不存在，则抛出ValueError异常
- rindex()方法：查找子串substr最后一次出现的位置，如果子串不存在，则抛出ValueError异常
- find()方法：查找子串substr第一次出现的位置，如果子串不存在，返回-1
- rfind()方法：查找子串substr最后一次出现的位置，如果子串不存在，返回-1

```python
a = 'abc%asdasdqwegrthvhbn'
# index()
print(a.index('d'))  # 6
# rindex()
print(a.rindex("d"))  # 9
# find()
print(a.find("1"))  # -1
print(a.find('d'))  # 6
# rfind()
print(a.rfind('d'))  # 9
```

> 字符串大小写转换

方法会产生新的对象，原字符不变，方法产生新的对象地址，即使方法进行后与原字符相同，地址也会不同

- upper()：将字符串中的所有字符都转成大写字母
- lower()：将字符串中的所有字符都转成小写字母
- swapcase()：把字符串中的所有小写字母都转成大写字母，大写字母都转成小写字母
- capitalize()：把第一个字符转为大写，其余都转成小写
- title()：把每个单词的第一个字符转成大写，每个单词的其他剩余字符都转成小写

```python
a = 'asdJkhzXcHui3QazXc,hello,world'
# upper()
print(a.upper())  # ASDJKHZXCHUI3QAZXC,HELLO,WORLD
# lower()
print(a.lower())  # asdjkhzxchui3qazxc,hello,world
# swapcase()
print(a.swapcase())  # ASDjKHZxChUI3qAZxC,HELLO,WORLD
# capitalize()
print(a.capitalize())  # Asdjkhzxchui3qazxc,hello,world
# title()
print(a.title())  # Asdjkhzxchui3Qazxc,Hello,World
```

> 字符串内容对齐

- center()：居中对齐，第1个参数指定宽，第2个参数指定填充符，第2个参数可选，默认为空格，若设置宽度小于实际宽度则返回原字符串
- ljust()：左对齐，第1个参数指定宽，第2个参数指定填充符，第2个参数可选，默认为空格，若设置宽度小于实际宽度则返回原字符串
- rjust()：右对齐,第1个参数指定宽，第2个参数指定填充符，第2个参数可选，默认为空格，若设置宽度小于实际宽度则返回原字符串
- zfill()：右对齐，左边用0填充，该方法只接受一个参数，用于指定字符串的宽度，若设置宽度小于实际宽度则返回原字符串

```python
a = 'avsadwqs'
#填充符默认是空格，宽度过小就返回原字符
# center()
print(a.center(20, '*'))  # ******avsadwqs******
# ljust()
print(a.ljust(20, '*'))  # avsadwqs************
# rjust()
print(a.rjust(20, '*'))  # ************avsadwqs
# zfill()
print(a.zfill(20))  # 000000000000avsadwqs
# zfill() 若是带符号数字，则在符号后面数字前面加0
# 若是有多个符号，则只在第一个符号后面加0
print('-5464645'.zfill(20))  # -0000000000005464645
print('-+213412'.zfill(20))  # -000000000000+213412
```

> 字符串的劈分

- split()：从字符的**左边**开始劈分，默认劈分字符为空格，返回值都是一个列表
  - 可以通过sep指定劈分字符串的劈分符
  - 可以通过maxsplit指定劈分字符串的最大劈分次数，超出劈分次数的字符串会单独称为一部分
- rsplit()：从字符的**右边**开始劈分，默认劈分字符为空格，返回值都是一个列表
  - 可以通过sep指定劈分字符串的劈分符
  - 可以通过maxsplit指定劈分字符串的最大劈分次数，超出劈分次数的字符串会单独称为一部分

```python
a = '1999 04 25 5013 zz'
b = '1999-04-25-5013-zz'
# split()
print(a.split())  # ['1999', '04', '25', '5013', 'zz']
# 指定sep
print(b.split('-'))  # ['1999', '04', '25', '5013', 'zz']
# 指定maxsplit
print(b.split('-', 2))  # ['1999', '04', '25-5013-zz']
a = 'asd asda das dafd gf hgjgh khj'
b = 'as-d-as-dxz-c-z-fd'
# rsplit()
print(a.rsplit())  # ['asd', 'asda', 'das', 'dafd', 'gf', 'hgjgh', 'khj']
# 指定sep
print(b.rsplit('-'))  # ['as', 'd', 'as', 'dxz', 'c', 'z', 'fd']
# 指定maxsplit
print(b.rsplit('-', 2))  # ['as-d-as-dxz-c', 'z', 'fd']
```

> 判断字符串

- isidentifier()：判断指定字符串是否为合法标识符
- isspace()：判断指定字符串是否全由空白字符组成（回车，换行，水平制表符）
- isalpha()：判断字符串是否全由字母组成
- isdecimal()：判断字符串是否全由10进制数字组成
- isnumeric()：判断字符串是否全由数字组成，这个字符串包含罗马数字、中文数字、标志数字等都算是数字
- isalnum() ：判断字符串是否全由数字和字母组成

```python
# 判断字符串
a = '!sadak?>>?{:{!$*&^'
b = 'a213__dasjkh'  # 合法标识符就是有数字字母（包含中文）下划线组成的，不以数字开头的字符
# isidentifier()
print(a.isidentifier())  # False
print(b.isidentifier())  # True
# isspace()
c = "  \n \t"
print(a.isspace())  # False
print(c.isspace())  # True
# isalpha()
d = "kahsdkuhuiwhdkj"
print(a.isalpha())  # False
print(d.isalpha())  # True
# isdecimal()
e = '325634864651'
print(e.isdecimal())  # True
print(a.isdecimal())  # False
# isnumeric()
f = '1123四⑤Ⅶ捌'
print(f.isnumeric())  # True
print(a.isnumeric())  # False
# isalnum()
g = 'aksyd9y12uieh23iu5hjk23'
print(g.isalnum())  # True
print(a.isalnum())  # False
```

> 字符串的其他操作

- replace()：第一个参数指定被替换的子串，第二个参数指定替换子串的字符串，第三个参数是最大替换次数（可选），该方法产生新的字符串
- join()：将列表或者元组中的字符合并成一个字符串，是字符串的方法，但作用于列表和元组

```python
# replace()
a = 'what you have down will return tomorrow'
print(a.replace(' ', '-'))  # what-you-have-down-wil  l-return-tomorrow
print(a.replace(' ', '-', 3))  # what-you-have-down will return tomorrow
# join
b = a.split()
print(b)  # ['what', 'you', 'have', 'down', 'will', 'return', 'tomorrow']
print('|'.join(b))  # what|you|have|down|will|return|tomorrow
print('-'.join('whosyourdaddy', ))  # w-h-o-s-y-o-u-r-d-a-d-d-y
```

### 字符串的比较

- 运算符：>,>=,<,<=,==,!=
- 比较规则：首先比较两个字符的第一个字符，如果相等则继续比较下一个字符，直到两个字符串的字符不相等，其比较结果就是两个字符串的比较结果，两个字符串中的所有后续字符将不再被比较
- 比较原理：两个字符进行比较，比较的是其ordinal value（原始值），调用内置函数ord可以得到指定字符的ordinal value。与内置函数ord对应的是内置函数chr，调用chr时可以通过ordinal value获取到对应的字符
- 这个ordinal value其实就是字符在ASCII码中对应的位置

```python
#字符串比较
# >
print('apple'>'banaba') #False
print(ord('a'),ord('b')) #97 98
print(chr(985),chr(211)) #ϙ Ó
# == 和 is
'''
 ==比较的是value值，is比较的是地址
'''
```

### 字符串的切片操作

字符串是不可变类型，不具备增、删、改等操作，切片操作将产生新的对象

> 格式

- str [ start : stop : step]
- 只要是不同的字符串即使不同的对象 

> 例子 

```python
# 字符串切片
a = 'hello,world'
print(a[:5])  # hello
print(a[6:])  # world
c = a[:5] + '!' + a[6:]
print(c)  # hello!world
print(c[0:9:2])  # hlowr
# 倒叙输出
print(c[::-1])  # dlrow!olleh
print(c[-5::1])  #world
```

### 格式化字符串

> 目的

解决多次拼接字符串造成的资源浪费

> 方式

- %作为占位符
- {}作为占位符

```python
# 字符串格式化
# %d:int | %s:str
print('my name is : %s , age : %d' % ('mcl', 20))  # my name is : mcl , age : 20
# 若是d前面有数字，类似 %10d，这个10指的是宽度
print('%10d' % (20)) #        20
# %f需要保留n位小数
print('%.4f' % 3.1415926) #3.1416
# {}
'''
    who is you daddy? mcl
    how old is he? 20
'''
print('who is you daddy? {0}\nhow old is he? {1}'.format('mcl', 20))
s1 = 'mcl'
age = 20
print(f'my name is {s1},my age is {age}')  # my name is mcl,my age is 20
# 保留3位数 并占位10个字符
print('{0:10.3}'.format(3.1415926)) #       3.14
# 保留3位小数并占位13个字符
print('{0:10.3f}'.format(3.1415926)) #      3.142
```

### 字符串的编码转换

> 目的

解决字符传输时只能以二进制的形式传输（字符 ---(编码)---> 二进制(bytes) ---(解码)--->字符）

> 方式

- 编码：str . encode(encoding=' 编码格式 ')
- 解码：byte . decode(encoding=' 编码格式 ')
- 用什么方式编的码，就要相同的方式解码

```python
# 字符串编码转换
s = '李在赣神魔'
# 编码
# 在GBK中一个中文占两个字节，所以输出有10个字节
print(s.encode(encoding='GBK'))  # b'\xc0\xee\xd4\xda\xb8\xd3\xc9\xf1\xc4\xa7'
# 在UTF-8中一个中文占3个字节，所有输出有15个字节
print(s.encode(encoding='UTF-8'))  # b'\xe6\x9d\x8e\xe5\x9c\xa8\xe8\xb5\xa3\xe7\xa5\x9e\xe9\xad\x94'
# 解码,用什么方式编码就要用什么方式解码
byte = s.encode(encoding='GBK')
print(byte.decode(encoding='GBK'))  # 李在赣神魔
byte = s.encode(encoding='UTF-8')
print(byte.decode(encoding='UTF-8'))  # 李在赣神魔
```

## 函数

### 函数的创建

> 格式

```python
def  函数名([输入参数]):
	函数体
	[return xxx]
```

> 创建与调用

```python
def cal(a, b): # a,b形参
    return a + b
    
print(cal(10, 20)) #10，20实参
```

### 函数的参数传递

> 位置实参

根据位置传输参数，对应位置上的实参会传给对应位置上的形参

> 关键字实参

根据实参名称传递参数

```python
def cal(a, b):
    return a + b

print(cal(10, 20))

print(cal(b=20, a=3))
```

> 参数传递原理

- 实参传入形参时，其实就是指针指向了同一个对象，所以形参发生改变时，不会影响实参的实值，典型的就是不可变值，若是形参变换不可变对象，实参不会发生改变

- 但是，若是实参是一个可变值，那么当形参实参指向同一个值的时候，对形参进行处理也会同时影响到实参的数据，因为他们指向的是同一个对象

### 函数的返回值

1. 函数如果没有返回值，return可以不写
2. 函数的返回值如果是1个，直接返回对应类型的返回值
3. 如果函数的返回值是多个，返回的结果是元组类型

```python
def fun2(num):
    odd = []  # 奇数
    even = []  # 偶数
    for i in num:
        if i % 2:
            odd.append(i)
        else:
            even.append(i)
    return odd, even


print(fun2([1, 23, 4, 12, 3, 124, 23, 4523, 5, 23, 412, 13, 12, 4, 123, 12]))
```

### 函数的参数定义

1. 函数定义默认值参数
   - 函数定义时，给形参设置默认值，只有与默认值不符时才会传递实参
2. 位置实参传递和关键字实参传递
   - 位置实参传递：直接放入实参，位置对应了形参的位置
   - 关键字实参传递：对应的性参对应了实参
   - 二者可以混用，也可以设置为只能混用
3. 个数可变的位置形参
   - 定义函数时，无法确定传入位置的参数个数时可以使用可变位置参数
   - 个数可变的位置参数只能有一个，多个会报错
   - 使用*定义个数可变的位置形参，结果为一个元组
4. 个数可变的关键字形参
   - 定义函数时，无法确定传递实参的个数时，可以使用可变关键字形参
   - 个数可变的关键字形参只能由一个，多个会报错
   - 使用**定义个数可变的关键字形参，结果为一个字典
5. 同时出现个数可变的位置参数和个数可变的关键字形参，位置形参要放在关键字形参的前面

```python
# 默认值参数
def fun3(a, b=3):
    print(a, b)
fun3(3)  # 3 3
fun3(3, 4)  # 3 4


#个数可变的位置形参
def fun4(a, *b):
    print(a, b)
fun4(1, 'asd', 'asdas', 'aarr')  # 1 ('asd', 'asdas', 'aarr')


# 个数可变的关键字形参
def fun5(**a):
    print(a)
fun5(a=10, b=20, c='aa')  # {'a': 10, 'b': 20, 'c': 'aa'}


#同时出现两个可变形参
def fun6(*a,**b):  # 只能这么写
    print(a, b)
fun6(1, 2, 3, 4, a=1, b=3)  # (1, 2, 3, 4) {'a': 1, 'b': 3}


#字典列表转为实参
def fun7(a, b, c):
    print(a, b, c)
fun7(1, 2, 3)  # 1 2 3
# 列表元素转实参
lis = [1, 23, 3]  # 数量必须对应，不然会报错
fun7(*lis)  # 1 23 3
# 字典元素转实参
dict = {'a': 1, 'b': 11, 'c': 111}  # 数量字母必须对应，不然会报错
fun7(**dict)  # 1 11 111
# 位置参数和关键字参数昏庸
fun7(1, b=3, c=3)  # 1 3 3


# * 左边只能是位置参数，右边只能是关键字参数
def fun8(a, b, *, c, d):
    print(a, b, c, d)
fun8(1, 2, c=12, d=22)  # 1 2 12 22


# 函数定义时形参的顺序问题
def f1(a, b, *, c, d, **e): pass
def f2(*a, **b): pass
def f3(a, b=1, *c, **d): pass
```

> 总结

| 序号 |      参数类型      | 函数定义 | 函数调用 |    使用方法     |
| :--: | :----------------: | :------: | :------: | :-------------: |
|  1   |      位置实参      |          |    √     |                 |
|  2   |     列表转实参     |          |    √     |      *lis       |
|  3   |     关键字实参     |          |    √     |                 |
|  4   |     字典转实参     |          |    √     |     **dict      |
|  5   |     默认值形参     |    √     |          |                 |
|  6   |     关键字形参     |    √     |          | *分割默认与关键 |
|  7   |  个数可变位置形参  |    √     |          |      *args      |
|  8   | 个数可变关键字形参 |    √     |          |     **args      |

### 变量的作用域

- 局部变量：函数体内有效，函数体外无效
- 全局变量：函数体内外都有效

### 递归函数

函数调用自身就是递归，每次调用都会压栈

> 优缺点

- 缺点：占用内存多，效率低
- 优点：思路代码简单

> 两个例子：阶乘 & 斐波那契

```python
# 阶乘
def recur(num):
    if num == 0: return 1
    return num * recur(num - 1)


a = recur(6)
print(str(a)) #720


# 斐波那契
def f(num):
    if num == 1: return 1
    if num == 2: return 1
    res = f(num - 1) + f(num - 2)
    return res


r = f(6)
for i in range(1, 11):
    print(f(i), end='\t')
# 1	1	2	3	5	8	13	21	34	55
```

## BUG

### 异常处理机制

> try...except

```python
def minus(a,b):
    return a/b
try:
    a = int(input('输入一个被除数'))
    b = int(input('输入一个除数'))
    res = minus(a,b)
    print(res)
except ZeroDivisionError: # 输出自定义信息
    print('除数不能为0')
except ValueError as e: # 输出报错的默认信息
    print(e)
```

> try...except...else

如果try中没有抛出异常则执行else，如果抛出异常则执行except

```python
try:
    a = int(input('输入一个被除数'))
    b = int(input('输入一个除数'))
    res = minus(a, b)
except ZeroDivisionError: #try语句没有正常结束时执行
    print('除数不能为0')
except ValueError as e:
    print(e)
else:	#try语句执行完成后执行
    print(res)
```

> try...except...else...finally

finally中的语句块不管有没有执行except或者try都会在最后执行finally中的语句

```python
try:
    a = int(input('输入一个被除数'))
    b = int(input('输入一个除数'))
    res = minus(a, b)
except ZeroDivisionError:
    print('除数不能为0')
except ValueError as e:
    print('有问题：',e)
else:
    print(res)
finally:
    print('程序结束')
```

### 常见的异常类型

| 序号 |     异常类型      |             描述             |
| :--: | :---------------: | :--------------------------: |
|  1   | ZeroDivisionError | 除（取模）零（所有数据结构） |
|  2   |    IndexError     |       序列中没有次索引       |
|  3   |     KeyError      |       映射中没有这个键       |
|  4   |     NameError     |      未声明/初始化对象       |
|  5   |    SyntaxError    |        Python语法错误        |
|  6   |    ValueError     |         传入无效参数         |

​	traceback模块打印出错信息，其实本来出现bug时，控制台的错误就是traceback信息

```python
import traceback
try:
    print(10/0)
except:
    traceback.print_exc()
```

## 类与对象



### 类的创建

> 类的成分

- 属性：这里的属性是指类的属性，是这个类对象所共享的
- 构造方法(自带self)，构造方法可以直接放入形参作为类实例的属性
- 实例方法：只有实例化对象之后才能调用的方法
- 类方法：可以用类名.方法名直接调用，可以使用类中的成员属性
- 静态方法：可以用类名.方法名直接调用，但不能使用类中的成员属性

> 创建类的语法

```python
class Student:
	pass
```

> 例子

```python
class Student:
    native_pace = 'cc'

    # 构造方法
    def __init__(self, name, age):
        self.age = age
        self.name = name

    # 实例方法
    def eat(self):
        print('实例方法--》吃点啥？')

    # 静态方法
    @staticmethod
    def sth():
        print('静态方法')

    # 类方法
    @classmethod
    def drink(cls):
        print('类方法')
```

### 类对象的创建

> 语法

```python
stu = Student()
```

> 方法的调用

```python
from test02.demo01.Student import Student

# 实例化对象
stu = Student('c', 20)

# 输出对象属性
print(stu.name,stu.age)

# 调用对象实例方法
stu.eat()
Student.eat(stu)

# 调用类的类方法
Student.drink()

# 调用类的静态方法
Student.sth()
```

### 动态绑定属性与方法

动态绑定得到属性和方法都只对绑定的对象有用，对其他对象和对类本身没有用

- 类的构造方法的形参在创建对象时赋值后，任意一个对象都会有这个属性
- 但是动态绑定的属性只适用于绑定的对象，其他对象仍然没有这个属性
- 方法和属性也很类似，类中的方法，类的对象都可以调用
- 但动态绑定的方法只适用于绑定的对象，其他对象没有这个方法

```python
# 类对象动态绑定属性和方法
stu1 = Student('mcl', 20)
print(stu1.name, stu1.age)  # mcl 20


# 动态添加属性
stu1.gender = '男'
print(stu1.name, stu1.age, stu1.gender)  # mcl 20 男


# 动态绑定方法
def show():
    print('动态绑定的方法') # 动态绑定的方法

stu1.show = show  # 动态绑定的方法名可以的自定义(见下)，也可以相同,
stu1.show()
'''
    stu1.ot = show # 动态绑定的方法名可以的自定义，也可以相同
    stu1.ot()
'''
```

## python的面向对象

python中没有专门的修饰符用于修饰属性的私有，如果该属性不希望被类对象外部访问，在前面加两个"_"就可以了

```python
class Student:
    __native_pace = 'cc' # 这样用类.属性的方式就访问不到了，只能通过创建类对象来访问
```

### python的下划线

- 单前导下划线：_var

  - 一个Python命名约定，表示这个名称是供内部使用的，仅仅作为一种对程序员的提示

- 单后缀下划线：var_

  - 单个末尾下划线（后缀）是一个约定，用来避免与Python关键字产生命名冲突

- 双前导下划线：__var

  - 私有化，防止被子类重写，加了双前导下划线后，原有属性命名为： _类名__属性名

- 双前导和双后缀下划线：\_\_var\_\_

  - 用于特殊用途，这些dunder方法通常被称为神奇方法，很多人不喜欢用，最好避免在自己的程序中使用以双下划线（“dunders”）开头和结尾的名称，以避免与将来Python语言的变化产生冲突。

- 单下划线：_

  - 表示某个临时的或无关紧要的变量，shell中表示最近一个表达式的结果。

  <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211120211111374.png" alt="image-20211120211111374" style="zoom: 80%;" /> 

### pyhon的封装

```python
class User:
	# 两个下划线表示是私有属性，不建议访问
    # 但是即使不写封装方法也可以通过"_User__name"的方式进行访问
    def __init__(self, name, age):
        self.__name = name
        self.__age = age
	
    #提供get，set方法用于访问和赋值
    
    def set_name(self, name):
        self.__name = name

    def get_name(self):
        return self.__name

    def set_age(self, age):
        self.__age = age

    def get_age(self):
        return self.__age


user = User('mcl', 22)
print(user.get_name(), user.get_age())  # mcl 22
user.set_name('ccc')
user.set_age(54)
print(user.get_name(), user.get_age())  # ccc 54

# 即使不写封装方法也可以访问
print(user._User__name,user._User__age)  # ccc 54
```

### python的继承

- python支持多继承，定义子类时，必须在其构造方法中调用父类的构造方法
- 一个类默认继承Object类

> 继承的语法

```python
class 子类类名(父类1，父类2):
	pass
```

> 继承实例

**单继承**

```python
# 单继承
class Vip(User):
    def __init__(self, name, age, id):
        super().__init__(name, age)
        self.id = id


vip = Vip('mcl', 20, 'k1021')
vip.show()  # mcl的User的实例方法
print(vip.id)  # k1021
```

**多继承**

```python
# 多继承
# A类
class A:
    def __init__(self, name):
        self.name = name

    def info(self):
        print(self.name, self.__class__)


# B类
class B:
    def __init__(self, age):
        self.age = age

    def info(self):
        print(self.age, self.__class__)


# C类继承A,B类
class C(A, B):
    def __init__(self, name, age, price):
        # A的构造方法
        A.__init__(self, name)
        # B的构造方法
        B.__init__(self, age)
        self.price = price

    # 方法重写
    def info(self):
        print(self.name, self.age, self.price, self.__class__)


a = A('cc')
a.info()  # cc <class '__main__.A'>
b = B(11)
b.info()  # 11 <class '__main__.B'>
c = C('mcl', 11, 21312)
c.info()  # mcl 11 21312 <class '__main__.C'>
```

### Object类

> Object类中自带的方法

```
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__',
'__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', 
'__hash__', '__init__', '__init_subclass__', '__le__', '__lt__',
'__module__', '__ne__', '__new__', '__private__', '__reduce__', 
'__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__',
'__subclasshook__', '__weakref__', 'name']
```

> \_\_str\_\_方法

```python
class Student:
    def __init__(self, name):
        self.name = name

    def __str__(self):
        return '传入一个name--》' + self.name + '  相当于toString方法'
    def __private__(self):
        print('没有后面两个斜杠的私有方法')

stu = Student('mcl')
print(dir(stu))
# 直接输出对象就是调用对象的__str__方法
print(stu)  # 传入一个name--》mcl  相当于toString方法
stu.__private__()
```

### python的多态

- Java中其实就是父类型可以接受子类的对象，在python中没有父类型这个概念，所以感觉python的多态表现得不是很明显，但他毕竟是面向对象的，所以还是有多态的性质的

- python的多态性更多可以体现在方法上，Java更多体现在对象上。python一个函数可以接受其他各种不同类的对象，并调用他们的方法，是方法上的多态
- python是动态语言，Java是静态语言。因此，python的多态体现在方法上，Java的多态体现在对象上

> 实例

```python
# 父类
class Animal:
    # name='animal'
    def __init__(self, name):
        self.name = name

    def eat(self):
        print(self.name, 'is eating !')

# 子类1
class Bird(Animal):
    def __init__(self, name):
        super().__init__(name)

# 子类2
class Dog(Animal):
    def __init__(self, name):
        super().__init__(name)

# 通过一个函数承载多个对象，只关注于对象有没有这个方法
def fun(obj):
    obj.eat()


a = Animal('animal')
b = Bird('bird')
d = Dog('dog')
fun(a)  # animal is eating !
fun(b)  # bird is eating !
fun(d)  # dog is eating !
```

## 特殊方法和属性

|          |     名称      |                           描述                            |
| :------: | :-----------: | :-------------------------------------------------------: |
| 特殊属性 | \_\_dict\_\_  |      获得类或者实例对象所绑定的所有属性和方法的字典       |
| 特殊方法 | \_\_len\_\_() | 通过重写\_\_len__()方法，让内置函数len()的参数可以自定义  |
| 特殊方法 | \_\_add\_\_() |  通过重写\_\_add__()方法，可使用自定义对象具有“+”的功能   |
| 特殊方法 |  \_\_new__()  | 用于创建对象，new方法先于init方法，有返回值，返回实例对象 |
| 特殊方法 | \_\_init__()  |                  对创建的对象进行初始化                   |

> 特殊属性

```python
# 特殊属性
x = Dog('cc', 6, 3500)
# 对实例的属性字典
print(x.__dict__)  # {'name': 'cc', 'age': 6, 'price': 3500}
# 对类的属性:方法字典
# {'__module__': '__main__', '__init__': <function Dog.__init__ at 0x00000212DD812160>, '__doc__': None}
print(Dog.__dict__)  
# __class__输出对象的类型
print(x.__class__)  # <class '__main__.Dog'>
# 查看继承的类（父类）__bases__ 返回的是元组
print(Dog.__bases__)  # (<class '__main__.Animal'>,)
# __base__查看第一个父类
print(Dog.__base__)  # <class '__main__.Animal'>
# 查看继承结构 __mro__   :  Dog继承Animal继承Object
print(Dog.__mro__)  # (<class '__main__.Dog'>, <class '__main__.Animal'>, <class 'object'>)
# 查看类的子类
print(Animal.__subclasses__())  # [<class '__main__.Bird'>, <class '__main__.Dog'>]
```

> 特殊方法 : \_\_add__() 方法和 \_\_len\_\_() 方法

- \_\_add__()方法

这个方法相当于 + 号，对于int类型来说，相加操作其实就是调用了这个方法，但是对于对象来说，这个方法是不允许的，但是可以通过重写来强行实现

- \_\_len__()方法

这个方法和内置函数len()方法的作用是一样的，对于序列来说都是计算长度，但对于对象来说，不重写这个方法就是不允许获取长度，所以需要重写才能获取对象的长度

```python
class A:
    def __init__(self, name):
        self.name = name

    def __add__(self, other):
        return self.name + other.name

    def __len__(self):
        return len(self.name)

a = 20
b = 30
c = a + b
print(c)
# + 等同于
print(a.__add__(b))

a1 = A('cc')
a2 = A('dd')
print(a1 + a2)  # 不重写__add__()方法会报错,重写之后就是ccdd

lis = [11, 22, 33, 45, 66]
print(len(lis))  # 5 len这个内置函数其实本质上就是调用了__len__()方法
print(lis.__len__())  # 5
a3 = A('Rose')
print(len(a3))  # 4 不重写__len__()方法会报错,重写之后就是 4
```

> 特殊方法：\_\_new__() 方法和 \_\_init\_\_() 方法

​	new方法和init方法都是创建对象时使用的方法，new方法先于init方法，先通过new方法创建实例对象，这个实例对象就是self，再将这个self赋给init方法对其进行初始化赋值

```python
class Person:
    def __new__(cls, *args, **kwargs):
        print('__ new__方法调用，Person对象产生')
        print('Person类的id为:{0}'.format(id(cls)))  # 0272
        obj = super().__new__(cls)
        print('创建的Person对象,id为:{0}'.format(id(obj)))  # 3008 父类的new方法创建Person对象
        return obj

    def __init__(self, name, age):
        print('__init__方法被调用')
        print('init方法被调用时，self的id：{0}'.format(id(self)))  # 3008 new方法之后self就指对象本身
        self.name = name
        self.age = age


print('Object类的id为：{0}'.format(id(object)))  # 9568
print('Person类的id为:{0}'.format(id(Person)))  # 0272

p1 = Person('mcl', 11)
print('p1为Person的实例对象，p1的id为:{0}'.format(id(p1)))  # 3008
```

### 拷贝

使用拷贝需要加入API

> 浅拷贝

浅拷贝生成的对象会有新的地址，但是他包含的对象地址不会改变，都指向同一个对象

python的拷贝一般都是浅拷贝

```python
import copy
class A:
    def __init__(self, name):
        self.name = name


a = A('cc')
b = a
print(a)  # 4BB0
print(b)  # 4BB0
# 浅拷贝，对象不同，但他的子对象仍然和被复制的对象的子对象指向同一个地址
b = copy.copy(a)
print(a, id(a.name))  # 4BB0  0352
print(b, id(b.name))  # 6160  0352
```

> 深拷贝

使用copy模块的deepcopy函数，递归拷贝对象中包含的子对象，源对象和拷贝对象所有的子对象地址都不同

## 模块化

python程序就是由模块组成的，一个模块就是一个.py文件 ，文件中可以包含函数、类、语句。

模块与模块之间的变量彼此并不影响

> 自定义模块

新建一个.py文件，名称不要与python自带的标准模块相同就行

> 导入模块

- import  模块名称  [ as别名 ]

- from  模块名称  import  函数/变量/类

```python
import math

print(dir(math))
# 指数计算 直接使用pow用的不是math的包
print(pow(2, 3), type(pow(2, 3)))  # 8 <class 'int'>
print(math.pow(2, 3), type(math.pow(2, 3)))  # 8.0 <class 'float'>
# 计算大于自身的最小整数
print(math.ceil(9.000000001))  # 10
# 计算小于自身的最大整数(小数点后16位之后就是相当于整数了)
print(math.floor(9.999999999999999))  # 9

from math import pi

print(pi)  # 3.141592653589793
```

> 自定义模块

很奇怪，教学上面直接导入同包下的模块时，会报错，必须将包设置为Resource包才能正常导入，但我在实际测试中，发现即使不用标记，普通包也可以直接使用，可能是新版优化了吧

```python
#calc模块
def add(x,y):
    return x+y
def div(x,y):
    return x/y
```

```python
# 调用模块
import calc
print(calc.add(2, 3))  # 5
```

> 主程序形式运行

**格式：**

```python
if __name__ = '__main__':
	pass
```

**作用：**当我们调用其他模块时，其他模块中可能有许多直接写在类、函数外面的一些执行语句，在调用这个模块时，也会同时执行这些语句，为了实现这些语句只在当前模块执行时执行，而不是调用时执行，可以用这个方法来区分。

```python
# calc 被调用模块
def add(x, y):
    return x + y

def div(x, y):
    return x / y

#  加入这个之后，这个函数所包含的语句只在本模块运行时运行，被调用时不会执行
if __name__ == '__main__':
    print('calc模块---->' + str(add(1, 2)))
```

```python
# 调用模块
import calc

# 不加入主程序运行方式
print(calc.add(10, 20))
'''
会输出被调用的输出语句
结果为：
    calc模块---->3
    30
'''

# 加入主程序运行方式
print(calc.add(10, 20))  # 30
```

## 包

> 包与目录的区别

- 包含\_\_init\_\_.py的文件的目录称为包
- 目录里通常不包含\_\_init\_\_.py文件
- 导入包：import  包名.模块名

```python
# 模块A
a=10
# 模块B
c = 20
```

```python
# 调用模块
import package1_1.module_A as ma

print(ma.a)  # 10
import package1_1.module_B as mb

print(mb.c)  # 20
```

> import和from...import的区别

- import只能导入包名或者模块名，具体的方法和属性不能导入
- form...import不仅能导入包名和模块名，还能导入具体的属性方法函数等

```python
# import方式
import package1_1.module_A as ma
import package1_1.module_B as mb

print(ma.a)  # 10
print(mb.c)  # 20

# form...import方式
# 导入属性
from package1_1.module_A import a
# 导入方法
from package1_1.module_B import mbfun

print(a)  # 10
print(mbfun(1))  # 1
```

## python的内置模块

| 模块名   | 描述                                                       |
| -------- | ---------------------------------------------------------- |
| sys      | 与python解释器及其环境相关的标准库                         |
| time     | 提供与时间相关的各种标准库                                 |
| os       | 提供访问操作系统服务功能的标准库                           |
| calendar | 提供与日期相关的各种函数的标准库                           |
| urllib   | 用于读取来自网上（服务器）的数据标准库                     |
| json     | 用于使用JSON序列化与反序列化对象                           |
| re       | 用于在字符串中执行正则表达式匹配和替换                     |
| math     | 提供标准数学运算函数的标准库                               |
| decimal  | 用于进行精确控制运算精度、有效数位和四舍五入的十进制运算   |
| logging  | 提供了灵活的记录事件、错误、警告和调试信息等日志信息的功能 |

```python
import sys
import time
import urllib.request
import math

# sys模块
print(sys.getsizeof('ccc'))  # 52 占用52个字节
print(sys.getsizeof(False))  # 24 占用24个字节
# time模块
t = time.time()
print(t)  # 输出秒单位的事件：1637473709.4180799
print(time.localtime(t))  # 输出具体的年月日时分秒等
'''
time.struct_time(tm_year=2021, 
                tm_mon=11, 
                tm_mday=21, 
                tm_hour=13, 
                tm_min=48, 
                tm_sec=29, 
                tm_wday=6, 
                tm_yday=325, 
                tm_isdst=0)
'''

# urllib  常用于python爬虫
# request表示这是一个请求，urlopen表示打开这个网页，read方法就是读取网页中的内容
print(urllib.request.urlopen('https://www.baidu.com').read())
# JSON 和 urllib常常一起使用，用于处理爬虫爬出来的信息

# math
print(math.pi)  # 3.141592653589793

# decimal  用于处理精确的数据，又是浮点数会出现一些不准确的情况，可以用decimal校准

# logging 日志模块
```

## 第三方模块的安装与使用

> 第三方模块的安装

pip  install  模块名

> 第三方模块的使用

import  模块名

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211121141806707.png" alt="image-20211121141806707" style="zoom:80%;" /> 

> 例子：schedule的使用

```python
import schedule  # 定期化
import time

def fun():
    print('每2s执行一次------------')

schedule.every(2).seconds.do(fun)

while True:
    schedule.run_pending()
    time.sleep(1)
```

## 编码以及文件操作

### 常见的字符编码格式

- Python的解释器使用的是Unicode（内存）
- .py文件在磁盘上使用的是UTF-8存储（外存）

```
若是需要修改.py文件的编码格式，在文件最上面加上以下代码（以GBK为例）： 
	#encoding = GBK 
就可以了
```

### 文件读写

**python操作文件 ---> 打开或者新建文件 ---> 读写文件 ---> 关闭资源**

python代码被解释器解释之后，解释器通过操作系统对硬盘上的文件进行操作

> 语法规则

```python
'''
file:文件对象
open:创建文件对象（打开文件）
filename:要打开、创建的文件名称
mode:打开模式，默认只读
encoding:默认文本格式为GBK
'''
file = open(filename [,mode,encoding])
```

> 实例：读取磁盘内容

```python
# a.txt文件在同一个包中
file = open('a.txt','r',encoding='UTF-8')
print(file.readlines())
file.close()
```

> 常用的文件打开模式

| 打开模式 | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| r        | 以只读模式打开文件，文件的指针放在文件的开头                 |
| w        | 以只读模式打开文件，如果文件不存在就创建，如果文件存在就覆盖原有内容，文件的指针在文件的开头 |
| a        | 以追加模式打开文件，如果文件不存在就创建，文件指针在文件开头，如果文件存在，就在文件末尾追加内容，文件指针在原文件末尾 |
| b        | 以二进制方式打开文件，不能单独使用，需要与其他模式一起使用，rb或者wb |
| +        | 以读写方式打开文件，不能单独使用，需要与其他模式一起使用，a+ |

```python
# 只读文件
file = open('a.txt','r',encoding='UTF-8')
print(file.readlines())
file.close()

# 重写文件
file = open('b.txt','w',encoding='UTF-8')
file.write('helloworld')
file.close()

# 追加文件内容
file = open('b.txt','a',encoding='UTF-8')
file.write(',python')
file.close()

# 二进制文件的读写复制操作
src_file = open('a.txt','rb')
copy_file = open('copy_a.txt','wb')
copy_file.write(src_file.read())
src_file.close()
copy_file.close()
```

> 文件对象的常用方法

| 方法名                      | 说明                                                         |
| --------------------------- | ------------------------------------------------------------ |
| read ( [ size ] )           | 从文件中读取size个字节或者字符的内容返回，若省略size就表示读完整个文件 |
| readline ()                 | 从文本中读取一行数据                                         |
| readlines ()                | 将文本中的每一行都作为独立的字符串对象，并将这些对象放入列表返回 |
| write ( str )               | 将字符串str内容写入文件                                      |
| writelines ( s_list )       | 将字符串列表s_list写入文本文件，不添加换行符                 |
| seek ( offset [ , where ] ) | 将文件指针移动到新的位置，offset表示相对于whence的位置：<br />offset : 正数表示往结束方向移动，负数表示往开始方向移动<br />whence：不同的值表示不同的含义：<br /></t>0：表示从文件头开始计算<br /></t>1：从当前位置开始计算<br /></t>2：从文件尾开始计算 |
| tell ()                     | 返回文件指针的当前位置                                       |
| flush ()                    | 把缓冲区的内容写入文件，但不关闭文件                         |
| close ()                    | 把缓冲区的内容写入文件，同时关闭文件，释放文件对象相关资源   |

```python
# read seek tell
a = open('a.txt','r',encoding='utf-8')
a.seek(6) # 设置文件读取的开始位置
print(a.read())
print(a.tell())  # 文件当前位置，因为读完了，所以在文件末尾
a.close()

# flush  close
a = open('a.txt', 'a+', encoding='utf-8')
a.write('\n what the fxxk')  # 追加内容
a.flush() # 将缓冲区信息写入文件中
a.write('\n who is your daddy?') #写入之后仍然可以继续写
a.close() # 将缓冲区写入文件并关闭流
```

### with语句（上下文管理器）

> 作用

with语句可以自动管理上下文资源，不论什么原因跳出with块，都能确保文件的正确关闭，以此来达到释放资源的目的

> 语法

```python
# open('logo.png','rb')这句话被称为上下文语句
with open('logo.png','rb') as src_file:
	# src_file.read()
	# with语句体
	pass
```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20211121152441024.png" alt="image-20211121152441024" style="zoom: 80%;" /> 

> 原理

实现了 \_\_enter\_\_() 方法和 \_\_exit\_\_() 方法的类称该类遵守了上下文管理器协议，那么该类的实例对象被称为上下文管理器

```python
class MyTest():
    def __enter__(self):
        print('enter方法执行了')
        return self   # 进入with语句时自动调用

    def __exit__(self, exc_type, exc_val, exc_tb):
        print('exit方法执行了')  # 退出with语句时自动调用

    def show(self):
        print('这个类遵守了上下文管理协议')


# 不用手动关闭
with open('b.txt', 'rb') as src_file:
    print(src_file.read())
# 自定义了一个遵守上下文管理协议类
with MyTest() as mt:
    mt.show()
    '''
        enter方法执行了
        这个类遵守了上下文管理协议
        exit方法执行了
    '''
```

## 目录操作

- os模块，是python内置的与操作系统功能和文件系统相关的模块，该模块的运行结果通常与操作系统有关，不同操作系统得到的结果可能不同
- os模块与os.path模块用于对目录或文件进行操作

### os

> 应用相关

```python
# 与command命令相关
os.system('motepad.exe') # 打开记事本
os.system('calc.exe')  # 打开计算器
# 打开应用
os.startfile('C:\\Users\\Administrator\\Desktop\\hadoop.md') #打开桌面的hadoop.md文件
```

> 目录相关

| 函数                                 | 说明                           |
| ------------------------------------ | ------------------------------ |
| getcwd()                             | 返回当前的工作目录             |
| listdir( path )                      | 返回指定路径下的文件和目录信息 |
| mkdir( path[ ,model ] )              | 创建目录                       |
| makedirs( path1/path2...[ ,model ] ) | 创建多级目录                   |
| rmdir( path )                        | 删除目录                       |
| removedirs( path1/path2.....)        | 删除多级目录                   |
| chdir( path )                        | 将path设置为当前工作目录       |

```python
import os
#获取当前工作目录
print(os.getcwd())  # F:\PyProject\test02\OSTest

# 获取目录下的文件，以当前目录为起始位置
lisdir = os.listdir('../IOTest')
'''
    ['a.txt', 'b.txt', 'copy_a.txt', 'demo01.py', 'demo02.py', 'demo03.py', '__init__.py']
'''
print(lisdir)

# 在当前目录下创建一个新的子目录
os.mkdir('newdir')
# 在当前目录下创建一个新的多级目录
os.makedirs('A/B/C/D')
# 删除当前目录下的一个子目录
os.rmdir('PP')
# 删除当前目录下的一个多级目录
os.removedirs('a/b/c')
# 切换工作目录
os.chdir('..\innerFun')
print(os.getcwd(), os.listdir())
'''
    F:\PyProject\test02\innerFun   ['demo01.py', 'demo02.py', '__init__.py']
'''
```

### os.path

> 方法

| 函数                | 说明                                                    |
| ------------------- | ------------------------------------------------------- |
| abspath( path )     | 获取文件或者目录的绝对路劲                              |
| exists( path )      | 判断文件或者目录是否存在，存在返回True，不存在返回False |
| join( path , name ) | 将目录与目录或文件名拼接起来                            |
| splitext()          | 分离文件名和扩展名                                      |
| basename( path )    | 从一个目录中提取文件名                                  |
| dirname( path )     | 从一个路径中提取文件路径，不包括文件名                  |
| isdir( path )       | zz                                                      |

```python
import os.path as op

# 获取当前目录下文件或目录的绝对路径
p = op.abspath('demo02.py')
print(p)  # F:\PyProject\test02\OSTest\demo02.py
# 判断文件或者目录是否存在
print(op.exists('demo02.py'), op.exists('aa.txt'))  # True False
# 拼接目录与目录或文件名
print(op.join('c:\\test', 'test11111'))  # c:\test\test11111
# 分离文件和路径
print(op.split('c:\\test\\a.py'))  # ('c:\\test', 'a.py')
# 分离文件名和扩展名
print(op.splitext('a.py'))  # ('a', '.py')
# 从一个目录中提取文件名
print(op.basename('c:\\ca\\cxz\\c\\a.txt'))  # a.txt
# 从一个路径中提取文件路径，不包括文件名
print(op.dirname('c:\\ca\\cxz\\c\\a.txt'))  # c:\ca\cxz\c
# 判断是否为路径
print(op.isdir('c:\\ca\\cxz\\c\\a.txt'))  # False
print(op.isdir('..\\IOTest'))  # True

```

> 实例1：遍历某一目录下的所有.py文件

```python
#应用实例：获取所有的python文件
import os
#切换工作目录为test01目录
os.chdir('../../test01')
# 查看当前工作目录
workdir = os.getcwd()
print(workdir)
# 获取目录下的所有文件
lis_dir_file = os.listdir(workdir)
print(lis_dir_file)
# 遍历，以.py结尾就打印出来
for filename in lis_dir_file:
    if filename.endswith('.py'):
        print(filename)
```

> 实例2：walk获取目录及其子目录下的所有.py文件

```python
import os
import os.path as op
os.chdir('../../test01')
path = os.getcwd()
list_files = os.walk(path)
for dirpath,dirname,filename in list_files:
    # print('目录路径:{0} \n目录名称:{1} \n文件名:{2}'.format(dirpath,dirname,filename))
    print('-------------目录----------------')
    for dir in dirname:
        print(op.join(dirpath,dir))
    print('-------------文件----------------')
    for file in filename:
        print(op.join(dirpath,file))
```

**结果**

```
-------------目录----------------
F:\PyProject\test01\A
-------------文件----------------
F:\PyProject\test01\function.py
F:\PyProject\test01\function01.py
F:\PyProject\test01\function2.py
F:\PyProject\test01\hello.py
F:\PyProject\test01\ifTest.py
F:\PyProject\test01\inputTest.py
F:\PyProject\test01\listR.py
F:\PyProject\test01\print不换行.py
F:\PyProject\test01\三目.py
-------------目录----------------
F:\PyProject\test01\A\B
-------------文件----------------
-------------目录----------------
F:\PyProject\test01\A\B\C
-------------文件----------------
-------------目录----------------
-------------文件----------------

Process finished with exit code 0

```

## 打包

1. 安装第三方库

   ```shell
   F:\PyProject> pip install PyInstall
   ```

2. 打包

   ```shell
    F:\PyProject> pyinstall -F F:\PyProject\test02\OSTest\demo04.py
   ```

3. 运行

   打包之后cmd会显示exe文件存放路径，根据路径找到exe文件打开就直接可以运行了

