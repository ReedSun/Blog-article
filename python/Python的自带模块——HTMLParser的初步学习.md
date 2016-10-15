# Python的自带模块——HTMLParser的初步学习

- HTMLParser是Python自带的模块，使用简单，能够很容易的实现HTML文件的分析。
- 本文主要简单讲一下HTMLParser的用法. 

- 使用时需要定义一个**从模块`html.parser`中的类`HTMLParser`继承的**类，重定义函数：

 - `handle_starttag( tag, attrs)`
 - `handle_startendtag( tag, attrs)`
 - `handle_endtag( tag)`
 - `handle_data(data)`

##1. 获取标签属性

- `tag`是的html标签，`attrs`是 (属性，值)元组(`tuple`)的列表(`list`). 

如一个标签为：
>`<input type="hidden" name="NXX" id="IDXX" value="VXX" />`

那么它的`attrs`列表为
>`[('type', 'hidden'), ('name', 'NXX'), ('id', 'IDXX'), ('value', 'VXX')]

- `HTMLParser`自动将`tag`和`attrs`都转为小写。

##2. 获取标签内容

- 解析时碰到`<***>`，自动调用`handle_starttag()`；碰到`</***>`，自动调用`handle_endtag()`
- 每一个标签，无论`<>` 还是`</>`，均会调用`handle_data()`

- html中第一行、第二行分别为`<html>`和`<head>`，后面无具体数据，只有回车换行，所用调用`handle_data()`，打印结果为换行；`</html></head>`同理。
 
##3. 一个简单的例子

- 获取豆瓣上正在上映影片的基本信息
```
# encoding=utf8
import requests
from html.parser import HTMLParser
from html.entities import name2codepoint


class Myparser(HTMLParser):
    def __init__(self):
        HTMLParser.__init__(self)
        self.movies = []

    def handle_starttag(self, tag, attrs):
        def _attr(attrlist, attrname):
            for each in attrlist:
                if attrname == each[0]:
                    return each[1]
            return None

        if tag == 'li' and _attr(attrs, 'data-title'):
            movie = {}
            movie['actors'] = _attr(attrs, 'data-actors')
            movie['director'] = _attr(attrs, 'data-director')
            movie['duration'] = _attr(attrs, 'data-dutation')
            movie['title'] = _attr(attrs, 'data-title')
            movie['rate'] = _attr(attrs, 'data-rate')
            self.movies.append(movie)


def movieparser(url):
    headers = {}
    req = requests.get(url)
    s = req.text
    myparser = Myparser()
    myparser.feed(s)
    myparser.close()
    return myparser.movies


if __name__ == '__main__':
    url = 'https://movie.douban.com/'
    movies = movieparser(url)
    for each in movies:
        print('%(title)s|%(rate)s|%(actors)s|%(director)s|%(duration)s' % each)
```

运行结果:
```
湄公河行动|8.2|张涵予 / 彭于晏 / 孙淳|林超贤|None
圆梦巨人 The BFG|6.7|鲁比·巴恩希尔 / 马克·里朗斯 / 比尔·哈德尔|史蒂文·斯皮尔伯格|None
从你的全世界路过|5.5|邓超 / 白百何 / 杨洋|张一白|None
暗杀游戏 Мафия: Игра на выживание|4.3|瓦蒂姆·提萨拉提 / 维奥莱塔·吉特孟斯塔雅 / 韦尼亚明·斯梅霍夫|萨里·奥德赛耶|None
鲁滨逊漂流记 Robinson Crusoe|5.5|马提亚斯·施维赫夫 / 卡亚·叶娜尔 / 伊尔卡·贝桑|文森特·凯斯特鲁特|None
勇士|4.7|李东学 / 于小伟 / 聂远|宁海强|None
黑处有什么|6.8|苏晓彤 / 郭笑 / 陆琦蔚|王一淳|None
王牌逗王牌|3.6|刘德华 / 黄晓明 / 王祖蓝|王晶|None
爵迹|3.7|范冰冰 / 吴亦凡 / 陈学冬|郭敬明|None
铠甲勇士捕王|4.1|赖艺 / 王畅唱 / 曲澔濬|郑国伟|None
逗鸟外传：萌宝满天飞 Storks|7.7|安迪·萨姆伯格 / 凯蒂·克朗 / 凯尔希·格兰莫|尼古拉斯·斯托勒|None
宾虚 Ben-Hur|5.8|杰克·休斯顿 / 纳赞宁·波妮阿蒂 / 摩根·弗里曼|提莫·贝克曼贝托夫|None
幽灵医院|2.4|白歆惠 / 李昌熙 / 罗家英|殷国君|None
T台魔王||朱研 / 班嘉佳 / 侯静文|牛辉|None
我是哪吒|4.9|陶典 / 韩娇娇 / 刘垚|舒展|None
神兽金刚之青龙再现|5.5|黄健 / 李仲鑫 / 谢安晟|朱晓兵|None
疯狂丑小鸭|2.8|苏倩芸 / 王雪沁 / 杨进|蒋叶峰|None
樱桃小丸子：来自意大利的少年 ちびまる子ちゃん イタリアから来た少年|7.1|鳕子 / 屋良有作 / 一龙斋贞友|高木淳|None
新东方神娃||王燕华 / 王晓彤 / 李晔|殷晓东|None
七月与安生|7.6|周冬雨 / 马思纯 / 李程彬|曾国祥|None
爱上处女座|4.1|萧蔷 / 贺刚 / 高天|刘观伟|None

Process finished with exit code 0
```

