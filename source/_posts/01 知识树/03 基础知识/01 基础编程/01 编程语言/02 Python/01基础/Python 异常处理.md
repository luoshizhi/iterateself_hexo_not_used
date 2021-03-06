---
title: Python 异常处理
toc: true
date: 2018-06-11 08:14:43
---
---
author: evo
comments: true
date: 2018-05-03 10:25:20+00:00
layout: post
link: http://106.15.37.116/2018/05/03/python-except/
slug: python-except
title: Python 异常处理
wordpress_id: 5000
categories:
- 随想与反思
---

<!-- more -->

[mathjax]


# REFERENCE





 	
  1. [python基础教程 w3cschool](https://www.w3cschool.cn/python/)

 	
  2. [Python 3 教程 菜鸟教程](http://www.runoob.com/python3/python3-tutorial.html)




# TODO





 	
  * aaa




# MOTIVE





 	
  * aaa





* * *





## Python 异常处理


python提供了两个非常重要的功能来处理python程序在运行中出现的异常和错误。你可以使用该功能来调试python程序。



 	
  * 异常处理: 本站Python教程会具体介绍。

 	
  * 断言(Assertions):本站Python教程会具体介绍。





* * *





## python标准异常


<table class="reference " >
<tbody >
<tr >
异常名称
描述
</tr>
<tr >

<td >BaseException
</td>

<td >所有异常的基类
</td>
</tr>
<tr >

<td >SystemExit
</td>

<td >解释器请求退出
</td>
</tr>
<tr >

<td >KeyboardInterrupt
</td>

<td >用户中断执行(通常是输入^C)
</td>
</tr>
<tr >

<td >Exception
</td>

<td >常规错误的基类
</td>
</tr>
<tr >

<td >StopIteration
</td>

<td >迭代器没有更多的值
</td>
</tr>
<tr >

<td >GeneratorExit
</td>

<td >生成器(generator)发生异常来通知退出
</td>
</tr>
<tr >

<td >StandardError
</td>

<td >所有的内建标准异常的基类
</td>
</tr>
<tr >

<td >ArithmeticError
</td>

<td >所有数值计算错误的基类
</td>
</tr>
<tr >

<td >FloatingPointError
</td>

<td >浮点计算错误
</td>
</tr>
<tr >

<td >OverflowError
</td>

<td >数值运算超出最大限制
</td>
</tr>
<tr >

<td >ZeroDivisionError
</td>

<td >除(或取模)零 (所有数据类型)
</td>
</tr>
<tr >

<td >AssertionError
</td>

<td >断言语句失败
</td>
</tr>
<tr >

<td >AttributeError
</td>

<td >对象没有这个属性
</td>
</tr>
<tr >

<td >EOFError
</td>

<td >没有内建输入,到达EOF 标记
</td>
</tr>
<tr >

<td >EnvironmentError
</td>

<td >操作系统错误的基类
</td>
</tr>
<tr >

<td >IOError
</td>

<td >输入/输出操作失败
</td>
</tr>
<tr >

<td >OSError
</td>

<td >操作系统错误
</td>
</tr>
<tr >

<td >WindowsError
</td>

<td >系统调用失败
</td>
</tr>
<tr >

<td >ImportError
</td>

<td >导入模块/对象失败
</td>
</tr>
<tr >

<td >LookupError
</td>

<td >无效数据查询的基类
</td>
</tr>
<tr >

<td >IndexError
</td>

<td >序列中没有此索引(index)
</td>
</tr>
<tr >

<td >KeyError
</td>

<td >映射中没有这个键
</td>
</tr>
<tr >

<td >MemoryError
</td>

<td >内存溢出错误(对于Python 解释器不是致命的)
</td>
</tr>
<tr >

<td >NameError
</td>

<td >未声明/初始化对象 (没有属性)
</td>
</tr>
<tr >

<td >UnboundLocalError
</td>

<td >访问未初始化的本地变量
</td>
</tr>
<tr >

<td >ReferenceError
</td>

<td >弱引用(Weak reference)试图访问已经垃圾回收了的对象
</td>
</tr>
<tr >

<td >RuntimeError
</td>

<td >一般的运行时错误
</td>
</tr>
<tr >

<td >NotImplementedError
</td>

<td >尚未实现的方法
</td>
</tr>
<tr >

<td >SyntaxError
</td>

<td >Python 语法错误
</td>
</tr>
<tr >

<td >IndentationError
</td>

<td >缩进错误
</td>
</tr>
<tr >

<td >TabError
</td>

<td >Tab 和空格混用
</td>
</tr>
<tr >

<td >SystemError
</td>

<td >一般的解释器系统错误
</td>
</tr>
<tr >

<td >TypeError
</td>

<td >对类型无效的操作
</td>
</tr>
<tr >

<td >ValueError
</td>

<td >传入无效的参数
</td>
</tr>
<tr >

<td >UnicodeError
</td>

<td >Unicode 相关的错误
</td>
</tr>
<tr >

<td >UnicodeDecodeError
</td>

<td >Unicode 解码时的错误
</td>
</tr>
<tr >

<td >UnicodeEncodeError
</td>

<td >Unicode 编码时错误
</td>
</tr>
<tr >

<td >UnicodeTranslateError
</td>

<td >Unicode 转换时错误
</td>
</tr>
<tr >

<td >Warning
</td>

<td >警告的基类
</td>
</tr>
<tr >

<td >DeprecationWarning
</td>

<td >关于被弃用的特征的警告
</td>
</tr>
<tr >

<td >FutureWarning
</td>

<td >关于构造将来语义会有改变的警告
</td>
</tr>
<tr >

<td >OverflowWarning
</td>

<td >旧的关于自动提升为长整型(long)的警告
</td>
</tr>
<tr >

<td >PendingDeprecationWarning
</td>

<td >关于特性将会被废弃的警告
</td>
</tr>
<tr >

<td >RuntimeWarning
</td>

<td >可疑的运行时行为(runtime behavior)的警告
</td>
</tr>
<tr >

<td >SyntaxWarning
</td>

<td >可疑的语法的警告
</td>
</tr>
<tr >

<td >UserWarning
</td>

<td >用户代码生成的警告
</td>
</tr>
</tbody>
</table>


## 什么是异常？


异常即是一个事件，该事件会在程序执行过程中发生，影响了程序的正常执行。

一般情况下，在Python无法正常处理程序时就会发生一个异常。

异常是Python对象，表示一个错误。

当Python脚本发生异常时我们需要捕获处理它，否则程序会终止执行。



* * *





## 异常处理


捕捉异常可以使用try/except语句。

try/except语句用来检测try语句块中的错误，从而让except语句捕获异常信息并处理。

如果你不想在异常发生时结束你的程序，只需在try里捕获它。

语法：

以下为简单的_try....except...else_的语法：

    
    try:
    <语句>        #运行别的代码
    except <名字>：
    <语句>        #如果在try部份引发了'名字'异常
    except <名字>，<数据>:
    <语句>        #如果引发了'名字'异常，获得附加的数据
    else:
    <语句>        #如果没有异常发生
    


try的工作原理是，当开始一个try语句后，python就在当前程序的上下文中作标记，这样当异常出现时就可以回到这里，try子句先执行，接下来会发生什么依赖于执行时是否出现异常。



 	
  * 如果当try后的语句执行时发生异常，python就跳回到try并执行第一个匹配该异常的except子句，异常处理完毕，控制流就通过整个try语句（除非在处理异常时又引发新的异常）。

 	
  * 如果在try后的语句里发生了异常，却没有匹配的except子句，异常将被递交到上层的try，或者到程序的最上层（这样将结束程序，并打印缺省的出错信息）。

 	
  * 如果在try子句执行时没有发生异常，python将执行else语句后的语句（如果有else的话），然后控制流通过整个try语句。




### 实例


下面是简单的例子，它打开一个文件，在该文件中的内容写入内容，且并未发生异常：

    
    #!/usr/bin/python
    
    try:
       fh = open("testfile", "w")
       fh.write("This is my test file for exception handling!!")
    except IOError:
       print "Error: can\'t find file or read data"
    else:
       print "Written content in the file successfully"
       fh.close()
    


以上程序输出结果：

    
     Written content in the file successfully
    




### 实例


下面是简单的例子，它打开一个文件，在该文件中的内容写入内容，但文件没有写入权限，发生了异常：

    
    #!/usr/bin/python
    
    try:
       fh = open("testfile", "w")
       fh.write("This is my test file for exception handling!!")
    except IOError:
       print "Error: can\'t find file or read data"
    else:
       print "Written content in the file successfully"
    


以上程序输出结果：

    
    Error: can't find file or read data
    





* * *





## 使用except而不带任何异常类型


你可以不带任何异常类型使用except，如下实例：

    
    try:
       You do your operations here;
       ......................
    except:
       If there is any exception, then execute this block.
       ......................
    else:
       If there is no exception then execute this block. 
    


以上方式try-except语句捕获所有发生的异常。但这不是一个很好的方式，我们不能通过该程序识别出具体的异常信息。因为它捕获所有的异常。



* * *





## 使用except而带多种异常类型


你也可以使用相同的except语句来处理多个异常信息，如下所示：

    
    try:
       You do your operations here;
       ......................
    except(Exception1[, Exception2[,...ExceptionN]]]):
       If there is any exception from the given exception list, 
       then execute this block.
       ......................
    else:
       If there is no exception then execute this block.  
    





* * *





## try-finally 语句


try-finally 语句无论是否发生异常都将执行最后的代码。

    
    try:
    <语句>
    finally:
    <语句>    #退出try时总会执行
    raise
    


**注意：**你可以使用except语句或者finally语句，但是两者不能同时使用。else语句也不能与finally语句同时使用


### 实例



    
    #!/usr/bin/python
    
    try:
       fh = open("testfile", "w")
       fh.write("This is my test file for exception handling!!")
    finally:
       print "Error: can\'t find file or read data"
    


如果打开的文件没有可写权限，输出如下所示：

    
    Error: can't find file or read data
    


同样的例子也可以写成如下方式：

    
    #!/usr/bin/python
    
    try:
       fh = open("testfile", "w")
       try:
          fh.write("This is my test file for exception handling!!")
       finally:
          print "Going to close the file"
          fh.close()
    except IOError:
       print "Error: can\'t find file or read data"
    


当在try块中抛出一个异常，立即执行finally块代码。

finally块中的所有语句执行后，异常被再次提出，并执行except块代码。

参数的内容不同于异常。



* * *





## 异常的参数


一个异常可以带上参数，可作为输出的异常信息参数。

你可以通过except语句来捕获异常的参数，如下所示：

    
    try:
       You do your operations here;
       ......................
    except ExceptionType, Argument:
       You can print value of Argument here...
    


变量接收的异常值通常包含在异常的语句中。在元组的表单中变量可以接收一个或者多个值。

元组通常包含错误字符串，错误数字，错误位置。


### 实例


以下为单个异常的实例：

    
    #!/usr/bin/python
    
    # Define a function here.
    def temp_convert(var):
       try:
          return int(var)
       except ValueError, Argument:
          print "The argument does not contain numbers\n", Argument
    
    # Call above function here.
    temp_convert("xyz");
    


以上程序执行结果如下：

    
    The argument does not contain numbers
    invalid literal for int() with base 10: 'xyz'
    





* * *





### 触发异常


我们可以使用raise语句自己触发异常

raise语法格式如下：

    
    raise [Exception [, args [, traceback]]]
    


语句中Exception是异常的类型（例如，NameError）参数是一个异常参数值。该参数是可选的，如果不提供，异常的参数是"None"。

最后一个参数是可选的（在实践中很少使用），如果存在，是跟踪异常对象。


### 实例


一个异常可以是一个字符串，类或对象。 Python的内核提供的异常，大多数都是实例化的类，这是一个类的实例的参数。

定义一个异常非常简单，如下所示：

    
    def functionName( level ):
       if level < 1:       raise "Invalid level!", level       # The code below to this would not be executed       # if we raise the exception


**注意：**为了能够捕获异常，"except"语句必须有用相同的异常来抛出类对象或者字符串。

例如我们捕获以上异常，"except"语句如下所示：

    
    try:
       Business Logic here...
    except "Invalid level!":
       Exception handling here...
    else:
       Rest of the code here...
    





* * *





## 用户自定义异常


通过创建一个新的异常类，程序可以命名它们自己的异常。异常应该是典型的继承自Exception类，通过直接或间接的方式。

以下为与RuntimeError相关的实例,实例中创建了一个类，基类为RuntimeError，用于在异常触发时输出更多的信息。

在try语句块中，用户自定义的异常后执行except块语句，变量 e 是用于创建Networkerror类的实例。

    
    class Networkerror(RuntimeError):
       def __init__(self, arg):
          self.args = arg
    


在你定义以上类后，你可以触发该异常，如下所示：

    
    try:
       raise Networkerror("Bad hostname")
    except Networkerror,e:
       print e.args
























* * *





# COMMENT



