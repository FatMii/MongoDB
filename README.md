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