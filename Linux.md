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

## curl概括
```shell
# 获取网页源码
curl '网站地址'
curl -o src.html '网站地址'   # 将网站源码存入指定文件
curl -i '网站地址'   # 显示网站http response的头信息以及网页源码

# 获取get接口数据
curl '接口地址?参数'

# 获取post接口数据
curl -X POST '接口地址'
curl -d '参数' -X POST '接口地址'

curl --cookie 'cookie参数' '接口地址'
```