# 学习笔记_itertools
*学习日期：2016年10月11日*
*学习课程：[itertools - 廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/00143200162233153835cfdd1a541a18ddc15059e3ddeec000)*

- Python的内建模块`itertools`提供了非常有用的用于操作迭代对象的函数。

## 无限迭代器

- `count(x, y)`      从x开始的整数循环器，每次增加y，如果不指定y则默认y为1

- `cycle('abc')`    重复序列的元素，既a, b, c, a, b, c ...

- `repeat(x,y)`     重复x这个参数y次，如果不指定y即无限重复。

- 无限序列只有在`for`迭代时才会无限地迭代下去，如果只是创建了一个迭代对象，它不会事先把无限个元素生成出来，事实上也不可能在内存中创建无限多个元素。（迭代器）

- 无限序列虽然可以无限迭代下去，但是通常我们会通过`takewhile()`等函数根据条件判断来截取出一个有限的序列。

- 例子：

```
>>> natuals = itertools.count(1)
>>> ns = itertools.takewhile(lambda x: x <= 10, natuals)
>>> list(ns)
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```

## chain()

- chain()可以把一组迭代对象串联起来，形成一个更大的迭代器。

##groupby()

- groupby()把迭代器中相邻的重复元素挑出来放在一起。

- 实际上挑选规则是通过函数完成的（即第二个参数可以是函数），只要作用于函数的两个元素返回的值相等，这两个元素就被认为是在一组的，而函数返回值作为组的key。

- 例子：
```
>>> for key, group in itertools.groupby('AaaBBbcCAAa', lambda c: c.upper()):
...     print(key, list(group))
...
A ['A', 'a', 'a']
B ['B', 'B', 'b']
C ['c', 'C']
A ['A', 'A', 'a']
```