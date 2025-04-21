# Flask Learning

Flask learning notes.
<!--more-->

{{< admonition question "Question"  false >}}

3.8环境下，无法引入flask_script

在python2.7环境下学习。pip安装不了

换成python3.3 同安装不了pip

而miniconda只支持高版本python.最终决定卸载miniconda, 安装anaconda. 

20220422
换成Windows了

Linux,  python, [我知道你们的厉害了](https://zli2001.github.io/trouble-误删ubuntu的python3/).



{{< /admonition >}}

## 程序的基本结构
### 初始化
```python
from flask import Flask 
app = Flask(__name__)
```
### 路由和视图函数
- 定义路由
{{< admonition note "Notes" >}}
修饰器是 Python 语言的标准特性，可以使用不同的方式修改函数的行为。惯常用法是使用修饰器把函数注册为事件的处理程序。
下面的app.route是一个修饰器，index() 函数注册为程序根地址的处理程序

如果部署程序的服务器域名为 www. example.com，在浏览器中访问 http://www.example.com 后，会触发服务器执行 index() 函 数。这个函数的返回值称为响应，是客户端接收到的内容。如果客户端是 Web 浏览器，响
应就是显示给用户查看的文档。
{{< /admonition >}}

```python
@app.route('/')
def index():#把index()注册为程序根地址的处理程序，当访问网址后会触发服务器执行index()
	return '<h1>Hello World!</h1>'#函数的返回值为响应，即客户端接收到的内容。当客户端是web服务器，响应就是显示给用户查看的内容

```
- 视图函数
像index()这样的函数被称为视图函数。它返回的响应可以是包含HTML的简单字符串，也可以是复杂表单



### 2.5.3 请求钩子

## 3 模板

视图函数：返回值为响应，可以是包含 HTML的简单字符串，也可以是复杂的表单。

**模板**是一个包含响应文本的文件（templates中的文件），包括用占位变量表示的动态部分。如：`{{user.id}}`，其中的值只能在请求的上下文知道。

**渲染**：使用真实值替换变量，再返回最终得到的响应字符串。

>  render（粉刷）指渲染函数

### 3.1 Jinja2模板引擎

render_template将jinja2模板引擎集成到程序中。

`render_template('user.html', name=name)`第一个参数是模板的文件名。随后的参数都是键值对(关键字参数)，表示模板中变量对应的真实值。

**过滤器**：过滤器名添加在变量名后，使用竖线分割。可以修改变量。

>`hello，{{name|capitalize}}`：以首字母大写显示变量的值
- 除了常用过滤器，也可以自定义过滤器。

```python
#定义获取用户名的过滤器
@bp.app_template_filter('user_name')
def get_username(user_id):
    user = FrontUserModel.query.get(user_id)
    return user
```

#### 重复使用代码的两种方式：宏和模板继承
**由于"{百分号"的模板格式会导致博客页面编译报错，因此代码中用“百分号”替代%**
1. **宏**（macro）：类似python中的函数

定义：

```html
{百分号 macro render_comment(filename) 百分号}
    {{ url_for("static",filename=filename) }}
{百分号 endmacro 百分号}
```
使用：
```html
<!--先引入宏-->
{百分号 from 'test_1.html' import render_comment 百分号}

<!--或者-->
{百分号 import 'macros.html' as macros 百分号}
<!--再使用宏-->
<ul>
    {百分号 for comment in comments 百分号}
    	{{render_comment(comment)}}
    {百分号 endfor 百分号}
</ul>
```


2. 模板继承：类似python中的类继承

   - 创建名为base.html的基模板

     ```html
     <html> 
         <head> {百分号 block head 百分号} 
             <title>{百分号 block title 百分号}
                 {百分号 endblock 百分号} - My Application
             </title> 
             {百分号 endblock 百分号}
     	</head>
         <body>
             {百分号 block body 百分号} 
             {百分号 endblock 百分号}
     	</body>
     </html>
     ```

     block 标签定义的元素可在衍生模板中修改。在本例中，我们定义了名为 head、title 和 body 的块。注意，title 包含在 head 中。下面这个示例是基模板的衍生模板：

     ```html
     {百分号 extends "base.html" 百分号} <!--声明衍生自base.html-->
     {百分号 block title 百分号}Index{百分号 endblock 百分号} <!--重新定义基模板中的块-->
     {百分号 block head 百分号} 
         {{ super() }} <!-- 使用super()获取基模板中内容不为空的内容-->
         <style> 
         </style>
     {百分号 endblock 百分号} 
     {百分号 block body 百分号} 
     <h1>Hello, World!</h1>
     {百分号 endblock 百分号}
     ```

     

   

### 3.6使用Flask-Moment本地化时间日期
## Web表单
```python
class NameForm(Form):
    name=StringField("What is your name?",validators=[Required()])#validators指定一个由验证函数组成的列表。Required确保提交的字段不为空
    submit=SubmitField('Submit')
```
### 4.2表单类
app.config用于存储框架
### 4.3把表单渲染成HTML
视图函数把一个NameForm实例通过参数form传入模板，在模板中可以生成一个简单的表单。但是用BootStrao渲染表单对象更为便捷。

### 4.4在视图函数中处理表单
```python
@app.route('/', methods=['GET', 'POST']) 
def index():
	...#methods参数告诉Flask在URL映射中把这个视图函数注册为GET和POST 请求的处理程序。如果没指定 methods 参数，就只把视图函数注册为GET请求的处理程序。
```
-  GET请求没有主体，提交的数据以查询字符串的形式附加的URL中，可在浏览器的地址中看到
-  将提交表单作为POST请求更为便利

{{< admonition question "Question"  true >}}
另一个问题：请求结束时，获取的用户输入的名字数据也丢失了。需要保存输入才能使重定向后的请求获得并使用这个名字。
{{< /admonition >}}
#### 用户会话
程序可以把数据存储在用户会话中。它是一种私有存储，存在于每个连接到服务器的客户端中。
它是请求中的变量，名为session，像python字典一样操作。




### 4.5重定向和用户会话
刷新页面时浏览器会重新发送之前最后一个请求，如果这个请求是包含表单数据的post请求,就会再次提交表单。**最好不要让Web程序把post请求作为浏览器发送的最后一个请求**

实现方式：**Post/重定向/Get模式**使用重定向作为post请求的响应，而不是常规响应。
> 重定向是一种特殊的响应，响应内容是url，当浏览器收到这种响应，会向重定向的url发起GET请求，显示页面的内容。
### 4.6 Flash消息
flash()函数：确认消息、警告消息、错误提醒。让用户知道状态发生了变化

```python
@app.route('/', methods=['GET', 'POST'])
def index():
    form = NameForm()
    if form.validate_on_submit():
        old_name = session.get('name')
        if old_name is not None and old_name != form.name.data:  # 提交的名字与存储的名字比较，不一样的就会调用flash()发给客户端的下一个响应中显示一个消息。
            flash('Looks like you have changed your name!')
        session['name'] = form.name.data
        return redirect(url_for('index'))
    return render_template('index.html', form=form, name=session.get('name'))
```

## 5 数据库

{{< admonition question "Note"  true >}}
__repr__() 方法是类的实例化对象用来做“自我介绍”的方法。

返回一个具有可读性的字符串表示模型，可在调试和测试时使用。
{{< /admonition >}}
```python
class Role(db.Model):
    __tablename__ = 'roles'# 定义表名，如果没有则会使用一个默认表面
    id = db.Column(db.Integer, primary_key=True)# Flask-SQLAlchemy 要求每个模型都要定义主键，这一列经常命名为 id。
    name = db.Column(db.String(64), unique=True)
    def __repr__(self):
        return '<Role %r>' % self.name
class User(db.Model):
    __tablename__ = 'users'
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(64), unique=True, index=True)
    def __repr__(self):
        return '<User %r>' % self.username
```
### 5.7 关系
```python
class Role(db.Model): 
	# ...
	users = db.relationship('User', backref='role')# backref 参数向 User 模型中添加一个role 属性，从而定义反向关系
class User(db.Model): 
	# ...
	role_id = db.Column(db.Integer, db.ForeignKey('roles.id'))
```
### 5.9在视图函数中操作数据库
### 5.10集成Python shell
### 5.11数据库迁移
更新表的更好方法是使用数据库迁移框架。数据库迁移框架能跟踪数据库模式的变化，然后增量式的把变化应用到数据库中。

## 6电子邮件

### 使用gmail

{{< admonition  note "Note">}}

千万不要把账户密令直接写入脚本，特别是当你计划开源自己的作品时。为 了保护账户信息，你需要让脚本从环境中导入敏感信息

{{< /admonition>}}

`set MAIL_USERNAME=klliiuuu`设置用户名及密码

发送邮件失败，需要设置google应用专用密码。

打开pop设置后需要按保存！

## 7 大型项目的结构
### 项目结构
- Flask 程序一般都保存在名为 app 的包中；
- 和之前一样，migrations 文件夹包含数据库迁移脚本； 
- 单元测试编写在 tests 包中；
- 和之前一样，venv 文件夹包含 Python 虚拟环境。
### 7.3程序包
#### 程序工厂函数
`create_app(config_name)`
#### 在蓝本中实现程序功能
（解决路由问题）蓝本和程序类似，也可以定义路由。

在蓝本中定义的路由处于休眠状态，直到蓝本注册到程序上后，路由才真正成为程序 的一部分。
```python
from flask import Blueprint 
main = Blueprint('main', __name__)
from . import views, errors
```
在蓝本中编写视图函数主要有两点不同：
1. 和前面的错误处理程序一样，路由修饰器由蓝本提供；
2. url_for() 函数的用法不同。
> Flask 会为蓝本中的全部端点加上一个命名空间。
> 
> 所以视图函数 index() 注册的端点名是 main.index， 其 URL 使用 url_for('main.index') 获取。
> 或者：url_for('. index')。在这种写法中，命名空间是当前请求所在的蓝本。
### 需求文件
生成
`pip freeze >requirements.txt`

### 7.6单元测试
### 7.7创建数据库


### 8.5 注册用户
写完这一章后有好几天没有打开项目，今天再打开突然全部报错Internal error!
到后来也没有解决。--2022.5.1

## 8 用户认证

### 8.6确认账户
--2022.5.2再打开项目，又可以运行了。
验证电子邮件地址。
itsdangerous 提供了多种生成令牌的方法。

```python
from itsdangerous import TimedJSONWebSignatureSerializer as Serializer #TimedJSONWebSignatureSerializer 类生成具有过期时间的 JSON Web 签名（JSON Web Signatures，JWS）。
s = Serializer(app.config['SECRET_KEY'], expires_in = 3600)#这个类的构造函数接收
的参数是一个密钥，在 Flask 程序中可使用 SECRET_KEY 设置。
#expires_in 参数设置令牌的过期时间，单位为秒。
token = s.dumps({ 'confirm': 23 })#dumps() 方法为指定的数据生成一个加密签名，然后再对数据和签名进行序列化，生成令 牌字符串。
```



由于模型中新加入了一个列用来保存账户的确认状态，因此要生成并执行一 个新数据库迁移。

#### 发送确认邮件

[os.environ.get('MAIL_USERNAME')为None的解决办法](https://www.javazxz.com/thread-11481-1-1.html)

(saved my ass)

当前的 /register 路由把新用户添加到数据库中后，会重定向到 /index。在重定向之前，这 个路由需要发送确认邮件。

更改app/auth/views.py

## 10 用户资料

### 10.1资料信息

### 10.2 资料页面

### 10.3 资料编辑器

### 10.4 用户头像

1. 到gravatar注册，上传图片
2. 计算电子邮件地址的MD5散列值：

## 11 博客文章

### 11.1

1. 创建数据库文章模型

文章包括正文、时间戳以及和User之间的多对一关系。

2. 写文章的表单

3. 显示文章的首页模板

   NameError: name 'Post' is not defined

   报这个错误原因在于manager.py中没有引入Post,解决方法:

   ```python
   from app.models import User, Role, Post
   
   def make_shell_context():
   
       return dict(app=app, db=db, User=User, Role=Role, Post=Post)
   ```

   

### 11.2 在资料页显示博客文章

### 11.3 创建虚拟博客文章数据

1. 安装forgeryPy 生成虚拟用户和博客文章

### 11.4 显示MarkDown

1. 安装flask-pagedown markdown bleach

   - PageDown：使用 JavaScript 实现的客户端 Markdown 到HTML的转换程序。 

   - Flask-PageDown：为 Flask 包装的 PageDown，把 PageDown 集成到 Flask-WTF 表单中。 

   - Markdown：使用 Python 实现的服务器端 Markdown 到HTML的转换程序。
   - Bleach：使用 Python 实现的HTML清理器。

2. 将首页的多行文本控件转换为Markdown富文本编辑器。

### 11.6 博客文章编辑器

1. 下载安装了sqlite. (C:/sqlite)https://blog.csdn.net/qq_38693757/article/details/122366390

## 17 部署

### 17.1 部署流程

- 自动化部署


