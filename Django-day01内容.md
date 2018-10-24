##### 客户端给服务器传递参数的方式

1.  从url地址中提取参数

2. 通过查询字符串传递参数

3. 通过请求体传递数据：post表单类型，json数据...

4. 通过请求头传递数据

   ```js
   $.ajax({
       type : 'post',
       headers: {
           'X-CSRFToken': 'csrf_token数据'
       }，
       ...
   })
   ```

##### 从url地址中提取参数

```python
@app.route('/users/<int:id>')
def users(id):
	...
```

##### 从查询字符串中获取参数

```python
from flask import request

request.args: 获取查询字符串参数
```

##### 获取表单类型数据

```html
<form method='get' action='/handle/'>
    <input type='text' name='username' />
    <input type='text' name='age' />
    <input type='submit' value='提交' />
</form>

# 注意: 在页面使用表单时，如果表单method设置`get`，提交的时候不是表单数据，而是查询字符串
```

##### 获取json数据

```python
request.json: 获取请求体中的json数据并且宝json数据转换为dict

request.data:
```

##### 获取请求头数据

```
request.headers: 获取请求头中数据
```



##### 响应时返回json数据

```python
from flask import jsonify

return jsonify(字典数据)
```

##### 响应时进行页面重定向

```python
from flask import redirect
from flask import url_for

return redirect('url地址')

return redirect(url_for('视图名称'))
```

