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
