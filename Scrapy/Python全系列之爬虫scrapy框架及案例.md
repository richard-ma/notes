# Python全系列之爬虫scrapy框架及案例

## 安装scrapy
* `pip install scrapy`

## 创建基础项目
* 创建项目`scrapy startproject [project_name]`
* 生成爬虫`scrapy genspider [spider_name] [allowed_domains]`
	* 设置allowed_domains为要爬取网站的域名
	* 设置start_urls为起始页url
	* 实现spider.parse(self, response)解析返回数据
	* 定义Item,并且import到spider文件中使用
* 提取数据: xpath, extract(), extract_first()
* 保存数据: pipeline
	* 在settings中打开对应的pipeline并且赋予权重
	* 实现process_item(self, item, spider)方法,处理爬取到的数据
    
## settings
* 关闭robots协议
* 设置`USER_AGENT`来欺骗服务器识别

## logging
* settings中设置`LOG\_LEVEL`和`LOG\_FILE`
* import logging
* logging.warning(), logging.error()等等输出log信息

## 构造请求
* scrapy.Request(url, callback)
	* url 为请求的url,注意检查请求url是否完整
	* callback 为请求对应的处理函数,如果不设置就使用parse
	* meta 为传递给callback的字典数据,比如meta={"item": item}

## scrapy shell
* `scrapy shell [url]`
* 可以直接获得response来进行各种操作
	* response.url 当前响应的url地址
	* response.request.url 当前响应对应请求的url
	* response.headers 响应头
	* response.body 响应体
	* response.body.decode() 解码响应体(文字)
	* response.request.headers 当前响应对应的请求头

## pipeline
* 在爬虫启动或者停止时进行数据文件操作
	* open_spider(self, spider) 打开数据文件操作写这里
	* close_spider(self, spider) 关闭数据文件操作写这里
	* 中途报错,文件不会有任何数据,因为没有被关闭,推荐数据库存储,每次insert或者commit都会保存数据

## crawl spider
* `scrapy genspider -t crawl [spider_name] [allowed_domains]`
* spider继承自CrawlSpider
* rules 定义提取url地址规则
	* 是一个元组,证明规则有执行顺序
	* LinxExtractor
		* allow=r"" 正则表达式提取url规则
		* callback 对应的处理函数
		* follow 匹配的url是否还匹配其他规则,还是直接停止本轮匹配
* 实现parse_item(self, response)处理响应

## 携带cookie
* spider中定义cookies字典
* 重写start_requests(self)函数
	* yield scrapy.Request(start_urls, callback=self.parse, cookies=self.cookies)
* settings打开`COOKIES_DEBUG`

## 模拟登陆
* 发送post请求来模拟登陆过程,获得登陆后的cookies
* start_urls填入登陆form所在的url
* parse处理并获得登陆form,提交用户名和密码,获得cookies
	* yield scrapy.FormRequest.form_response(response, formdata={}, callback)
	* response为响应,会自动在这里找到form
	* fromdata为要填入的用户名密码等数据
	* callback为处理响应的函数,就是处理登陆后页面的函数,可以用来检测登陆是否成功
* response的响应头中set-cookie字段设置cookie,有可能时网站用来识别是否登陆的方法
* response的响应头中302会地址重定向,有可能在不同的几个页面中set-cookie


## Downloader Middleware
* settings中开启middleware并且赋予权重
* 实现方法
	* `process_request(self, request, spider)` 发送请求前调用
	* `process_response(self, response, spider)` 获得响应后调用

## scrapy_redis
* 用redis处理scheduler的请求队列和pipeline保存数据
* [视频源码分析](https://www.bilibili.com/video/BV1jt411Q7PD?p=22)

## scrapyd
* 执行spider的daemon进程
* 可以通过http协议进行远程操作和管理
