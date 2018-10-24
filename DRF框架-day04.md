### DRF框架

##### 其他功能

1）认证&权限

2）限流

控制用户访问API接口的频率。

针对匿名用户和认证用户分别进行限流。

```python
# 限流(针对匿名用户和认证用户分别进行限流控制)
'DEFAULT_THROTTLE_CLASSES': (
    'rest_framework.throttling.AnonRateThrottle', # 针对匿名用户
    'rest_framework.throttling.UserRateThrottle' # 针对认证用户
),

# 限流频次设置
'DEFAULT_THROTTLE_RATES': {
    'user': '5/minute', # 认证用户5次每分钟
	'anon': '3/minute', # 匿名用户3次每分钟
},
```

针对匿名用户和认证用户统一进行限流。

```python
# 限流(针对匿名用户和认证用户进行统一限流控制)
'DEFAULT_THROTTLE_CLASSES': (
	'rest_framework.throttling.ScopedRateThrottle',
),

'DEFAULT_THROTTLE_RATES': {
    'contacts': '5/minute', 
    'upload': '3/minute',
},
```

3）过滤&排序

4）分页

两种分页方式**PageNumberPagination**和**LimitOffsetPagination**。

使用**PageNumberPagination**分页时，获取分页数据时可以通过page传递页码参数。如果想要分页时指定页容量，需要自定义分页类。

```python
class StandardResultPagination(PageNumberPagination):
	# 默认页容量
	page_size = 3
	# 指定页容量参数名称
	page_size_query_param = 'page_size'
	# 最大页容量
	max_page_size = 5
```

使用**LimitOffsetPagination**分页时，获取分页数据时可以传递参数offset(偏移量)和limit(限制条数)。

注：如果使用的全局分页设置，某个列表视图如果不需要分页，直接在视图类中设置`pagination_class = None`。

5）异常

DRF自带异常处理功能，可以对某些特定的异常进行处理并返回给客户端组织好的错误信息。能够处理的异常如下:

```http
APIException 所有异常的父类
ParseError 解析错误
AuthenticationFailed 认证失败
NotAuthenticated 尚未认证
PermissionDenied 权限决绝
NotFound 未找到
MethodNotAllowed 请求方式不支持
NotAcceptable 要获取的数据格式不支持
Throttled 超过限流次数
ValidationError 校验失败
```

可以自定义DRF框架的异常处理函数(补充一些异常处理)并指定`EXCEPTION_HANDLER`配置项。

6）接口文档