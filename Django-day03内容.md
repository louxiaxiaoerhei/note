### 定义模型类

```python
from django.db import models

class BookInfo(models.Model):
    """模型类"""
    # 类属性名 = models.字段类型(选项参数)
   	
    class Meta:
        # db_table = '表名'
        
class HeroInfo(models.Model):
    # ...
    # 关联属性 = models.ForeignKey('关联模型类名称')
```

### 模型类生成表

1）生成迁移文件

​	python manage.py makemigrations

2）执行迁移生成数据库中的表

​	python manage.py migrate

### 数据库增\删\改

1）新增

```python
from datetime import date
book = BookInfo(btitle='西游记', bpub_date=date(1990, 1, 1))
book.save()

BookInfo.objects.create(btitle='西游记', bpub_date=date(1990, 1, 1))
```

2）删除

```python
book = BookInfo.objects.get(id=1)
book.delete()

BookInfo.objects.filter(id=1).delete()
```

3）更新

```python
hero = HeroInfo.objects.get(hname='猪八戒')
hero.hname = '沙悟能'
hero.save()

HeroInfo.objects.filter(hname='猪八戒').update(hname='沙悟能')
```

### 数据库查询

1）查询相关函数

| 函数名称  | 参数     | 作用                             | 返回值                               |
| --------- | -------- | -------------------------------- | ------------------------------------ |
| all       | 无       | 查询模型类对应表格中的所有数据   | QuerySet(查询集)                     |
| get       | 查询条件 | 查询满足条件一条且只能有一条数据 | 模型类对象，查不到会报错DoesNotExist |
| filter    | 查询条件 | 返回满足条件的所有数据           | QuerySet(查询集)                     |
| exclude   | 查询条件 | 返回不满足条件的所有数据         | QuerySet(查询集)                     |
| order_by  | 排序字段 | 对查询结果进行排序               | QuerySet(查询集)                     |
| aggregate | 聚合     | 查询时进行聚合操作               | 字典: {'属性名__聚合类小写':值}      |
| count     | 无       | 返回查询结果的数目               | 数字                                 |

2）查询条件

对于get, filter, exclude的参数都可以跟查询条件，格式如下:

> 属性名称__条件 = 值

3）F对象

作用: 用于查询时字段之间的比较。

```
from django.db.models import F
```

4）Q对象

作用: 用于查询时条件之间的逻辑关系。`&`(与) `|`(或) `~` (非)

> from django.db.models import Q

5）聚合操作

相关聚合类:

> from django.db.models import Sum, Avg, Min, Max, Count

6）排序

默认是升序，在排序字段前加`-`号表示降序排列。

### 关联查询(一对多)

1）查询和一个模型类对象关联的信息。

2）通过模型类实现关联查询

### 查询集

all, filter, exclude, order_by返回值都是QuerySet类的对象，也就是查询集。

1）惰性查询。

2）缓存。

查询集可以进行切片和取下标操作。

### Admin站点

1）语言时区本地化

2）创建管理员账户

> python manage.py createsuperuser

3）后台管理注册模型类

4）自定义后台管理页面