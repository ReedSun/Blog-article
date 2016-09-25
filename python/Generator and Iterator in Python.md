# Generator and Iterator in Python
*Generator——生成器*
*Iterator——迭代器*
## 迭代器
- 迭代是python最强大的功能之一，是访问集合元素的一种方式。
- **迭代器是一个可以记住遍历位置的对象**。
- 迭代器对象从集合的第一个元素开始访问，直到所有元素被访问完结束，迭代器只能往前不能向后。
- 迭代器有两个基本的方法 `iter()`和`next()`。
  - `iter()`作用是创建一个迭代器对象。
  - `next()`作用是输出迭代器的下一个对象。
- 字符串，列表和元组对象都可用于创建迭代器，例子如下：
```
>>> list=[1,2,3,4]
>>> it = iter(list)    # 创建迭代器对象
>>> print (next(it))   # 输出迭代器的下一个元素
1
>>> print (next(it))
2
>>> 
```
- 迭代器对象可以使用常规for语句进行遍历，例子如下：
```
#!/usr/bin/python3

list=[1,2,3,4]
it = iter(list)    # 创建迭代器对象
for x in it:
    print (x, end=" ") 
#print默认是打印一行，结尾加换行。end=' '意思是末尾不换行，加空格。

# 执行以上程序，输出结果如下：
1 2 3 4
```
- 也可以用next()函数，例子如下：
```
#!/usr/bin/python3

import sys         # 引入 sys 模块

list=[1,2,3,4]
it = iter(list)    # 创建迭代器对象

while True:
    try:
        print (next(it))
    except StopIteration:
        sys.exit()
#执行以上程序，输出结果如下：
1
2
3
4
```
## 生成器
- 在Python中，使用了`yield`的函数被称为生成器（generator)。
- 与普通函数不同的是，生成器是一个返回迭代器的函数，只能用于迭代操作。更简单点理解，生成器也是一个迭代器。
- 在调用生成器运行的过程中，每次遇到`yield`会函数会暂停并保存当前所有的运行信息，返回`yield`的值。并在下一次执行`next()`操作时从当前位置继续运行。
- 以下是用生成器返回斐波那契数的实例。
```
#!/usr/bin/python3

import sys

def fibonacci(n): # 生成器函数 - 斐波那契
    a, b, counter = 0, 1, 0
    while True:
        if (counter > n): 
            return
        yield a
        a, b = b, a + b
        counter += 1
f = fibonacci(10) # f 是一个迭代器，由生成器返回生成

while True:
    try:
        print (next(f), end=" ")
    except StopIteration:
        sys.exit()
#执行以上程序，输出结果如下：
0 1 1 2 3 5 8 13 21 34 55
```