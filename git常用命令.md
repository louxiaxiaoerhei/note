# git常用命令

### 安装

```shell
# Linux安装git
sudo apt-get install git
# windows安装git
https://www.git-scm.com/download/
```

[windows下载git](https://www.git-scm.com/download/)

### 仓库管理常用命令

```shell
# 克隆仓库
git clone git地址

# 创建项目分支
git branch 分支名称
# 例：
git branch dev
# 切换分支
git checkout 分支名称
# 例：
git checkout dev
# 将分支推送到服务器
git push origin 分支名称
# 例：
git push origin dev
# 将本地分支跟踪服务器分支
git branch --set-upstream-to=origin/分支名称 分支名称
# 例：
git branch --set-upstream-to=origin/dev dev
# 创建并切换分支
git checkout -b 分支名称
# 例：
git checkout -b itcast
# 查看所有分支，当前分支前标记为星*
git branch
# 删除分支
git branch -d 分支名称
# 推送dev分支
git push origin dev
# 将dev分支合并到master分支
git checkout master
git merge dev

```

### 开发常用命令

```shell
# 删除~/.ssh目录，这里存储了旧的密钥
rm -r .ssh
# 运行如下命令生成密钥
# 在1处可以填写保存密钥的目录
# 在2处可以填写密码，如果填写，一般为项目的名称，后续操作时会要求填写此密码
# 公钥名称为id_rsa.pub
# 私钥名称为id_rsa
ssh-keygen -t rsa -C "Github账号，可以是用户名，也可以是邮箱地址"
#查看公钥内容，复制此内容
cat id_rsa.pub
```



#### 同步分支

```shell
# 以自己的姓名创建分支，如果此分支已经存在可以添加数字后缀，具体要与项目经理商量
git checkout -b zhujiao
# 将本地分支推送到服务器
git push origin zhujiao
# 将本地分支跟踪服务器分支
git branch --set-upstream-to=origin/分支名称 分支名称
# 例：
git branch --set-upstream-to=origin/zhujiao zhujiao
# 将github上的dev分支同步到本地，因为开发过程中，所有组员都向这个分支上提交阶段性代码，并从这个分支获取最新代码
git checkout -b dev origin/dev
```

#### 开发管理

```shell
#上面的操作，只有我们在加入项目的第一天需要进行，只操作一次就够了
# 接下来的操作，是我们每天开发中都要进行的操作，这是必须做到熟练操作的命令
# 当前用户以zhujiao分支进行开发
git checkout zhujiao

# 按照工作分配，需要创建df_user模块，此时文件位于工作区
python manage.py startapp df_user
# 将目录df_user及所有子目录和文件添加到暂存区
git add 文件1 文件2 ...
git add 目录
# 例：
git add df_user/
# 使用暂时区的内容恢复工作区的内容，放弃工作区的更改
# 在ide中编辑df_user/models.py文件，删除掉str方法
#此时无str方法的类在工作区，暂存区中的类是有str方法的，如果想回到暂存区的状态，则
git checkout -- 文件名
# 例：
git checkout -- df_user/models.py
# 查看暂存区未提交的记录
git status
# 将暂存区的记录提交到仓库区
git commit -m '本次提交的说明信息'
# 例：
git commit -m '创建df_user模块'

# 推送指将特定分支在本地仓库区的记录发送到服务器上
# 获取指将服务器特定分支向本地工作区同步
# 建议：在每天开始编写代码前，先与服务器同步一次；或者在公用分支如dev上开发时，建议先同步后开发
# 什么时候会用到dev分支呢？答：合并阶段代码到dev分支，编辑公用文件如dailyfresh/urls.py
# 1.切换到dev分支
git checkout dev
# 2.获取代码，如果dev分支上有更新的记录则会同步到本地
git pull
# 3.切换回自己的分支继续开发
git checkout zhujiao
# 建议：在每天下班前将当天开发推送到服务器，这样可以在服务器中存储一个备份，即使本机出问题，在服务器上还能存在代码备份
# 注意：只会将仓库区的记录提交到服务器的对应分支下
# 推送前要将此分支跟踪服务器上的同名分支，推荐在创建分支时就完成跟踪
# 如果要推送自己分支以外的分支，需要先获取，再解决冲突，然后再推送
git push origin zhujiao
# 合并分支
# 一个功能模块开发完了，合并到dev分支
# 1.切换到dev分支
git checkout dev
# 2.获取代码，如果dev分支上有更新的记录则会同步到本地
git pull
# 3.合并
git merge zhujiao
# 4.添加、提交并推送
git push origin dev
# 5.切换回工作分支
git checkout zhujiao
# 6.在最新代码上继续开发，所以将dev分支合并到zhujiao分支
git merge dev

```

#### 解决冲突

- 建议：在更改公用文件如dailyfresh/urls.py时需要操作dev分支，因为大家都可以操作dev分支，所以在合并时可能出现冲突
- 冲突的示例如下，修改dailyfresh/urls.py文件

###### 项目经理的操作

- 1.项目经理负责前台的开发，需要修改dailyfresh/urls.py文件

```shell
git checkout dev
```

- 2.在dailyfresh/urls.py文件中添加一条url

```python
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', include(admin.site.urls)),
    url(r'^',include('df_goods.urls')),
]
```

- 3.添加并提交

```shell
git add dailyfresh/urls.py
git commit -m '配置前台url'
```

- 4.同步到服务器

```shell
git push origin dev
```

###### 员工的操作

- 1.员工助教负责用户模块的开发，需要修改dailyfresh/urls.py文件

```
git checkout dev
```

- 2.在dailyfresh/urls.py文件中添加一条url

```python
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', include(admin.site.urls)),
    url(r'^user/',include('df_user.urls')),
]
```

- 3.添加并提交

```shell
git add dailyfresh/urls.py
git commit -m '配置用户模块url'
```

- 4.向服务器推送

```shell
git push origin dev
```

- 根据提示，需要先获取服务器的变更

```shell
git pull
```

- 当前dailyfresh/urls.py文件内容如下

```python
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', include(admin.site.urls)),
<<<<<<< HEAD
    url(r'^user/',include('df_user.urls')),
=======
        url(r'^',include('df_goods.urls')),
>>>>>>> ae79e1fd93d0d9e7f8ca36481c611a2b4a38a9db
]
```

- 其中，<<<<<<< HEAD表示当前版本的内容，=======后面，表示>>>>>>> ae79e1fd93d0d9e7f8ca36481c611a2b4a38a9db版本的内容，发现两句代码并不冲突，都需要保留，如果不能确定是否保留，可以与编写该语句的人员沟通，当前代码更改后如下

```python
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', include(admin.site.urls)),
    url(r'^user/',include('df_user.urls')),
    url(r'^',include('df_goods.urls')),
]
```

- 6.冲突解决完成，再次添加、提交、推送

```shell
git add dailyfresh/urls.py
git commit -m '配置用户模块url-解决冲突后'
git push origin dev
```