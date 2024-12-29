---
title: "Django自带 Auth 数据库扩展字段"
date: 2024-06-22T16:04:23+08:00
categories: ["Python"]
tags: ["Django"]

showToc: true
TocOpen: true # 是否展开目录
disableHLJS: true # to disable highlightjs
weight:
draft: false
---

## auth模块的介绍

- 1、导包

  ```py
  from django.contrib import auth
  ```

- 2、默认使用`auth_user`表来存储用户数据

  ```py
  # 默认用户表是位置
  from django.contrib.auth.models import User
  ```

  ```sql
  mysql> show tables;
  +----------------------------+
  | Tables_in_django_rest01    |
  +----------------------------+
  | auth_group                 |
  | auth_group_permissions     |
  | auth_permission            |
  | auth_user                  |
  | auth_user_groups           |
  | auth_user_user_permissions |
  | django_admin_log           |
  | django_content_type        |
  | django_migrations          |
  | django_session             |
  +----------------------------+
  10 rows in set (0.00 sec)
  
  auth_group: 存储用户组信息。每个用户组可以有多个权限。
  auth_group_permissions: 存储用户组和权限的关联关系。一个用户组可以有多个权限。
  auth_permission: 存储系统中定义的所有权限。权限通常与特定的模型和操作相关联。
  auth_user: 存储用户账户信息，如用户名、密码、电子邮件等。
  auth_user_groups: 存储用户和用户组的关联关系。一个用户可以属于多个用户组。
  auth_user_user_permissions: 存储用户特定的权限，这些权限不会通过用户组继承。
  django_admin_log: 存储管理站点的操作日志，如添加、更改或删除记录。
  django_content_type: 存储Django中所有模型的元数据，包括app_label和model字段，用于确定对象类型。
  django_migrations: 存储关于应用的迁移历史信息，包括迁移的名称和应用时间。
  django_session: 存储会话数据，用于支持Django的会话框架。
  ```

```sql
mysql> desc auth_user;
+--------------+--------------+------+-----+---------+----------------+
| Field        | Type         | Null | Key | Default | Extra          |
+--------------+--------------+------+-----+---------+----------------+
| id           | int(11)      | NO   | PRI | NULL    | auto_increment |
| password     | varchar(128) | NO   |     | NULL    |                |
| last_login   | datetime(6)  | YES  |     | NULL    |                |
| is_superuser | tinyint(1)   | NO   |     | NULL    |                |
| username     | varchar(150) | NO   | UNI | NULL    |                |
| first_name   | varchar(30)  | NO   |     | NULL    |                |
| last_name    | varchar(150) | NO   |     | NULL    |                |
| email        | varchar(254) | NO   |     | NULL    |                |
| is_staff     | tinyint(1)   | NO   |     | NULL    |                |
| is_active    | tinyint(1)   | NO   |     | NULL    |                |
| date_joined  | datetime(6)  | NO   |     | NULL    |                |
+--------------+--------------+------+-----+---------+----------------+
11 rows in set (0.01 sec)

mysql>
```

### 常用方法

- 1、`authenticate()`用来校验用户名及密码

  ```python
  from django.contrib.auth import authenticate, login, logout
  # 如果校验成功就返回当前用户,否则就返回None
  user = authenticate(username='username', password='password')
  ```

- 2、`login(HttpRequest, user)`用户用户登录,使用这个登录方式会生成相关的`session`

- 3、`logout(request)`退出操作,清除相关的`session`

- 4、`is_authenticated()`用来判断当前请求是否通过认证

  ```python
  def my_view(request):
    if not request.user.is_authenticated():
      return redirect('%s?next=%s' % (settings.LOGIN_URL, request.path))
  ```

- 5、`login_requierd()`必须登录的装饰器

  ```python
  from django.contrib.auth.decorators import login_required
  @login_required
  def my_view(request):
      pass
  ```

  若用户没有登录，则会跳转到`django`默认的 登录`URL '/accounts/login/ '` 并传递当前访问`url`的绝对路径 (登陆成功后，会重定向到该路径)。如果需要自定义登录的URL，则需要在`settings.py`文件中通过`LOGIN_URL`进行修改。

  ```python
  LOGIN_URL = '/login/'  # 这里配置成你项目登录页面的路由
  ```

- 6、`create_user()`创建普通用户的

  ```python
  from django.contrib.auth.models import User
  user = User.objects.create_user（username='用户名',password='密码',email='邮箱',...）
  ```

- 7、`create_superuser()`创建超级用户

- 8、`check_password(password)`检查密码是否正确(用户登录)

  ```python
  user.check_password('密码')
  ```

- 9、`set_password(password)`设置密码(用于注册及修改密码)

## 扩展auth_user表

> 默认你创建好了一个 app, 没有的话 `python manage.py startapp web`

1. 修改 `web/models`

```python
from django.db import models
from django.contrib.auth.models import AbstractUser


class UserProfileModel(AbstractUser):
  """
  创建一个用户的模型
  """
  SEX_CHOICES = [
    ('male', '男'),
    ('female', '女')
  ]
  phone = models.CharField(max_length=11, verbose_name='手机号', unique=True)
  birthday = models.DateField(verbose_name='生日', null=True, blank=True)
  sex = models.CharField(max_length=6, choices=SEX_CHOICES, default='', verbose_name='性别')
  # 头像
  avatar = models.CharField(max_length=100, default='default.jpg', verbose_name='头像')
  address = models.CharField(max_length=100, default='', verbose_name='地址')

  class Meta(object):
    db_table = 'user_profile'
    verbose_name = '用户信息'
    verbose_name_plural = verbose_name

  def __str__(self):
    # 定义模型对象的显示信息
    return self.username
```

2. 修改settings.py, 添加设置让[django](https://so.csdn.net/so/search?q=django&spm=1001.2101.3001.7020)用 我们web下的modles.py中的user模型

```python
# Custom user model
AUTH_USER_MODEL = 'web.UserProfileModel'
```

3. 迁移模型到数据库

```
$ python manage.py makemigrations
$ python manage.py migrate
```

4. 创建超级用户，登录后台

```
python manage.py createsuperuser
Username: 
Email address:
Password:
Password (again):
Superuser created successfully.
```

开启服务后进去一看，用户管理那一栏不见了

![在这里插入图片描述](https://qiniu.waite.wang/202406142217114.png)

5. 在 `web/admin/py` 中注册一下就好了

```python
from django.contrib import admin
from web.models import UserProfileModel

# Register your models here.
admin.site.register(UserProfileModel)
```

![image-20240614221851196](https://qiniu.waite.wang/202406142218031.png)

![image-20240614222130109](https://qiniu.waite.wang/202406142221851.png)