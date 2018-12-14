# py_django
Django Notes.

## 创建项目
打开命令行，cd 到一个你想放置你代码的目录，然后运行以下命令
```dos
$  django-admin startproject py_django
```

测试项目是否创建成功的命令
```dos
$  python3 manage.py runserver
```

看到以下命令则说明项目创建成功
```dos
Performing system checks...

System check identified no issues (0 silenced).

You have 15 unapplied migration(s). Your project may not work properly until you apply the migrations for app(s): admin, auth, contenttypes, sessions.
Run 'python manage.py migrate' to apply them.

November 25, 2018 - 07:27:28
Django version 2.1.3, using settings 'py_django.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```
## 替换数据库
> 实际项目中，有可能使用其他的数据库，因此以下以 mysql 为例，来配置第三方的数据库连接

### 安装 `mysql` 数据库
使用 `homebrew` 来安装 `mysql` 数据库
```cmd
# 安装
brew install mysql

# 启动mysql服务
mysql.server start

# 停止mysql服务
mysql.server stop

# 初始化mysql配置
mysql_secure_installation
```

### 创建数据库
1. 进入 mysql 的命令行界面，输入数据库的密码
```sql
$ mysql -u root -p
```

2. 创建数据库，记得保持这里的数据库名称和 settings 文件 database 中设置的 name 一致即可
```sql
mysql> create database if not exists py_django_1 default charset utf8;
```

3. 退出命令行界面
```sql
mysql> quit
```

### 修改项目的配置文件
修改 py_django 项目下的 settings 文件如下，具体内容以实际情况填写

```python
DATABASES = {
    'default': {
        # 'ENGINE': 'django.db.backends.sqlite3',
        # 'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),

        # 使用本地的 mysql 数据库代替
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'py_django_1',
        'USER': 'root',
        'PASSWORD': 'root',
        'HOST': 'localhost',
        'PORT': '3306',
        'CHARSET': 'utf8',
    }
}
```

## 创建应用
> 项目和应用有啥区别？应用是一个专门做某件事的网络应用程序——比如博客系统，或者公共记录的数据库，或者简单的投票程序。项目则是一个网站使用的配置和应用的集合。项目可以包含很多个应用。应用可以被很多个项目使用。

1. 使用以下命令来创建应用(AppName是你要创建的应用名称)
```dos
$ python3 manage.py startapp AppName
```

2. 在新建的应用的 views.py 文件,并添加以下代码
```python
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello, world. You're at the AppName index.")
```

3. 在新建的应用中添加 urls.py 文件,并添加以下代码
```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

4. 在根 URLconf 文件中指定我们创建的 AppName.urls 模块，在 py_django/urls.py 文件的 urlpatterns 列表里插入一个 include()
```python
from django.contrib import admin
from django.urls import path,include

urlpatterns = [
    path('admin/', admin.site.urls),
    #  这里是新的应用 url 地址文件
    path('AppName/',include('AppName.urls')),
]
```

5. 激活模型
>为了在我们的工程中包含这个应用，我们需要在配置类 INSTALLED_APPS 中添加设置。因为 AppNameConfig 类写在文件 AppName/apps.py 中，所以它的点式路径是 'AppName.apps.AppNameConfig'。在文件 py_django/settings.py 中 INSTALLED_APPS 子项添加点式路径后，它看起来像这样：
```python
INSTALLED_APPS = [
    # 这一行是添加的
    'AppName.apps.AppNameConfig',

    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

6. 检测模型修改，生成数据库表
```dos
$ python3 manage.py makemigrations AppName
$ python3 manage.py migrate
```

7. 检测新应用是否引入成功
```
访问 http://127.0.0.1:8000/admin/ 进入登录界面
访问 http://127.0.0.1:8000/model_layer/ 可正常显示文字“Hello, world. You're at the AppName index.”
```
