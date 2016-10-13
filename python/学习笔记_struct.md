# 学习笔记_struct
*学习日期：2016年10月8日*
*学习课程：[struct - 廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431955007656a66f831e208e4c189b8a9e9f3f25ba53000)*

- Python提供了一个`struct`模块来解决bytes和其他二进制数据类型的转换。

- `struct`的`pack`函数把任意数据类型变成`bytes`。
 - `pack(fmt, v1, v2, ...)   `  按照给定的格式(fmt)，把数据封装成字符串(实际上是类似于c结构体的字节流)

- `unpack`把bytes变成相应的数据类型。
 - `unpack(fmt, string)     `  按照给定的格式(fmt)解析字节流string，返回解析出来的tuple。
 
- `struct`中支持的格式如下表。

|Format	|C Type|	Python	|字节数|
|--|--|--|--|
|x	|pad byte|	no value|	1
|c|	char|	string of length 1	|1
|b|	signed char	|integer|	1
|B|	unsigned char|	integer|	1
|?|	_Bool|	bool|	1
|h|	short|	integer|	2
|H|	unsigned short|	integer	|2
|i|	int|	integer|	4
|I|	unsigned int|	integer or long|	4
|l|	long|	integer|	4
|L|	unsigned long|	long|	4
|q|	long long|	long|	8
|Q|	unsigned long long|	long|	8
|f|	float|	float|	4
|d|	double|	float|	8
|s|	char[]|	string|	1
|p|	char[]|	string|	1