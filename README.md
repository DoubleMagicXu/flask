# Flask Web开发笔记
## 虚拟环境安装Flask
使用虚拟环境的好处：
    程序只能使用虚拟环境中的包，便于管理。
创建虚拟环境:
~~~~shell
apt install python3-venv
python3.6 -m venv venv
~~~~
激活虚拟环境:
~~~~shell
source venv/bin/active
~~~~
安装Ｆlask:
~~~~shell
pip install flask
~~~~
退出虚拟环境:
~~~~shell
deactive
~~~~
## 程序的基本结构
### 初始化
*即创建一个程序实例来处理客户端请求，该实例为Flask类的对象。*
~~~~python
from flask import Flask
app=Flask(__name__) #name参数决定了程序的根目录，以便稍后找到资源文件的相对位置
~~~~
### 路由和视图函数
~~~~python
＠app.route("/user/<name>")#路由
def user(name):#视图函数
    return("<h1>Hello,%s!</h1>" % name)
~~~~
### 启动服务器
~~~python
if __name__=="__main__":#确保直接执行脚本才启动web服务器
    app.run(debug=True)
~~~
### Flask 扩展
~~~~shell
pip install flask-script
~~~~
~~~~python
from flask_script import Manager
manager=Manager(app)
#...
if__name__=="__main__":
    manager.run()
~~~~
## 模板
视图函数的功能：
    业务逻辑
    表现逻辑
把表现逻辑移到模板中可提升程序可维护性。
模板渲染：用真实值替换变量，返回最终响应字符串。
### Jinja2模板引擎
~~~~html
<!--templates/user.html-->
<h1>Hello,{{name}}</h1> 
~~~~
~~~~python
#encoding=utf-8
from flask import Flask
from flask_script import Manager
from flask import render_template
app=Flask(__name__)
manager=Manager(app)
@app.route("/user/<name>")
def user(name):
    return(render_template("user.html",name=name))#左边的name代表模板中占位符,右边的name代表当前作用域的变量。
if __name__=="__main__":
    manager.run()
~~~~
Jinjia2模板继承
~~~~html
<!--templates/base.html-->
<html>
<head>
	{%block head%}
	<title>
		{% block title%}
		{%endblock%}
		- My Application
	</title>
	{%endblock%}
</head>
<body>
	{%block body%}
	{%endblock%}
</body>
</html>
~~~~
~~~~html
<!--templates/user.html>
{% extends "base.html" %}
{% block title %} Index{% endblock %}
{%block head%}
{{super()}}
<style></style>
{%endblock%}
{%block body%}
<h1> Hello, {{name}}</h1>
{%endblock%}
~~~~





