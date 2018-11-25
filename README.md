# py_django
Django Notes.

## 创建项目
打开命令行，cd 到一个你想放置你代码的目录，然后运行以下命令
```dos
$  django-admin startproject py_django
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
实际项目中，有可能使用其他的数据库，因此以下以 mysql 为例，来配置第三方的数据库连接

### 创建数据库
1.进入 mysql 的命令行界面，输入数据库的密码
```sql
$ mysql -u root -p
```

2.创建数据库，记得保持这里的数据库名称和 settings 文件 database 中设置的 name 一致即可
```sql
mysql> create database if not exists py_django_1 default charset utf8;
```

3.退出命令行界面
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
