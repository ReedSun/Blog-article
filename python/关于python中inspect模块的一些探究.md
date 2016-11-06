# 关于python中inspect模块的一些探究

标签： Python

---

## 前言
我在学习到[实战Day5 - python教程 - 廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001432339008728d0ddbe19ee594980be3f0644a9371894000)时，遇到了inspect模块，之前对这个inspect模块一无所知啊，所以本着打破砂锅问到底的精神，决定对inspect模块做一些探究。

根据度娘搜到的，inspect模块主要提供了四种用处：

(1). 对是否是模块，框架，函数等进行类型检查。

(2). 获取源码

(3). 获取类或函数的参数的信息

(4). 解析堆栈

我在这次课程中，只用到了第三种用处，即获取类或函数的参数的信息，下面我来探究一下。

## 探究
结合我正在学习的课程，我自己也对inspect做了一些探究。根据在课程中用到的一些函数及方法，我做了一个python脚本。
```
# test
import inspect
def a(a, b=0, *c, d, e=1, **f):
    pass
aa = inspect.signature(a)
print("inspect.signature（fn)是:%s" % aa)
print("inspect.signature（fn)的类型：%s" % (type(aa)))
print("\n")

bb = aa.parameters
print("signature.paramerters属性是:%s" % bb)
print("ignature.paramerters属性的类型是%s" % type(bb))
print("\n")

for cc, dd in bb.items():
    print("mappingproxy.items()返回的两个值分别是：%s和%s" % (cc, dd))
    print("mappingproxy.items()返回的两个值的类型分别是：%s和%s" % (type(cc), type(dd)))
    print("\n")
    ee = dd.kind
    print("Parameter.kind属性是:%s" % ee)
    print("Parameter.kind属性的类型是:%s" % type(ee))
    print("\n")
    gg = dd.default
    print("Parameter.default的值是: %s" % gg)
    print("Parameter.default的属性是: %s" % type(gg))
    print("\n")
    

ff = inspect.Parameter.KEYWORD_ONLY
print("inspect.Parameter.KEYWORD_ONLY的值是:%s" % ff)
print("inspect.Parameter.KEYWORD_ONLY的类型是:%s" % type(ff))
```
执行以上脚本，将得到如下输出：
```
inspect.signature（fn)是:(a, b=0, *c, d, e=1, **f)
inspect.signature（fn)的类型：<class 'inspect.Signature'>


signature.paramerters属性是:OrderedDict([('a', <Parameter "a">), ('b', <Parameter "b=0">), ('c', <Parameter "*c">), ('d', <Parameter "d">), ('e', <Parameter "e=1">), ('f', <Parameter "**f">)])
ignature.paramerters属性的类型是<class 'mappingproxy'>


mappingproxy.items()返回的两个值分别是：a和a
mappingproxy.items()返回的两个值的类型分别是：<class 'str'>和<class 'inspect.Parameter'>


Parameter.kind属性是:POSITIONAL_OR_KEYWORD
Parameter.kind属性的类型是:<enum '_ParameterKind'>


Parameter.default的值是: <class 'inspect._empty'>
Parameter.default的属性是: <class 'type'>


mappingproxy.items()返回的两个值分别是：b和b=0
mappingproxy.items()返回的两个值的类型分别是：<class 'str'>和<class 'inspect.Parameter'>


Parameter.kind属性是:POSITIONAL_OR_KEYWORD
Parameter.kind属性的类型是:<enum '_ParameterKind'>


Parameter.default的值是: 0
Parameter.default的属性是: <class 'int'>


mappingproxy.items()返回的两个值分别是：c和*c
mappingproxy.items()返回的两个值的类型分别是：<class 'str'>和<class 'inspect.Parameter'>


Parameter.kind属性是:VAR_POSITIONAL
Parameter.kind属性的类型是:<enum '_ParameterKind'>


Parameter.default的值是: <class 'inspect._empty'>
Parameter.default的属性是: <class 'type'>


mappingproxy.items()返回的两个值分别是：d和d
mappingproxy.items()返回的两个值的类型分别是：<class 'str'>和<class 'inspect.Parameter'>


Parameter.kind属性是:KEYWORD_ONLY
Parameter.kind属性的类型是:<enum '_ParameterKind'>


Parameter.default的值是: <class 'inspect._empty'>
Parameter.default的属性是: <class 'type'>


mappingproxy.items()返回的两个值分别是：e和e=1
mappingproxy.items()返回的两个值的类型分别是：<class 'str'>和<class 'inspect.Parameter'>


Parameter.kind属性是:KEYWORD_ONLY
Parameter.kind属性的类型是:<enum '_ParameterKind'>


Parameter.default的值是: 1
Parameter.default的属性是: <class 'int'>


mappingproxy.items()返回的两个值分别是：f和**f
mappingproxy.items()返回的两个值的类型分别是：<class 'str'>和<class 'inspect.Parameter'>


Parameter.kind属性是:VAR_KEYWORD
Parameter.kind属性的类型是:<enum '_ParameterKind'>


Parameter.default的值是: <class 'inspect._empty'>
Parameter.default的属性是: <class 'type'>


inspect.Parameter.KEYWORD_ONLY的值是:KEYWORD_ONLY
inspect.Parameter.KEYWORD_ONLY的类型是:<enum '_ParameterKind'>

```

##总结

- `inspect.signature（fn)`将返回一个`inspect.Signature`类型的对象，值为fn这个函数的所有参数

- `inspect.Signature`对象的`paramerters`属性是一个`mappingproxy`（映射）类型的对象，值为一个有序字典（Orderdict)。

 - 这个字典里的key是即为参数名，`str`类型

 - 这个字典里的value是一个`inspect.Parameter`类型的对象，根据我的理解，这个对象里包含的一个参数的各种信息

- `inspect.Parameter`对象的`kind`属性是一个`_ParameterKind`枚举类型的对象，值为这个参数的类型（可变参数，关键词参数，etc）

- `inspect.Parameter`对象的`default`属性：如果这个参数有默认值，即返回这个默认值，如果没有，返回一个`inspect._empty`类。

