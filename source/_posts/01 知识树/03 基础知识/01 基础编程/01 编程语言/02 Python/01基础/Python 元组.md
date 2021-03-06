---
title: Python 元组
toc: true
date: 2018-06-11 22:58:32
---

# TODO


  * 





## Python 元组


Python的元组与列表类似，不同之处在于元组的元素不能修改。

元组使用小括号，列表使用方括号。

元组创建很简单，只需要在括号中添加元素，并使用逗号隔开即可。

如下实例：


    tup1 = ('physics', 'chemistry', 1997, 2000);
    tup2 = (1, 2, 3, 4, 5 );
    tup3 = "a", "b", "c", "d";

创建空元组


    tup1 = ();

元组中只包含一个元素时，需要在元素后面添加逗号


    tup1 = (50,);


元组与字符串类似，下标索引从0开始，可以进行截取，组合等。



* * *





## 访问元组


元组可以使用下标索引来访问元组中的值，如下实例:


    #!/usr/bin/python
    
    tup1 = ('physics', 'chemistry', 1997, 2000);
    tup2 = (1, 2, 3, 4, 5, 6, 7 );
    
    print "tup1[0]: ", tup1[0]
    print "tup2[1:5]: ", tup2[1:5]


以上实例输出结果：


    tup1[0]:  physics
    tup2[1:5]:  (2, 3, 4, 5)





* * *





## 修改元组


元组中的元素值是不允许修改的，但我们可以对元组进行连接组合，如下实例:


    #coding=utf-8
    #!/usr/bin/python
    
    tup1 = (12, 34.56);
    tup2 = ('abc', 'xyz');
    
    # 以下修改元组元素操作是非法的。
    # tup1[0] = 100;
    
    # 创建一个新的元组
    tup3 = tup1 + tup2;
    print tup3;


以上实例输出结果：


    (12, 34.56, 'abc', 'xyz')





* * *





## 删除元组


元组中的元素值是不允许删除的，但我们可以使用del语句来删除整个元组，如下实例:


    #!/usr/bin/python
    
    tup = ('physics', 'chemistry', 1997, 2000);
    
    print tup;
    del tup;
    print "After deleting tup : "
    print tup;


以上实例元组被删除后，输出变量会有异常信息，输出如下所示：


    ('physics', 'chemistry', 1997, 2000)
    After deleting tup :
    Traceback (most recent call last):
      File "test.py", line 9, in <module>
        print tup;
    NameError: name 'tup' is not defined





* * *





## 元组运算符


与字符串一样，元组之间可以使用 + 号和 * 号进行运算。这就意味着他们可以组合和复制，运算后会生成一个新的元组。
<table class="reference" >
<tbody >
<tr >
Python 表达式
结果
描述
</tr>
<tr >

<td >len((1, 2, 3))
</td>

<td >3
</td>

<td >计算元素个数
</td>
</tr>
<tr >

<td >(1, 2, 3) + (4, 5, 6)
</td>

<td >(1, 2, 3, 4, 5, 6)
</td>

<td >连接
</td>
</tr>
<tr >

<td >['Hi!'] * 4
</td>

<td >['Hi!', 'Hi!', 'Hi!', 'Hi!']
</td>

<td >复制
</td>
</tr>
<tr >

<td >3 in (1, 2, 3)
</td>

<td >True
</td>

<td >元素是否存在
</td>
</tr>
<tr >

<td >for x in (1, 2, 3): print x,
</td>

<td >1 2 3
</td>

<td >迭代
</td>
</tr>
</tbody>
</table>



* * *





## 元组索引，截取


因为元组也是一个序列，所以我们可以访问元组中的指定位置的元素，也可以截取索引中的一段元素，如下所示：

元组：


    L = ('spam', 'Spam', 'SPAM!')



<table class="reference" >
<tbody >
<tr >
Python 表达式
结果
描述
</tr>
<tr >

<td >L[2]
</td>

<td >'SPAM!'
</td>

<td >读取第三个元素
</td>
</tr>
<tr >

<td >L[-2]
</td>

<td >'Spam'
</td>

<td >反向读取；读取倒数第二个元素
</td>
</tr>
<tr >

<td >L[1:]
</td>

<td >('Spam', 'SPAM!')
</td>

<td >截取元素
</td>
</tr>
</tbody>
</table>



* * *

## 无关闭分隔符


任意无符号的对象，以逗号隔开，默认为元组，如下实例：


    #!/usr/bin/python
    
    print 'abc', -4.24e93, 18+6.6j, 'xyz';
    x, y = 1, 2;
    print "Value of x , y : ", x,y;

以上实例允许结果：


    abc -4.24e+93 (18+6.6j) xyz
    Value of x , y : 1 2





* * *



# 元组内置函数

Python元组包含了以下内置函数

| 序号 | 方法               | 描述                   |
| ---- | ------------------ | ---------------------- |
|      | cmp(tuple1,tuple2) | 比较两个元组元素       |
|      | len(tuple)         | 计算元组元素个数。     |
|      | max(tuple)         | 返回元组中元素最大值。 |
|      | min(tuple)         | 返回元组中元素最小值。 |
|      | tuple(seq)         | 将列表转换为元组。     |









# REF

1. [python基础教程 w3cschool](https://www.w3cschool.cn/python/)
2. [Python 3 教程 菜鸟教程](http://www.runoob.com/python3/python3-tutorial.html)















* * *





# COMMENT



