# python函数中的参数
##参数类型
python函数中的参数类型一共有五种，他们分别是 

- `POSITIONAL_OR_KEYWORD` 位置和关键字参数
- `VAR_POSITIONAL` 可变参数
- `KEYWORD_ONLY` 关键词参数
-  `VAR_KEYWORD` 可变的关键词参数
-  `POSITIONAL_ONLY`

### POSITIONAL_OR_KEYWORD 位置和关键字参数

第一种参数是`POSITIONAL_OR_KEYWORD`位置和关键字参数，其特点是没有任何`*`前缀。

如名字所见，`POSITIONAL_OR_KEYWORD`类型的参数可以通过**位置** `POSITIONAL`传参调用，也可以通过**关键字**`KEYWORD`传参调用。

例子：
```
def hahaha(a): 
    pass

# 位置传参调用
hahaha(1)
# 关键字传参调用
hahaha(a=1)
```

### VAR_POSITIONAL 可变参数

第二种参数是可变参数`VAR_POSTIONAL`,其特点是有一个`*`作为前缀声明，例如`*xxx`，即说明此参数是可变参数。

也如名字所见，`VAR_POSTIONAL`类型参数仅可通过**位置**`POSTIONAL`传参调用，可以接收任意个位置参数甚至是零个，不可以通过**关键字**`KEYWORD`传参调用。

- `VAR_POSITIONAL` 可变参数的一些特点
  - 在函数内部，`VAR_POSITIONAL`可变参数以一个元组显示。
  - `VAR_POSITIONAL`可变参数可以不传入任何参数调用也不会报错。
  - `VAR_POSITIONAL`可变参数只允许存在一个
例子：
```
>>> def hahaha(*a):
...     return a
...
#不传入参数也不会报错，会返回一个空元组
>>> hahaha()
()
#也可以传入任意个位置参数调用
>>> hahaha(3,2,4,5)
(3, 2, 4, 5)
```

### KEYWORK_ONLY 关键字参数

第三种参数是关键字参数`KEYWORK_ONLY`，其特点是，只会在`VAR_POSITIONAL`可变类型参数的后面，且不带`**`前缀。

也如名字所见，`KEYWORK_ONLY`类型参数仅可以通过**关键字**`KEYWORD`传参调用，不可以通过**位置**`POSTIONAL`传参调用。*（因为通过位置传的参数已经全部让前面的`VAR_POSITONAL`参数接收了，所以`KEYWORK_ONLY`只能通过关键字接受参数调用。）*

例子：
```
# VAR_POSITIONAL不需要使用时，可以匿名化
def hahaha(*, a):
    pass

# 只能关键字传参调用
hahaha(a=1)
```
###VAR_KEYWORD 可变的关键词参数

第四种参数是可变的关键词参数，其特点是，其特点是有一个`**`作为前缀声明，例如`**xxx`，即说明此参数是可变的关键词参数。

`VAR_KEYWORD`类型参数仅可通过**关键字**`KEYWORD`传参调用，可以接收任意个位置参数甚至是零个，不通过**位置**`POSTIONAL`传参调用。

- `VAR_KEYWORD`可变的关键词参数的一些特点
  - `VAR_KEYWORD`在函数内部以一个字典`dict`显示。
  - `VAR_KEYWORD`只允许有一个。
  - `VAR_KEYWORD`只允许在函数的最后声明。

例子：
```
>>> def hahaha(**a):
...     return a
...
>>> hahaha(a=3,b=111,v="asasad")
{'v': 'asasad', 'a': 3, 'b': 111}
```

### POSITIONAL_ONLY 位置参数
第五种是位置参数，它一点也不重要，属于python的历史产物，我们已经无法在高版本python中创建一个`POSITIONAL_ONLY`参数了已经。

### 几种参数的综合比较

参数名|英文名|定义时顺序|表达方式|如何调用
-|
位置和关键词参数|`POSITIONAL_OR_KEYWORD`|1|正常表达|关键词&位置
可变参数|`VAR_POSITIONAL`|2|前缀`*`|位置|
关键词参数|`KEYWORD_ONLY`|3|只能在可变参数后面，不带`**`|关键词
可变的关键词参数|`VAR_KEYWORD`|4|前缀`**`|关键词

## 默认参数
- `VAR`系列的两个参数*（可变参数、可变的关键词参数）*不允许设置默认参数，而另外两个参数*（位置和关键词参数、关键词参数）*可以设置默认参数。
  - 因为`VAR_POSITIONAL`的默认参数是`tuple()`空元祖，而`VAR_KEYWORD`的默认参数是`dict()`空字典。
- 默认参数的位置
  - `POSITIONAL_OR_KEYWORD`类型的默认参数一定要放在后面，否则会报错。
  - `KEYWORD_ONLY`虽然没有强制要求，因为都是用关键字传参，谁先谁后都无所谓，但最好还是尽可能地放在后面吧。
- 默认参数绝对不可以设置为可变类型（比如dict、list、set）。
  - 因为如果你把默认参数设置为可变类型，下次再调用它就不是默认值了。


##接受参数的优先级
1. 先接收`POSITIONAL_OR_KEYWORD`
1. 再接收`KEYWORD_ONLY`
1. 再接收`VAR_POSITIONAL`和`VAR_KEYWORD`，这两者没有交集

例子：
```
>>> hahaha(a=3,b=111,v="asasad")
{'v': 'asasad', 'a': 3, 'b': 111}
>>> def hahaha(a,*b,c,**d):
...     print(a)
...     print(b)
...     print(c)
...     print(d)
...
>>> hahaha(1,2,"3",c=4,one=5,two=6)
1                          #a的值
(2, '3')                   #b的值
4                          #c的值
{'one': 5, 'two': 6}       #d的值
```

##参数组合 
在python中，这几种参数都可以组合使用，但请一定要注意**参数的顺序**

例子：
```
def f1(a, b, c=0, *args, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)

def f2(a, b, c=0, *, d, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)

#在函数调用的时候，Python解释器自动按照参数位置和参数名把对应的参数传进去。

>>> f1(1, 2)
a = 1 b = 2 c = 0 args = () kw = {}
>>> f1(1, 2, c=3)
a = 1 b = 2 c = 3 args = () kw = {}
>>> f1(1, 2, 3, 'a', 'b')
a = 1 b = 2 c = 3 args = ('a', 'b') kw = {}
>>> f1(1, 2, 3, 'a', 'b', x=99)
a = 1 b = 2 c = 3 args = ('a', 'b') kw = {'x': 99}
>>> f2(1, 2, d=99, ext=None)
a = 1 b = 2 c = 0 d = 99 kw = {'ext': None}

#通过一个tuple和dict，你也可以调用上述函数。

>>> args = (1, 2, 3, 4)
>>> kw = {'d': 99, 'x': '#'}
>>> f1(*args, **kw)
a = 1 b = 2 c = 3 args = (4,) kw = {'d': 99, 'x': '#'}
>>> args = (1, 2, 3)
>>> kw = {'d': 88, 'x': '#'}
>>> f2(*args, **kw)
a = 1 b = 2 c = 3 d = 88 kw = {'x': '#'}
```
所以，对于任意函数，都可以通过类似`func(*args, **kw)`的形式调用它，无论它的参数是如何定义的。

###小结及注意事项
- 默认参数一定要用不可变对象，如果是可变对象，程序运行时会有逻辑错误！
- `*args`是可变参数，args接收的是一个tuple。
- `**kw`是可变的关键字参数，kw接收的是一个dict。
- 可变参数既可以直接传入：`func(1, 2, 3)`，又可以先组装list或tuple，再通过`*args`传入：`func(*(1, 2, 3))`
- 可变的关键字参数既可以直接传入：`func(a=1, b=2)`，又可以先组装dict，再通过`**kw`传入：`func(**{'a': 1, 'b': 2})`。
- 使用`*args和**kw是Python的习惯写法，当然也可以用其他参数名，但最好使用习惯用法。
- 关键字参数在没有可变参数的情况下不要忘了写分隔符*，否则定义的将是位置和关键词参数。

###参考文献
1.[python的类型（www.qiangtaoli.com）](http://www.qiangtaoli.com/bootstrap/blog/001474192642421efbf41449b2e4a3b99e8202c54d2d8a2000)
1.[函数的参数 - 廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431752945034eb82ac80a3e64b9bb4929b16eeed1eb9000#0)

    







