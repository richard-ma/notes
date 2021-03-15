# 利用ImagesPipeline下载图片

## 操作过程
* 在Item定义中添加字段
	* image_urls 是一个列表,保存待下载的图片地址,注意不完整的地址要补全
	* images 是一个字典,保存图片的校验码和下载路径等信息
	* image_paths 是一个列表,保存图片下载路径
* 在settings中添加设置
	* 打开对应的Pipeline,并赋予权重
	* IMAGE_STORE 下载图片保存的路径
	* IMAGE_EXPIRES 图片的有效期(单位为天),在期限内图片不会被重新抓取
* spider中获取图片url
	* 将图片url填入item的image_urls字段中,如果只有一个也要放入列表中
* 实现pipeline
	* 定义的pipeline必须继承自ImagesPipeline
	* 实现get_media_requests(self, item, info)方法
		* 向headers中添加信息
		* yield返回scrapy.Request对象来实现下载
	* 实现item_completed(self, results, item, info)
		* results是一个表示图片是否下载成功的标志
		* 此函数处理下载成功的后续工作

## 参考文献
* [https://www.jianshu.com/p/c12d2ac7d55f](https://www.jianshu.com/p/c12d2ac7d55f)
