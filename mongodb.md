# mongodb

## install
* sudo apt-get install mongodb

## daemon
* `sudo service mongodb start` 启动数据库服务
* `sudo service mongodb stop`
* `sudo service mongodb restart`

## client
* `mongo`

## database
* `db` 显示当前所在的数据库
* `show databases` | `show dbs` 显示所有数据库
* `use [database-name]` 切换到要使用的数据库
* `db.dropDatabase()` 删除当前数据库

## collection / table 
* `db.createCollection("[collection-name]")` 创建集合
* `show collections` 显示所有集合
* `db.[collection-name].drop()` 删除某个集合

## data type
* Object ID:
	* 4字节为当前时间戳
	* 3字节机器ID
	* 2字节服务器进程ID
	* 3字节增量值
* String: UTF-8
* Boolean: true/false
* Integer
* Double
* Arrays
* Object
* Null
* Timestamp
* Date: python datetime

## CURD
* `db.[collection-name].insert({[json]})` 数据_id不可以重复
* `db.[collection-name].save({json})` 当_id重复时为update操作,否则为insert操作
* `db.[collection-name].find()` 显示所有数据
* `db.[collection-name].update(<query>, <update>, {multi: <boolean>})` 更新数据
	* query 更新条件
	* update 更新后的值
	* multi true为多条,false之更新第一条,默认为只更新第一条
	* `db.[collection-name].update({name: "xiaowang"}, {name: "xiaozhang"})` 其他数据会被抹掉
	* `db.[collection-name].update({name: "xiaowang"}, {$set: {name: "xiaozhang"}})` 其他数据不会被抹掉,仅替换指定的键,这个更常用
* `db.[collection-name].remove(<query>, {justOne: <boolean>})` 删除数据
	* query 删除条件
	* justOne true只删除第一条,false删除所有满足条件的数据,默认删除所有满足条件的数据
## 高级查询
* `db.[collection-name].find(<query>)/findOne(<query>).pretty()`
	* query 为查询条件
	* `db.[collection-name].find({age: {$lte:18}})` 年龄小于等于18
	* $lt $lte $gt $gte $ne 默认为等于
	* $in / $nin 范围 `db.[collection-name].find({age: {$in:[16, 18, 20]}})`
	* 默认多条件是and效果
	* $or `db.[collection-name].find($or[{age:18}, {hometown:"桃花岛"}}])`
* 支持正则表达式
	* `db.[collection-name].find({sku:/^abc/})` abc开头的数据
	* `db.[collection-name].find({sku:{$regex:"789$"}})` 789结尾的数据
* limit skip
	* limit 选中若干个
	* `db.[collection-name].find().limit(2)`
	* skip 跳过若干个
	* `db.[collection-name].find().skip(2)`
	* `db.[collection-name].find().skip(2).limit(2)` 先skip速度快,用于翻页
* $where 可以直接用js语句
`
db.[collection-name].find({
	$where: function(){
		return this.age <= 18
	}
})
`
* 投影
`db.[collection-name].find({age:{$gt:18}}, {_id:0, name:1})` 只显示name列

* 排序
`db.[collection-name].find().sort({age:-1, gender:-1})` 1为升序,-1为降序

* 统计
`db.[collection-name].find().count()`

* 去重
`db.[collection-name].distinct('去重字段', {条件})`

## 数据备份和恢复
* `mongodump -h [host] -d [db-name] -o [output-dir]` 备份数据
* `mongorestore -h [host] -d [db-name] --dir [data-path]` 恢复数据

## 聚合功能
* `db.[collection-name].aggregate({管道:{表达式}})`
* 聚合功能
	* $group 分组
	* $match 过滤数据
	* $project 修改输入文档的结构, 重命名 增加删除字段(类似影射) 创建计算结果
	* $sort 排序
	* $limit 限制返回数
	* $skip 跳过指定数量文档
	* $unwind 数组类型字段进行拆分
* 表达式
	* $sum 求和 `$sum: 1`可以用来计数
	* $avg 平均值
	* $min 最小值
	* $max 最大值
	* $push 插入数组
	* $first 第一个文档数据
	* $last 最后一个文档数据

* Group by null 不指定分组,即所有文档为一组

## 例子代码
```
# 按照gender进行分组
db.student.aggregate({
	$group: {_id: "$gender", count:{$sum: 1}}
})

# 按照gender进行分组, 并计数获取每组平均年龄
db.student.aggregate({
	$group: {_id: "$gender", count:{$sum: 1}, avg_age:{$avg: "$age"}}
})

# 按照hometown进行分组,获取不同分组的平均年龄
db.student.aggregate({
	$group: {_id: "$hometown", avg_age:{$avg: "$age"}}
})

# group by null
db.student.aggregate({
	$group: {_id: null, count:{$sum: 1}, avg_age:{$avg: "$age"}}
})

# 查询姓名和年龄
db.student.aggregate(
	{$project: {_id: 0, name:1, age:1}}
)

# 查询男女生人数和总人数
db.student.aggregate(
	{$group: {_id: "$gender", count:{$sum: 1}}},
	{$project: {_id:0, count:1}}
)


```

