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
* `db.[collection-name].insert({[json]})`
* `db.[collection-name].find()`
* `db.[collection-name].update()`
* ``
