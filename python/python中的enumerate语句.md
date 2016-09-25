#python中的enumerate语句
##enumerate()用法说明
- enumerate()用于遍历序列中的元素以及他们的下标。
- enumerate()是python的内置函数。
- enumerate的意思是枚举，列举的意思。
- 对于一个可迭代的或者可遍历的对象，enumerate将其组成一个索引序列，利用它同时获得索引和值
- enumerate多由于for语句中得到计数。
##enumerate()的语法和用法
- 语法
`enumerate（Iterable，start)`
  - 语句中的第一个元素`Iterable`代表我们要进行遍历的序列（列表，字典，元组，……）。
  - 语句中的第二个元素`start`代表索引的起始值，默认是0。
##enumerate()例子
```
>>> L=[1,2,3,4,5]
>>> list(enumerate(L))
[(0, 1), (1, 2), (2, 3), (3, 4), (4, 5)]
>>> list(enumerate(L,4))
[(4, 1), (5, 2), (6, 3), (7, 4), (8, 5)]
```
##参考文献
- [python enumerate用法总结-竹聿Simon的专栏](http://blog.csdn.net/churximi/article/details/51648388)


