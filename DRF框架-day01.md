### DRF框架

##### 1. 授课思路

1）web开发的两种模式(前后端不分离&前后端分离)

2）RestAPI设计风格

3）使用Django基础知识自定义一套RestAPI接口

4）分析定义RestAPI接口开发时的主要工作

5）DRF框架的特点及使用。

##### 2. web开发的两种方式

| 开发方式     | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| 前后端不分离 | 前端看到的页面效果是由后端进行控制的，后端进行模板渲染，返回渲染之后完整页面。 |
| 前后端分离   | 后端只返回前端所需要的数据，至于数据在前端怎么展示，由前端自己控制。 |

##### 3. Restful API设计风格

```
GET /books/: 返回所有图书的信息
GET /books/1/: 返回指定的图书的信息
POST /books/: 返回创建的图书信息
PUT /books/1/: 返回修改的图书信息
DELETE /books/1/: 空文档
```

RestfulAPI 风格关键点:

1）设计url地址时，地址尽量使用名词，不要使用动词。

2）执行不同的操作，要采用不同的请求方式。

```
GET 获取
POST 新建
PUT 修改
DELETE 删除
```

3）过滤参数放在查询字符串中。

4）响应状态码的选择

```
200 获取或修改
201 新建
204 删除
400 客户端请求有误
404 请求的资源找不到
500 服务器错误
```

5）响应数据返回json.

##### 4. 使用Django基础知识自定义RestAPI接口

```
需求:
1. 获取所有图书信息	GET /books/
2. 新增一本图书信息 POST /books/
3. 获取指定的图书的信息(根据id)	GET /books/<图书id>/
4. 修改指定的图书的信息(根据id) PUT /books/<图书id>/
5. 删除指定的图书的信息(根据id) DELETE /books/<图书id>/
```

##### 5. 序列化&反序列化

序列化: 在上面例子中，把模型对象转换为python字典或json数据过程，就叫做序列化过程。

反序列化: 在上面例子中，把字典数据和json数据转换保存到模型对象中过程，就叫做反序列化过程。

##### 6. 序列化器

1）定义序列化器类

```python
from rest_framework import serializers

# serializers.Serializer: DRF框架中所有序列化器类父类
# serializers.ModelSerializer: Serializer类的子类，在Serializer基础上增加一些功能

class User(object):
    """用户类"""
    def __init__(self, username, age):
        self.username = username
        self.age = age
        
class UserSerializer(serializers.Serializer):
    """用户序列化器类"""
    # 序化器字段名 = serializers.字段类型(选项参数)
    username = serializers.CharField()
    age = serializers.IntegerField()
        
if __name__ == '__main__':
    # 创建user对象
    user = User('smart', 18)
    
    {
        'username': 'smart',
        'age': 18
    }
    
    # 创建一个序列化器类对象
    serializer = UserSerializer(user)
    # 获取序列化之后数据
    print(serializer.data)
```
