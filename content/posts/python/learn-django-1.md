---
title: "初识 Django"
date: 2023-09-02T20:40:50+08:00
lastmod: 2024-01-02T20:40:50+08:00
categories: ["Python"]
tags: ["Django"]
author: "Waite Wang"
showToc: true
TocOpen: true
draft: false
hidemeta: false
description: "Django 是一个高级的 Python Web 框架，它鼓励快速开发和干净、实用的设计。"
disableHLJS: true
disableShare: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
---

## 认识 Django

### 简介

Django(发音:[`dʒæŋɡəʊ]) 也有的小伙伴读成 “酱狗”，"贱狗"，"进狗"，"撞狗"，甚至还有读成"打
狗"。
官方：<https://www.djangoproject.com/>
Django是一个高级的Python Web框架，可以快速开发安全和可维护的网站。由经验丰富的开发者构
建，Django负责处理网站开发中麻烦的部分，可以专注于编写应用程序，而无需重新开发。它是免费和
开源的，有活跃繁荣的社区，丰富的文档，以及很多免费和付费的解决方案。目前最新版本：5.0.1

### 安装

pip安装：

```
pip install Django==5.0.1 -i https://pypi.tuna.tsinghua.edu.cn/simple
```

执行安装完成后，在python目录的Scripts下，会多出一个`diango-admin.exe` 这个是`django`项目创建工具

![image-20240530202013347](https://qiniu.waite.wang/202405302020692.png)

当然同时Lib下的`site-packages`下，也会有一个`django`目录，这个是后面我们开发项目会用到的`django`开发包。

![image-20240530202047189](https://qiniu.waite.wang/202405302020300.png)

### 项目创建与项目配置

#### Django5创建项目用命令方式

通过以下命令可以查看版本号：

```
## python3 -V
Python 3.9.7
## python3 -m django --version
4.2.7
```

```
django-admin startproject 项目名称
```

![image-20240530202153533](https://qiniu.waite.wang/202405302021361.png)

#### Django5创建项目用PyCharm工具

![image-20240530202246884](https://qiniu.waite.wang/202405302022094.png)

### 目录结构

使用 django-admin 来创建 HelloWorld 项目：

```
django-admin startproject HelloWorld
```

创建完成后我们可以查看下项目的目录结构：

```
$ cd HelloWorld/
$ tree
.
|-- HelloWorld
|   |-- __init__.py
|   |-- asgi.py
|   |-- settings.py
|   |-- urls.py
|   `-- wsgi.py
`-- manage.py
```

目录说明：

- **HelloWorld:** 项目的容器。
- **manage.py:** 一个实用的命令行工具，可让你以各种方式与该 Django 项目进行交互。
- **HelloWorld/**init**.py:** 一个空文件，告诉 Python 该目录是一个 Python 包。
- **HelloWorld/asgi.py:** 一个 ASGI 兼容的 Web 服务器的入口，以便运行你的项目。
- **HelloWorld/settings.py:** 该 Django 项目的设置/配置。
- **HelloWorld/urls.py:** 该 Django 项目的 URL 声明; 一份由 Django 驱动的网站"目录"。
- **HelloWorld/wsgi.py:** 一个 WSGI 兼容的 Web 服务器的入口，以便运行你的项目。

接下来我们进入 HelloWorld 目录输入以下命令，启动服务器：

```
python3 manage.py runserver 0.0.0.0:8000
```

0.0.0.0 让其它电脑可连接到开发服务器，8000 为端口号。如果不说明，那么端口号默认为 8000。

在浏览器输入你服务器的 ip（这里我们输入本机 IP 地址： **127.0.0.1:8000**） 及端口号，如果正常启动，输出结果如下：

![img](https://qiniu.waite.wang/202405302024176.jpeg)

### 视图和 URL 配置

在先前创建的 HelloWorld 目录下的 HelloWorld 目录新建一个 views.py 文件，并输入代码：

```python
from django.http import HttpResponse
 
def hello(request):
    return HttpResponse("Hello world ! ")
```

接着，绑定 URL 与视图函数。打开 urls.py 文件，删除原来代码，将以下代码复制粘贴到 urls.py 文件中：

```python
from django.urls import path
 
from . import views
 
urlpatterns = [
    path("", views.hello, name="hello"),
]
```

整个目录结构如下：

```
$ tree
.
|-- HelloWorld
|   |-- __init__.py
|   |-- __init__.pyc
|   |-- settings.py
|   |-- settings.pyc
|   |-- urls.py              ## url 配置
|   |-- urls.pyc
|   |-- views.py              ## 添加的视图文件
|   |-- views.pyc             ## 编译后的视图文件
|   |-- wsgi.py
|   `-- wsgi.pyc
`-- manage.py
```

完成后，启动 Django 开发服务器，并在浏览器访问打开浏览器并访问：

![img](https://qiniu.waite.wang/202405302027460.jpeg)

我们也可以修改以下规则：

```python
## HelloWorld/HelloWorld/urls.py 文件代码：
from django.urls import path
 
from . import views
 
urlpatterns = [
    path('hello/', views.hello),
]
```

通过浏览器打开 **<http://127.0.0.1:8000/hello**，输出结果如下：>

![img](https://qiniu.waite.wang/202405302028324.jpeg)

**注意：**项目中如果代码有改动，服务器会自动监测代码的改动并自动重新载入，所以如果你已经启动了服务器则不需手动重启。

### 创建app

```
- 项目
 - app，用户管理【表结构、函数、HTML模板、CSS】
 - app，订单管理【表结构、函数、HTML模板、CSS】
 - app，后台管理【表结构、函数、HTML模板、CSS】
 - app，网站   【表结构、函数、HTML模板、CSS】
 - app，API    【表结构、函数、HTML模板、CSS】
 ..
 
注意：我们开发比较简洁，用不到多app，一般情况下，项目下创建1个app即可。
```

![image-20240601153948318](https://qiniu.waite.wang/202406011539511.png)

```
├── app01
│   ├── __init__.py
│   ├── admin.py         【固定，不用动】django默认提供了admin后台管理。
│   ├── apps.py          【固定，不用动】app启动类
│   ├── migrations       【固定，不用动】数据库变更记录
│   │   └── __init__.py
│   ├── models.py        【**重要**】，对数据库操作。
│   ├── tests.py         【固定，不用动】单元测试
│   └── views.py         【**重要**】，函数。
├── manage.py
└── mysite2
    ├── __init__.py
    ├── asgi.py
    ├── settings.py
    ├── urls.py          【URL->函数】
    └── wsgi.py
```

## Django 模板

我们接着上一章节的项目将在 HelloWorld 目录底下创建 templates 目录并建立`hello.html`文件，整个目录结构如下：

```
HelloWorld/
|-- HelloWorld
|   |-- __init__.py
|   |-- __init__.pyc
|   |-- settings.py
|   |-- settings.pyc
|   |-- urls.py
|   |-- urls.pyc
|   |-- views.py
|   |-- views.pyc
|   |-- wsgi.py
|   `-- wsgi.pyc
|-- manage.py
`-- templates
    `-- hello.html
```

```html
<!-- hello.html -->
<h1> {{ hello }} </h1>
```

我们现在修改 views.py，增加一个新的对象，用于向模板提交数据：

```python
## views.py 
def hello_world(request):
    context = {'hello': 'Hello World11!'}
    return render(request, 'hello.html', context)
```

```python
## urls
from django.urls import path

from . import views

urlpatterns = [
    path("hello/", views.hello_world, name="hello_world"),
]
```

![img](https://qiniu.waite.wang/6BC47EC1-ABC8-4EA7-A536-756648FBBC20.jpg)

### Django 模板标签

#### 变量

模板语法：

```
view：｛"HTML变量名" : "views变量名"｝
HTML：｛｛变量名｝｝
```

```python
## views.py

from django.shortcuts import render

def runoob(request): 
  views_name = "菜鸟教程" 
  return  render(request,"runoob.html", {"name":views_name})
```

templates 中的 runoob.html ：

```
<p>{{ name }}</p>
```

![img](https://qiniu.waite.wang/202406011609908.jpeg)

#### 列表

```python
from django.shortcuts import render

def runoob(request):
    views_list = ["菜鸟教程1","菜鸟教程2","菜鸟教程3"]
    return render(request, "runoob.html", {"views_list": views_list})
```

```html
<p>{{ views_list }}</p>   ## 取出整个列表
<p>{{ views_list.0 }}</p> ## 取出列表的第一个元素
```

![image-20240601161226606](https://qiniu.waite.wang/202406011612712.png)

#### 过滤器

模板语法：

```
{{ 变量名 | 过滤器：可选参数 }}
```

模板过滤器可以在变量被显示前修改它，过滤器使用管道字符，如下所示：

```
{{ name|lower }}
```

{{ name }} 变量被过滤器 lower 处理后，文档大写转换文本为小写。

过滤管道可以被*套接* ，既是说，一个过滤器管道的输出又可以作为下一个管道的输入：

```
{{ my_list|first|upper }}
```

以上实例将第一个元素并将其转化为大写。

有些过滤器有参数。 过滤器的参数跟随冒号之后并且总是以双引号包含。 例如：

```
{{ bio|truncatewords:"30" }}
```

这个将显示变量 bio 的前30个词。

其他过滤器：

- addslashes : 添加反斜杠到任何反斜杠、单引号或者双引号前面。

- date : 按指定的格式字符串参数格式化 date 或者 datetime 对象，实例：

  ```
  {{ pub_date|date:"F j, Y" }}
  ```

- length : 返回变量的长度。

**default**

default 为变量提供一个默认值。

如果 views 传的变量的布尔值是 false，则使用指定的默认值。

以下值为 false：

```
0  0.0  False  0j  ""  []  ()  set()  {}  None
```

```python
from django.shortcuts import render

def runoob(request):
    name =0
    return render(request, "runoob.html", {"name": name})
```

```html
{{ name|default:"菜鸟教程666" }}
```

![image-20240601161412458](https://qiniu.waite.wang/202406011614220.png)

**length**

返回对象的长度，适用于字符串和列表。

字典返回的是键值对的数量，集合返回的是去重后的长度。

```html
{{ name|length}}
```

**filesizeformat**

以更易读的方式显示文件的大小（即'13 KB', '4.1 MB', '102 bytes'等）。

字典返回的是键值对的数量，集合返回的是去重后的长度。

```python
from django.shortcuts import render

def runoob(request):
    num=1024
    return render(request, "runoob.html", {"num": num})
```

```
{{ num|filesizeformat}}
```

![img](https://qiniu.waite.wang/202406011615570.png)

**date**

根据给定格式对一个日期变量进行格式化。

格式 **Y-m-d H:i:s**返回 **年-月-日 小时:分钟:秒** 的格式时间。

```python
from django.shortcuts import render

def runoob(request):
    import datetime
    now  =datetime.datetime.now()
    return render(request, "runoob.html", {"time": now})
```

```
{{ time|date:"Y-m-d" }}
```

![img](https://qiniu.waite.wang/202406011617906.png)

**truncatechars**

如果字符串包含的字符总个数多于指定的字符数量，那么会被截断掉后面的部分。

截断的字符串将以 **...** 结尾。

```python
from django.shortcuts import render

def runoob(request):
    views_str = "菜鸟教程"
    return render(request, "runoob.html", {"views_str": views_str})
```

```
{{ views_str|truncatechars:2}}
```

![img](https://qiniu.waite.wang/202406011617607.png)

**safe**

将字符串标记为安全，不需要转义。

要保证 views.py 传过来的数据绝对安全，才能用 safe。

和后端 views.py 的 mark_safe 效果相同。

Django 会自动对 views.py 传到HTML文件中的标签语法进行转义，令其语义失效。加 safe 过滤器是告诉 Django 该数据是安全的，不必对其进行转义，可以让该数据语义生效。

```python
from django.shortcuts import render

def runoob(request):
    views_str = "<a href='https://www.runoob.com/'>点击跳转</a>"
    return render(request, "runoob.html", {"views_str": views_str})
```

```
{{ views_str|safe }}
```

> 对比

![image-20240601161931407](https://qiniu.waite.wang/202406011619757.png)

![image-20240601161951788](https://qiniu.waite.wang/202406011619704.png)

#### if/else 标签

基本语法格式如下：

```
{% if condition %}
     ... display
{% endif %}
```

或者：

```
{% if condition1 %}
   ... display 1
{% elif condition2 %}
   ... display 2
{% else %}
   ... display 3
{% endif %}
```

根据条件判断是否输出。if/else 支持嵌套。

{% if %} 标签接受 and ， or 或者 not 关键字来对多个变量做判断 ，或者对变量取反（ not )，例如：

```
{% if athlete_list and coach_list %}
     athletes 和 coaches 变量都是可用的。
{% endif %}
```

#### for 标签

{% for %} 允许我们在一个序列上迭代。

与 Python 的 for 语句的情形类似，循环语法是 for X in Y ，Y 是要迭代的序列而 X 是在每一个特定的循环中使用的变量名称。

每一次循环中，模板系统会渲染在 **{% for %}** 和 **{% endfor %}** 之间的所有内容。

例如，给定一个运动员列表 athlete_list 变量，我们可以使用下面的代码来显示这个列表：

```
<ul>
{% for athlete in athlete_list %}
    <li>{{ athlete.name }}</li>
{% endfor %}
</ul>
```

给标签增加一个 reversed 使得该列表被反向迭代：

```
{% for athlete in athlete_list reversed %}
...
{% endfor %}
```

**遍历字典**: 可以直接用字典 **.items** 方法，用变量的解包分别获取键和值。

在 {% for %} 标签里可以通过 {{forloop}} 变量获取循环序号。

- forloop.counter: 顺序获取循环序号，从 1 开始计算
- forloop.counter0: 顺序获取循环序号，从 0 开始计算
- forloop.revcounter: 倒序获取循环序号，结尾序号为 1
- forloop.revcounter0: 倒序获取循环序号，结尾序号为 0
- forloop.first（一般配合if标签使用）: 第一条数据返回 True，其他数据返回 False
- forloop.last（一般配合if标签使用）: 最后一条数据返回 True，其他数据返回 False

### 模块化

![image-20240605150947706](https://qiniu.waite.wang/202406051509154.png)

调整 `djangoProject/urls.py`

```python
from django.urls import path, include

from app import views

## from . import views

urlpatterns = [
  path('book/', include("app.urls")),
]
```

`app/urls.py`

```python
from django.urls import path
from app import views

urlpatterns = [
  path("home", views.home, name="home"),
  path("add", views.add_book, name="add_book"),
  path("query", views.query_book, name="query_book"),
  path("delete", views.delete_book, name="delete_book"),
  path("update", views.update_book, name="update_book"),
]
```

![image-20240605151102668](https://qiniu.waite.wang/202406051511917.png)

## 数据库操作

### 数据库配置

安装环境

```
pip install mysqlclient
```

更改 settings.py 中的配置

```python
DATABASES = { 
    'default': 
    { 
        'ENGINE': 'django.db.backends.mysql',    ## 数据库引擎
        'NAME': 'runoob', ## 数据库名称
        'HOST': '127.0.0.1', ## 数据库地址，本机 ip 地址 127.0.0.1 
        'PORT': 3306, ## 端口 
        'USER': 'root',  ## 数据库用户名
        'PASSWORD': '123456', ## 数据库密码
    }  
}
```

数据库配置是选择项目所使用的数据库的类型，不同的数据库需要设置不同的数据库引擎，数据库引擎

用于实现项目与数据库的连接，Django提供4种数据库引擎:

- 'django.db.backends.postgresql'
- 'django.db.backends.mysql'
- 'django.db.backends.sqlite3'
- 'django.db.backends.oracle'

我们有两种方式进行数据库的操作, 如下

### 普通 SQL 增删查改

```python
## app/views

from django.shortcuts import render
from django.db import connection


## Create your views here.
def home(request):
    ## 获取数据库连接
    cursor = connection.cursor()

    ## 创建 book 表如果不存在
    cursor.execute("CREATE TABLE IF NOT EXISTS book (id INT PRIMARY KEY AUTO_INCREMENT, name VARCHAR(255))")

    ## 插入数据
    cursor.execute("INSERT INTO book (name) VALUES ('Python')")

    ## 查询数据
    cursor.execute("SELECT * FROM book")

    ## 获取所有数据
    books = cursor.fetchall()

    ## 更改数据
    if books[0][1] == 'Python':
        cursor.execute("UPDATE book SET name = 'Java' WHERE id = 1")
    else:
        cursor.execute("UPDATE book SET name = 'Python' WHERE id = 1")

    ## 输出数据
    for book in books:
        print(book)

    ## 关闭数据库连接
    cursor.close()

    return render(request, "home.html", {"books": books})
```

```python
## 主项目/urls
from django.urls import path
from app import views

urlpatterns = [
    path("home/", views.home, name="home"),
]
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
{% for book in books %}
    <h1>{{ book}}</h1>
{% endfor %}
</body>
</html>
```

> 效果如下

![image-20240602205856202](https://qiniu.waite.wang/202406022058040.png)

### Django ORM

Django 模型使用自带的 ORM。

对象关系映射（Object Relational Mapping，简称 ORM ）用于实现面向对象编程语言里不同类型系统的数据之间的转换。

ORM 在业务逻辑层和数据库层之间充当了桥梁的作用。

ORM 是通过使用描述对象和数据库之间的映射的元数据，将程序中的对象自动持久化到数据库中。

![img](https://qiniu.waite.wang/202406021619602.png)

使用 ORM 的好处：

- 提高开发效率。
- 不同数据库可以平滑切换。

使用 ORM 的缺点：

- ORM 代码转换为 SQL 语句时，需要花费一定的时间，执行效率会有所降低。
- 长期写 ORM 代码，会降低编写 SQL 语句的能力。

ORM 解析过程:

- 1、ORM 会将 Python 代码转成为 SQL 语句。
- 2、SQL 语句通过 pymysql 传送到数据库服务端。
- 3、在数据库中执行 SQL 语句并将结果返回。

ORM 对应关系表：

![img](https://qiniu.waite.wang/202406021619663.png)

#### 定义模型

Django 规定，如果要使用模型，必须要创建一个 app。我们使用以下命令创建一个 app 的 app:

```
django-admin startapp app
```

创建一个新的 模块, 更改其中的 app.models

```python
from django.db import models

## Create your models here.
class Book(models.Model):
  name = models.CharField(max_length=255)
  author = models.CharField(max_length=255)
  pub_time = models.DateField(auto_now_add=True)
  price = models.FloatField(default=0)
```

以上的类名代表了数据库表名，且继承了models.Model，类里面的字段代表数据表中的字段(name)，数据类型则由CharField（相当于varchar）、DateField（相当于datetime）， max_length 参数限定长度。

接下来在 settings.py 中找到INSTALLED_APPS这一项，如下：

```python
INSTALLED_APPS = (
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'app',               ## 添加此项
)
```

在命令行中运行：

```cmd
python3 manage.py makemigrations  ## 让 Django 知道我们在我们的模型有一些变更
python3 manage.py migrate   ## 创建表结构
```

![image-20240603144151895](https://qiniu.waite.wang/202406031441224.png)

执行此命令, 也会把 django 自带的数据库导入进数据库中

![image-20240603144845731](https://qiniu.waite.wang/202406031448969.png)

编辑 `mysite/settings.py` 文件前，先设置 [`TIME_ZONE`](https://docs.djangoproject.com/zh-hans/5.0/ref/settings/#std-setting-TIME_ZONE) 为你自己时区。

此外，关注一下文件头部的 [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/5.0/ref/settings/#std-setting-INSTALLED_APPS) 设置项。这里包括了会在你项目中启用的所有 Django 应用。应用能在多个项目中使用，你也可以打包并且发布应用，让别人使用它们。

通常， [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/5.0/ref/settings/#std-setting-INSTALLED_APPS) 默认包括了以下 Django 的自带应用：

- [`django.contrib.admin`](https://docs.djangoproject.com/zh-hans/5.0/ref/contrib/admin/#module-django.contrib.admin) -- 管理员站点， 你很快就会使用它。
- [`django.contrib.auth`](https://docs.djangoproject.com/zh-hans/5.0/topics/auth/#module-django.contrib.auth) -- 认证授权系统。
- [`django.contrib.contenttypes`](https://docs.djangoproject.com/zh-hans/5.0/ref/contrib/contenttypes/#module-django.contrib.contenttypes) -- 内容类型框架。
- [`django.contrib.sessions`](https://docs.djangoproject.com/zh-hans/5.0/topics/http/sessions/#module-django.contrib.sessions) -- 会话框架。
- [`django.contrib.messages`](https://docs.djangoproject.com/zh-hans/5.0/ref/contrib/messages/#module-django.contrib.messages) -- 消息框架。
- [`django.contrib.staticfiles`](https://docs.djangoproject.com/zh-hans/5.0/ref/contrib/staticfiles/#module-django.contrib.staticfiles) -- 管理静态文件的框架。

这些应用被默认启用是为了给常规项目提供方便。

默认开启的某些应用需要至少一个数据表，所以，在使用他们之前需要在数据库中创建一些表。请执行以下命令：

```
python manage.py migrate
```

这个 [`migrate`](https://docs.djangoproject.com/zh-hans/5.0/ref/django-admin/#django-admin-migrate) 命令查看 [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/5.0/ref/settings/#std-setting-INSTALLED_APPS) 配置，并根据 `mysite/settings.py` 文件中的数据库配置和随应用提供的数据库迁移文件（我们将在后面介绍这些），创建任何必要的数据库表。你会看到它应用的每一个迁移都有一个消息。如果你有兴趣，运行你的数据库的命令行客户端，输入 `\dt` （PostgreSQL）， `SHOW TABLES;` （MariaDB，MySQL）， `.tables` （SQLite）或 `SELECT TABLE_NAME FROM USER_TABLES;` （Oracle）来显示 Django 创建的表。

> 就像之前说的，为了方便大多数项目，我们默认激活了一些应用，但并不是每个人都需要它们。如果你不需要某个或某些应用，你可以在运行 [`migrate`](https://docs.djangoproject.com/zh-hans/5.0/ref/django-admin/#django-admin-migrate) 前毫无顾虑地从 [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/5.0/ref/settings/#std-setting-INSTALLED_APPS) 里注释或者删除掉它们。 [`migrate`](https://docs.djangoproject.com/zh-hans/5.0/ref/django-admin/#django-admin-migrate) 命令只会为在 [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/5.0/ref/settings/#std-setting-INSTALLED_APPS) 里声明了的应用进行数据库迁移。

#### 实现基本 CRUD 操作

##### 数据库添加

```python
def add_book(request):
  book = Book(name="Python", author="菜鸟教程", price=1024)
  book.save()

  ## books = Book.objects.create(name="如来神掌", price=200, author="功夫出版社", pub_time="2010-10-10")

  return HttpResponse("添加数据成功！")
```

#### 数据库查询

> 以下可能不全 具体可以参看官方文档

- 使用 **all()** 方法来查询所有内容。返回的是 QuerySet 类型数据，类似于 list，里面放的是一个个模型类的对象，可用索引下标取出模型类的对象。

- **filter()** 方法用于查询符合条件的数据。返回的是 QuerySet 类型数据，类似于 list，里面放的是满足条件的模型类的对象，可用索引下标取出模型类的对象。pk=3 的意思是主键 primary key=3，相当于 id=3。因为 id 在 pycharm 里有特殊含义，是看内存地址的内置函数 id()，因此用 pk。

- **exclude()** 方法用于查询不符合条件的数据。返回的是 QuerySet 类型数据，类似于 list，里面放的是不满足条件的模型类的对象，可用索引下标取出模型类的对象。

- **get()** 方法用于查询符合条件的返回模型类的对象符合条件的对象只能为一个，如果符合筛选条件的对象超过了一个或者没有一个都会抛出错误。

- **order_by()** 方法用于对查询结果进行排序。返回的是 QuerySet类型数据，类似于list，里面放的是排序后的模型类的对象，可用索引下标取出模型类的对象。

  **注意：**

  - a、参数的字段名要加引号。
  - b、降序为在字段前面加个负号 **-**。

- **reverse()** 方法用于对查询结果进行反转。返回的是 QuerySe t类型数据，类似于 list，里面放的是反转后的模型类的对象，可用索引下标取出模型类的对象。

- **count()** 方法用于查询数据的数量返回的数据是整数。

- **first()** 方法返回第一条数据返回的数据是模型类的对象也可以用索引下标 **[0]**。

- last() 方法返回最后一条数据返回的数据是模型类的对象不能用索引下标 **[-1]**，ORM 没有逆序索引。

- **exists()** 方法用于判断查询的结果 QuerySet 列表里是否有数据。返回的数据类型是布尔，有为 true，没有为 false。**注意：**判断的数据类型只能为 QuerySet 类型数据，不能为整型和模型类的对象。

- **values()** 方法用于查询部分字段的数据。返回的是 QuerySet 类型数据，类似于 list，里面不是模型类的对象，而是一个可迭代的字典序列，字典里的键是字段，值是数据。

  **注意：**

  - 参数的字段名要加引号
  - 想要字段名和数据用 **values**实例

```python
def query_book(request):
  books = Book.objects.all()
  ## books = Book.objects.filter(name="Python")
  for book in books:
    print(book.name)

  book_return = [
    {
      "id": book.id,
      "name": book.name,
      "author": book.author,
      "price": book.price
    } for book in books
  ]

  return HttpResponse("查询数据成功！" + str(book_return))
```

##### 删除

**方式一：**使用模型类的 **对象.delete()**。

**返回值：**元组，第一个元素为受影响的行数。

```python
books=models.Book.objects.filter(pk=8).first().delete()
```

**方式二**：使用 QuerySet **类型数据.delete()**(推荐)

**返回值：**元组，第一个元素为受影响的行数。

```python
books=models.Book.objects.filter(pk__in=[1,2]).delete()
```

```python
def delete_book(request):
  ## 使用 QuerySet 类型数据.delete()(推荐)
  books = Book.objects.filter(pk__in=[1, 2]).delete()
  print(books)

  ## 使用模型类的 对象.delete()。
  books = Book.objects.filter(name="Python").first().delete()
  print(books)
  return HttpResponse("删除数据成功！")
```

**注意：**

- a. Django 删除数据时，会模仿 SQL约束 ON DELETE CASCADE 的行为，也就是删除一个对象时也会删除与它相关联的外键对象。
- b. delete() 方法是 QuerySet 数据类型的方法，但并不适用于 Manager 本身。也就是想要删除所有数据，不能不写 all。

```python
books=models.Book.objects.delete()　 ## 报错
books=models.Book.objects.all().delete()　　 ## 删除成功
```

##### 修改

**方式一：**

```
模型类的对象.属性 = 更改的属性值
模型类的对象.save()
```

**返回值：**编辑的模型类的对象。

```python
def update_book(request):
  book = Book.objects.filter(name="Python").first()
  book.name = "Java"
  book.save()
  return HttpResponse("更新数据成功！")
```

**方式二：**QuerySet 类型数据.update(字段名=更改的数据)（推荐）

**返回值：**整数，受影响的行数

```python
Book.objects.filter(name="Java").update(price=100)
```

#### 模型常用Field和参数

##### 常用字段

在Django中，定义了一些Field来与数据库表中的字段类型来进行映射。以下将介绍那些常用的字段类

型。

1. AutoField：映射到数据库中是int类型，可以有自动增长的特性。一般不需要使用这个类型，如果不指定主键，那么模型会自动的生成一个叫做id的自动增长的主键。如果你想指定一个其他名字的并且具有自动增长的主键，使用AutoField也是可以的。
2. BigAutoField：64 位的整形，类似于AutoField，只不过是产生的数据的范围是从1-9223372036854775807。

3. BooleanField：在模型层面接收的是True/False。在数据库层面是tinyint类型。如果没有指定默认值，默认值是None。

4. CharField：在数据库层面是varchar类型。在Python层面就是普通的字符串。这个类型在使用的时候必须要指定最大的长度，也即必须要传递max_length这个关键字参数进去。

5. DateField：日期类型。在Python中是datetime.date类型，可以记录年月日。在映射到数据库中也是date类型。使用这个Field可以传递以下几个参数：
   1. auto_now：在每次这个数据保存的时候，都使用当前的时间。比如作为一个记录修改日期的字段，可以将这个属性设置为True。

   2. auto_now_add：在每次数据第一次被添加进去的时候，都使用当前的时间。比如作为一个记录第一次入库的字段，可以将这个属性设置为True。

6. DateTimeField：日期时间类型，类似于DateField。不仅仅可以存储日期，还可以存储时间。映射到数据库中是datetime类型。这个Field也可以使用auto_now和auto_now_add两个属性。

7. TimeField：时间类型。在数据库中是time类型。在Python中是datetime.time类型。

8. EmailField：类似于CharField。在数据库底层也是一个varchar类型。最大长度是 254 个字符。

9. FileField：用来存储文件的。这个请参考后面的文件上传章节部分。

10. ImageField：用来存储图片文件的。这个请参考后面的图片上传章节部分。

11. FloatField：浮点类型。映射到数据库中是float类型。

12. IntegerField：整形。值的区间是-2147483648——2147483647。

13. BigIntegerField：大整形。值的区间是-9223372036854775808——9223372036854775807。

14. PositiveIntegerField：正整形。值的区间是0——2147483647。

15. SmallIntegerField：小整形。值的区间是-32768——32767。

16. PositiveSmallIntegerField：正小整形。值的区间是0——32767。

17. TextField：大量的文本类型。映射到数据库中是longtext类型。

18. UUIDField：只能存储uuid格式的字符串。uuid是一个 32 位的全球唯一的字符串，一般用来作为主键。

19. URLField：类似于CharField，只不过只能用来存储url格式的字符串。并且默认的max_length是 200 。

##### Field的常用参数

1. null：如果设置为True，Django将会在映射表的时候指定是否为空。默认是为False。在使用字符串相关的Field（CharField/TextField）的时候，官方推荐尽量不要使用这个参数，也就是保持默认值False。因为Django在处理字符串相关的Field的时候，即使这个Field的null=False，如果你没有给这个Field传递任何值，那么Django也会使用一个空的字符串""来作为默认值存储进去。因此如果再使用null=True，Django会产生两种空值的情形（NULL或者空字符串）。如果想要在表单验证的时候允许这个字符串为空，那么建议使用blank=True。如果你的Field是BooleanField，那么对应的可空的字段则为NullBooleanField。
2. blank：标识这个字段在表单验证的时候是否可以为空。默认是False。这个和null是有区别的，null是一个纯数据库级别的。而blank是表单验证级别的。

3. db_column：这个字段在数据库中的名字。如果没有设置这个参数，那么将会使用模型中属性的名字。

4. default：默认值。可以为一个值，或者是一个函数，但是不支持lambda表达式。并且不支持列表/字典/集合等可变的数据结构。

5. primary_key：是否为主键。默认是False。

6. unique：在表中这个字段的值是否唯一。一般是设置手机号码/邮箱等。

> 更多Field参数请参考官方文档：<https://docs.djangoproject.com/zh-hans/5.0/ref/models/fields/>

##### 模型中 Meta 配置

1. db_table：这个模型映射到数据库中的表名。如果没有指定这个参数，那么在映射的时候将会使用模型名来作为默认的表名。
2. ordering：设置在提取数据的排序方式。后面章节会讲到如何查找数据。比如我想在查找数据的时候根据添加的时间排序，那么示例代码如下：

```python
class Book(models.Model):
    name = models.CharField(max_length=20,null=False)
    desc = models.CharField(max_length=100,name='description',db_column="description1")
    pub_date = models.DateTimeField(auto_now_add=True)
    class Meta:
        db_table = 'book_model'
        ordering = ['pub_date']
```

更多的配置后面会慢慢介绍到。 官方文档：<https://docs.djangoproject.com/zh-hans/5.0/ref/models/>

#### 外键使用

在MySQL中，表有两种引擎，一种是`InnoDB`，另外一种是`myisam`。如果使用的是`InnoDB`引擎，是支持外键约束的。外键的存在使得ORM框架在处理表关系的时候异常的强大。因此这里我们首先来介绍下外键在Django中的使用。

> 1. **InnoDB**:
>    - **事务支持**：InnoDB支持事务处理，具有提交(commit)、回滚(rollback)和崩溃恢复能力，适合需要事务处理的应用。
>    - **行级锁定**：InnoDB使用行级锁定和MVCC（多版本并发控制），在高并发环境下性能更好。
>    - **外键约束**：InnoDB支持外键约束，有助于保持数据的完整性。
>    - **崩溃恢复**：InnoDB具有崩溃恢复的能力，可以保证数据的安全性。
>    - **存储限制**：InnoDB表有16TB的存储限制。
>    - **默认引擎**：从MySQL 5.5.5版本开始，InnoDB成为了MySQL的默认存储引擎。
> 2. **MyISAM**:
>    - **表级锁定**：MyISAM使用表级锁定，这意味着在高并发环境下，性能可能不如InnoDB。
>    - **全文索引**：MyISAM提供了全文索引功能，适合需要全文搜索的应用。
>    - **没有事务支持**：MyISAM不支持事务处理，适用于不需要事务的应用。
>    - **存储限制**：MyISAM表有256TB的存储限制。
>    - **快速读取**：MyISAM通常在读取密集型的应用中表现更好，因为它的索引结构设计得更简单。
>
> 选择哪种存储引擎取决于你的应用需求。如果需要事务支持、行级锁定和外键约束，InnoDB通常是更好的选择。如果应用主要是读取密集型，并且需要全文索引功能，那么MyISAM可能更合适。不过，由于InnoDB的广泛特性和性能优势，它在现代应用中被广泛使用。

##### 定义

类定义为 class ForeignKey(to,on_delete,**options) 。第一个参数是引用的是哪个模型，第二个参数是在使用外键引用的模型数据被删除了，这个字段该如何处理，比如有 CASCADE 、 SET_NULL 等。这里以一个实际案例来说明。比如有一个 User 和一个 Article 两个模型。一个 User 可以发表多篇文章，一个 Article 只能有一个 Author ，并且通过外键进行引用。那么相关的示例代码如下：

```python
from django.db import models


class User(models.Model):
  username = models.CharField(max_length=255)
  password = models.CharField(max_length=255)


class Article(models.Model):
  title = models.CharField(max_length=255)
  content = models.TextField()

  ## 外键
  author = models.ForeignKey('User', on_delete=models.CASCADE)
```

> 如果不同的 app, 可以这样子使用外键 `author = models.ForeignKey('app.User', on_delete=models.CASCADE)`

以上使用 ForeignKey 来定义模型之间的关系。即在 article 的实例中可以通过 author 属性来操作对应的 User 模型。这样使用起来非常的方便。示例代码如下：

```python
from django.shortcuts import HttpResponse

from .models import User, Article


def article_test(request):
  ## user = User(username="admin", password="123456")
  ## user.save()
  #
  ## article = Article(title="Django", content="Django 是一个开放源代码的 Web 应用框架，由 Python 编写。", author=user)
  ## article.save()
  article = Article.objects.first()
  return HttpResponse(article.author.username)

```

![image-20240605171858982](https://qiniu.waite.wang/202406051719076.png)

如果模型的外键引用的是本身自己这个模型，那么 to 参数可以为 'self' ，或者是这个模型的名字。在论坛开发中，一般评论都可以进行二级评论，即可以针对另外一个评论进行评论，那么在定义模型的时候就需要使用外键来引用自身。示例代码如下：

```python
class Comment(models.Model):
  content = models.TextField()
  article = models.ForeignKey('self', on_delete=models.CASCADE, null=True)
  ## or
  article = models.ForeignKey('Comment', on_delete=models.CASCADE, null=True)
```

##### 外键删除操作

在数据库中使用外键（Foreign Key）时，我们需要考虑当外键所引用的记录被删除时，应如何处理与该记录相关联的其他记录。Django ORM 提供了几种 `on_delete` 选项来处理这种情况，每种选项都定义了不同的行为。以下是这些选项的解释：

1. **CASCADE**：当外键所引用的主键记录被删除时，所有引用该记录的外键记录也将被自动删除。这种操作称为级联删除，它确保了数据库的引用完整性。
2. **PROTECT**：当尝试删除一个被外键引用的主键记录时，如果存在引用，Django 会抛出一个 `ProtectedError` 异常，阻止删除操作。这是一种保护机制，确保数据的完整性不被破坏。
3. **SET_NULL**：如果外键所引用的主键记录被删除，那么所有引用该记录的外键字段将被设置为 `NULL`。使用这个选项之前，需要确保外键字段在数据库中允许为空（`null=True`）。
4. **SET_DEFAULT**：当外键所引用的主键记录被删除时，所有引用该记录的外键字段将被设置为字段的默认值。这要求外键字段在定义时有一个默认值。
5. **SET()**：允许你指定一个可调用的对象（如函数或方法），当外键所引用的主键记录被删除时，Django 会调用这个对象，并使用返回的值来更新外键字段。这提供了高度的灵活性，可以根据业务逻辑来决定如何更新外键字段。
6. **DO_NOTHING**：这个选项不会在 Django 层面上做任何操作，所有的约束都依赖于数据库级别的约束。如果数据库设置为不允许删除被引用的记录（如 `RESTRICT`），那么删除操作将会失败。

需要注意的是，尽管 Django 提供了多种 `on_delete` 选项，但数据库层面的行为不会改变。例如，如果数据库设置为 `RESTRICT`，则即使 Django 设置为 `CASCADE`，数据库也不会执行级联删除，而是会阻止删除操作。因此，在使用 `on_delete` 选项时，还需要考虑数据库层面的约束。

#### 表的关系

表与表之间的关系可分为以下三种：

- **一对一**: 一个人对应一个身份证号码，数据字段设置 unique。
- **一对多**: 一个家庭有多个人，一般通过外键来实现。
- **多对多**: 一个学生有多门课程，一个课程有很多学生，一般通过第三个表来实现关联。

##### 一对多

1. 应用场景：比如文章和作者之间的关系。一个文章只能由一个作者编写，但是一个作者可以写多篇文章。文章和作者之间的关系就是典型的多对一的关系。

2. 实现方式：一对多或者多对一，都是通过ForeignKey来实现的。还是以文章和作者的案例进行讲解。那么以后在给Article对象指定author，就可以使用以下代码来完成：

```python
from django.db import models


class User(models.Model):
  username = models.CharField(max_length=255)
  password = models.CharField(max_length=255)


class Article(models.Model):
  title = models.CharField(max_length=255)
  content = models.TextField()

  ## 外键
  author = models.ForeignKey('User', on_delete=models.CASCADE)
```

并且以后如果想要获取某个用户下所有的文章，可以通过`article_set`来实现。示例代码如下：

```python
def article_test(request):
  user = User.objects.first()
  ## ## user.save()
  ## #
  ## article = Article(title="Django12", content="Django 是一个开放源代码的 Web 应用框架，由 Python 编写。", author=user)
  ## article.save()
  article = user.article_set.all()
  for a in article:
    print(a.title)
    print(a.content)
    print(a.author.username)
  return HttpResponse("添加数据成功！")
```

##### 一对一

1. 应用场景：比如一个用户表和一个用户信息表。在实际网站中，可能需要保存用户的许多信息，但是有些信息是不经常用的。如果把所有信息都存放到一张表中可能会影响查询效率，因此可以把用户的一些不常用的信息存放到另外一张表中我们叫做`UserExtension`。但是用户表`User`和用户信息表`UserExtension`就是典型的一对一了。

2. 实现方式：Django为一对一提供了一个专门的Field叫做`OneToOneField`来实现一对一操作。示例代码如下：

```python
from django.db import models


class User(models.Model):
  username = models.CharField(max_length=255)
  password = models.CharField(max_length=255)


class UserExtension(models.Model):
  birthday = models.DateField(null=True)
  school = models.CharField(max_length=255)
  ## user = models.OneToOneField('User', on_delete=models.CASCADE, related_name='extension')
  user = models.OneToOneField('User', on_delete=models.CASCADE)
```

![image-20240605174601778](https://qiniu.waite.wang/202406051746248.png)

> 只允许一对一, 超过既会报错,
>
> 在`UserExtension`模型上增加了一个一对一的关系映射。其实底层是在`UserExtension`这个表上增加了一个user_id，来和user表进行关联，并且这个外键数据在表中必须是唯一的，来保证一对一。
>

##### 多对多

1. 应用场景：比如文章和标签的关系。一篇文章可以有多个标签，一个标签可以被多个文章所引用。因此标签和文章的关系是典型的多对多的关系。

2. 实现方式：Django为这种多对多的实现提供了专门的Field。叫做ManyToManyField。还是拿文章和标签为例进行讲解。示例代码如下：

```python
class Article(models.Model):
  title = models.CharField(max_length=255)
  content = models.TextField()

  ## 外键
  author = models.ForeignKey('User', on_delete=models.CASCADE)
  ## 多对多
  tags = models.ManyToManyField('Tag')


class Tag(models.Model):
  name = models.CharField(max_length=255)
```

在数据库层面，实际上Django是为这种多对多的关系建立了一个中间表。这个中间表分别定义了两个外键，引用到article和tag两张表的主键。

![image-20240605175459605](https://qiniu.waite.wang/202406051755978.png)

#### related_name和related_query_name

在 Django 中，`related_name` 和 `related_query_name` 是两个与模型的外键关系相关的参数，它们允许你自定义反向关系和查询的名称。

1. `related_name`

`related_name` 是 `ForeignKey` 或 `ManyToManyField` 字段的一个选项，它允许你为反向关系指定一个自定义名称。默认情况下，Django 会使用 `<模型名>_set` 作为反向关系的名称（例如，`Article` 模型的反向关系默认为 `article_set`）。但是，如果你想要一个更有意义的名称或避免名称冲突，你可以使用 `related_name` 来指定。

例如，如果你有一个 `User` 模型和一个 `Article` 模型，并且每个用户可以有多个文章，你可以这样定义模型：

```python
class Article(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    ## 使用related_name指定反向关系名称为'articles'
    author = models.ForeignKey("User", on_delete=models.SET_NULL, null=True, related_name='articles')
```

这样，你就可以通过 `user.articles.all()` 来获取一个用户的所有文章。

如果你不想使用任何自定义的反向关系名称，可以设置 `related_name='+'`，这将禁用自动生成的 `<模型名>_set` 名称。

```python
class Article(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    ## 传递related_name参数，以后在方向引用的时候使用articles进行访问
    author = models.ForeignKey("User",on_delete=models.SET_NULL,null=True,related_name='+')
```

2. `related_query_name`

`related_query_name` 是 `related_name` 的一个补充选项，它允许你自定义在查询集中使用 `filter` 方法时反向关系的名称。默认情况下，Django 会使用 `related_name` 的值加上下划线和字段名来生成查询名称（例如，`articles_title`）。

如果你设置了 `related_name='articles'`，那么在进行反向查询时，你可以这样写：

```python
users = User.objects.filter(articles__title='abc')
```

如果你想要一个不同的查询名称，可以使用 `related_query_name` 来指定：

```python
class Article(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    ## 使用related_name指定反向关系名称为'articles'
    ## 使用related_query_name指定查询时的名称为'article'
    author = models.ForeignKey("User", on_delete=models.SET_NULL, null=True, related_name='articles', related_query_name='article')
```

这样，你就可以使用 `article__title` 来进行查询：

```python
users = User.objects.filter(article__title='abc')
```

请注意，`related_query_name` 只在反向查询中使用，而 `related_name` 用于定义反向关系名称。

最后，你提供的代码片段中有一些格式错误和不完整的部分，我已经根据上下文进行了修正。如果你有更具体的问题或需要进一步的帮助，请随时提问。

#### 查询操作

查找是数据库操作中一个非常重要的技术。查询一般就是使用 filter 、 exclude 以及 get 三个方法来实现。我们可以在调用这些方法的时候传递不同的参数来实现查询需求。在 ORM 层面，这些查询条件都是使用 field + __ + condition 的方式来使用的。以下将那些常用的查询条件来一一解释。

在 Django 中，查询集（QuerySet）提供了多种方法来过滤和查询数据库中的数据。以下是对您提供的查询条件的优化格式和解释：

##### 1. `exact`

##### 执行精确匹配，相当于 SQL 中的 `=` 操作符。如果查询值为 `None`，则在 SQL 中对应 `IS NULL`

**示例**:

```
article = Article.objects.get(id__exact=14)
article = Article.objects.get(id__exact=None)
```

**SQL 翻译**:

```
SELECT ... FROM article WHERE id = 14;
SELECT ... FROM article WHERE id IS NULL;
```

##### 2. `iexact`

执行不区分大小写的模糊匹配，相当于 SQL 中的 `LIKE` 操作符。

**示例**:

```
article = Article.objects.filter(title__iexact='hello world')
```

**SQL 翻译**:

```
SELECT ... FROM article WHERE title LIKE 'hello world';
```

##### 3. `contains`

执行大小写敏感的模糊匹配，字段值包含给定的字符串。

**示例**:

```
articles = Article.objects.filter(title__contains='hello')
```

**SQL 翻译**:

```
SELECT ... WHERE title LIKE BINARY '%hello%';
```

##### 4. `icontains`

与 `contains` 类似，但不区分大小写。

**示例**:

```
articles = Article.objects.filter(title__icontains='hello')
```

**SQL 翻译**:

```
SELECT ... WHERE title LIKE '%hello%';
```

##### 5. `in`

检查字段值是否包含在给定的容器中，容器可以是列表、元组或任何可迭代对象。

**示例**:

```
articles = Article.objects.filter(id__in=[1, 2, 3])
```

**SQL 翻译**:

```
SELECT ... WHERE id IN (1, 2, 3);
```

##### 6. `gt`

检查字段值是否大于给定值。

**示例**:

```
articles = Article.objects.filter(id__gt=4)
```

**SQL 翻译**:

```
SELECT ... WHERE id > 4;
```

##### 7. `gte`

检查字段值是否大于或等于给定值。

##### 8. `lt`

检查字段值是否小于给定值。

##### 9. `lte`

检查字段值是否小于或等于给定值。

##### 10. `startswith`

检查字段值是否以特定字符串开始，大小写敏感。

**示例**:

```
articles = Article.objects.filter(title__startswith='hello')
```

**SQL 翻译**:

```
SELECT ... WHERE title LIKE 'hello%';
```

##### 11. `istartswith`

与 `startswith` 类似，但不区分大小写。

##### 12. `endswith`

检查字段值是否以特定字符串结束，大小写敏感。

**示例**:

```
articles = Article.objects.filter(title__endswith='world')
```

**SQL 翻译**:

```
SELECT ... WHERE title LIKE '%world';
```

##### 13. `iendswith`

与 `endswith` 类似，但不区分大小写。

##### 14. `range`

检查字段值是否在给定的范围内。

**示例**:

```
start_date = datetime(2018, 1, 1)
end_date = datetime(2018, 12, 31)
articles = Article.objects.filter(pub_date__range=(start_date, end_date))
```

**SQL 翻译**:

```
SELECT ... FROM article WHERE pub_date BETWEEN '2018-01-01' AND '2018-12-31';
```

##### 15. `date`

针对 `DateField` 或 `DateTimeField` 类型的字段，可以指定日期的范围。

**示例**:

```
articles = Article.objects.filter(pub_date__date=date(2018, 3, 29))
```

**SQL 翻译**:

```
SELECT ... WHERE DATE(pub_date) = '2018-03-29';
```

##### 16. `year`、`month`、`day`、`week_day`、`time`

这些查询条件允许你根据日期和时间的特定部分进行过滤。

**示例**:

```
articles = Article.objects.filter(pub_date__year=2018)
articles = Article.objects.filter(pub_date__time=datetime.time(12, 12, 12))
```

**SQL 翻译**:

```
SELECT ... WHERE YEAR(pub_date) = 2018;
SELECT ... WHERE TIME(pub_date) = '12:12:12';
```

这些查询条件使得在 Django 中进行数据库查询变得非常灵活和强大。你可以根据需要选择合适的查询条件来获取所需的数据。更多详细信息和高级用法，请参考 [Django 官方文档](https://docs.djangoproject.com/en/5.0/ref/models/querysets/#field-lookups)。

#### 聚合函数

> 前期准备

```python
from django.db import models


class Author(models.Model):
  """作者模型"""
  name = models.CharField(max_length=100)
  age = models.IntegerField()
  email = models.EmailField()

  class Meta:
    db_table = 'front_author'


class Publisher(models.Model):
  """出版社模型"""
  name = models.CharField(max_length=300)

  class Meta:
    db_table = 'front_publisher'


class Book(models.Model):
  """图书模型"""
  name = models.CharField(max_length=300)
  pages = models.IntegerField()
  price = models.FloatField()
  rating = models.FloatField()
  author = models.ForeignKey(Author, on_delete=models.CASCADE)
  publisher = models.ForeignKey(Publisher, on_delete=models.CASCADE)

  class Meta:
    db_table = 'front_book'


class BookOrder(models.Model):
  """图书订单模型"""
  book = models.ForeignKey("Book", on_delete=models.CASCADE)
  price = models.FloatField()

  class Meta:
    db_table = 'front_book_order'
```

```sql
/*
Navicat MySQL Data Transfer

Source Server         : zhiliao
Source Server Version : 50718
Source Host           : localhost:3306
Source Database       : orm_aggregate_demo2

Target Server Type    : MYSQL
Target Server Version : 50718
File Encoding         : 65001
*/

SET FOREIGN_KEY_CHECKS=0;


-- ----------------------------
-- Records of author
-- ----------------------------
INSERT INTO `front_author` VALUES (1, '曹雪芹', '35', 'cxq@qq.com');
INSERT INTO `front_author` VALUES (2, '吴承恩', '28', 'wce@qq.com');
INSERT INTO `front_author` VALUES (3, '罗贯中', '36', 'lgz@qq.com');
INSERT INTO `front_author` VALUES (4, '施耐庵', '46', 'sna@qq.com');


-- ----------------------------
-- Records of book
-- ----------------------------
INSERT INTO `front_book` VALUES ('1', '三国演义', '987', '98', '4.8', '3', '1');
INSERT INTO `front_book` VALUES ('2', '水浒传', '967', '97', '4.83', '4', '1');
INSERT INTO `front_book` VALUES ('3', '西游记', '1004', '95', '4.85', '2', '2');
INSERT INTO `front_book` VALUES ('4', '红楼梦', '1007', '99', '4.9', '1', '2');


-- ----------------------------
-- Records of book_order
-- ----------------------------
INSERT INTO `front_book_order` VALUES ('1', '95', '1');
INSERT INTO `front_book_order` VALUES ('2', '85', '1');
INSERT INTO `front_book_order` VALUES ('3', '88', '1');
INSERT INTO `front_book_order` VALUES ('4', '94', '2');
INSERT INTO `front_book_order` VALUES ('5', '93', '2');


-- ----------------------------
-- Records of publisher
-- ----------------------------
INSERT INTO `front_publisher` VALUES ('1', '中国邮电出版社');
INSERT INTO `front_publisher` VALUES ('2', '清华大学出版社');
```

##### 聚合查询

聚合查询函数是对一组值执行计算，并返回单个值。Django 使用聚合查询前要先从 django.db.models 引入 Avg、Max、Min、Count、Sum（首字母大写）。

```python
from .models import Book
from django.db.models import Avg, Max, Min, Count, Sum


def test(request):
  ## 查询所有图书的平均价格
  avg_price = Book.objects.aggregate(Avg('price'))
  print(avg_price)

  ## 查询所有图书的最高价格
  max_price = Book.objects.aggregate(Max('price'))
  print(max_price)

  ## 查询所有图书的最低价格
  min_price = Book.objects.aggregate(Min('price'))
  print(min_price)

  ## 查询所有图书的价格之和
  sum_price = Book.objects.aggregate(Sum('price'))
  print(sum_price)

  ## 查询所有图书的数量
  count = Book.objects.aggregate(Count('id'))
  print(count)

  return HttpResponse("查询数据成功！")
```

![image-20240605212533357](https://qiniu.waite.wang/202406052125655.png)

##### aggregate() 示例

假设我们想要计算所有书籍的平均价格。

```
from django.db.models import Avg
from .models import Book

## 使用 aggregate() 方法来计算书籍的平均价格
average_price = Book.objects.aggregate(average=Avg('price'))

## average_price 是一个字典，包含键 'average'，其值为平均价格
print(average_price)  ## 输出: {'average': 25.5} 假设平均价格为25.5
```

##### annotate() 示例

假设我们想要为每个出版社添加一个字段，表示该出版社出版的所有书籍的平均价格。

```
from django.db.models import Avg
from .models import Publisher, Book

## 使用 annotate() 方法为每个出版社添加一个平均价格字段
publishers_with_average_price = Publisher.objects.annotate(average_price=Avg('book__price'))

## 现在 publishers_with_average_price 是一个查询集，我们可以通过迭代来查看每个出版社及其平均价格
for publisher in publishers_with_average_price:
    print(f"{publisher.name}: Average Book Price = {publisher.average_price}")
```

在这个例子中，`annotate()` 方法为 `Publisher` 查询集中的每个出版社对象添加了一个 `average_price` 属性，表示该出版社出版的所有书籍的平均价格。

- `aggregate()` 返回一个字典，其中包含了聚合函数的结果，适用于计算总和、平均值、最大值、最小值等聚合数据。
- `annotate()` 返回一个查询集，其中的每个对象都添加了聚合字段，适用于为查询集中的每个对象添加聚合数据相关的字段。

这两个方法都使用 Django 的聚合函数，如 `Sum`、`Avg`、`Max`、`Min` 等，但是它们的使用场景和返回结果不同。`aggregate()` 用于获取单个或多个聚合值，而 `annotate()` 用于扩展查询集中每个对象的属性。

#### F 表达式 以及 Q表达式

##### F 表达式

F 表达式（Field expressions）允许你引用模型字段，并在数据库层面上执行操作，而不是在 Python 层面上。这意味着你可以在查询中使用字段的当前值，而无需在 Python 中加载整个对象。

**使用场景**:

- 当你想基于现有字段的值进行计算或条件判断时。
- 当你想避免在 Python 层面上加载对象，从而提高查询效率时。

**示例**: 假设我们有一个 `Book` 模型，我们想要更新所有书籍的价格，使其变为当前价格的两倍。

```
from .models import Book

Book.objects.update(price=F('price') * 2)
```

在这个例子中，我们没有首先加载所有的 `Book` 对象到 Python 中，而是直接在数据库层面上将 `price` 字段的值乘以 2。

##### Q 表达式

Q 表达式（Query expressions）提供了一种查询接口，允许你构建复杂的查询，可以包含多个条件，并且这些条件可以使用 `&`（和）、`|`（或）和 `~`（非）运算符来组合。

**使用场景**:

- 当你想构建包含多个条件的复杂查询时。
- 当你想使用逻辑运算符组合查询条件时。

**示例**: 假设我们想要查询所有由特定作者编写，并且价格大于 10 的书籍。

```
from .models import Book
from django.db.models import Q

query = Q(author__name='John Doe') & Q(price__gt=10)
books = Book.objects.filter(query)
```

在这个例子中，我们创建了一个 Q 对象，它表示作者是 "John Doe" 并且价格大于 10 的书籍。然后我们使用 `filter()` 方法来获取匹配这些条件的书籍。

- **F 表达式** 用于引用模型字段并在数据库层面上执行操作，如字段值的计算。
- **Q 表达式** 用于构建复杂的查询，可以包含多个逻辑条件，并使用逻辑运算符进行组合。

F 表达式和 Q 表达式都是 Django ORM 中不可或缺的工具，它们可以帮助你编写更高效、更灵活的数据库查询。

## Cookie 和 Session 操作

### Cookie和Session简介

#### 1. Cookie

Cookies用于解决HTTP协议无状态的问题。当用户登录成功后，服务器会返回一些数据（cookie）给浏览器，浏览器将其保存。随后的请求中，浏览器自动携带cookie，服务器通过这些数据识别用户。

- **限制**: Cookie的数据量有限，通常不超过4KB，因此只能存储少量数据。

#### 2. Session

Session用于存储用户信息，与Cookie类似但更安全，因为数据存储在服务器端。不同服务器和框架有不同的实现方式。

#### 3. 使用场景

- **Server Side Session**: 通过cookie存储session ID，数据存储在服务器的session中。
- **Client Side Session**: Session数据加密后存储在cookie中。

### Django中操作Cookie

#### 设置Cookie

使用`response.set_cookie`方法，参数包括：

- `key`: Cookie的键名。
- `value`: Cookie的值。
- `max_age`: 生命周期（秒）。
- `expires`: 过期日期。
- `path`: 有效的路径。
- `domain`: 有效的域名。
- `secure`: 是否仅在HTTPS下有效。
- `httponly`: 是否禁止客户端脚本访问。

#### 删除Cookie

使用`delete_cookie`，将cookie的值设置为空字符串，并设置过期时间为0。

#### 获取Cookie

通过`request.COOKIES`获取，这是一个字典类型对象。

### Django中操作Session

Django的session默认存储在数据库中，通过session ID与cookie交互。操作session使用`request.session`，包括：

- `get`: 获取值。
- `pop`: 删除值。
- `keys`: 获取所有键。
- `items`: 获取所有项。
- `clear`: 清除session数据。
- `flush`: 删除session及cookie中的session ID。
- `set_expiry`: 设置过期时间。
- `clear_expired`: 清理过期session。

示例代码：

```python
# 获取所有cookie
cookies = request.COOKIES
for cookie_key, cookie_value in cookies.items():
    print(cookie_key, cookie_value)

# 使用session
def index(request):
    username = request.session.get('username')
    return HttpResponse('index')
```

### 修改Session的存储机制

Django默认将Session数据存储在数据库中。但您可以通过设置`SESSION_ENGINE`来更改Session的存储位置。以下是几种可选的存储方案：

1. **数据库存储** (`django.contrib.sessions.backends.db`): 默认方案，Session数据存储在数据库中。
2. **文件存储** (`django.contrib.sessions.backends.file`): Session数据存储在服务器的文件系统中。
3. **缓存存储** (`django.contrib.sessions.backends.cache`):
   - 将Session数据存储在缓存中，如Memcached。
   - 需要在`settings.py`中配置`CACHES`。
4. **缓存数据库混合存储** (`django.contrib.sessions.backends.cached_db`):
   - 首先将Session数据存储在缓存中，然后同步到数据库。
   - 即使缓存系统出现问题，Session数据也不会丢失。
5. **加密Cookie存储** (`django.contrib.sessions.backends.signed_cookies`):
   - 将Session信息加密后存储在浏览器的Cookie中。
   - 需要考虑安全性，建议设置`SESSION_COOKIE_HTTPONLY=True`以防止通过JavaScript操作Session数据。
   - 保护`settings.py`中的`SECRET_KEY`，防止解密Session数据。
   - Cookie中存储的数据量不能超过4KB。

请注意，选择存储方案时，需要根据您的应用需求和安全要求来决定最合适的方法。
