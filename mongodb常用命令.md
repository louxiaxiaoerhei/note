# mongodb 常用命令

#### 常用的命令

```shell
1. mongodb服务端启动
	sudo mongod --auth # 以权限认证的方式启动
		        --fork # 后台运行
		        --logpath= # 指定日志文件路径
		        --logappend # 声明日志以追加方式写入
		        --dbpath= # 指定数据库文件夹所在路径

2. 进入mongo shell：mongo

3. 超级管理员账号的创建
a. use admin
	b .db.createUser({"user":"python","pwd":"python","roles":["root"]})
	c. 以权限认证的方式重启mongod服务
	d. use admin
	e. db.auth('账号', '密码')

4. 普通用户创建
	在admin库中执行
		use admin
		db.createUser({"user":"python1", "pwd":"python1", roles:[{"role":"read","db":"dbname1"},{"role":"readWrite","db":"dbname2"}]})

5. 查看用户 show users

6. 删除用户 db.dropUser('账号')

7. mongodb的命令
	show dbs
	use 数据库名
	db # 查看当前数据库
	db.dropDatabase() 
	show collections # 查看当前数据库中集合
	db.集合名称.drop() # 删除集合

8. 增删改
	db.集合名称.insert(document)
	db.集合名称.save(document)
		# 根据数据中的_id，存在就更新，不存在就insert
	db.集合名称.update({条件}}, {$set:{指定更新的kv}}, {multi:false/true})
		# multi参数默认false，true表示更新全部
	db.集合名称.remove({条件}, {justOne: false/true})
		# justOne默认是false，true表示删除一条

9. 查
	a. db.集合名.find({条件}).pretty()
	b. db.stu.find({age:{$gte:18}}) # $lte $ne 
	c. db.stu.find({$or:[{age:{$gte:18}},{gender:true}],name:'gj'})
	d. db.stu.find({age:{$in:[18,28,38]}}) # $nin
	e. db.stu.find({name:{$regex:'靖$'}})
	f. db.stu.find().skip(1).limit(1)
	g. db.stu.find({},{_id:0,name:1,gender:1})
	h. db.stu.find().sort({gender:-1,age:1})
	i. db.stu.find().count()


10. 聚合操作
	a. $group
		db.集合名.aggregate(
			{$group: 
				{
					_id:'$k', # _id: null
					k2: {$sum:1},
					k3: {$push:'$$ROOT'} # 把整个数据放到k3中
				}
			}
		)

	db.stu.aggregate(
		{
			$group:{
				_id:{
					hometown1:'$hometown',
					age1:'$age',
					name1:'$name'
				}
			}
		}
	)

	b. $match
	 db.stu.aggregate(
     {$match:{age:{$gt:20}}},
     {$group:{_id:"$gender",counter:{$sum:1}}}
  )

  	c. $project
		db.stu.aggregate(
			{$group:{_id:"$gender",counter:{$sum:1}}},
			{$project:{_id:0,counter:1}}
		)

	d. db.stu.aggregate(
     {$group:{_id:'$gender', count:{$sum:1}}},
     {$sort:{count:1}},
     {$skip:1},
     {$limit:1}
 ) # 先skip，再limit!

11. 索引
	索引的作用：
		提高查询速度
		对数据做去重
	创建索引
	db.集合名.ensureIndex({k1:1,k2:-1}, {"unique":true})
		# 普通索引：{k1:1升序/-1降序}
		# 联合索引：{k1:1,k2:-1}
		# 唯一索引：{"unique":true}
	查看索引
	db.集合名.getIndexes()
	删除索引
	db.集合名.dropIndex({k1:1})

	# 注意
		# 提高查询建立索引的字段的速度
		# 尤其是联合索引，会降低写入速度，这也是读写分离的原理


12. 备份恢复、导出导入
	备份
		mongodump -h ip:port -d 数据库 -o 数据库文件夹位置
	恢复
		mongorestore -h ip:port -d 数据库 --dir=备份的数据库文件的位置
	导出
		mongoexport -h ip:port -d 数据库 -c 集合名 -o 导出的文件完整路径
	导入
		mongoimport -h ip:port -d 数据库 -c 集合名 --file=导出文件的完整路径
```

