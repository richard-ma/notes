# 创建scrapyd环境的check list
* `sudo apt-get install python3 virtualenv` 安装python3和virtualenv
* `virtualenv -p python3 scrapyd-env` 创建一个virtualenv环境
* `. ./scrapyd-env/bin/activate` 进入创建的虚拟环境
* `pip install scrapyd` 安装scrapyd
* 创建scrapyd.conf [https://scrapyd.readthedocs.io/en/stable/config.html](https://scrapyd.readthedocs.io/en/stable/config.html)
* 将scrapyd.conf复制到scrapyd-env目录下
* `scrapyd` 测试运行
* `访问http://ip:6800`
