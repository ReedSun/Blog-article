# 学习笔记_XML
*学习日期：2016年10月13日*
*学习课程：[XML - 廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001432002075057b594f70ecb58445da6ef6071aca880af000)*

- 什么是 XML？
 - XML 指可扩展标记语言（EXtensible Markup Language）。
 - XML 是一种很像HTML的标记语言。
 - XML 的设计宗旨是传输数据，而不是显示数据。
 - XML 标签没有被预定义。您需要自行定义标签。
 - XML 被设计为具有自我描述性。
 - XML 是 W3C 的推荐标准。

- XML 和 HTML 之间的差异
 - XML 不是 HTML 的替代。
 - XML 和 HTML 为不同的目的而设计：
 - - XML 被设计用来传输和存储数据，其焦点是数据的内容。
 - - HTML 被设计用来显示数据，其焦点是数据的外观。
 - HTML 旨在显示信息，而 XML 旨在传输信息。
## 解析字符串

- 操作XML有两种方法：DOM和SAX。DOM会把整个XML读入内存，解析为树，因此占用内存大，解析慢，优点是可以任意遍历树的节点。SAX是流模式，边读边解析，占用内存小，解析快，缺点是我们需要自己处理事件。

- 正常情况下，优先考虑SAX，因为DOM实在太占内存。

- 我们用一个例子来说明解析字符串（用SAX）
```
from xml.parsers.expat import ParserCreate

class DefaultSaxHandler(object):
    def start_element(self, name, attrs):
        print('sax:start_element: %s, attrs: %s' % (name, str(attrs)))

    def end_element(self, name):
        print('sax:end_element: %s' % name)

    def char_data(self, text):
        print('sax:char_data: %s' % text)

xml = r'''<?xml version="1.0"?>
<ol>
    <li><a href="/python">Python</a></li>
    <li><a href="/ruby">Ruby</a></li>
</ol>
'''

handler = DefaultSaxHandler()
parser = ParserCreate()
parser.StartElementHandler = handler.start_element
parser.EndElementHandler = handler.end_element
parser.CharacterDataHandler = handler.char_data
parser.Parse(xml)
```
输出如下结果：
```
sax:start_element: ol, attrs: {}
sax:char_data: 

sax:char_data:     
sax:start_element: li, attrs: {}
sax:start_element: a, attrs: {'href': '/python'}
sax:char_data: Python
sax:end_element: a
sax:end_element: li
sax:char_data: 

sax:char_data:     
sax:start_element: li, attrs: {}
sax:start_element: a, attrs: {'href': '/ruby'}
sax:char_data: Ruby
sax:end_element: a
sax:end_element: li
sax:char_data: 

sax:end_element: ol
```

##生成XML
- 最简单也是最有效的生成XML的方法是拼接字符串，例子如下：
```
L = []
L.append(r'<?xml version="1.0"?>')
L.append(r'<root>')
L.append(encode('some & data'))
L.append(r'</root>')
return ''.join(L)
```