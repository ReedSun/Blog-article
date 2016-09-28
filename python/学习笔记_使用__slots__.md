# 学习笔记_使用\__slots__
*学习日期：2016年9月27日*
*学习课程：[使用\__slots__ - 廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/00143186739713011a09b63dcbd42cc87f907a778b3ac73000)*

python属于动态语言，所以可以随意给实例和类绑定任何属性和方法，但是如果我们想要限制实例的属性的话，就可以定义一个特殊的\__slots__变量，来限制该class实例能添加的属性。

##绑定属性和方法

- 给实例和类绑定属性详见 [实例属性和类属性 - 廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014319117128404c7dd0cf0e3c4d88acc8fe4d2c163625000)

- 给实例绑定方法。（使用`Type`模块中的`MethodType`函数）
  - `MethodType`不适合用于给类绑定方法，只适用于给实例绑定方法（python的帮助文档对`MethodType`作的解释）。
  
  ```
  class Student(object):
      pass
  >>> def set_age(self, age): # 定义一个函数作为实例方法
  ...     self.age = age
  ...
  >>> from types import MethodType
  >>> s.set_age = MethodType(set_age, s) # 给实例绑定一个方法
  >>> s.set_age(25) # 调用实例方法
  >>> s.age # 测试结果
  25
  ```
  
- 给类绑定方法。

  ```
  >>> def set_score(self, score):
  ...     self.score = score
  ...
  >>> Student.set_score = set_score
  ```
  
  - 给class绑定方法后，所有实例均可调用。
  - 通常情况下，给类绑定方法可以直接定义在class中，但动态绑定允许我们在程序运行的过程中动态给class加上功能。

##使用`__slots__`

- `__slots__`的作用是限制class实例能添加的属性。即实例只能绑定`__slots__`中允许的属性名称，绑定其他名称将得到`AttributeError`的错误。
- 举例说明用法：
  
  ```
  class Student(object):
      __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
  ```
- `__slots__`定义的属性仅对当前类实例起作用，**对继承的子类是不起作用的**。
- 除非在子类中也定义`__slots__`，这样，子类实例允许定义的属性就是自身的`__slots__`加上父类的`__slots__`。