# Python语法笔记

## Python基础语法



### 标识符

- 第一个字符必须是字母表中字母或下划线 _
- 标识符的其他的部分由字母、数字和下划线组成。
- 标识符对大小写敏感。
- Python关键字不能用作标识符。



### 注释

- 单行注释以#开头

- 多行注释可采用多个#，或‘’，“”



### 行与缩进

- 以缩进表示代码块
- 同一代码块缩进的空格数相同
- 可用Tab键缩进或固定每次缩进2或4个空格，
- ![image-20220926221219509](D:/Photo/本地图床/photobedVer2/image-20220926221219509.png)





### 数字类型（Number）



- int (整数),如 1,只有一种整数类型 int。
- bool(布尔),如 True。
- float(浮点数),如1.23、3E-2
- complex(复数),如1+2j、1.1+2.2j



### 字符串

以引号（’）、双引号（”）、三引号（’’’或”””）表示字符串

![image-20220926221502980](D:/Photo/本地图床/photobedVer2/image-20220926221502980.png)




### 代码组

- 缩进相同的一组语句构成一个代码组

- 如if(if...else)、while、def、class等复合语句，首行以关键字开始，以冒号：结束，该行之后的一行或多行代码构成代码组

 ```python
  if True:
  	print('Yes')
  	print('Hello')
  else:
  	print('No')
 ```



### Print语句

- print输出默认换行
- 要实现不换行，需在变量末尾加 end=“”





### Input语句

实现用户输入

- 如：num=input("请输入一个三位整数：")
- 用户输入的数字将以字符串的形式存储到num字符串变量中
-  后续需要改变格式来进行数值操作（将字符串输出为数字）



## Python3基本数据类型

- Number（数字）
- String（字符串）
- List（列表）
- Tuple（元组）
- Set（集合）
- Dictionary（字典）



- 其中Number（数字）、String（字符串）、Tuple（元组）为**不可变数据**
- List（列表）、Set（集合）、Dictionary（字典）为可变数据



### Number（数字）

#### 数字类型转换

- int(x)将x转换为一个整数
- float(x)将x转换到一个浮点数
- complex(x)将x转换到一个复数，实数部分为x，虚数部分为0
- complex(x,y)将x和 y 转换到一个复数，实数部分为x，虚数部分为y

#### 数字运算

- 十（加），-（减），*（乘）, /（除）
- / 整数除法返回浮点型
- // 返回结果向下取整值
- % 返回余数
- **幂运算



### String（字符串）

- 字符串截取的语法格式，变量[头下标:尾下标]



#### 字符串运算符

- +，用于字符串连接
- *，重复输出字符串
- []，通过索引获取字符串中的字符
- [:]，截取字符串中的一部分，左闭右开
- in，如果字符串中包含给定字符，返回True
- not in，如果字符串中不包含给定字符，返回True



#### 格式化字符串

- %s 格式化字符串

- %d 格式化整数

- EX:  

  ```python
  print(小明出生于%s，英文名字%s，今年%d岁('上海',Bob',10))
  ```



### List（列表）

由一系列按特定顺序排列的元素组成，用方括号[] 表示列表，并用逗号分隔其中的元素

#### 创建列表

```python
friends = ['Alice', 'Bob', 'Mike', 'David’]
print('创建的列表：', friends)
```

#### 获取列表的长度

```python
len(friends)
```

#### 修改列表元素

```python
friends.append('Alice') #append方法在列表末尾添加元素
friends.insert(0,'XiaoMing') #insert()方法在指定位置添加元素
```

#### 删除列表元素

```python
del friends[0] #知道待元素索引，可用del删除
friend = friends.pop(2) #删除列表指定索引元素并保存该值
friends.remove('Bob') #remove()方法删除某一值元素
```

#### 对列表元素进行排序

```python
friends.sort() #sort方法是永久的，按字母顺序排序
friends.sort(reverse=True) #表示按字母顺序相反排序
sorted(friends) #sorted方法排序是临时的
```



### Tuple（元组）

- 元组是不可变的，用圆括号()来标识，元素用逗号隔开
- 访问元组元素的方法与列表相同
- 不能单独改变元组内的元素，但可给存储元组的变量重新赋值

#### 创建元组

```python
dimensions = (200,50)
print(dimensions[0]) #output: 200
```



### Set（集合）

- 是一个无序不重复元素的序列
- 可用大括号{}或set（）函数创建集合
- 创建空集合只能用set（）,因为{}用来表示空字典

#### 创建集合

```python
basket = {'apple', 'orange', 'apple', 'pear', 'orange',
'banana’}  #集合只保留非重复元素
```

#### 添加元素

```python
basket.add() #add用于添加元素
```

#### 移除元素

```python
basket.remove() # remove用于移除元素
```

#### 清空集合

```python
basket.clear() #clear用于清空集合
```



### Dictionary（字典）

- 字典可储存任意类型的对象，用花括号①标识
- 其中的键值对（key:value）用冒号：分割，键必须是唯一的，但值不必
- 值可取任何数据类型，但键必须是不可变的

#### 创建字典

```python
d = {'name': 'Alice', 'age': 18, 'Class': 'First'}
```

#### 修改字典

```python
d['age'] = 20
d['school'] = 'SJTU'
```

#### 删除字典元素

```python
del d['Class'] #删除键Class
d.clear
```









## 条件控制

条件语句是通过一条或多条语句的执行结果（True or False）来决定执行的代码块

if... else

if...elif...else...

```python
if guess == number:
	print("恭喜，你猜对了！”)
else:
	print("猜错了...")
```



## 循环语句

### for循环

```python
#可以遍历任何序列的项目，如一个列表或者一个字符串
for <variable> in <sequence>:
	<statements>
else:
	<statements>
```



### while循环

```
while <判断条件>:
	<statements>
```



### break & continue





## 函数

- 函数是组织好的，可重复使用的，用来实现单一或相关联功能的代码段

- 函数能提高应用的模块性和代码的重复利用率

- 定义一个函数

  - 函数代码块以def关键词开头，后接函数标识符名称和圆括号 O。
  - 任何传入参数和自变量必须放在圆括号中间，圆括号之间可以用于定义参数。
  - 函数内容以冒号起始，并且缩进。
  - return [表达式] 结束函数，选择性地返回一个值给调用方。不带表达式的return相当于返回None。

- def 函数名（参数列表）：

  ​			函数体

  ​		return []

## 模块

- 模块
  - 包含所有定义的函数和变量的文件，其后缀名是.py
  - 模块可以被别的程序引入，以使用该模块中的函数等功能
  - 是使用python标准库的方法

- Python数据分析常用的模块（库）

  - numpy
    - 科学计算基础包,提供矩阵数据类型、矢量处理，以及精密的运算库
  - pandas
    - 主要提供快速便捷地处理结构化数据的大量数据结构和函数
  - matplotlib
    - 用于绘制数据图表
  - sklearn
    - 基于python的机器学习库，可以方便进行机器学习算法的实施，包括：分类、回归、聚类、降维、模型选择和预处理等数据挖掘的相关算法

- 第三方库的安装

  >Windows系统
  >▪ 方式一pip install package_name
  >▪ pip在安装python3时就已经安装
  >▪ 方式二conda install package_name
  >▪ 需要安装Ananconda或miniconda使用
  >Macos系统
  >▪ python3 -m pip install package_name
  >▪ 例子
  >▪ python3 -m pip install sklearn
  >▪ python3 -m pip install tensorflow  



### 导入模块

**import与from...import**

- 将整个模块(somemodule)导入，格式为：
  -  import somemodule
  - 后加as来为模块起别名方便引用，例如，import pandas as pd
- 从某个模块导入某个函数，格式为：
  - from somemodule import somefunction
  - 例如，from matplotlib import pyplot
- 从某个模块中带入多个函数，格式为：
  - from somemodule import firstfunc, secondfunc, thirdfunc  
- 将某个模块中的全部函数导入，格式为
  - from somemodule import *



## PEP8 (Python Enhancement Proposal)

完整内容：https://www.python.org/dev/peps/pep-0008/

**PEP8是python推荐的编码规范，目的是使代码美观并方便分销，主要由以下几条：**

- 使用4个空格缩进
- 在运算符前后和逗号后使用空格，但不能直接在括号内使用：
- 如果可能，把注释放到单独的一行
- 使用空行分隔函数和类，以及函数内的较大的代码块
- 以一致的规则为你的函数等命名；习惯上使用小写字符加下划线命名函数、方法、包和模块


