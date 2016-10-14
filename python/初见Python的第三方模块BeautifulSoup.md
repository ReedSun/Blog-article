# 初见Python的第三方模块BeautifulSoup

- [中文版官方文档](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/)

- 安装BeautifulSoup。
```
$ pip install beautifulsoup4
```

- `Beautiful Soup` 是一个可以从`HTML`或`XML`文件中提取数据的Python库.它能够通过你喜欢的转换器实现惯用的文档导航,查找,修改文档的方式.`Beautiful Soup`会帮你节省数小时甚至数天的工作时间.

- 解析器
 - `Beautiful Soup`支持Python标准库中的HTML解析器,还支持一些第三方的解析器,其中一个是 `lxml` .根据操作系统不同,可以选择下列方法来安装`lxml`:
 ```
 $ apt-get install Python-lxml
 
 $ easy_install lxml
 
 $ pip install lxml
 ```

 - 另一个可供选择的解析器是纯Python实现的 `html5lib` , html5lib的解析方式与浏览器相同,可以选择下列方法来安装`html5lib`:
 ```
 $ apt-get install Python-html5lib
 
 $ easy_install html5lib
 
 $ pip install html5lib
 ```
 - 推荐使用lxml作为解析器,因为效率更高.
 - 下表列出了主要的解析器,以及它们的优缺点:
 |解析器	|使用方法|	优势	|劣势|
 | --- | --- | --- | --- |
 |Python标准库|	BeautifulSoup(markup, "html.parser")	|Python的内置标准库,执行速度适中,文档容错能力强|Python 2.7.3 or 3.2.2)前 的版本中文档容错能力差|   
 |lxml HTML 解析器|	BeautifulSoup(markup, "lxml")	   |速度快,文档容错能力强|需要安装C语言库|
 |lxml XML 解析器|	BeautifulSoup(markup, ["lxml", "xml"])，BeautifulSoup(markup, "xml")|速度快唯一支持XML的解析器|需要安装C语言库|
 |html5lib|	BeautifulSoup(markup, "html5lib")|	最好的容错性，以浏览器的方式解析文档，生成HTML5格式的文档|速度慢，不依赖外部扩展|

##如何使用

- 将一段文档传入`BeautifulSoup` 的构造方法,就能得到一个文档的对象, 可以传入一段字符串或一个文件句柄.
```
from bs4 import BeautifulSoup

soup = BeautifulSoup(open("index.html"))

soup = BeautifulSoup("<html>data</html>")
```
- 首先,文档被转换成Unicode,并且HTML的实例都被转换成Unicode编码
```
BeautifulSoup("Sacr&eacute; bleu!")
<html><head></head><body>Sacré bleu!</body></html>
```
然后,Beautiful Soup选择最合适的解析器来解析这段文档,如果手动指定解析器那么Beautiful Soup会选择指定的解析器来解析文档.

- 建议，传入`BeautifulSoup`时就定义好解析器，就像如下：
```
BeautifulSoup([your markup], "html.parser")
```

##对象的种类
- `Beautiful Soup`将复杂HTML文档转换成一个复杂的树形结构,每个节点都是Python对象,所有对象可以归纳为4种: `Tag`, `NavigableString` , `BeautifulSoup` , `Comment` .

##搜索文档树
- Beautiful Soup定义了很多搜索方法,在这里我先只学习这一个`find()`

###find()
- `find( name , attrs , recursive , text , **kwargs )`

- `find_all()` 方法将返回文档中符合条件的所有tag的列表,尽管有时候我们只想得到一个结果.比如文档中只有一个`<body>`标签,那么使用 `find_all()` 方法来查找`<body>`标签就不太合适, 使用 `find_all` 方法并设置 `limit=1` 参数也可以做到，但是远不如直接使用 find() 方法来的方便.下面两行代码是等价的:
```
soup.find_all('title', limit=1)
# [<title>The Dormouse's story</title>]

soup.find('title')
# <title>The Dormouse's story</title>
```
- 唯一的区别是 find_all() 方法的返回结果是值包含一个元素的列表,而 find() 方法直接返回结果.

- find_all() 方法没有找到目标是返回空列表, find() 方法找不到目标时,返回 None .

##CSS选择器

- `Beautiful Soup`支持大部分的CSS选择器 ,在 `Tag` 或 `BeautifulSoup` 对象的 `.select()` 方法中传入字符串参数,即可使用CSS选择器的语法找到tag:
```
soup.select("title")
# [<title>The Dormouse's story</title>]
```
- 可以通过传入多个tag标签逐层查找:
```
soup.select("body a")
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie"  id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

soup.select("html head title")
# [<title>The Dormouse's story</title>]
```
- 也可以通过使用大于号<的方式找到某个tag标签下的直接子标签：
```
soup.select("head > title")
# [<title>The Dormouse's story</title>]

soup.select("p > a")
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie"  id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

soup.select("p > a:nth-of-type(2)")
# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]

soup.select("p > #link1")
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]

soup.select("body > a")
# []
```
- 通过CSS的类名查找
```
soup.select(".sister")
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

soup.select("[class~=sister]")
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
```
- 通过tag的id查找
```
soup.select("#link1")
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]

soup.select("a#link2")
# [<a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>]
```
- 通过是否存在某个属性来查找:
```
soup.select('a[href]')
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
```
- 通过属性的值来查找:
```
soup.select('a[href="http://example.com/elsie"]')
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]

soup.select('a[href^="http://example.com/"]')
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

soup.select('a[href$="tillie"]')
# [<a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

soup.select('a[href*=".com/el"]')
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>]
```

**BeautifulSoup实际上就是对HTMLParser的人性化封装，它的功能很强大，我现在只学习了一点皮毛，以后还要多翻看官方文档才是，加油~**