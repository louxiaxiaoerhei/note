# Linux 常用命令总结

创建用户

```bash
一、新增用户
新增用户test:

sudo useradd -d /home/test -m test
-d 指定用户的主目录,-m 表示当主目录不存在时,创建它。

二、设定密码
用户创建好后,可以通过以下命令修改密码:

sudo passwd test 三、设置用户为管理员
如果想修改用户的一些属性(比如shell),可以用usermod,下面将用户的shell设置为bash:

sudo usermod -s /bin/bash test
同样地,使用usermod可以增加用户所属的用户组,一个用户可以属于多个组,如果用户属于sudo组,用户就是管理员用户了,可以使用sudo命令提升权限了。 
增加sudo用户组的命令如下:

sudo usermod -G sudo test
```

