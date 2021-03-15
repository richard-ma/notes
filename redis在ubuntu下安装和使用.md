# redis在ubuntu下安装和使用

# 安装
* sudo apt-get install redis-server
* 修改/etc/redis/redis.conf中的superise no -> supervise systemd
* 用systemd来控制redis进程启停

# 后台进程管理
* sudo systemctl start redis
* sudo systemctl stop redis
* sudo systemctl restart redis
* sudo systemctl status redis

# 默认连接信息
* 无密码
* 默认端口6379

# 连接redis server
* redis-cli
* > ping 用来测试是否连接上了服务器,连接上会显示pong
* > set [key] [value] 设置一个key-value对
* > get [key] 取key对应的value
* > del [key] 删除key及其对应的value
