# Linux

## CentOS安装nginx
```shell
# 第一步：安装nginx
yum install nginx
# 查看nginx安装位置
which nginx
# 查看nginx配置文件
nginx -t
# 查看nginx配置文件
cat nginx.conf
# 停止nginx
nginx -s stop
# 启动nginx
nginx
# 重新加载资源
nginx -s reload
```