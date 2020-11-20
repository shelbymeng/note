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
### Limit与Skip方法  
- Limit方法
可以在mongoDB中读取指定数量的数据记录，可以使用limit方法，limit方法接受一个数字参数，指定从mongoDB中读取的记录条数。  
`db.collection.find().limit(NUMBER)`   
- Skip方法  
limt指定读取数据的数量，Skip方法可以跳过指定数量的数据，接受一个数字作为参数。  
`db.collection.find().skip(NUMBER)`  
**注意：**  
`db.col.find({},{"title":1,_id:0}).limit(2)`  
1. find参数中第一个{}放置where条件，空表示返回集合中所有的文档。  
2. 第二个参数指定哪些列显示和不显示（1表示显示，0表示不显示）。  

skip和find方法只适合小数据量分页，百万级效率极低，因为skip方法是一条条数据数过去，建议使用where_limit。  

### mongoDB的sort方法  
可以对数据进行排序，sort()方法可以通过参数指定排序的字段，并使用1和-1来指定排序的方式，1为升序序列，而-1用于降序排列。  
`db.collection.find().sort({key: key})`  
skip(),limit(),sort()放在一起执行的时候，执行的顺序是先sort，然后skip，最后是显示的limit。  

### mongoDB索引  
索引能够提高查询效率，没有索引，mongoDB在读取数据时必须扫描集合中的每个文件并选取那些符合查询条件的记录。   
扫描全集合的查询效率很低。  
索引是一种特殊的数据结构，索引存储在一个易遍历读取的数据集合中，索引是对数据库表中一列或者多列的值进行排序的一种结构。  
createIndex()方法  
`db.collection.createIndex(key,options)`  
key值是要创建的索引字段，1为指定按升序创建索引。  
`db.collection.createIndex({"title": 1})`  
createIndex方法中也可以设置使用多个字段创建索引（关系型数据库中称为复合索引）。  
`db.collection.createIndex({"title": 1,"des": -1})`  
| Parameter | Type | Desription |
| -- | -- | -- |
| background | Boolean | 建索引的过程会阻塞其他数据库操作，background可以指定以后台方式创建索引，默认值为false |
| unipue | Boolean | 建立的索引是否唯一。true创建唯一索引，默认值为false。| 
| name | string | 索引名称，若未指定，mongoDB通过连接多因的字段名和排序顺序生成一个索引名称。 | 
| sparse | Boolean | 对文档中不存在的字段数据不启用索引；这个参数需要特别注意，若设置为true，在索引字段中不会查询出不包含对应字段的文档。默认为false。 | 
| expireAfterSeconds | integer | 指定一个以秒为单位的数值，完成TTL设定，设定集合的生存时间。 | 
| v | index version | 索引的版本号，默认值取决于mongod创建索引时运行的版本。 | 
| weights | document | 索引权重值，1-99999之间，表示该索引相对于其他索引字段的得分权重。 | 
| default_language | string | 对于文本索引，该参数决定了停用词及词干和词器的规则的列表，默认英语。 | 
| language_override | string | 对于文本索引，参数指定了包含在文档中的字段名，语言覆盖默认的language，默认为language。 |  
在后台创建索引：  
`db.values.createIndex({open: 1,close: 1}, {background: true})`  
通过创建索引时追加background的选项，使创建工作在后台进行。  
note:  
1. 查看集合索引  
`db.col.getIndexes()`  
2. 查看集合索引大小  
`db.col.totalIndexSize()`  
3. 删除集合所有索引  
`db.col.dropIndexes()`  
4. 删除集合指定索引  
`db.col.dropIndex("索引名称")`  
利用TTL集合对存储的数据进行失效时间设置：经过指定的时间段后或在指定的时间点过期，mongoDB独立线程去清除数据。类似于设置定时自动删除任务，可以清除历史记录或日志等前提条件，设置index的关键字段为日期类型new Date()。  

- 例如数据记录中createDate为日期类型时：  
1. 设置时间180秒后清除。  
2. 设置在创建记录后，180秒所有清除。  
- 由记录中设定日期点清除。  
设置A记录在2019年1月22日晚上11点左右删除，A记录中需添加"ClearUpDate": new Date('Jan 22, 2019 23:00:00')，且index中expireAfterSeconds设值为0。  
`db.col.createIndex({"clearUpDate": 1}, {expireAfterSeconds: 0})`  
1. 索引关键字段必须是Date类型。  
2. 非立即执行，扫描Document过期数据并删除是独立线程执行，默认60s扫描一次，删除也不一定是立即删除成功。  
3. 单字段索引，混合类型不支持。  

### mongoDB聚合  
聚合aggregate主要用于处理数据（统计平均值，求和等），并返回计算后的数据结果，类似sql中的count（*）。  
aggregate()方法  
`db.COLLECTION_NAME.aggregate(AGGREGATE_OPREATION)`  
`db.mycol.aggregate([{$group:{_id:"$by_user", num_tutorial: {$sum: 1}}}])`  
上例通过字段by_user对数据进行分组，并计算by_user字段相同值的总和。  
| 表达式 | 描述 | 
| -- | -- | -- |  
| $sum | 计算总和 | 
| $avg | 计算平均值 | 
| $min | 获取集合中所有文档对应的最小值 |
| $max | 获取集合中所有文档对应的最大值 | 
| $push | 在结果文档中插入值到一个数组中，但不创建副本 |
| $addToSet | 在结果文档中插入值到一个数组中 |
| $first | 根据资源文档的排序获取第一个文档数据 |
| $last | 根据资源文档的排序获取最后一个文档数据 |  

管道概念  
mongoDB的聚合管道将mongoDB文档在一个管道处理完毕后将结果传递给下一个管道处理。管道操作是可以重复的。  
表达式：处理输入文档并输出，表达式是无状态的，只能用于当前聚合管道的文档，不能处理其它的文档。  
| 表达式 | 描述 |  
| -- | -- |  
| $project | 修改输入文档的结构。可以用来重命名，增加或删除域，也可以用于创建计算结果以及嵌套文档。 |
| $match | 用于过滤数据，只输出符合条件的文档。 | 
| $limit | 用来限制mongoDB聚合管道返回的文档数。 |
| $skip | 在聚合管道跳过指定数量的文档，并返回余下的文档。 |  
| $unwind | 将文档中的某一个数组类型字段拆分成多条，每条包含数组中的一个值。 |
| $group | 将集合中的文档分组，而用于计算统计结果。 |
| $sort | 将输出文档排序后输出。 |
| $geoNear | 输出接近某一地理位置的有序文档。 |  
当match与group一起出现时，顺序会影响检索的结果。一般先将match写在前面。  
