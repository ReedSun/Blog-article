# 使用python的email、smtplib、poplib模块收发邮件

- 一封电子邮件的旅程是：
  - MUA：Mail User Agent——邮件用户代理。(即类似Outlook的电子邮件软件）
 - MTA：Mail Transfer Agent——邮件传输代理，就是那些Email服务提供商，比如网易、新浪等等。
 - MDA：Mail Delivery Agent——邮件投递代理。Email服务提供商的某个服务器

>发件人 -> MUA -> MTA -> MTA -> 若干个MTA -> MDA <- MUA <- 收件人



- 要编写程序来发送和接收邮件，本质上就是：

 - 编写MUA把邮件发到MTA；

 - 编写MUA从MDA上收邮件。
 
- 发邮件时，MUA和MTA使用的协议就是SMTP：Simple Mail Transfer Protocol，后面的MTA到另一个MTA也是用SMTP协议。
- 收邮件时，MUA和MDA使用的协议有两种：POP：Post Office Protocol，目前版本是3，俗称POP3；IMAP：Internet Message Access Protocol，目前版本是4，优点是不但能取邮件，还可以直接操作MDA上存储的邮件，比如从收件箱移到垃圾箱，等等。


- 构造一个邮件对象就是一个Messag对象，如果构造一个MIMEText对象，就表示一个文本邮件对象，如果构造一个MIMEImage对象，就表示一个作为附件的图片，要把多个对象组合起来，就用MIMEMultipart对象，而MIMEBase可以表示任何对象。它们的继承关系如下：

>Message
>>+- MIMEBase
>>>   +- MIMEMultipart
      +- MIMENonMultipart
>>>>      +- MIMEMessage
      +- MIMEText
      +- MIMEImage

##发送邮件
```
from email.mime.text import MIMEText
# email模块负责构造邮件
# 类email.mime.text.MIMEText(_text)，是使用字符串_text来生成MIME对象的主体文本
# MIME是(Multipurpose Internet Mail Extensions) 多用途互联网邮件扩展类型
# MIME是设置将某种扩展名文件用一种应用程序来打开的方式类型
# MIME设置的目的是为了在发送电子邮件时附加多媒体数据，让邮件根据其类型进行处理。


import smtplib
# smtplib模块负责发送邮件
# 类smtplib.SMTP([host[, port[, local_hostname[, timeout]]]]) ：SMTP对象
    # 其中，host：smtp服务器主机名
    # 其中，port：smtp服务器的端口，默认是25
# 如果在创建SMTP对象时定义了这两个参数，在初始化时会自动调用connect方式连接服务器
# smtplib模块还提供了SMTP_SSL类和LMTP类，对它们的操作与SMTP基本一致。
# SSL是一种安全传输，LMTP是与SMTP不同的另一种传输协议


from email.header import Header
# 如果你想让你的邮件标题使用非ASCII字符集，就要使用email.header编码非ASCII字符集
# email.header.Header(s=None, charset=None, maxlinelen=None, header_name=None, continuation_ws=' ', errors='strict')
    # 创建一个能容纳不同字符集的字符串的MIME对象的标头
    # 其中，s：初始标头，即要编码之前的标头
    # 其中，chatset：字符集，默认为ASCII
    # 其中，maxlinelen：标头名的行的最大长度，默认为76
    # 其中，header_name：标头名，默认无
    # 其中，continuation_ws：默认为单个空格字符
    # 其中，errors：直接传递到Header的append()方法里


from email.utils import parseaddr, formataddr
# email.utils:杂项工具
# email.utils.parseaddr(address):解析地址的功能，
    # 其中，address是一个包含用户名和email地址的值（realname<address>），返回一个二元组(realname, email address)
# email.utils.formataddr(pair, charset='utf-8')
    # 其中，pair是二元组(realname, email address)
    # 其中，charset是字符串，默认为utf-8
# 实际上，parseaddr(), formataddr(),两者互逆


from email.mime.base import MIMEBase
# email.mime.base.MIMEBase(_maintype, _subtype, **_params)
    # 这是MIME的一个基类。一般不需要在使用时创建实例。
    # 其中_maintype是内容类型，如text或者image。
    # _subtype是内容的minor type 类型，如plain或者gif。
    # **_params是一个字典，直接传递给Message.add_header()。


from email.mime.multipart import MIMEMultipart
# email.mime.multipart.MIMEMultipart(_subtype='mixed', boundary=None, _subparts=None, **_params)
    # MIMEMultipart是MIMEBase的一个子类，多个MIME对象的集合
    # _subtype默认值为mixed。boundary是MIMEMultipart的边界，默认边界是可数的。


from email import encoders
# email.encoders 功能是编码器


def _format_addr(s):
# 这个函数的作用是把一个标头的用户名编码成utf-8格式的，如果不编码原标头中文用户名，用户名将无法被邮件解码
    name, addr = parseaddr(s)
    return formataddr((Header(name, "utf-8").encode(), addr))
    # Header().encode(splitchars=';, \t', maxlinelen=None, linesep='\n')
        # 功能：编码一个邮件标头，使之变成一个RFC兼容的格式


# 先定义Email的地址，口令和SMTP服务器地址
from_addr = "947503956@qq.com"
password = input("请输入发送邮箱的密码:")
to_addr = "sunwstmz@163.com"
smtp_server = "smtp.qq.com"


# 接下来定义邮件本身的内容
msg = MIMEMultipart()
msg["From"] = _format_addr("发送者ReedSun <%s>" % from_addr)
msg["To"] = _format_addr("接收者ReedSun <%s>" % to_addr)
msg["Subject"] = Header("哈哈哈 这是一封测试信", "utf-8").encode()
# 定义邮件正文
msg.attach(MIMEText("这是一封来自大魔王ReedSun，用Python发来的邮件", "plain", "utf-8"))
# class email.mime.text.MIMEText(_text[, _subtype[, _charset]])：MIME文本对象，
    # 其中, _text是邮件内容，
    # 其中, _subtype邮件类型(MIME类型)，可以是text/plain（普通文本邮件），html/plain(html邮件),
    # 其中, _charset编码（charset：字符集），可以是gb2312等等。
# message.attch(payload) 将给定的附件或信息，添加到已有的有效附件或信息中，在调用之前必须是None或者List，调用后。payload将变成信息对象的列表
# 如果你想将payload设置成一个标量对象，要使用set_payload()
with open('c:/work/test.jpg', 'rb') as f:
    mime = MIMEBase("image", "jpg", filename="test.jpg")
    mime.add_header("Content-Disposition", "attchment", filename="test.jpg")
    # add_header(_name, _value, **_params) 扩展标头设置
        # _name：要添加的标头字段
        # _value：标头的内容
    # Content-Disposition就是当用户想把请求所得的内容存为一个文件的时候提供一个默认的文件名。
        # 希望某类或者某已知MIME 类型的文件（比如：*.gif;*.txt;*.htm）能够在访问时弹出“文件下载”对话框。
    mime.add_header("Content-ID", "<0>")
    mime.add_header("X-Attachment-Id", "0")
    mime.set_payload(f.read())
    # set_payload(payload, charset=None)
        # 将附件添加到payload中
    encoders.encode_base64(mime)
    # encoders.encode_base64(mime) 将payload内容编码为base64格式
    msg.attach(mime)


# 接下来定义发送文件
server = smtplib.SMTP_SSL(smtp_server, 465)
# qq邮箱使用SSL连接，端口为465
server.set_debuglevel(1)
# SMTP.set_debuglevel(level)：设置是否为调试模式。默认为False，即非调试模式，
# 非调试模式：表示不输出任何调试信息。
server.login(from_addr, password)
# 登陆到smtp服务器。
# 现在几乎所有的smtp服务器，都必须在验证用户信息合法之后才允许发送邮件。
server.sendmail(from_addr, [to_addr], msg.as_string())
# SMTP.sendmail(from_addr, to_addrs, msg[, mail_options, rcpt_options]) ：发送邮件。
# 这里要注意一下第三个参数，msg是字符串，表示邮件。
# 我们知道邮件一般由标题，发信人，收件人，邮件内容，附件等构成，
# 发送邮件的时候，要注意msg的格式。这个格式就是smtp协议中定义的格式。
# 第二个参数，接受邮箱为一个列表，表示可以设置多个接受邮箱
# as_string()是MIMEMessage对象的一个方法，表示把MIMETest对象变成str
server.quit()
# 断开与smtp服务器的连接，相当于发送"quit"指令。
```

##接受邮件
```
from email.parser import Parser
# email.parser 解析电子邮件
    # 返回这个对象的email.message.Message实例
from email.header import decode_header
from email.utils import parseaddr
import poplib


email = "947503956@qq.com"
password = input("密码:")
pop3_server = "pop.qq.com"

# 连接到POP3服务器
server = poplib.POP3_SSL(pop3_server)
# 注意qq邮箱使用SSL连接
# 打开调试信息
server.set_debuglevel(1)
# 打印POP3服务器的欢迎文字
print(server.getwelcome().decode("utf-8"))

# 身份认证
server.user(email)
server.pass_(password)

# stat()返回邮件数量和占用空间
print("信息数量：%s 占用空间 %s" % server.stat())
# list()返回(response, ['mesg_num octets', ...], octets)，第二项是编号
resp, mails, octets = server.list()
print("list!!!!",server.list())
# 返回的列表类似[b'1 82923', b'2 2184', ...]
print(mails)

# 获取最新一封邮件，注意索引号从1开始
# POP3.retr(which) 检索序号which的真个消息，然后设置他的出现标志 返回(response, ['line', ...], octets)这个三元组
index = len(mails)
resp, lines, ocetes = server.retr(index)

# lines 存储了邮件的原始文本的每一行
# 可以获得整个邮件的原始文本
msg_content = b"\r\n".join(lines).decode("utf-8")
# 稍后解析出邮件
msg = Parser().parsestr(msg_content)
# email.Parser.parsestr(text, headersonly=False)
    # 与parser()方法类似，不同的是他接受一个字符串对象而不是一个类似文件的对象
    # 可选的headersonly表示是否在解析玩标题后停止解析，默认为否
    # 返回根消息对象

# 关闭连接
server.quit()


#### 解析邮件



# 邮件的Subject或者Email中包含的名字都是经过编码后的str，要正常显示，就必须decode
def decode_str(s):
    value, charset = decode_header(s)[0]
    # decode_header()返回一个list，因为像Cc、Bcc这样的字段可能包含多个邮件地址，所以解析出来的会有多个元素。上面的代码我们偷了个懒，只取了第一个元素。
    if charset:
        value = value.decode(charset)
    return value


# 文本邮件的内容也是str，还需要检测编码，否则，非UTF-8编码的邮件都无法正常显示
def guess_charset(msg):
    charset = msg.get_charset()
    if charset is None:
        content_type = msg.get('Content-Type', '').lower()
        pos = content_type.find('charset=')
        if pos >= 0:
            charset = content_type[pos + 8:].strip()
    return charset




# 解析邮件与构造邮件的步骤正好相反
def print_info(msg, indent=0):
    if indent ==0:
        for header in ["From", "To", "Subject"]:
            value = msg.get(header, "")
            if value:
                if header == "Subject":
                    vaule = decode_str(value)
                else:
                    hdr, addr =parseaddr(value)
                    name = decode_str(hdr)
                    value = u"%s <%s>" % (name, addr)
            print("%s%s:%s" % ("  " * indent, header, value))
        if (msg .is_multipart()):
            parts = msg.get_payload()
            for n, part in enumerate(parts):
                print('%spart %s' % ('  ' * indent, n))
                print('%s--------------------' % ('  ' * indent))
                print_info(part, indent + 1)
        else:
            content_type = msg.get_content_type()
            if content_type=='text/plain' or content_type=='text/html':
                content = msg.get_payload(decode=True)
                charset = guess_charset(msg)
                if charset:
                    content = content.decode(charset)
                print('%sText: %s' % ('  ' * indent, content + '...'))
            else:
                print('%sAttachment: %s' % ('  ' * indent, content_type))




```


