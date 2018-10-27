### 美多商城项目

##### 1. 用户信息的存储

```
用户名
密码
手机号
邮箱
是否管理员
是否注销
```

##### 2. 用户注册业务逻辑分析

`子业务`:

1）短信验证码获取

2）用户名是否重复

3）手机号是否重复

4）注册用户信息的保存

`接口设计`:

```bash
1. 接口请求方式
2. 接口url路径定义
3. 接口所需客户端传递的参数和格式
4. 接口响应数据和格式
```

##### 3. 短信验证码接口设计

获取短信验证码

```http
API: GET /sms_codes/(?P<mobile>1[3-9]\d{9})/
参数:
	通过url地址传递手机号
响应:
	{
        "message": "OK"
	}
```

##### 4. 前后端服务器域名设置

live-server(静态文件服务器)：127.0.0.1:8080  -> `www.meiduo.site`

后端API服务器：127.0.0.1:8000 -> `api.meiduo.site`

​	域名对应IP，浏览器通过域名访问网站时，会先到本地`/etc/hosts`查找域名和IP之间对应关系，如果找到直接访问对应IP，如果找不到再进行DNS解析。

##### 5. 跨域请求

同源策略: 对于两个url地址，如果`协议`,`ip`和`port`完全一致，那么这两个地址时同源的。

浏览器在发起ajax请求时，如果当前页面的地址和被请求的地址不是同源，那么这个请求就属于跨域请求。

`http://www.meiduo.site:8080/`

`http://api.meiduo.site:8000/`

浏览器在发起ajax跨域请求时，会在请求头中添加请求头`Origin`

> Origin: 源请求地址

服务器在收到请求时，如果允许源请求地址进行跨域请求，需要在响应头中添加响应头`Access-Control-Allow-Origin`

> Access-Control-Allow-Origin: 源请求地址

浏览器在收到服务器响应时，就会查看响应头中是否有`Access-Control-Allow-Origin: 源请求地址`，如果没有，就认为被请求服务器不允许源地址进行跨域请求，直接将请求驳回。

##### 6. celery异步任务队列

本质: 通过提前创建的进程调用函数实现异步任务。

概念:

​	任务执行者(worker): 创建好的进程。

​	任务发出者: 发出任务消息，任务就是所要调用函数。

​	中间人(broker): 保存任务消息

使用:

1）安装: `pip install celery`

2）创建Celery类实例对象并进行相关设置

```python
# main.py
from celery import Celery

# 创建Celery类实例对象
celery_app = Celery('demo')

# 加载配置
celery_app.config_from_object('celery配置文件包路径')

# config.py
# 设置中间人地址
broker_url = 'redis://172.16.179.139:6379/3'
```

3）封装任务函数

```python
@celery_app.task(name='send_sms_code')
def send_sms_code(a, b):
	# 封装的函数的代码...
```

4）创建celery worker并启动

```bash
celery -A 'celery_app对象所在文件包路径' worker -l info
```

5）发出任务消息

```python
send_sms_code.delay(1, 3)
```









































