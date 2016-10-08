# 学习笔记_datetime
*学习日期：2016年10月3日*
*学习课程：[datetime - 廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431937554888869fb52b812243dda6103214cd61d0c2000)*

- `datetime`是python中处理日期和时间的标准库

##获取当前的时间

- 注意到`datetime`是模块，`datetime`模块还包含一个`datetime`类，通过`from datetime import datetime`导入的才是`datetime`这个类。

- 如果仅导入`import datetime`，则必须引用全名`datetime.datetime`。

- `datetime.now()`返回当前日期和时间，其类型是`datetime`。

##获取指定的日期和时间

- 要指定某个日期和时间，我们直接用参数构造一个`datetime`类,例子如下:

```
 >>> from datetime import datetime
>>> dt = datetime(2015, 4, 19, 12, 20) # 用指定日期时间创建datetime
>>> print(dt)
2015-04-19 12:20:00
```

## datetime转化为timestamp

- `timestamp`的定义：
 - 在计算机中，时间实际上是用数字表示的。我们把1970年1月1日 00:00:00 UTC+00:00时区的时刻称为epoch time，记为`0`（1970年以前的时间timestamp为负数），当前时间就是相对于epoch time的秒数，称为`timestamp`。
 
 - 即`timestamp = 0 = 1970-1-1 00:00:00 UTC+0:00`
 
- `timestamp`与时区的值毫无关系，全球所有计算机中的`timestamp`的值都应该是相同的。

- 把一个`datetime`类型转化为`timestamp`只需要调用`timestamp()`方法。

- 注意Python的`timestamp`是一个浮点数(`float`类型)。如果有小数位，小数位表示毫秒数。

##timestamp转化为datetime

- 要把`timestamp`转换为`datetime`，使用`datetime`提供的`fromtimestamp()`方法

- `timestamp`是一个浮点数，它没有时区的概念，而`datetime`是有时区的。上述转换是在`timestamp`和本地时间做转换。本地时间是指当前操作系统设定的时区。

- `timestamp`也可以直接被转换到UTC标准时区(UTC+0:00)的时间，通过使用`datetime`提供的`utcfromtimestamp()`方法。

## str转化为datetime

- 转换方法是通过`datetime.strptime()`实现，需要一个日期和时间的格式化字符串。字符串`'%Y-%m-%d %H:%M:%S'`规定了日期和时间部分的格式。例子如下：

```
>>> from datetime import datetime
>>> cday = datetime.strptime('2015-6-1 18:19:59', '%Y-%m-%d %H:%M:%S')
>>> print(cday)
2015-06-01 18:19:59
```

- 注意转换后的`datetime`是没有时区信息的。

##datetime转化为str

- 如果已经有了`datetime`对象，要把它格式化为字符串显示给用户，就需要转换为str，转换方法是通过`strftime()`实现的，同样需要一个日期和时间的格式化字符串。

```
>>> from datetime import datetime
>>> now = datetime.now()
>>> print(now.strftime('%a, %b %d %H:%M'))
Mon, May 05 16:28
```

##datetime加减

- 对日期和时间进行加减实际上就是把datetime往后或往前计算，得到新的datetime。加减可以直接用+和-运算符，不过需要导入timedelta这个类，使用timedelta你可以很容易地算出前几天和后几天的时刻。例子如下：

```
>>> from datetime import datetime, timedelta
>>> now = datetime.now()
>>> now
datetime.datetime(2015, 5, 18, 16, 57, 3, 540997)
>>> now + timedelta(hours=10)
datetime.datetime(2015, 5, 19, 2, 57, 3, 540997)
>>> now - timedelta(days=1)
datetime.datetime(2015, 5, 17, 16, 57, 3, 540997)
>>> now + timedelta(days=2, hours=12)
datetime.datetime(2015, 5, 21, 4, 57, 3, 540997)
```

##本地时间转化为UTC时间

- 一个`datetime`类型有一个时区属性`tzinfo`，但是默认为`None`，所以无法区分这个`datetime`到底是哪个时区，除非强行给`datetime`设置一个时区。

- 设置时区分两步。
 - 第一步是用`timedelta()`创建时区
 - 第二部是用`replace(tzinfo=刚才创建时区)`强制设置时区
 
##时区转换

- 我们可以通过`utcnow()`拿到当前的UTC=0时间(例`utc_dt = datetime.utcnow().replace(tzinfo=timezone.utc)`)


- 利用带时区的`datetime`，通过`astimezone()`方法(例`bj_dt = utc_dt.astimezone(timezone(timedelta(hours=8)))`)，可以转换到任意时区。