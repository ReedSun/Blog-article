# python中的元类Metaclass

## 理解元类之前需要学习的知识
 如果说让我们创建一个类，最先想到的肯定是用`class`创建，当我们使用`class`创建类的时候，python解释器自动创建这个对象，但是python同样也提供了手动处理的方法来创建类，这就是用python的自建函数`type()`。
  
 我们所熟知的`type()`函数的作用是返回一个参数的类型，但是实际上，它也有一种完全不同的能力，即接受一个类的一些描述作为参数，然后返回一个类。
  
 `type()`函数的语法是这样的:
```
type(类名, 父类的元组（针对继承的情况，可以为空），包含属性的字典（名称和值）)
```

举个例子：
```
class ReedSun(ShuaiGe):
    shuai = True
    def test(x):
        return x+2
# 就等价于
type("ReedSun", (ShuaiGe,), {"shuai":True, "test":lambda x: x+2})
# （属性和方法本质上都是方法）
```

在python中，类也是对象，当我们使用class关键词创建一个类的时候，Python解释器仅仅是扫描一下class定义的语法，然后调用`type()`函数创建出class。

## 元类是什么

元类是什么？元类实际上就是用来创建类的东西。为了帮助我们理解，我们可以这样想，我们创建类就是为了创建类的实例，同样的，我们创建元类就是为了创建类。元类就是类*（实例）*的类，就像下面这样
```
Metaclass() = class
class() = object  # object==>实例
```

理解了什么是元类，我们再来看一看type()函数。

其实type就是一个元类，type就是我们用来创建所有的类的元类。（如果我们要创建自己定义的元类的话，也要从type中继承）

## 元类的工作原理

我们来看一下下面这个例子
```
class ReedSunMetaclass(type):
    pass
    
class Foo(object， metaclass = ReedSunMetaclass): 
    pass
    
class Bar(Foo):
    pass
```

1. 首先，我们创建了一个元类`ReedSunMetaclass`*（注意！按照默认习惯，元类的类名总是以`Metaclass`结尾，以便清楚地表示这是一个元类）*。

1. 然后，我们又用元类`ReedSunMetaclass`创建了一个`Foo`类，*（同时，`Foo`类的属性`__metaclass__`就变成了`ReedSunMetaclass`)*。

1. 最后，我们创建了一个子类`Bar`继承自`Foo`。

我们来试着理解一下在python内部是怎么执行这几个步骤的：

- 对于父类`Foo`，Python会在类的定义中寻找`__metaclass__`属性，如果找到了，Python就会用它来创建类`Foo`，如果没有找到，就会用内建的type来创建这个类。很显然，它找到了。

- 对于子类`Bar`, python会先在子类中寻找`__metaclass__`属性，如果找到了，Python就会用它来创建类`Bar`，如果没有找到，就再从父类中寻找，直到type。显然，它在父类中找到了。

**我们可以看到使用元类的一个好处了，即他可以让子类隐式的继承一些东西。**

## 自定义元类

元类的主要目的就是为了当创建类时能够自动地改变类。创建类我们需要定义`__new__()`函数，`__new__` 是在`__init__`之前被调用的特殊方法，是用来创建对象并返回之的方法。我们举个例子来说明定义自定义元类的方法。

`__new__()`方法接收到的参数依次是：
1. 当前准备创建的类的对象；
2. 类的名字；
3. 类继承的父类集合；
4. 类的方法集合。
```
class ReedSunMetaclass(type):
    def __new__(cls, name, bases, attrs):
        # 添加一个属性
        attrs['哈哈哈'] = True
        return type.__new__(cls, name, bases, attrs)
```

## 我们用一个实际例子来说明元类的用法

`ORM`就是一个典型的使用元类的例子。`ORM`全称“Object Relational Mapping”，即对象-关系映射，就是把关系数据库的一行映射为一个对象，也就是一个类对应一个表，这样，写代码更简单，不用直接操作SQL语句。下面我就用这个`ORM`的例子来说明一下元类的用法。

```
#ORM:object relational mapping 对象-关系映射
#把关系数据库的一行映射为一个对象，也就是一个类对应一个表
#ORM框架所有的类只能动态定义


# 定义Field(定义域：元类遇到Field的方法或属性时即进行修改）
class Field(object):

    def __init__(self, name, column_type):  # column==>列类型
        self.name = name
        self.column_type = column_type

    # 当用print打印输出的时候，python会调用他的str方法
    # 在这里是输出<类的名字，实例的name参数(定义实例时输入)>
    # 在ModelMetaclass中会用到
    def __str__(self):
        return "<%s:%s>" % (self.__class__.__name__, self. name)  # __class__获取对象的类，__name__取得类名



# 进一步定义各种类型的Field
class StringField(Field):

    def __init__(self, name):
        # super(type[, object-or-type])  返回type的父类对象
        # super().__init()的作用是调用父类的init函数
        # varchar(100)和bigint都是sql中的一些数据类型
        super(StringField, self).__init__(name, "varchar(100)")  

class IntegerField(Field):

    def __init__(self, name):
        super(IntegerField, self).__init__(name, "bigint")


# 编写ModelMetaclass
class ModelMetaclass(type):
    
    # __new__方法接受的参数依次是：
    # 1.当前准备创建的类的对象（cls）
    # 2.类的名字（name）
    # 3.类继承的父类集合(bases)
    # 4.类的方法集合(attrs)
    def __new__(cls, name, bases, attrs):
        # 如果说新创建的类的名字是Model，那直接返回不做修改
        if name == "Model":
            return type.__new__(cls, name, bases, attrs)
        print("Found model:%s" % name)
        mappings = dict()
        for k, v in attrs.items():
            if isinstance(v, Field):
                print("Found mappings:%s ==> %s" % (k, v))  # 找到映射， 这里用到上面的__str__
                mappings[k] = v
            # 结合之前，即把之前在方法集合中的零散的映射删除，
            # 把它们从方法集合中挑出，组成一个大方法__mappings__
            # 把__mappings__添加到方法集合attrs中
        for k in mappings.keys():
                attrs.pop(k)
        attrs["__mappings__"] = mappings
        attrs["__table__"] = name # 添加表名，假设表名与类名一致
        return type.__new__(cls, name, bases, attrs)


# 编写Model基类继承自dict中，这样可以使用一些dict的方法
class Model(dict, metaclass=ModelMetaclass):

    def __init__(self,  **kw):
        # 调用父类，即dict的初始化方法
        super(Model, self).__init__(**kw)

    # 让获取key的值不仅仅可以d[k]，也可以d.k
    def __getattr__(self, key):
        try:
            return self[key]
        except KeyError:
            raise AttributeError(r"'Model' object has no attribute '%s'" % key)

    # 允许动态设置key的值，不仅仅可以d[k]，也可以d.k
    def __setattr__(self, key, value):
        self[key] = value

    def save(self):
        fields = []
        params = []
        args = []
        # 在所有映射中迭代
        for k, v in self.__mappings__.items():
            fields.append(v.name)
            params.append("?")
            args.append(getattr(self, k, None))
        sql = "insert into %s (%s) values (%s)" % (self.__table__, ",".join(fields), ",".join(params))
        print("SQL: %s" % sql)
        print("ARGS: %s" % str(args))


# 这样一个简单的ORM就写完了


# 下面实际操作一下，先定义个User类来对应数据库的表User
class User(Model):
    # 定义类的属性到列的映射
    id = IntegerField("id")
    name = StringField("username")
    email = StringField("email")
    password = StringField("password")


# 创建一个实例
u = User(id=12345, name="ReedSun", email="sunhongzhao@foxmail.com", password="nicaicai")
u.save()

```

## 参考资料
1. [使用元类-廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014319106919344c4ef8b1e04c48778bb45796e0335839000)
1. [深刻理解python中的元类](http://blog.jobbole.com/21351/)