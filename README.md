# MongoDB version 4.x
MongoDB learning

查看数据库
```sql
show dbs
```

创建/使用数据库
```sql
use test
```

创建集合
```sql
db.createCollection('test_table')
```

```sql
show collections
```

删除集合
```sql
db.user.drop()
```

删除数据库
```sql
db.dropDatebase()
```

插入数据
```sql
db.user.insert()
```

查找所有数据
```sql
db.user.find()
```

查询去掉后的当前集合中的某列的重复数据(返回某列重复的数据)
```sql
db.user.distinct('name')
```

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
```sql
db.user.find({age:22})
```


查找age > 22的记录
```sql
db.test_table.find({age:{$gt:22}})
```

查找age < 22 的记录
```sql
db.test_table.find({age:{$lt:22}})
```

查找age ≥ 22的记录
```sql
db.test_table.find({age:{$gte:22}})
```

查找age ≤ 22 的记录
```sql
db.test_table.find({age:{$lte:22}})
```

查找age ≥ 22 并且 age ≤ 22 的记录

```sql
db.test_table.find({age:{$gte:22,$lte:100}})
```

查找name中带有mongo的数据
```sql
db.test_table.find({name:/mongo/})
```

查找name中以mongo开头的数据
```sql
db.test_table.find({name:/^mongo/})
```

查询指定列name,age的数据
```sql
db.test_table.find({},{name:1,age:1})
```

排序 1:升序 -1降序
```sql
db.test_table.find().sort({age:-1})
```

查询name等于张三,age=22的数据
```sql
db.test_table.find({name:"张三",age:22})
```

查询前5条数据
```sql
db.user.find().limit(5)
```

查询10条以后的数据
```sql
db.user.find().skip(10)
```

查询在5到10之间的数据
```sql
db.user.find().limit(10).skip(5)
```

得到查询结果数量
```sql
db.user.find().count()
```

or与查询
```sql
db.user.find({$or:[{age:22},{age:25}]})
```

查询第一条数据
```sql
db.user.findOne() 相当于db.user.find().limit(1)
```

## 修改数据
```sql
db.user.update({name:"owllai"},{$set:{age:100}})
```

批量修改
```sql
db.user.update({name:"owllai"},{$set:{age:100}},{multi:true})
```

## 删除数据
```sql
db.user.remove({name:"owllai"})
```

只删除一条数据
```sql
db.user.remove({name:"owllai"},{justOne:true})
```


## 创建索引

```sql
db.user.ensureIndex({"username":1})
```

获取当前集合的索引

```sql
db.user.getIndexes()
```

删除索引

```sql
db.user.dropIndex({"username":1})
```

## explain executionStats

复合索引
```sql
db.user.ensureIndex({"username":1,"age":-1})

如果传入username和age查询,命中索引
如果只传入username查询,命中索引
如果只传入age查询,不命中索引
如果传入age和username查询,也可以命中索引,mongo会自动帮我们调整顺序
```

唯一索引

```sql
如果设置了唯一索引,无法插入name相同的数据
db.user.ensureIndex({"username":1},{"unique":true})
```


## 权限配置

第一步:创建超级管理用户

```sql
use admin
db.createUser({
    user:"root",
    pwd:"123456",
    roles:[{role:"root",db:"admin"}]
})
```

第二步:修改MongoDB数据库配置文件

```sql
路径: D:\SoftWare\MongoDB\bin\mongod.cfg

配置:
  security:
        authorization:enabled
```

第三步:重启mongodb服务

第四步:用超级管理员账户连接数据库

```sql
mongo admin -u 用户名 -密码
```

第五步:给test数据库创建一个用户:只能访问test数据库不能访问其他数据库
```sql
use test
db.createUser({
    user:"testadmin",
    pwd:"123456",
    roles:[{role:"dbOwner",db:"test"}]
}
)
```

## Mongodb 数据库角色
```sql
MongoDB数据库角色
内建的角色
数据库用户角色：read、readWrite;
数据库管理角色：dbAdmin、dbOwner、userAdmin；
集群管理角色：clusterAdmin、clusterManager、clusterMonitor、hostManager；
备份恢复角色：backup、restore；
所有数据库角色：readAnyDatabase、readWriteAnyDatabase、userAdminAnyDatabase、dbAdminAnyDatabase
超级用户角色：root // 这里还有几个角色间接或直接提供了系统超级用户的访问（dbOwner 、userAdmin、userAdminAnyDatabase）
内部角色：__system
```

## 用户设置相关命令
查询用户
```sql
show users;
```

删除用户
```sql
db.dropUser("testuser")
```

修改用户
```sql
db.updateUser("admin":{pwd})
```

验证用户
```sql
db.auth("admin","password")
```

## 聚合管道

使用聚合管道可以对集合中的文档进行变换和组合

MongoDB 中聚合(aggregate)主要用于处理数据(诸如统计平均值，求和等)，并返回计算后的数据结果。

集合中的数据如下：

```

{
   _id: ObjectId(7df78ad8902c)
   title: 'MongoDB Overview', 
   description: 'MongoDB is no sql database',
   by_user: 'runoob.com',
   url: 'http://www.runoob.com',
   tags: ['mongodb', 'database', 'NoSQL'],
   likes: 100
},
{
   _id: ObjectId(7df78ad8902d)
   title: 'NoSQL Overview', 
   description: 'No sql database is very fast',
   by_user: 'runoob.com',
   url: 'http://www.runoob.com',
   tags: ['mongodb', 'database', 'NoSQL'],
   likes: 10
},
{
   _id: ObjectId(7df78ad8902e)
   title: 'Neo4j Overview', 
   description: 'Neo4j is no sql database',
   by_user: 'Neo4j',
   url: 'http://www.neo4j.com',
   tags: ['neo4j', 'database', 'NoSQL'],
   likes: 750
},

```

现在我们通过以上集合计算每个作者所写的文章数，使用aggregate()计算结果如下：

```
> db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : 1}}}])
{
   "result" : [
      {
         "_id" : "runoob.com",
         "num_tutorial" : 2
      },
      {
         "_id" : "Neo4j",
         "num_tutorial" : 1
      }
   ],
   "ok" : 1
}

```



下表展示了一些聚合的表达式:

| 表达式 | 	描述	|             实例  | 
| -------- | -------- |-------- |
| $sum | 	计算总和| 	db.mycol.aggregate([{$group : {_id : "$by_user",  num_tutorial : {$sum : "$likes"}}}]) |
|$avg	|计算平均值|	db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$avg : "$likes"}}}])|
|$min|	获取集合中所有文档对应值得最小值。|	db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$min : "$likes"}}}])|
|$max|	获取集合中所有文档对应值得最大值。|	db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$max : "$likes"}}}])|
|$push|	在结果文档中插入值到一个数组中。|	db.mycol.aggregate([{$group : {_id : "$by_user", url : {$push: "$url"}}}])|
|$addToSet|	在结果文档中插入值到一个数组中，但不创建副本。|	db.mycol.aggregate([{$group : {_id : "$by_user", url : {$addToSet : "$url"}}}])|
|$first	|根据资源文档的排序获取第一个文档数据。|	db.mycol.aggregate([{$group : {_id : "$by_user", first_url : {$first : "$url"}}}])|
|$last|	根据资源文档的排序获取最后一个文档数据|	db.mycol.aggregate([{$group : {_id : "$by_user", last_url : {$last : "$url"}}}])|


```
$project：修改输入文档的结构。可以用来重命名、增加或删除域，也可以用于创建计算结果以及嵌套文档。
$match：用于过滤数据，只输出符合条件的文档。$match使用MongoDB的标准查询操作。
$limit：用来限制MongoDB聚合管道返回的文档数。
$skip：在聚合管道中跳过指定数量的文档，并返回余下的文档。
$unwind：将文档中的某一个数组类型字段拆分成多条，每条包含数组中的一个值。
$group：将集合中的文档分组，可用于统计结果。
$sort：将输入文档排序后输出。
$geoNear：输出接近某一地理位置的有序文档。
$lookup: 多表关联（3.2版本新增）
```

### 数据准备

```
> db.order.find()
{ "_id" : ObjectId("5fd62f7b4b76fc8004cf77f4"), "order_id" : "1", "uid" : 10, "trade_no" : 111, "all_price" : 100, "all_num" : 2 }
{ "_id" : ObjectId("5fd62f914b76fc8004cf77f5"), "order_id" : "2", "uid" : 7, "trade_no" : 222, "all_price" : 90, "all_num" : 2 }
{ "_id" : ObjectId("5fd62fa34b76fc8004cf77f6"), "order_id" : "3", "uid" : 9, "trade_no" : 333, "all_price" : 90, "all_num" : 6 }

> db.order_item.find()
{ "_id" : ObjectId("5fd62fe84b76fc8004cf77f7"), "order_id" : 1, "title" : "kuangquanshui", "price" : 50, "num" : 1 }
{ "_id" : ObjectId("5fd62ff94b76fc8004cf77f8"), "order_id" : 1, "title" : "瓜子", "price" : 10, "num" : 5 }
{ "_id" : ObjectId("5fd630684b76fc8004cf77f9"), "order_id" : 2, "title" : "牛娜", "price" : 10, "num" : 5 }
{ "_id" : ObjectId("5fd630764b76fc8004cf77fa"), "order_id" : 2, "title" : "香烟", "price" : 10, "num" : 4 }
{ "_id" : ObjectId("5fd630a84b76fc8004cf77fb"), "order_id" : 3, "title" : "毛巾", "price" : 10, "num" : 5 }
{ "_id" : ObjectId("5fd630b34b76fc8004cf77fc"), "order_id" : 3, "title" : "键盘", "price" : 40, "num" : 1 }
```

```
$project：
> db.order.aggregate([{$project:{order_id:1,all_price:1} }])
{ "_id" : ObjectId("5fd62f7b4b76fc8004cf77f4"), "order_id" : "1", "all_price" : 100 }
{ "_id" : ObjectId("5fd62f914b76fc8004cf77f5"), "order_id" : "2", "all_price" : 90 }
{ "_id" : ObjectId("5fd62fa34b76fc8004cf77f6"), "order_id" : "3", "all_price" : 90 }
```

```
$match：
> db.order.aggregate([{$project:{order_id:1,all_price:1}},{$match:{all_price:{$gte:90}}  }])
{ "_id" : ObjectId("5fd62f7b4b76fc8004cf77f4"), "order_id" : "1", "all_price" : 100 }
{ "_id" : ObjectId("5fd62f914b76fc8004cf77f5"), "order_id" : "2", "all_price" : 90 }
{ "_id" : ObjectId("5fd62fa34b76fc8004cf77f6"), "order_id" : "3", "all_price" : 90 }
> db.order.aggregate([{$project:{order_id:1,all_price:1}},{$match:{all_price:{$gte:95}}  }])
{ "_id" : ObjectId("5fd62f7b4b76fc8004cf77f4"), "order_id" : "1", "all_price" : 100 }
```

```
$group

> db.order.aggregate([ {$group:{_id:"$order_id",totle:{$sum: "$num"} } } ])
{ "_id" : "2", "totle" : 0 }
{ "_id" : "1", "totle" : 0 }
{ "_id" : "3", "totle" : 0 }

> db.order_item.aggregate([ {$group:{_id:"$order_id",totle:{$sum: "$price"} } } ])
{ "_id" : 1, "totle" : 60 }
{ "_id" : 3, "totle" : 50 }
{ "_id" : 2, "totle" : 20 }
```

```
$lookup 表关联

db.order.aggregate(
    [ 
        {
            $lookup:{
                from:"order_item", 
                localField:"order_id",
                foreignField:"order_id",
                as:"items"
                }
                } 
 ])

 { "_id" : ObjectId("5fd62f7b4b76fc8004cf77f4"), "order_id" : "1", "uid" : 10, "trade_no" : 111, "all_price" : 100, "all_num" : 2, "items" : [ ] }
{ "_id" : ObjectId("5fd62f914b76fc8004cf77f5"), "order_id" : "2", "uid" : 7, "trade_no" : 222, "all_price" : 90, "all_num" : 2, "items" : [ ] }
{ "_id" : ObjectId("5fd62fa34b76fc8004cf77f6"), "order_id" : "3", "uid" : 9, "trade_no" : 333, "all_price" : 90, "all_num" : 6, "items" : [ ] }
```