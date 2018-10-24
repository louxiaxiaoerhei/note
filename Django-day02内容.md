## Django-day02内容

### 状态保持-cookie

```python
# 设置：
response.set_cookie('key', 'value', max_age='过期时间: s')

# 获取：
request.COOKIES.get('key')

# 删除
response.delete_cookie('key')
```

### 状态保持-session

```python
# 设置:
	request.session['key'] = 'value'
# 获取:
	request.session.get('key')
    
# 设置将session存储在redis中
a）安装
	pip install django-redis
    
b）配置
CACHES = {
    "default": {
        "BACKEND": "django_redis.cache.RedisCache",
        "LOCATION": "redis://127.0.0.1:6379/1",
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
        }
    }
}
SESSION_ENGINE = "django.contrib.sessions.backends.cache"
SESSION_CACHE_ALIAS = "default"
```

### 类视图

采用不同的请求方式访问同一个url地址时，会调用对应类视图中不同的方法。

1）在views.py中定义类视图。

```python
# views.py
from django.views.generic import View

# /register/
class RegisterView(View):
    def get(self, request):
        return 'get page'
    
    def post(self, request):
        return 'post page'
```

2）在urls.py中进行url配置。

```python
from django.conf.urls import url
from . import views

urlpatterns = [
    url(r'^register/$', views.RegisterView.as_view()),
]
```

### 类视图的原理

​	dispatch方法在进行路由分发，会根据`请求方式`获取类视图中对应的方法并进行调用。

### 给类视图添加装饰器

1）直接在url配置时进行添加。

2）使用**method_decorator**添加装饰器。

### 扩展类

把一些通用的功能抽取到扩展类中，其他类如果需要这个功能，继承即可。

### 中间件

相当于Flask框架中的钩子函数，可以根据需要在请求前后进行一些处理。

```python
def simple_middleware(get_response):
    # 此处编写的代码仅在Django第一次配置和初始化的时候执行一次。

    def middleware(request):
        # 此处编写的代码会在每个请求处理视图前被调用。

        response = get_response(request)

        # 此处编写的代码会在每个请求处理视图之后被调用。

        return response

    return middleware

# 注：自己写的中间件需要在`settings.py`中的`MIDDLEWARE`配置项中进行注册。
```

### 模板

1）使用

```python
from django.shortcuts import render 
render(request, '模板文件', '传递给模板的数据')

# 详细步骤:
# 1. 加载模板文件(指定使用的模板文件是哪个)
from django.template import loader
temp = loader.get_template('模板文件')
# 2. 模板渲染(给模板文件传递数据，替换模板文件中的变量并返回替换后的内容)
res_html = temp.render('传递给模板的数据)
```

2）模板变量

> a）Django使用模板变量时，无论是字典、列表或元组的元素，都需要使用`.`，不能使用`[]`，这是和Flask有区别的地方。
>
> b）Django中的模板变量不能直接进行算术运算。

3）模板控制语句

> a）Django模板中在进行条件判断时，比较操作符的两边必须有空格。
>
> b）Django模板中的for循环和Jinja2模板中for循环对比。
>
> ```jinja2
> # Jinja2模板中for循环
> {% for ... in ... %}
> 	# 遍历不为空时的逻辑
> 	# 获取for循环遍历到了第几次
> 	{{ loop.index }}
> {% else %}
> 	# 遍历为空时的逻辑
> {% endfor %}
> 
> # Django模板中for循环
> {% for ... in ... %}
> 	# 遍历不为空时的逻辑
> 	# 获取for循环遍历到了第几次
> 	{{ forloop.couter }}
> {% empty %}
> 	# 遍历为空时的逻辑
> {% endfor %}
> ```

4）模板过滤器

> a）Jinja2模板过滤器使用:
>
> {{ 模板变量|过滤器(参数...)}}
>
> b）Django中模板过滤器使用:
>
> {{ 模板变量|过滤器:参数 }}
>
> 注: Django中过滤器`:`号之后只能接收一个参数。

## 数据库配置

```python
DATABASES = {
    'default': {
        # 'ENGINE': 'django.db.backends.sqlite3',
        # 'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),

        'ENGINE': 'django.db.backends.mysql',
        'HOST': '172.16.179.139',  # 数据库主机
        'PORT': 3306,  # 数据库端口
        'USER': 'root',  # 数据库用户名
        'PASSWORD': 'mysql',  # 数据库用户密码
        'NAME': 'gz02_django_bak'  # 数据库名字
    }
}
```
