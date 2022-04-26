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

Linux,  python, [我知道你们的厉害了](https://kliiu.github.io/trouble-误删ubuntu的python3/).



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
index()被称为视图函数



### 2.5.3 请求钩子
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
```html
{% extends "base.html" %} {% import "bootstrap/wtf.html" as wtf %}
{% block title %}Flasky{% endblock %}
{% block page_content %} <div class="page-header"> <h1>Hello, {% if name %}{{ name }}{% else %}Stranger{% endif %}!</h1>#条件控制渲染什么指令
</div>
{{ wtf.quick_form(form) }}#使用wtf.quick_form() 函数渲染 NameForm 对象
{% endblock %}
```
### 4.4在视图函数中处理表单
```python
@app.route('/', methods=['GET', 'POST']) 
def index():
	...//methods参数告诉Flask在URL映射中把这个视图函数注册为GET和POST 请求的处理程序。如果没指定 methods 参数，就只把视图函数注册为GET请求的处理程序。
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
