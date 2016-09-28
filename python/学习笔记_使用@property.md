# 学习笔记_使用@property
*学习日期：2016年9月27日*
*学习课程：[使用@property - 廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/00143186781871161bc8d6497004764b398401a401d4cce000)*

- `@property`是一个装饰器。
- `@property`就是负责把一个方法变成属性调用的。它既能检查参数，又可以用类似属性这样简单的方式来访问类的变量。
- `@property`的使用：
  - 在定义属性的语句（`def……`）之前加上`@property`此时，`@propert`y本身又创建了另一个装饰器`@xxx.setter`(xxx是属性名)，负责把一个setter方法变成属性赋值。
  - 在定义属性的范围语句之前加上`@xxx.setter`(xxx是属性名)。于是，我们就拥有一个可控的属性操作。
- 例子。

```
class Student(object):

    @property
    def score(self):
        return self._score

    @score.setter
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value

>>> s = Student()
>>> s.score = 60 # OK，实际转化为s.set_score(60)
>>> s.score # OK，实际转化为s.get_score()
60
>>> s.score = 9999
Traceback (most recent call last):
  ...
ValueError: score must between 0 ~ 100!
```

- `@property`还可以定义只读属性，只定义`@property`，不定义`@xxx.setter`就是一个只读属性。