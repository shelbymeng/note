## mongo

### 创建数据库

`use 数据库名称`  
`db //显示创建的库`  
查看所有数据库  
`show dbs`

### 删除数据库

切换到当前数据库  
`db.dropDatabase()`  
查看集合  
`show collections`

检查数据库是否删除成功  
`show dbs`

### 创建集合

`db.createCollection('集合名') //类似数据库中的`

### 删除集合

`db.collection.drop()`

### 插入文档

向集合中插入一个新文档。  
`db.collection.insertOne()`  
向集合中插入一个或者多个文档。  
`db.collection.insertMany()`

### 更新文档

```
db.collection.update(
   <query>,
   <update>,
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }
)
```

- query: 查询条件
- update：需要 update 的对象和一些更新操作符。
- upsert： 可选，若不存在 update 的记录，是否可以插入，默认 false 不插入。
- multi： 可选，mongodb 默认 false，只更新找到的第一条记录，若参数为 true，就把按条件查出来的多条记录全部更新。
- writeConcern： 可选，抛出异常的级别。

### 删除文档

新版方法  
`db.collection.deleteOne()`  
`db.collection.deleteMany()`  
**注意：**  
remove()方法并不会真正释放空间，需要继续执行 db.repairDatabase 来回收磁盘空间。

### 查询文档

```
db.collection.find(query,projection)
query: 可选，使用查询操作符指定查询条件。
projection： 可选，使用投影操作符指定返回的键，查询时返回文档中所有键值，省略该参数即可。
```

`db.coollection.find().pretty()`  
pretty()方法以格式化的方式来显示所有文档。  
`db.collection.findOne()方法，只返回一个文档`

- mongoDB AND 条件  
  mongoDB 的 find 方法可以传入多个键（key），每个键以逗号隔开。  
  `db.collection.find(key1: value1,key2: value2)`
- mongoDB OR 条件

```
db.col.find(
   {
      $or: [
         {key1: value1}, {key2:value2}
      ]
   }
).pretty()
```  

### 条件操作符   
- 简要说明  
```
$gt ------- greater than >  
$gte -------gt equal >=  
$lt ------- less than <  
$lte ------- lt equal <=  
$ne ------- not equal !=    
$eq ------- equal =  
```
- 模糊查询  
```
title中包含a字符的文档  
db.col.find({title: /a/})
title以a字符开头的文档  
db.col.find({title: /^a/})
title以a结尾的文档  
db.col.find({title: /a$/})
```