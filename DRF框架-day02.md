### DRF框架

##### 1. 序列化器-序列化功能

1）序列化单个对象

```python
# 获取对象
book = BookInfo.objects.get(id=1)

# 创建序列化器对象
serializer = BookInfoSerializer(book)

# 获取序列化之后的数据
serializer.data
```

2）序列化多个对象

```python
# 获取所有的图书
books = BookInfo.objects.all()

# 创建序列化器对象
serializer = BookInfoSerializer(books, many=True)

# 获取序列化之后的数据
serializer.data
```

3）关联对象的嵌套序列化

```python
1. 将关联对象序列化为关联对象的主键 PrimaryKeyRelatedField
hbook = serializers.PrimaryKeyRelatedField(label='图书', read_only=True)

2. 使用指定的序列化器将关联对象进行序列化
hbook = BookInfoSerializer(label='图书')

3. 将关联对象序列化为关联对象模型类__str__方法返回值 StringRelatedField
hbook = serializers.StringRelatedField(label='图书')

# 注意: 如果关联的对象有多个，在定义字段的时候，需要添加`many=True`
```

##### 2. 序列化器-反序列化功能

1）反序列化-数据校验

```python
data = {'btitle': 'python', 'bpub_date': '1991-1-1'}

# 创建序列化器对象
serializer = BookInfoSerializer(data=data)
# 调用`is_valid`进行数据校验
serilaizer.is_valid() # 校验通过返回True，否则返回False
# 获取校验失败的错误信息
serializer.errors
# 获取校验之后的数据
serializer.validated_data

# 补充序列化器验证
# 1. 自定义验证方法，给序列化器对应的字段增加选项参数`validators`
# 2. 直接在序列化器类中定义验证方法针对特定的字段进行验证，方法名称: validate_<fieldname>
# 3. 在序列化器类中定义validate方法进行验证：结合多个字段的内容进行验证
```

2）反序列化-数据保存

数据保存之后必须通过校验。

`数据新增`:

```python
data = {'btitle': 'python', 'bpub_date': '1991-1-1'}
# 创建序列化器对象
serializer = BookInfoSerializer(data=data)
# 调用`is_valid`进行数据校验
serilaizer.is_valid() # 校验通过返回True，否则返回False

# 数据保存，调用序列化器类中create，可以实现create方法实现数据新增
serializer.save()
```

`数据更新`:

```python
book = BookInfo.objects.get(id=9)
data = {'btitle': 'python-2', 'bpub_date': '1991-1-1'}
# 创建序列化器对象
serializer = BookInfoSerializer(book，data=data)
# 调用`is_valid`进行数据校验
serilaizer.is_valid() # 校验通过返回True，否则返回False

# 数据保存，调用序列化器类中update，可以实现update方法实现数据更新
serializer.save()
```

##### 3. ModelSerializer

1）基于模型类的字段自动生成序列化器类的字段。

2）实现create和update方法。

##### 4. APIView视图类

**request对象**: 不再是Django原始的HttpRequest对象，而是由DRF框架封装成了Request类的对象。

​	request.data: 包含解析之后的请求体的数据，并且已经转换成字典或类字典(QueryDict, OrderedDict)
                相当于Django原始request对象中(request.POST, request.FILES, request.body)
    	request.query_params: 包含查询字符串的数据，相当于Django原始request对象中request.GET

**Response对象**:

​	直接返回Respone对象，传入原始的响应数据，会根据客户端的请求头`Accpet`把响应数据转换为对应的格式进行返回，默认返回json，仅支持json或html

**异常处理**: 将某些异常自动处理，给客户端返回对应的错误信息

其他功能：

1）认证

2）权限

3）限流
