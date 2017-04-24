---
title: Django + Celery in windows
date: 2017-04-22 19:05:26
categories: [Python, Django]
tags: [Python, Django, Celery]
---

最新版的 Celery 不支援 Windows，網路上教學大部分都是Linux系統，想讓它動起來花了點心力，這邊紀錄一下過程。
<!--more-->
* [前言](#前言)
* [建置環境](#建置環境)
* [建立流程](#建立流程)
  * [安裝 Celery](#安裝Celery)
  * [安裝 ERLANG/OTP](#安裝ERLANG)
  * [安裝 RabbitMQ](#安裝RabbitMQ)
  * [Django project 文件設定](#Django)
  * [啟動 worker](#啟動)
  * [測試](#測試)
* [Reference](#Reference)

## 前言 <a href="#前言" id="前言">#</a>
下方內容如果有英文內容，那是從官方文件轉貼來，用來說明程式碼的。

## 建置環境 <a href="#建置環境" id="建置環境">#</a>
OS: Windows 8.1
[Django](https://www.djangoproject.com/): 1.8.17
[Celery](http://docs.celeryproject.org/en/3.1/): 3.1.25 (最後支援 windows 的版本)
[ERLANG/OTP](http://www.erlang.org/downloads): 19.3
[RabbitMQ](https://www.rabbitmq.com/): 3.6.9

## 建立流程 <a href="#建立流程" id="建立流程">#</a>
#### 安裝 Celery <a href="#安裝Celery" id="安裝Celery">#</a>
先安裝 Celery 3.1.25
```
pip install celery==3.1.25
```
#### 安裝 ERLANG/OTP <a href="#安裝ERLANG" id="安裝ERLANG">#</a>
在安裝 RabbitMQ 前要先去 [ERLANG/OTP](http://www.erlang.org/downloads) 官網下載來安裝。
我是下載 ``OTP 19.3 Windows 64-bit Binary File (103012097)``

#### 安裝 RabbitMQ <a href="#安裝RabbitMQ" id="安裝RabbitMQ">#</a>
到 [RabbitMQ](https://www.rabbitmq.com/) 官網下載來安裝即可。

#### Django project 文件設定 <a href="#Django" id="Django">#</a>
我們現在的 Django project 結構如下
```
proj               # Django project
├── manage.py
├── myapp          # demo app
└── proj
```
首先新增一個 celery.py
```
proj
├── manage.py
├── myapp
└── proj
    ├── __init__.py
    ├── settings.py
    ├── urls.py
    ├── views.py
    ├── wsgi.py
    └── celery.py   # 新增

```
#### celery.py
```
from __future__ import absolute_import
import os
from celery import Celery

# set the default Django settings module for the 'celery' program.
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'proj.settings')

from django.conf import settings  # noqa

app = Celery('proj')

# Using a string here means the worker will not have to
# pickle the object when using Windows.
app.config_from_object('django.conf:settings')
app.autodiscover_tasks(lambda: settings.INSTALLED_APPS)

@app.task(bind=True)
def debug_task(self):
    print('Request: {0!r}'.format(self.request))
```
然後在 ``proj/proj/__int__.py`` 添加程式碼。讓我們能使用這個 celery app。
```
proj
├── manage.py
├── myapp
└── proj
    ├── __init__.py   # 這邊
    ├── settings.py
    ├── urls.py
    ├── views.py
    ├── wsgi.py
    └── celery.py
```
#### \__int\__.py
```
from __future__ import absolute_import

# This will make sure the app is always imported when
# Django starts so that shared_task will use this app.
from .celery import app as celery_app  # noqa
```
<hr>
Note that this example project layout is suitable for larger projects, for simple projects you may use a single contained module that defines both the app and tasks, like in the [First Steps with Celery](http://docs.celeryproject.org/en/3.1/getting-started/first-steps-with-celery.html#tut-celery) tutorial.

Let's break down what happens in the first module, first we import absolute imports from the future, so that our celery.py module will not clash with the library:
```
from __future__ import absolute_import
```
Then we set the default ``DJANGO_SETTINGS_MODULE`` for the ``celery`` command-line program:
```
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'proj.settings')
```
Specifying the settings here means the celery command line program will know where your Django project is. This statement must always appear before the app instance is created, which is what we do next:
```
app = Celery('proj')
```
This is your instance of the library, you can have many instances but there’s probably no reason for that when using Django.

We also add the Django settings module as a configuration source for Celery. This means that you don’t have to use multiple configuration files, and instead configure Celery directly from the Django settings.

You can pass the object directly here, but using a string is better since then the worker doesn’t have to serialize the object when using Windows or execv:
````
app.config_from_object('django.conf:settings')
````
Next, a common practice for reusable apps is to define all tasks in a separate tasks.py module, and Celery does have a way to autodiscover these modules:
````
app.autodiscover_tasks(lambda: settings.INSTALLED_APPS)
````
<hr>
接下來在 ``proj/proj/settings.py`` 添加 Celery 設定。
```
proj
├── manage.py
├── myapp
└── proj
    ├── __init__.py
    ├── settings.py   # 這邊
    ├── urls.py
    ├── views.py
    ├── wsgi.py
    └── celery.py
```
#### settings.py
```
# Celery settings
# 如果rabbitmq運行在默認設置下，CELERY不需要其他信息，只要amqp://即可。
# BROKER_URL = 'amqp://guest:guest@localhost:5672//'
BROKER_URL = 'amqp://'
CELERY_RESULT_BACKEND = 'amqp://'
#: Only add pickle to this list if your broker is secured
#: from unwanted access (see userguide/security.html)
CELERY_ACCEPT_CONTENT = ['json']
CELERY_TASK_SERIALIZER = 'json'
CELERY_RESULT_SERIALIZER = 'json'
CELERY_TIMEZONE = 'Asia/Taipei'
CELERY_ENABLE_UTC = True
```
接下來到 ``proj/myapp`` 新增 task.py
```
proj
├── manage.py
├── myapp
|   ├── __init__.py
|   ├── admin.py
|   ├── models.py
|   ├── tests.py
|   ├── view.py
|   └── tasks.py       #新增
└── proj
```
#### tasks.py
```
from __future__ import absolute_import

from celery import shared_task

@shared_task
def add(x, y):
    return x + y

@shared_task
def mul(x, y):
    return x * y

@shared_task
def xsum(numbers):
    return sum(numbers)
```
#### 啟動 worker  <a href="#啟動" id="啟動">#</a>
在平時使用 ``python manage.py runserver`` 的目錄下輸入指令即可啟動 celery worker
```
celery -A pqrl worker -l info
```
<img src="https://raw.githubusercontent.com/zxc7415239/MarkdownPhotos/master/photos/run_celery.png" alt="啟動成功畫面" width="50%" height="50%" />
#### 測試 <a href="#測試" id="測試">#</a>
接下來測試是否能運行。再開啟一個 cmd。
```
python manage.py shell
```
進入 shell 後，輸入命令。
```
from myapp.tasks import add
add(5,5)
>> 10
results = add.delay(5,5)   # 加了.delay 後會發現 worker 開始運作
results.get()
>> 10
```
成功運行。
## Reference  <a href="#Reference" id="Reference">#</a>
[First steps with Django](http://docs.celeryproject.org/en/3.1/django/first-steps-with-django.html#using-celery-with-django)