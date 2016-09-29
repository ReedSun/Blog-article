# 学习笔记_StringIO和BytesIO
*学习日期：2016年9月29日*
*学习课程：[StringIO和BytesIO - 廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431918785710e86a1a120ce04925bae155012c7fc71e000)*

##StringIO

- `StringIO`顾名思义就是在内存中读写`str`。

- 要把str写入StringIO，我们需要先创建一个StringIO，然后，像文件一样写入即可。

- `getvalue()`方法用于获得写入后的str。

- 要读取StringIO，可以用一个str初始化StringIO，然后，像读文件一样读取。

##BytesIO

- StringIO操作的只能是str，如果要操作二进制数据，就需要使用BytesIO。BytesIO实现了在内存中读写bytes。

- 和StringIO类似，可以用一个bytes初始化BytesIO，然后，像读文件一样读取。