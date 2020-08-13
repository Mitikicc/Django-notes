# Django-notes
创建一个投票应用，用作学习。

投票应用由两部分组成：

- 一个让人们查看和投票的公共站点。
- 一个让你能添加、修改和删除投票的管理站点。

Django安装完成后，查看版本命令

```python
py -m django --version
3.0.7
```

## 创建项目

创建项目之前我们先要手动创建一个放所有文件的projects，我放在d盘目录下，名称为‘projects‘

然后进入该目录：

```python
C:\Users\XiaoChen>d:

d:\>cd projects

d:\projects>
```

执行以下命令，来创建一个包含这个项目所有配置的目录，目录名叫mysite

```python
d:\projects>django-admin startproject mysite
```

创建好后，mysite里面会出现六个py文件

```
mysite/
    mysite/
        __init__.py
        settings.py
        urls.py
        asgi.py
        wsgi.py
    manage.py
```



- `manage.py`：是让你用各种方式管理django的命令行工具
- `__init__.py`：一个空文件，告诉 Python 这个目录应该被认为是一个 Python 包。python包下面必须要有一个`__init__.py`。
- `settings.py`：django的项目配置文件，包含了非常重要的配置项。
- `urls.py`django 项目的 URL 声明，就像你网站的“目录”。里面存放了一张表，声明了前端发过来的各种http请求分别应该由哪些函数处理。
- `asgi.py`:作为项目运行在 ASGI 兼容的Web服务器上的入口。
- `wsgi.py`:作为项目运行在 WSGI 兼容的Web服务器上的入口。

## 运行服务器

```python
python manage.py runserver 0.0.0.0:80
```

其中 `0.0.0.0:80` 是指定 web服务绑定的 IP 地址和端口。

`0.0.0.0` 表示绑定本机所有的IP地址， 就是可以通过任何一个本机的IP (包括 环回地址 `127.0.0.1 `) 都可以访问我们的服务。

浏览器输入http://127.0.0.1/即可访问，访问成功会有小火箭页面。另外注意此时不能关闭cmd窗口，不然网站就停止运行了。

## 第一部分，创建投票应用

在 Django 中，每一个应用都是一个 Python 包，所谓python包就是包含了`__init__.py`的头文件，是一个文件夹，里面存放了很多py文件，也就是模块。

现在处于 `manage.py` 所在的目录下，然后运行这行命令来创建一个应用（polls就是投票的意思）:

```python
py manage.py startapp polls
```

下面是polls目录的全部内容：

```
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```

#### 编写第一个试图

所谓视图，我的理解就是`views.py`文件

打开 `polls/views.py`，输入下面这些代码进去：

```python
from django.http import HttpResponse


def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

如果想要在网页上看见效果，我们还需要用一个url进行映射

在 polls 目录里新建一个 `urls.py` 文件,polls目录变为：

```
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    urls.py
    views.py
```

在`polls/urls.py`中写入以下内容：

```python
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

下一步是要在根 URLconf 文件中指定我们创建的 `polls.urls` 模块。在 `mysite/urls.py` 文件的 `urlpatterns` 列表里插入一个 `include()`， 如下：

```python
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]
```

函数 `include()`允许引用其它 URLconfs。每当 Django 遇到 `include()`时，它会截断与此项匹配的 URL 的部分，并将剩余的字符串发送到其他 URLconf 以供进一步处理。意思就是分组的功能。

然后执行命令：

```
py manage.py runserver 0.0.0.0:80
```

输入网址：http://127.0.0.1/polls/  会返回我们在`polls/views.py`中定义的index中的内容。

如果输入的运行服务器命令是：

```
py manage.py runserver
```

那么就要输入网址：http://localhost:8000/polls/

## 第二部分，运行数据库

打开`mysite\settings.py`,查看`INSTALLED_APPS`设置项

 [`INSTALLED_APPS`](https://docs.djangoproject.com/zh-hans/3.0/ref/settings/#std:setting-INSTALLED_APPS) 默认包括了以下 Django 的自带应用：

- [`django.contrib.admin`](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/admin/#module-django.contrib.admin) -- 管理员站点。
- [`django.contrib.auth`](https://docs.djangoproject.com/zh-hans/3.0/topics/auth/#module-django.contrib.auth) -- 认证授权系统。
- [`django.contrib.contenttypes`](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/contenttypes/#module-django.contrib.contenttypes) -- 内容类型框架。
- [`django.contrib.sessions`](https://docs.djangoproject.com/zh-hans/3.0/topics/http/sessions/#module-django.contrib.sessions) -- 会话框架。
- [`django.contrib.messages`](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/messages/#module-django.contrib.messages) -- 消息框架。
- [`django.contrib.staticfiles`](https://docs.djangoproject.com/zh-hans/3.0/ref/contrib/staticfiles/#module-django.contrib.staticfiles) -- 管理静态文件的框架。



默认开启的某些应用需要至少一个数据表，所以，在使用他们之前需要在数据库中创建一些表。

```
python manage.py migrate
```

执行以后会创建数据库表，大小为128kb。

### 创建模型

```

```

