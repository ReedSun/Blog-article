# 学习笔记_collections
*学习日期：2016年10月8日*
*学习课程：[collections - 廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431953239820157155d21c494e5786fce303f3018c86000)*

- `collections`是python内建的一个集合模块，提供了许多有用的集合类。

##namedtuple

- `namedtuple`是一个函数，它用来创建一个自定义的`tuple`对象，并且规定`了tuple`元素的个数，并可以用属性而不是索引来引用`tuple`的某个元素。

- 这样一来，我们用`namedtuple`可以很方便地定义一种数据类型，它具备tuple的不变性，又可以根据属性来引用，使用十分方便。

##deque

- `deque`是为了高效实现插入和删除操作的双向列表，适合用于队列和栈。（因为list的插入和删除效率很低。

- `deque`除了实现`list`的`append()`和`pop()`外，还支持`appendleft()`和`popleft()`，这样就可以非常高效地往头部添加或删除元素。

##defaultdict

- 使用`dict`时，如果引用的`Key`不存在，就会抛出`KeyError`。如果希望`key`不存在时，返回一个默认值，就可以用`defaultdict`。

- 注意默认值是调用函数返回的，而函数在创建`defaultdict`对象时传入。

- 除了在Key不存在时返回默认值，`defaultdict`的其他行为跟`dict`是完全一样的。

##OrderedDict

- 使用`dict`时，`Key`是无序的。如果要保持`Key`的顺序，可以用`OrderedDict`。

- 注意，`OrderedDict`的`Key`会按照插入的顺序排列，不是`Key`本身排序。

- `OrderedDict`可以实现一个FIFO（先进先出）的`dict`，当容量超出限制时，先删除最早添加的Key。

```
from collections import OrderedDict

class LastUpdatedOrderedDict(OrderedDict):

    def __init__(self, capacity):
        super(LastUpdatedOrderedDict, self).__init__()
        self._capacity = capacity

    def __setitem__(self, key, value):
        containsKey = 1 if key in self else 0
        if len(self) - containsKey >= self._capacity:
            last = self.popitem(last=False)
            print('remove:', last)
        if containsKey:
            del self[key]
            print('set:', (key, value))
        else:
            print('add:', (key, value))
        OrderedDict.__setitem__(self, key, value)
```

##Counter

- `Counter`是一个简单的计数器，例如，可以统计字符出现的个数。

- `Counter`实际上也是`dict`的一个子类。
