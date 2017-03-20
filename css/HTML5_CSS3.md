### HTML5是什么？有哪些新特性？有哪些新增标签？如何让低版本的 IE 支持 HTML5新标签

- HTML5是超文本标记语言的第五次重大修改，2014年10月29日标准规范制定完成。
- 新特性：
    - **语义特性**：HTML5增加了许多新标签，可以赋予网页更好的意义和价值。使文档语义化。
    - **本地存储特性**：基于HTML5开发的网页APP拥有更短的启动时间，更快的联网速度，这些全得益于HTML5 APP Cache，以及本地存储功能。
    - **设备兼容特性**：HTML5提供了更多的数据和应用接口，使外部应用可以直接与浏览器内部的数据直接相连。例如视频影音可直接与摄像头相联。
    - **连接特性**： HTML5有更有效的连接工作效率，使得基于页面的实时聊天技术、更快速的网页游戏体验，更优化的在线交流得到实现。
    - **网页多媒体特性**：HTML5支持了网页端的Audio、Video等多媒体功能。同时也提供了基于SVG,Cancas,WebGL及CSS3的3D功能。
    - **性能与集成特性**：HTML5通过XMLHttpRequest2等技术，更好的解决了跨域等问题。
    - **CSS3特性**：CSS3提供了更多的风格和更强的效果。
- 新增标签：

    |标签|描述|
    |-|-|
    |canvas|定义图形标签，比如图表和其他图像。该标签基于JavaScript的绘图API|
    |audio|定义音频内容标签|
    |video|定义视频（video或者movie）|
    |source|定义多媒体资源`<video>`和`<audio>`|
    |embed|定义嵌入的内容，比如插件|
    |track|为诸如`<video>`和`<audio>`的元素之类的媒介规定外部文本轨道
    |datalist|定义选项列表。与input元素配合使用该元素，来定义input可能的值
    |keygen|规定用于表单的秘钥对生成器字段（已被废弃）|
    |output|定义不同类型的输出，比如脚本的输出|
    |article|定义页面正文内容|
    |aside|定义页面内容之外的内容|
    |bdi|设置一段文本，使其脱离其父元素的文本方向设置
    |command|定义命令按钮，比如单选按钮、复选框或按钮
    |details|用于描述文档或文档的某个部分的细节
    |dialog|定义对话框，比如提示框|
    |summary|标签包含details元素的标题|
    |figure|规定独立的流内容（图像、图表、照片、代码等等）
    |figcaption|定义`<figure>`元素标题
    |footer|定义section或document的页脚
    |header|定义了文档的头部区域|
    |mark|定义带有记号的文本|
    |meter|用来显示已知范围的标量值或者分数值|
    |nav|定义导航栏|
    |progress|用来显示一项任务的完成进度，通常情况下，该元素都显示为一个进度条形式|
    |ruby|被用来展示东亚（中文）文字注音或字符注释
    |rt|定义字符（中文注音或字符）的解释或发音
    |rp|在 ruby 注释中使用，定义不支持 ruby 元素的浏览器所显示的内容
    |section|定义文档中的节（section、区段）
    |time|用来表示24小时制时间或者公历日期，若表示日期则也可包含时间和时区
    |wbr|规定在文本中的何处适合添加换行符

- 让低版本的 IE 支持 HTML5新标签可以通过在HTML文件最开头声明DOCTYPE：
```
<!DOCTYPE html>
```


###  input 有哪些新增类型？

|类型|描述|
|--|--|
|color|用于指定颜色的控件
|date|用于输入日期的控件（年、月、日，不包括时间）
|datatime|基于UTC时区的日期时间输入控件（时，分，秒及几分之一秒）
|datatime-loacl|用于输入日期时间控件，不包含时区
|email|用于编辑e-mail的字段|
|month|用于输入年月的控件，不带时区|
|number|用于输入浮点数的控件|
|range|用于输入不精确值得控件|
|search|用于输入搜索字符串的单行文本字段，换行会被从输入的值中自动移除|
|tel|用于输入电话号码的控件|
|time|用于输入不含时区的时间控件|
|url|用于编辑URL的字段|
|week|用于输入一个由星期-年组成的日期，日期不包括时区|

### 浏览器本地存储中 cookie 和 localStorage 有什么区别？ localStorage 如何存储删除数据。
- `cookie`和`loaclStorage`的区别：
    - **储存方式不同**：`loaclStorage`不会自动把数据发送给服务器，仅在本地保存，`cookie`数据始终在同源的http请求中携带，即`cookie`在浏览器和服务器间来回传递。
    - **大小限制不同**：`cookie`的数据不能超过4k，而`loaclStorage`则大的多，可以达到5M或更大。
    - **数据有效期不同***：`cookie`只在设置的cookie过期时间之前一直有效；而`loaclStorage`则是始终生效，除非清除缓存。
- localStorage 如何存储删除数据。
    - 存储数据：
    
        - `localStorage.setItem('myName', 'ReedSun');`

            该方法接受一个键名作为参数，并把该键名从存储中删除。 

    - 删除数据：

        - `loaclStorage.remove('myName')`

            该方法接受一个键名作为参数，并把该键名从存储中删除。

        - `loaclStorge.clear()`
        
            调用该方法会清空存储中的所有键名。

### CSS3效果简单示例

- [示例](http://book.jirengu.com/jirengu-inc/jrg-renwu10/homework/%E5%AD%99%E7%BA%A2%E7%85%A7/senior7/senior7_question4.html)

### 全屏图加过渡色的效果

- [demo](http://book.jirengu.com/jirengu-inc/jrg-renwu10/homework/%E5%AD%99%E7%BA%A2%E7%85%A7/senior7/senior7_question5.html)

### 简单的loading 动画效果

- [demo](http://book.jirengu.com/jirengu-inc/jrg-renwu10/homework/%E5%AD%99%E7%BA%A2%E7%85%A7/senior7/senior7_question6.html)