---
title: Django 编写你的第一个 Django 应用，第 1 部分
toc: true
date: 2018-06-22 22:00:01
---
---
author: evo
comments: true
date: 2018-05-31 03:19:09+00:00
layout: post
link: http://106.15.37.116/2018/05/31/django-%e7%bc%96%e5%86%99%e4%bd%a0%e7%9a%84%e7%ac%ac%e4%b8%80%e4%b8%aa-django-%e5%ba%94%e7%94%a8%ef%bc%8c%e7%ac%ac-1-%e9%83%a8%e5%88%86/
slug: django-%e7%bc%96%e5%86%99%e4%bd%a0%e7%9a%84%e7%ac%ac%e4%b8%80%e4%b8%aa-django-%e5%ba%94%e7%94%a8%ef%bc%8c%e7%ac%ac-1-%e9%83%a8%e5%88%86
title: Django 编写你的第一个 Django 应用，第 1 部分
wordpress_id: 7083
categories:
- 基础程序设计
tags:
- python
---

<!-- more -->

[mathjax]

**注：非原创，所有版权属于原作者，原文已列在 ORIGINAL 中。为了方便个人学习做了整合、修改，仅供个人学习使用。**


# ORIGINAL






  1.


[编写你的第一个 Django 应用](https://github.com/gatieme/CodingInterviews)  **这个非常好，就按这个来**







# TODO






  * aaa





* * *





# INTRODUCTION






  * aaa




# 前提知识






  * [CentOS 安装python 3](http://106.15.37.116/2018/05/30/linux-centos-%e5%ae%89%e8%a3%85python-3/)


  * [CentOS 安装Django](http://106.15.37.116/2018/05/31/linux-centos-%e5%ae%89%e8%a3%85django/)




# 目的


创建一个基本的投票应用程序。

它将由两部分组成：




  * 一个让人们查看和投票的公共站点。


  * 一个让你能添加、修改和删除投票的管理站点。


可以从哪里获得帮助？

如果你在阅读或实践本教程中遇到困难, 请发消息给 [django-users](file:///C:/Users/evo/Desktop/django-docs-2.0-zh-hans/internals/mailing-lists.html#django-users-mailing-list) 或加入 [#django on irc.freenode.net](irc://irc.freenode.net/django) 来和其他的 Django 用户交流，他们也许能帮到你。



创建项目
如果这是你第一次使用 Django 的话，你需要一些初始化设置。也就是说，你需要用一些自动生成的代码配置一个 Django project —— 即一个 Django 项目实例需要的设置项集合，包括数据库配置、Django 配置和应用程序配置。

打开命令行，cd 到一个你想放置你代码的目录，然后运行以下命令：

$ django-admin startproject mysite
这行代码将会在当前目录下创建一个 mysite 目录。如果命令失败了，查看 运行``django-admin``时遇到的问题，可能能给你提供帮助。

Note

你得避免使用 Python 或 Django 的内部保留字来命名你的项目。具体地说，你得避免使用像 django (会和 Django 自己产生冲突)或 test (会和 Python 的内置组件产生冲突)这样的名字。

我的代码该放在哪？

如果你曾经是原生 PHP 程序员（没有使用过现代框架），你可能会习惯于把代码放在 Web 服务器的文档根目录(诸如 /var/www)。当使用 Django 时不需要这样做。把所有 Python 代码放在 Web 服务器的根目录不是个好主意，因为这样会有风险。比如会提高人们在网站上看到你的代码的可能性。这不利于网站的安全。

把你的代码放在文档根目录 以外 的某些地方吧，比如 /home/mycode。

让我们看看 startproject 创建了些什么:

mysite/
manage.py
mysite/
__init__.py
settings.py
urls.py
wsgi.py
这些目录和文件的用处是：

最外层的:file: mysite/ 根目录只是你项目的容器， Django 不关心它的名字，你可以将它重命名为任何你喜欢的名字。
manage.py: 一个让你用各种方式管理 Django 项目的命令行工具。你可以阅读 django-admin and manage.py 获取所有 manage.py 的细节。
里面一层的 mysite/ 目录包含你的项目，它是一个纯 Python 包。它的名字就是当你引用它内部任何东西时需要用到的 Python 包名。 (比如 mysite.urls).
mysite/__init__.py：一个空文件，告诉 Python 这个目录应该被认为是一个 Python 包。如果你是 Python 初学者，阅读官方文档中的 更多关于包的知识。
mysite/settings.py：Django 项目的配置文件。如果你想知道这个文件是如何工作的，请查看 Django settings 了解细节。
mysite/urls.py：Django 项目的 URL 声明，就像你网站的“目录”。阅读 URL调度器 文档来获取更多关于 URL 的内容。
mysite/wsgi.py：作为你的项目的运行在 WSGI 兼容的Web服务器上的入口。阅读 如何使用 WSGI 进行部署 了解更多细节。
用于开发的简易服务器
让我们来确认一下你的 Django 项目是否真的创建成功了。如果你的当前目录不是外层的 mysite 目录的话，请切换到此目录，然后运行下面的命令：

$ python manage.py runserver
你应该会看到如下输出：

Performing system checks...

System check identified no issues (0 silenced).

You have unapplied migrations; your app may not work properly until they are applied.
Run 'python manage.py migrate' to apply them.

五月 27, 2018 - 15:50:53
Django version 2.0, using settings 'mysite.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
Note

忽略有关未应用最新数据库迁移的警告，稍后我们处理数据库。

你刚刚启动的是 Django 自带的用于开发的简易服务器，它是一个用纯 Python 写的轻量级的 Web 服务器。我们将这个服务器内置在 Django 中是为了让你能快速的开发出想要的东西，因为你不需要进行配置生产级别的服务器（比如 Apache）方面的工作，除非你已经准备好投入生产环境了。

现在是个提醒你的好时机：千万不要 将这个服务器用于和生产环境相关的任何地方。这个服务器只是为了开发而设计的。(我们在 Web 框架方面是专家，在 Web 服务器方面并不是。)

现在，服务器正在运行，浏览器访问 https://127.0.0.1:8000/。你将会看到一个“祝贺”页面，随着一只火箭发射，服务器已经运行了。

更换端口

默认情况下，runserver 命令会将服务器设置为监听本机内部 IP 的 8000 端口。

如果你想更换服务器的监听端口，请使用命令行参数。举个例子，下面的命令会使服务器监听 8080 端口：

$ python manage.py runserver 8080
如果你想要修改服务器监听的IP，在端口之前输入新的。比如，为了监听所有服务器的公开IP（这你运行 Vagrant 或想要向网络上的其它电脑展示你的成果时很有用），使用：

$ python manage.py runserver 0:8000
0 是 0.0.0.0 的简写。完整的关于开发服务器的文档可以在 :djamdin:`runserver` 参考文档中找到。

会自动重新加载的服务器 runserver

用于开发的服务器在需要的情况下会对每一次的访问请求重新载入一遍 Python 代码。所以你不需要为了让修改的代码生效而频繁的重新启动服务器。然而，一些动作，比如添加新文件，将不会触发自动重新加载，这时你得自己手动重启服务器。

创建投票应用
现在你的开发环境——这个“项目” ——已经配置好了，你可以开始干活了。

在 Django 中，每一个应用都是一个 Python 包，并且遵循着相同的约定。Django 自带一个工具，可以帮你生成应用的基础目录结构，这样你就能专心写代码，而不是创建目录了。

项目 VS 应用

项目和应用有啥区别？应用是一个专门做某件事的网络应用程序——比如博客系统，或者公共记录的数据库，或者简单的投票程序。项目则是一个网站使用的配置和应用的集合。项目可以包含很多个应用。应用可以被很多个项目使用。

你的应用可以存放在任何 Python path 中定义的路径。在这个教程中，我们将在你的 manage.py 同级目录下创建投票应用。这样它就可以作为顶级模块导入，而不是 mysite 的子模块。

请确定你现在处于 manage.py 所在的目录下，然后运行这行命令来创建一个应用：

$ python manage.py startapp polls
这将会创建一个 polls 目录，它的目录结构大致如下：

polls/
__init__.py
admin.py
apps.py
migrations/
__init__.py
models.py
tests.py
views.py
这个目录结构包括了投票应用的全部内容。

编写第一个视图
让我们开始编写第一个视图吧。打开 polls/views.py，把下面这些 Python 代码输入进去：

polls/views.py
from django.http import HttpResponse

def index(request):
return HttpResponse("Hello, world. You're at the polls index.")
这是 Django 中最简单的视图。如果想看见效果，我们需要将一个 URL 映射到它——这就是我们需要 URLconf 的原因了。

为了创建 URLconf，请在 polls 目录里新建一个 urls.py 文件。你的应用目录现在看起来应该是这样：

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
在 polls/urls.py 中，输入如下代码：

polls/urls.py
from django.urls import path

from . import views

urlpatterns = [
path('', views.index, name='index'),
]
下一步是要在根 URLconf 文件中指定我们创建的 polls.urls 模块。在 mysite/urls.py 文件的 urlpatterns 列表里插入一个 include()， 如下：

mysite/urls.py
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
path('polls/', include('polls.urls')),
path('admin/', admin.site.urls),
]
函数 include() 允许引用其它 URLconfs。每当 Django 遇到 :func：~django.urls.include 时，它会截断与此项匹配的 URL 的部分，并将剩余的字符串发送到 URLconf 以供进一步处理。

我们设计 include() 的理念是使其可以即插即用。因为投票应用有它自己的 URLconf( polls/urls.py )，他们能够被放在 "/polls/" ， "/fun_polls/" ，"/content/polls/"，或者其他任何路径下，这个应用都能够正常工作。

何时使用 include()

当包括其它 URL 模式时你应该总是使用 include() ， admin.site.urls 是唯一例外。

你现在把 index 视图添加进了 URLconf。可以验证是否正常工作，运行下面的命令:

$ python manage.py runserver
用你的浏览器访问 http://localhost:8000/polls/，你应该能够看见 "Hello, world. You're at the polls index." ，这是你在 index 视图中定义的。

函数 path() 具有四个参数，两个必须参数：route 和 view，两个可选参数：kwargs 和 name。现在，是时候来研究这些参数的含义了。

path() 参数： route
route 是一个匹配 URL 的准则（类似正则表达式）。当 Django 响应一个请求时，它会从 urlpatterns 的第一项开始，按顺序依次匹配列表中的项，直到找到匹配的项。

这些准则不会匹配 GET 和 POST 参数或域名。例如，URLconf 在处理请求 https://www.example.com/myapp/ 时，它会尝试匹配 myapp/ 。处理请求 https://www.example.com/myapp/?page=3 时，也只会尝试匹配 myapp/。

path() 参数： view
当 Django 找到了一个匹配的准则，就会调用这个特定的视图函数，并传入一个 HttpRequest 对象作为第一个参数，被“捕获”的参数以关键字参数的形式传入。稍后，我们会给出一个例子。

path() 参数： kwargs
任意个关键字参数可以作为一个字典传递给目标视图函数。本教程中不会使用这一特性。

path() 参数： name
为你的 URL 取名能使你在 Django 的任意地方唯一地引用它，尤其是在模板中。这个有用的特性允许你只改一个文件就能全局地修改某个 URL 模式。

当你了解了基本的请求和响应流程后，请阅读 教程的第 2 部分 开始使用数据库.












* * *





# COMMENT
