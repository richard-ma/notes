# scrapyd安装使用方法

## scrapy-server

### 安装
* `pip install scrapyd`

### 测试运行
* `scrapyd`
* 访问`http://127.0.0.1:6800/`会出现响应信息

## scrapy-client

### 安装
* `pip install scrapyd-client`

### 打包scrapy项目
* cd到有scrapy.cfg的目录中
* 配置项目中的scrapy.cfg文件
	* url = [scrapyd-server-ip:6800] /scrapyd接口地址/
	* project = [project-name]
	* username = [username] /可选/
	* password = [password] /可选/
	* version = [version | HG | GIT] /可选/
* 运行`scrapyd-deploy`

### 调度运行
* `curl http://[scrapyd-server-ip]:6800/schedule.json -d project=[project-name] -d spider=[spider-name]`
