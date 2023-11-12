---
title: "from .csrf import CSRFProtect, CsrfProtect SyntaxError"
date: 2022-04-24T13:46:18+08:00
draft: true

lastmod: 2022-04-24T13:46:18+08:00
draft: false
author: "kliiu"
authorLink: "https://kliiu.github.io"

tags: ["flask","python"]
categories: ["消灭bug"]

lightgallery: true
---
《Flask+Web开发》这本书的4.4在视图函数中处理表单程序引入flask_wtf报错<!--more-->

## 报错

  {{< admonition failure "Error" >}}

```bash
Traceback (most recent call last):
  File "C:\Program Files\JetBrains\PyCharm 2021.1.3\plugins\python\helpers\pydev\pydevd.py", line 1483, in _exec
    pydev_imports.execfile(file, globals, locals)  # execute the script
  File "D:/Git/webDev/flask/flasky/hello.py", line 4, in <module>
    from flask_wtf import FlaskForm
  File "C:\Users\k\anaconda3\envs\flask\lib\site-packages\flask_wtf\__init__.py", line 1, in <module>
    from .csrf import CSRFProtect, CsrfProtect
SyntaxError: ('invalid syntax', ('C:\\Users\\k\\anaconda3\\envs\\flask\\lib\\site-packages\\flask_wtf\\csrf.py', 220, 55, "            dest = f'{view.__module__}.{view.__name__}'\n"))
python-BaseException
```

{{< /admonition  >}}

## 分析与解决

代码没有问题，是包的版本问题。GOOGLE找到了[解决方案：](https://stackoverflow.com/questions/67876074/syntaxerror-invalid-syntax-while-using-flask-wtf-package)

> `pip install flask-wtf==0.14.3`
## 项目场景

- Anaconda python2.7
- windows10
- flask=1.1.4
- flask-wtf=0.15.0


## 原代码

```python
from flask import Flask, render_template
from flask_bootstrap import Bootstrap
from flask_moment import Moment
from flask_wtf import FlaskForm
from wtforms import StringField, SubmitField
from wtforms.validators import DataRequired

app = Flask(__name__)
app.config['SECRET_KEY'] = 'hard to guess string'

bootstrap = Bootstrap(app)
moment = Moment(app)


class NameForm(FlaskForm):
    name = StringField('What is your name?', validators=[DataRequired()])
    submit = SubmitField('Submit')


@app.errorhandler(404)
def page_not_found(e):
    return render_template('404.html'), 404


@app.errorhandler(500)
def internal_server_error(e):
    return render_template('500.html'), 500


@app.route('/', methods=['GET', 'POST'])
def index():
    name = None
    form = NameForm()
    if form.validate_on_submit():
        name = form.name.data
        form.name.data = ''
    return render_template('index.html', form=form, name=name)

```
