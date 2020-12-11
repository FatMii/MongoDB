# MongoDB
MongoDB learning

查看数据库
show dbs

创建/使用数据库
use test

创建集合
db.createCollection('test_table')

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

## 查询数据

查找age =22的记录

db.user.find({age:22})

查找age > 22的记录

 db.test_table.find({age:{$gt:22}})

查找age < 22 的记录

 db.test_table.find({age:{$lt:22}})

查找age ≥ 22的记录

 db.test_table.find({age:{$gte:22}})

查找age ≤ 22 的记录

 db.test_table.find({age:{$lte:22}})

查找age ≥ 22 并且 age ≤ 22 的记录

 db.test_table.find({age:{$gte:22,$lte:100}})

查找name中带有mongo的数据

db.test_table.find({name:/mongo/})

查找name中以mongo开头的数据

db.test_table.find({name:/^mongo/})

查询指定列name,age的数据
db.test_table.find({},{name:1,age:1})

排序 1:升序 -1降序
db.test_table.find().sort({age:-1})

查询name等于张三,age=22的数据
db.test_table.find({name:"张三",age:22})

查询前5条数据
db.user.find().limit(5)

查询10条以后的数据
db.user.find().skip(10)

查询在5到10之间的数据
db.user.find().limit(10).skip(5)

得到查询结果数量
db.user.find().count()

or与查询
db.user.find({$or:[{age:22},{age:25}]})

查询第一条数据
db.user.findOne() 相当于db.user.find().limit(1)


## 修改数据

db.user.update({name:"owllai"},{$set:{age:100}})

批量修改
db.user.update({name:"owllai"},{$set:{age:100}},{multi:true})

## 删除数据

db.user.remove({name:"owllai"})

只删除一条数据

db.user.remove({name:"owllai"},{justOne:true})
