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

Linux,  python, [我知道你们的厉害了](../troubleshooting/trouble-误删Ubuntu的python3).



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
下面的app.route是一个修饰器，把修饰的函数注册为路由
{{< /admonition >}}
```python
@app.route('/')
def index():#把index()注册为程序根地址的处理程序，当访问网址后会触发服务器执行index()
	return '<h1>Hello World!</h1>'#函数的返回值为响应，即客户端接收到的内容。当客户端是web服务器，响应就是显示给用户查看的内容

```
- 视图函数
index()被称为视图函数


{{< admonition question "Question"  true >}}

{{< /admonition >}}


### 2.5.3 请求钩子
## 7 大型项目的结构
### 项目结构
- Flask 程序一般都保存在名为 app 的包中；
- 和之前一样，migrations 文件夹包含数据库迁移脚本； 
- 单元测试编写在 tests 包中；
- 和之前一样，venv 文件夹包含 Python 虚拟环境。
### 需求文件
生成
`pip freeze >requirements.txt`
### 7.6单元测试
### 7.7创建数据库
