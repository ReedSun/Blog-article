# 两步实现在Git Bash中用Sublime打开文件
每次都要用鼠标点来点去才能用sublime打开文件！太不科学！今天来配置一下在git bash中用sublime打开文件
## 方法
1.  新建一个文件命名为你想要的命令，比如 `subl`（注意不能有后缀名），内容：
```
#!/bin/sh
"C:\Program Files\Sublime Text 3\sublime_text.exe" $1 &
```
- 第一行是说这是个 shell 脚本
- 第二行的字符串是sublime 的安装目录**注意这里要输入你自己的目录**
- 第二行的$1 是取的命令之后输入的参数
- 第二行的&是此命令在后台打开，这样sublime打开之后，就不会阻塞你的git bash
2. 保存到 `C:\Program Files (x86)\Git\mingW32\bin` 目录下(你的git目录可能与我的不一样，注意改成你自己的)