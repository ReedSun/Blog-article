# 学习urblib和requests


- urblib是python的一个自建模块，它提供了一系列用于操作URL的功能。

- 而第三方模块requests是对urllib的人性化封装。
 
##requests

- [中文官方文档](http://cn.python-requests.org/zh_CN/latest/)
- [快速上手](http://cn.python-requests.org/zh_CN/latest/user/quickstart.html)


##urllib

- urllib是基于http的高层库，它有以下三个主要功能：
 - request处理客户端的请求
 - response处理服务端的响应
 - parse会解析url

- 下面是使用Python3中urllib来获取资源的一些示例
 - 最简单的抓取url内容。
 ```
 import urllib.request  
response = urllib.request.urlopen('http://python.org/')  
html = response.read()  
  ```
 - 使用response处理服务器的相应。
 ```
 import urllib.request  
req = urllib.request.Request('http://python.org/')  
response = urllib.request.urlopen(req)  
the_page = response.read()  
 ```
 - 发送数据
 ```
 import urllib.parse  
import urllib.request  
url = '"  
values = {  
'act' : 'login',  
'login[email]' : '',  
'login[password]' : ''  
}  
data = urllib.parse.urlencode(values)  
req = urllib.request.Request(url, data)  
req.add_header('Referer', 'http://www.python.org/')  
response = urllib.request.urlopen(req)  
the_page = response.read()  
print(the_page.decode("utf8"))  
 ```
 - 发送数据和header
 ```
 import urllib.parse  
import urllib.request  
url = ''  
user_agent = 'Mozilla/4.0 (compatible; MSIE 5.5; Windows NT)'  
values = {  
'act' : 'login',  
'login[email]' : '',  
'login[password]' : ''  
}  
headers = { 'User-Agent' : user_agent }  
data = urllib.parse.urlencode(values)  
req = urllib.request.Request(url, data, headers)  
response = urllib.request.urlopen(req)  
the_page = response.read()  
print(the_page.decode("utf8"))  
 ```
 - 处理http 错误  
 ```
 import urllib.request  
 req = urllib.request.Request(' ')  
 try:  
        urllib.request.urlopen(req)  
 except urllib.error.HTTPError as e:  
        print(e.code)  
        print(e.read().decode("utf8"))  
 ```
 - 异常处理1
 ```
 from urllib.request import Request, urlopen  
 from urllib.error import URLError, HTTPError  
 req = Request("http://www..net /")  
 try:  
        response = urlopen(req)  
 except HTTPError as e:  
        print('The server couldn't fulfill the request.')  
        print('Error code: ', e.code)  
 except URLError as e:  
        print('We failed to reach a server.')  
        print('Reason: ', e.reason)  
else:  
        print("good!")  
        print(response.read().decode("utf8"))  
 ```
 - 异常处理2  
 ```
 from urllib.request import Request, urlopen  
  from urllib.error import  URLError  
  req = Request("http://www.Python.org/")  
 try:  
        response = urlopen(req)  
 except URLError as e:  
        if hasattr(e, 'reason'):  
            print('We failed to reach a server.')  
            print('Reason: ', e.reason)  
 elif hasattr(e, 'code'):  
        print('The server couldn't fulfill the request.')  
        print('Error code: ', e.code)  
 else:  
        print("good!")  
        print(response.read().decode("utf8"))  
 ```
