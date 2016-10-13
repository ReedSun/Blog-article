# 学习笔记_常用第三方模块-PIL
*学习日期：2016年10月11日*
*学习课程：[常用第三方模块PIL - 廖雪峰的官方网站](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014320027235877860c87af5544f25a8deeb55141d60c5000)*

- PIL：Python Imaging Library，已经是Python平台事实上的图像处理标准库了。PIL功能非常强大，但API却非常简单易用。

## 安装PIL

- 在命令行下直接通过pip安装：
```
$ pip install pillow
```

##操作图像

- 通过 `Image` 类中的 `open()` 方法即可载入一个图像文件。如果载入文件失败，则会引起一个 `IOError` ；若无返回错误，则 `open()` 函数返回一个 `Image` 对象,Image 对象里有三个属性。
 - `format` : 识别图像的源格式，如果该文件不是从文件中读取的，则被置为 None 值。
 - `size` : 返回的一个元组，有两个元素，其值为象素意义上的宽和高。
 - `mode` : RGB（true color image），此外还有，L（luminance），CMTK（pre-press image）。
 
- `show()`方法显示最新载入的图像。

- `crop()`方法 : 从图像中提取出某个矩形大小的图像。它接收一个四元素的元组作为参数，各元素为`（left, upper, right, lower）`，坐标系统的原点（0, 0）是左上角。

- PIL模块可以实现图像缩放，切片，旋转，滤镜，输入文字，调色板等许多功能,具体功能怎么实现可以百度哈哈哈。

- PIL的`ImageDraw`提供了一系列绘图方法，让我们可以直接绘图。