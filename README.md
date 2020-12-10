# MongoDB
MongoDB learning

查看数据库
show dbs

使用数据库
use test

show collections

删除集合
db.user.drop()

删除数据库
db.dropDatebase()

插入数据
db.user.insert()

查找所有数据
db.user.find()

查询去掉后的当前集合中的某列的重复数据(返回某列重复的数据)
db.user.distinct('name')

```sql
 > db.test_table.find()

{ "_id" : ObjectId("5ee6ea7b6132c707d9e4d003"), "username" : "owllai" }
{ "_id" : ObjectId("5fd1f51de415faa4bffc4be5"), "name" : "owllai" }
{ "_id" : ObjectId("5fd1f522e415faa4bffc4be6"), "name" : "owllai1" }
{ "_id" : ObjectId("5fd1f525e415faa4bffc4be7"), "name" : "owllai2" }
{ "_id" : ObjectId("5fd1f607e415faa4bffc4be8"), "name" : "owllai2" }
{ "_id" : ObjectId("5fd1f60ae415faa4bffc4be9"), "name" : "owllai2" }
{ "_id" : ObjectId("5fd1f60de415faa4bffc4bea"), "name" : "owllai3" }

> db.test_table.distinct('name')

[ "owllai", "owllai1", "owllai2", "owllai3" ]

```

查找age =22的记录
db.user.find({age:22})

查找age > 22的记录

查找age < 22 的记录