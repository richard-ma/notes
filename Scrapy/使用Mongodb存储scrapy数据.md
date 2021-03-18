# 使用Mongodb存储scrapy数据

## 安装
`pip install pymongo`

## settings.py设置数据库信息
```
MONGODB_SERVER = 'localhost'
MONGODB_PORT = 27017
MONGODB_DB = "[db-name]"
MONGODB_COLLECTION = "[collection-name]"
```

## pipeline
* 在settings.py中打开pipeline并设置权重
* 导入settings
`from scrapy.utils.project import get_project_settings`
* 设置初始化配置
```
    def __init__(self):
        self.settings = get_project_settings()
        self.connection = None
        self.db = None
        self.collection = None
```
* 创建连接
```
    def open_spider(self, spider):
        self.connection = pymongo.MongoClient(
            self.settings['MONGODB_SERVER'],
            self.settings['MONGODB_PORT']
        )
        self.db = self.connection[self.settings['MONGODB_DB']]
        self.collection = self.db[self.settings['MONGODB_COLLECTION']]
```
* 关闭连接
```
    def close_spider(self, spider):
        self.connection.close()
```
* 保存item
```
    def process_item(self, item, spider):
        self.collection.insert(dict(item))
        # log.msg("Question added to MongoDB database!",
        #         level=log.DEBUG, spider=spider)
        return item
```

## 参考文献
* [https://realpython.com/web-scraping-with-scrapy-and-mongodb/#store-the-data-in-mongodb](https://realpython.com/web-scraping-with-scrapy-and-mongodb/#store-the-data-in-mongodb)
* [https://stackoverflow.com/questions/14075941/how-to-access-scrapy-settings-from-item-pipeline](https://stackoverflow.com/questions/14075941/how-to-access-scrapy-settings-from-item-pipeline)
